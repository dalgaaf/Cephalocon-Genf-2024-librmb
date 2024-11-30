<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-1" data-timing="20s" data-menu-title="Findings - MDS Load" -->
## MDS load distribution

### Issue: MDS performance low in default setup

* bottlenecks
  * single threaded MDS code
  * numbers of files and directory structure
  * cache/memory default settings

### Solution:
  * multiple MDS per hardware node (8)
  * dynamic balancer wasn't reliable in the cluster¹
  * used subtree pinning instead

Note: 
* single threaded: MDS nodes under-utilized
¹ in nautilus


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="Findings - Expunge" -->
## Nightly dovecot jobs

### Nightly expire/expunge jobs run too long

* runs nightly to e.g. delete emails from trash older than x days
* job generates load only on one MDS: bottleneck
* caused by file system structure and MDS subtree pinning

### Solution:
* moved job related directories to separate CephFS
* no subtree pinning on these MDS to distribute load


<!-- .slide: data-state="normal" id="findings-3" data-timing="20s" data-menu-title="Findings - MDS Failover" -->
## MDS Failover

### Issue: MDS Failover slightly slow

* initially we used only active and cold-standby MDSs (16+8)
* failover was slow under load
  * due to replay

### Solution
* added standby-replay servers
* change numbers (16/16/16)
* caused new issue after some time 
  * failover to standby-replay caused issues with left over locks
  * switched back to cold-standby, more reliable


<!-- .slide: data-state="normal" id="findings-4" data-timing="20s" data-menu-title="Findings - Mailbox repair" -->
## Recover broken mailbox indexes

### Issue: Recovery takes long time and produces high load

* lost or broken indexes require search trough the full namespaces
* listing all objects takes a very long time on Rados
  * not done in parallel
  * large number of objects

### Solution

* idea: distribute / parallelize the listing of objects
* final solution:
  * add additional index for mail oids per UUID
  * use as input for the repair

Note: https://github.com/ceph-dovecot/dovecot-ceph-plugin/issues/349


<!-- .slide: data-state="normal" id="findings-5" data-timing="20s" data-menu-title="Findings - Performance" -->
## Performance during initial import

### Issue: R/W performance low with increasing number of accounts

* Performance analysis
* Issues identified:
  * PGs size and number of objects per PG
    * autoscaling not used in nautilus
    * 45 GB / 190k objects per PG
  * num_strays defaults to low

### Solution:
  * PG splitting / double number of PGs
    * positiv side effect: deep scrubbing from 90 to 40min
  * increased num_strays to 2m

Note:
* issue 594


<!-- .slide: data-state="normal" id="findings-10" data-timing="20s" data-menu-title="Findings - Cost" -->
## Findings - Cost
### Issue 1: Power Consumption 
### Issue 2: Space / Density

* both topics connected
* power consumption per TB/account too high
* PoC hardware is not up-to-date¹
  * energy efficiency per TByte increased for HDDs and SSD/NVMe

### Solution: update hardware concept
* higher density (2U: 4.6-13x higher capacity)
* higher efficiency CPUs
* higher density requires higher network bandwith / cooling
* may open options to use slightly more efficient EC 

Note:
* Density: 2U DL345 w/ 24/34 SSDs/NVMe vs current: 4,6x/6,5 SSD | 13x NVMe Storage


<!-- .slide: data-state="normal" id="findings-11" data-timing="20s" data-menu-title="Findings - Cost" -->
## Findings - Cost
### Storage device power consumption
<canvas data-chart="bar">
<!--
{
 "data" : {
     "labels": ["HDD 4TB SAS", "SSD 1.6TB SAS", "" , "HDD 20TB SAS", "HDD 24TB SAS", "HDD 24TB SATA", "NVMe 15.36TB", "SSD 7.68TB SATA", "SSD 6.4TB SAS" ],
     "datasets": [
         {
             "data": [3, 5.4, "" , 0.5, 0.39, 0.375, 1, 0.46, 1.5 ],
             "backgroundColor": [
                 "rgba(206, 22, 22, 0.45)",
                 "rgba(206, 22, 22, 0.7)",
                 "rgba(168, 222, 143, 0.8)",
                 "rgba(168, 222, 143, 0.7)",
                 "rgba(168, 222, 143, 0.8)",
                 "rgba(168, 222, 143, 0.9)",
                 "rgba(168, 222, 143, 0.5)",
                 "rgba(168, 222, 143, 0.7)",
                 "rgba(168, 222, 143, 0.3)"]
         }
     ]
 },
 "options": {
     "animateScale": "true",
     "responsive": "true",
     "legend": {
           "display": 0
     },
     "layout": {
         "padding": {
             "left": 0,
             "right": 0,
             "top": 10,
             "bottom": 0
         }
     },
     "plugins": {
         "datalabels": {
             "align": "end",
             "anchor": "end"
         }
     },
     "scales": {
         "yAxes": [{
             "gridLines": {
                 "color": "rgba(0, 0, 0, 0)"
             },
             "scaleLabel": {
                 "display": 1,
                 "labelString": "W/TByte under load (source: HPE Quick Specs)"
             },
             "ticks": {
                 "display": 0
             }
         }],
         "xAxes": [{
             "gridLines": {
                 "color": "rgba(0, 0, 0, 0)"
             }
         }]
    }
 }
}
-->
</canvas>

Note: 
¹ from 2017/2019, 5-8y
 * HDD 4 TB SAS 7.2k: 3W/TB
 * SSD 1.6TB SAS: 5.4W/TB
Currently available with HPE:
 * HDD 20TB SAS 7.2k: 0.5W/TB, f. 6
 * HDD 24TB SAS 7.2k: 0.39W/TB, f. 7.7
 * HDD 24TB SATA 7.2k: 0.375W/TB, f. 8
 * NVMe 15.36TB: 1W/TB, f. 5.4
 * SSD 7.68TB SATA: 0.46W/TB, f. 11.7
 * SSD 6.4TB SAS 12G: 1.5W/TB, f. 3.6


<!-- .slide: data-state="normal" id="findings-12" data-timing="20s" data-menu-title="Findings - Cost" -->
## Findings - Cost
### Issue 3: Network ports
### Issue 4: Support

* mainly driven by internal port costs
* Current Ceph setup vs NAS/NFS : 264 vs 16 ports¹
* Ceph 16.5 times higher port costs anually
  * means: high 4-digits vs low 6-digits
* SES canceled!

### K2BW1S: Ceph storage appliance
* only connecting ports count
* support and may management part of the appliance costs

Note:
¹) internal ports of appliances/blackboxes not counted
