<!-- .slide: data-state="section-break" id="section-break-7" data-timing="10s" -->
# Phase One


<!-- .slide: data-state="normal" id="phase-one-1" data-timing="20s" data-menu-title="PoC Phase One Setup" -->
## Setup

### physical setup as described before <!-- .element class="fragment" data-fragment-index="1"-->
  * Rados on HDDs, CephFS on SSDs <!-- .element class="fragment" data-fragment-index="2"-->
  * 3 MONs / MGR <!-- .element class="fragment" data-fragment-index="3"-->
  * 3 MDS nodes <!-- .element class="fragment" data-fragment-index="4"-->

### load generation from 36 servers (heads) <!-- .element class="fragment" data-fragment-index="5"-->
  * running typical IMAP jobs for simulated customer accounts <!-- .element class="fragment" data-fragment-index="5"-->
  * increasing number of accounts per head <!-- .element class="fragment" data-fragment-index="5"-->
  * filling up the cluster <!-- .element class="fragment" data-fragment-index="5"-->

Note: 
* MDS: 16 active, 8 standby, bad distribution since not div-by-3


<!-- .slide: data-state="normal" id="phase-one-2" data-timing="20s" data-menu-title="PoC Phase One Focus" -->
## Focus

### Performance <!-- .element class="fragment" data-fragment-index="1"-->

### Data safety/loss <!-- .element class="fragment" data-fragment-index="2"-->

### Processes <!-- .element class="fragment" data-fragment-index="3"-->

### Impact on customers <!-- .element class="fragment" data-fragment-index="4"-->
  * not every cluster issue will be visable to users <!-- .element class="fragment" data-fragment-index="4"-->
  * identify severity <!-- .element class="fragment" data-fragment-index="4"-->


<!-- .slide: data-state="normal" id="phase-one-3" data-timing="20s" data-menu-title="PoC Phase One Tests 1" -->
## Test scenarios

### typical server hardware related failures <!-- .element class="fragment" data-fragment-index="1"-->
  * one/two MON/MDS instances and nodes <!-- .element class="fragment" data-fragment-index="2"-->
  * one OSD capacity node (email) <!-- .element class="fragment" data-fragment-index="2"--> 
  * one OSD performance node (CephFS) <!-- .element class="fragment" data-fragment-index="2"-->
  * one OSD <!-- .element class="fragment" data-fragment-index="2"-->
  * one fire compartment, +2 OSDs <!-- .element class="fragment" data-fragment-index="2"-->

### typical network related failures <!-- .element class="fragment" data-fragment-index="3"-->
  * frontend/backend network of an OSD node <!-- .element class="fragment" data-fragment-index="4"-->
  * LACP check: disable 1-3 network ports of a node <!-- .element class="fragment" data-fragment-index="4"-->


<!-- .slide: data-state="normal" id="phase-one-4" data-timing="20s" data-menu-title="PoC Phase One Tests 2" -->
## Test scenarios

### admin scenarios <!-- .element class="fragment" data-fragment-index="1"-->
* removing/adding an OSD node <!-- .element class="fragment" data-fragment-index="2"-->
* ordered restart of the full Ceph Cluster <!-- .element class="fragment" data-fragment-index="2"-->
* update the network fabric switches under load <!-- .element class="fragment" data-fragment-index="2"-->
* update Ceph under load <!-- .element class="fragment" data-fragment-index="2"-->
* behavior of a completely filled cluster <!-- .element class="fragment" data-fragment-index="2"-->

### misc <!-- .element class="fragment" data-fragment-index="3"-->
* loosing time sync on a MON <!-- .element class="fragment" data-fragment-index="4"-->
* failure of SALT master <!-- .element class="fragment" data-fragment-index="4"-->
* restore admin node from backup/clone <!-- .element class="fragment" data-fragment-index="4"-->
* test performance parameter <!-- .element class="fragment" data-fragment-index="4"-->


<!-- .slide: data-state="normal" id="phase-one-5" data-timing="20s" data-menu-title="PoC Phase One Testscases" -->
## Selected Testcases
### 1FC + 2 OSDs down

<center><img data-src="images/test_BA_2OSD.png" style="width:75%"></center>

* noout flag set after power off
* tuned backfill/recovery prozesses during recovery

Note:
* 165m objects, 4k PGs ~20% filled
* power off:
  * 6 OSD capacity nodes
  * 3 OSD performance nodes
  * 1 MON node (1x MON, 1xMGR)
  * 1 MDS node (8x MDS)
* take out:
  * 1 OSD on perf node
  * 1 OSD on capacity node
* time line:
  * blue line: read, red: write
  * power off: 10:40, power on: 11:50, recovery complete: 2:15h
  * OSDs out: 11:15 and 11:25, back: 10:40
  * tuned: 12:01 (from 275 obj/s to 40k/s)


<!-- .slide: data-state="normal" id="phase-one-6" data-timing="20s" data-menu-title="PoC Phase One Testscases" -->
## Selected Testcases
### Cluster full 

### Procedure: <!-- .element class="fragment" data-fragment-index="1"-->
* fillup would take a long time: reduce capacity, set 9 nodes as out <!-- .element class="fragment" data-fragment-index="2"-->
* to avoid recovery: clean up,  would have taken ~24-26h¹, instead re-deploy nodes <!-- .element class="fragment" data-fragment-index="3"-->
* fillup to 90% with test data, start tests to fill up to 100% <!-- .element class="fragment" data-fragment-index="4"-->
* check cluster behaviour <!-- .element class="fragment" data-fragment-index="5"-->
* add new OSD nodes to check behaviour <!-- .element class="fragment" data-fragment-index="6"-->

### Result: <!-- .element class="fragment" data-fragment-index="7"-->
* affects clients <!-- .element class="fragment" data-fragment-index="8"-->
* painful long recovery <!-- .element class="fragment" data-fragment-index="8"-->
* prevent at all costs <!-- .element class="fragment" data-fragment-index="8"-->

Note:
¹ 100% load on RocksDB SSDs


<!-- .slide: data-state="normal" id="phase-one-7" data-timing="20s" data-menu-title="PoC Phase One Results" -->
## Selected Testcases
### Cluster full
<center><img data-src="images/test_fullyfilled.png" style="width:50%"></center>

Note:
- 7:20 added first node with 10 OSDs, cluster starts reballance/recovery
- 7:30 next node
- 8:00 next node: still in status "1 pool full"
- 8:45 add another 2 nodes
- cluster in status full till 14:15, 7h after adding the first node.
- clients massivly affected


<!-- .slide: data-state="normal" id="phase-one-8" data-timing="20s" data-menu-title="PoC Phase One Testscases" -->
## Results

### in total 22 major test scenarios <!-- .element class="fragment" data-fragment-index="1"-->

### none failed <!-- .element class="fragment" data-fragment-index="2"-->
  * no unexpected behavior <!-- .element class="fragment" data-fragment-index="3"-->
  * no major unpreventable impact for customers <!-- .element class="fragment" data-fragment-index="3"-->
  * no data loss <!-- .element class="fragment" data-fragment-index="3"-->

### Compared to NFS <!-- .element class="fragment" data-fragment-index="4"-->
  * expected behaviour <!-- .element class="fragment" data-fragment-index="5"-->
  * comparable performance to HDD based NFS <!-- .element class="fragment" data-fragment-index="5"-->

Note: cluster may unresponsive for short period of time, unlikely to be visible for customer (due to cashes and queues)
