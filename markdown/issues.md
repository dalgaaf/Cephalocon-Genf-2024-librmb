<!-- .slide: data-state="section-break" id="section-break-7.1" data-timing="10s" -->
# Findings and Learnings


<!-- .slide: data-state="normal" id="findings-0" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Performance
<canvas data-chart="line">
<!--
{
 "data" : {
     "labels": ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10"],
     "datasets": [
         {
             "label": "librmb@CephFS",
             "borderColor":"rgba(227, 26, 28, 0.6)",
             "fill": "false",
             "data": [84, 141, 209, 290, 425, 1191, 2765, 7670, 12804, 21062]
         },
         {
             "label": "librmb@LocalFS",
             "borderColor":"rgba(51, 160, 44, 0.6)",
             "fill": "false",
             "data": [22, 21, 18, 29, 15, 19, 18, 27, 17, 21]
         },
         {
             "label": "NFS",
             "borderColor":"rgba(166, 206, 227, 0.6)",
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
		"labelString": "ms/cmd avg"
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
- Rados performance is okay
- CephFS is the issue


<!-- .slide: data-state="normal" id="findings-1" data-timing="20s" data-menu-title="Findings - Performance - mds" -->
## Findings - Performance
<canvas data-chart="bar">
<!--
{
 "data" : {
     "labels": ["req_create_latency", "req_getfilelock_latency", "req_link_latency", "req_lookup_latency", "req_mkdir_latency"],
     "datasets": [
         {
             "borderColor":"rgba(227, 26, 28, 0.6)",
	     "backgroundColor": "rgba(227, 26, 28, 0.2)",
             "label": "EC+Replication",
             "data": [361, 195, 269, 341, 1085]
         },
         {
             "borderColor":"rgba(51, 160, 44, 0.6)",
             "backgroundColor":"rgba(51, 160, 44, 0.2)",
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


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Performance

### CephFS performance

* imaptest performance way too slow with EC
  * metadata/indexes on local HDD significant faster

* find with 3m files need:
  * 16 sec
  * second run not faster

* no significant load on OSDs/MDS or the cluster
  * no error messages


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Performance

### CephFS Troubleshooting
* no obvious issues in network
* multi-active MDS
  * no relevant improvement
* too low number of PGs by default
  * increased, no improvement
* MDS Cache sizes increased
  * no significant improvement!
* cephfs_metadata pool set from 6x to 3x
  * no significant improvement!

Note: 
- next steps: move to replication


<!-- .slide: data-state="normal" id="findings-2" data-timing="20s" data-menu-title="Findings - Performance" -->
## Findings - Operations

* encryption didn't work out-of-the-box with SES5.5
  * not all OSDs detected on boot due to timeout
  * fixed for SES6, workaround till then

* wipefs didn't fully work
  * reboot required

Note: 
* wipefs: assume labels still somewhere in cache, may needed udev handling?


<!-- .slide: data-state="normal" id="findings-10" data-timing="20s" data-menu-title="Conclusion" -->
## Learnings

### The technical part works and will be fixed!

### Project success depends on 3 components:
* People
* People
* People

### You need motivated developers, operators and backing from management!
