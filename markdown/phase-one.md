<!-- .slide: data-state="section-break" id="section-break-7" data-timing="10s" -->
# Phase One


<!-- .slide: data-state="normal" id="phase-one-1" data-timing="20s" data-menu-title="PoC Phase One Setup" -->
## Setup

* physical setup as described before
  * Rados on HDDs, CephFS on SSDs
  * 3 MONs
  * 24 MDS on 3 nodes
    * 16 active
    * 8 standby

* load generation from 36 servers (heads) 
  * running typical IMAP jobs for simulated customer accounts
  * increasing number of accounts per head
  * filling up the cluster


<!-- .slide: data-state="normal" id="phase-one-2" data-timing="20s" data-menu-title="PoC Phase One Focus" -->
## Focus

* Performance

* Data safety/loss

* Processes

* Impact on customers
  * not every cluster issue will be visable to users
  * identify severity 


<!-- .slide: data-state="normal" id="phase-one-3" data-timing="20s" data-menu-title="PoC Phase One Tests 1" -->
## Test scenarios

* typical hardware related failures
  * one/two MON/MDS instances and nodes
  * one OSD capacity node (email)
  * one OSD performance node (CephFS)
  * one OSD
  * one fire compartment, +2 OSDs

* typical network related failures
  * frontend/backend network of an OSD node
  * LACP check: disable 1-3 network ports of a node


<!-- .slide: data-state="normal" id="phase-one-4" data-timing="20s" data-menu-title="PoC Phase One Tests 2" -->
## Test scenarios

* admin scenarios
  * removing/adding an OSD node
  * ordered restart of the full Ceph Cluster
  * update the network fabric switches under load
  * update Ceph under load
  * behavior of a completely filled cluster

* misc
  * loosing time sync on a MON
  * failure of SALT master
  * restore admin node from backup/clone
  * test performance parameter


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

Procedure:
* fillup would take a long time: reduce capacity, set 9 nodes as out
* to avoid recovery: clean up rados_mail, would have taken ~24-26h¹, instead re-deloy nodes
* fillup to 90% with test data, start tests to fill up to 100%
* check cluster behaviour
* add new OSD nodes to check behaviour

Result:
* affects clients
* painful long recovery
* prevent at all costs

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

* in total 22 major test scenarios

* none failed
  * no unexpected behavior
  * no major unpreventable impact for customers
  * no data loss

* Compared to NFS
  * expected behaviour
  * comparable performance to HDD based NFS

Note: cluster may unresponsive for short period of time, unlikely to be visible for customer (due to cashes and queues)
