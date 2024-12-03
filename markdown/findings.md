<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-1" data-timing="20s" data-menu-title="Findings - MDS Load" -->
## MDS load distribution

### Issue: MDS performance low in default setup <!-- .element class="fragment" data-fragment-index="1"-->

* bottlenecks <!-- .element class="fragment" data-fragment-index="2"-->
  * single threaded MDS code <!-- .element class="fragment" data-fragment-index="3"-->
  * numbers of files and directory structure <!-- .element class="fragment" data-fragment-index="3"-->
  * cache/memory default settings <!-- .element class="fragment" data-fragment-index="3"-->

### Solution: <!-- .element class="fragment" data-fragment-index="4"-->
  * multiple MDS per hardware node (8) <!-- .element class="fragment" data-fragment-index="5"-->
    * 16 active / 8 stand-by
  * dynamic balancer wasn't reliable in the cluster¹ <!-- .element class="fragment" data-fragment-index="5"-->
  * used subtree pinning instead <!-- .element class="fragment" data-fragment-index="5"-->

Note: 
* single threaded: MDS nodes under-utilized
¹ in nautilus
* MDS: 16 active, 8 standby, bad distribution since not div-by-3


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="Findings - Expunge" -->
## Nightly dovecot jobs

### Nightly expire/expunge jobs run too long <!-- .element class="fragment" data-fragment-index="1"-->

* runs nightly to e.g. delete emails from trash older than x days <!-- .element class="fragment" data-fragment-index="2"-->
* job generates load only on one MDS: bottleneck <!-- .element class="fragment" data-fragment-index="2"-->
* caused by file system structure and MDS subtree pinning <!-- .element class="fragment" data-fragment-index="2"-->

### Solution: <!-- .element class="fragment" data-fragment-index="3"-->
* moved job related directories to separate CephFS <!-- .element class="fragment" data-fragment-index="4"-->
* no subtree pinning on these MDS to distribute load <!-- .element class="fragment" data-fragment-index="4"-->


<!-- .slide: data-state="normal" id="findings-3" data-timing="20s" data-menu-title="Findings - MDS Failover" -->
## MDS Failover

### Issue: MDS Failover slightly slow <!-- .element class="fragment" data-fragment-index="1"-->

* initially we used only active and cold-standby MDSs (16+8) <!-- .element class="fragment" data-fragment-index="2"-->
* failover was slow under load <!-- .element class="fragment" data-fragment-index="3"-->
  * due to replay <!-- .element class="fragment" data-fragment-index="3"-->

### Solution <!-- .element class="fragment" data-fragment-index="4"-->
* added standby-replay servers <!-- .element class="fragment" data-fragment-index="5"-->
  * change numbers (16/16/16) <!-- .element class="fragment" data-fragment-index="5"-->
* caused new issue after some time <!-- .element class="fragment" data-fragment-index="6"-->
  * failover to standby-replay caused issues with left over locks <!-- .element class="fragment" data-fragment-index="6"-->
  * switched back to cold-standby, more reliable <!-- .element class="fragment" data-fragment-index="6"-->


<!-- .slide: data-state="normal" id="findings-4" data-timing="20s" data-menu-title="Findings - Mailbox repair" -->
## Recover broken mailbox indexes

### Issue: Recovery takes long time and produces high load <!-- .element class="fragment" data-fragment-index="1"-->

* lost or broken indexes require search trough the full namespaces <!-- .element class="fragment" data-fragment-index="2"-->
* listing all objects takes a very long time on Rados <!-- .element class="fragment" data-fragment-index="3"-->
  * not done in parallel <!-- .element class="fragment" data-fragment-index="4"-->
  * large number of objects <!-- .element class="fragment" data-fragment-index="4"-->

### Solution <!-- .element class="fragment" data-fragment-index="5"-->

