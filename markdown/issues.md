<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-0" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Performance (Luminous)
<canvas data-chart="line">
<!--
{
 "data" : {
     "labels": ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"],
     "datasets": [
         {
             "label": "librmb@CephFS+Rados",
             "borderColor":"rgba(227, 26, 28, 0.8)",
             "fill": "false",
             "data": [84, 141, 209, 290, 425, 1191, 2765, 7670, 12804, 21062]
         },
         {
             "label": "librmb@LocalFS+Rados",
             "borderColor":"rgba(51, 160, 44, 0.8)",
             "fill": "false",
             "data": [22, 21, 18, 29, 15, 19, 18, 27, 17, 21]
         },
         {
             "label": "NFS",
             "borderColor":"rgba(166, 206, 227, 1.0)",
             "fill": "false",
             "data": [10, 7, 9, 7, 16, 12, 10, 12, 15, 13]
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

Note:
- tests done with SES5.5
- Rados performance is okay-ish
- CephFS is the issue, imaptest performance way to slow with EC
- metadata/indexes o local HDD significant faster
- no signifcant load on OSDs/MDS
- find with 3million files: 16sec, second run the same


<!-- .slide: data-state="normal" id="findings-1" data-timing="20s" data-menu-title="Findings - Performance - mds" -->
## Findings - Performance (Luminous)
<canvas data-chart="bar">
<!--
{
 "data" : {
     "labels": ["req_create_latency", "req_getfilelock_latency", "req_link_latency", "req_lookup_latency", "req_mkdir_latency"],
     "datasets": [
         {
             "borderColor":"rgba(227, 26, 28, 0.8)",
	     "backgroundColor": "rgba(227, 26, 28, 0.5)",
             "label": "EC+Replication",
             "data": [361, 195, 269, 341, 1085]
         },
         {
             "borderColor":"rgba(51, 160, 44, 0.8)",
             "backgroundColor":"rgba(51, 160, 44, 0.5)",
             "label": "Replication (other cluster)",
             "data": [1.5, 0.35, 2.6, 0.6, 1.35]
         }
     ]
 },
 "options": {
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
		"labelString": "log(avgtime * 1000)"
	     },
             "ticks": {
                 "display": 0
             }
         }],
         "xAxes": [{
             "gridLines": {
                 "color": "rgba(0, 0, 0, 0)"
             },
	     "scaleLabel": {
	        "display": 1,
		"labelString": "msd perf dump"
	     }
         }]
     }
 }
}
-->
</canvas>

