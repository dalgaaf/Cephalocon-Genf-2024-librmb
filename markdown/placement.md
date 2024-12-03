<!-- .slide: data-state="section-break" id="section-break-6" data-timing="10s" -->
# Cluster Setup


<!-- .slide: data-state="normal" id="placement-1" data-timing="20s" data-menu-title="3FCs" -->
## Current setup
<div>
  <center><img data-src="images/fc-ceph-EC-color_white_v2.svg" style="width:75%"></center>
</div>

Note:
- 1G OAM
- 4x10G ports per node, SFP+ DAC
- MC-LAG/M-LAG for aggregation and failover
- 40 or 80G interconnect between FCs and to spine
- interconnect must not reflect theoretical rack/FC bandwidth
- L2 terminated in rack, L3 for rest
- will be updated to 100G + may 25G to the Ceph nodes


<!-- .slide: data-state="normal" id="placement-2" data-timing="20s" data-menu-title="Data safety" -->
## Requirements for Production

* <!-- .element: class="fragment" data-fragment-index="0" --> Lost of customer data <b>MUST</b> be prevented at any cost
* At least replica 3 or comparable <!-- .element: class="fragment" data-fragment-index="1" -->
* <!-- .element: class="fragment" data-fragment-index="2" --> Cluster <b>MUST</b> be fully operational (R/W) in any scenario

### <!-- .element: class="fragment" data-fragment-index="4" --> Failure scenarios to be covered: 
* <!-- .element: class="fragment" data-fragment-index="5" --> 1 of 3 fire compartments (FCs)
* <!-- .element: class="fragment" data-fragment-index="5" --> 1 FC + 1 Server (or HDD/SSD)
* <!-- .element: class="fragment" data-fragment-index="5" --> 1 FC + 1 Server + 1 HDD/SSD 
* <!-- .element: class="fragment" data-fragment-index="6" --> Additonal failure <b>MUST</b> not cause data loss

Note: 
- requirements: apply to whole cluster, Rados and CephFS
- additional error sets cluster in hold, no writes to the cluster any more, cause stop of mail system.
- max. failure scenario would be 4 independet errors before cluster stops to be responsive, even then no data loss.


<!-- .slide: data-state="normal" id="EC-10" data-timing="20s" data-menu-title="Cluster settings" -->
## Settings

### Rados <!-- .element: class="fragment" data-fragment-index="0" -->
* k4m5 <!-- .element: class="fragment" data-fragment-index="0" -->

### CephFS <!-- .element: class="fragment" data-fragment-index="1" -->
* data: 6x replication <!-- .element: class="fragment" data-fragment-index="1" -->
* meta_data: 6x replication <!-- .element: class="fragment" data-fragment-index="1" -->
* Ops may need to intervene <!-- .element: class="fragment" data-fragment-index="1" -->
* Cluster able to run recovery <!-- .element: class="fragment" data-fragment-index="1" -->

### Production <!-- .element: class="fragment" data-fragment-index="2" -->
* EC/Replication need to change again <!-- .element: class="fragment" data-fragment-index="2" -->

Note:
- Rados settings: comparable regarding chunks written, PoC less overhead
- Rados: comparabel usable space to NAS/NFS
- CephFS settings same for both
- Ceph: In case of max error OPS need to inervene to ensure max_size=k+1 handling.

