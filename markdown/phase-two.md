<!-- .slide: data-state="section-break" id="section-break-7" data-timing="10s" -->
# Phase Two


<!-- .slide: data-state="normal" id="status-1" data-timing="20s" data-menu-title="PoC" -->
## Objectives and Procedure

### Test with real customers <!-- .element class="fragment" data-fragment-index="1"-->
  * randomly selected <!-- .element class="fragment" data-fragment-index="2"-->
  * reflecting typical customer distribution <!-- .element class="fragment" data-fragment-index="2"-->
    * active and inactive mail accounts <!-- .element class="fragment" data-fragment-index="2"-->
    * size¹ <!-- .element class="fragment" data-fragment-index="2"-->

### move accounts from NFS to Ceph <!-- .element class="fragment" data-fragment-index="3"-->
  * test account migration <!-- .element class="fragment" data-fragment-index="4"-->
  * identify cluster limits <!-- .element class="fragment" data-fragment-index="4"-->
  * run cluster for ~6 months <!-- .element class="fragment" data-fragment-index="4"-->
  * learn from typical cluster tasks and behaviour <!-- .element class="fragment" data-fragment-index="4"-->

### move accounts back to NFS after PoC <!-- .element class="fragment" data-fragment-index="5"-->

Note: 
* ¹excluded accounts which have emails which exceed default osd_max_object_size


<!-- .slide: data-state="normal" id="status-2" data-timing="20s" data-menu-title="PoC" -->
## Results

### ~1.1 million accounts moved to Ceph with: <!-- .element class="fragment" data-fragment-index="1"-->
  * ~440 million emails <!-- .element class="fragment" data-fragment-index="2"-->
  * ~4 billion EC chunks <!-- .element class="fragment" data-fragment-index="2"-->
  * 434 TByte use capacity from Dovecot view <!-- .element class="fragment" data-fragment-index="2"-->
  * 406 TByte raw used capacity from Ceph view <!-- .element class="fragment" data-fragment-index="2"-->
  * CephFS Cluster: 27.5 TByte used <!-- .element class="fragment" data-fragment-index="2"-->

### successfull! <!-- .element class="fragment" data-fragment-index="3"-->
  * no customer impact <!-- .element class="fragment" data-fragment-index="4"-->
  * no data loss <!-- .element class="fragment" data-fragment-index="4"-->


<!-- .slide: data-state="normal" id="status-3" data-timing="20s" data-menu-title="PoC" -->
## Results

<center><img data-src="images/grafana_full_view.png" style="width:95%"></center>

