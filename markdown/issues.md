<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-7" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Performance

### CephFS Troubleshooting <!-- .element: class="fragment" data-fragment-index="0" -->
* too low number of PGs by default <!-- .element: class="fragment" data-fragment-index="1" -->
  * increased, no improvement <!-- .element: class="fragment" data-fragment-index="1" -->
* MDS Cache sizes increased <!-- .element: class="fragment" data-fragment-index="2" -->
  * no significant improvement! <!-- .element: class="fragment" data-fragment-index="2" -->
* cephfs_metadata pool set from 6x to 3x <!-- .element: class="fragment" data-fragment-index="3" -->
  * no significant improvement! <!-- .element: class="fragment" data-fragment-index="3" -->
* Multi-MDS <!-- .element: class="fragment" data-fragment-index="4" -->
  * dynamic balancer: caused unpredictable performance <!-- .element: class="fragment" data-fragment-index="5" -->
  * subtree pinning: predictable performance <!-- .element: class="fragment" data-fragment-index="5" -->
  * Multi-MDS per server required changes in SES automation: improved performance <!-- .element: class="fragment" data-fragment-index="5" -->


<!-- .slide: data-state="normal" id="findings-7.1" data-timing="20s" data-menu-title="Findings - Performance - Multi-MDS" -->
## Findings - Performance Multi-MDS
<canvas data-chart="line">
<!--
{
 "data" : {
     "labels": ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"],
     "datasets": [
         {
             "label": "1 MDS(p)",
             "borderColor":"rgba(227, 26, 28, 0.5)",
             "fill": "false",
             "data": [8, 19, 29, 34, 50, 79, 160, 216, 281, 415]
         },
         {
             "label": "2 MDS(p)",
             "borderColor":"rgba(166, 206, 227, 0.7)",
             "fill": "false",
             "data": [6, 7, 9, 12, 15, 20, 19, 28, 38, 62]
         },
         {
             "label": "4 MDS(p)",
             "borderColor":"rgba(51, 160, 44, 1.0)",
             "fill": "false",
             "data": [7, 7, 8, 9, 10, 11, 11, 15, 15, 16]
         }
     ]
 },
 "options": {
     "fill": "false",
     "animateScale": "true",
     "responsive": "true",
     "legend": {
           "display": 1
     },
     "layout": {
            "padding": {
                "left": 20,
                "right": 20,
                "top": 40,
                "bottom": 0
            }
     },
     "plugins": {
         "datalabels": {
             "align": "end",
             "anchor": "end"
         }
     },
     "scales": {
         "yAxes": [{
	     "type": "logarithmic",
             "gridLines": {
                 "color": "rgba(0, 0, 0, 0)"
             },
	     "scaleLabel": {
	        "display": 1,
		"labelString": "log(ms/cmd avg)"
	     },
             "ticks": {
	         "min": 7,
                 "display": 0
             }
         }],
         "xAxes": [{
             "gridLines": {
                 "color": "rgba(0, 0, 0, 0)"
             },
	     "scaleLabel": {
	        "display": 1,
		"labelString": "# of server running imaptest with 1500 clients each"
	     }
         }]
     }
 }
}
-->
</canvas>


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


<!-- .slide: data-state="normal" id="findings-9" data-timing="20s" data-menu-title="Findings - Recovery Performance" -->
## Findings - Recovery Performance

### Drawbacks of billions of emails ???

* Recovery/Backfill <!-- .element: class="fragment" data-fragment-index="0" -->
  * 1200 OSDs would have at least 32k PGs <!-- .element: class="fragment" data-fragment-index="1" -->
  * k4m5 cause 9 chunks per object <!-- .element: class="fragment" data-fragment-index="2" -->
  * 7 billion emails -> 63 billion objects, ~2m objects per PG <!-- .element: class="fragment" data-fragment-index="3" -->
  * Recovery is object based, big impact on HDDs <!-- .element: class="fragment" data-fragment-index="4" -->
  * Cluster with 100 OSDs, 40m emails <!-- .element: class="fragment" data-fragment-index="5" -->
    * failure of 1 OSD requires ~4m objects to be moved <!-- .element: class="fragment" data-fragment-index="5" -->


<!-- .slide: data-state="normal" id="findings-10" data-timing="20s" data-menu-title="Findings - Recovery Performance" -->
## Findings - Recovery Performance

### Alternatives
* Move from HDD to SSD depending on TCO impact. <!-- .element: class="fragment" data-fragment-index="0" -->
* Base code on dbox like format ? <!-- .element: class="fragment" data-fragment-index="1" -->
  * impact due to EC rewrite performance <!-- .element: class="fragment" data-fragment-index="2" -->
  * may add SSD pool to store boxes till full <!-- .element: class="fragment" data-fragment-index="3" -->
    * when dbox full, write to HDD <!-- .element: class="fragment" data-fragment-index="3" -->
    * if dbox not full, move after defined time to HDD <!-- .element: class="fragment" data-fragment-index="3" -->




<!-- .slide: data-state="normal" id="findings-14" data-timing="20s" data-menu-title="Conclusion" -->
## Learnings

### The technical part works in general! <!-- .element: class="fragment" data-fragment-index="0" -->
### Performance issues will be indentified and fixed! <!-- .element: class="fragment" data-fragment-index="1" -->

### Project success depends on 3 components <!-- .element: class="fragment" data-fragment-index="2" -->
* People <!-- .element: class="fragment" data-fragment-index="3" -->
* People <!-- .element: class="fragment" data-fragment-index="4" -->
* People <!-- .element: class="fragment" data-fragment-index="5" -->

### You need motivated Dev/Ops! <!-- .element: class="fragment" data-fragment-index="6" -->

