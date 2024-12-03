<!-- .slide: data-state="section-break" id="section-break-4" data-timing="10s" -->
# Dovecot and Ceph


<!-- .slide: data-state="normal" id="librmb-DT" data-timing="20s" data-menu-title="DT's approach" -->
## DT's approach
<div>
     <img style="position: absolute; width:30%; left: 63%;" alt="Partner"
          data-src="images/partner_latest_2.png" />
</div> <!-- .element class="fragment" data-fragment-index="4"-->

* no open source solution on the market <!-- .element class="fragment" data-fragment-index="0"-->
* closed source or licence no option <!-- .element class="fragment" data-fragment-index="1"-->
* develop / sponsor a solution <!-- .element class="fragment" data-fragment-index="2"-->
* open source it <!-- .element class="fragment" data-fragment-index="3"-->
* started 2016/2017 <!-- .element class="fragment" data-fragment-index="4"-->
* partner(ed) with: <!-- .element class="fragment" data-fragment-index="5"-->
  * `Tallence AG` for development <!-- .element class="fragment" data-fragment-index="5"-->
  * <del>`SUSE`</del> for Ceph <!-- .element class="fragment current-visibl" data-fragment-index="5"-->
  * <del>`Wido den Hollander`</del> (42on.com) <!-- .element class="fragment current-visibl" data-fragment-index="5"-->

Note: 
- Dovecot Pro licence no option due to TCO impact. Model is to pay per account if active or inactive. With 39m accounts it would break the BC.
- partnered with SUSE till end of PoC, since Ceph product discontinued


<!-- .slide: data-state="normal" id="librmb-DT-2.1" data-timing="20s" data-menu-title="librmb" -->
## Ceph plugin for Dovecot

<div>
     <img style="width:90%" alt="librmb architecture overview"
          data-src="images/dovecot-plugin-architecture-normal.svg" />
</div>

Note:
* Hybrid approach: emails in RADOS, Metadata/indexes in CephFS
* Generic email abstraction on top of librados
* Split code into libraries, Give code back to corresponding upstream projects
* out of scope: user data management and credential storage; full text search


<!-- .slide: data-state="normal" id="librmb-DT-3" data-timing="20s" data-menu-title="librmb" -->
## It's open source!

<div>
    <img style="position: absolute; width: 55%; left: 45%;" alt="Github Project Screenshot"
         data-src="images/github-ceph-dovecot_new.png" />
</div> <!-- .element: class="fragment" data-fragment-index="2" -->

### <span>License: `LGPLv2.1`</span><!-- .element: class="fragment" data-fragment-index="0" -->

### <span>Language: `C++`</span> <!-- .element: class="fragment" data-fragment-index="1" -->

### Current version: <!-- .element: class="fragment" data-fragment-index="2" -->
* v1.0.0 <!-- .element: class="fragment" data-fragment-index="2" -->
* since June 2023 <!-- .element: class="fragment" data-fragment-index="2" -->

### <span><a href="https://github.com/ceph-dovecot/">github.com/ceph-dovecot/</a></span> <!-- .element: class="fragment" data-fragment-index="3" -->

