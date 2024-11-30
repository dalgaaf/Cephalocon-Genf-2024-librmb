<!-- .slide: data-state="section-break" id="section-break-7" data-timing="10s" -->
# Proof-of-Concept


<!-- .slide: data-state="normal" id="status-1" data-timing="20s" data-menu-title="PoC" -->
## Limitations

* Updates but no upgrades of Ceph version
  * SES with `Nautilus`

* No hardware changes

* No placement changes
  * hardware
  * erasure coding

* No changes of deployment and automation

Note: we are aware of the limitiations caused by SUSE Product decisions (cancel SES), 
      and changes in deployment methodes, tried to minimize impact on the PoC even if this
      may end up in e.g. make not use of major improvements


<!-- .slide: data-state="normal" id="status-2" data-timing="20s" data-menu-title="PoC" -->
## Phase One

* <!-- .element: class="fragment" data-fragment-index="1" --> run load tests
  * <!-- .element: class="fragment" data-fragment-index="1" --> full synthetic
  * <!-- .element: class="fragment" data-fragment-index="1" --> customer-like patterns
* <!-- .element: class="fragment" data-fragment-index="2" --> run failure scenarios against Ceph
  * <!-- .element: class="fragment" data-fragment-index="2" --> all HW failures
  * <!-- .element: class="fragment" data-fragment-index="2" --> load impact
  * <!-- .element: class="fragment" data-fragment-index="2" --> recovery
  * <!-- .element: class="fragment" data-fragment-index="2" --> operational procedures
* <!-- .element: class="fragment" data-fragment-index="3" --> improve and fine-tune Ceph setup
* <!-- .element: class="fragment" data-fragment-index="4" --> verify and optimize hardware


<!-- .slide: data-state="normal" id="status-2" data-timing="20s" data-menu-title="PoC" -->
## Phase Two

* <!-- .element: class="fragment" data-fragment-index="1" --> limited real world test
  * <!-- .element: class="fragment" data-fragment-index="1" --> move real users to the platform
  * <!-- .element: class="fragment" data-fragment-index="1" --> step by step, increasing

* <!-- .element: class="fragment" data-fragment-index="2" --> identify issues with migration process
* <!-- .element: class="fragment" data-fragment-index="3" --> learn about real user load on Ceph
  * <!-- .element: class="fragment" data-fragment-index="3" --> identify and fix issues
  * <!-- .element: class="fragment" data-fragment-index="3" --> improve performance if required
* <!-- .element: class="fragment" data-fragment-index="4" --> create a base for final decision regarding HW, sizing and cost
