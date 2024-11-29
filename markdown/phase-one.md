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
  * one/two MON/MDS nodes
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



<!-- .slide: data-state="normal" id="phase-one-6" data-timing="20s" data-menu-title="PoC Phase One Results" -->
## Results

* in total 22 major test scenarios

* none failed
  * no unexpected behavior
  * no major impact for customers
  * no data loss

Note: cluster may unresponsive for short period of time, unlikely to be visible for customer (due to cashes and queues)