Note: 
- These are different Cluster. 
- DT CephFS cluster runs on SSD+EC+Replication
- CephFS replication cluster is on HDD


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="Findings - Performance - CephFS" -->
## Findings - Performance (Luminous)
<canvas data-chart="line">
<!--
{
 "data" : {
     "labels": ["1", "2", "3", "4", "5"],
     "datasets": [
         {
             "label": "3-rooms@CephFS",
             "borderColor":"rgba(227, 26, 28, 0.5)",
             "fill": "false",
             "data": [84, 141, 209, 290, 425]
         },
         {
             "label": "3-rooms@NFS+Rados",
             "borderColor":"rgba(166, 206, 227, 0.7)",
             "fill": "false",
             "data": [21, 33, 28, 24, 29]
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


<!-- .slide: data-state="normal" id="findings-3" data-timing="20s" data-menu-title="Findings - Performance - mds" -->
## Findings - Performance (Luminous)
<canvas data-chart="line">
<!--
{
 "data" : {
     "labels": ["1", "2", "3", "4", "5"],
     "datasets": [
         {
             "label": "3-rooms@CephFS",
             "borderColor":"rgba(227, 26, 28, 0.5)",
             "fill": "false",
             "data": [84, 141, 209, 290, 425]
         },
         {
             "label": "3-rooms@NFS+Rados",
             "borderColor":"rgba(166, 206, 227, 0.7)",
             "fill": "false",
             "data": [21, 33, 28, 24, 29]
         },
         {
             "label": "1-room-A@CephFS",
             "borderColor":"rgba(51, 160, 44, 1.0)",
             "fill": "false",
             "data": [8, 9, 11, 20, 33]
         },
         {
             "label": "1-room-B@CephFS",
             "borderColor":"rgba(255, 127, 0, 1.0)",
             "fill": "false",
             "data": [50, 56, 65, 223, 294]
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


<!-- .slide: data-state="normal" id="findings-4" data-timing="20s" data-menu-title="Findings - Performance - Rooms" -->
## Findings - Performance

### uneven performance between rooms <!-- .element: class="fragment" data-fragment-index="0" -->
### single room faster than multiple <!-- .element: class="fragment" data-fragment-index="1" -->
 * expected due to less network hops <!-- .element: class="fragment" data-fragment-index="2" -->
 * but higher load should be better served by more servers <!-- .element: class="fragment" data-fragment-index="2" -->

### root cause: network <!-- .element: class="fragment" data-fragment-index="3" -->
  * volatile number of retransmitts <!-- .element: class="fragment" data-fragment-index="4" -->
  * Switches discards packages <!-- .element: class="fragment" data-fragment-index="4" -->
  * 40G to 4x10G Breakout cables caused trouble <!-- .element: class="fragment" data-fragment-index="5" -->
  * different firmware version on Intel NICs <!-- .element: class="fragment" data-fragment-index="6" -->


<!-- .slide: data-state="normal" id="findings-5" data-timing="20s" data-menu-title="Findings - Performance Degradation" -->
## Findings - Performance Degradation

<canvas data-chart="line">
<!--
{
 "data" : {
     "labels": ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"],
     "datasets": [
         {
             "label": "MDS-A (re)start",
             "borderColor":"rgba(227, 26, 28, 0.8)",
             "fill": "false",
             "data": [8, 9, 21, 48, 97, 147, 165, 210, 261, 295]
         },
         {
             "label": "MDS-A next morning",
             "borderColor":"rgba(51, 160, 44, 0.8)",
             "fill": "false",
             "data": [112, 179, 444, 535, 785, 991, 1206, 979, 1755, 1521]
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


<!-- .slide: data-state="normal" id="findings-6" data-timing="20s" data-menu-title="Findings - Performance Degradation" -->
## Findings - Performance Degradation

* restart of MDS helps <!-- .element: class="fragment" data-fragment-index="0" -->
* switching to another MDS improves performance <!-- .element: class="fragment" data-fragment-index="1" -->
* drop of caches doesn't help <!-- .element: class="fragment" data-fragment-index="2" -->
* expected to (may) be gone with SES 6.0 <!-- .element: class="fragment" data-fragment-index="3" -->


<!-- .slide: data-state="normal" id="findings-7" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Performance

### CephFS Troubleshooting <!-- .element: class="fragment" data-fragment-index="0" -->
* multi-active MDS <!-- .element: class="fragment" data-fragment-index="2" -->
  * no relevant improvement <!-- .element: class="fragment" data-fragment-index="2" -->
* too low number of PGs by default <!-- .element: class="fragment" data-fragment-index="3" -->
  * increased, no improvement <!-- .element: class="fragment" data-fragment-index="3" -->
* MDS Cache sizes increased <!-- .element: class="fragment" data-fragment-index="4" -->
  * no significant improvement! <!-- .element: class="fragment" data-fragment-index="4" -->
* cephfs_metadata pool set from 6x to 3x <!-- .element: class="fragment" data-fragment-index="5" -->
  * no significant improvement! <!-- .element: class="fragment" data-fragment-index="5" -->


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
  * 7 billion emails lead to 63 billion objects, ~2m objects per PG <!-- .element: class="fragment" data-fragment-index="3" -->
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


<!-- .slide: data-state="normal" id="findings-11" data-timing="20s" data-menu-title="Findings - Operations" -->
## Findings - Operations

* Which EC profile is assigned to a pool?

```bash
$> ceph osd pool ls detail | grep cephfs_data
pool 6 'cephfs_data' erasure size 6 min_size 3 crush_rule 1 \
    		     object_hash rjenkins pg_num 2048 \
		     pgp_num 2048 last_change 8199 lfor 0/4568 \
		     flags hashpspool,ec_overwrites,nodeep-scrub \
		     stripe_width 8192 application cephfs
```


<!-- .slide: data-state="normal" id="findings-12" data-timing="20s" data-menu-title="Findings - Operations" -->
## Findings - Operations

* Which EC profile is assigned to a pool?

```bash
$> ceph osd pool ls detail | grep cephfs_data
pool 6 'cephfs_data' erasure size 6 min_size 3 crush_rule 1 \
    		     object_hash rjenkins pg_num 2048 \
		     pgp_num 2048 last_change 8199 lfor 0/4568 \
		     flags hashpspool,ec_overwrites,nodeep-scrub \
		     stripe_width 8192 application cephfs
```

```bash
$> ceph osd dump | grep cephfs_data
pool 6 'cephfs_data' erasure size 6 min_size 3 crush_rule 1 \
    		     object_hash rjenkins pg_num 2048 \
		     pgp_num 2048 last_change 8199 lfor 0/4568 \
		     flags hashpspool,ec_overwrites,nodeep-scrub \
		     stripe_width 8192 application cephfs
```


<!-- .slide: data-state="normal" id="findings-13" data-timing="20s" data-menu-title="Findings - Operations" -->
## Findings - Operations

* Which EC profile is assigned to a pool?

```bash
$> ceph osd pool ls detail -f json-pretty
[
    {
        "pool_name": "cephfs_data",
[...]
        "erasure_code_profile": "ec-fast-k2m4",
[...]
```
```bash
$> ceph osd erasure-code-profile get ec-fast-k2m4 

crush-device-class=ssd
crush-failure-domain=host
crush-root=default
jerasure-per-chunk-alignment=false
k=2
m=4
plugin=jerasure
technique=reed_sol_van
w=8
```


<!-- .slide: data-state="normal" id="findings-14" data-timing="20s" data-menu-title="Conclusion" -->
## Learnings

### The technical part works in general! <!-- .element: class="fragment" data-fragment-index="0" -->
### Performance issues will be indentified and fixed! <!-- .element: class="fragment" data-fragment-index="1" -->

### Project success depends on 3 components <!-- .element: class="fragment" data-fragment-index="2" -->
* People <!-- .element: class="fragment" data-fragment-index="3" -->
* People <!-- .element: class="fragment" data-fragment-index="4" -->
* People <!-- .element: class="fragment" data-fragment-index="5" -->

### You need motivated Dev/Ops! <!-- .element: class="fragment" data-fragment-index="6" -->

