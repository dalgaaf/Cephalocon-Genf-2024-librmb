<!-- .slide: data-state="section-break" id="section-break-7" data-timing="10s" -->
# Phase Two


<!-- .slide: data-state="normal" id="status-1" data-timing="20s" data-menu-title="PoC" -->
## Objectives and Procedure

* Test with real customers
  * randomly selected
  * reflecting typical customer distribution
    * active and inactive mail accounts
    * size¹

* move accounts from NFS to Ceph
  * test account migration
  * identify cluster limits
  * run cluster for ~6 months
  * learn from typical cluster tasks and behaviour
* move accounts back to NFS after PoC

Note: 
* ¹excluded accounts which have emails which exceed default osd_max_object_size


<!-- .slide: data-state="normal" id="status-2" data-timing="20s" data-menu-title="PoC" -->
## Results

* ~1.1 million accounts moved to Ceph with:
  * ~440 million emails
  * ~4 billion EC chunks
  * 434 TByte use capacity from Dovecot view
  * 370 TiB raw used capacity from Ceph view
  * CephFS Cluster: 25 TiB used

* successfull!
  * no customer impact
  * no data loss


<!-- .slide: data-state="normal" id="status-3" data-timing="20s" data-menu-title="PoC" -->
## Results

<center><img data-src="images/grafana_full_view.png" style="width:95%"></center>

