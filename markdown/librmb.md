<!-- .slide: data-state="section-break" id="section-break-4" data-timing="10s" -->
# Dovecot and Ceph


<!-- .slide: data-state="normal" id="librmb-DT" data-timing="20s" data-menu-title="DT's approach" -->
## DT's approach
<div>
     <img style="position: absolute; width:30%; left: 63%;" alt="Partner"
          data-src="images/partner_latest.png" />
</div> <!-- .element class="fragment" data-fragment-index="4"-->

* no open source solution on the market <!-- .element class="fragment" data-fragment-index="0"-->
* closed source or licence no option <!-- .element class="fragment" data-fragment-index="1"-->
* develop / sponsor a solution <!-- .element class="fragment" data-fragment-index="2"-->
* open source it <!-- .element class="fragment" data-fragment-index="3"-->
* partner with: <!-- .element class="fragment" data-fragment-index="4"-->
  * `Tallence AG` for development <!-- .element class="fragment" data-fragment-index="4"-->
  * `SUSE` for Ceph <!-- .element class="fragment" data-fragment-index="4"-->

Note: 
- Dovecot Pro licence no option due to TCO impact. Model is to pay per account if active or inactive. With 39m accounts it would break the BC.


<!-- .slide: data-state="normal" id="librmb-DT-1" data-timing="20s" data-menu-title="Ceph Dovecot Plugin" -->
## Ceph plugin for Dovecot
### Hybrid approach <!-- .element class="fragment" data-fragment-index="0"-->

### Emails <!-- .element class="fragment" data-fragment-index="1"-->
* RADOS <!-- .element class="fragment" data-fragment-index="1"-->

### Metadata and indexes <!-- .element class="fragment" data-fragment-index="2"-->
* CephFS <!-- .element class="fragment" data-fragment-index="2"-->

### Generic email abstraction on top of librados <!-- .element class="fragment" data-fragment-index="4"-->
* Split code into libraries <!-- .element class="fragment" data-fragment-index="4"-->
* Give code back to corresponding upstream projects <!-- .element class="fragment" data-fragment-index="4"-->

Note: out of scope - user data and credential storage; full text search


<!-- .slide: data-state="normal" id="librmb-DT-2.1" data-timing="20s" data-menu-title="librmb" -->
## Librados mailbox (librmb)

<div>
     <img style="width:90%" alt="librmb architecture overview"
          data-src="images/dovecot-plugin-architecture-normal.svg" />
</div>


<!-- .slide: data-state="normal" id="librmb-DT-3" data-timing="20s" data-menu-title="librmb" -->
## It's open source!

<div>
    <img style="position: absolute; width: 65%; left: 45%;" alt="Github Project Screenshot"
         data-src="images/github-ceph-dovecot_new.png" />
</div> <!-- .element: class="fragment" data-fragment-index="2" -->

### <span>License: `LGPLv2.1`</span><!-- .element: class="fragment" data-fragment-index="0" -->

### <span>Language: `C++`</span> <!-- .element: class="fragment" data-fragment-index="1" -->

### Supported Dovecot versions: <!-- .element: class="fragment" data-fragment-index="2" -->
* 2.2 >= 2.2.21 <!-- .element: class="fragment" data-fragment-index="2" -->
* 2.3 <!-- .element: class="fragment" data-fragment-index="2" -->

### <span><a href="https://github.com/ceph-dovecot/">github.com/ceph-dovecot/</a></span> <!-- .element: class="fragment" data-fragment-index="3" -->

### still under development <!-- .element: class="fragment" data-fragment-index="4" -->


<!-- .slide: data-state="normal" id="ceph-version" data-timing="20s" data-menu-title="Ceph version" -->
## Which recommended Ceph Release?

<div>
     <img style="width: 50%; left: 47%; position: absolute" alt="ceph luminous"
          data-src="images/ceph_nautilus_fixed.svg" />
</div> <!-- .element: class="fragment" data-fragment-index="5" -->

### Required Features: <!-- .element: class="fragment" data-fragment-index="0" -->

* <!-- .element: class="fragment" data-fragment-index="1" --> Bluestore
  * <!-- .element: class="fragment" data-fragment-index="1" --> write performance is critical
* <!-- .element: class="fragment" data-fragment-index="2" --> CephFS
  * <!-- .element: class="fragment" data-fragment-index="2" --> Multi-MDS
* <!-- .element: class="fragment" data-fragment-index="3" --> Erasure coding
* <!-- .element: class="fragment" data-fragment-index="3" --> High level of security

### Enterprise currently products used: <!-- .element: class="fragment" data-fragment-index="6" -->
* <!-- .element: class="fragment" data-fragment-index="6" --> SES 6, SLES 15-SP1

Note:
- minimum release is Ceph Luminous
