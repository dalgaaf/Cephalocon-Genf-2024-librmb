<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-1" data-timing="20s" data-menu-title="PoC" -->
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


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="PoC" -->
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


<!-- .slide: data-state="normal" id="findings-3" data-timing="20s" data-menu-title="PoC" -->
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


<!-- .slide: data-state="normal" id="findings-4" data-timing="20s" data-menu-title="PoC" -->
## Load at night 

* caused by dovecot cleanup processes (expire job)

TODO!!!


<!-- .slide: data-state="normal" id="findings-5" data-timing="20s" data-menu-title="PoC" -->
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


<!-- .slide: data-state="normal" id="findings-10" data-timing="20s" data-menu-title="Findings - Recovery Performance" -->
## Findings - Cost

TODO

### power consumptions
### space / density
### network
