<!-- .slide: data-state="section-break" id="section-break-6" data-timing="10s" -->
# Placement


<!-- .slide: data-state="normal" id="placement-0" data-timing="20s" data-menu-title="Data Center" -->
## Data Center 

### Former assumption
* Two independent fire compartments (FCs)
* May additional virtual FCs

### Updated
* Production will be placed in a new DC
* \>3 independent fire compartments
* Network will fit bandwidth requirements

Note: 
- requirements adapted since the last presentation


<!-- .slide: data-state="normal" id="placement-3" data-timing="20s" data-menu-title="3FCs" -->
### Current setup
<div>
  <center><img data-src="images/fc-ceph-EC+3xReplication-color.svg" style="width:75%"></center>
</div>

Note: current intermediate state


<!-- .slide: data-state="normal" id="placement-3" data-timing="20s" data-menu-title="3FCs" -->
### PoC Setup
<div>
  <center><img data-src="images/fc-ceph-EC-color_white_v2.svg" style="width:75%"></center>
</div>

Note:
- cluster gets extended 18 HDD nodes on 3 FCs
- for better distribution, especially for EC

<!-- .slide: data-state="normal" id="placement-net-2" data-timing="20s" data-menu-title="Network Overview" -->
<div>
  <center><img data-src="images/network-infra-mailplatform_v1.svg" style="width:85%"></center>
</div>

Note: 
- 1G OAM
- 4x10G ports per node, SFP+ DAC
- MC-LAG/M-LAG for aggregation and failover
- 40 or 80G interconnect between FCs and to spine
- interconnect must not reflect theoretical rack/FC bandwidth
- L2 terminated in rack, L3 for rest


<!-- .slide: data-state="normal" id="placement-1" data-timing="20s" data-menu-title="Data safety" -->
## Requirements for Production

* Lost of customer data __MUST__ be prevented at any cost
* At least replica 3 or comparable

* Failure scenarios to be covered:
  * 1 of 3 fire compartments (FCs)
  * 1 FC + 1 Server (or HDD/SSD)
  * 1 FC + 1 Server + 1 HDD/SSD

* Cluster __MUST__ be fully operational (R/W) in any scenario
* Additonal failure __MUST__ not cause data loss
* Applies to CephFS and Rados part

Note: 
- additional error sets cluster in hold, no writes to the cluster any more, cause stop of mail system.
- max. failure scenario would be 4 independet errors before cluster stops to be responsive, even then no data loss.