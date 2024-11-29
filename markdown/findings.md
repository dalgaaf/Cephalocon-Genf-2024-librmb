<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-1" data-timing="20s" data-menu-title="PoC" -->
## MDS load distribution


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="PoC" -->
## MDS Failover


<!-- .slide: data-state="normal" id="findings-3" data-timing="20s" data-menu-title="PoC" -->
## Recover broken mailboxes

* reqires search trough the indexes/namespaces


<!-- .slide: data-state="normal" id="findings-4" data-timing="20s" data-menu-title="PoC" -->
## Load at night 

* caused by dovecot cleanup processes (expire job)


<!-- .slide: data-state="normal" id="findings-5" data-timing="20s" data-menu-title="PoC" -->
## Number of PGs


<!-- .slide: data-state="normal" id="findings-8" data-timing="20s" data-menu-title="Findings - Partial Rewrite" -->
## Findings - Partial Rewrite Performance

### Dovecot behaviour on index/cache/log files: <!-- .element: class="fragment" data-fragment-index="1" -->
  * cause small changes/adds on these files <!-- .element: class="fragment" data-fragment-index="1" -->
  * requires partial rewrites <!-- .element: class="fragment" data-fragment-index="1" -->

### Partial rewrite changes: <!-- .element: class="fragment" data-fragment-index="2" -->
  * fully read file from the OSDs <!-- .element: class="fragment" data-fragment-index="2" -->
  * change file <!-- .element: class="fragment" data-fragment-index="2" -->
  * recalculate chunks <!-- .element: class="fragment" data-fragment-index="2" -->
  * rewrite to cluster <!-- .element: class="fragment" data-fragment-index="2" -->
  * -> at least read+write <!-- .element: class="fragment" data-fragment-index="2" -->

### high impact on performance <!-- .element: class="fragment" data-fragment-index="3" -->


<!-- .slide: data-state="normal" id="findings-10" data-timing="20s" data-menu-title="Findings - Recovery Performance" -->
## Findings - Cost

### power consumptions
### space / density
### network
