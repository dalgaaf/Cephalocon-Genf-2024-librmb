<!-- .slide: data-state="section-break" id="section-break-8" data-timing="10s" -->
# Conclusion


<!-- .slide: data-state="normal" id="conclusion-0" data-timing="20s" data-menu-title="Conclusion" -->
## Summary and conclusions

### Ceph can replace NFS for emails <!-- .element: class="fragment" data-fragment-index="0" -->
* even in large enterprise setups <!-- .element: class="fragment" data-fragment-index="0" -->

### Optimize cluster concept <!-- .element: class="fragment" data-fragment-index="1" -->
* higher density, SSD only¹ <!-- .element: class="fragment" data-fragment-index="2" -->
* faster network (100G), decrease port numbers <!-- .element: class="fragment" data-fragment-index="3" -->
* replace discontinued SES <!-- .element: class="fragment" data-fragment-index="4" -->
* partner to provide <!-- .element: class="fragment" data-fragment-index="6" -->
  * storage appliance per rack <!-- .element: class="fragment" data-fragment-index="5" -->
  * up-to-date Ceph version <!-- .element: class="fragment" data-fragment-index="5" -->
  * support for Ceph <!-- .element: class="fragment" data-fragment-index="5" -->
  * plugin optimization <!-- .element: class="fragment" data-fragment-index="5" -->
<div>
     <img style="position: absolute; width:30%; left: 50%; margin: -130px 50px" alt="Clyso"
          data-src="images/CLYSO-Logo_black.svg" />
</div> <!-- .element class="fragment" data-fragment-index="6"-->

Note:
¹) also HDDs, especially for inactive accounts would make may sense, could be handled via Dovecot without Tiering in Ceph.
