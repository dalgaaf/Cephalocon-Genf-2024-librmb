<!-- .slide: data-state="section-break" id="section-break-1" data-timing="10s" -->
# TelekomMail Platform


<!-- .slide: data-state="normal" id="telekommail" data-timing="20s" data-menu-title="TelekomMail" -->
## TelekomMail

* DT's mail platform for customers <!-- .element class="fragment" data-fragment-index="1"-->
  * includes also voice mail files <!-- .element class="fragment" data-fragment-index="1"-->
* dovecot <!-- .element class="fragment" data-fragment-index="2"-->
  * LGPL 2.1, MIT <!-- .element class="fragment" data-fragment-index="2"-->
  * 77% market share (openemailsurvey.org, 2020) <!-- .element class="fragment" data-fragment-index="2"-->
* NAS with sharded NFS <!-- .element class="fragment" data-fragment-index="3"-->

### Dimensions¹ <!-- .element class="fragment" data-fragment-index="5"-->
* ~50.6 million accounts <!-- .element class="fragment" data-fragment-index="5"-->
* ~14.5 billion emails <!-- .element class="fragment" data-fragment-index="5"-->
* ~2,25 petabyte net storage <!-- .element class="fragment" data-fragment-index="6"-->
* ~42% usable raw space <!-- .element class="fragment" data-fragment-index="6"-->

Note:
- NFS with multiple RAID arrays
- emails are stored compressed
- Accounts +45%, +73% storage, +116% emails since last presentation 3y ago
- ¹) Numbers from End of 2022


<!-- .slide: data-state="normal" id="project-motivation" data-timing="20s" data-menu-title="Project Motivation" -->
## Motivation

### Explore new storage technologies

* technical <!-- .element class="fragment" data-fragment-index="0"-->
  * faster and automatic self healing <!-- .element class="fragment" data-fragment-index="1"-->
  * global namespace <!-- .element class="fragment" data-fragment-index="2"-->
  * less IO overhead <!-- .element class="fragment" data-fragment-index="3"-->
* business <!-- .element class="fragment" data-fragment-index="4"-->
  * prevent vendor lock-in <!-- .element class="fragment" data-fragment-index="5"-->
  * commodity hardware <!-- .element class="fragment" data-fragment-index="6"-->
  * open source where feasible <!-- .element class="fragment" data-fragment-index="7"-->
  * reduce Total Cost of Ownership <!-- .element class="fragment" data-fragment-index="8"-->

Note: 
- self healing: compared to sharded RAID setups
- overhead: compared to NFS getattr/access/lookups
- TOC: nice to have but very likely not possible within the first years due to required training and support