* idea: distribute / parallelize the listing of objects <!-- .element class="fragment" data-fragment-index="6"-->
* final solution: <!-- .element class="fragment" data-fragment-index="7"-->
  * add additional index for mail oids per UUID <!-- .element class="fragment" data-fragment-index="7"-->
  * use as input for the repair <!-- .element class="fragment" data-fragment-index="7"-->

Note: https://github.com/ceph-dovecot/dovecot-ceph-plugin/issues/349


<!-- .slide: data-state="normal" id="findings-5" data-timing="20s" data-menu-title="Findings - Performance" -->
## Performance during initial import

### Issue: R/W performance low with increasing number of accounts <!-- .element class="fragment" data-fragment-index="1"-->

* Performance analysis <!-- .element class="fragment" data-fragment-index="2"-->
* Issues identified: <!-- .element class="fragment" data-fragment-index="3"-->
  * PGs size and number of objects per PG <!-- .element class="fragment" data-fragment-index="3"-->
    * autoscaling not used in nautilus <!-- .element class="fragment" data-fragment-index="3"-->
    * 45 GB / 190k objects per PG <!-- .element class="fragment" data-fragment-index="3"-->
  * num_strays defaults to low <!-- .element class="fragment" data-fragment-index="4"-->

### Solution: <!-- .element class="fragment" data-fragment-index="5"-->
  * PG splitting / double number of PGs <!-- .element class="fragment" data-fragment-index="6"-->
    * positiv side effect: deep scrubbing time cut in half <!-- .element class="fragment" data-fragment-index="6"-->
  * increased num_strays to 2m <!-- .element class="fragment" data-fragment-index="7"-->

Note:
* issue 594


<!-- .slide: data-state="normal" id="findings-10" data-timing="20s" data-menu-title="Findings - Cost" -->
## Findings - Cost
### Issue 1: Power Consumption <!-- .element class="fragment" data-fragment-index="1"-->
### Issue 2: Space / Density <!-- .element class="fragment" data-fragment-index="2"-->

* both topics connected <!-- .element class="fragment" data-fragment-index="3"-->
* power consumption per TB/account too high <!-- .element class="fragment" data-fragment-index="4"-->
* PoC hardware is not up-to-date¹ <!-- .element class="fragment" data-fragment-index="4"-->
  * energy efficiency per TByte increased for HDDs and SSD/NVMe <!-- .element class="fragment" data-fragment-index="4"-->

### Solution: update hardware concept <!-- .element class="fragment" data-fragment-index="5"-->
* higher density (2U: 4.6-13x higher capacity) <!-- .element class="fragment" data-fragment-index="6"-->
* higher efficiency CPUs <!-- .element class="fragment" data-fragment-index="6"-->
* higher density requires higher network bandwith / cooling <!-- .element class="fragment" data-fragment-index="6"-->
* may open options to use slightly more efficient EC <!-- .element class="fragment" data-fragment-index="6"-->

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
### Issue 3: Network ports <!-- .element class="fragment" data-fragment-index="1"-->
### Issue 4: Support <!-- .element class="fragment" data-fragment-index="2"-->

* mainly driven by internal port costs <!-- .element class="fragment" data-fragment-index="3"-->
* Current Ceph setup vs NAS/NFS : 264 vs 16 ports¹ <!-- .element class="fragment" data-fragment-index="4"-->
* Ceph 16.5 times higher port costs anually <!-- .element class="fragment" data-fragment-index="4"-->
  * means: high 4-digits vs low 6-digits <!-- .element class="fragment" data-fragment-index="4"-->
* SES canceled! <!-- .element class="fragment" data-fragment-index="5"-->

### K2BW1S: Ceph storage appliance <!-- .element class="fragment" data-fragment-index="6"-->
* only connecting ports count <!-- .element class="fragment" data-fragment-index="7"-->
* support and may management part of the appliance costs <!-- .element class="fragment" data-fragment-index="7"-->

Note:
¹) internal ports of appliances/blackboxes not counted
