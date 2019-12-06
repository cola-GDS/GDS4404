cola Report for Consensus Partitioning
==================

**Date**: 2019-12-03 16:43:51 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 31632    50
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:hclust](#SD-hclust)     |          5| 1.000|           0.954|       0.978|** |2,3,4      |
|[SD:kmeans](#SD-kmeans)     |          2| 1.000|           1.000|       1.000|** |           |
|[SD:pam](#SD-pam)           |          6| 1.000|           0.970|       0.987|** |2,3,4,5    |
|[CV:kmeans](#CV-kmeans)     |          2| 1.000|           1.000|       1.000|** |           |
|[CV:pam](#CV-pam)           |          6| 1.000|           0.966|       0.981|** |2,3,4,5    |
|[MAD:kmeans](#MAD-kmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[MAD:pam](#MAD-pam)         |          6| 1.000|           0.979|       0.991|** |2,3,4,5    |
|[ATC:kmeans](#ATC-kmeans)   |          2| 1.000|           1.000|       1.000|** |           |
|[ATC:skmeans](#ATC-skmeans) |          5| 1.000|           0.990|       0.993|** |2,3,4      |
|[ATC:mclust](#ATC-mclust)   |          5| 1.000|           0.995|       0.995|** |2,3,4      |
|[ATC:NMF](#ATC-NMF)         |          2| 1.000|           1.000|       1.000|** |           |
|[MAD:mclust](#MAD-mclust)   |          6| 0.993|           0.984|       0.989|** |2,3,4,5    |
|[SD:mclust](#SD-mclust)     |          6| 0.980|           0.927|       0.969|** |2,3,4,5    |
|[SD:skmeans](#SD-skmeans)   |          5| 0.976|           0.954|       0.970|** |2,3,4      |
|[MAD:skmeans](#MAD-skmeans) |          5| 0.972|           0.948|       0.964|** |2,3,4      |
|[ATC:pam](#ATC-pam)         |          6| 0.969|           0.937|       0.968|** |2,3,4,5    |
|[MAD:hclust](#MAD-hclust)   |          5| 0.961|           0.914|       0.950|** |2,3,4      |
|[MAD:NMF](#MAD-NMF)         |          5| 0.960|           0.944|       0.952|** |2,3,4      |
|[SD:NMF](#SD-NMF)           |          5| 0.951|           0.930|       0.944|** |2,3,4      |
|[CV:NMF](#CV-NMF)           |          5| 0.939|           0.915|       0.934|*  |2,3,4      |
|[ATC:hclust](#ATC-hclust)   |          6| 0.918|           0.833|       0.914|*  |2          |
|[CV:skmeans](#CV-skmeans)   |          6| 0.914|           0.812|       0.869|*  |2,3,4,5    |
|[CV:mclust](#CV-mclust)     |          5| 0.907|           0.762|       0.901|*  |2,3,4      |
|[CV:hclust](#CV-hclust)     |          5| 0.904|           0.782|       0.857|*  |2,3,4      |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased Rand Jaccard
#&gt; SD:NMF      2     1               1           1          0.471 0.53    0.53
#&gt; CV:NMF      2     1               1           1          0.471 0.53    0.53
#&gt; MAD:NMF     2     1               1           1          0.471 0.53    0.53
#&gt; ATC:NMF     2     1               1           1          0.471 0.53    0.53
#&gt; SD:skmeans  2     1               1           1          0.471 0.53    0.53
#&gt; CV:skmeans  2     1               1           1          0.471 0.53    0.53
#&gt; MAD:skmeans 2     1               1           1          0.471 0.53    0.53
#&gt; ATC:skmeans 2     1               1           1          0.471 0.53    0.53
#&gt; SD:mclust   2     1               1           1          0.471 0.53    0.53
#&gt; CV:mclust   2     1               1           1          0.471 0.53    0.53
#&gt; MAD:mclust  2     1               1           1          0.471 0.53    0.53
#&gt; ATC:mclust  2     1               1           1          0.471 0.53    0.53
#&gt; SD:kmeans   2     1               1           1          0.471 0.53    0.53
#&gt; CV:kmeans   2     1               1           1          0.471 0.53    0.53
#&gt; MAD:kmeans  2     1               1           1          0.471 0.53    0.53
#&gt; ATC:kmeans  2     1               1           1          0.471 0.53    0.53
#&gt; SD:pam      2     1               1           1          0.471 0.53    0.53
#&gt; CV:pam      2     1               1           1          0.471 0.53    0.53
#&gt; MAD:pam     2     1               1           1          0.471 0.53    0.53
#&gt; ATC:pam     2     1               1           1          0.471 0.53    0.53
#&gt; SD:hclust   2     1               1           1          0.471 0.53    0.53
#&gt; CV:hclust   2     1               1           1          0.471 0.53    0.53
#&gt; MAD:hclust  2     1               1           1          0.471 0.53    0.53
#&gt; ATC:hclust  2     1               1           1          0.471 0.53    0.53
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.927           0.922       0.966          0.434 0.792   0.607
#&gt; CV:NMF      3 0.949           0.933       0.969          0.436 0.792   0.607
#&gt; MAD:NMF     3 0.966           0.972       0.974          0.415 0.794   0.612
#&gt; ATC:NMF     3 0.809           0.964       0.927          0.321 0.804   0.630
#&gt; SD:skmeans  3 0.950           0.905       0.965          0.435 0.794   0.612
#&gt; CV:skmeans  3 1.000           0.966       0.984          0.440 0.792   0.607
#&gt; MAD:skmeans 3 1.000           0.989       0.995          0.438 0.794   0.612
#&gt; ATC:skmeans 3 1.000           0.984       0.994          0.395 0.811   0.644
#&gt; SD:mclust   3 1.000           1.000       1.000          0.416 0.804   0.630
#&gt; CV:mclust   3 1.000           1.000       1.000          0.416 0.804   0.630
#&gt; MAD:mclust  3 1.000           1.000       1.000          0.416 0.804   0.630
#&gt; ATC:mclust  3 1.000           1.000       1.000          0.416 0.804   0.630
#&gt; SD:kmeans   3 0.704           0.906       0.878          0.344 0.794   0.612
#&gt; CV:kmeans   3 0.707           0.921       0.860          0.338 0.794   0.612
#&gt; MAD:kmeans  3 0.699           0.874       0.856          0.338 0.794   0.612
#&gt; ATC:kmeans  3 0.775           0.952       0.907          0.303 0.811   0.644
#&gt; SD:pam      3 0.956           0.969       0.985          0.426 0.798   0.619
#&gt; CV:pam      3 0.974           0.953       0.976          0.438 0.792   0.607
#&gt; MAD:pam     3 0.952           0.959       0.981          0.436 0.794   0.612
#&gt; ATC:pam     3 0.974           0.913       0.968          0.409 0.811   0.644
#&gt; SD:hclust   3 1.000           0.958       0.982          0.432 0.794   0.612
#&gt; CV:hclust   3 0.971           0.944       0.969          0.435 0.794   0.612
#&gt; MAD:hclust  3 1.000           0.962       0.981          0.431 0.794   0.612
#&gt; ATC:hclust  3 0.846           0.908       0.950          0.378 0.804   0.630
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.996           0.966       0.979         0.0990 0.909   0.729
#&gt; CV:NMF      4 0.985           0.921       0.966         0.0935 0.922   0.762
#&gt; MAD:NMF     4 1.000           0.975       0.981         0.1144 0.912   0.736
#&gt; ATC:NMF     4 0.869           0.883       0.867         0.1320 0.919   0.758
#&gt; SD:skmeans  4 1.000           0.985       0.993         0.1030 0.912   0.736
#&gt; CV:skmeans  4 0.958           0.894       0.949         0.0974 0.899   0.700
#&gt; MAD:skmeans 4 1.000           0.975       0.991         0.1020 0.912   0.736
#&gt; ATC:skmeans 4 1.000           0.890       0.956         0.1358 0.882   0.670
#&gt; SD:mclust   4 0.971           0.919       0.967         0.1192 0.919   0.758
#&gt; CV:mclust   4 0.966           0.920       0.966         0.1189 0.918   0.756
#&gt; MAD:mclust  4 0.959           0.920       0.966         0.1176 0.922   0.765
#&gt; ATC:mclust  4 1.000           1.000       1.000         0.1174 0.922   0.765
#&gt; SD:kmeans   4 0.794           0.794       0.824         0.1277 0.939   0.814
#&gt; CV:kmeans   4 0.619           0.866       0.836         0.1225 0.928   0.783
#&gt; MAD:kmeans  4 0.635           0.881       0.831         0.1311 0.912   0.736
#&gt; ATC:kmeans  4 0.645           0.532       0.805         0.1402 0.984   0.952
#&gt; SD:pam      4 1.000           0.987       0.994         0.1135 0.912   0.737
#&gt; CV:pam      4 0.975           0.939       0.972         0.1002 0.899   0.700
#&gt; MAD:pam     4 1.000           0.991       0.995         0.1049 0.900   0.704
#&gt; ATC:pam     4 1.000           0.986       0.994         0.1247 0.900   0.714
#&gt; SD:hclust   4 1.000           0.957       0.983         0.0933 0.941   0.819
#&gt; CV:hclust   4 0.982           0.944       0.969         0.0511 0.980   0.940
#&gt; MAD:hclust  4 1.000           0.956       0.976         0.0916 0.941   0.819
#&gt; ATC:hclust  4 0.815           0.874       0.929         0.0799 0.974   0.922
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.951           0.930       0.944         0.0348 0.958   0.838
#&gt; CV:NMF      5 0.939           0.915       0.934         0.0389 0.958   0.842
#&gt; MAD:NMF     5 0.960           0.944       0.952         0.0360 0.958   0.838
#&gt; ATC:NMF     5 0.846           0.904       0.897         0.0439 0.984   0.941
#&gt; SD:skmeans  5 0.976           0.954       0.970         0.0372 0.958   0.838
#&gt; CV:skmeans  5 0.971           0.870       0.924         0.0341 0.985   0.942
#&gt; MAD:skmeans 5 0.972           0.948       0.964         0.0361 0.958   0.838
#&gt; ATC:skmeans 5 1.000           0.990       0.993         0.0341 0.971   0.888
#&gt; SD:mclust   5 1.000           0.992       0.996         0.0377 0.958   0.840
#&gt; CV:mclust   5 0.907           0.762       0.901         0.0344 0.987   0.948
#&gt; MAD:mclust  5 1.000           0.944       0.980         0.0395 0.965   0.864
#&gt; ATC:mclust  5 1.000           0.995       0.995         0.0381 0.971   0.888
#&gt; SD:kmeans   5 0.693           0.697       0.811         0.0628 0.958   0.850
#&gt; CV:kmeans   5 0.736           0.691       0.822         0.0795 0.974   0.902
#&gt; MAD:kmeans  5 0.693           0.756       0.820         0.0692 0.974   0.898
#&gt; ATC:kmeans  5 0.764           0.840       0.823         0.0741 0.878   0.633
#&gt; SD:pam      5 0.968           0.960       0.971         0.0348 0.958   0.840
#&gt; CV:pam      5 0.999           0.966       0.984         0.0286 0.977   0.910
#&gt; MAD:pam     5 1.000           0.966       0.980         0.0372 0.958   0.840
#&gt; ATC:pam     5 0.926           0.880       0.909         0.0446 0.945   0.792
#&gt; SD:hclust   5 1.000           0.954       0.978         0.0483 0.961   0.852
#&gt; CV:hclust   5 0.904           0.782       0.857         0.0615 0.948   0.832
#&gt; MAD:hclust  5 0.961           0.914       0.950         0.0457 0.963   0.862
#&gt; ATC:hclust  5 0.863           0.796       0.866         0.0717 0.922   0.745
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.924           0.926       0.944         0.0139 1.000   1.000
#&gt; CV:NMF      6 0.902           0.914       0.934         0.0198 1.000   1.000
#&gt; MAD:NMF     6 0.973           0.934       0.951         0.0107 1.000   1.000
#&gt; ATC:NMF     6 0.807           0.860       0.887         0.0263 1.000   1.000
#&gt; SD:skmeans  6 0.962           0.901       0.926         0.0156 1.000   1.000
#&gt; CV:skmeans  6 0.914           0.812       0.869         0.0239 0.980   0.918
#&gt; MAD:skmeans 6 0.972           0.911       0.917         0.0164 1.000   1.000
#&gt; ATC:skmeans 6 0.982           0.930       0.946         0.0143 0.990   0.957
#&gt; SD:mclust   6 0.980           0.927       0.969         0.0279 0.978   0.903
#&gt; CV:mclust   6 0.855           0.679       0.851         0.0342 0.984   0.932
#&gt; MAD:mclust  6 0.993           0.984       0.989         0.0272 0.969   0.866
#&gt; ATC:mclust  6 0.989           0.950       0.956         0.0268 0.990   0.957
#&gt; SD:kmeans   6 0.762           0.693       0.803         0.0584 0.971   0.882
#&gt; CV:kmeans   6 0.715           0.703       0.785         0.0485 0.974   0.892
#&gt; MAD:kmeans  6 0.751           0.699       0.804         0.0509 1.000   1.000
#&gt; ATC:kmeans  6 0.747           0.771       0.777         0.0570 0.962   0.832
#&gt; SD:pam      6 1.000           0.970       0.987         0.0173 0.988   0.946
#&gt; CV:pam      6 1.000           0.966       0.981         0.0297 0.953   0.810
#&gt; MAD:pam     6 1.000           0.979       0.991         0.0162 0.988   0.946
#&gt; ATC:pam     6 0.969           0.937       0.968         0.0397 0.971   0.869
#&gt; SD:hclust   6 0.924           0.893       0.926         0.0239 1.000   1.000
#&gt; CV:hclust   6 0.890           0.873       0.871         0.0398 0.947   0.802
#&gt; MAD:hclust  6 0.936           0.787       0.871         0.0227 0.993   0.971
#&gt; ATC:hclust  6 0.918           0.833       0.914         0.0588 0.949   0.780
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<pre><code>#&gt; Error in valid.viewport(x, y, width, height, just, gp, clip, xscale, yscale, : invalid &#39;xscale&#39; in viewport
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n tissue(p) disease.state(p) other(p) individual(p) k
#&gt; SD:NMF      50         1            0.934 2.67e-07       0.00142 2
#&gt; CV:NMF      50         1            0.934 2.67e-07       0.00142 2
#&gt; MAD:NMF     50         1            0.934 2.67e-07       0.00142 2
#&gt; ATC:NMF     50         1            0.934 2.67e-07       0.00142 2
#&gt; SD:skmeans  50         1            0.934 2.67e-07       0.00142 2
#&gt; CV:skmeans  50         1            0.934 2.67e-07       0.00142 2
#&gt; MAD:skmeans 50         1            0.934 2.67e-07       0.00142 2
#&gt; ATC:skmeans 50         1            0.934 2.67e-07       0.00142 2
#&gt; SD:mclust   50         1            0.934 2.67e-07       0.00142 2
#&gt; CV:mclust   50         1            0.934 2.67e-07       0.00142 2
#&gt; MAD:mclust  50         1            0.934 2.67e-07       0.00142 2
#&gt; ATC:mclust  50         1            0.934 2.67e-07       0.00142 2
#&gt; SD:kmeans   50         1            0.934 2.67e-07       0.00142 2
#&gt; CV:kmeans   50         1            0.934 2.67e-07       0.00142 2
#&gt; MAD:kmeans  50         1            0.934 2.67e-07       0.00142 2
#&gt; ATC:kmeans  50         1            0.934 2.67e-07       0.00142 2
#&gt; SD:pam      50         1            0.934 2.67e-07       0.00142 2
#&gt; CV:pam      50         1            0.934 2.67e-07       0.00142 2
#&gt; MAD:pam     50         1            0.934 2.67e-07       0.00142 2
#&gt; ATC:pam     50         1            0.934 2.67e-07       0.00142 2
#&gt; SD:hclust   50         1            0.934 2.67e-07       0.00142 2
#&gt; CV:hclust   50         1            0.934 2.67e-07       0.00142 2
#&gt; MAD:hclust  50         1            0.934 2.67e-07       0.00142 2
#&gt; ATC:hclust  50         1            0.934 2.67e-07       0.00142 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n tissue(p) disease.state(p) other(p) individual(p) k
#&gt; SD:NMF      49     0.981            0.952 1.63e-11      6.57e-05 3
#&gt; CV:NMF      50     0.939            0.875 8.67e-12      3.79e-05 3
#&gt; MAD:NMF     50     1.000            0.931 1.69e-11      8.93e-05 3
#&gt; ATC:NMF     50     1.000            0.931 1.26e-12      1.60e-05 3
#&gt; SD:skmeans  47     0.847            0.875 1.47e-11      8.15e-05 3
#&gt; CV:skmeans  50     0.939            0.875 8.67e-12      3.79e-05 3
#&gt; MAD:skmeans 50     1.000            0.931 1.69e-11      8.93e-05 3
#&gt; ATC:skmeans 50     0.933            0.869 1.06e-11      4.14e-05 3
#&gt; SD:mclust   50     1.000            0.931 1.26e-12      1.60e-05 3
#&gt; CV:mclust   50     1.000            0.931 1.26e-12      1.60e-05 3
#&gt; MAD:mclust  50     1.000            0.931 1.26e-12      1.60e-05 3
#&gt; ATC:mclust  50     1.000            0.931 1.26e-12      1.60e-05 3
#&gt; SD:kmeans   49     0.870            0.944 1.68e-11      6.67e-05 3
#&gt; CV:kmeans   50     0.776            0.931 1.69e-11      8.93e-05 3
#&gt; MAD:kmeans  46     0.905            0.934 3.31e-11      1.38e-04 3
#&gt; ATC:kmeans  50     0.933            0.869 1.06e-11      4.14e-05 3
#&gt; SD:pam      50     0.937            0.873 9.23e-12      3.89e-05 3
#&gt; CV:pam      50     0.939            0.875 8.67e-12      3.79e-05 3
#&gt; MAD:pam     49     0.981            0.952 1.63e-11      6.57e-05 3
#&gt; ATC:pam     46     1.000            0.871 3.31e-11      6.62e-05 3
#&gt; SD:hclust   49     0.972            0.870 1.68e-11      6.67e-05 3
#&gt; CV:hclust   50     0.776            0.931 1.69e-11      8.93e-05 3
#&gt; MAD:hclust  50     1.000            0.931 1.69e-11      8.93e-05 3
#&gt; ATC:hclust  50     1.000            0.931 1.26e-12      1.60e-05 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n tissue(p) disease.state(p) other(p) individual(p) k
#&gt; SD:NMF      49     0.995            0.988 2.28e-17      4.54e-07 4
#&gt; CV:NMF      48     1.000            0.987 7.68e-17      1.01e-06 4
#&gt; MAD:NMF     50     1.000            0.986 6.71e-18      2.02e-07 4
#&gt; ATC:NMF     49     0.995            0.961 2.28e-17      4.54e-07 4
#&gt; SD:skmeans  50     1.000            0.986 6.71e-18      2.02e-07 4
#&gt; CV:skmeans  46     0.957            0.891 2.21e-14      7.92e-06 4
#&gt; MAD:skmeans 49     0.995            0.988 2.28e-17      4.54e-07 4
#&gt; ATC:skmeans 47     0.993            0.986 2.58e-16      7.31e-07 4
#&gt; SD:mclust   48     0.987            0.948 6.57e-16      1.01e-06 4
#&gt; CV:mclust   48     0.974            0.948 1.91e-15      3.99e-06 4
#&gt; MAD:mclust  48     0.985            0.946 7.68e-17      1.01e-06 4
#&gt; ATC:mclust  50     1.000            0.986 6.71e-18      2.02e-07 4
#&gt; SD:kmeans   46     1.000            0.987 8.58e-16      1.62e-06 4
#&gt; CV:kmeans   48     1.000            0.987 4.74e-15      7.28e-06 4
#&gt; MAD:kmeans  47     0.981            0.940 2.58e-16      2.20e-06 4
#&gt; ATC:kmeans  41     0.967            0.954 7.24e-11      1.02e-04 4
#&gt; SD:pam      50     0.977            0.951 1.45e-16      7.83e-07 4
#&gt; CV:pam      49     0.995            0.988 1.32e-14      7.04e-06 4
#&gt; MAD:pam     50     0.977            0.951 1.45e-16      7.83e-07 4
#&gt; ATC:pam     50     0.979            0.866 8.76e-16      7.14e-07 4
#&gt; SD:hclust   49     0.996            0.964 5.65e-16      2.18e-06 4
#&gt; CV:hclust   50     0.528            0.986 4.32e-13      2.04e-04 4
#&gt; MAD:hclust  50     1.000            0.986 8.26e-16      4.62e-06 4
#&gt; ATC:hclust  49     0.998            0.993 2.28e-17      4.54e-07 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n tissue(p) disease.state(p) other(p) individual(p) k
#&gt; SD:NMF      49     0.996            0.982 2.33e-20      7.98e-08 5
#&gt; CV:NMF      50     1.000            0.998 3.06e-20      2.01e-07 5
#&gt; MAD:NMF     50     1.000            0.998 3.06e-20      2.01e-07 5
#&gt; ATC:NMF     50     1.000            0.986 6.71e-18      2.02e-07 5
#&gt; SD:skmeans  50     1.000            0.998 3.06e-20      2.01e-07 5
#&gt; CV:skmeans  44     0.970            0.955 1.80e-18      6.18e-07 5
#&gt; MAD:skmeans 49     0.996            0.982 2.33e-20      7.98e-08 5
#&gt; ATC:skmeans 50     1.000            0.998 3.76e-23      2.67e-09 5
#&gt; SD:mclust   50     1.000            0.998 1.59e-16      6.55e-06 5
#&gt; CV:mclust   40     0.592            0.989 1.08e-13      2.01e-05 5
#&gt; MAD:mclust  48     0.992            0.975 8.94e-16      1.54e-06 5
#&gt; ATC:mclust  50     0.987            0.975 6.70e-21      2.57e-08 5
#&gt; SD:kmeans   45     1.000            0.614 2.70e-15      1.17e-07 5
#&gt; CV:kmeans   43     0.999            0.719 1.42e-16      7.38e-07 5
#&gt; MAD:kmeans  45     0.999            0.723 1.15e-19      2.75e-08 5
#&gt; ATC:kmeans  48     0.982            0.532 5.15e-16      6.33e-08 5
#&gt; SD:pam      50     1.000            0.998 3.06e-20      2.01e-07 5
#&gt; CV:pam      50     0.979            0.964 3.20e-17      9.29e-07 5
#&gt; MAD:pam     50     1.000            0.998 3.06e-20      2.01e-07 5
#&gt; ATC:pam     46     0.982            0.560 3.62e-16      1.08e-07 5
#&gt; SD:hclust   49     0.996            0.982 2.33e-20      7.98e-08 5
#&gt; CV:hclust   41     0.554            0.958 3.28e-14      9.32e-06 5
#&gt; MAD:hclust  49     0.996            0.993 2.50e-19      7.39e-07 5
#&gt; ATC:hclust  46     0.922            0.927 2.35e-20      4.18e-08 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n tissue(p) disease.state(p) other(p) individual(p) k
#&gt; SD:NMF      50     1.000            0.998 3.06e-20      2.01e-07 6
#&gt; CV:NMF      50     1.000            0.998 3.06e-20      2.01e-07 6
#&gt; MAD:NMF     50     1.000            0.998 3.06e-20      2.01e-07 6
#&gt; ATC:NMF     48     1.000            0.962 7.68e-17      3.25e-07 6
#&gt; SD:skmeans  49     0.996            0.982 2.33e-20      7.98e-08 6
#&gt; CV:skmeans  43     0.860            0.987 2.10e-16      4.86e-06 6
#&gt; MAD:skmeans 49     0.996            0.982 2.33e-20      7.98e-08 6
#&gt; ATC:skmeans 48     1.000            0.863 1.22e-26      7.95e-11 6
#&gt; SD:mclust   48     0.860            0.863 1.85e-13      3.25e-04 6
#&gt; CV:mclust   39     0.691            0.980 4.22e-10      1.80e-03 6
#&gt; MAD:mclust  50     0.994            0.989 5.92e-15      1.19e-04 6
#&gt; ATC:mclust  49     0.692            0.996 8.70e-20      2.73e-06 6
#&gt; SD:kmeans   46     0.998            0.586 7.36e-16      4.18e-08 6
#&gt; CV:kmeans   43     0.999            0.420 5.80e-14      3.43e-06 6
#&gt; MAD:kmeans  44     1.000            0.752 5.57e-19      7.88e-08 6
#&gt; ATC:kmeans  43     0.998            0.998 2.92e-15      1.90e-06 6
#&gt; SD:pam      50     0.991            0.984 4.62e-22      7.12e-08 6
#&gt; CV:pam      50     0.979            0.969 1.52e-20      2.12e-07 6
#&gt; MAD:pam     50     0.991            0.984 4.62e-22      7.12e-08 6
#&gt; ATC:pam     48     0.986            0.666 6.76e-21      5.72e-09 6
#&gt; SD:hclust   48     0.989            0.970 4.44e-20      2.22e-08 6
#&gt; CV:hclust   50     0.549            0.990 7.86e-20      2.17e-06 6
#&gt; MAD:hclust  45     0.703            0.994 1.15e-19      4.58e-07 6
#&gt; ATC:hclust  45     0.970            1.000 1.15e-19      1.17e-07 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           0.958       0.982         0.4316 0.794   0.612
#> 4 4 1.000           0.957       0.983         0.0933 0.941   0.819
#> 5 5 1.000           0.954       0.978         0.0483 0.961   0.852
#> 6 6 0.924           0.893       0.926         0.0239 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892345     1  0.0424      0.981 0.992  0 0.008
#&gt; GSM892349     3  0.6126      0.337 0.400  0 0.600
#&gt; GSM892353     1  0.0237      0.982 0.996  0 0.004
#&gt; GSM892355     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892361     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892369     3  0.0424      0.947 0.008  0 0.992
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892347     1  0.0424      0.981 0.992  0 0.008
#&gt; GSM892351     1  0.4750      0.710 0.784  0 0.216
#&gt; GSM892357     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892371     3  0.0424      0.947 0.008  0 0.992
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892346     1  0.0424      0.981 0.992  0 0.008
#&gt; GSM892350     1  0.0424      0.981 0.992  0 0.008
#&gt; GSM892354     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892370     3  0.0424      0.947 0.008  0 0.992
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.4504      0.750 0.196  0 0.804
#&gt; GSM892348     1  0.0424      0.981 0.992  0 0.008
#&gt; GSM892352     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892358     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892364     1  0.0000      0.984 1.000  0 0.000
#&gt; GSM892368     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.950 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.955 0.000  0 0.000 1.000
#&gt; GSM892349     3  0.4877      0.287 0.000  0 0.592 0.408
#&gt; GSM892353     1  0.0188      0.993 0.996  0 0.004 0.000
#&gt; GSM892355     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.0336      0.945 0.000  0 0.992 0.008
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892347     4  0.0000      0.955 0.000  0 0.000 1.000
#&gt; GSM892351     4  0.3688      0.715 0.000  0 0.208 0.792
#&gt; GSM892357     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892371     3  0.0336      0.945 0.000  0 0.992 0.008
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892346     4  0.0000      0.955 0.000  0 0.000 1.000
#&gt; GSM892350     4  0.0000      0.955 0.000  0 0.000 1.000
#&gt; GSM892354     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.0336      0.945 0.000  0 0.992 0.008
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.3895      0.747 0.012  0 0.804 0.184
#&gt; GSM892348     4  0.0000      0.955 0.000  0 0.000 1.000
#&gt; GSM892352     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892358     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      0.997 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0817      0.975 0.976  0 0.000 0.024
#&gt; GSM892368     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.949 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.1197      0.852 0.000  0 0.048 0.000 0.952
#&gt; GSM892345     4  0.0000      0.951 0.000  0 0.000 1.000 0.000
#&gt; GSM892349     5  0.4192      0.279 0.000  0 0.000 0.404 0.596
#&gt; GSM892353     1  0.0290      0.992 0.992  0 0.000 0.000 0.008
#&gt; GSM892355     1  0.0162      0.995 0.996  0 0.000 0.000 0.004
#&gt; GSM892361     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.0290      0.996 0.000  0 0.992 0.000 0.008
#&gt; GSM892369     3  0.0000      0.993 0.000  0 1.000 0.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.1197      0.852 0.000  0 0.048 0.000 0.952
#&gt; GSM892347     4  0.0000      0.951 0.000  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.3210      0.700 0.000  0 0.000 0.788 0.212
#&gt; GSM892357     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.0290      0.996 0.000  0 0.992 0.000 0.008
#&gt; GSM892371     3  0.0000      0.993 0.000  0 1.000 0.000 0.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.1197      0.852 0.000  0 0.048 0.000 0.952
#&gt; GSM892346     4  0.0000      0.951 0.000  0 0.000 1.000 0.000
#&gt; GSM892350     4  0.0162      0.949 0.000  0 0.000 0.996 0.004
#&gt; GSM892354     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0162      0.995 0.996  0 0.000 0.000 0.004
#&gt; GSM892362     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.0290      0.996 0.000  0 0.992 0.000 0.008
#&gt; GSM892370     3  0.0000      0.993 0.000  0 1.000 0.000 0.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.4851      0.710 0.008  0 0.084 0.176 0.732
#&gt; GSM892348     4  0.0000      0.951 0.000  0 0.000 1.000 0.000
#&gt; GSM892352     5  0.0290      0.833 0.000  0 0.008 0.000 0.992
#&gt; GSM892358     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.996 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0865      0.972 0.972  0 0.000 0.024 0.004
#&gt; GSM892368     3  0.0290      0.996 0.000  0 0.992 0.000 0.008
#&gt; GSM892372     3  0.0290      0.996 0.000  0 0.992 0.000 0.008
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.1007      0.808 0.000 0.000 0.044 0.000 0.956 0.000
#&gt; GSM892345     4  0.3390      0.807 0.000 0.000 0.000 0.704 0.000 0.296
#&gt; GSM892349     5  0.5743      0.339 0.000 0.000 0.000 0.404 0.428 0.168
#&gt; GSM892353     1  0.2482      0.895 0.848 0.000 0.000 0.000 0.004 0.148
#&gt; GSM892355     1  0.2300      0.900 0.856 0.000 0.000 0.000 0.000 0.144
#&gt; GSM892361     1  0.0260      0.956 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM892365     3  0.0146      0.972 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM892369     3  0.1267      0.955 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM892373     2  0.2048      0.911 0.000 0.880 0.000 0.000 0.000 0.120
#&gt; GSM892377     2  0.1387      0.942 0.000 0.932 0.000 0.000 0.000 0.068
#&gt; GSM892381     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0458      0.967 0.000 0.984 0.000 0.000 0.000 0.016
#&gt; GSM892344     5  0.1007      0.808 0.000 0.000 0.044 0.000 0.956 0.000
#&gt; GSM892347     4  0.3390      0.807 0.000 0.000 0.000 0.704 0.000 0.296
#&gt; GSM892351     4  0.3487      0.452 0.000 0.000 0.000 0.788 0.044 0.168
#&gt; GSM892357     1  0.0260      0.956 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM892359     1  0.0363      0.954 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM892363     1  0.0790      0.953 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; GSM892367     3  0.0146      0.972 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM892371     3  0.1267      0.955 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM892375     2  0.0458      0.966 0.000 0.984 0.000 0.000 0.000 0.016
#&gt; GSM892379     2  0.0146      0.968 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892385     2  0.1204      0.941 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; GSM892389     2  0.0260      0.967 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892341     5  0.1007      0.808 0.000 0.000 0.044 0.000 0.956 0.000
#&gt; GSM892346     4  0.3765      0.787 0.000 0.000 0.000 0.596 0.000 0.404
#&gt; GSM892350     4  0.0000      0.666 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892354     1  0.0260      0.956 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM892356     1  0.2260      0.901 0.860 0.000 0.000 0.000 0.000 0.140
#&gt; GSM892362     1  0.0000      0.956 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0146      0.972 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM892370     3  0.1267      0.955 0.000 0.000 0.940 0.000 0.000 0.060
#&gt; GSM892374     2  0.2048      0.911 0.000 0.880 0.000 0.000 0.000 0.120
#&gt; GSM892378     2  0.1387      0.942 0.000 0.932 0.000 0.000 0.000 0.068
#&gt; GSM892382     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.968 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.2135      0.894 0.000 0.872 0.000 0.000 0.000 0.128
#&gt; GSM892343     5  0.4634      0.693 0.000 0.000 0.020 0.160 0.724 0.096
#&gt; GSM892348     4  0.3765      0.787 0.000 0.000 0.000 0.596 0.000 0.404
#&gt; GSM892352     5  0.2454      0.742 0.000 0.000 0.000 0.000 0.840 0.160
#&gt; GSM892358     1  0.0458      0.955 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM892360     1  0.0363      0.954 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM892364     1  0.1686      0.937 0.924 0.000 0.000 0.012 0.000 0.064
#&gt; GSM892368     3  0.0146      0.972 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM892372     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892376     2  0.0458      0.966 0.000 0.984 0.000 0.000 0.000 0.016
#&gt; GSM892380     2  0.0146      0.968 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892386     2  0.0146      0.968 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892390     2  0.0260      0.967 0.000 0.992 0.000 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) disease.state(p) other(p) individual(p) k
#> SD:hclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> SD:hclust 49     0.972            0.870 1.68e-11      6.67e-05 3
#> SD:hclust 49     0.996            0.964 5.65e-16      2.18e-06 4
#> SD:hclust 49     0.996            0.982 2.33e-20      7.98e-08 5
#> SD:hclust 48     0.989            0.970 4.44e-20      2.22e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.704           0.906       0.878         0.3438 0.794   0.612
#> 4 4 0.794           0.794       0.824         0.1277 0.939   0.814
#> 5 5 0.693           0.697       0.811         0.0628 0.958   0.850
#> 6 6 0.762           0.693       0.803         0.0584 0.971   0.882
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM892342     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892345     3  0.6126      0.760 0.400 0.000 0.600
#&gt; GSM892349     3  0.5178      0.944 0.256 0.000 0.744
#&gt; GSM892353     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892355     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892365     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892369     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892373     2  0.4796      0.867 0.000 0.780 0.220
#&gt; GSM892377     2  0.3879      0.909 0.000 0.848 0.152
#&gt; GSM892381     2  0.0424      0.958 0.000 0.992 0.008
#&gt; GSM892383     2  0.1163      0.956 0.000 0.972 0.028
#&gt; GSM892387     2  0.0424      0.958 0.000 0.992 0.008
#&gt; GSM892344     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892347     3  0.6126      0.760 0.400 0.000 0.600
#&gt; GSM892351     3  0.5178      0.944 0.256 0.000 0.744
#&gt; GSM892357     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892367     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892371     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892375     2  0.0592      0.958 0.000 0.988 0.012
#&gt; GSM892379     2  0.1411      0.957 0.000 0.964 0.036
#&gt; GSM892385     2  0.1529      0.956 0.000 0.960 0.040
#&gt; GSM892389     2  0.1031      0.958 0.000 0.976 0.024
#&gt; GSM892341     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892346     1  0.4062      0.700 0.836 0.000 0.164
#&gt; GSM892350     3  0.6126      0.760 0.400 0.000 0.600
#&gt; GSM892354     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892366     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892370     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892374     2  0.4796      0.867 0.000 0.780 0.220
#&gt; GSM892378     2  0.3879      0.909 0.000 0.848 0.152
#&gt; GSM892382     2  0.0424      0.958 0.000 0.992 0.008
#&gt; GSM892384     2  0.0424      0.958 0.000 0.992 0.008
#&gt; GSM892388     2  0.3116      0.923 0.000 0.892 0.108
#&gt; GSM892343     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892348     1  0.6111     -0.154 0.604 0.000 0.396
#&gt; GSM892352     3  0.5178      0.944 0.256 0.000 0.744
#&gt; GSM892358     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.939 1.000 0.000 0.000
#&gt; GSM892368     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892372     3  0.5327      0.956 0.272 0.000 0.728
#&gt; GSM892376     2  0.0592      0.958 0.000 0.988 0.012
#&gt; GSM892380     2  0.1163      0.958 0.000 0.972 0.028
#&gt; GSM892386     2  0.0424      0.958 0.000 0.992 0.008
#&gt; GSM892390     2  0.1289      0.958 0.000 0.968 0.032
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.0469     0.8081 0.000 0.000 0.988 0.012
#&gt; GSM892345     4  0.6915     0.7632 0.108 0.000 0.416 0.476
#&gt; GSM892349     3  0.4941    -0.2668 0.000 0.000 0.564 0.436
#&gt; GSM892353     1  0.3550     0.9621 0.860 0.000 0.096 0.044
#&gt; GSM892355     1  0.3463     0.9643 0.864 0.000 0.096 0.040
#&gt; GSM892361     1  0.2611     0.9830 0.896 0.000 0.096 0.008
#&gt; GSM892365     3  0.0000     0.8084 0.000 0.000 1.000 0.000
#&gt; GSM892369     3  0.2081     0.7725 0.000 0.000 0.916 0.084
#&gt; GSM892373     2  0.4898     0.7086 0.000 0.584 0.000 0.416
#&gt; GSM892377     2  0.5198     0.8015 0.040 0.708 0.000 0.252
#&gt; GSM892381     2  0.1284     0.9021 0.024 0.964 0.000 0.012
#&gt; GSM892383     2  0.2319     0.8957 0.036 0.924 0.000 0.040
#&gt; GSM892387     2  0.1406     0.9040 0.024 0.960 0.000 0.016
#&gt; GSM892344     3  0.0469     0.8081 0.000 0.000 0.988 0.012
#&gt; GSM892347     4  0.6915     0.7632 0.108 0.000 0.416 0.476
#&gt; GSM892351     3  0.4992    -0.4096 0.000 0.000 0.524 0.476
#&gt; GSM892357     1  0.2741     0.9829 0.892 0.000 0.096 0.012
#&gt; GSM892359     1  0.2611     0.9830 0.896 0.000 0.096 0.008
#&gt; GSM892363     1  0.2281     0.9823 0.904 0.000 0.096 0.000
#&gt; GSM892367     3  0.0000     0.8084 0.000 0.000 1.000 0.000
#&gt; GSM892371     3  0.2081     0.7725 0.000 0.000 0.916 0.084
#&gt; GSM892375     2  0.1706     0.9035 0.016 0.948 0.000 0.036
#&gt; GSM892379     2  0.2521     0.8997 0.024 0.912 0.000 0.064
#&gt; GSM892385     2  0.2919     0.8925 0.044 0.896 0.000 0.060
#&gt; GSM892389     2  0.1913     0.9034 0.020 0.940 0.000 0.040
#&gt; GSM892341     3  0.0469     0.8081 0.000 0.000 0.988 0.012
#&gt; GSM892346     4  0.7375     0.4872 0.348 0.000 0.172 0.480
#&gt; GSM892350     4  0.6915     0.7632 0.108 0.000 0.416 0.476
#&gt; GSM892354     1  0.2611     0.9830 0.896 0.000 0.096 0.008
#&gt; GSM892356     1  0.3463     0.9643 0.864 0.000 0.096 0.040
#&gt; GSM892362     1  0.2611     0.9830 0.896 0.000 0.096 0.008
#&gt; GSM892366     3  0.0000     0.8084 0.000 0.000 1.000 0.000
#&gt; GSM892370     3  0.2081     0.7725 0.000 0.000 0.916 0.084
#&gt; GSM892374     2  0.4898     0.7086 0.000 0.584 0.000 0.416
#&gt; GSM892378     2  0.5198     0.8015 0.040 0.708 0.000 0.252
#&gt; GSM892382     2  0.1284     0.9021 0.024 0.964 0.000 0.012
#&gt; GSM892384     2  0.1284     0.9021 0.024 0.964 0.000 0.012
#&gt; GSM892388     2  0.4799     0.8131 0.032 0.744 0.000 0.224
#&gt; GSM892343     3  0.2469     0.7504 0.000 0.000 0.892 0.108
#&gt; GSM892348     4  0.7438     0.7386 0.188 0.000 0.328 0.484
#&gt; GSM892352     3  0.4661     0.0688 0.000 0.000 0.652 0.348
#&gt; GSM892358     1  0.2741     0.9784 0.892 0.000 0.096 0.012
#&gt; GSM892360     1  0.2611     0.9830 0.896 0.000 0.096 0.008
#&gt; GSM892364     1  0.2466     0.9817 0.900 0.000 0.096 0.004
#&gt; GSM892368     3  0.0000     0.8084 0.000 0.000 1.000 0.000
#&gt; GSM892372     3  0.0000     0.8084 0.000 0.000 1.000 0.000
#&gt; GSM892376     2  0.1610     0.9037 0.016 0.952 0.000 0.032
#&gt; GSM892380     2  0.1584     0.9028 0.012 0.952 0.000 0.036
#&gt; GSM892386     2  0.1297     0.9029 0.020 0.964 0.000 0.016
#&gt; GSM892390     2  0.1584     0.9031 0.012 0.952 0.000 0.036
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     3  0.0404     0.7327 0.000 0.000 0.988 0.012 0.000
#&gt; GSM892345     4  0.4744     0.8927 0.056 0.000 0.252 0.692 0.000
#&gt; GSM892349     3  0.4557    -0.4544 0.000 0.000 0.516 0.476 0.008
#&gt; GSM892353     1  0.4536     0.8814 0.780 0.000 0.060 0.028 0.132
#&gt; GSM892355     1  0.4451     0.8833 0.784 0.000 0.060 0.024 0.132
#&gt; GSM892361     1  0.2193     0.9486 0.912 0.000 0.060 0.000 0.028
#&gt; GSM892365     3  0.2648     0.7804 0.000 0.000 0.848 0.000 0.152
#&gt; GSM892369     3  0.4637     0.7474 0.000 0.000 0.728 0.076 0.196
#&gt; GSM892373     5  0.4235     1.0000 0.000 0.424 0.000 0.000 0.576
#&gt; GSM892377     2  0.6103    -0.2892 0.008 0.532 0.000 0.108 0.352
#&gt; GSM892381     2  0.0992     0.7092 0.000 0.968 0.000 0.024 0.008
#&gt; GSM892383     2  0.3244     0.6662 0.008 0.860 0.000 0.084 0.048
#&gt; GSM892387     2  0.2721     0.6933 0.012 0.892 0.000 0.068 0.028
#&gt; GSM892344     3  0.0404     0.7327 0.000 0.000 0.988 0.012 0.000
#&gt; GSM892347     4  0.4744     0.8927 0.056 0.000 0.252 0.692 0.000
#&gt; GSM892351     4  0.4108     0.7749 0.000 0.000 0.308 0.684 0.008
#&gt; GSM892357     1  0.2264     0.9501 0.912 0.000 0.060 0.004 0.024
#&gt; GSM892359     1  0.1410     0.9524 0.940 0.000 0.060 0.000 0.000
#&gt; GSM892363     1  0.1410     0.9524 0.940 0.000 0.060 0.000 0.000
#&gt; GSM892367     3  0.3074     0.7717 0.000 0.000 0.804 0.000 0.196
#&gt; GSM892371     3  0.4637     0.7474 0.000 0.000 0.728 0.076 0.196
#&gt; GSM892375     2  0.3191     0.6587 0.016 0.868 0.000 0.040 0.076
#&gt; GSM892379     2  0.4002     0.6486 0.000 0.796 0.000 0.120 0.084
#&gt; GSM892385     2  0.3926     0.6363 0.008 0.808 0.000 0.132 0.052
#&gt; GSM892389     2  0.3384     0.6868 0.004 0.848 0.000 0.088 0.060
#&gt; GSM892341     3  0.0404     0.7327 0.000 0.000 0.988 0.012 0.000
#&gt; GSM892346     4  0.5535     0.7317 0.188 0.000 0.124 0.676 0.012
#&gt; GSM892350     4  0.4744     0.8927 0.056 0.000 0.252 0.692 0.000
#&gt; GSM892354     1  0.2193     0.9486 0.912 0.000 0.060 0.000 0.028
#&gt; GSM892356     1  0.4314     0.8894 0.796 0.000 0.060 0.024 0.120
#&gt; GSM892362     1  0.1809     0.9507 0.928 0.000 0.060 0.000 0.012
#&gt; GSM892366     3  0.2648     0.7804 0.000 0.000 0.848 0.000 0.152
#&gt; GSM892370     3  0.4637     0.7474 0.000 0.000 0.728 0.076 0.196
#&gt; GSM892374     5  0.4235     1.0000 0.000 0.424 0.000 0.000 0.576
#&gt; GSM892378     2  0.6103    -0.2892 0.008 0.532 0.000 0.108 0.352
#&gt; GSM892382     2  0.0992     0.7092 0.000 0.968 0.000 0.024 0.008
#&gt; GSM892384     2  0.1082     0.7072 0.000 0.964 0.000 0.028 0.008
#&gt; GSM892388     2  0.6277    -0.0681 0.028 0.604 0.000 0.128 0.240
#&gt; GSM892343     3  0.2230     0.6652 0.000 0.000 0.884 0.116 0.000
#&gt; GSM892348     4  0.5196     0.8600 0.100 0.000 0.208 0.688 0.004
#&gt; GSM892352     3  0.4392    -0.1592 0.000 0.000 0.612 0.380 0.008
#&gt; GSM892358     1  0.1571     0.9523 0.936 0.000 0.060 0.004 0.000
#&gt; GSM892360     1  0.1410     0.9524 0.940 0.000 0.060 0.000 0.000
#&gt; GSM892364     1  0.2519     0.9419 0.900 0.000 0.060 0.004 0.036
#&gt; GSM892368     3  0.2648     0.7804 0.000 0.000 0.848 0.000 0.152
#&gt; GSM892372     3  0.2891     0.7774 0.000 0.000 0.824 0.000 0.176
#&gt; GSM892376     2  0.3067     0.6654 0.016 0.876 0.000 0.040 0.068
#&gt; GSM892380     2  0.2992     0.6802 0.000 0.868 0.000 0.068 0.064
#&gt; GSM892386     2  0.1843     0.7055 0.008 0.932 0.000 0.052 0.008
#&gt; GSM892390     2  0.2843     0.6956 0.000 0.876 0.000 0.076 0.048
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     3  0.4224      0.635 0.016 0.000 0.660 0.012 0.312 0.000
#&gt; GSM892345     4  0.2164      0.830 0.032 0.000 0.068 0.900 0.000 0.000
#&gt; GSM892349     4  0.5970      0.510 0.016 0.000 0.128 0.528 0.320 0.008
#&gt; GSM892353     1  0.3725      0.768 0.676 0.000 0.000 0.008 0.000 0.316
#&gt; GSM892355     1  0.3601      0.771 0.684 0.000 0.000 0.004 0.000 0.312
#&gt; GSM892361     1  0.2201      0.888 0.904 0.000 0.000 0.004 0.036 0.056
#&gt; GSM892365     3  0.0458      0.778 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM892369     3  0.4860      0.710 0.016 0.000 0.752 0.096 0.072 0.064
#&gt; GSM892373     5  0.6125      1.000 0.000 0.312 0.000 0.000 0.348 0.340
#&gt; GSM892377     2  0.5190     -0.167 0.000 0.460 0.000 0.000 0.452 0.088
#&gt; GSM892381     2  0.1369      0.704 0.000 0.952 0.000 0.016 0.016 0.016
#&gt; GSM892383     2  0.3583      0.672 0.000 0.820 0.000 0.032 0.108 0.040
#&gt; GSM892387     2  0.3097      0.683 0.000 0.856 0.000 0.036 0.028 0.080
#&gt; GSM892344     3  0.4224      0.635 0.016 0.000 0.660 0.012 0.312 0.000
#&gt; GSM892347     4  0.2164      0.830 0.032 0.000 0.068 0.900 0.000 0.000
#&gt; GSM892351     4  0.3085      0.810 0.016 0.000 0.084 0.860 0.032 0.008
#&gt; GSM892357     1  0.2263      0.888 0.900 0.000 0.000 0.004 0.036 0.060
#&gt; GSM892359     1  0.0146      0.897 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM892363     1  0.0436      0.896 0.988 0.000 0.000 0.004 0.004 0.004
#&gt; GSM892367     3  0.2698      0.762 0.016 0.000 0.880 0.000 0.064 0.040
#&gt; GSM892371     3  0.4860      0.710 0.016 0.000 0.752 0.096 0.072 0.064
#&gt; GSM892375     2  0.2831      0.678 0.000 0.876 0.000 0.028 0.044 0.052
#&gt; GSM892379     2  0.4485      0.645 0.000 0.724 0.000 0.012 0.180 0.084
#&gt; GSM892385     2  0.4252      0.652 0.000 0.772 0.000 0.032 0.116 0.080
#&gt; GSM892389     2  0.3992      0.681 0.000 0.788 0.000 0.020 0.088 0.104
#&gt; GSM892341     3  0.4224      0.635 0.016 0.000 0.660 0.012 0.312 0.000
#&gt; GSM892346     4  0.2936      0.786 0.080 0.000 0.024 0.868 0.004 0.024
#&gt; GSM892350     4  0.2307      0.830 0.032 0.000 0.068 0.896 0.000 0.004
#&gt; GSM892354     1  0.2263      0.888 0.900 0.000 0.000 0.004 0.036 0.060
#&gt; GSM892356     1  0.3489      0.786 0.708 0.000 0.000 0.004 0.000 0.288
#&gt; GSM892362     1  0.1194      0.891 0.956 0.000 0.000 0.004 0.008 0.032
#&gt; GSM892366     3  0.0458      0.778 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM892370     3  0.4860      0.710 0.016 0.000 0.752 0.096 0.072 0.064
#&gt; GSM892374     5  0.6125      1.000 0.000 0.312 0.000 0.000 0.348 0.340
#&gt; GSM892378     2  0.5190     -0.167 0.000 0.460 0.000 0.000 0.452 0.088
#&gt; GSM892382     2  0.1369      0.704 0.000 0.952 0.000 0.016 0.016 0.016
#&gt; GSM892384     2  0.1237      0.703 0.000 0.956 0.000 0.020 0.004 0.020
#&gt; GSM892388     2  0.5723     -0.219 0.000 0.468 0.016 0.008 0.080 0.428
#&gt; GSM892343     3  0.5866      0.501 0.016 0.000 0.524 0.148 0.312 0.000
#&gt; GSM892348     4  0.2897      0.815 0.048 0.000 0.048 0.876 0.004 0.024
#&gt; GSM892352     4  0.6576      0.189 0.016 0.000 0.276 0.364 0.340 0.004
#&gt; GSM892358     1  0.0146      0.897 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM892360     1  0.0146      0.897 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM892364     1  0.2101      0.871 0.892 0.000 0.000 0.004 0.004 0.100
#&gt; GSM892368     3  0.0458      0.778 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM892372     3  0.1718      0.773 0.016 0.000 0.932 0.000 0.044 0.008
#&gt; GSM892376     2  0.2769      0.680 0.000 0.880 0.000 0.032 0.036 0.052
#&gt; GSM892380     2  0.3495      0.668 0.000 0.792 0.000 0.008 0.172 0.028
#&gt; GSM892386     2  0.2231      0.699 0.000 0.908 0.000 0.028 0.016 0.048
#&gt; GSM892390     2  0.3357      0.695 0.000 0.840 0.000 0.024 0.068 0.068
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) disease.state(p) other(p) individual(p) k
#> SD:kmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> SD:kmeans 49     0.870            0.944 1.68e-11      6.67e-05 3
#> SD:kmeans 46     1.000            0.987 8.58e-16      1.62e-06 4
#> SD:kmeans 45     1.000            0.614 2.70e-15      1.17e-07 5
#> SD:kmeans 46     0.998            0.586 7.36e-16      4.18e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.950           0.905       0.965         0.4349 0.794   0.612
#> 4 4 1.000           0.985       0.993         0.1030 0.912   0.736
#> 5 5 0.976           0.954       0.970         0.0372 0.958   0.838
#> 6 6 0.962           0.901       0.926         0.0156 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892345     3   0.628      0.186 0.460  0 0.540
#&gt; GSM892349     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892353     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892355     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892361     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892365     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892369     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892373     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892344     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892347     3   0.628      0.186 0.460  0 0.540
#&gt; GSM892351     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892357     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892359     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892363     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892367     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892371     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892341     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892346     1   0.440      0.751 0.812  0 0.188
#&gt; GSM892350     3   0.628      0.186 0.460  0 0.540
#&gt; GSM892354     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892356     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892362     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892366     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892370     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892374     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892388     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892343     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892348     1   0.460      0.726 0.796  0 0.204
#&gt; GSM892352     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892358     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892360     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892364     1   0.000      0.965 1.000  0 0.000
#&gt; GSM892368     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892372     3   0.000      0.911 0.000  0 1.000
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0336      0.973 0.000  0 0.992 0.008
#&gt; GSM892345     4  0.0000      0.987 0.000  0 0.000 1.000
#&gt; GSM892349     4  0.0000      0.987 0.000  0 0.000 1.000
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.975 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.0336      0.973 0.000  0 0.992 0.008
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0336      0.973 0.000  0 0.992 0.008
#&gt; GSM892347     4  0.0000      0.987 0.000  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.987 0.000  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.975 0.000  0 1.000 0.000
#&gt; GSM892371     3  0.0336      0.973 0.000  0 0.992 0.008
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0336      0.973 0.000  0 0.992 0.008
#&gt; GSM892346     4  0.0336      0.982 0.008  0 0.000 0.992
#&gt; GSM892350     4  0.0000      0.987 0.000  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.975 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.0336      0.973 0.000  0 0.992 0.008
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.3873      0.711 0.000  0 0.772 0.228
#&gt; GSM892348     4  0.0188      0.985 0.004  0 0.000 0.996
#&gt; GSM892352     4  0.1940      0.918 0.000  0 0.076 0.924
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.975 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.975 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.0703      0.875 0.000  0 0.024 0.000 0.976
#&gt; GSM892345     4  0.0162      0.994 0.000  0 0.004 0.996 0.000
#&gt; GSM892349     5  0.4047      0.561 0.000  0 0.004 0.320 0.676
#&gt; GSM892353     1  0.0162      0.997 0.996  0 0.000 0.000 0.004
#&gt; GSM892355     1  0.0162      0.997 0.996  0 0.000 0.000 0.004
#&gt; GSM892361     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.3395      0.835 0.000  0 0.764 0.000 0.236
#&gt; GSM892369     3  0.0000      0.870 0.000  0 1.000 0.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.0609      0.875 0.000  0 0.020 0.000 0.980
#&gt; GSM892347     4  0.0162      0.994 0.000  0 0.000 0.996 0.004
#&gt; GSM892351     4  0.0451      0.991 0.000  0 0.004 0.988 0.008
#&gt; GSM892357     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.1270      0.878 0.000  0 0.948 0.000 0.052
#&gt; GSM892371     3  0.0000      0.870 0.000  0 1.000 0.000 0.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.0703      0.875 0.000  0 0.024 0.000 0.976
#&gt; GSM892346     4  0.0162      0.993 0.000  0 0.004 0.996 0.000
#&gt; GSM892350     4  0.0162      0.994 0.000  0 0.000 0.996 0.004
#&gt; GSM892354     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0162      0.997 0.996  0 0.000 0.000 0.004
#&gt; GSM892362     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.3424      0.831 0.000  0 0.760 0.000 0.240
#&gt; GSM892370     3  0.0000      0.870 0.000  0 1.000 0.000 0.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.1774      0.873 0.000  0 0.016 0.052 0.932
#&gt; GSM892348     4  0.0162      0.993 0.000  0 0.004 0.996 0.000
#&gt; GSM892352     5  0.2338      0.839 0.000  0 0.004 0.112 0.884
#&gt; GSM892358     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892368     3  0.3274      0.845 0.000  0 0.780 0.000 0.220
#&gt; GSM892372     3  0.2605      0.872 0.000  0 0.852 0.000 0.148
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     5  0.1910      0.787 0.000 0.000 0.108 0.000 0.892 NA
#&gt; GSM892345     4  0.0820      0.949 0.000 0.000 0.000 0.972 0.016 NA
#&gt; GSM892349     5  0.5983      0.248 0.000 0.000 0.004 0.340 0.452 NA
#&gt; GSM892353     1  0.1814      0.930 0.900 0.000 0.000 0.000 0.000 NA
#&gt; GSM892355     1  0.1501      0.947 0.924 0.000 0.000 0.000 0.000 NA
#&gt; GSM892361     1  0.0260      0.979 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892365     3  0.1049      0.777 0.000 0.000 0.960 0.000 0.032 NA
#&gt; GSM892369     3  0.3828      0.696 0.000 0.000 0.560 0.000 0.000 NA
#&gt; GSM892373     2  0.0865      0.974 0.000 0.964 0.000 0.000 0.000 NA
#&gt; GSM892377     2  0.0713      0.978 0.000 0.972 0.000 0.000 0.000 NA
#&gt; GSM892381     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892383     2  0.0000      0.990 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM892387     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892344     5  0.1806      0.790 0.000 0.000 0.088 0.000 0.908 NA
#&gt; GSM892347     4  0.0363      0.949 0.000 0.000 0.000 0.988 0.000 NA
#&gt; GSM892351     4  0.2629      0.882 0.000 0.000 0.000 0.868 0.040 NA
#&gt; GSM892357     1  0.0260      0.979 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892359     1  0.0146      0.979 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892363     1  0.0260      0.980 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892367     3  0.2302      0.781 0.000 0.000 0.872 0.000 0.008 NA
#&gt; GSM892371     3  0.3810      0.701 0.000 0.000 0.572 0.000 0.000 NA
#&gt; GSM892375     2  0.0260      0.990 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892379     2  0.0363      0.988 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892385     2  0.0000      0.990 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM892389     2  0.0260      0.990 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892341     5  0.1910      0.785 0.000 0.000 0.108 0.000 0.892 NA
#&gt; GSM892346     4  0.0993      0.939 0.000 0.000 0.000 0.964 0.012 NA
#&gt; GSM892350     4  0.1265      0.940 0.000 0.000 0.000 0.948 0.008 NA
#&gt; GSM892354     1  0.0260      0.979 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892356     1  0.1075      0.964 0.952 0.000 0.000 0.000 0.000 NA
#&gt; GSM892362     1  0.0146      0.980 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892366     3  0.0935      0.779 0.000 0.000 0.964 0.000 0.032 NA
#&gt; GSM892370     3  0.3828      0.696 0.000 0.000 0.560 0.000 0.000 NA
#&gt; GSM892374     2  0.0865      0.974 0.000 0.964 0.000 0.000 0.000 NA
#&gt; GSM892378     2  0.0713      0.978 0.000 0.972 0.000 0.000 0.000 NA
#&gt; GSM892382     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892384     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892388     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892343     5  0.3694      0.755 0.000 0.000 0.044 0.032 0.812 NA
#&gt; GSM892348     4  0.0891      0.940 0.000 0.000 0.000 0.968 0.008 NA
#&gt; GSM892352     5  0.4534      0.654 0.000 0.000 0.008 0.052 0.672 NA
#&gt; GSM892358     1  0.0146      0.979 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892360     1  0.0146      0.979 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892364     1  0.0458      0.978 0.984 0.000 0.000 0.000 0.000 NA
#&gt; GSM892368     3  0.0935      0.781 0.000 0.000 0.964 0.000 0.032 NA
#&gt; GSM892372     3  0.0914      0.787 0.000 0.000 0.968 0.000 0.016 NA
#&gt; GSM892376     2  0.0260      0.990 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892380     2  0.0260      0.990 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892386     2  0.0260      0.990 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892390     2  0.0146      0.990 0.000 0.996 0.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> SD:skmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> SD:skmeans 47     0.847            0.875 1.47e-11      8.15e-05 3
#> SD:skmeans 50     1.000            0.986 6.71e-18      2.02e-07 4
#> SD:skmeans 50     1.000            0.998 3.06e-20      2.01e-07 5
#> SD:skmeans 49     0.996            0.982 2.33e-20      7.98e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.956           0.969       0.985         0.4258 0.798   0.619
#> 4 4 1.000           0.987       0.994         0.1135 0.912   0.737
#> 5 5 0.968           0.960       0.971         0.0348 0.958   0.840
#> 6 6 1.000           0.970       0.987         0.0173 0.988   0.946
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892345     3  0.3116      0.898 0.108  0 0.892
#&gt; GSM892349     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892353     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892355     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892361     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892369     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892347     3  0.2448      0.926 0.076  0 0.924
#&gt; GSM892351     3  0.1163      0.960 0.028  0 0.972
#&gt; GSM892357     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892371     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892346     1  0.5397      0.586 0.720  0 0.280
#&gt; GSM892350     3  0.3116      0.898 0.108  0 0.892
#&gt; GSM892354     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892370     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.0747      0.966 0.016  0 0.984
#&gt; GSM892348     3  0.3412      0.881 0.124  0 0.876
#&gt; GSM892352     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892358     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.975 1.000  0 0.000
#&gt; GSM892364     1  0.0237      0.972 0.996  0 0.004
#&gt; GSM892368     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.974 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892349     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.3528      0.767 0.000  0 0.808 0.192
#&gt; GSM892347     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892371     3  0.2281      0.888 0.000  0 0.904 0.096
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892346     4  0.0188      0.995 0.004  0 0.000 0.996
#&gt; GSM892350     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892348     4  0.0000      0.998 0.000  0 0.000 1.000
#&gt; GSM892352     4  0.0336      0.992 0.000  0 0.008 0.992
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.970 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.0000      0.849 0.000  0 0.000 0.000 1.000
#&gt; GSM892345     4  0.0000      1.000 0.000  0 0.000 1.000 0.000
#&gt; GSM892349     5  0.4060      0.561 0.000  0 0.000 0.360 0.640
#&gt; GSM892353     1  0.2127      0.874 0.892  0 0.000 0.000 0.108
#&gt; GSM892355     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.2471      0.936 0.000  0 0.864 0.000 0.136
#&gt; GSM892369     3  0.0000      0.913 0.000  0 1.000 0.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.1121      0.862 0.000  0 0.000 0.044 0.956
#&gt; GSM892347     4  0.0000      1.000 0.000  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.0000      1.000 0.000  0 0.000 1.000 0.000
#&gt; GSM892357     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.1792      0.934 0.000  0 0.916 0.000 0.084
#&gt; GSM892371     3  0.0771      0.902 0.000  0 0.976 0.020 0.004
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.0000      0.849 0.000  0 0.000 0.000 1.000
#&gt; GSM892346     4  0.0000      1.000 0.000  0 0.000 1.000 0.000
#&gt; GSM892350     4  0.0000      1.000 0.000  0 0.000 1.000 0.000
#&gt; GSM892354     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.2471      0.936 0.000  0 0.864 0.000 0.136
#&gt; GSM892370     3  0.0000      0.913 0.000  0 1.000 0.000 0.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.2377      0.848 0.000  0 0.000 0.128 0.872
#&gt; GSM892348     4  0.0000      1.000 0.000  0 0.000 1.000 0.000
#&gt; GSM892352     5  0.2605      0.838 0.000  0 0.000 0.148 0.852
#&gt; GSM892358     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.990 1.000  0 0.000 0.000 0.000
#&gt; GSM892368     3  0.2471      0.936 0.000  0 0.864 0.000 0.136
#&gt; GSM892372     3  0.2471      0.936 0.000  0 0.864 0.000 0.136
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.0000      0.927 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892345     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892349     5  0.3330      0.616 0.000 0.000 0.000 0.284 0.716 0.000
#&gt; GSM892353     1  0.1910      0.872 0.892 0.000 0.000 0.000 0.108 0.000
#&gt; GSM892355     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.951 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892369     6  0.0260      1.000 0.000 0.000 0.008 0.000 0.000 0.992
#&gt; GSM892373     2  0.0260      0.994 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892377     2  0.0260      0.994 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892381     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.0000      0.927 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892347     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892351     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892357     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.2697      0.766 0.000 0.000 0.812 0.000 0.000 0.188
#&gt; GSM892371     6  0.0260      1.000 0.000 0.000 0.008 0.000 0.000 0.992
#&gt; GSM892375     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892379     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.0000      0.927 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892346     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892350     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892354     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.951 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892370     6  0.0260      1.000 0.000 0.000 0.008 0.000 0.000 0.992
#&gt; GSM892374     2  0.0260      0.994 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892378     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892343     5  0.0000      0.927 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892348     4  0.0000      1.000 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892352     5  0.0713      0.912 0.000 0.000 0.000 0.028 0.972 0.000
#&gt; GSM892358     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.0000      0.951 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892372     3  0.0000      0.951 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892376     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) disease.state(p) other(p) individual(p) k
#> SD:pam 50     1.000            0.934 2.67e-07      1.42e-03 2
#> SD:pam 50     0.937            0.873 9.23e-12      3.89e-05 3
#> SD:pam 50     0.977            0.951 1.45e-16      7.83e-07 4
#> SD:pam 50     1.000            0.998 3.06e-20      2.01e-07 5
#> SD:pam 50     0.991            0.984 4.62e-22      7.12e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           1.000       1.000         0.4158 0.804   0.630
#> 4 4 0.971           0.919       0.967         0.1192 0.919   0.758
#> 5 5 1.000           0.992       0.996         0.0377 0.958   0.840
#> 6 6 0.980           0.927       0.969         0.0279 0.978   0.903
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM892342     3       0          1  0  0  1
#&gt; GSM892345     3       0          1  0  0  1
#&gt; GSM892349     3       0          1  0  0  1
#&gt; GSM892353     1       0          1  1  0  0
#&gt; GSM892355     1       0          1  1  0  0
#&gt; GSM892361     1       0          1  1  0  0
#&gt; GSM892365     3       0          1  0  0  1
#&gt; GSM892369     3       0          1  0  0  1
#&gt; GSM892373     2       0          1  0  1  0
#&gt; GSM892377     2       0          1  0  1  0
#&gt; GSM892381     2       0          1  0  1  0
#&gt; GSM892383     2       0          1  0  1  0
#&gt; GSM892387     2       0          1  0  1  0
#&gt; GSM892344     3       0          1  0  0  1
#&gt; GSM892347     3       0          1  0  0  1
#&gt; GSM892351     3       0          1  0  0  1
#&gt; GSM892357     1       0          1  1  0  0
#&gt; GSM892359     1       0          1  1  0  0
#&gt; GSM892363     1       0          1  1  0  0
#&gt; GSM892367     3       0          1  0  0  1
#&gt; GSM892371     3       0          1  0  0  1
#&gt; GSM892375     2       0          1  0  1  0
#&gt; GSM892379     2       0          1  0  1  0
#&gt; GSM892385     2       0          1  0  1  0
#&gt; GSM892389     2       0          1  0  1  0
#&gt; GSM892341     3       0          1  0  0  1
#&gt; GSM892346     3       0          1  0  0  1
#&gt; GSM892350     3       0          1  0  0  1
#&gt; GSM892354     1       0          1  1  0  0
#&gt; GSM892356     1       0          1  1  0  0
#&gt; GSM892362     1       0          1  1  0  0
#&gt; GSM892366     3       0          1  0  0  1
#&gt; GSM892370     3       0          1  0  0  1
#&gt; GSM892374     2       0          1  0  1  0
#&gt; GSM892378     2       0          1  0  1  0
#&gt; GSM892382     2       0          1  0  1  0
#&gt; GSM892384     2       0          1  0  1  0
#&gt; GSM892388     2       0          1  0  1  0
#&gt; GSM892343     3       0          1  0  0  1
#&gt; GSM892348     3       0          1  0  0  1
#&gt; GSM892352     3       0          1  0  0  1
#&gt; GSM892358     1       0          1  1  0  0
#&gt; GSM892360     1       0          1  1  0  0
#&gt; GSM892364     1       0          1  1  0  0
#&gt; GSM892368     3       0          1  0  0  1
#&gt; GSM892372     3       0          1  0  0  1
#&gt; GSM892376     2       0          1  0  1  0
#&gt; GSM892380     2       0          1  0  1  0
#&gt; GSM892386     2       0          1  0  1  0
#&gt; GSM892390     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2    p3    p4
#&gt; GSM892342     3  0.0000     0.8782  0  0 1.000 0.000
#&gt; GSM892345     4  0.0000     0.9167  0  0 0.000 1.000
#&gt; GSM892349     4  0.0592     0.9079  0  0 0.016 0.984
#&gt; GSM892353     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892355     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892361     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892365     3  0.0000     0.8782  0  0 1.000 0.000
#&gt; GSM892369     3  0.4564     0.4888  0  0 0.672 0.328
#&gt; GSM892373     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892377     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892381     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892383     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892387     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892344     3  0.0469     0.8769  0  0 0.988 0.012
#&gt; GSM892347     4  0.0000     0.9167  0  0 0.000 1.000
#&gt; GSM892351     4  0.0000     0.9167  0  0 0.000 1.000
#&gt; GSM892357     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892359     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892363     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892367     3  0.1022     0.8624  0  0 0.968 0.032
#&gt; GSM892371     3  0.4985     0.0974  0  0 0.532 0.468
#&gt; GSM892375     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892379     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892385     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892389     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892341     3  0.0188     0.8785  0  0 0.996 0.004
#&gt; GSM892346     4  0.0000     0.9167  0  0 0.000 1.000
#&gt; GSM892350     4  0.0000     0.9167  0  0 0.000 1.000
#&gt; GSM892354     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892356     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892362     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892366     3  0.0188     0.8782  0  0 0.996 0.004
#&gt; GSM892370     4  0.4164     0.6298  0  0 0.264 0.736
#&gt; GSM892374     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892378     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892382     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892384     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892388     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892343     3  0.3726     0.7082  0  0 0.788 0.212
#&gt; GSM892348     4  0.0000     0.9167  0  0 0.000 1.000
#&gt; GSM892352     4  0.4304     0.5967  0  0 0.284 0.716
#&gt; GSM892358     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892360     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892364     1  0.0000     1.0000  1  0 0.000 0.000
#&gt; GSM892368     3  0.0000     0.8782  0  0 1.000 0.000
#&gt; GSM892372     3  0.0592     0.8744  0  0 0.984 0.016
#&gt; GSM892376     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892380     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892386     2  0.0000     1.0000  0  1 0.000 0.000
#&gt; GSM892390     2  0.0000     1.0000  0  1 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2    p3    p4    p5
#&gt; GSM892342     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892345     4  0.0000      1.000  0  0 0.000 1.000 0.000
#&gt; GSM892349     5  0.2020      0.890  0  0 0.000 0.100 0.900
#&gt; GSM892353     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892369     5  0.0609      0.965  0  0 0.020 0.000 0.980
#&gt; GSM892373     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892344     3  0.1410      0.938  0  0 0.940 0.000 0.060
#&gt; GSM892347     4  0.0000      1.000  0  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.0000      1.000  0  0 0.000 1.000 0.000
#&gt; GSM892357     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892371     5  0.0000      0.973  0  0 0.000 0.000 1.000
#&gt; GSM892375     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892341     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892346     4  0.0000      1.000  0  0 0.000 1.000 0.000
#&gt; GSM892350     4  0.0000      1.000  0  0 0.000 1.000 0.000
#&gt; GSM892354     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892370     5  0.0290      0.972  0  0 0.008 0.000 0.992
#&gt; GSM892374     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.0162      0.973  0  0 0.000 0.004 0.996
#&gt; GSM892348     4  0.0000      1.000  0  0 0.000 1.000 0.000
#&gt; GSM892352     5  0.0000      0.973  0  0 0.000 0.000 1.000
#&gt; GSM892358     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000  1  0 0.000 0.000 0.000
#&gt; GSM892368     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892372     3  0.0000      0.992  0  0 1.000 0.000 0.000
#&gt; GSM892376     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000  0  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000  0  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM892342     3  0.0146      0.963 0.000  0 0.996 0.000 0.004 0.000
#&gt; GSM892345     4  0.0260      0.989 0.000  0 0.000 0.992 0.000 0.008
#&gt; GSM892349     5  0.0972      0.962 0.000  0 0.000 0.028 0.964 0.008
#&gt; GSM892353     6  0.0632      0.657 0.024  0 0.000 0.000 0.000 0.976
#&gt; GSM892355     6  0.3198      0.660 0.260  0 0.000 0.000 0.000 0.740
#&gt; GSM892361     1  0.0000      0.905 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.964 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892369     5  0.0146      0.988 0.000  0 0.004 0.000 0.996 0.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892344     3  0.2664      0.775 0.000  0 0.816 0.000 0.184 0.000
#&gt; GSM892347     4  0.0000      0.991 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM892351     4  0.0713      0.974 0.000  0 0.000 0.972 0.000 0.028
#&gt; GSM892357     1  0.0000      0.905 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0937      0.878 0.960  0 0.000 0.000 0.000 0.040
#&gt; GSM892363     1  0.0146      0.904 0.996  0 0.000 0.000 0.000 0.004
#&gt; GSM892367     3  0.0000      0.964 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892371     5  0.0000      0.988 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892341     3  0.0458      0.959 0.000  0 0.984 0.000 0.016 0.000
#&gt; GSM892346     4  0.0260      0.989 0.000  0 0.000 0.992 0.000 0.008
#&gt; GSM892350     4  0.0146      0.990 0.000  0 0.000 0.996 0.000 0.004
#&gt; GSM892354     1  0.0363      0.900 0.988  0 0.000 0.000 0.000 0.012
#&gt; GSM892356     1  0.1387      0.840 0.932  0 0.000 0.000 0.000 0.068
#&gt; GSM892362     1  0.0000      0.905 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.964 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892370     5  0.0146      0.988 0.000  0 0.004 0.000 0.996 0.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892343     5  0.0405      0.984 0.000  0 0.004 0.008 0.988 0.000
#&gt; GSM892348     4  0.0000      0.991 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM892352     5  0.0000      0.988 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892358     6  0.3672      0.500 0.368  0 0.000 0.000 0.000 0.632
#&gt; GSM892360     1  0.0000      0.905 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.3854      0.049 0.536  0 0.000 0.000 0.000 0.464
#&gt; GSM892368     3  0.0000      0.964 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892372     3  0.0547      0.956 0.000  0 0.980 0.000 0.020 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) disease.state(p) other(p) individual(p) k
#> SD:mclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> SD:mclust 50     1.000            0.931 1.26e-12      1.60e-05 3
#> SD:mclust 48     0.987            0.948 6.57e-16      1.01e-06 4
#> SD:mclust 50     1.000            0.998 1.59e-16      6.55e-06 5
#> SD:mclust 48     0.860            0.863 1.85e-13      3.25e-04 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.927           0.922       0.966         0.4345 0.792   0.607
#> 4 4 0.996           0.966       0.979         0.0990 0.909   0.729
#> 5 5 0.951           0.930       0.944         0.0348 0.958   0.838
#> 6 6 0.924           0.926       0.944         0.0139 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892345     1   0.536      0.673 0.724  0 0.276
#&gt; GSM892349     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892353     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892355     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892361     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892365     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892369     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892373     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892344     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892347     1   0.581      0.575 0.664  0 0.336
#&gt; GSM892351     3   0.593      0.337 0.356  0 0.644
#&gt; GSM892357     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892359     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892363     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892367     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892371     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892341     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892346     1   0.382      0.812 0.852  0 0.148
#&gt; GSM892350     1   0.590      0.543 0.648  0 0.352
#&gt; GSM892354     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892356     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892362     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892366     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892370     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892374     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892388     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892343     3   0.153      0.927 0.040  0 0.960
#&gt; GSM892348     1   0.475      0.748 0.784  0 0.216
#&gt; GSM892352     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892358     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892360     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892364     1   0.000      0.910 1.000  0 0.000
#&gt; GSM892368     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892372     3   0.000      0.967 0.000  0 1.000
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.0376      0.948 0.004 0.000 0.992 0.004
#&gt; GSM892345     4  0.1022      0.966 0.000 0.000 0.032 0.968
#&gt; GSM892349     4  0.2530      0.910 0.000 0.000 0.112 0.888
#&gt; GSM892353     1  0.0188      0.995 0.996 0.000 0.004 0.000
#&gt; GSM892355     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0188      0.997 0.996 0.000 0.000 0.004
#&gt; GSM892365     3  0.0188      0.947 0.004 0.000 0.996 0.000
#&gt; GSM892369     3  0.0817      0.945 0.000 0.000 0.976 0.024
#&gt; GSM892373     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892377     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892381     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892387     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892344     3  0.1489      0.933 0.004 0.000 0.952 0.044
#&gt; GSM892347     4  0.1022      0.966 0.000 0.000 0.032 0.968
#&gt; GSM892351     4  0.1118      0.965 0.000 0.000 0.036 0.964
#&gt; GSM892357     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.0188      0.947 0.004 0.000 0.996 0.000
#&gt; GSM892371     3  0.1792      0.913 0.000 0.000 0.932 0.068
#&gt; GSM892375     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892385     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM892389     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892341     3  0.0779      0.947 0.004 0.000 0.980 0.016
#&gt; GSM892346     4  0.0895      0.959 0.004 0.000 0.020 0.976
#&gt; GSM892350     4  0.1022      0.966 0.000 0.000 0.032 0.968
#&gt; GSM892354     1  0.0188      0.997 0.996 0.000 0.000 0.004
#&gt; GSM892356     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0188      0.997 0.996 0.000 0.000 0.004
#&gt; GSM892366     3  0.0188      0.947 0.004 0.000 0.996 0.000
#&gt; GSM892370     3  0.0921      0.943 0.000 0.000 0.972 0.028
#&gt; GSM892374     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892378     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892388     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM892343     3  0.4730      0.411 0.000 0.000 0.636 0.364
#&gt; GSM892348     4  0.0817      0.962 0.000 0.000 0.024 0.976
#&gt; GSM892352     4  0.2868      0.883 0.000 0.000 0.136 0.864
#&gt; GSM892358     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0188      0.997 0.996 0.000 0.000 0.004
#&gt; GSM892364     1  0.0188      0.997 0.996 0.000 0.000 0.004
#&gt; GSM892368     3  0.0188      0.947 0.004 0.000 0.996 0.000
#&gt; GSM892372     3  0.0524      0.948 0.004 0.000 0.988 0.008
#&gt; GSM892376     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892380     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892386     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM892390     2  0.0188      0.997 0.000 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     5  0.3399      0.773 0.000 0.000 0.168 0.020 0.812
#&gt; GSM892345     4  0.1608      0.937 0.000 0.000 0.000 0.928 0.072
#&gt; GSM892349     5  0.4564      0.447 0.000 0.000 0.016 0.372 0.612
#&gt; GSM892353     1  0.0703      0.977 0.976 0.000 0.000 0.000 0.024
#&gt; GSM892355     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.3336      0.822 0.000 0.000 0.772 0.000 0.228
#&gt; GSM892369     3  0.0671      0.831 0.000 0.000 0.980 0.004 0.016
#&gt; GSM892373     2  0.0609      0.988 0.000 0.980 0.000 0.000 0.020
#&gt; GSM892377     2  0.0404      0.990 0.000 0.988 0.000 0.000 0.012
#&gt; GSM892381     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892383     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892387     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892344     5  0.3432      0.805 0.000 0.000 0.132 0.040 0.828
#&gt; GSM892347     4  0.0703      0.943 0.000 0.000 0.000 0.976 0.024
#&gt; GSM892351     4  0.2286      0.900 0.000 0.000 0.004 0.888 0.108
#&gt; GSM892357     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.1638      0.874 0.000 0.000 0.932 0.004 0.064
#&gt; GSM892371     3  0.1877      0.874 0.000 0.000 0.924 0.012 0.064
#&gt; GSM892375     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0162      0.994 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892385     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892389     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.3454      0.791 0.000 0.000 0.156 0.028 0.816
#&gt; GSM892346     4  0.0566      0.924 0.000 0.000 0.004 0.984 0.012
#&gt; GSM892350     4  0.1478      0.942 0.000 0.000 0.000 0.936 0.064
#&gt; GSM892354     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.3508      0.793 0.000 0.000 0.748 0.000 0.252
#&gt; GSM892370     3  0.0162      0.843 0.000 0.000 0.996 0.004 0.000
#&gt; GSM892374     2  0.0609      0.985 0.000 0.980 0.000 0.000 0.020
#&gt; GSM892378     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892384     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892388     2  0.0510      0.991 0.000 0.984 0.000 0.000 0.016
#&gt; GSM892343     5  0.4364      0.802 0.000 0.000 0.112 0.120 0.768
#&gt; GSM892348     4  0.0000      0.934 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892352     5  0.3852      0.710 0.000 0.000 0.020 0.220 0.760
#&gt; GSM892358     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.3010      0.866 0.000 0.000 0.824 0.004 0.172
#&gt; GSM892372     3  0.2930      0.870 0.000 0.000 0.832 0.004 0.164
#&gt; GSM892376     2  0.0162      0.995 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892380     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     5  0.2163      0.814 0.000 0.000 0.096 0.004 0.892 NA
#&gt; GSM892345     4  0.2178      0.880 0.000 0.000 0.000 0.868 0.132 NA
#&gt; GSM892349     5  0.3684      0.522 0.000 0.000 0.004 0.332 0.664 NA
#&gt; GSM892353     1  0.0363      0.989 0.988 0.000 0.000 0.000 0.012 NA
#&gt; GSM892355     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892361     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892365     3  0.1387      0.952 0.000 0.000 0.932 0.000 0.068 NA
#&gt; GSM892369     3  0.1078      0.943 0.000 0.000 0.964 0.012 0.016 NA
#&gt; GSM892373     2  0.2697      0.864 0.000 0.812 0.000 0.000 0.000 NA
#&gt; GSM892377     2  0.2003      0.916 0.000 0.884 0.000 0.000 0.000 NA
#&gt; GSM892381     2  0.0146      0.964 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892383     2  0.0146      0.964 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892387     2  0.0260      0.964 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892344     5  0.2138      0.835 0.000 0.000 0.052 0.036 0.908 NA
#&gt; GSM892347     4  0.1387      0.891 0.000 0.000 0.000 0.932 0.068 NA
#&gt; GSM892351     4  0.3287      0.768 0.000 0.000 0.000 0.768 0.220 NA
#&gt; GSM892357     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892359     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892363     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892367     3  0.0806      0.961 0.000 0.000 0.972 0.000 0.020 NA
#&gt; GSM892371     3  0.0779      0.961 0.000 0.000 0.976 0.008 0.008 NA
#&gt; GSM892375     2  0.0937      0.956 0.000 0.960 0.000 0.000 0.000 NA
#&gt; GSM892379     2  0.0790      0.959 0.000 0.968 0.000 0.000 0.000 NA
#&gt; GSM892385     2  0.0146      0.964 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892389     2  0.0260      0.964 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892341     5  0.2121      0.825 0.000 0.000 0.096 0.012 0.892 NA
#&gt; GSM892346     4  0.1219      0.821 0.000 0.000 0.004 0.948 0.000 NA
#&gt; GSM892350     4  0.2146      0.887 0.000 0.000 0.000 0.880 0.116 NA
#&gt; GSM892354     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892356     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892362     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892366     3  0.1700      0.942 0.000 0.000 0.916 0.000 0.080 NA
#&gt; GSM892370     3  0.0508      0.955 0.000 0.000 0.984 0.004 0.000 NA
#&gt; GSM892374     2  0.2762      0.859 0.000 0.804 0.000 0.000 0.000 NA
#&gt; GSM892378     2  0.1267      0.948 0.000 0.940 0.000 0.000 0.000 NA
#&gt; GSM892382     2  0.0146      0.964 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892384     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM892388     2  0.1714      0.928 0.000 0.908 0.000 0.000 0.000 NA
#&gt; GSM892343     5  0.3103      0.826 0.000 0.000 0.064 0.100 0.836 NA
#&gt; GSM892348     4  0.0972      0.874 0.000 0.000 0.000 0.964 0.028 NA
#&gt; GSM892352     5  0.3323      0.731 0.000 0.000 0.008 0.204 0.780 NA
#&gt; GSM892358     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892360     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892364     1  0.0260      0.993 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892368     3  0.1349      0.957 0.000 0.000 0.940 0.000 0.056 NA
#&gt; GSM892372     3  0.1194      0.963 0.000 0.000 0.956 0.004 0.032 NA
#&gt; GSM892376     2  0.0363      0.964 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892380     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM892386     2  0.0000      0.964 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM892390     2  0.0146      0.964 0.000 0.996 0.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) disease.state(p) other(p) individual(p) k
#> SD:NMF 50     1.000            0.934 2.67e-07      1.42e-03 2
#> SD:NMF 49     0.981            0.952 1.63e-11      6.57e-05 3
#> SD:NMF 49     0.995            0.988 2.28e-17      4.54e-07 4
#> SD:NMF 49     0.996            0.982 2.33e-20      7.98e-08 5
#> SD:NMF 50     1.000            0.998 3.06e-20      2.01e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.971           0.944       0.969         0.4350 0.794   0.612
#> 4 4 0.982           0.944       0.969         0.0511 0.980   0.940
#> 5 5 0.904           0.782       0.857         0.0615 0.948   0.832
#> 6 6 0.890           0.873       0.871         0.0398 0.947   0.802
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892345     3  0.5948      0.569 0.360  0 0.640
#&gt; GSM892349     3  0.2796      0.869 0.092  0 0.908
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892369     3  0.2165      0.891 0.064  0 0.936
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892347     3  0.5988      0.554 0.368  0 0.632
#&gt; GSM892351     3  0.4555      0.778 0.200  0 0.800
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892371     3  0.2165      0.891 0.064  0 0.936
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892346     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892350     3  0.5560      0.667 0.300  0 0.700
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892370     3  0.2165      0.891 0.064  0 0.936
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.0892      0.905 0.020  0 0.980
#&gt; GSM892348     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892352     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892364     1  0.0237      0.995 0.996  0 0.004
#&gt; GSM892368     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.908 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892345     3  0.4713      0.585 0.000  0 0.640 0.360
#&gt; GSM892349     3  0.2216      0.862 0.000  0 0.908 0.092
#&gt; GSM892353     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892355     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.1716      0.886 0.000  0 0.936 0.064
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892347     3  0.4746      0.571 0.000  0 0.632 0.368
#&gt; GSM892351     3  0.3610      0.774 0.000  0 0.800 0.200
#&gt; GSM892357     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892371     3  0.1716      0.886 0.000  0 0.936 0.064
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892346     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM892350     3  0.4406      0.675 0.000  0 0.700 0.300
#&gt; GSM892354     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.1716      0.886 0.000  0 0.936 0.064
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.0707      0.903 0.000  0 0.980 0.020
#&gt; GSM892348     4  0.0000      1.000 0.000  0 0.000 1.000
#&gt; GSM892352     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892358     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      0.999 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0188      0.994 0.996  0 0.004 0.000
#&gt; GSM892368     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.907 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4 p5
#&gt; GSM892342     3  0.5534      0.478 0.000  0 0.508 0.068 NA
#&gt; GSM892345     4  0.3857      0.532 0.000  0 0.312 0.688 NA
#&gt; GSM892349     3  0.6127     -0.220 0.000  0 0.456 0.416 NA
#&gt; GSM892353     1  0.0162      0.966 0.996  0 0.000 0.000 NA
#&gt; GSM892355     1  0.0162      0.966 0.996  0 0.000 0.000 NA
#&gt; GSM892361     1  0.1965      0.945 0.904  0 0.000 0.000 NA
#&gt; GSM892365     3  0.0703      0.640 0.000  0 0.976 0.000 NA
#&gt; GSM892369     3  0.3471      0.539 0.000  0 0.836 0.072 NA
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892344     3  0.5534      0.478 0.000  0 0.508 0.068 NA
#&gt; GSM892347     4  0.3816      0.535 0.000  0 0.304 0.696 NA
#&gt; GSM892351     4  0.4961      0.282 0.000  0 0.448 0.524 NA
#&gt; GSM892357     1  0.1965      0.945 0.904  0 0.000 0.000 NA
#&gt; GSM892359     1  0.1792      0.950 0.916  0 0.000 0.000 NA
#&gt; GSM892363     1  0.0000      0.967 1.000  0 0.000 0.000 NA
#&gt; GSM892367     3  0.0451      0.630 0.000  0 0.988 0.008 NA
#&gt; GSM892371     3  0.3471      0.539 0.000  0 0.836 0.072 NA
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892341     3  0.5534      0.478 0.000  0 0.508 0.068 NA
#&gt; GSM892346     4  0.4126      0.416 0.000  0 0.000 0.620 NA
#&gt; GSM892350     4  0.4668      0.483 0.000  0 0.352 0.624 NA
#&gt; GSM892354     1  0.1965      0.945 0.904  0 0.000 0.000 NA
#&gt; GSM892356     1  0.0162      0.966 0.996  0 0.000 0.000 NA
#&gt; GSM892362     1  0.1121      0.961 0.956  0 0.000 0.000 NA
#&gt; GSM892366     3  0.0703      0.640 0.000  0 0.976 0.000 NA
#&gt; GSM892370     3  0.3471      0.539 0.000  0 0.836 0.072 NA
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892343     3  0.5400      0.512 0.000  0 0.632 0.096 NA
#&gt; GSM892348     4  0.4126      0.416 0.000  0 0.000 0.620 NA
#&gt; GSM892352     3  0.6300      0.391 0.000  0 0.424 0.152 NA
#&gt; GSM892358     1  0.0000      0.967 1.000  0 0.000 0.000 NA
#&gt; GSM892360     1  0.0510      0.966 0.984  0 0.000 0.000 NA
#&gt; GSM892364     1  0.0324      0.964 0.992  0 0.004 0.000 NA
#&gt; GSM892368     3  0.0290      0.635 0.000  0 0.992 0.000 NA
#&gt; GSM892372     3  0.0703      0.640 0.000  0 0.976 0.000 NA
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 NA
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 NA
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.5737      0.888 0.000  0 0.236 0.248 0.516 0.000
#&gt; GSM892345     4  0.4215      0.797 0.000  0 0.056 0.700 0.000 0.244
#&gt; GSM892349     4  0.2147      0.657 0.000  0 0.084 0.896 0.020 0.000
#&gt; GSM892353     1  0.0976      0.801 0.968  0 0.000 0.016 0.008 0.008
#&gt; GSM892355     1  0.0976      0.801 0.968  0 0.000 0.016 0.008 0.008
#&gt; GSM892361     1  0.3838      0.677 0.552  0 0.000 0.000 0.448 0.000
#&gt; GSM892365     3  0.2170      0.830 0.000  0 0.888 0.100 0.012 0.000
#&gt; GSM892369     3  0.2764      0.776 0.000  0 0.864 0.100 0.028 0.008
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.5737      0.888 0.000  0 0.236 0.248 0.516 0.000
#&gt; GSM892347     4  0.4145      0.788 0.000  0 0.048 0.700 0.000 0.252
#&gt; GSM892351     4  0.2852      0.785 0.000  0 0.064 0.856 0.000 0.080
#&gt; GSM892357     1  0.3833      0.679 0.556  0 0.000 0.000 0.444 0.000
#&gt; GSM892359     1  0.3765      0.700 0.596  0 0.000 0.000 0.404 0.000
#&gt; GSM892363     1  0.0713      0.815 0.972  0 0.000 0.000 0.028 0.000
#&gt; GSM892367     3  0.2003      0.837 0.000  0 0.884 0.116 0.000 0.000
#&gt; GSM892371     3  0.2764      0.776 0.000  0 0.864 0.100 0.028 0.008
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.5737      0.888 0.000  0 0.236 0.248 0.516 0.000
#&gt; GSM892346     6  0.0260      1.000 0.000  0 0.000 0.008 0.000 0.992
#&gt; GSM892350     4  0.3885      0.816 0.000  0 0.064 0.756 0.000 0.180
#&gt; GSM892354     1  0.3838      0.677 0.552  0 0.000 0.000 0.448 0.000
#&gt; GSM892356     1  0.0976      0.801 0.968  0 0.000 0.016 0.008 0.008
#&gt; GSM892362     1  0.2854      0.779 0.792  0 0.000 0.000 0.208 0.000
#&gt; GSM892366     3  0.2170      0.830 0.000  0 0.888 0.100 0.012 0.000
#&gt; GSM892370     3  0.2764      0.776 0.000  0 0.864 0.100 0.028 0.008
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892343     5  0.6029      0.725 0.000  0 0.260 0.324 0.416 0.000
#&gt; GSM892348     6  0.0260      1.000 0.000  0 0.000 0.008 0.000 0.992
#&gt; GSM892352     5  0.5271      0.759 0.000  0 0.104 0.380 0.516 0.000
#&gt; GSM892358     1  0.0363      0.813 0.988  0 0.000 0.000 0.012 0.000
#&gt; GSM892360     1  0.1327      0.812 0.936  0 0.000 0.000 0.064 0.000
#&gt; GSM892364     1  0.0820      0.811 0.972  0 0.000 0.012 0.016 0.000
#&gt; GSM892368     3  0.2053      0.837 0.000  0 0.888 0.108 0.004 0.000
#&gt; GSM892372     3  0.2170      0.830 0.000  0 0.888 0.100 0.012 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) disease.state(p) other(p) individual(p) k
#> CV:hclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> CV:hclust 50     0.776            0.931 1.69e-11      8.93e-05 3
#> CV:hclust 50     0.528            0.986 4.32e-13      2.04e-04 4
#> CV:hclust 41     0.554            0.958 3.28e-14      9.32e-06 5
#> CV:hclust 50     0.549            0.990 7.86e-20      2.17e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.707           0.921       0.860         0.3385 0.794   0.612
#> 4 4 0.619           0.866       0.836         0.1225 0.928   0.783
#> 5 5 0.736           0.691       0.822         0.0795 0.974   0.902
#> 6 6 0.715           0.703       0.785         0.0485 0.974   0.892
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM892342     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892345     3  0.5016      0.547 0.240 0.000 0.760
#&gt; GSM892349     3  0.0424      0.929 0.008 0.000 0.992
#&gt; GSM892353     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892355     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892361     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892365     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892369     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892373     2  0.4452      0.909 0.192 0.808 0.000
#&gt; GSM892377     2  0.4235      0.916 0.176 0.824 0.000
#&gt; GSM892381     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM892383     2  0.1411      0.942 0.036 0.964 0.000
#&gt; GSM892387     2  0.1860      0.945 0.052 0.948 0.000
#&gt; GSM892344     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892347     3  0.5016      0.547 0.240 0.000 0.760
#&gt; GSM892351     3  0.0424      0.929 0.008 0.000 0.992
#&gt; GSM892357     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892359     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892363     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892367     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892371     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892375     2  0.3116      0.935 0.108 0.892 0.000
#&gt; GSM892379     2  0.2261      0.945 0.068 0.932 0.000
#&gt; GSM892385     2  0.3340      0.919 0.120 0.880 0.000
#&gt; GSM892389     2  0.2165      0.947 0.064 0.936 0.000
#&gt; GSM892341     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892346     1  0.5529      0.975 0.704 0.000 0.296
#&gt; GSM892350     3  0.5016      0.547 0.240 0.000 0.760
#&gt; GSM892354     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892356     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892362     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892366     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892370     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892374     2  0.4452      0.909 0.192 0.808 0.000
#&gt; GSM892378     2  0.4235      0.916 0.176 0.824 0.000
#&gt; GSM892382     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM892384     2  0.0237      0.945 0.004 0.996 0.000
#&gt; GSM892388     2  0.3038      0.924 0.104 0.896 0.000
#&gt; GSM892343     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892348     1  0.6140      0.792 0.596 0.000 0.404
#&gt; GSM892352     3  0.0424      0.929 0.008 0.000 0.992
#&gt; GSM892358     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892360     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892364     1  0.5591      0.986 0.696 0.000 0.304
#&gt; GSM892368     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892372     3  0.0000      0.935 0.000 0.000 1.000
#&gt; GSM892376     2  0.2959      0.937 0.100 0.900 0.000
#&gt; GSM892380     2  0.0000      0.945 0.000 1.000 0.000
#&gt; GSM892386     2  0.1964      0.934 0.056 0.944 0.000
#&gt; GSM892390     2  0.2625      0.931 0.084 0.916 0.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.3763      0.905 0.144 0.000 0.832 0.024
#&gt; GSM892345     4  0.7336      0.764 0.256 0.000 0.216 0.528
#&gt; GSM892349     3  0.4307      0.895 0.144 0.000 0.808 0.048
#&gt; GSM892353     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892355     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892361     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.2973      0.905 0.144 0.000 0.856 0.000
#&gt; GSM892369     3  0.6295      0.677 0.144 0.000 0.660 0.196
#&gt; GSM892373     2  0.4605      0.814 0.000 0.664 0.000 0.336
#&gt; GSM892377     2  0.4807      0.839 0.000 0.728 0.024 0.248
#&gt; GSM892381     2  0.0524      0.891 0.000 0.988 0.008 0.004
#&gt; GSM892383     2  0.2021      0.882 0.000 0.932 0.012 0.056
#&gt; GSM892387     2  0.3307      0.887 0.000 0.868 0.028 0.104
#&gt; GSM892344     3  0.3763      0.905 0.144 0.000 0.832 0.024
#&gt; GSM892347     4  0.7336      0.764 0.256 0.000 0.216 0.528
#&gt; GSM892351     4  0.7083      0.463 0.144 0.000 0.328 0.528
#&gt; GSM892357     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.3300      0.902 0.144 0.000 0.848 0.008
#&gt; GSM892371     3  0.6295      0.677 0.144 0.000 0.660 0.196
#&gt; GSM892375     2  0.3910      0.875 0.000 0.820 0.024 0.156
#&gt; GSM892379     2  0.2737      0.894 0.000 0.888 0.008 0.104
#&gt; GSM892385     2  0.4817      0.829 0.000 0.784 0.088 0.128
#&gt; GSM892389     2  0.2924      0.894 0.000 0.884 0.016 0.100
#&gt; GSM892341     3  0.3763      0.905 0.144 0.000 0.832 0.024
#&gt; GSM892346     4  0.5000      0.430 0.496 0.000 0.000 0.504
#&gt; GSM892350     4  0.7336      0.764 0.256 0.000 0.216 0.528
#&gt; GSM892354     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892362     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.2973      0.905 0.144 0.000 0.856 0.000
#&gt; GSM892370     3  0.6295      0.677 0.144 0.000 0.660 0.196
#&gt; GSM892374     2  0.4605      0.814 0.000 0.664 0.000 0.336
#&gt; GSM892378     2  0.4776      0.840 0.000 0.732 0.024 0.244
#&gt; GSM892382     2  0.0524      0.891 0.000 0.988 0.008 0.004
#&gt; GSM892384     2  0.0927      0.891 0.000 0.976 0.016 0.008
#&gt; GSM892388     2  0.5080      0.830 0.000 0.764 0.092 0.144
#&gt; GSM892343     3  0.4541      0.886 0.144 0.000 0.796 0.060
#&gt; GSM892348     4  0.6229      0.631 0.416 0.000 0.056 0.528
#&gt; GSM892352     3  0.3763      0.905 0.144 0.000 0.832 0.024
#&gt; GSM892358     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.998 1.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892368     3  0.2973      0.905 0.144 0.000 0.856 0.000
#&gt; GSM892372     3  0.3300      0.902 0.144 0.000 0.848 0.008
#&gt; GSM892376     2  0.3862      0.876 0.000 0.824 0.024 0.152
#&gt; GSM892380     2  0.0817      0.893 0.000 0.976 0.000 0.024
#&gt; GSM892386     2  0.3168      0.877 0.000 0.884 0.060 0.056
#&gt; GSM892390     2  0.3652      0.867 0.000 0.856 0.052 0.092
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     3  0.2376      0.827 0.052 0.000 0.904 0.044 0.000
#&gt; GSM892345     4  0.4280      0.850 0.088 0.000 0.140 0.772 0.000
#&gt; GSM892349     3  0.3849      0.793 0.052 0.000 0.820 0.116 0.012
#&gt; GSM892353     1  0.1478      0.925 0.936 0.000 0.000 0.000 0.064
#&gt; GSM892355     1  0.1478      0.925 0.936 0.000 0.000 0.000 0.064
#&gt; GSM892361     1  0.1671      0.938 0.924 0.000 0.000 0.000 0.076
#&gt; GSM892365     3  0.2592      0.835 0.052 0.000 0.892 0.000 0.056
#&gt; GSM892369     3  0.6737      0.596 0.052 0.000 0.584 0.208 0.156
#&gt; GSM892373     5  0.4593      1.000 0.000 0.480 0.004 0.004 0.512
#&gt; GSM892377     2  0.5392     -0.542 0.000 0.568 0.024 0.024 0.384
#&gt; GSM892381     2  0.0324      0.619 0.000 0.992 0.004 0.004 0.000
#&gt; GSM892383     2  0.2853      0.572 0.000 0.880 0.004 0.040 0.076
#&gt; GSM892387     2  0.3707      0.470 0.000 0.828 0.016 0.036 0.120
#&gt; GSM892344     3  0.2376      0.827 0.052 0.000 0.904 0.044 0.000
#&gt; GSM892347     4  0.4280      0.850 0.088 0.000 0.140 0.772 0.000
#&gt; GSM892351     4  0.4481      0.782 0.052 0.000 0.176 0.760 0.012
#&gt; GSM892357     1  0.1671      0.938 0.924 0.000 0.000 0.000 0.076
#&gt; GSM892359     1  0.1544      0.940 0.932 0.000 0.000 0.000 0.068
#&gt; GSM892363     1  0.0510      0.939 0.984 0.000 0.000 0.000 0.016
#&gt; GSM892367     3  0.3863      0.799 0.052 0.000 0.796 0.000 0.152
#&gt; GSM892371     3  0.6737      0.596 0.052 0.000 0.584 0.208 0.156
#&gt; GSM892375     2  0.4734      0.143 0.000 0.720 0.020 0.032 0.228
#&gt; GSM892379     2  0.2957      0.533 0.000 0.860 0.008 0.012 0.120
#&gt; GSM892385     2  0.5086      0.402 0.000 0.700 0.000 0.156 0.144
#&gt; GSM892389     2  0.3155      0.563 0.000 0.852 0.008 0.020 0.120
#&gt; GSM892341     3  0.2376      0.827 0.052 0.000 0.904 0.044 0.000
#&gt; GSM892346     4  0.4576      0.688 0.268 0.000 0.000 0.692 0.040
#&gt; GSM892350     4  0.4280      0.850 0.088 0.000 0.140 0.772 0.000
#&gt; GSM892354     1  0.1671      0.938 0.924 0.000 0.000 0.000 0.076
#&gt; GSM892356     1  0.1478      0.925 0.936 0.000 0.000 0.000 0.064
#&gt; GSM892362     1  0.1608      0.939 0.928 0.000 0.000 0.000 0.072
#&gt; GSM892366     3  0.2592      0.835 0.052 0.000 0.892 0.000 0.056
#&gt; GSM892370     3  0.6737      0.596 0.052 0.000 0.584 0.208 0.156
#&gt; GSM892374     5  0.4593      1.000 0.000 0.480 0.004 0.004 0.512
#&gt; GSM892378     2  0.5392     -0.542 0.000 0.568 0.024 0.024 0.384
#&gt; GSM892382     2  0.0324      0.619 0.000 0.992 0.004 0.004 0.000
#&gt; GSM892384     2  0.0833      0.617 0.000 0.976 0.004 0.016 0.004
#&gt; GSM892388     2  0.5032      0.399 0.000 0.704 0.000 0.168 0.128
#&gt; GSM892343     3  0.3555      0.799 0.052 0.000 0.824 0.124 0.000
#&gt; GSM892348     4  0.4288      0.758 0.224 0.000 0.004 0.740 0.032
#&gt; GSM892352     3  0.3308      0.808 0.052 0.000 0.860 0.076 0.012
#&gt; GSM892358     1  0.0162      0.940 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892360     1  0.1197      0.942 0.952 0.000 0.000 0.000 0.048
#&gt; GSM892364     1  0.1410      0.927 0.940 0.000 0.000 0.000 0.060
#&gt; GSM892368     3  0.2592      0.835 0.052 0.000 0.892 0.000 0.056
#&gt; GSM892372     3  0.3359      0.820 0.052 0.000 0.840 0.000 0.108
#&gt; GSM892376     2  0.4676      0.180 0.000 0.728 0.020 0.032 0.220
#&gt; GSM892380     2  0.1106      0.613 0.000 0.964 0.000 0.012 0.024
#&gt; GSM892386     2  0.3281      0.565 0.000 0.848 0.000 0.092 0.060
#&gt; GSM892390     2  0.3682      0.543 0.000 0.820 0.000 0.072 0.108
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     3  0.3411      0.762 0.000 0.000 0.824 0.008 0.100 NA
#&gt; GSM892345     4  0.2510      0.905 0.028 0.000 0.100 0.872 0.000 NA
#&gt; GSM892349     3  0.5392      0.700 0.000 0.000 0.684 0.120 0.116 NA
#&gt; GSM892353     1  0.4049      0.802 0.648 0.000 0.020 0.000 0.000 NA
#&gt; GSM892355     1  0.4049      0.802 0.648 0.000 0.020 0.000 0.000 NA
#&gt; GSM892361     1  0.1401      0.858 0.948 0.000 0.020 0.000 0.028 NA
#&gt; GSM892365     3  0.0000      0.776 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM892369     3  0.6128      0.486 0.000 0.000 0.576 0.228 0.064 NA
#&gt; GSM892373     5  0.3717      0.789 0.000 0.384 0.000 0.000 0.616 NA
#&gt; GSM892377     5  0.5699      0.769 0.008 0.436 0.000 0.040 0.472 NA
#&gt; GSM892381     2  0.0790      0.648 0.000 0.968 0.000 0.000 0.000 NA
#&gt; GSM892383     2  0.3092      0.570 0.000 0.836 0.000 0.000 0.060 NA
#&gt; GSM892387     2  0.3934      0.508 0.000 0.796 0.000 0.052 0.116 NA
#&gt; GSM892344     3  0.3503      0.760 0.000 0.000 0.816 0.008 0.108 NA
#&gt; GSM892347     4  0.2510      0.905 0.028 0.000 0.100 0.872 0.000 NA
#&gt; GSM892351     4  0.3252      0.847 0.000 0.000 0.128 0.828 0.032 NA
#&gt; GSM892357     1  0.1401      0.858 0.948 0.000 0.020 0.000 0.028 NA
#&gt; GSM892359     1  0.0692      0.864 0.976 0.000 0.020 0.000 0.004 NA
#&gt; GSM892363     1  0.3043      0.862 0.832 0.000 0.020 0.000 0.008 NA
#&gt; GSM892367     3  0.3150      0.717 0.000 0.000 0.844 0.008 0.060 NA
#&gt; GSM892371     3  0.6128      0.486 0.000 0.000 0.576 0.228 0.064 NA
#&gt; GSM892375     2  0.4826      0.184 0.000 0.676 0.000 0.048 0.244 NA
#&gt; GSM892379     2  0.3465      0.524 0.000 0.820 0.000 0.016 0.120 NA
#&gt; GSM892385     2  0.5109      0.353 0.000 0.572 0.000 0.000 0.100 NA
#&gt; GSM892389     2  0.3526      0.593 0.000 0.828 0.000 0.032 0.092 NA
#&gt; GSM892341     3  0.3411      0.762 0.000 0.000 0.824 0.008 0.100 NA
#&gt; GSM892346     4  0.4462      0.811 0.116 0.000 0.020 0.776 0.040 NA
#&gt; GSM892350     4  0.2764      0.903 0.028 0.000 0.100 0.864 0.008 NA
#&gt; GSM892354     1  0.1515      0.856 0.944 0.000 0.020 0.000 0.028 NA
#&gt; GSM892356     1  0.4034      0.804 0.652 0.000 0.020 0.000 0.000 NA
#&gt; GSM892362     1  0.0692      0.863 0.976 0.000 0.020 0.000 0.004 NA
#&gt; GSM892366     3  0.0000      0.776 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM892370     3  0.6128      0.486 0.000 0.000 0.576 0.228 0.064 NA
#&gt; GSM892374     5  0.3717      0.789 0.000 0.384 0.000 0.000 0.616 NA
#&gt; GSM892378     5  0.5699      0.769 0.008 0.436 0.000 0.040 0.472 NA
#&gt; GSM892382     2  0.0790      0.648 0.000 0.968 0.000 0.000 0.000 NA
#&gt; GSM892384     2  0.1124      0.648 0.000 0.956 0.000 0.000 0.008 NA
#&gt; GSM892388     2  0.5331      0.371 0.004 0.588 0.000 0.008 0.092 NA
#&gt; GSM892343     3  0.4990      0.734 0.000 0.000 0.720 0.096 0.116 NA
#&gt; GSM892348     4  0.4308      0.852 0.088 0.000 0.040 0.796 0.032 NA
#&gt; GSM892352     3  0.4882      0.724 0.000 0.000 0.728 0.068 0.124 NA
#&gt; GSM892358     1  0.2633      0.867 0.864 0.000 0.020 0.000 0.004 NA
#&gt; GSM892360     1  0.1889      0.869 0.920 0.000 0.020 0.000 0.004 NA
#&gt; GSM892364     1  0.4071      0.811 0.672 0.000 0.020 0.000 0.004 NA
#&gt; GSM892368     3  0.0000      0.776 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM892372     3  0.1821      0.757 0.000 0.000 0.928 0.008 0.024 NA
#&gt; GSM892376     2  0.4743      0.190 0.000 0.684 0.000 0.044 0.240 NA
#&gt; GSM892380     2  0.1341      0.634 0.000 0.948 0.000 0.000 0.028 NA
#&gt; GSM892386     2  0.4497      0.560 0.012 0.768 0.000 0.036 0.064 NA
#&gt; GSM892390     2  0.4075      0.560 0.000 0.760 0.000 0.012 0.060 NA
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) disease.state(p) other(p) individual(p) k
#> CV:kmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> CV:kmeans 50     0.776            0.931 1.69e-11      8.93e-05 3
#> CV:kmeans 48     1.000            0.987 4.74e-15      7.28e-06 4
#> CV:kmeans 43     0.999            0.719 1.42e-16      7.38e-07 5
#> CV:kmeans 43     0.999            0.420 5.80e-14      3.43e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           0.966       0.984         0.4404 0.792   0.607
#> 4 4 0.958           0.894       0.949         0.0974 0.899   0.700
#> 5 5 0.971           0.870       0.924         0.0341 0.985   0.942
#> 6 6 0.914           0.812       0.869         0.0239 0.980   0.918
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892345     1   0.529      0.680 0.732  0 0.268
#&gt; GSM892349     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892353     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892355     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892361     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892365     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892369     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892373     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892344     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892347     1   0.529      0.680 0.732  0 0.268
#&gt; GSM892351     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892357     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892359     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892363     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892367     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892371     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892341     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892346     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892350     1   0.529      0.680 0.732  0 0.268
#&gt; GSM892354     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892356     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892362     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892366     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892370     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892374     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892388     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892343     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892348     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892352     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892358     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892360     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892364     1   0.000      0.948 1.000  0 0.000
#&gt; GSM892368     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892372     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.1302      0.886 0.000  0 0.956 0.044
#&gt; GSM892345     4  0.0921      0.780 0.028  0 0.000 0.972
#&gt; GSM892349     3  0.4877      0.365 0.000  0 0.592 0.408
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.889 0.000  0 1.000 0.000
#&gt; GSM892369     4  0.4977      0.363 0.000  0 0.460 0.540
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.1302      0.886 0.000  0 0.956 0.044
#&gt; GSM892347     4  0.0921      0.781 0.028  0 0.000 0.972
#&gt; GSM892351     4  0.0817      0.763 0.000  0 0.024 0.976
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0469      0.883 0.000  0 0.988 0.012
#&gt; GSM892371     4  0.4961      0.385 0.000  0 0.448 0.552
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.1302      0.886 0.000  0 0.956 0.044
#&gt; GSM892346     4  0.1716      0.769 0.064  0 0.000 0.936
#&gt; GSM892350     4  0.0817      0.781 0.024  0 0.000 0.976
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.889 0.000  0 1.000 0.000
#&gt; GSM892370     4  0.4977      0.363 0.000  0 0.460 0.540
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.3024      0.803 0.000  0 0.852 0.148
#&gt; GSM892348     4  0.1637      0.772 0.060  0 0.000 0.940
#&gt; GSM892352     3  0.3837      0.699 0.000  0 0.776 0.224
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.889 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0188      0.888 0.000  0 0.996 0.004
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     5  0.4101      0.653 0.000 0.000 0.372 0.000 0.628
#&gt; GSM892345     4  0.0703      0.971 0.000 0.000 0.024 0.976 0.000
#&gt; GSM892349     5  0.6775      0.426 0.000 0.000 0.348 0.276 0.376
#&gt; GSM892353     1  0.1121      0.973 0.956 0.000 0.044 0.000 0.000
#&gt; GSM892355     1  0.0880      0.981 0.968 0.000 0.032 0.000 0.000
#&gt; GSM892361     1  0.0162      0.988 0.996 0.000 0.004 0.000 0.000
#&gt; GSM892365     5  0.0162      0.482 0.000 0.000 0.004 0.000 0.996
#&gt; GSM892369     3  0.5556      0.984 0.000 0.000 0.524 0.072 0.404
#&gt; GSM892373     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892377     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892381     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.4088      0.653 0.000 0.000 0.368 0.000 0.632
#&gt; GSM892347     4  0.0404      0.974 0.000 0.000 0.012 0.988 0.000
#&gt; GSM892351     4  0.1430      0.943 0.000 0.000 0.052 0.944 0.004
#&gt; GSM892357     1  0.0162      0.988 0.996 0.000 0.004 0.000 0.000
#&gt; GSM892359     1  0.0000      0.989 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0162      0.989 0.996 0.000 0.004 0.000 0.000
#&gt; GSM892367     5  0.3796     -0.378 0.000 0.000 0.300 0.000 0.700
#&gt; GSM892371     3  0.5658      0.984 0.000 0.000 0.512 0.080 0.408
#&gt; GSM892375     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892385     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892341     5  0.4101      0.653 0.000 0.000 0.372 0.000 0.628
#&gt; GSM892346     4  0.1018      0.963 0.016 0.000 0.016 0.968 0.000
#&gt; GSM892350     4  0.0162      0.974 0.000 0.000 0.004 0.996 0.000
#&gt; GSM892354     1  0.0162      0.988 0.996 0.000 0.004 0.000 0.000
#&gt; GSM892356     1  0.0794      0.982 0.972 0.000 0.028 0.000 0.000
#&gt; GSM892362     1  0.0162      0.988 0.996 0.000 0.004 0.000 0.000
#&gt; GSM892366     5  0.0000      0.479 0.000 0.000 0.000 0.000 1.000
#&gt; GSM892370     3  0.5524      0.983 0.000 0.000 0.516 0.068 0.416
#&gt; GSM892374     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892378     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892382     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892343     5  0.5691      0.607 0.000 0.000 0.400 0.084 0.516
#&gt; GSM892348     4  0.0451      0.973 0.004 0.000 0.008 0.988 0.000
#&gt; GSM892352     5  0.5281      0.625 0.000 0.000 0.400 0.052 0.548
#&gt; GSM892358     1  0.0290      0.988 0.992 0.000 0.008 0.000 0.000
#&gt; GSM892360     1  0.0162      0.989 0.996 0.000 0.004 0.000 0.000
#&gt; GSM892364     1  0.0794      0.983 0.972 0.000 0.028 0.000 0.000
#&gt; GSM892368     5  0.0290      0.469 0.000 0.000 0.008 0.000 0.992
#&gt; GSM892372     5  0.2074      0.291 0.000 0.000 0.104 0.000 0.896
#&gt; GSM892376     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892380     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
#&gt; GSM892386     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0162      0.998 0.000 0.996 0.004 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     3  0.3828    0.05925 0.000 0.000 0.560 0.000 0.440 0.000
#&gt; GSM892345     4  0.0858    0.89632 0.000 0.000 0.000 0.968 0.028 0.004
#&gt; GSM892349     5  0.6413    0.49984 0.000 0.000 0.180 0.296 0.484 0.040
#&gt; GSM892353     1  0.3439    0.86303 0.808 0.000 0.000 0.000 0.120 0.072
#&gt; GSM892355     1  0.2554    0.91079 0.876 0.000 0.000 0.000 0.076 0.048
#&gt; GSM892361     1  0.0777    0.94319 0.972 0.000 0.000 0.000 0.024 0.004
#&gt; GSM892365     3  0.0405    0.58541 0.000 0.000 0.988 0.000 0.008 0.004
#&gt; GSM892369     6  0.3933    0.98230 0.000 0.000 0.248 0.036 0.000 0.716
#&gt; GSM892373     2  0.1572    0.96683 0.000 0.936 0.000 0.000 0.036 0.028
#&gt; GSM892377     2  0.1003    0.97371 0.000 0.964 0.000 0.000 0.016 0.020
#&gt; GSM892381     2  0.0725    0.97726 0.000 0.976 0.000 0.000 0.012 0.012
#&gt; GSM892383     2  0.0914    0.97617 0.000 0.968 0.000 0.000 0.016 0.016
#&gt; GSM892387     2  0.0909    0.97790 0.000 0.968 0.000 0.000 0.020 0.012
#&gt; GSM892344     3  0.3854    0.00777 0.000 0.000 0.536 0.000 0.464 0.000
#&gt; GSM892347     4  0.1088    0.89610 0.000 0.000 0.000 0.960 0.024 0.016
#&gt; GSM892351     4  0.2981    0.79421 0.000 0.000 0.000 0.820 0.160 0.020
#&gt; GSM892357     1  0.0777    0.94319 0.972 0.000 0.000 0.000 0.024 0.004
#&gt; GSM892359     1  0.0520    0.94550 0.984 0.000 0.000 0.000 0.008 0.008
#&gt; GSM892363     1  0.1334    0.94027 0.948 0.000 0.000 0.000 0.020 0.032
#&gt; GSM892367     3  0.3371    0.05566 0.000 0.000 0.708 0.000 0.000 0.292
#&gt; GSM892371     6  0.4306    0.97453 0.000 0.000 0.248 0.044 0.008 0.700
#&gt; GSM892375     2  0.1261    0.97399 0.000 0.952 0.000 0.000 0.024 0.024
#&gt; GSM892379     2  0.0725    0.97777 0.000 0.976 0.000 0.000 0.012 0.012
#&gt; GSM892385     2  0.1003    0.97267 0.000 0.964 0.000 0.000 0.016 0.020
#&gt; GSM892389     2  0.0717    0.97828 0.000 0.976 0.000 0.000 0.016 0.008
#&gt; GSM892341     3  0.3810    0.06891 0.000 0.000 0.572 0.000 0.428 0.000
#&gt; GSM892346     4  0.2816    0.83584 0.036 0.000 0.000 0.876 0.028 0.060
#&gt; GSM892350     4  0.1701    0.87652 0.000 0.000 0.000 0.920 0.072 0.008
#&gt; GSM892354     1  0.0806    0.94354 0.972 0.000 0.000 0.000 0.020 0.008
#&gt; GSM892356     1  0.2119    0.92274 0.904 0.000 0.000 0.000 0.060 0.036
#&gt; GSM892362     1  0.0717    0.94442 0.976 0.000 0.000 0.000 0.016 0.008
#&gt; GSM892366     3  0.0260    0.58402 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM892370     6  0.4049    0.97929 0.000 0.000 0.256 0.032 0.004 0.708
#&gt; GSM892374     2  0.1261    0.96853 0.000 0.952 0.000 0.000 0.024 0.024
#&gt; GSM892378     2  0.1003    0.97273 0.000 0.964 0.000 0.000 0.016 0.020
#&gt; GSM892382     2  0.0909    0.97717 0.000 0.968 0.000 0.000 0.020 0.012
#&gt; GSM892384     2  0.0508    0.97857 0.000 0.984 0.000 0.000 0.012 0.004
#&gt; GSM892388     2  0.1088    0.97381 0.000 0.960 0.000 0.000 0.024 0.016
#&gt; GSM892343     5  0.6286    0.47188 0.000 0.000 0.240 0.112 0.560 0.088
#&gt; GSM892348     4  0.1624    0.88005 0.004 0.000 0.000 0.936 0.020 0.040
#&gt; GSM892352     5  0.5575    0.37705 0.000 0.000 0.344 0.080 0.548 0.028
#&gt; GSM892358     1  0.0820    0.94437 0.972 0.000 0.000 0.000 0.016 0.012
#&gt; GSM892360     1  0.0508    0.94503 0.984 0.000 0.000 0.000 0.004 0.012
#&gt; GSM892364     1  0.2905    0.90124 0.852 0.000 0.000 0.000 0.084 0.064
#&gt; GSM892368     3  0.0146    0.58487 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892372     3  0.2446    0.50087 0.000 0.000 0.864 0.000 0.012 0.124
#&gt; GSM892376     2  0.0820    0.97679 0.000 0.972 0.000 0.000 0.012 0.016
#&gt; GSM892380     2  0.0622    0.97831 0.000 0.980 0.000 0.000 0.012 0.008
#&gt; GSM892386     2  0.0725    0.97656 0.000 0.976 0.000 0.000 0.012 0.012
#&gt; GSM892390     2  0.1003    0.97352 0.000 0.964 0.000 0.000 0.016 0.020
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> CV:skmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> CV:skmeans 50     0.939            0.875 8.67e-12      3.79e-05 3
#> CV:skmeans 46     0.957            0.891 2.21e-14      7.92e-06 4
#> CV:skmeans 44     0.970            0.955 1.80e-18      6.18e-07 5
#> CV:skmeans 43     0.860            0.987 2.10e-16      4.86e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.974           0.953       0.976         0.4381 0.792   0.607
#> 4 4 0.975           0.939       0.972         0.1002 0.899   0.700
#> 5 5 0.999           0.966       0.984         0.0286 0.977   0.910
#> 6 6 1.000           0.966       0.981         0.0297 0.953   0.810
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892345     1  0.5760      0.606 0.672  0 0.328
#&gt; GSM892349     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892353     1  0.1643      0.901 0.956  0 0.044
#&gt; GSM892355     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892361     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892365     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892369     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892347     1  0.5431      0.676 0.716  0 0.284
#&gt; GSM892351     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892357     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892367     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892371     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892346     1  0.0424      0.921 0.992  0 0.008
#&gt; GSM892350     1  0.5650      0.634 0.688  0 0.312
#&gt; GSM892354     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892366     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892370     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892348     1  0.4605      0.769 0.796  0 0.204
#&gt; GSM892352     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892358     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.924 1.000  0 0.000
#&gt; GSM892364     1  0.0237      0.923 0.996  0 0.004
#&gt; GSM892368     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892372     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.955 0.000  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.864 0.000  0 0.000 1.000
#&gt; GSM892349     4  0.4543      0.556 0.000  0 0.324 0.676
#&gt; GSM892353     1  0.0188      0.995 0.996  0 0.000 0.004
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.955 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.3074      0.831 0.000  0 0.848 0.152
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0921      0.947 0.000  0 0.972 0.028
#&gt; GSM892347     4  0.0000      0.864 0.000  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.864 0.000  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.955 0.000  0 1.000 0.000
#&gt; GSM892371     4  0.4817      0.327 0.000  0 0.388 0.612
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0817      0.948 0.000  0 0.976 0.024
#&gt; GSM892346     4  0.1022      0.843 0.032  0 0.000 0.968
#&gt; GSM892350     4  0.0000      0.864 0.000  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.955 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.2408      0.887 0.000  0 0.896 0.104
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     4  0.3942      0.683 0.000  0 0.236 0.764
#&gt; GSM892348     4  0.0000      0.864 0.000  0 0.000 1.000
#&gt; GSM892352     3  0.2281      0.890 0.000  0 0.904 0.096
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.955 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.955 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette   p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.0000      0.974 0.00  0 0.000 0.000 1.000
#&gt; GSM892345     4  0.0000      0.897 0.00  0 0.000 1.000 0.000
#&gt; GSM892349     4  0.3661      0.659 0.00  0 0.000 0.724 0.276
#&gt; GSM892353     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892365     5  0.0000      0.974 0.00  0 0.000 0.000 1.000
#&gt; GSM892369     3  0.0000      1.000 0.00  0 1.000 0.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.0000      0.974 0.00  0 0.000 0.000 1.000
#&gt; GSM892347     4  0.0162      0.895 0.00  0 0.004 0.996 0.000
#&gt; GSM892351     4  0.0000      0.897 0.00  0 0.000 1.000 0.000
#&gt; GSM892357     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892367     5  0.1908      0.896 0.00  0 0.092 0.000 0.908
#&gt; GSM892371     3  0.0000      1.000 0.00  0 1.000 0.000 0.000
#&gt; GSM892375     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.0000      0.974 0.00  0 0.000 0.000 1.000
#&gt; GSM892346     4  0.0771      0.880 0.02  0 0.004 0.976 0.000
#&gt; GSM892350     4  0.0000      0.897 0.00  0 0.000 1.000 0.000
#&gt; GSM892354     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892366     5  0.0000      0.974 0.00  0 0.000 0.000 1.000
#&gt; GSM892370     3  0.0000      1.000 0.00  0 1.000 0.000 0.000
#&gt; GSM892374     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892343     4  0.4354      0.668 0.00  0 0.032 0.712 0.256
#&gt; GSM892348     4  0.0000      0.897 0.00  0 0.000 1.000 0.000
#&gt; GSM892352     5  0.1908      0.887 0.00  0 0.000 0.092 0.908
#&gt; GSM892358     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.00  0 0.000 0.000 0.000
#&gt; GSM892368     5  0.0000      0.974 0.00  0 0.000 0.000 1.000
#&gt; GSM892372     5  0.0162      0.972 0.00  0 0.004 0.000 0.996
#&gt; GSM892376     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.00  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.0937      0.870 0.000  0 0.040 0.000 0.960 0.000
#&gt; GSM892345     4  0.0865      0.984 0.000  0 0.000 0.964 0.036 0.000
#&gt; GSM892349     5  0.1633      0.844 0.000  0 0.024 0.044 0.932 0.000
#&gt; GSM892353     5  0.3684      0.509 0.332  0 0.004 0.000 0.664 0.000
#&gt; GSM892355     1  0.0146      0.998 0.996  0 0.004 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.998 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.0146      0.983 0.000  0 0.996 0.000 0.004 0.000
#&gt; GSM892369     6  0.0000      1.000 0.000  0 0.000 0.000 0.000 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.0937      0.870 0.000  0 0.040 0.000 0.960 0.000
#&gt; GSM892347     4  0.0865      0.984 0.000  0 0.000 0.964 0.036 0.000
#&gt; GSM892351     4  0.0865      0.984 0.000  0 0.000 0.964 0.036 0.000
#&gt; GSM892357     1  0.0000      0.998 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0146      0.998 0.996  0 0.004 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.998 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.1327      0.929 0.000  0 0.936 0.000 0.000 0.064
#&gt; GSM892371     6  0.0000      1.000 0.000  0 0.000 0.000 0.000 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.2597      0.777 0.000  0 0.176 0.000 0.824 0.000
#&gt; GSM892346     4  0.0146      0.968 0.000  0 0.000 0.996 0.004 0.000
#&gt; GSM892350     4  0.0865      0.984 0.000  0 0.000 0.964 0.036 0.000
#&gt; GSM892354     1  0.0000      0.998 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.998 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.998 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0146      0.983 0.000  0 0.996 0.000 0.004 0.000
#&gt; GSM892370     6  0.0000      1.000 0.000  0 0.000 0.000 0.000 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892343     5  0.0260      0.869 0.000  0 0.008 0.000 0.992 0.000
#&gt; GSM892348     4  0.0146      0.968 0.000  0 0.000 0.996 0.004 0.000
#&gt; GSM892352     5  0.0146      0.868 0.000  0 0.004 0.000 0.996 0.000
#&gt; GSM892358     1  0.0146      0.998 0.996  0 0.004 0.000 0.000 0.000
#&gt; GSM892360     1  0.0146      0.998 0.996  0 0.004 0.000 0.000 0.000
#&gt; GSM892364     1  0.0146      0.998 0.996  0 0.004 0.000 0.000 0.000
#&gt; GSM892368     3  0.0146      0.983 0.000  0 0.996 0.000 0.004 0.000
#&gt; GSM892372     3  0.0146      0.983 0.000  0 0.996 0.000 0.004 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) disease.state(p) other(p) individual(p) k
#> CV:pam 50     1.000            0.934 2.67e-07      1.42e-03 2
#> CV:pam 50     0.939            0.875 8.67e-12      3.79e-05 3
#> CV:pam 49     0.995            0.988 1.32e-14      7.04e-06 4
#> CV:pam 50     0.979            0.964 3.20e-17      9.29e-07 5
#> CV:pam 50     0.979            0.969 1.52e-20      2.12e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           1.000       1.000         0.4158 0.804   0.630
#> 4 4 0.966           0.920       0.966         0.1189 0.918   0.756
#> 5 5 0.907           0.762       0.901         0.0344 0.987   0.948
#> 6 6 0.855           0.679       0.851         0.0342 0.984   0.932
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM892342     3       0          1  0  0  1
#&gt; GSM892345     3       0          1  0  0  1
#&gt; GSM892349     3       0          1  0  0  1
#&gt; GSM892353     1       0          1  1  0  0
#&gt; GSM892355     1       0          1  1  0  0
#&gt; GSM892361     1       0          1  1  0  0
#&gt; GSM892365     3       0          1  0  0  1
#&gt; GSM892369     3       0          1  0  0  1
#&gt; GSM892373     2       0          1  0  1  0
#&gt; GSM892377     2       0          1  0  1  0
#&gt; GSM892381     2       0          1  0  1  0
#&gt; GSM892383     2       0          1  0  1  0
#&gt; GSM892387     2       0          1  0  1  0
#&gt; GSM892344     3       0          1  0  0  1
#&gt; GSM892347     3       0          1  0  0  1
#&gt; GSM892351     3       0          1  0  0  1
#&gt; GSM892357     1       0          1  1  0  0
#&gt; GSM892359     1       0          1  1  0  0
#&gt; GSM892363     1       0          1  1  0  0
#&gt; GSM892367     3       0          1  0  0  1
#&gt; GSM892371     3       0          1  0  0  1
#&gt; GSM892375     2       0          1  0  1  0
#&gt; GSM892379     2       0          1  0  1  0
#&gt; GSM892385     2       0          1  0  1  0
#&gt; GSM892389     2       0          1  0  1  0
#&gt; GSM892341     3       0          1  0  0  1
#&gt; GSM892346     3       0          1  0  0  1
#&gt; GSM892350     3       0          1  0  0  1
#&gt; GSM892354     1       0          1  1  0  0
#&gt; GSM892356     1       0          1  1  0  0
#&gt; GSM892362     1       0          1  1  0  0
#&gt; GSM892366     3       0          1  0  0  1
#&gt; GSM892370     3       0          1  0  0  1
#&gt; GSM892374     2       0          1  0  1  0
#&gt; GSM892378     2       0          1  0  1  0
#&gt; GSM892382     2       0          1  0  1  0
#&gt; GSM892384     2       0          1  0  1  0
#&gt; GSM892388     2       0          1  0  1  0
#&gt; GSM892343     3       0          1  0  0  1
#&gt; GSM892348     3       0          1  0  0  1
#&gt; GSM892352     3       0          1  0  0  1
#&gt; GSM892358     1       0          1  1  0  0
#&gt; GSM892360     1       0          1  1  0  0
#&gt; GSM892364     1       0          1  1  0  0
#&gt; GSM892368     3       0          1  0  0  1
#&gt; GSM892372     3       0          1  0  0  1
#&gt; GSM892376     2       0          1  0  1  0
#&gt; GSM892380     2       0          1  0  1  0
#&gt; GSM892386     2       0          1  0  1  0
#&gt; GSM892390     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.862  0  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.921  0  0 0.000 1.000
#&gt; GSM892349     4  0.4866      0.143  0  0 0.404 0.596
#&gt; GSM892353     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892365     3  0.0921      0.860  0  0 0.972 0.028
#&gt; GSM892369     4  0.2868      0.786  0  0 0.136 0.864
#&gt; GSM892373     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892344     3  0.0188      0.863  0  0 0.996 0.004
#&gt; GSM892347     4  0.0000      0.921  0  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.921  0  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892367     3  0.2011      0.846  0  0 0.920 0.080
#&gt; GSM892371     4  0.0817      0.909  0  0 0.024 0.976
#&gt; GSM892375     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892341     3  0.0188      0.863  0  0 0.996 0.004
#&gt; GSM892346     4  0.0000      0.921  0  0 0.000 1.000
#&gt; GSM892350     4  0.0000      0.921  0  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892366     3  0.2149      0.837  0  0 0.912 0.088
#&gt; GSM892370     4  0.0707      0.912  0  0 0.020 0.980
#&gt; GSM892374     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892343     3  0.4250      0.630  0  0 0.724 0.276
#&gt; GSM892348     4  0.0000      0.921  0  0 0.000 1.000
#&gt; GSM892352     3  0.4843      0.391  0  0 0.604 0.396
#&gt; GSM892358     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892368     3  0.0188      0.863  0  0 0.996 0.004
#&gt; GSM892372     3  0.3942      0.721  0  0 0.764 0.236
#&gt; GSM892376     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000  0  1 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     3  0.0404      0.862 0.000 0.000 0.988 0.000 0.012
#&gt; GSM892345     4  0.4262     -0.208 0.000 0.000 0.000 0.560 0.440
#&gt; GSM892349     4  0.5703     -0.274 0.000 0.000 0.408 0.508 0.084
#&gt; GSM892353     1  0.0162      0.993 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892355     1  0.0290      0.992 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892361     1  0.0290      0.992 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892365     3  0.0451      0.864 0.000 0.000 0.988 0.004 0.008
#&gt; GSM892369     4  0.0609      0.334 0.000 0.000 0.000 0.980 0.020
#&gt; GSM892373     2  0.2891      0.853 0.000 0.824 0.000 0.000 0.176
#&gt; GSM892377     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892344     3  0.1386      0.862 0.000 0.000 0.952 0.016 0.032
#&gt; GSM892347     4  0.4268     -0.218 0.000 0.000 0.000 0.556 0.444
#&gt; GSM892351     4  0.4262     -0.208 0.000 0.000 0.000 0.560 0.440
#&gt; GSM892357     1  0.0290      0.992 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892359     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.2694      0.818 0.000 0.000 0.884 0.040 0.076
#&gt; GSM892371     4  0.1121      0.333 0.000 0.000 0.000 0.956 0.044
#&gt; GSM892375     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892341     3  0.1281      0.863 0.000 0.000 0.956 0.012 0.032
#&gt; GSM892346     5  0.3452      0.802 0.000 0.000 0.000 0.244 0.756
#&gt; GSM892350     4  0.4273     -0.228 0.000 0.000 0.000 0.552 0.448
#&gt; GSM892354     1  0.0290      0.992 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892356     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0671      0.864 0.000 0.000 0.980 0.004 0.016
#&gt; GSM892370     4  0.0510      0.339 0.000 0.000 0.000 0.984 0.016
#&gt; GSM892374     2  0.2891      0.853 0.000 0.824 0.000 0.000 0.176
#&gt; GSM892378     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.2929      0.850 0.000 0.820 0.000 0.000 0.180
#&gt; GSM892343     3  0.5267      0.448 0.000 0.000 0.524 0.428 0.048
#&gt; GSM892348     5  0.3966      0.780 0.000 0.000 0.000 0.336 0.664
#&gt; GSM892352     3  0.4937      0.464 0.000 0.000 0.544 0.428 0.028
#&gt; GSM892358     1  0.0290      0.992 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892360     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.1216      0.969 0.960 0.000 0.000 0.020 0.020
#&gt; GSM892368     3  0.0566      0.865 0.000 0.000 0.984 0.004 0.012
#&gt; GSM892372     3  0.2438      0.848 0.000 0.000 0.900 0.060 0.040
#&gt; GSM892376     2  0.2127      0.902 0.000 0.892 0.000 0.000 0.108
#&gt; GSM892380     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.966 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.0146     0.8699 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; GSM892345     3  0.3851    -0.0729 0.000 0.000 0.540 0.460 0.000 0.000
#&gt; GSM892349     3  0.4453    -0.2003 0.000 0.000 0.592 0.036 0.372 0.000
#&gt; GSM892353     6  0.5147     0.9293 0.436 0.000 0.000 0.084 0.000 0.480
#&gt; GSM892355     1  0.3766     0.1570 0.736 0.000 0.000 0.032 0.000 0.232
#&gt; GSM892361     1  0.1245     0.8153 0.952 0.000 0.000 0.032 0.000 0.016
#&gt; GSM892365     5  0.0000     0.8698 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892369     3  0.0692     0.4009 0.000 0.000 0.976 0.000 0.004 0.020
#&gt; GSM892373     2  0.3838     0.5373 0.000 0.552 0.000 0.000 0.000 0.448
#&gt; GSM892377     2  0.0146     0.9088 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892381     2  0.0000     0.9089 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0146     0.9088 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892387     2  0.0146     0.9085 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892344     5  0.0405     0.8688 0.000 0.000 0.000 0.004 0.988 0.008
#&gt; GSM892347     3  0.3851    -0.0683 0.000 0.000 0.540 0.460 0.000 0.000
#&gt; GSM892351     3  0.3838    -0.0496 0.000 0.000 0.552 0.448 0.000 0.000
#&gt; GSM892357     1  0.0914     0.8246 0.968 0.000 0.000 0.016 0.000 0.016
#&gt; GSM892359     1  0.1812     0.7342 0.912 0.000 0.000 0.008 0.000 0.080
#&gt; GSM892363     1  0.0146     0.8315 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM892367     5  0.3224     0.8017 0.000 0.000 0.128 0.036 0.828 0.008
#&gt; GSM892371     3  0.0547     0.4032 0.000 0.000 0.980 0.020 0.000 0.000
#&gt; GSM892375     2  0.0260     0.9077 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892379     2  0.0000     0.9089 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0260     0.9076 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892389     2  0.0260     0.9077 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892341     5  0.0146     0.8699 0.000 0.000 0.000 0.004 0.996 0.000
#&gt; GSM892346     4  0.2562     0.8212 0.000 0.000 0.172 0.828 0.000 0.000
#&gt; GSM892350     3  0.3854    -0.0820 0.000 0.000 0.536 0.464 0.000 0.000
#&gt; GSM892354     1  0.1245     0.8153 0.952 0.000 0.000 0.032 0.000 0.016
#&gt; GSM892356     1  0.0405     0.8286 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM892362     1  0.0260     0.8288 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM892366     5  0.0520     0.8684 0.000 0.000 0.000 0.008 0.984 0.008
#&gt; GSM892370     3  0.0622     0.3981 0.000 0.000 0.980 0.012 0.000 0.008
#&gt; GSM892374     2  0.3838     0.5373 0.000 0.552 0.000 0.000 0.000 0.448
#&gt; GSM892378     2  0.0363     0.9065 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; GSM892382     2  0.0146     0.9088 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892384     2  0.0146     0.9088 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892388     2  0.4702     0.4619 0.000 0.496 0.000 0.044 0.000 0.460
#&gt; GSM892343     5  0.5110     0.5940 0.000 0.000 0.316 0.020 0.604 0.060
#&gt; GSM892348     4  0.3221     0.8030 0.000 0.000 0.264 0.736 0.000 0.000
#&gt; GSM892352     5  0.4415     0.5014 0.000 0.000 0.420 0.004 0.556 0.020
#&gt; GSM892358     6  0.5145     0.9299 0.432 0.000 0.000 0.084 0.000 0.484
#&gt; GSM892360     1  0.0146     0.8300 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM892364     1  0.4152     0.1251 0.664 0.000 0.000 0.032 0.000 0.304
#&gt; GSM892368     5  0.0291     0.8698 0.000 0.000 0.004 0.000 0.992 0.004
#&gt; GSM892372     5  0.3043     0.8101 0.000 0.000 0.140 0.008 0.832 0.020
#&gt; GSM892376     2  0.2730     0.7852 0.000 0.808 0.000 0.000 0.000 0.192
#&gt; GSM892380     2  0.0146     0.9088 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892386     2  0.0146     0.9088 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892390     2  0.0547     0.9031 0.000 0.980 0.000 0.000 0.000 0.020
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) disease.state(p) other(p) individual(p) k
#> CV:mclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> CV:mclust 50     1.000            0.931 1.26e-12      1.60e-05 3
#> CV:mclust 48     0.974            0.948 1.91e-15      3.99e-06 4
#> CV:mclust 40     0.592            0.989 1.08e-13      2.01e-05 5
#> CV:mclust 39     0.691            0.980 4.22e-10      1.80e-03 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.949           0.933       0.969         0.4361 0.792   0.607
#> 4 4 0.985           0.921       0.966         0.0935 0.922   0.762
#> 5 5 0.939           0.915       0.934         0.0389 0.958   0.842
#> 6 6 0.902           0.914       0.934         0.0198 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892345     1  0.5968      0.524 0.636  0 0.364
#&gt; GSM892349     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892353     1  0.0892      0.902 0.980  0 0.020
#&gt; GSM892355     1  0.0424      0.907 0.992  0 0.008
#&gt; GSM892361     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892369     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892347     1  0.5882      0.553 0.652  0 0.348
#&gt; GSM892351     3  0.4346      0.738 0.184  0 0.816
#&gt; GSM892357     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892371     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892346     1  0.2261      0.872 0.932  0 0.068
#&gt; GSM892350     1  0.5968      0.524 0.636  0 0.364
#&gt; GSM892354     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892370     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.0747      0.968 0.016  0 0.984
#&gt; GSM892348     1  0.4399      0.770 0.812  0 0.188
#&gt; GSM892352     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892358     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892364     1  0.0000      0.910 1.000  0 0.000
#&gt; GSM892368     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.984 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0592     0.9036 0.000  0 0.984 0.016
#&gt; GSM892345     4  0.0817     0.9037 0.000  0 0.024 0.976
#&gt; GSM892349     4  0.4933     0.0899 0.000  0 0.432 0.568
#&gt; GSM892353     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892355     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0188     0.9959 0.996  0 0.000 0.004
#&gt; GSM892365     3  0.0000     0.8998 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.1637     0.8867 0.000  0 0.940 0.060
#&gt; GSM892373     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0921     0.9019 0.000  0 0.972 0.028
#&gt; GSM892347     4  0.0895     0.9037 0.004  0 0.020 0.976
#&gt; GSM892351     4  0.1211     0.8938 0.000  0 0.040 0.960
#&gt; GSM892357     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0188     0.8977 0.000  0 0.996 0.004
#&gt; GSM892371     3  0.3074     0.8112 0.000  0 0.848 0.152
#&gt; GSM892375     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0592     0.9036 0.000  0 0.984 0.016
#&gt; GSM892346     4  0.0804     0.8939 0.012  0 0.008 0.980
#&gt; GSM892350     4  0.0817     0.9037 0.000  0 0.024 0.976
#&gt; GSM892354     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0336     0.9025 0.000  0 0.992 0.008
#&gt; GSM892370     3  0.1792     0.8835 0.000  0 0.932 0.068
#&gt; GSM892374     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.4164     0.6520 0.000  0 0.736 0.264
#&gt; GSM892348     4  0.0804     0.9001 0.008  0 0.012 0.980
#&gt; GSM892352     3  0.4981     0.1314 0.000  0 0.536 0.464
#&gt; GSM892358     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000     0.9989 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0336     0.9923 0.992  0 0.000 0.008
#&gt; GSM892368     3  0.0188     0.8977 0.000  0 0.996 0.004
#&gt; GSM892372     3  0.0592     0.9035 0.000  0 0.984 0.016
#&gt; GSM892376     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000     1.0000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000     1.0000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     5  0.2732      0.851 0.000 0.000 0.160 0.000 0.840
#&gt; GSM892345     4  0.2020      0.917 0.000 0.000 0.000 0.900 0.100
#&gt; GSM892349     5  0.3764      0.783 0.000 0.000 0.044 0.156 0.800
#&gt; GSM892353     1  0.0404      0.988 0.988 0.000 0.000 0.000 0.012
#&gt; GSM892355     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.4060      0.621 0.000 0.000 0.640 0.000 0.360
#&gt; GSM892369     3  0.0510      0.729 0.000 0.000 0.984 0.016 0.000
#&gt; GSM892373     2  0.1270      0.960 0.000 0.948 0.000 0.000 0.052
#&gt; GSM892377     2  0.0510      0.986 0.000 0.984 0.000 0.000 0.016
#&gt; GSM892381     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0290      0.990 0.000 0.992 0.000 0.000 0.008
#&gt; GSM892387     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.2583      0.881 0.000 0.000 0.132 0.004 0.864
#&gt; GSM892347     4  0.1544      0.920 0.000 0.000 0.000 0.932 0.068
#&gt; GSM892351     4  0.3336      0.794 0.000 0.000 0.000 0.772 0.228
#&gt; GSM892357     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.1544      0.764 0.000 0.000 0.932 0.000 0.068
#&gt; GSM892371     3  0.2676      0.761 0.000 0.000 0.884 0.036 0.080
#&gt; GSM892375     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0290      0.990 0.000 0.992 0.000 0.000 0.008
#&gt; GSM892389     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.2561      0.873 0.000 0.000 0.144 0.000 0.856
#&gt; GSM892346     4  0.0290      0.891 0.000 0.000 0.008 0.992 0.000
#&gt; GSM892350     4  0.2230      0.910 0.000 0.000 0.000 0.884 0.116
#&gt; GSM892354     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.4219      0.506 0.000 0.000 0.584 0.000 0.416
#&gt; GSM892370     3  0.1012      0.746 0.000 0.000 0.968 0.012 0.020
#&gt; GSM892374     2  0.1670      0.951 0.000 0.936 0.012 0.000 0.052
#&gt; GSM892378     2  0.0510      0.986 0.000 0.984 0.000 0.000 0.016
#&gt; GSM892382     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0162      0.991 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892343     5  0.3037      0.883 0.000 0.000 0.100 0.040 0.860
#&gt; GSM892348     4  0.0404      0.902 0.000 0.000 0.000 0.988 0.012
#&gt; GSM892352     5  0.3159      0.849 0.000 0.000 0.056 0.088 0.856
#&gt; GSM892358     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.3752      0.707 0.000 0.000 0.708 0.000 0.292
#&gt; GSM892372     3  0.3508      0.735 0.000 0.000 0.748 0.000 0.252
#&gt; GSM892376     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      0.992 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0162      0.990 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892390     2  0.0290      0.990 0.000 0.992 0.000 0.000 0.008
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     5  0.1714      0.835 0.000 0.000 0.092 0.000 0.908 NA
#&gt; GSM892345     4  0.2163      0.898 0.000 0.000 0.000 0.892 0.092 NA
#&gt; GSM892349     5  0.3736      0.626 0.000 0.000 0.008 0.268 0.716 NA
#&gt; GSM892353     1  0.0000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892355     1  0.0146      0.996 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892361     1  0.0000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892365     3  0.2454      0.876 0.000 0.000 0.840 0.000 0.160 NA
#&gt; GSM892369     3  0.0777      0.888 0.000 0.000 0.972 0.000 0.004 NA
#&gt; GSM892373     2  0.2854      0.844 0.000 0.792 0.000 0.000 0.000 NA
#&gt; GSM892377     2  0.2527      0.882 0.000 0.832 0.000 0.000 0.000 NA
#&gt; GSM892381     2  0.0000      0.951 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM892383     2  0.0937      0.944 0.000 0.960 0.000 0.000 0.000 NA
#&gt; GSM892387     2  0.0363      0.951 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892344     5  0.1633      0.864 0.000 0.000 0.044 0.024 0.932 NA
#&gt; GSM892347     4  0.1584      0.898 0.000 0.000 0.000 0.928 0.064 NA
#&gt; GSM892351     4  0.3023      0.778 0.000 0.000 0.000 0.784 0.212 NA
#&gt; GSM892357     1  0.0000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892359     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892363     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892367     3  0.0972      0.908 0.000 0.000 0.964 0.000 0.028 NA
#&gt; GSM892371     3  0.0964      0.905 0.000 0.000 0.968 0.012 0.016 NA
#&gt; GSM892375     2  0.1007      0.944 0.000 0.956 0.000 0.000 0.000 NA
#&gt; GSM892379     2  0.0865      0.948 0.000 0.964 0.000 0.000 0.000 NA
#&gt; GSM892385     2  0.1007      0.944 0.000 0.956 0.000 0.000 0.000 NA
#&gt; GSM892389     2  0.0363      0.952 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892341     5  0.1387      0.855 0.000 0.000 0.068 0.000 0.932 NA
#&gt; GSM892346     4  0.2151      0.812 0.000 0.000 0.008 0.904 0.016 NA
#&gt; GSM892350     4  0.2048      0.883 0.000 0.000 0.000 0.880 0.120 NA
#&gt; GSM892354     1  0.0146      0.996 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892356     1  0.0000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892362     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892366     3  0.3023      0.800 0.000 0.000 0.768 0.000 0.232 NA
#&gt; GSM892370     3  0.0520      0.902 0.000 0.000 0.984 0.000 0.008 NA
#&gt; GSM892374     2  0.3126      0.808 0.000 0.752 0.000 0.000 0.000 NA
#&gt; GSM892378     2  0.2527      0.882 0.000 0.832 0.000 0.000 0.000 NA
#&gt; GSM892382     2  0.0260      0.950 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892384     2  0.0260      0.950 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892388     2  0.0363      0.951 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892343     5  0.2492      0.856 0.000 0.000 0.036 0.068 0.888 NA
#&gt; GSM892348     4  0.1245      0.882 0.000 0.000 0.000 0.952 0.032 NA
#&gt; GSM892352     5  0.2994      0.781 0.000 0.000 0.008 0.164 0.820 NA
#&gt; GSM892358     1  0.0000      0.997 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892360     1  0.0146      0.997 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892364     1  0.0458      0.989 0.984 0.000 0.000 0.000 0.000 NA
#&gt; GSM892368     3  0.2212      0.900 0.000 0.000 0.880 0.000 0.112 NA
#&gt; GSM892372     3  0.2003      0.901 0.000 0.000 0.884 0.000 0.116 NA
#&gt; GSM892376     2  0.0790      0.948 0.000 0.968 0.000 0.000 0.000 NA
#&gt; GSM892380     2  0.0260      0.951 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892386     2  0.0547      0.951 0.000 0.980 0.000 0.000 0.000 NA
#&gt; GSM892390     2  0.0260      0.950 0.000 0.992 0.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) disease.state(p) other(p) individual(p) k
#> CV:NMF 50     1.000            0.934 2.67e-07      1.42e-03 2
#> CV:NMF 50     0.939            0.875 8.67e-12      3.79e-05 3
#> CV:NMF 48     1.000            0.987 7.68e-17      1.01e-06 4
#> CV:NMF 50     1.000            0.998 3.06e-20      2.01e-07 5
#> CV:NMF 50     1.000            0.998 3.06e-20      2.01e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           0.962       0.981         0.4312 0.794   0.612
#> 4 4 1.000           0.956       0.976         0.0916 0.941   0.819
#> 5 5 0.961           0.914       0.950         0.0457 0.963   0.862
#> 6 6 0.936           0.787       0.871         0.0227 0.993   0.971
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892345     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892349     3  0.4002      0.828 0.160  0 0.840
#&gt; GSM892353     1  0.0424      0.974 0.992  0 0.008
#&gt; GSM892355     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892361     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892369     3  0.2356      0.922 0.072  0 0.928
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892347     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892351     1  0.5560      0.549 0.700  0 0.300
#&gt; GSM892357     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892371     3  0.2356      0.922 0.072  0 0.928
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892346     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892350     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892354     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892370     3  0.2356      0.922 0.072  0 0.928
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.5016      0.727 0.240  0 0.760
#&gt; GSM892348     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892352     3  0.0424      0.949 0.008  0 0.992
#&gt; GSM892358     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.981 1.000  0 0.000
#&gt; GSM892364     1  0.0237      0.977 0.996  0 0.004
#&gt; GSM892368     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.951 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0336      0.946 0.000  0 0.992 0.008
#&gt; GSM892345     4  0.0817      0.932 0.024  0 0.000 0.976
#&gt; GSM892349     3  0.3266      0.815 0.000  0 0.832 0.168
#&gt; GSM892353     1  0.0469      0.983 0.988  0 0.000 0.012
#&gt; GSM892355     1  0.0188      0.987 0.996  0 0.000 0.004
#&gt; GSM892361     1  0.0817      0.980 0.976  0 0.000 0.024
#&gt; GSM892365     3  0.0000      0.948 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.1867      0.917 0.000  0 0.928 0.072
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0336      0.946 0.000  0 0.992 0.008
#&gt; GSM892347     4  0.0817      0.932 0.024  0 0.000 0.976
#&gt; GSM892351     4  0.4908      0.556 0.016  0 0.292 0.692
#&gt; GSM892357     1  0.0707      0.982 0.980  0 0.000 0.020
#&gt; GSM892359     1  0.0000      0.988 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      0.988 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.948 0.000  0 1.000 0.000
#&gt; GSM892371     3  0.1867      0.917 0.000  0 0.928 0.072
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0336      0.946 0.000  0 0.992 0.008
#&gt; GSM892346     4  0.0469      0.922 0.012  0 0.000 0.988
#&gt; GSM892350     4  0.0817      0.932 0.024  0 0.000 0.976
#&gt; GSM892354     1  0.0817      0.980 0.976  0 0.000 0.024
#&gt; GSM892356     1  0.0188      0.987 0.996  0 0.000 0.004
#&gt; GSM892362     1  0.0336      0.986 0.992  0 0.000 0.008
#&gt; GSM892366     3  0.0000      0.948 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.1867      0.917 0.000  0 0.928 0.072
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.4538      0.733 0.024  0 0.760 0.216
#&gt; GSM892348     4  0.0921      0.931 0.028  0 0.000 0.972
#&gt; GSM892352     3  0.0592      0.944 0.000  0 0.984 0.016
#&gt; GSM892358     1  0.0000      0.988 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      0.988 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.1398      0.954 0.956  0 0.004 0.040
#&gt; GSM892368     3  0.0000      0.948 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.948 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.3876      0.775 0.000  0 0.316 0.000 0.684
#&gt; GSM892345     4  0.0000      0.925 0.000  0 0.000 1.000 0.000
#&gt; GSM892349     5  0.3197      0.567 0.000  0 0.024 0.140 0.836
#&gt; GSM892353     1  0.0404      0.984 0.988  0 0.000 0.000 0.012
#&gt; GSM892355     1  0.0162      0.988 0.996  0 0.000 0.000 0.004
#&gt; GSM892361     1  0.0703      0.981 0.976  0 0.000 0.000 0.024
#&gt; GSM892365     3  0.0162      0.870 0.000  0 0.996 0.000 0.004
#&gt; GSM892369     3  0.2659      0.838 0.000  0 0.888 0.060 0.052
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.3876      0.775 0.000  0 0.316 0.000 0.684
#&gt; GSM892347     4  0.0000      0.925 0.000  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.3895      0.619 0.000  0 0.000 0.680 0.320
#&gt; GSM892357     1  0.0609      0.983 0.980  0 0.000 0.000 0.020
#&gt; GSM892359     1  0.0000      0.988 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.988 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.0162      0.870 0.000  0 0.996 0.000 0.004
#&gt; GSM892371     3  0.2659      0.838 0.000  0 0.888 0.060 0.052
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.3876      0.775 0.000  0 0.316 0.000 0.684
#&gt; GSM892346     4  0.1124      0.910 0.004  0 0.000 0.960 0.036
#&gt; GSM892350     4  0.0404      0.920 0.000  0 0.000 0.988 0.012
#&gt; GSM892354     1  0.0703      0.981 0.976  0 0.000 0.000 0.024
#&gt; GSM892356     1  0.0162      0.988 0.996  0 0.000 0.000 0.004
#&gt; GSM892362     1  0.0290      0.987 0.992  0 0.000 0.000 0.008
#&gt; GSM892366     3  0.0162      0.870 0.000  0 0.996 0.000 0.004
#&gt; GSM892370     3  0.2659      0.838 0.000  0 0.888 0.060 0.052
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     3  0.6855      0.211 0.024  0 0.512 0.192 0.272
#&gt; GSM892348     4  0.0566      0.921 0.004  0 0.000 0.984 0.012
#&gt; GSM892352     5  0.1270      0.704 0.000  0 0.052 0.000 0.948
#&gt; GSM892358     1  0.0000      0.988 1.000  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.988 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.1300      0.957 0.956  0 0.000 0.028 0.016
#&gt; GSM892368     3  0.0162      0.870 0.000  0 0.996 0.000 0.004
#&gt; GSM892372     3  0.0162      0.870 0.000  0 0.996 0.000 0.004
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.0000    0.78930 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892345     4  0.3695   -0.14715 0.000  0 0.000 0.624 0.000 0.376
#&gt; GSM892349     5  0.4289    0.56648 0.000  0 0.012 0.444 0.540 0.004
#&gt; GSM892353     1  0.2389    0.91090 0.864  0 0.000 0.000 0.008 0.128
#&gt; GSM892355     1  0.1501    0.93998 0.924  0 0.000 0.000 0.000 0.076
#&gt; GSM892361     1  0.0937    0.94661 0.960  0 0.000 0.000 0.000 0.040
#&gt; GSM892365     3  0.5839    0.65430 0.000  0 0.488 0.000 0.236 0.276
#&gt; GSM892369     3  0.0458    0.55654 0.000  0 0.984 0.016 0.000 0.000
#&gt; GSM892373     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.0000    0.78930 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892347     4  0.3765   -0.31004 0.000  0 0.000 0.596 0.000 0.404
#&gt; GSM892351     4  0.0363    0.14729 0.000  0 0.012 0.988 0.000 0.000
#&gt; GSM892357     1  0.0790    0.94925 0.968  0 0.000 0.000 0.000 0.032
#&gt; GSM892359     1  0.0458    0.95329 0.984  0 0.000 0.000 0.000 0.016
#&gt; GSM892363     1  0.1765    0.93077 0.904  0 0.000 0.000 0.000 0.096
#&gt; GSM892367     3  0.5839    0.65430 0.000  0 0.488 0.000 0.236 0.276
#&gt; GSM892371     3  0.0458    0.55654 0.000  0 0.984 0.016 0.000 0.000
#&gt; GSM892375     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.0000    0.78930 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892346     6  0.3975    0.86878 0.004  0 0.000 0.452 0.000 0.544
#&gt; GSM892350     4  0.3802    0.11642 0.000  0 0.012 0.676 0.000 0.312
#&gt; GSM892354     1  0.0937    0.94661 0.960  0 0.000 0.000 0.000 0.040
#&gt; GSM892356     1  0.0790    0.95114 0.968  0 0.000 0.000 0.000 0.032
#&gt; GSM892362     1  0.0547    0.95149 0.980  0 0.000 0.000 0.000 0.020
#&gt; GSM892366     3  0.5839    0.65430 0.000  0 0.488 0.000 0.236 0.276
#&gt; GSM892370     3  0.0458    0.55654 0.000  0 0.984 0.016 0.000 0.000
#&gt; GSM892374     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892343     3  0.6268   -0.00826 0.016  0 0.584 0.156 0.204 0.040
#&gt; GSM892348     6  0.3995    0.86043 0.004  0 0.000 0.480 0.000 0.516
#&gt; GSM892352     5  0.3446    0.68491 0.000  0 0.000 0.308 0.692 0.000
#&gt; GSM892358     1  0.0146    0.95344 0.996  0 0.000 0.000 0.000 0.004
#&gt; GSM892360     1  0.0458    0.95329 0.984  0 0.000 0.000 0.000 0.016
#&gt; GSM892364     1  0.2664    0.89894 0.848  0 0.000 0.016 0.000 0.136
#&gt; GSM892368     3  0.5839    0.65430 0.000  0 0.488 0.000 0.236 0.276
#&gt; GSM892372     3  0.5839    0.65430 0.000  0 0.488 0.000 0.236 0.276
#&gt; GSM892376     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000    1.00000 0.000  1 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> MAD:hclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> MAD:hclust 50     1.000            0.931 1.69e-11      8.93e-05 3
#> MAD:hclust 50     1.000            0.986 8.26e-16      4.62e-06 4
#> MAD:hclust 49     0.996            0.993 2.50e-19      7.39e-07 5
#> MAD:hclust 45     0.703            0.994 1.15e-19      4.58e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.699           0.874       0.856         0.3384 0.794   0.612
#> 4 4 0.635           0.881       0.831         0.1311 0.912   0.736
#> 5 5 0.693           0.756       0.820         0.0692 0.974   0.898
#> 6 6 0.751           0.699       0.804         0.0509 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM892342     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892345     3  0.5859      0.202 0.344 0.000 0.656
#&gt; GSM892349     3  0.0424      0.877 0.008 0.000 0.992
#&gt; GSM892353     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892355     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892361     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892365     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892369     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892373     2  0.4750      0.897 0.216 0.784 0.000
#&gt; GSM892377     2  0.3879      0.933 0.152 0.848 0.000
#&gt; GSM892381     2  0.0000      0.949 0.000 1.000 0.000
#&gt; GSM892383     2  0.1289      0.948 0.032 0.968 0.000
#&gt; GSM892387     2  0.0747      0.949 0.016 0.984 0.000
#&gt; GSM892344     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892347     3  0.5859      0.202 0.344 0.000 0.656
#&gt; GSM892351     3  0.5178      0.479 0.256 0.000 0.744
#&gt; GSM892357     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892359     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892363     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892367     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892371     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892375     2  0.2165      0.941 0.064 0.936 0.000
#&gt; GSM892379     2  0.2878      0.946 0.096 0.904 0.000
#&gt; GSM892385     2  0.3267      0.938 0.116 0.884 0.000
#&gt; GSM892389     2  0.2878      0.943 0.096 0.904 0.000
#&gt; GSM892341     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892346     1  0.5497      0.969 0.708 0.000 0.292
#&gt; GSM892350     3  0.5859      0.202 0.344 0.000 0.656
#&gt; GSM892354     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892356     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892362     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892366     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892370     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892374     2  0.4750      0.897 0.216 0.784 0.000
#&gt; GSM892378     2  0.3879      0.933 0.152 0.848 0.000
#&gt; GSM892382     2  0.0000      0.949 0.000 1.000 0.000
#&gt; GSM892384     2  0.0000      0.949 0.000 1.000 0.000
#&gt; GSM892388     2  0.4235      0.908 0.176 0.824 0.000
#&gt; GSM892343     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892348     1  0.6235      0.683 0.564 0.000 0.436
#&gt; GSM892352     3  0.0424      0.877 0.008 0.000 0.992
#&gt; GSM892358     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892360     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892364     1  0.5560      0.980 0.700 0.000 0.300
#&gt; GSM892368     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892372     3  0.0000      0.884 0.000 0.000 1.000
#&gt; GSM892376     2  0.2066      0.942 0.060 0.940 0.000
#&gt; GSM892380     2  0.1163      0.949 0.028 0.972 0.000
#&gt; GSM892386     2  0.0424      0.948 0.008 0.992 0.000
#&gt; GSM892390     2  0.2625      0.944 0.084 0.916 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892345     4  0.7457      0.786 0.276 0.000 0.220 0.504
#&gt; GSM892349     4  0.7026      0.475 0.120 0.000 0.404 0.476
#&gt; GSM892353     1  0.0707      0.979 0.980 0.000 0.000 0.020
#&gt; GSM892355     1  0.0707      0.979 0.980 0.000 0.000 0.020
#&gt; GSM892361     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892369     3  0.3907      0.943 0.120 0.000 0.836 0.044
#&gt; GSM892373     2  0.4907      0.756 0.000 0.580 0.000 0.420
#&gt; GSM892377     2  0.4459      0.856 0.000 0.780 0.032 0.188
#&gt; GSM892381     2  0.1833      0.892 0.000 0.944 0.032 0.024
#&gt; GSM892383     2  0.1406      0.893 0.000 0.960 0.024 0.016
#&gt; GSM892387     2  0.2797      0.891 0.000 0.900 0.032 0.068
#&gt; GSM892344     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892347     4  0.7457      0.786 0.276 0.000 0.220 0.504
#&gt; GSM892351     4  0.7478      0.753 0.240 0.000 0.256 0.504
#&gt; GSM892357     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892371     3  0.3907      0.943 0.120 0.000 0.836 0.044
#&gt; GSM892375     2  0.4290      0.865 0.000 0.800 0.036 0.164
#&gt; GSM892379     2  0.2699      0.889 0.000 0.904 0.028 0.068
#&gt; GSM892385     2  0.3764      0.872 0.000 0.852 0.076 0.072
#&gt; GSM892389     2  0.2670      0.889 0.000 0.904 0.024 0.072
#&gt; GSM892341     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892346     4  0.5000      0.386 0.496 0.000 0.000 0.504
#&gt; GSM892350     4  0.7457      0.786 0.276 0.000 0.220 0.504
#&gt; GSM892354     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892370     3  0.3907      0.943 0.120 0.000 0.836 0.044
#&gt; GSM892374     2  0.4907      0.756 0.000 0.580 0.000 0.420
#&gt; GSM892378     2  0.4459      0.856 0.000 0.780 0.032 0.188
#&gt; GSM892382     2  0.1724      0.892 0.000 0.948 0.032 0.020
#&gt; GSM892384     2  0.1724      0.892 0.000 0.948 0.032 0.020
#&gt; GSM892388     2  0.5358      0.808 0.000 0.700 0.048 0.252
#&gt; GSM892343     3  0.4227      0.921 0.120 0.000 0.820 0.060
#&gt; GSM892348     4  0.6672      0.626 0.408 0.000 0.088 0.504
#&gt; GSM892352     4  0.7026      0.475 0.120 0.000 0.404 0.476
#&gt; GSM892358     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.996 1.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892372     3  0.2647      0.972 0.120 0.000 0.880 0.000
#&gt; GSM892376     2  0.4197      0.867 0.000 0.808 0.036 0.156
#&gt; GSM892380     2  0.1042      0.894 0.000 0.972 0.008 0.020
#&gt; GSM892386     2  0.1356      0.894 0.000 0.960 0.032 0.008
#&gt; GSM892390     2  0.2521      0.889 0.000 0.912 0.024 0.064
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     3  0.3950      0.838 0.068 0.000 0.796 0.000 0.136
#&gt; GSM892345     4  0.4428      0.856 0.144 0.000 0.096 0.760 0.000
#&gt; GSM892349     4  0.7026      0.610 0.064 0.000 0.212 0.556 0.168
#&gt; GSM892353     1  0.2574      0.919 0.876 0.000 0.000 0.012 0.112
#&gt; GSM892355     1  0.2574      0.919 0.876 0.000 0.000 0.012 0.112
#&gt; GSM892361     1  0.1608      0.938 0.928 0.000 0.000 0.000 0.072
#&gt; GSM892365     3  0.1704      0.886 0.068 0.000 0.928 0.000 0.004
#&gt; GSM892369     3  0.4465      0.852 0.068 0.000 0.800 0.056 0.076
#&gt; GSM892373     5  0.4256      1.000 0.000 0.436 0.000 0.000 0.564
#&gt; GSM892377     2  0.4971      0.397 0.000 0.708 0.000 0.116 0.176
#&gt; GSM892381     2  0.1934      0.663 0.000 0.932 0.008 0.040 0.020
#&gt; GSM892383     2  0.1990      0.664 0.000 0.920 0.008 0.068 0.004
#&gt; GSM892387     2  0.3666      0.580 0.000 0.840 0.020 0.048 0.092
#&gt; GSM892344     3  0.4036      0.836 0.068 0.000 0.788 0.000 0.144
#&gt; GSM892347     4  0.4428      0.856 0.144 0.000 0.096 0.760 0.000
#&gt; GSM892351     4  0.5029      0.842 0.128 0.000 0.112 0.740 0.020
#&gt; GSM892357     1  0.1544      0.940 0.932 0.000 0.000 0.000 0.068
#&gt; GSM892359     1  0.0290      0.950 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892363     1  0.0609      0.951 0.980 0.000 0.000 0.000 0.020
#&gt; GSM892367     3  0.2645      0.883 0.068 0.000 0.888 0.000 0.044
#&gt; GSM892371     3  0.4465      0.852 0.068 0.000 0.800 0.056 0.076
#&gt; GSM892375     2  0.4719      0.345 0.000 0.732 0.012 0.052 0.204
#&gt; GSM892379     2  0.3970      0.614 0.000 0.820 0.016 0.080 0.084
#&gt; GSM892385     2  0.4510      0.581 0.000 0.788 0.032 0.112 0.068
#&gt; GSM892389     2  0.3800      0.621 0.000 0.836 0.028 0.052 0.084
#&gt; GSM892341     3  0.3950      0.838 0.068 0.000 0.796 0.000 0.136
#&gt; GSM892346     4  0.4223      0.739 0.248 0.000 0.000 0.724 0.028
#&gt; GSM892350     4  0.4428      0.856 0.144 0.000 0.096 0.760 0.000
#&gt; GSM892354     1  0.1544      0.940 0.932 0.000 0.000 0.000 0.068
#&gt; GSM892356     1  0.1768      0.940 0.924 0.000 0.000 0.004 0.072
#&gt; GSM892362     1  0.0609      0.948 0.980 0.000 0.000 0.000 0.020
#&gt; GSM892366     3  0.1704      0.886 0.068 0.000 0.928 0.000 0.004
#&gt; GSM892370     3  0.4465      0.852 0.068 0.000 0.800 0.056 0.076
#&gt; GSM892374     5  0.4256      1.000 0.000 0.436 0.000 0.000 0.564
#&gt; GSM892378     2  0.4971      0.397 0.000 0.708 0.000 0.116 0.176
#&gt; GSM892382     2  0.1710      0.664 0.000 0.940 0.004 0.040 0.016
#&gt; GSM892384     2  0.1787      0.662 0.000 0.936 0.004 0.044 0.016
#&gt; GSM892388     2  0.5841     -0.409 0.000 0.560 0.032 0.044 0.364
#&gt; GSM892343     3  0.6278      0.721 0.068 0.000 0.652 0.124 0.156
#&gt; GSM892348     4  0.4285      0.808 0.208 0.000 0.032 0.752 0.008
#&gt; GSM892352     4  0.7095      0.594 0.064 0.000 0.224 0.544 0.168
#&gt; GSM892358     1  0.0671      0.951 0.980 0.000 0.000 0.004 0.016
#&gt; GSM892360     1  0.0290      0.950 0.992 0.000 0.000 0.000 0.008
#&gt; GSM892364     1  0.1410      0.941 0.940 0.000 0.000 0.000 0.060
#&gt; GSM892368     3  0.1704      0.886 0.068 0.000 0.928 0.000 0.004
#&gt; GSM892372     3  0.2491      0.884 0.068 0.000 0.896 0.000 0.036
#&gt; GSM892376     2  0.4574      0.383 0.000 0.748 0.008 0.060 0.184
#&gt; GSM892380     2  0.1597      0.674 0.000 0.940 0.000 0.048 0.012
#&gt; GSM892386     2  0.1843      0.672 0.000 0.932 0.008 0.052 0.008
#&gt; GSM892390     2  0.3656      0.623 0.000 0.844 0.024 0.052 0.080
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     3  0.4002      0.727 0.020 0.000 0.660 0.000 0.000 NA
#&gt; GSM892345     4  0.2647      0.848 0.088 0.000 0.044 0.868 0.000 NA
#&gt; GSM892349     4  0.6375      0.530 0.020 0.000 0.132 0.528 0.028 NA
#&gt; GSM892353     1  0.3885      0.829 0.756 0.000 0.000 0.004 0.048 NA
#&gt; GSM892355     1  0.3791      0.834 0.768 0.000 0.000 0.004 0.048 NA
#&gt; GSM892361     1  0.2390      0.902 0.888 0.000 0.000 0.000 0.056 NA
#&gt; GSM892365     3  0.1408      0.828 0.020 0.000 0.944 0.000 0.000 NA
#&gt; GSM892369     3  0.3921      0.790 0.020 0.000 0.816 0.040 0.092 NA
#&gt; GSM892373     5  0.3464      1.000 0.000 0.312 0.000 0.000 0.688 NA
#&gt; GSM892377     2  0.6405      0.121 0.000 0.504 0.008 0.032 0.296 NA
#&gt; GSM892381     2  0.1053      0.619 0.000 0.964 0.000 0.004 0.012 NA
#&gt; GSM892383     2  0.2923      0.613 0.000 0.856 0.008 0.004 0.024 NA
#&gt; GSM892387     2  0.2854      0.573 0.000 0.872 0.000 0.024 0.068 NA
#&gt; GSM892344     3  0.4064      0.722 0.020 0.000 0.644 0.000 0.000 NA
#&gt; GSM892347     4  0.2647      0.848 0.088 0.000 0.044 0.868 0.000 NA
#&gt; GSM892351     4  0.3818      0.830 0.072 0.000 0.060 0.824 0.028 NA
#&gt; GSM892357     1  0.2390      0.904 0.888 0.000 0.000 0.000 0.056 NA
#&gt; GSM892359     1  0.0603      0.918 0.980 0.000 0.000 0.000 0.016 NA
#&gt; GSM892363     1  0.1168      0.916 0.956 0.000 0.000 0.000 0.028 NA
#&gt; GSM892367     3  0.0951      0.824 0.020 0.000 0.968 0.000 0.004 NA
#&gt; GSM892371     3  0.3921      0.790 0.020 0.000 0.816 0.040 0.092 NA
#&gt; GSM892375     2  0.4256      0.330 0.000 0.728 0.004 0.032 0.220 NA
#&gt; GSM892379     2  0.4680      0.515 0.000 0.684 0.000 0.000 0.132 NA
#&gt; GSM892385     2  0.5609      0.393 0.000 0.600 0.008 0.024 0.088 NA
#&gt; GSM892389     2  0.4733      0.523 0.000 0.688 0.000 0.004 0.120 NA
#&gt; GSM892341     3  0.4002      0.727 0.020 0.000 0.660 0.000 0.000 NA
#&gt; GSM892346     4  0.3476      0.792 0.132 0.000 0.000 0.816 0.024 NA
#&gt; GSM892350     4  0.2647      0.848 0.088 0.000 0.044 0.868 0.000 NA
#&gt; GSM892354     1  0.2451      0.902 0.884 0.000 0.000 0.000 0.056 NA
#&gt; GSM892356     1  0.2201      0.906 0.896 0.000 0.000 0.000 0.028 NA
#&gt; GSM892362     1  0.1074      0.916 0.960 0.000 0.000 0.000 0.028 NA
#&gt; GSM892366     3  0.1408      0.828 0.020 0.000 0.944 0.000 0.000 NA
#&gt; GSM892370     3  0.3921      0.790 0.020 0.000 0.816 0.040 0.092 NA
#&gt; GSM892374     5  0.3464      1.000 0.000 0.312 0.000 0.000 0.688 NA
#&gt; GSM892378     2  0.6405      0.121 0.000 0.504 0.008 0.032 0.296 NA
#&gt; GSM892382     2  0.0692      0.620 0.000 0.976 0.000 0.000 0.004 NA
#&gt; GSM892384     2  0.1074      0.617 0.000 0.960 0.000 0.000 0.012 NA
#&gt; GSM892388     2  0.6845     -0.279 0.000 0.420 0.000 0.068 0.324 NA
#&gt; GSM892343     3  0.5834      0.585 0.020 0.000 0.532 0.132 0.000 NA
#&gt; GSM892348     4  0.2853      0.819 0.124 0.000 0.008 0.852 0.008 NA
#&gt; GSM892352     4  0.6443      0.508 0.020 0.000 0.136 0.512 0.028 NA
#&gt; GSM892358     1  0.0146      0.920 0.996 0.000 0.000 0.000 0.004 NA
#&gt; GSM892360     1  0.0603      0.918 0.980 0.000 0.000 0.000 0.016 NA
#&gt; GSM892364     1  0.2201      0.903 0.900 0.000 0.000 0.000 0.048 NA
#&gt; GSM892368     3  0.1408      0.828 0.020 0.000 0.944 0.000 0.000 NA
#&gt; GSM892372     3  0.0547      0.825 0.020 0.000 0.980 0.000 0.000 NA
#&gt; GSM892376     2  0.4185      0.345 0.000 0.740 0.004 0.028 0.208 NA
#&gt; GSM892380     2  0.2839      0.620 0.000 0.860 0.000 0.004 0.044 NA
#&gt; GSM892386     2  0.1500      0.624 0.000 0.936 0.000 0.000 0.012 NA
#&gt; GSM892390     2  0.4474      0.528 0.000 0.704 0.000 0.000 0.108 NA
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> MAD:kmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> MAD:kmeans 46     0.905            0.934 3.31e-11      1.38e-04 3
#> MAD:kmeans 47     0.981            0.940 2.58e-16      2.20e-06 4
#> MAD:kmeans 45     0.999            0.723 1.15e-19      2.75e-08 5
#> MAD:kmeans 44     1.000            0.752 5.57e-19      7.88e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           0.989       0.995         0.4377 0.794   0.612
#> 4 4 1.000           0.975       0.991         0.1020 0.912   0.736
#> 5 5 0.972           0.948       0.964         0.0361 0.958   0.838
#> 6 6 0.972           0.911       0.917         0.0164 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892345     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892349     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892353     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892355     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892361     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892365     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892369     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892373     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892344     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892347     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892351     1   0.445      0.770 0.808  0 0.192
#&gt; GSM892357     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892359     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892363     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892367     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892371     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892341     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892346     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892350     1   0.207      0.931 0.940  0 0.060
#&gt; GSM892354     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892356     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892362     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892366     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892370     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892374     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892388     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892343     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892348     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892352     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892358     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892360     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892364     1   0.000      0.985 1.000  0 0.000
#&gt; GSM892368     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892372     3   0.000      1.000 0.000  0 1.000
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892349     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892369     3  0.0188      0.956 0.000  0 0.996 0.004
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     3  0.0469      0.949 0.000  0 0.988 0.012
#&gt; GSM892347     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892371     3  0.0188      0.956 0.000  0 0.996 0.004
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892346     4  0.0336      0.991 0.008  0 0.000 0.992
#&gt; GSM892350     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892370     3  0.0188      0.956 0.000  0 0.996 0.004
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     3  0.4941      0.235 0.000  0 0.564 0.436
#&gt; GSM892348     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892352     4  0.0000      0.999 0.000  0 0.000 1.000
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.957 0.000  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     5  0.0609      0.807 0.000 0.000 0.020 0.000 0.980
#&gt; GSM892345     4  0.0162      0.996 0.000 0.000 0.000 0.996 0.004
#&gt; GSM892349     5  0.4201      0.441 0.000 0.000 0.000 0.408 0.592
#&gt; GSM892353     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892355     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0162      0.997 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892365     3  0.3074      0.896 0.000 0.000 0.804 0.000 0.196
#&gt; GSM892369     3  0.0000      0.879 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892373     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892377     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892344     5  0.0510      0.808 0.000 0.000 0.016 0.000 0.984
#&gt; GSM892347     4  0.0000      0.996 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892351     4  0.0290      0.994 0.000 0.000 0.000 0.992 0.008
#&gt; GSM892357     1  0.0162      0.997 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892359     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.2230      0.906 0.000 0.000 0.884 0.000 0.116
#&gt; GSM892371     3  0.0000      0.879 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892375     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892341     5  0.0609      0.807 0.000 0.000 0.020 0.000 0.980
#&gt; GSM892346     4  0.0162      0.993 0.000 0.000 0.000 0.996 0.004
#&gt; GSM892350     4  0.0162      0.996 0.000 0.000 0.000 0.996 0.004
#&gt; GSM892354     1  0.0162      0.997 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892356     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0162      0.997 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892366     3  0.3074      0.896 0.000 0.000 0.804 0.000 0.196
#&gt; GSM892370     3  0.0000      0.879 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892374     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892378     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892343     5  0.3321      0.783 0.000 0.000 0.032 0.136 0.832
#&gt; GSM892348     4  0.0000      0.996 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892352     5  0.3336      0.721 0.000 0.000 0.000 0.228 0.772
#&gt; GSM892358     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.999 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.3074      0.896 0.000 0.000 0.804 0.000 0.196
#&gt; GSM892372     3  0.2648      0.906 0.000 0.000 0.848 0.000 0.152
#&gt; GSM892376     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892380     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892390     2  0.0162      0.998 0.000 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     5  0.1349      0.803 0.000 0.000 0.056 0.000 0.940 NA
#&gt; GSM892345     4  0.0891      0.949 0.000 0.000 0.000 0.968 0.008 NA
#&gt; GSM892349     5  0.5847      0.428 0.000 0.000 0.000 0.260 0.488 NA
#&gt; GSM892353     1  0.2118      0.922 0.888 0.000 0.000 0.000 0.008 NA
#&gt; GSM892355     1  0.1644      0.942 0.920 0.000 0.000 0.000 0.004 NA
#&gt; GSM892361     1  0.0547      0.970 0.980 0.000 0.000 0.000 0.000 NA
#&gt; GSM892365     3  0.0405      0.827 0.000 0.000 0.988 0.000 0.008 NA
#&gt; GSM892369     3  0.3789      0.715 0.000 0.000 0.584 0.000 0.000 NA
#&gt; GSM892373     2  0.0458      0.991 0.000 0.984 0.000 0.000 0.000 NA
#&gt; GSM892377     2  0.0363      0.992 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892381     2  0.0260      0.992 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892383     2  0.0363      0.994 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892387     2  0.0146      0.994 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892344     5  0.1196      0.805 0.000 0.000 0.040 0.000 0.952 NA
#&gt; GSM892347     4  0.0363      0.950 0.000 0.000 0.000 0.988 0.000 NA
#&gt; GSM892351     4  0.2494      0.881 0.000 0.000 0.000 0.864 0.016 NA
#&gt; GSM892357     1  0.0547      0.970 0.980 0.000 0.000 0.000 0.000 NA
#&gt; GSM892359     1  0.0713      0.968 0.972 0.000 0.000 0.000 0.000 NA
#&gt; GSM892363     1  0.0790      0.967 0.968 0.000 0.000 0.000 0.000 NA
#&gt; GSM892367     3  0.0790      0.830 0.000 0.000 0.968 0.000 0.000 NA
#&gt; GSM892371     3  0.3765      0.722 0.000 0.000 0.596 0.000 0.000 NA
#&gt; GSM892375     2  0.0458      0.993 0.000 0.984 0.000 0.000 0.000 NA
#&gt; GSM892379     2  0.0146      0.994 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892385     2  0.0146      0.993 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892389     2  0.0146      0.994 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892341     5  0.1204      0.804 0.000 0.000 0.056 0.000 0.944 NA
#&gt; GSM892346     4  0.0713      0.940 0.000 0.000 0.000 0.972 0.000 NA
#&gt; GSM892350     4  0.1285      0.940 0.000 0.000 0.000 0.944 0.004 NA
#&gt; GSM892354     1  0.0458      0.970 0.984 0.000 0.000 0.000 0.000 NA
#&gt; GSM892356     1  0.1007      0.969 0.956 0.000 0.000 0.000 0.000 NA
#&gt; GSM892362     1  0.0547      0.971 0.980 0.000 0.000 0.000 0.000 NA
#&gt; GSM892366     3  0.0405      0.827 0.000 0.000 0.988 0.000 0.008 NA
#&gt; GSM892370     3  0.3774      0.720 0.000 0.000 0.592 0.000 0.000 NA
#&gt; GSM892374     2  0.0458      0.991 0.000 0.984 0.000 0.000 0.000 NA
#&gt; GSM892378     2  0.0363      0.992 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892382     2  0.0260      0.992 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892384     2  0.0146      0.993 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892388     2  0.0146      0.993 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892343     5  0.3208      0.758 0.000 0.000 0.008 0.040 0.832 NA
#&gt; GSM892348     4  0.0632      0.942 0.000 0.000 0.000 0.976 0.000 NA
#&gt; GSM892352     5  0.5105      0.630 0.000 0.000 0.004 0.096 0.600 NA
#&gt; GSM892358     1  0.0260      0.972 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892360     1  0.0632      0.969 0.976 0.000 0.000 0.000 0.000 NA
#&gt; GSM892364     1  0.1141      0.965 0.948 0.000 0.000 0.000 0.000 NA
#&gt; GSM892368     3  0.0260      0.828 0.000 0.000 0.992 0.000 0.008 NA
#&gt; GSM892372     3  0.0547      0.831 0.000 0.000 0.980 0.000 0.000 NA
#&gt; GSM892376     2  0.0458      0.992 0.000 0.984 0.000 0.000 0.000 NA
#&gt; GSM892380     2  0.0146      0.994 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892386     2  0.0146      0.993 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892390     2  0.0146      0.993 0.000 0.996 0.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) disease.state(p) other(p) individual(p) k
#> MAD:skmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> MAD:skmeans 50     1.000            0.931 1.69e-11      8.93e-05 3
#> MAD:skmeans 49     0.995            0.988 2.28e-17      4.54e-07 4
#> MAD:skmeans 49     0.996            0.982 2.33e-20      7.98e-08 5
#> MAD:skmeans 49     0.996            0.982 2.33e-20      7.98e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.952           0.959       0.981         0.4362 0.794   0.612
#> 4 4 1.000           0.991       0.995         0.1049 0.900   0.704
#> 5 5 1.000           0.966       0.980         0.0372 0.958   0.840
#> 6 6 1.000           0.979       0.991         0.0162 0.988   0.946
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892345     1  0.3412      0.868 0.876  0 0.124
#&gt; GSM892349     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892353     1  0.0237      0.948 0.996  0 0.004
#&gt; GSM892355     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892361     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892369     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892347     1  0.3412      0.868 0.876  0 0.124
#&gt; GSM892351     1  0.6062      0.431 0.616  0 0.384
#&gt; GSM892357     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892371     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892346     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892350     1  0.3551      0.860 0.868  0 0.132
#&gt; GSM892354     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892370     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.3551      0.833 0.132  0 0.868
#&gt; GSM892348     1  0.2165      0.913 0.936  0 0.064
#&gt; GSM892352     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892358     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892364     1  0.0000      0.950 1.000  0 0.000
#&gt; GSM892368     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.989 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette   p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892349     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892353     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892365     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892369     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892344     3  0.2530      0.874 0.00  0 0.888 0.112
#&gt; GSM892347     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892367     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892371     3  0.0469      0.978 0.00  0 0.988 0.012
#&gt; GSM892375     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892341     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892346     4  0.1637      0.935 0.06  0 0.000 0.940
#&gt; GSM892350     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892366     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892370     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892374     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892343     4  0.1302      0.950 0.00  0 0.044 0.956
#&gt; GSM892348     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892352     4  0.0000      0.987 0.00  0 0.000 1.000
#&gt; GSM892358     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.00  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892372     3  0.0000      0.987 0.00  0 1.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.00  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.00  1 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.1608      0.916 0.000  0 0.072 0.000 0.928
#&gt; GSM892345     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892349     5  0.3177      0.785 0.000  0 0.000 0.208 0.792
#&gt; GSM892353     1  0.3636      0.615 0.728  0 0.000 0.000 0.272
#&gt; GSM892355     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.0162      0.969 0.000  0 0.996 0.000 0.004
#&gt; GSM892369     3  0.1608      0.949 0.000  0 0.928 0.000 0.072
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.1310      0.924 0.000  0 0.024 0.020 0.956
#&gt; GSM892347     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892357     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.0162      0.969 0.000  0 0.996 0.000 0.004
#&gt; GSM892371     3  0.1608      0.949 0.000  0 0.928 0.000 0.072
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.1608      0.916 0.000  0 0.072 0.000 0.928
#&gt; GSM892346     4  0.0609      0.972 0.020  0 0.000 0.980 0.000
#&gt; GSM892350     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892354     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.0162      0.969 0.000  0 0.996 0.000 0.004
#&gt; GSM892370     3  0.1608      0.949 0.000  0 0.928 0.000 0.072
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.0510      0.912 0.000  0 0.000 0.016 0.984
#&gt; GSM892348     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892352     5  0.1792      0.905 0.000  0 0.000 0.084 0.916
#&gt; GSM892358     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.974 1.000  0 0.000 0.000 0.000
#&gt; GSM892368     3  0.0162      0.969 0.000  0 0.996 0.000 0.004
#&gt; GSM892372     3  0.0162      0.969 0.000  0 0.996 0.000 0.004
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2 p3    p4    p5 p6
#&gt; GSM892342     5  0.0000      0.963 0.000  0  0 0.000 1.000  0
#&gt; GSM892345     4  0.0000      1.000 0.000  0  0 1.000 0.000  0
#&gt; GSM892349     5  0.2340      0.822 0.000  0  0 0.148 0.852  0
#&gt; GSM892353     1  0.3266      0.621 0.728  0  0 0.000 0.272  0
#&gt; GSM892355     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892361     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892365     3  0.0000      1.000 0.000  0  1 0.000 0.000  0
#&gt; GSM892369     6  0.0000      1.000 0.000  0  0 0.000 0.000  1
#&gt; GSM892373     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892377     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892381     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892383     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892387     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892344     5  0.0000      0.963 0.000  0  0 0.000 1.000  0
#&gt; GSM892347     4  0.0000      1.000 0.000  0  0 1.000 0.000  0
#&gt; GSM892351     4  0.0000      1.000 0.000  0  0 1.000 0.000  0
#&gt; GSM892357     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892359     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892363     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892367     3  0.0000      1.000 0.000  0  1 0.000 0.000  0
#&gt; GSM892371     6  0.0000      1.000 0.000  0  0 0.000 0.000  1
#&gt; GSM892375     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892379     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892385     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892389     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892341     5  0.0000      0.963 0.000  0  0 0.000 1.000  0
#&gt; GSM892346     4  0.0000      1.000 0.000  0  0 1.000 0.000  0
#&gt; GSM892350     4  0.0000      1.000 0.000  0  0 1.000 0.000  0
#&gt; GSM892354     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892356     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892362     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892366     3  0.0000      1.000 0.000  0  1 0.000 0.000  0
#&gt; GSM892370     6  0.0000      1.000 0.000  0  0 0.000 0.000  1
#&gt; GSM892374     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892378     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892382     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892384     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892388     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892343     5  0.0000      0.963 0.000  0  0 0.000 1.000  0
#&gt; GSM892348     4  0.0000      1.000 0.000  0  0 1.000 0.000  0
#&gt; GSM892352     5  0.0363      0.957 0.000  0  0 0.012 0.988  0
#&gt; GSM892358     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892360     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892364     1  0.0000      0.973 1.000  0  0 0.000 0.000  0
#&gt; GSM892368     3  0.0000      1.000 0.000  0  1 0.000 0.000  0
#&gt; GSM892372     3  0.0000      1.000 0.000  0  1 0.000 0.000  0
#&gt; GSM892376     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892380     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892386     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
#&gt; GSM892390     2  0.0000      1.000 0.000  1  0 0.000 0.000  0
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) disease.state(p) other(p) individual(p) k
#> MAD:pam 50     1.000            0.934 2.67e-07      1.42e-03 2
#> MAD:pam 49     0.981            0.952 1.63e-11      6.57e-05 3
#> MAD:pam 50     0.977            0.951 1.45e-16      7.83e-07 4
#> MAD:pam 50     1.000            0.998 3.06e-20      2.01e-07 5
#> MAD:pam 50     0.991            0.984 4.62e-22      7.12e-08 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           1.000       1.000         0.4158 0.804   0.630
#> 4 4 0.959           0.920       0.966         0.1176 0.922   0.765
#> 5 5 1.000           0.944       0.980         0.0395 0.965   0.864
#> 6 6 0.993           0.984       0.989         0.0272 0.969   0.866
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3
#&gt; GSM892342     3       0          1  0  0  1
#&gt; GSM892345     3       0          1  0  0  1
#&gt; GSM892349     3       0          1  0  0  1
#&gt; GSM892353     1       0          1  1  0  0
#&gt; GSM892355     1       0          1  1  0  0
#&gt; GSM892361     1       0          1  1  0  0
#&gt; GSM892365     3       0          1  0  0  1
#&gt; GSM892369     3       0          1  0  0  1
#&gt; GSM892373     2       0          1  0  1  0
#&gt; GSM892377     2       0          1  0  1  0
#&gt; GSM892381     2       0          1  0  1  0
#&gt; GSM892383     2       0          1  0  1  0
#&gt; GSM892387     2       0          1  0  1  0
#&gt; GSM892344     3       0          1  0  0  1
#&gt; GSM892347     3       0          1  0  0  1
#&gt; GSM892351     3       0          1  0  0  1
#&gt; GSM892357     1       0          1  1  0  0
#&gt; GSM892359     1       0          1  1  0  0
#&gt; GSM892363     1       0          1  1  0  0
#&gt; GSM892367     3       0          1  0  0  1
#&gt; GSM892371     3       0          1  0  0  1
#&gt; GSM892375     2       0          1  0  1  0
#&gt; GSM892379     2       0          1  0  1  0
#&gt; GSM892385     2       0          1  0  1  0
#&gt; GSM892389     2       0          1  0  1  0
#&gt; GSM892341     3       0          1  0  0  1
#&gt; GSM892346     3       0          1  0  0  1
#&gt; GSM892350     3       0          1  0  0  1
#&gt; GSM892354     1       0          1  1  0  0
#&gt; GSM892356     1       0          1  1  0  0
#&gt; GSM892362     1       0          1  1  0  0
#&gt; GSM892366     3       0          1  0  0  1
#&gt; GSM892370     3       0          1  0  0  1
#&gt; GSM892374     2       0          1  0  1  0
#&gt; GSM892378     2       0          1  0  1  0
#&gt; GSM892382     2       0          1  0  1  0
#&gt; GSM892384     2       0          1  0  1  0
#&gt; GSM892388     2       0          1  0  1  0
#&gt; GSM892343     3       0          1  0  0  1
#&gt; GSM892348     3       0          1  0  0  1
#&gt; GSM892352     3       0          1  0  0  1
#&gt; GSM892358     1       0          1  1  0  0
#&gt; GSM892360     1       0          1  1  0  0
#&gt; GSM892364     1       0          1  1  0  0
#&gt; GSM892368     3       0          1  0  0  1
#&gt; GSM892372     3       0          1  0  0  1
#&gt; GSM892376     2       0          1  0  1  0
#&gt; GSM892380     2       0          1  0  1  0
#&gt; GSM892386     2       0          1  0  1  0
#&gt; GSM892390     2       0          1  0  1  0
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2    p3    p4
#&gt; GSM892342     3  0.0000      0.873  0  0 1.000 0.000
#&gt; GSM892345     4  0.0000      0.922  0  0 0.000 1.000
#&gt; GSM892349     4  0.2868      0.802  0  0 0.136 0.864
#&gt; GSM892353     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892365     3  0.0921      0.865  0  0 0.972 0.028
#&gt; GSM892369     3  0.3837      0.710  0  0 0.776 0.224
#&gt; GSM892373     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892344     3  0.0000      0.873  0  0 1.000 0.000
#&gt; GSM892347     4  0.0000      0.922  0  0 0.000 1.000
#&gt; GSM892351     4  0.0000      0.922  0  0 0.000 1.000
#&gt; GSM892357     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892367     3  0.0469      0.869  0  0 0.988 0.012
#&gt; GSM892371     3  0.4008      0.674  0  0 0.756 0.244
#&gt; GSM892375     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892341     3  0.0000      0.873  0  0 1.000 0.000
#&gt; GSM892346     4  0.0000      0.922  0  0 0.000 1.000
#&gt; GSM892350     4  0.0000      0.922  0  0 0.000 1.000
#&gt; GSM892354     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892366     3  0.0188      0.873  0  0 0.996 0.004
#&gt; GSM892370     3  0.4941      0.268  0  0 0.564 0.436
#&gt; GSM892374     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892343     3  0.4193      0.605  0  0 0.732 0.268
#&gt; GSM892348     4  0.0000      0.922  0  0 0.000 1.000
#&gt; GSM892352     4  0.4564      0.441  0  0 0.328 0.672
#&gt; GSM892358     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000  1  0 0.000 0.000
#&gt; GSM892368     3  0.0000      0.873  0  0 1.000 0.000
#&gt; GSM892372     3  0.0592      0.870  0  0 0.984 0.016
#&gt; GSM892376     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000  0  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000  0  1 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     3  0.0000      0.935 0.000  0 1.000 0.000 0.000
#&gt; GSM892345     4  0.0000      0.914 0.000  0 0.000 1.000 0.000
#&gt; GSM892349     4  0.4306      0.029 0.000  0 0.000 0.508 0.492
#&gt; GSM892353     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892355     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.935 0.000  0 1.000 0.000 0.000
#&gt; GSM892369     5  0.0000      0.987 0.000  0 0.000 0.000 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     3  0.4249      0.244 0.000  0 0.568 0.000 0.432
#&gt; GSM892347     4  0.0000      0.914 0.000  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.0000      0.914 0.000  0 0.000 1.000 0.000
#&gt; GSM892357     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.0162      0.932 0.000  0 0.996 0.004 0.000
#&gt; GSM892371     5  0.0162      0.987 0.000  0 0.000 0.004 0.996
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     3  0.0000      0.935 0.000  0 1.000 0.000 0.000
#&gt; GSM892346     4  0.0000      0.914 0.000  0 0.000 1.000 0.000
#&gt; GSM892350     4  0.0000      0.914 0.000  0 0.000 1.000 0.000
#&gt; GSM892354     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.935 0.000  0 1.000 0.000 0.000
#&gt; GSM892370     5  0.0510      0.980 0.000  0 0.000 0.016 0.984
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.0794      0.971 0.000  0 0.000 0.028 0.972
#&gt; GSM892348     4  0.0000      0.914 0.000  0 0.000 1.000 0.000
#&gt; GSM892352     5  0.0000      0.987 0.000  0 0.000 0.000 1.000
#&gt; GSM892358     1  0.0162      0.996 0.996  0 0.000 0.000 0.004
#&gt; GSM892360     1  0.0000      0.999 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0162      0.996 0.996  0 0.000 0.000 0.004
#&gt; GSM892368     3  0.0000      0.935 0.000  0 1.000 0.000 0.000
#&gt; GSM892372     3  0.0162      0.932 0.000  0 0.996 0.004 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5    p6
#&gt; GSM892342     3  0.0000      0.983 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892345     4  0.0000      0.998 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM892349     5  0.1082      0.955 0.000  0 0.000 0.040 0.956 0.004
#&gt; GSM892353     6  0.0865      0.929 0.036  0 0.000 0.000 0.000 0.964
#&gt; GSM892355     6  0.1663      0.950 0.088  0 0.000 0.000 0.000 0.912
#&gt; GSM892361     1  0.0000      0.987 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.983 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892369     5  0.0000      0.984 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892344     3  0.1863      0.878 0.000  0 0.896 0.000 0.104 0.000
#&gt; GSM892347     4  0.0000      0.998 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM892351     4  0.0000      0.998 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM892357     1  0.0000      0.987 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0260      0.983 0.992  0 0.000 0.000 0.000 0.008
#&gt; GSM892363     1  0.0000      0.987 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.0146      0.980 0.000  0 0.996 0.004 0.000 0.000
#&gt; GSM892371     5  0.0000      0.984 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892341     3  0.0000      0.983 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892346     4  0.0260      0.993 0.000  0 0.000 0.992 0.000 0.008
#&gt; GSM892350     4  0.0000      0.998 0.000  0 0.000 1.000 0.000 0.000
#&gt; GSM892354     1  0.0146      0.985 0.996  0 0.000 0.000 0.000 0.004
#&gt; GSM892356     1  0.0000      0.987 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.987 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.983 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892370     5  0.0363      0.980 0.000  0 0.000 0.012 0.988 0.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892343     5  0.0458      0.978 0.000  0 0.000 0.016 0.984 0.000
#&gt; GSM892348     4  0.0146      0.996 0.000  0 0.000 0.996 0.000 0.004
#&gt; GSM892352     5  0.0000      0.984 0.000  0 0.000 0.000 1.000 0.000
#&gt; GSM892358     6  0.2003      0.935 0.116  0 0.000 0.000 0.000 0.884
#&gt; GSM892360     1  0.0000      0.987 1.000  0 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.1663      0.910 0.912  0 0.000 0.000 0.000 0.088
#&gt; GSM892368     3  0.0000      0.983 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892372     3  0.0000      0.983 0.000  0 1.000 0.000 0.000 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> MAD:mclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> MAD:mclust 50     1.000            0.931 1.26e-12      1.60e-05 3
#> MAD:mclust 48     0.985            0.946 7.68e-17      1.01e-06 4
#> MAD:mclust 48     0.992            0.975 8.94e-16      1.54e-06 5
#> MAD:mclust 50     0.994            0.989 5.92e-15      1.19e-04 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.966           0.972       0.974         0.4151 0.794   0.612
#> 4 4 1.000           0.975       0.981         0.1144 0.912   0.736
#> 5 5 0.960           0.944       0.952         0.0360 0.958   0.838
#> 6 6 0.973           0.934       0.951         0.0107 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM892342     3  0.1529      0.959 0.040 0.000 0.960
#&gt; GSM892345     1  0.0237      0.985 0.996 0.000 0.004
#&gt; GSM892349     3  0.4555      0.834 0.200 0.000 0.800
#&gt; GSM892353     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892355     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892365     3  0.1529      0.959 0.040 0.000 0.960
#&gt; GSM892369     3  0.1753      0.959 0.048 0.000 0.952
#&gt; GSM892373     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892377     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892381     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892383     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892387     2  0.0747      0.989 0.000 0.984 0.016
#&gt; GSM892344     3  0.2165      0.951 0.064 0.000 0.936
#&gt; GSM892347     1  0.0592      0.979 0.988 0.000 0.012
#&gt; GSM892351     1  0.3116      0.873 0.892 0.000 0.108
#&gt; GSM892357     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892367     3  0.1529      0.959 0.040 0.000 0.960
#&gt; GSM892371     3  0.1753      0.959 0.048 0.000 0.952
#&gt; GSM892375     2  0.0424      0.994 0.000 0.992 0.008
#&gt; GSM892379     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892385     2  0.0424      0.994 0.000 0.992 0.008
#&gt; GSM892389     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892341     3  0.1860      0.958 0.052 0.000 0.948
#&gt; GSM892346     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892350     1  0.2066      0.931 0.940 0.000 0.060
#&gt; GSM892354     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892366     3  0.1529      0.959 0.040 0.000 0.960
#&gt; GSM892370     3  0.1643      0.959 0.044 0.000 0.956
#&gt; GSM892374     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892378     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892382     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892384     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892388     2  0.0592      0.992 0.000 0.988 0.012
#&gt; GSM892343     3  0.4291      0.857 0.180 0.000 0.820
#&gt; GSM892348     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892352     3  0.4399      0.849 0.188 0.000 0.812
#&gt; GSM892358     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.988 1.000 0.000 0.000
#&gt; GSM892368     3  0.1529      0.959 0.040 0.000 0.960
#&gt; GSM892372     3  0.1529      0.959 0.040 0.000 0.960
#&gt; GSM892376     2  0.0000      0.997 0.000 1.000 0.000
#&gt; GSM892380     2  0.0747      0.989 0.000 0.984 0.016
#&gt; GSM892386     2  0.0237      0.996 0.000 0.996 0.004
#&gt; GSM892390     2  0.0424      0.994 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.0895      0.956 0.004 0.000 0.976 0.020
#&gt; GSM892345     4  0.1724      0.979 0.020 0.000 0.032 0.948
#&gt; GSM892349     4  0.2125      0.943 0.004 0.000 0.076 0.920
#&gt; GSM892353     1  0.0336      0.990 0.992 0.000 0.000 0.008
#&gt; GSM892355     1  0.0000      0.997 1.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0336      0.993 0.992 0.000 0.000 0.008
#&gt; GSM892365     3  0.0000      0.962 0.000 0.000 1.000 0.000
#&gt; GSM892369     3  0.0336      0.960 0.000 0.000 0.992 0.008
#&gt; GSM892373     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM892377     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892381     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892387     2  0.0336      0.994 0.000 0.992 0.000 0.008
#&gt; GSM892344     3  0.1042      0.954 0.008 0.000 0.972 0.020
#&gt; GSM892347     4  0.1724      0.979 0.020 0.000 0.032 0.948
#&gt; GSM892351     4  0.1610      0.978 0.016 0.000 0.032 0.952
#&gt; GSM892357     1  0.0000      0.997 1.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.997 1.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.997 1.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.0000      0.962 0.000 0.000 1.000 0.000
#&gt; GSM892371     3  0.0469      0.960 0.000 0.000 0.988 0.012
#&gt; GSM892375     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892385     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892389     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892341     3  0.0895      0.956 0.004 0.000 0.976 0.020
#&gt; GSM892346     4  0.1510      0.965 0.028 0.000 0.016 0.956
#&gt; GSM892350     4  0.1724      0.979 0.020 0.000 0.032 0.948
#&gt; GSM892354     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892356     1  0.0000      0.997 1.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892366     3  0.0000      0.962 0.000 0.000 1.000 0.000
#&gt; GSM892370     3  0.0592      0.957 0.000 0.000 0.984 0.016
#&gt; GSM892374     2  0.0188      0.997 0.000 0.996 0.000 0.004
#&gt; GSM892378     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892388     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892343     3  0.4844      0.547 0.012 0.000 0.688 0.300
#&gt; GSM892348     4  0.1624      0.977 0.020 0.000 0.028 0.952
#&gt; GSM892352     4  0.2053      0.947 0.004 0.000 0.072 0.924
#&gt; GSM892358     1  0.0000      0.997 1.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892364     1  0.0188      0.996 0.996 0.000 0.000 0.004
#&gt; GSM892368     3  0.0000      0.962 0.000 0.000 1.000 0.000
#&gt; GSM892372     3  0.0000      0.962 0.000 0.000 1.000 0.000
#&gt; GSM892376     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892380     2  0.0336      0.994 0.000 0.992 0.000 0.008
#&gt; GSM892386     2  0.0000      0.999 0.000 1.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.999 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     5  0.3715      0.747 0.000 0.000 0.260 0.004 0.736
#&gt; GSM892345     4  0.1851      0.910 0.000 0.000 0.000 0.912 0.088
#&gt; GSM892349     5  0.4206      0.616 0.000 0.000 0.016 0.288 0.696
#&gt; GSM892353     1  0.1043      0.963 0.960 0.000 0.000 0.000 0.040
#&gt; GSM892355     1  0.0162      0.993 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892361     1  0.0162      0.993 0.996 0.000 0.000 0.004 0.000
#&gt; GSM892365     3  0.0794      0.962 0.000 0.000 0.972 0.000 0.028
#&gt; GSM892369     3  0.1300      0.948 0.000 0.000 0.956 0.016 0.028
#&gt; GSM892373     2  0.0794      0.984 0.000 0.972 0.000 0.000 0.028
#&gt; GSM892377     2  0.0510      0.990 0.000 0.984 0.000 0.000 0.016
#&gt; GSM892381     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0162      0.994 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892387     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; GSM892344     5  0.3803      0.808 0.000 0.000 0.140 0.056 0.804
#&gt; GSM892347     4  0.1043      0.919 0.000 0.000 0.000 0.960 0.040
#&gt; GSM892351     4  0.2966      0.803 0.000 0.000 0.000 0.816 0.184
#&gt; GSM892357     1  0.0000      0.995 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.995 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.995 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.0162      0.966 0.000 0.000 0.996 0.000 0.004
#&gt; GSM892371     3  0.0898      0.958 0.000 0.000 0.972 0.008 0.020
#&gt; GSM892375     2  0.0404      0.991 0.000 0.988 0.000 0.000 0.012
#&gt; GSM892379     2  0.0162      0.994 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892385     2  0.0162      0.994 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892389     2  0.0162      0.994 0.000 0.996 0.000 0.000 0.004
#&gt; GSM892341     5  0.4169      0.782 0.000 0.000 0.240 0.028 0.732
#&gt; GSM892346     4  0.1197      0.866 0.000 0.000 0.000 0.952 0.048
#&gt; GSM892350     4  0.1608      0.917 0.000 0.000 0.000 0.928 0.072
#&gt; GSM892354     1  0.0000      0.995 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0162      0.994 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892362     1  0.0162      0.994 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892366     3  0.1043      0.954 0.000 0.000 0.960 0.000 0.040
#&gt; GSM892370     3  0.0992      0.956 0.000 0.000 0.968 0.008 0.024
#&gt; GSM892374     2  0.0794      0.984 0.000 0.972 0.000 0.000 0.028
#&gt; GSM892378     2  0.0290      0.992 0.000 0.992 0.000 0.000 0.008
#&gt; GSM892382     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0510      0.989 0.000 0.984 0.000 0.000 0.016
#&gt; GSM892343     5  0.5344      0.784 0.000 0.000 0.168 0.160 0.672
#&gt; GSM892348     4  0.0290      0.908 0.000 0.000 0.000 0.992 0.008
#&gt; GSM892352     5  0.3929      0.714 0.000 0.000 0.028 0.208 0.764
#&gt; GSM892358     1  0.0000      0.995 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.995 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0162      0.994 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892368     3  0.0880      0.960 0.000 0.000 0.968 0.000 0.032
#&gt; GSM892372     3  0.0404      0.966 0.000 0.000 0.988 0.000 0.012
#&gt; GSM892376     2  0.0290      0.993 0.000 0.992 0.000 0.000 0.008
#&gt; GSM892380     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.994 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM892342     5  0.1901      0.788 0.000 0.000 0.076 0.004 0.912 NA
#&gt; GSM892345     4  0.1970      0.904 0.000 0.000 0.000 0.912 0.060 NA
#&gt; GSM892349     5  0.3728      0.568 0.000 0.000 0.000 0.344 0.652 NA
#&gt; GSM892353     1  0.1555      0.933 0.932 0.000 0.004 0.000 0.060 NA
#&gt; GSM892355     1  0.0146      0.991 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892361     1  0.0146      0.991 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892365     3  0.0937      0.970 0.000 0.000 0.960 0.000 0.040 NA
#&gt; GSM892369     3  0.0891      0.960 0.000 0.000 0.968 0.008 0.000 NA
#&gt; GSM892373     2  0.1714      0.940 0.000 0.908 0.000 0.000 0.000 NA
#&gt; GSM892377     2  0.1007      0.971 0.000 0.956 0.000 0.000 0.000 NA
#&gt; GSM892381     2  0.0260      0.980 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892383     2  0.0260      0.979 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892387     2  0.0363      0.979 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892344     5  0.1901      0.807 0.000 0.000 0.028 0.040 0.924 NA
#&gt; GSM892347     4  0.0806      0.906 0.000 0.000 0.000 0.972 0.020 NA
#&gt; GSM892351     4  0.2730      0.785 0.000 0.000 0.000 0.836 0.152 NA
#&gt; GSM892357     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892359     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892363     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892367     3  0.0508      0.973 0.000 0.000 0.984 0.000 0.012 NA
#&gt; GSM892371     3  0.0622      0.966 0.000 0.000 0.980 0.008 0.000 NA
#&gt; GSM892375     2  0.0937      0.973 0.000 0.960 0.000 0.000 0.000 NA
#&gt; GSM892379     2  0.0713      0.976 0.000 0.972 0.000 0.000 0.000 NA
#&gt; GSM892385     2  0.0458      0.979 0.000 0.984 0.000 0.000 0.000 NA
#&gt; GSM892389     2  0.0260      0.980 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892341     5  0.2006      0.801 0.000 0.000 0.080 0.016 0.904 NA
#&gt; GSM892346     4  0.2135      0.830 0.000 0.000 0.000 0.872 0.000 NA
#&gt; GSM892350     4  0.1398      0.904 0.000 0.000 0.000 0.940 0.052 NA
#&gt; GSM892354     1  0.0146      0.991 0.996 0.000 0.000 0.000 0.000 NA
#&gt; GSM892356     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892362     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892366     3  0.1075      0.964 0.000 0.000 0.952 0.000 0.048 NA
#&gt; GSM892370     3  0.0508      0.968 0.000 0.000 0.984 0.004 0.000 NA
#&gt; GSM892374     2  0.1908      0.934 0.000 0.900 0.000 0.000 0.004 NA
#&gt; GSM892378     2  0.0458      0.979 0.000 0.984 0.000 0.000 0.000 NA
#&gt; GSM892382     2  0.0146      0.980 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892384     2  0.0260      0.980 0.000 0.992 0.000 0.000 0.000 NA
#&gt; GSM892388     2  0.1387      0.955 0.000 0.932 0.000 0.000 0.000 NA
#&gt; GSM892343     5  0.3584      0.796 0.004 0.000 0.060 0.104 0.820 NA
#&gt; GSM892348     4  0.1196      0.896 0.000 0.000 0.000 0.952 0.008 NA
#&gt; GSM892352     5  0.4058      0.618 0.000 0.000 0.004 0.320 0.660 NA
#&gt; GSM892358     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892360     1  0.0000      0.993 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM892364     1  0.0260      0.988 0.992 0.000 0.000 0.000 0.000 NA
#&gt; GSM892368     3  0.0865      0.972 0.000 0.000 0.964 0.000 0.036 NA
#&gt; GSM892372     3  0.0713      0.973 0.000 0.000 0.972 0.000 0.028 NA
#&gt; GSM892376     2  0.0363      0.979 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892380     2  0.0146      0.979 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM892386     2  0.0363      0.979 0.000 0.988 0.000 0.000 0.000 NA
#&gt; GSM892390     2  0.0000      0.980 0.000 1.000 0.000 0.000 0.000 NA
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) disease.state(p) other(p) individual(p) k
#> MAD:NMF 50         1            0.934 2.67e-07      1.42e-03 2
#> MAD:NMF 50         1            0.931 1.69e-11      8.93e-05 3
#> MAD:NMF 50         1            0.986 6.71e-18      2.02e-07 4
#> MAD:NMF 50         1            0.998 3.06e-20      2.01e-07 5
#> MAD:NMF 50         1            0.998 3.06e-20      2.01e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.846           0.908       0.950         0.3783 0.804   0.630
#> 4 4 0.815           0.874       0.929         0.0799 0.974   0.922
#> 5 5 0.863           0.796       0.866         0.0717 0.922   0.745
#> 6 6 0.918           0.833       0.914         0.0588 0.949   0.780
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3   0.597      0.601 0.364  0 0.636
#&gt; GSM892345     1   0.288      0.886 0.904  0 0.096
#&gt; GSM892349     1   0.304      0.877 0.896  0 0.104
#&gt; GSM892353     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892355     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892361     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892365     3   0.000      0.816 0.000  0 1.000
#&gt; GSM892369     3   0.304      0.822 0.104  0 0.896
#&gt; GSM892373     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892344     3   0.599      0.596 0.368  0 0.632
#&gt; GSM892347     1   0.196      0.918 0.944  0 0.056
#&gt; GSM892351     1   0.288      0.886 0.904  0 0.096
#&gt; GSM892357     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892359     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892363     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892367     3   0.000      0.816 0.000  0 1.000
#&gt; GSM892371     3   0.304      0.822 0.104  0 0.896
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892341     3   0.597      0.601 0.364  0 0.636
#&gt; GSM892346     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892350     1   0.288      0.886 0.904  0 0.096
#&gt; GSM892354     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892356     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892362     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892366     3   0.000      0.816 0.000  0 1.000
#&gt; GSM892370     3   0.304      0.822 0.104  0 0.896
#&gt; GSM892374     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892388     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892343     3   0.599      0.596 0.368  0 0.632
#&gt; GSM892348     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892352     1   0.510      0.627 0.752  0 0.248
#&gt; GSM892358     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892360     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892364     1   0.000      0.954 1.000  0 0.000
#&gt; GSM892368     3   0.000      0.816 0.000  0 1.000
#&gt; GSM892372     3   0.103      0.821 0.024  0 0.976
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     4   0.289      0.996 0.124 0.000 0.004 0.872
#&gt; GSM892345     1   0.401      0.666 0.756 0.000 0.000 0.244
#&gt; GSM892349     1   0.422      0.625 0.728 0.000 0.000 0.272
#&gt; GSM892353     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892355     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892361     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892365     3   0.000      0.820 0.000 0.000 1.000 0.000
#&gt; GSM892369     3   0.445      0.716 0.000 0.000 0.692 0.308
#&gt; GSM892373     2   0.270      0.896 0.000 0.876 0.000 0.124
#&gt; GSM892377     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892381     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892383     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892387     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892344     4   0.270      0.996 0.124 0.000 0.000 0.876
#&gt; GSM892347     1   0.228      0.823 0.904 0.000 0.000 0.096
#&gt; GSM892351     1   0.416      0.639 0.736 0.000 0.000 0.264
#&gt; GSM892357     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892359     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892363     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892367     3   0.000      0.820 0.000 0.000 1.000 0.000
#&gt; GSM892371     3   0.445      0.716 0.000 0.000 0.692 0.308
#&gt; GSM892375     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892379     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892385     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892389     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892341     4   0.289      0.996 0.124 0.000 0.004 0.872
#&gt; GSM892346     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892350     1   0.416      0.639 0.736 0.000 0.000 0.264
#&gt; GSM892354     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892356     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892362     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892366     3   0.000      0.820 0.000 0.000 1.000 0.000
#&gt; GSM892370     3   0.445      0.716 0.000 0.000 0.692 0.308
#&gt; GSM892374     2   0.270      0.896 0.000 0.876 0.000 0.124
#&gt; GSM892378     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892382     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892384     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892388     2   0.270      0.896 0.000 0.876 0.000 0.124
#&gt; GSM892343     4   0.270      0.996 0.124 0.000 0.000 0.876
#&gt; GSM892348     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892352     1   0.497      0.151 0.548 0.000 0.000 0.452
#&gt; GSM892358     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892360     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892364     1   0.000      0.896 1.000 0.000 0.000 0.000
#&gt; GSM892368     3   0.000      0.820 0.000 0.000 1.000 0.000
#&gt; GSM892372     3   0.327      0.790 0.000 0.000 0.832 0.168
#&gt; GSM892376     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892380     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892386     2   0.000      0.981 0.000 1.000 0.000 0.000
#&gt; GSM892390     2   0.000      0.981 0.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1  p2    p3    p4    p5
#&gt; GSM892342     5  0.0162      0.996 0.000 0.0 0.004 0.000 0.996
#&gt; GSM892345     4  0.0963      0.610 0.036 0.0 0.000 0.964 0.000
#&gt; GSM892349     4  0.0290      0.628 0.000 0.0 0.000 0.992 0.008
#&gt; GSM892353     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892355     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892361     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892365     3  0.0000      0.826 0.000 0.0 1.000 0.000 0.000
#&gt; GSM892369     3  0.3969      0.730 0.000 0.0 0.692 0.004 0.304
#&gt; GSM892373     2  0.4182      0.581 0.400 0.6 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892344     5  0.0162      0.996 0.000 0.0 0.000 0.004 0.996
#&gt; GSM892347     4  0.3508      0.207 0.252 0.0 0.000 0.748 0.000
#&gt; GSM892351     4  0.0000      0.630 0.000 0.0 0.000 1.000 0.000
#&gt; GSM892357     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892359     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892363     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892367     3  0.0000      0.826 0.000 0.0 1.000 0.000 0.000
#&gt; GSM892371     3  0.3969      0.730 0.000 0.0 0.692 0.004 0.304
#&gt; GSM892375     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892341     5  0.0162      0.996 0.000 0.0 0.004 0.000 0.996
#&gt; GSM892346     4  0.4425     -0.600 0.452 0.0 0.000 0.544 0.004
#&gt; GSM892350     4  0.0000      0.630 0.000 0.0 0.000 1.000 0.000
#&gt; GSM892354     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892356     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892362     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892366     3  0.0000      0.826 0.000 0.0 1.000 0.000 0.000
#&gt; GSM892370     3  0.3969      0.730 0.000 0.0 0.692 0.004 0.304
#&gt; GSM892374     2  0.4182      0.581 0.400 0.6 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892388     2  0.4182      0.581 0.400 0.6 0.000 0.000 0.000
#&gt; GSM892343     5  0.0162      0.996 0.000 0.0 0.000 0.004 0.996
#&gt; GSM892348     4  0.4425     -0.600 0.452 0.0 0.000 0.544 0.004
#&gt; GSM892352     4  0.3534      0.250 0.000 0.0 0.000 0.744 0.256
#&gt; GSM892358     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892360     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892364     1  0.4182      1.000 0.600 0.0 0.000 0.400 0.000
#&gt; GSM892368     3  0.0000      0.826 0.000 0.0 1.000 0.000 0.000
#&gt; GSM892372     3  0.2813      0.799 0.000 0.0 0.832 0.000 0.168
#&gt; GSM892376     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.934 0.000 1.0 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.0405      0.992 0.000 0.000 0.004 0.000 0.988 0.008
#&gt; GSM892345     4  0.1408      0.833 0.020 0.000 0.000 0.944 0.000 0.036
#&gt; GSM892349     4  0.0405      0.851 0.004 0.000 0.000 0.988 0.000 0.008
#&gt; GSM892353     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.827 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892369     3  0.3446      0.732 0.000 0.000 0.692 0.000 0.308 0.000
#&gt; GSM892373     6  0.3868      0.256 0.000 0.492 0.000 0.000 0.000 0.508
#&gt; GSM892377     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.0260      0.992 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM892347     4  0.4060      0.590 0.032 0.000 0.000 0.684 0.000 0.284
#&gt; GSM892351     4  0.0146      0.853 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM892357     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.0000      0.827 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892371     3  0.3446      0.732 0.000 0.000 0.692 0.000 0.308 0.000
#&gt; GSM892375     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.0146      0.992 0.000 0.000 0.004 0.000 0.996 0.000
#&gt; GSM892346     6  0.4705     -0.479 0.044 0.000 0.000 0.472 0.000 0.484
#&gt; GSM892350     4  0.0146      0.853 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM892354     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.827 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892370     3  0.3446      0.732 0.000 0.000 0.692 0.000 0.308 0.000
#&gt; GSM892374     6  0.3868      0.256 0.000 0.492 0.000 0.000 0.000 0.508
#&gt; GSM892378     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892388     6  0.3868      0.256 0.000 0.492 0.000 0.000 0.000 0.508
#&gt; GSM892343     5  0.0000      0.992 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892348     6  0.4705     -0.479 0.044 0.000 0.000 0.472 0.000 0.484
#&gt; GSM892352     4  0.3373      0.586 0.000 0.000 0.000 0.744 0.248 0.008
#&gt; GSM892358     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.0000      0.827 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892372     3  0.2527      0.800 0.000 0.000 0.832 0.000 0.168 0.000
#&gt; GSM892376     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> ATC:hclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> ATC:hclust 50     1.000            0.931 1.26e-12      1.60e-05 3
#> ATC:hclust 49     0.998            0.993 2.28e-17      4.54e-07 4
#> ATC:hclust 46     0.922            0.927 2.35e-20      4.18e-08 5
#> ATC:hclust 45     0.970            1.000 1.15e-19      1.17e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.775           0.952       0.907         0.3034 0.811   0.644
#> 4 4 0.645           0.532       0.805         0.1402 0.984   0.952
#> 5 5 0.764           0.840       0.823         0.0741 0.878   0.633
#> 6 6 0.747           0.771       0.777         0.0570 0.962   0.832
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM892342     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892345     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892349     1   0.116      0.948 0.972 0.000 0.028
#&gt; GSM892353     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892355     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892361     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892365     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892369     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892373     2   0.562      0.761 0.000 0.692 0.308
#&gt; GSM892377     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892381     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892383     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892387     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892344     3   0.601      0.894 0.372 0.000 0.628
#&gt; GSM892347     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892351     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892357     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892359     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892363     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892367     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892371     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892375     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892379     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892385     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892389     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892341     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892346     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892350     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892354     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892356     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892362     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892366     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892370     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892374     2   0.562      0.761 0.000 0.692 0.308
#&gt; GSM892378     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892382     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892384     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892388     2   0.562      0.761 0.000 0.692 0.308
#&gt; GSM892343     1   0.450      0.609 0.804 0.000 0.196
#&gt; GSM892348     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892352     1   0.116      0.948 0.972 0.000 0.028
#&gt; GSM892358     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892360     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892364     1   0.000      0.981 1.000 0.000 0.000
#&gt; GSM892368     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892372     3   0.562      0.990 0.308 0.000 0.692
#&gt; GSM892376     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892380     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892386     2   0.000      0.958 0.000 1.000 0.000
#&gt; GSM892390     2   0.000      0.958 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.5160      0.911 0.136 0.000 0.760 0.104
#&gt; GSM892345     1  0.4916     -0.706 0.576 0.000 0.000 0.424
#&gt; GSM892349     1  0.5168     -0.972 0.504 0.000 0.004 0.492
#&gt; GSM892353     1  0.0592      0.575 0.984 0.000 0.000 0.016
#&gt; GSM892355     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892365     3  0.2868      0.931 0.136 0.000 0.864 0.000
#&gt; GSM892369     3  0.4514      0.930 0.136 0.000 0.800 0.064
#&gt; GSM892373     2  0.5000      0.596 0.000 0.504 0.000 0.496
#&gt; GSM892377     2  0.2408      0.889 0.000 0.896 0.104 0.000
#&gt; GSM892381     2  0.1022      0.899 0.000 0.968 0.032 0.000
#&gt; GSM892383     2  0.0817      0.900 0.000 0.976 0.024 0.000
#&gt; GSM892387     2  0.0921      0.901 0.000 0.972 0.028 0.000
#&gt; GSM892344     3  0.6980      0.636 0.164 0.000 0.572 0.264
#&gt; GSM892347     1  0.4916     -0.706 0.576 0.000 0.000 0.424
#&gt; GSM892351     1  0.4989     -0.886 0.528 0.000 0.000 0.472
#&gt; GSM892357     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.2868      0.931 0.136 0.000 0.864 0.000
#&gt; GSM892371     3  0.4514      0.930 0.136 0.000 0.800 0.064
#&gt; GSM892375     2  0.1118      0.898 0.000 0.964 0.036 0.000
#&gt; GSM892379     2  0.2216      0.890 0.000 0.908 0.092 0.000
#&gt; GSM892385     2  0.1302      0.900 0.000 0.956 0.044 0.000
#&gt; GSM892389     2  0.2081      0.892 0.000 0.916 0.084 0.000
#&gt; GSM892341     3  0.5160      0.911 0.136 0.000 0.760 0.104
#&gt; GSM892346     1  0.4877     -0.661 0.592 0.000 0.000 0.408
#&gt; GSM892350     1  0.4916     -0.706 0.576 0.000 0.000 0.424
#&gt; GSM892354     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892366     3  0.2868      0.931 0.136 0.000 0.864 0.000
#&gt; GSM892370     3  0.4514      0.930 0.136 0.000 0.800 0.064
#&gt; GSM892374     2  0.5000      0.596 0.000 0.504 0.000 0.496
#&gt; GSM892378     2  0.2345      0.889 0.000 0.900 0.100 0.000
#&gt; GSM892382     2  0.1022      0.899 0.000 0.968 0.032 0.000
#&gt; GSM892384     2  0.0592      0.900 0.000 0.984 0.016 0.000
#&gt; GSM892388     2  0.5000      0.596 0.000 0.504 0.000 0.496
#&gt; GSM892343     1  0.7824     -0.325 0.404 0.000 0.328 0.268
#&gt; GSM892348     1  0.4916     -0.706 0.576 0.000 0.000 0.424
#&gt; GSM892352     4  0.5168      0.000 0.496 0.000 0.004 0.500
#&gt; GSM892358     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      0.596 1.000 0.000 0.000 0.000
#&gt; GSM892368     3  0.2868      0.931 0.136 0.000 0.864 0.000
#&gt; GSM892372     3  0.2868      0.931 0.136 0.000 0.864 0.000
#&gt; GSM892376     2  0.1022      0.899 0.000 0.968 0.032 0.000
#&gt; GSM892380     2  0.1716      0.897 0.000 0.936 0.064 0.000
#&gt; GSM892386     2  0.0707      0.900 0.000 0.980 0.020 0.000
#&gt; GSM892390     2  0.2081      0.892 0.000 0.916 0.084 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     3  0.6353      0.698 0.008 0.000 0.556 0.180 0.256
#&gt; GSM892345     4  0.2179      0.823 0.100 0.000 0.000 0.896 0.004
#&gt; GSM892349     4  0.1082      0.789 0.000 0.000 0.008 0.964 0.028
#&gt; GSM892353     1  0.4763      0.880 0.632 0.000 0.000 0.336 0.032
#&gt; GSM892355     1  0.4475      0.967 0.692 0.000 0.000 0.276 0.032
#&gt; GSM892361     1  0.3814      0.976 0.720 0.000 0.000 0.276 0.004
#&gt; GSM892365     3  0.1928      0.853 0.004 0.000 0.920 0.072 0.004
#&gt; GSM892369     3  0.3900      0.851 0.008 0.000 0.816 0.108 0.068
#&gt; GSM892373     5  0.3913      0.990 0.000 0.324 0.000 0.000 0.676
#&gt; GSM892377     2  0.3993      0.784 0.216 0.756 0.028 0.000 0.000
#&gt; GSM892381     2  0.0912      0.851 0.012 0.972 0.016 0.000 0.000
#&gt; GSM892383     2  0.2580      0.845 0.064 0.892 0.044 0.000 0.000
#&gt; GSM892387     2  0.0451      0.855 0.004 0.988 0.008 0.000 0.000
#&gt; GSM892344     3  0.6952      0.465 0.008 0.000 0.408 0.328 0.256
#&gt; GSM892347     4  0.2179      0.823 0.100 0.000 0.000 0.896 0.004
#&gt; GSM892351     4  0.0992      0.809 0.024 0.000 0.000 0.968 0.008
#&gt; GSM892357     1  0.4229      0.969 0.704 0.000 0.000 0.276 0.020
#&gt; GSM892359     1  0.3661      0.977 0.724 0.000 0.000 0.276 0.000
#&gt; GSM892363     1  0.4138      0.974 0.708 0.000 0.000 0.276 0.016
#&gt; GSM892367     3  0.1608      0.854 0.000 0.000 0.928 0.072 0.000
#&gt; GSM892371     3  0.3900      0.851 0.008 0.000 0.816 0.108 0.068
#&gt; GSM892375     2  0.0693      0.851 0.012 0.980 0.008 0.000 0.000
#&gt; GSM892379     2  0.3395      0.783 0.236 0.764 0.000 0.000 0.000
#&gt; GSM892385     2  0.3003      0.842 0.092 0.864 0.044 0.000 0.000
#&gt; GSM892389     2  0.3282      0.802 0.188 0.804 0.008 0.000 0.000
#&gt; GSM892341     3  0.6353      0.698 0.008 0.000 0.556 0.180 0.256
#&gt; GSM892346     4  0.2389      0.805 0.116 0.000 0.000 0.880 0.004
#&gt; GSM892350     4  0.2020      0.823 0.100 0.000 0.000 0.900 0.000
#&gt; GSM892354     1  0.4229      0.969 0.704 0.000 0.000 0.276 0.020
#&gt; GSM892356     1  0.3661      0.977 0.724 0.000 0.000 0.276 0.000
#&gt; GSM892362     1  0.4138      0.974 0.708 0.000 0.000 0.276 0.016
#&gt; GSM892366     3  0.1928      0.853 0.004 0.000 0.920 0.072 0.004
#&gt; GSM892370     3  0.3900      0.851 0.008 0.000 0.816 0.108 0.068
#&gt; GSM892374     5  0.3913      0.990 0.000 0.324 0.000 0.000 0.676
#&gt; GSM892378     2  0.3424      0.781 0.240 0.760 0.000 0.000 0.000
#&gt; GSM892382     2  0.0912      0.851 0.012 0.972 0.016 0.000 0.000
#&gt; GSM892384     2  0.0290      0.855 0.000 0.992 0.008 0.000 0.000
#&gt; GSM892388     5  0.4812      0.980 0.012 0.324 0.012 0.004 0.648
#&gt; GSM892343     4  0.6808     -0.144 0.008 0.000 0.268 0.468 0.256
#&gt; GSM892348     4  0.2233      0.820 0.104 0.000 0.000 0.892 0.004
#&gt; GSM892352     4  0.1082      0.789 0.000 0.000 0.008 0.964 0.028
#&gt; GSM892358     1  0.3661      0.977 0.724 0.000 0.000 0.276 0.000
#&gt; GSM892360     1  0.3661      0.977 0.724 0.000 0.000 0.276 0.000
#&gt; GSM892364     1  0.4138      0.974 0.708 0.000 0.000 0.276 0.016
#&gt; GSM892368     3  0.1608      0.854 0.000 0.000 0.928 0.072 0.000
#&gt; GSM892372     3  0.2006      0.854 0.000 0.000 0.916 0.072 0.012
#&gt; GSM892376     2  0.0579      0.852 0.008 0.984 0.008 0.000 0.000
#&gt; GSM892380     2  0.3146      0.832 0.128 0.844 0.028 0.000 0.000
#&gt; GSM892386     2  0.0290      0.854 0.000 0.992 0.008 0.000 0.000
#&gt; GSM892390     2  0.3282      0.802 0.188 0.804 0.008 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.5422     0.1256 0.024 0.000 0.316 0.080 0.580 0.000
#&gt; GSM892345     4  0.2597     0.9196 0.176 0.000 0.000 0.824 0.000 0.000
#&gt; GSM892349     4  0.4287     0.8571 0.108 0.000 0.000 0.768 0.028 0.096
#&gt; GSM892353     1  0.3610     0.8378 0.792 0.000 0.000 0.052 0.004 0.152
#&gt; GSM892355     1  0.2340     0.8939 0.852 0.000 0.000 0.000 0.000 0.148
#&gt; GSM892361     1  0.1141     0.9456 0.948 0.000 0.000 0.000 0.000 0.052
#&gt; GSM892365     3  0.1088     0.8936 0.024 0.000 0.960 0.000 0.000 0.016
#&gt; GSM892369     3  0.4416     0.8530 0.024 0.000 0.776 0.016 0.096 0.088
#&gt; GSM892373     5  0.6049     0.0991 0.000 0.332 0.000 0.000 0.404 0.264
#&gt; GSM892377     2  0.1606     0.7628 0.000 0.932 0.004 0.056 0.000 0.008
#&gt; GSM892381     2  0.3850     0.8343 0.000 0.652 0.004 0.004 0.000 0.340
#&gt; GSM892383     2  0.4134     0.8348 0.000 0.708 0.000 0.052 0.000 0.240
#&gt; GSM892387     2  0.4009     0.8379 0.000 0.684 0.000 0.028 0.000 0.288
#&gt; GSM892344     5  0.5858     0.2254 0.028 0.000 0.228 0.164 0.580 0.000
#&gt; GSM892347     4  0.2597     0.9196 0.176 0.000 0.000 0.824 0.000 0.000
#&gt; GSM892351     4  0.3915     0.8865 0.128 0.000 0.000 0.776 0.004 0.092
#&gt; GSM892357     1  0.1387     0.9365 0.932 0.000 0.000 0.000 0.000 0.068
#&gt; GSM892359     1  0.0000     0.9481 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0937     0.9436 0.960 0.000 0.000 0.000 0.000 0.040
#&gt; GSM892367     3  0.0632     0.8961 0.024 0.000 0.976 0.000 0.000 0.000
#&gt; GSM892371     3  0.4416     0.8530 0.024 0.000 0.776 0.016 0.096 0.088
#&gt; GSM892375     2  0.3758     0.8356 0.000 0.668 0.000 0.008 0.000 0.324
#&gt; GSM892379     2  0.1003     0.7583 0.000 0.964 0.004 0.028 0.000 0.004
#&gt; GSM892385     2  0.3894     0.8328 0.000 0.740 0.004 0.036 0.000 0.220
#&gt; GSM892389     2  0.0984     0.7663 0.000 0.968 0.012 0.012 0.000 0.008
#&gt; GSM892341     5  0.5422     0.1256 0.024 0.000 0.316 0.080 0.580 0.000
#&gt; GSM892346     4  0.2838     0.9112 0.188 0.000 0.000 0.808 0.000 0.004
#&gt; GSM892350     4  0.3695     0.9153 0.176 0.000 0.000 0.776 0.004 0.044
#&gt; GSM892354     1  0.1444     0.9369 0.928 0.000 0.000 0.000 0.000 0.072
#&gt; GSM892356     1  0.0000     0.9481 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.1327     0.9431 0.936 0.000 0.000 0.000 0.000 0.064
#&gt; GSM892366     3  0.1088     0.8936 0.024 0.000 0.960 0.000 0.000 0.016
#&gt; GSM892370     3  0.4416     0.8530 0.024 0.000 0.776 0.016 0.096 0.088
#&gt; GSM892374     5  0.6049     0.0991 0.000 0.332 0.000 0.000 0.404 0.264
#&gt; GSM892378     2  0.0713     0.7602 0.000 0.972 0.000 0.028 0.000 0.000
#&gt; GSM892382     2  0.3850     0.8343 0.000 0.652 0.004 0.004 0.000 0.340
#&gt; GSM892384     2  0.4042     0.8376 0.000 0.664 0.004 0.016 0.000 0.316
#&gt; GSM892388     5  0.6410     0.0961 0.000 0.332 0.004 0.008 0.384 0.272
#&gt; GSM892343     5  0.6283     0.2458 0.088 0.000 0.160 0.172 0.580 0.000
#&gt; GSM892348     4  0.2631     0.9180 0.180 0.000 0.000 0.820 0.000 0.000
#&gt; GSM892352     4  0.4315     0.8527 0.104 0.000 0.000 0.768 0.032 0.096
#&gt; GSM892358     1  0.0547     0.9467 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM892360     1  0.0000     0.9481 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0937     0.9436 0.960 0.000 0.000 0.000 0.000 0.040
#&gt; GSM892368     3  0.0632     0.8961 0.024 0.000 0.976 0.000 0.000 0.000
#&gt; GSM892372     3  0.2209     0.8896 0.024 0.000 0.900 0.000 0.004 0.072
#&gt; GSM892376     2  0.3784     0.8359 0.000 0.680 0.000 0.012 0.000 0.308
#&gt; GSM892380     2  0.3254     0.8129 0.000 0.820 0.000 0.056 0.000 0.124
#&gt; GSM892386     2  0.3816     0.8373 0.000 0.688 0.000 0.016 0.000 0.296
#&gt; GSM892390     2  0.1078     0.7667 0.000 0.964 0.012 0.016 0.000 0.008
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> ATC:kmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> ATC:kmeans 50     0.933            0.869 1.06e-11      4.14e-05 3
#> ATC:kmeans 41     0.967            0.954 7.24e-11      1.02e-04 4
#> ATC:kmeans 48     0.982            0.532 5.15e-16      6.33e-08 5
#> ATC:kmeans 43     0.998            0.998 2.92e-15      1.90e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           0.984       0.994         0.3946 0.811   0.644
#> 4 4 1.000           0.890       0.956         0.1358 0.882   0.670
#> 5 5 1.000           0.990       0.993         0.0341 0.971   0.888
#> 6 6 0.982           0.930       0.946         0.0143 0.990   0.957
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892345     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892349     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892353     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892355     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892361     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892365     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892369     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.5591      0.562 0.304  0 0.696
#&gt; GSM892347     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892351     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892357     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892359     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892363     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892367     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892371     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892346     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892350     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892354     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892356     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892362     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892366     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892370     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     1  0.0592      0.987 0.988  0 0.012
#&gt; GSM892348     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892352     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892358     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892360     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892364     1  0.0000      0.999 1.000  0 0.000
#&gt; GSM892368     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892372     3  0.0000      0.968 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3  0.5000      0.945 0.000  0 0.500 0.500
#&gt; GSM892345     4  0.5925      0.787 0.036  0 0.452 0.512
#&gt; GSM892349     4  0.5268      0.782 0.008  0 0.452 0.540
#&gt; GSM892353     1  0.0817      0.973 0.976  0 0.000 0.024
#&gt; GSM892355     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892361     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892365     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892369     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     4  0.0707      0.140 0.000  0 0.020 0.980
#&gt; GSM892347     4  0.5925      0.787 0.036  0 0.452 0.512
#&gt; GSM892351     4  0.5383      0.783 0.012  0 0.452 0.536
#&gt; GSM892357     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892359     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892363     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892367     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892371     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     4  0.5000     -0.956 0.000  0 0.500 0.500
#&gt; GSM892346     4  0.6332      0.774 0.060  0 0.452 0.488
#&gt; GSM892350     4  0.5925      0.787 0.036  0 0.452 0.512
#&gt; GSM892354     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892356     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892362     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892366     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892370     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     4  0.0000      0.194 0.000  0 0.000 1.000
#&gt; GSM892348     4  0.6071      0.784 0.044  0 0.452 0.504
#&gt; GSM892352     4  0.4967      0.777 0.000  0 0.452 0.548
#&gt; GSM892358     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892360     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892364     1  0.0000      0.998 1.000  0 0.000 0.000
#&gt; GSM892368     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892372     3  0.4967      0.993 0.000  0 0.548 0.452
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM892342     5  0.0000      1.000 0.000  0 0.000 0.000 1.000
#&gt; GSM892345     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892349     4  0.0404      0.987 0.000  0 0.000 0.988 0.012
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892365     3  0.0000      0.954 0.000  0 1.000 0.000 0.000
#&gt; GSM892369     3  0.1965      0.931 0.000  0 0.904 0.000 0.096
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892344     5  0.0000      1.000 0.000  0 0.000 0.000 1.000
#&gt; GSM892347     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892351     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892367     3  0.0290      0.957 0.000  0 0.992 0.000 0.008
#&gt; GSM892371     3  0.1965      0.931 0.000  0 0.904 0.000 0.096
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892341     5  0.0000      1.000 0.000  0 0.000 0.000 1.000
#&gt; GSM892346     4  0.0162      0.991 0.004  0 0.000 0.996 0.000
#&gt; GSM892350     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892366     3  0.0000      0.954 0.000  0 1.000 0.000 0.000
#&gt; GSM892370     3  0.1965      0.931 0.000  0 0.904 0.000 0.096
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892343     5  0.0000      1.000 0.000  0 0.000 0.000 1.000
#&gt; GSM892348     4  0.0000      0.994 0.000  0 0.000 1.000 0.000
#&gt; GSM892352     4  0.0794      0.974 0.000  0 0.000 0.972 0.028
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000 0.000 0.000
#&gt; GSM892368     3  0.0290      0.957 0.000  0 0.992 0.000 0.008
#&gt; GSM892372     3  0.0510      0.957 0.000  0 0.984 0.000 0.016
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892345     4  0.0000      0.933 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892349     4  0.3068      0.867 0.000 0.000 0.008 0.840 0.032 0.120
#&gt; GSM892353     1  0.0603      0.981 0.980 0.000 0.000 0.000 0.016 0.004
#&gt; GSM892355     1  0.0146      0.996 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM892361     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892365     6  0.2762      0.993 0.000 0.000 0.196 0.000 0.000 0.804
#&gt; GSM892369     3  0.0458      0.685 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM892373     2  0.0146      0.997 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892377     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892381     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892383     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892387     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892344     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892347     4  0.0363      0.932 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; GSM892351     4  0.1075      0.925 0.000 0.000 0.000 0.952 0.000 0.048
#&gt; GSM892357     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3  0.3684      0.430 0.000 0.000 0.628 0.000 0.000 0.372
#&gt; GSM892371     3  0.0458      0.685 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM892375     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892379     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892385     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892389     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892341     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892346     4  0.0777      0.928 0.004 0.000 0.000 0.972 0.000 0.024
#&gt; GSM892350     4  0.0547      0.931 0.000 0.000 0.000 0.980 0.000 0.020
#&gt; GSM892354     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     6  0.2793      0.993 0.000 0.000 0.200 0.000 0.000 0.800
#&gt; GSM892370     3  0.0458      0.685 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM892374     2  0.0146      0.997 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892378     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892382     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892384     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892388     2  0.0146      0.997 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM892343     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892348     4  0.0632      0.929 0.000 0.000 0.000 0.976 0.000 0.024
#&gt; GSM892352     4  0.4585      0.770 0.000 0.000 0.016 0.724 0.096 0.164
#&gt; GSM892358     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1  0.0000      0.998 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1  0.0146      0.996 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM892368     3  0.3782      0.329 0.000 0.000 0.588 0.000 0.000 0.412
#&gt; GSM892372     3  0.3426      0.560 0.000 0.000 0.720 0.000 0.004 0.276
#&gt; GSM892376     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892380     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892386     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892390     2  0.0000      0.999 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) disease.state(p) other(p) individual(p) k
#> ATC:skmeans 50     1.000            0.934 2.67e-07      1.42e-03 2
#> ATC:skmeans 50     0.933            0.869 1.06e-11      4.14e-05 3
#> ATC:skmeans 47     0.993            0.986 2.58e-16      7.31e-07 4
#> ATC:skmeans 50     1.000            0.998 3.76e-23      2.67e-09 5
#> ATC:skmeans 48     1.000            0.863 1.22e-26      7.95e-11 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.974           0.913       0.968         0.4091 0.811   0.644
#> 4 4 1.000           0.986       0.994         0.1247 0.900   0.714
#> 5 5 0.926           0.880       0.909         0.0446 0.945   0.792
#> 6 6 0.969           0.937       0.968         0.0397 0.971   0.869
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4 5
```

There is also optional best $k$ = 2 3 4 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892345     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892349     1   0.604     0.3948 0.620  0 0.380
#&gt; GSM892353     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892355     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892361     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892365     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892369     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892373     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892377     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892381     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892383     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892387     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892344     3   0.588     0.3873 0.348  0 0.652
#&gt; GSM892347     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892351     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892357     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892359     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892363     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892367     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892371     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892375     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892379     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892385     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892389     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892341     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892346     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892350     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892354     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892356     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892362     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892366     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892370     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892374     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892378     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892382     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892384     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892388     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892343     1   0.630     0.0864 0.516  0 0.484
#&gt; GSM892348     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892352     1   0.606     0.3855 0.616  0 0.384
#&gt; GSM892358     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892360     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892364     1   0.000     0.9320 1.000  0 0.000
#&gt; GSM892368     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892372     3   0.000     0.9607 0.000  0 1.000
#&gt; GSM892376     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892380     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892386     2   0.000     1.0000 0.000  1 0.000
#&gt; GSM892390     2   0.000     1.0000 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4
#&gt; GSM892342     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892345     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892349     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892353     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892355     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892361     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892365     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892369     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892373     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892344     4   0.189      0.934 0.008  0 0.056 0.936
#&gt; GSM892347     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892351     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892357     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892359     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892363     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892367     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892371     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892341     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892346     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892350     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892354     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892356     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892362     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892366     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892370     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892374     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892388     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892343     1   0.511      0.678 0.740  0 0.056 0.204
#&gt; GSM892348     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892352     4   0.000      0.992 0.000  0 0.000 1.000
#&gt; GSM892358     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892360     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892364     1   0.000      0.979 1.000  0 0.000 0.000
#&gt; GSM892368     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892372     3   0.000      1.000 0.000  0 1.000 0.000
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000 0.000
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM892342     3   0.000      0.497 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892345     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892349     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892353     1   0.622      0.626 0.520 0.164 0.316 0.000 0.000
#&gt; GSM892355     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892361     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892365     3   0.430      0.820 0.000 0.000 0.520 0.000 0.480
#&gt; GSM892369     3   0.430      0.821 0.000 0.000 0.524 0.000 0.476
#&gt; GSM892373     5   0.430      1.000 0.480 0.000 0.000 0.000 0.520
#&gt; GSM892377     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892381     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892383     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892387     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892344     4   0.456      0.485 0.008 0.000 0.496 0.496 0.000
#&gt; GSM892347     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892351     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892357     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892359     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892363     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892367     3   0.430      0.821 0.000 0.000 0.524 0.000 0.476
#&gt; GSM892371     3   0.430      0.821 0.000 0.000 0.524 0.000 0.476
#&gt; GSM892375     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892379     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892385     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892389     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892341     3   0.000      0.497 0.000 0.000 1.000 0.000 0.000
#&gt; GSM892346     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892350     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892354     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892356     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892362     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892366     3   0.430      0.820 0.000 0.000 0.520 0.000 0.480
#&gt; GSM892370     3   0.430      0.821 0.000 0.000 0.524 0.000 0.476
#&gt; GSM892374     5   0.430      1.000 0.480 0.000 0.000 0.000 0.520
#&gt; GSM892378     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892382     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892384     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892388     5   0.430      1.000 0.480 0.000 0.000 0.000 0.520
#&gt; GSM892343     3   0.517     -0.385 0.464 0.000 0.496 0.040 0.000
#&gt; GSM892348     4   0.000      0.905 0.000 0.000 0.000 1.000 0.000
#&gt; GSM892352     4   0.391      0.677 0.000 0.000 0.324 0.676 0.000
#&gt; GSM892358     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892360     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892364     1   0.430      0.972 0.520 0.480 0.000 0.000 0.000
#&gt; GSM892368     3   0.430      0.820 0.000 0.000 0.520 0.000 0.480
#&gt; GSM892372     3   0.430      0.821 0.000 0.000 0.524 0.000 0.476
#&gt; GSM892376     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892380     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892386     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
#&gt; GSM892390     2   0.430      1.000 0.480 0.520 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3    p4    p5 p6
#&gt; GSM892342     5   0.000      1.000 0.000  0 0.000 0.000 1.000  0
#&gt; GSM892345     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892349     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892353     1   0.374      0.355 0.608  0 0.000 0.000 0.392  0
#&gt; GSM892355     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892361     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892365     3   0.000      0.880 0.000  0 1.000 0.000 0.000  0
#&gt; GSM892369     3   0.253      0.908 0.000  0 0.832 0.000 0.168  0
#&gt; GSM892373     6   0.000      1.000 0.000  0 0.000 0.000 0.000  1
#&gt; GSM892377     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892381     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892383     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892387     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892344     5   0.000      1.000 0.000  0 0.000 0.000 1.000  0
#&gt; GSM892347     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892351     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892357     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892359     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892363     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892367     3   0.186      0.907 0.000  0 0.896 0.000 0.104  0
#&gt; GSM892371     3   0.253      0.908 0.000  0 0.832 0.000 0.168  0
#&gt; GSM892375     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892379     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892385     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892389     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892341     5   0.000      1.000 0.000  0 0.000 0.000 1.000  0
#&gt; GSM892346     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892350     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892354     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892356     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892362     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892366     3   0.000      0.880 0.000  0 1.000 0.000 0.000  0
#&gt; GSM892370     3   0.253      0.908 0.000  0 0.832 0.000 0.168  0
#&gt; GSM892374     6   0.000      1.000 0.000  0 0.000 0.000 0.000  1
#&gt; GSM892378     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892382     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892384     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892388     6   0.000      1.000 0.000  0 0.000 0.000 0.000  1
#&gt; GSM892343     5   0.000      1.000 0.000  0 0.000 0.000 1.000  0
#&gt; GSM892348     4   0.000      0.932 0.000  0 0.000 1.000 0.000  0
#&gt; GSM892352     4   0.382      0.230 0.000  0 0.000 0.564 0.436  0
#&gt; GSM892358     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892360     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892364     1   0.000      0.962 1.000  0 0.000 0.000 0.000  0
#&gt; GSM892368     3   0.000      0.880 0.000  0 1.000 0.000 0.000  0
#&gt; GSM892372     3   0.253      0.908 0.000  0 0.832 0.000 0.168  0
#&gt; GSM892376     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892380     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892386     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
#&gt; GSM892390     2   0.000      1.000 0.000  1 0.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) disease.state(p) other(p) individual(p) k
#> ATC:pam 50     1.000            0.934 2.67e-07      1.42e-03 2
#> ATC:pam 46     1.000            0.871 3.31e-11      6.62e-05 3
#> ATC:pam 50     0.979            0.866 8.76e-16      7.14e-07 4
#> ATC:pam 46     0.982            0.560 3.62e-16      1.08e-07 5
#> ATC:pam 48     0.986            0.666 6.76e-21      5.72e-09 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 1.000           1.000       1.000         0.4159 0.804   0.630
#> 4 4 1.000           1.000       1.000         0.1174 0.922   0.765
#> 5 5 1.000           0.995       0.995         0.0381 0.971   0.888
#> 6 6 0.989           0.950       0.956         0.0268 0.990   0.957
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1 p2    p3
#&gt; GSM892342     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892345     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892349     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892353     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892355     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892361     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892365     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892369     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892373     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892377     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892381     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892383     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892387     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892344     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892347     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892351     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892357     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892359     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892363     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892367     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892371     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892375     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892379     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892385     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892389     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892341     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892346     3  0.0424      0.992 0.008  0 0.992
#&gt; GSM892350     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892354     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892356     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892362     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892366     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892370     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892374     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892378     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892382     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892384     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892388     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892343     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892348     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892352     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892358     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892360     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892364     1  0.0000      1.000 1.000  0 0.000
#&gt; GSM892368     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892372     3  0.0000      1.000 0.000  0 1.000
#&gt; GSM892376     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892380     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892386     2  0.0000      1.000 0.000  1 0.000
#&gt; GSM892390     2  0.0000      1.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2 p3 p4
#&gt; GSM892342     3       0          1  0  0  1  0
#&gt; GSM892345     4       0          1  0  0  0  1
#&gt; GSM892349     4       0          1  0  0  0  1
#&gt; GSM892353     1       0          1  1  0  0  0
#&gt; GSM892355     1       0          1  1  0  0  0
#&gt; GSM892361     1       0          1  1  0  0  0
#&gt; GSM892365     3       0          1  0  0  1  0
#&gt; GSM892369     3       0          1  0  0  1  0
#&gt; GSM892373     2       0          1  0  1  0  0
#&gt; GSM892377     2       0          1  0  1  0  0
#&gt; GSM892381     2       0          1  0  1  0  0
#&gt; GSM892383     2       0          1  0  1  0  0
#&gt; GSM892387     2       0          1  0  1  0  0
#&gt; GSM892344     3       0          1  0  0  1  0
#&gt; GSM892347     4       0          1  0  0  0  1
#&gt; GSM892351     4       0          1  0  0  0  1
#&gt; GSM892357     1       0          1  1  0  0  0
#&gt; GSM892359     1       0          1  1  0  0  0
#&gt; GSM892363     1       0          1  1  0  0  0
#&gt; GSM892367     3       0          1  0  0  1  0
#&gt; GSM892371     3       0          1  0  0  1  0
#&gt; GSM892375     2       0          1  0  1  0  0
#&gt; GSM892379     2       0          1  0  1  0  0
#&gt; GSM892385     2       0          1  0  1  0  0
#&gt; GSM892389     2       0          1  0  1  0  0
#&gt; GSM892341     3       0          1  0  0  1  0
#&gt; GSM892346     4       0          1  0  0  0  1
#&gt; GSM892350     4       0          1  0  0  0  1
#&gt; GSM892354     1       0          1  1  0  0  0
#&gt; GSM892356     1       0          1  1  0  0  0
#&gt; GSM892362     1       0          1  1  0  0  0
#&gt; GSM892366     3       0          1  0  0  1  0
#&gt; GSM892370     3       0          1  0  0  1  0
#&gt; GSM892374     2       0          1  0  1  0  0
#&gt; GSM892378     2       0          1  0  1  0  0
#&gt; GSM892382     2       0          1  0  1  0  0
#&gt; GSM892384     2       0          1  0  1  0  0
#&gt; GSM892388     2       0          1  0  1  0  0
#&gt; GSM892343     3       0          1  0  0  1  0
#&gt; GSM892348     4       0          1  0  0  0  1
#&gt; GSM892352     4       0          1  0  0  0  1
#&gt; GSM892358     1       0          1  1  0  0  0
#&gt; GSM892360     1       0          1  1  0  0  0
#&gt; GSM892364     1       0          1  1  0  0  0
#&gt; GSM892368     3       0          1  0  0  1  0
#&gt; GSM892372     3       0          1  0  0  1  0
#&gt; GSM892376     2       0          1  0  1  0  0
#&gt; GSM892380     2       0          1  0  1  0  0
#&gt; GSM892386     2       0          1  0  1  0  0
#&gt; GSM892390     2       0          1  0  1  0  0
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1    p2    p3 p4    p5
#&gt; GSM892342     5  0.0290      0.997  0 0.000 0.008  0 0.992
#&gt; GSM892345     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892349     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892353     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892355     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892361     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892365     3  0.0609      0.985  0 0.000 0.980  0 0.020
#&gt; GSM892369     5  0.0290      0.997  0 0.000 0.008  0 0.992
#&gt; GSM892373     2  0.0510      0.987  0 0.984 0.016  0 0.000
#&gt; GSM892377     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892381     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892383     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892387     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892344     5  0.0000      0.993  0 0.000 0.000  0 1.000
#&gt; GSM892347     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892351     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892357     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892359     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892363     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892367     3  0.0609      0.985  0 0.000 0.980  0 0.020
#&gt; GSM892371     5  0.0290      0.997  0 0.000 0.008  0 0.992
#&gt; GSM892375     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892379     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892385     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892389     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892341     5  0.0290      0.997  0 0.000 0.008  0 0.992
#&gt; GSM892346     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892350     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892354     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892356     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892362     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892366     3  0.0609      0.985  0 0.000 0.980  0 0.020
#&gt; GSM892370     5  0.0290      0.997  0 0.000 0.008  0 0.992
#&gt; GSM892374     2  0.0510      0.987  0 0.984 0.016  0 0.000
#&gt; GSM892378     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892382     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892384     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892388     2  0.0609      0.984  0 0.980 0.020  0 0.000
#&gt; GSM892343     5  0.0000      0.993  0 0.000 0.000  0 1.000
#&gt; GSM892348     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892352     4  0.0000      1.000  0 0.000 0.000  1 0.000
#&gt; GSM892358     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892360     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892364     1  0.0000      1.000  1 0.000 0.000  0 0.000
#&gt; GSM892368     3  0.0609      0.985  0 0.000 0.980  0 0.020
#&gt; GSM892372     3  0.1671      0.939  0 0.000 0.924  0 0.076
#&gt; GSM892376     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892380     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892386     2  0.0000      0.997  0 1.000 0.000  0 0.000
#&gt; GSM892390     2  0.0000      0.997  0 1.000 0.000  0 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1    p2    p3    p4    p5    p6
#&gt; GSM892342     5   0.385      0.993  0 0.000 0.000 0.000 0.544 0.456
#&gt; GSM892345     4   0.385      1.000  0 0.000 0.000 0.544 0.456 0.000
#&gt; GSM892349     4   0.385      1.000  0 0.000 0.000 0.544 0.456 0.000
#&gt; GSM892353     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892355     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892361     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892365     3   0.000      1.000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892369     5   0.416      0.988  0 0.000 0.012 0.000 0.544 0.444
#&gt; GSM892373     2   0.374      0.567  0 0.608 0.000 0.392 0.000 0.000
#&gt; GSM892377     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892381     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892383     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892387     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892344     5   0.385      0.992  0 0.000 0.000 0.000 0.536 0.464
#&gt; GSM892347     4   0.385      1.000  0 0.000 0.000 0.544 0.456 0.000
#&gt; GSM892351     4   0.385      1.000  0 0.000 0.000 0.544 0.456 0.000
#&gt; GSM892357     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892359     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892363     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892367     3   0.000      1.000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892371     5   0.385      0.993  0 0.000 0.000 0.000 0.544 0.456
#&gt; GSM892375     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892379     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892385     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892389     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892341     5   0.385      0.993  0 0.000 0.000 0.000 0.544 0.456
#&gt; GSM892346     6   0.408      1.000  0 0.000 0.000 0.008 0.456 0.536
#&gt; GSM892350     4   0.385      1.000  0 0.000 0.000 0.544 0.456 0.000
#&gt; GSM892354     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892356     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892362     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892366     3   0.000      1.000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892370     5   0.416      0.988  0 0.000 0.012 0.000 0.544 0.444
#&gt; GSM892374     2   0.374      0.567  0 0.608 0.000 0.392 0.000 0.000
#&gt; GSM892378     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892382     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892384     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892388     2   0.385      0.477  0 0.544 0.000 0.456 0.000 0.000
#&gt; GSM892343     5   0.385      0.992  0 0.000 0.000 0.000 0.536 0.464
#&gt; GSM892348     6   0.408      1.000  0 0.000 0.000 0.008 0.456 0.536
#&gt; GSM892352     4   0.385      1.000  0 0.000 0.000 0.544 0.456 0.000
#&gt; GSM892358     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892360     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892364     1   0.000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; GSM892368     3   0.000      1.000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892372     3   0.000      1.000  0 0.000 1.000 0.000 0.000 0.000
#&gt; GSM892376     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892380     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892386     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
#&gt; GSM892390     2   0.000      0.930  0 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) disease.state(p) other(p) individual(p) k
#> ATC:mclust 50     1.000            0.934 2.67e-07      1.42e-03 2
#> ATC:mclust 50     1.000            0.931 1.26e-12      1.60e-05 3
#> ATC:mclust 50     1.000            0.986 6.71e-18      2.02e-07 4
#> ATC:mclust 50     0.987            0.975 6.70e-21      2.57e-08 5
#> ATC:mclust 49     0.692            0.996 8.70e-20      2.73e-06 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 31632 rows and 50 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           1.000       1.000         0.4707 0.530   0.530
#> 3 3 0.809           0.964       0.927         0.3206 0.804   0.630
#> 4 4 0.869           0.883       0.867         0.1320 0.919   0.758
#> 5 5 0.846           0.904       0.897         0.0439 0.984   0.941
#> 6 6 0.807           0.860       0.887         0.0263 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette p1 p2
#&gt; GSM892342     1       0          1  1  0
#&gt; GSM892345     1       0          1  1  0
#&gt; GSM892349     1       0          1  1  0
#&gt; GSM892353     1       0          1  1  0
#&gt; GSM892355     1       0          1  1  0
#&gt; GSM892361     1       0          1  1  0
#&gt; GSM892365     1       0          1  1  0
#&gt; GSM892369     1       0          1  1  0
#&gt; GSM892373     2       0          1  0  1
#&gt; GSM892377     2       0          1  0  1
#&gt; GSM892381     2       0          1  0  1
#&gt; GSM892383     2       0          1  0  1
#&gt; GSM892387     2       0          1  0  1
#&gt; GSM892344     1       0          1  1  0
#&gt; GSM892347     1       0          1  1  0
#&gt; GSM892351     1       0          1  1  0
#&gt; GSM892357     1       0          1  1  0
#&gt; GSM892359     1       0          1  1  0
#&gt; GSM892363     1       0          1  1  0
#&gt; GSM892367     1       0          1  1  0
#&gt; GSM892371     1       0          1  1  0
#&gt; GSM892375     2       0          1  0  1
#&gt; GSM892379     2       0          1  0  1
#&gt; GSM892385     2       0          1  0  1
#&gt; GSM892389     2       0          1  0  1
#&gt; GSM892341     1       0          1  1  0
#&gt; GSM892346     1       0          1  1  0
#&gt; GSM892350     1       0          1  1  0
#&gt; GSM892354     1       0          1  1  0
#&gt; GSM892356     1       0          1  1  0
#&gt; GSM892362     1       0          1  1  0
#&gt; GSM892366     1       0          1  1  0
#&gt; GSM892370     1       0          1  1  0
#&gt; GSM892374     2       0          1  0  1
#&gt; GSM892378     2       0          1  0  1
#&gt; GSM892382     2       0          1  0  1
#&gt; GSM892384     2       0          1  0  1
#&gt; GSM892388     2       0          1  0  1
#&gt; GSM892343     1       0          1  1  0
#&gt; GSM892348     1       0          1  1  0
#&gt; GSM892352     1       0          1  1  0
#&gt; GSM892358     1       0          1  1  0
#&gt; GSM892360     1       0          1  1  0
#&gt; GSM892364     1       0          1  1  0
#&gt; GSM892368     1       0          1  1  0
#&gt; GSM892372     1       0          1  1  0
#&gt; GSM892376     2       0          1  0  1
#&gt; GSM892380     2       0          1  0  1
#&gt; GSM892386     2       0          1  0  1
#&gt; GSM892390     2       0          1  0  1
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM892342     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892345     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892349     1  0.5363      0.940 0.724 0.000 0.276
#&gt; GSM892353     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892355     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892361     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892365     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892369     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892373     2  0.0237      0.988 0.004 0.996 0.000
#&gt; GSM892377     2  0.0237      0.988 0.004 0.996 0.000
#&gt; GSM892381     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM892383     2  0.1031      0.981 0.024 0.976 0.000
#&gt; GSM892387     2  0.3619      0.901 0.136 0.864 0.000
#&gt; GSM892344     3  0.2448      0.876 0.076 0.000 0.924
#&gt; GSM892347     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892351     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892357     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892359     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892363     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892367     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892371     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892375     2  0.0592      0.985 0.012 0.988 0.000
#&gt; GSM892379     2  0.0237      0.988 0.004 0.996 0.000
#&gt; GSM892385     2  0.0237      0.988 0.004 0.996 0.000
#&gt; GSM892389     2  0.0592      0.986 0.012 0.988 0.000
#&gt; GSM892341     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892346     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892350     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892354     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892356     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892362     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892366     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892370     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892374     2  0.0237      0.988 0.004 0.996 0.000
#&gt; GSM892378     2  0.0424      0.988 0.008 0.992 0.000
#&gt; GSM892382     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM892384     2  0.0592      0.986 0.012 0.988 0.000
#&gt; GSM892388     2  0.0237      0.988 0.004 0.996 0.000
#&gt; GSM892343     3  0.4702      0.618 0.212 0.000 0.788
#&gt; GSM892348     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892352     1  0.6140      0.714 0.596 0.000 0.404
#&gt; GSM892358     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892360     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892364     1  0.5016      0.985 0.760 0.000 0.240
#&gt; GSM892368     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892372     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM892376     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM892380     2  0.1753      0.967 0.048 0.952 0.000
#&gt; GSM892386     2  0.0000      0.988 0.000 1.000 0.000
#&gt; GSM892390     2  0.0424      0.987 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM892342     3  0.1489      0.939 0.004 0.000 0.952 0.044
#&gt; GSM892345     4  0.2546      0.761 0.008 0.000 0.092 0.900
#&gt; GSM892349     4  0.3768      0.710 0.008 0.000 0.184 0.808
#&gt; GSM892353     1  0.6235      0.929 0.524 0.000 0.056 0.420
#&gt; GSM892355     1  0.5982      0.962 0.524 0.000 0.040 0.436
#&gt; GSM892361     1  0.5781      0.908 0.492 0.000 0.028 0.480
#&gt; GSM892365     3  0.0188      0.960 0.000 0.000 0.996 0.004
#&gt; GSM892369     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM892373     2  0.1398      0.971 0.040 0.956 0.000 0.004
#&gt; GSM892377     2  0.0188      0.988 0.000 0.996 0.000 0.004
#&gt; GSM892381     2  0.0376      0.987 0.004 0.992 0.000 0.004
#&gt; GSM892383     2  0.0188      0.988 0.000 0.996 0.000 0.004
#&gt; GSM892387     2  0.1256      0.978 0.028 0.964 0.000 0.008
#&gt; GSM892344     3  0.2861      0.876 0.016 0.000 0.888 0.096
#&gt; GSM892347     4  0.2197      0.761 0.004 0.000 0.080 0.916
#&gt; GSM892351     4  0.3217      0.757 0.012 0.000 0.128 0.860
#&gt; GSM892357     1  0.5894      0.964 0.536 0.000 0.036 0.428
#&gt; GSM892359     1  0.5821      0.965 0.536 0.000 0.032 0.432
#&gt; GSM892363     1  0.5838      0.961 0.524 0.000 0.032 0.444
#&gt; GSM892367     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM892371     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM892375     2  0.0376      0.988 0.004 0.992 0.000 0.004
#&gt; GSM892379     2  0.0376      0.987 0.004 0.992 0.000 0.004
#&gt; GSM892385     2  0.0524      0.987 0.008 0.988 0.000 0.004
#&gt; GSM892389     2  0.0376      0.987 0.004 0.992 0.000 0.004
#&gt; GSM892341     3  0.0895      0.953 0.004 0.000 0.976 0.020
#&gt; GSM892346     4  0.3959      0.594 0.092 0.000 0.068 0.840
#&gt; GSM892350     4  0.2197      0.761 0.004 0.000 0.080 0.916
#&gt; GSM892354     1  0.5808      0.959 0.544 0.000 0.032 0.424
#&gt; GSM892356     1  0.5833      0.964 0.528 0.000 0.032 0.440
#&gt; GSM892362     4  0.5862     -0.921 0.484 0.000 0.032 0.484
#&gt; GSM892366     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM892370     3  0.0592      0.956 0.000 0.000 0.984 0.016
#&gt; GSM892374     2  0.2002      0.959 0.044 0.936 0.000 0.020
#&gt; GSM892378     2  0.0524      0.987 0.008 0.988 0.000 0.004
#&gt; GSM892382     2  0.0188      0.988 0.004 0.996 0.000 0.000
#&gt; GSM892384     2  0.0779      0.985 0.016 0.980 0.000 0.004
#&gt; GSM892388     2  0.1297      0.977 0.016 0.964 0.000 0.020
#&gt; GSM892343     3  0.4370      0.733 0.044 0.000 0.800 0.156
#&gt; GSM892348     4  0.3198      0.701 0.040 0.000 0.080 0.880
#&gt; GSM892352     4  0.4163      0.686 0.020 0.000 0.188 0.792
#&gt; GSM892358     1  0.5888      0.961 0.540 0.000 0.036 0.424
#&gt; GSM892360     1  0.5853      0.944 0.508 0.000 0.032 0.460
#&gt; GSM892364     1  0.5982      0.962 0.524 0.000 0.040 0.436
#&gt; GSM892368     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM892372     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM892376     2  0.0524      0.988 0.008 0.988 0.000 0.004
#&gt; GSM892380     2  0.0524      0.987 0.008 0.988 0.000 0.004
#&gt; GSM892386     2  0.0376      0.988 0.004 0.992 0.000 0.004
#&gt; GSM892390     2  0.0657      0.987 0.012 0.984 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM892342     3  0.5560      0.724 0.060 0.000 0.716 0.132 NA
#&gt; GSM892345     4  0.4250      0.887 0.252 0.000 0.028 0.720 NA
#&gt; GSM892349     4  0.5813      0.831 0.200 0.000 0.116 0.660 NA
#&gt; GSM892353     1  0.3863      0.784 0.836 0.000 0.040 0.072 NA
#&gt; GSM892355     1  0.0867      0.953 0.976 0.000 0.008 0.008 NA
#&gt; GSM892361     1  0.1251      0.945 0.956 0.000 0.000 0.036 NA
#&gt; GSM892365     3  0.1041      0.879 0.004 0.000 0.964 0.000 NA
#&gt; GSM892369     3  0.2131      0.859 0.008 0.000 0.920 0.016 NA
#&gt; GSM892373     2  0.1851      0.950 0.000 0.912 0.000 0.000 NA
#&gt; GSM892377     2  0.0963      0.974 0.000 0.964 0.000 0.000 NA
#&gt; GSM892381     2  0.0609      0.976 0.000 0.980 0.000 0.000 NA
#&gt; GSM892383     2  0.1121      0.968 0.000 0.956 0.000 0.000 NA
#&gt; GSM892387     2  0.0880      0.973 0.000 0.968 0.000 0.000 NA
#&gt; GSM892344     3  0.6836      0.510 0.164 0.000 0.596 0.160 NA
#&gt; GSM892347     4  0.4245      0.883 0.236 0.000 0.020 0.736 NA
#&gt; GSM892351     4  0.5228      0.877 0.244 0.000 0.060 0.680 NA
#&gt; GSM892357     1  0.0404      0.960 0.988 0.000 0.000 0.012 NA
#&gt; GSM892359     1  0.1012      0.955 0.968 0.000 0.000 0.020 NA
#&gt; GSM892363     1  0.0451      0.961 0.988 0.000 0.000 0.008 NA
#&gt; GSM892367     3  0.0794      0.873 0.000 0.000 0.972 0.000 NA
#&gt; GSM892371     3  0.1869      0.870 0.012 0.000 0.936 0.016 NA
#&gt; GSM892375     2  0.0794      0.976 0.000 0.972 0.000 0.000 NA
#&gt; GSM892379     2  0.0510      0.975 0.000 0.984 0.000 0.000 NA
#&gt; GSM892385     2  0.1478      0.962 0.000 0.936 0.000 0.000 NA
#&gt; GSM892389     2  0.0609      0.976 0.000 0.980 0.000 0.000 NA
#&gt; GSM892341     3  0.3905      0.820 0.032 0.000 0.832 0.072 NA
#&gt; GSM892346     4  0.5331      0.748 0.344 0.000 0.008 0.600 NA
#&gt; GSM892350     4  0.4181      0.887 0.244 0.000 0.020 0.732 NA
#&gt; GSM892354     1  0.0807      0.958 0.976 0.000 0.000 0.012 NA
#&gt; GSM892356     1  0.0162      0.962 0.996 0.000 0.000 0.004 NA
#&gt; GSM892362     1  0.0703      0.957 0.976 0.000 0.000 0.024 NA
#&gt; GSM892366     3  0.0671      0.880 0.004 0.000 0.980 0.000 NA
#&gt; GSM892370     3  0.1200      0.877 0.016 0.000 0.964 0.012 NA
#&gt; GSM892374     2  0.1732      0.951 0.000 0.920 0.000 0.000 NA
#&gt; GSM892378     2  0.0609      0.977 0.000 0.980 0.000 0.000 NA
#&gt; GSM892382     2  0.0290      0.975 0.000 0.992 0.000 0.000 NA
#&gt; GSM892384     2  0.0671      0.976 0.000 0.980 0.000 0.004 NA
#&gt; GSM892388     2  0.1764      0.957 0.000 0.928 0.000 0.008 NA
#&gt; GSM892343     3  0.5324      0.642 0.204 0.000 0.696 0.080 NA
#&gt; GSM892348     4  0.4564      0.864 0.272 0.000 0.008 0.696 NA
#&gt; GSM892352     4  0.6182      0.766 0.164 0.000 0.152 0.644 NA
#&gt; GSM892358     1  0.0566      0.953 0.984 0.000 0.000 0.012 NA
#&gt; GSM892360     1  0.0798      0.957 0.976 0.000 0.000 0.016 NA
#&gt; GSM892364     1  0.0290      0.961 0.992 0.000 0.000 0.008 NA
#&gt; GSM892368     3  0.0671      0.880 0.004 0.000 0.980 0.000 NA
#&gt; GSM892372     3  0.0324      0.879 0.004 0.000 0.992 0.000 NA
#&gt; GSM892376     2  0.0880      0.974 0.000 0.968 0.000 0.000 NA
#&gt; GSM892380     2  0.0510      0.975 0.000 0.984 0.000 0.000 NA
#&gt; GSM892386     2  0.0880      0.974 0.000 0.968 0.000 0.000 NA
#&gt; GSM892390     2  0.0880      0.974 0.000 0.968 0.000 0.000 NA
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM892342     3  0.6071      0.501 0.040 0.000 0.500 0.112 NA 0.348
#&gt; GSM892345     4  0.3000      0.883 0.124 0.000 0.032 0.840 NA 0.004
#&gt; GSM892349     4  0.5417      0.782 0.112 0.000 0.100 0.684 NA 0.104
#&gt; GSM892353     1  0.3911      0.769 0.796 0.000 0.032 0.036 NA 0.132
#&gt; GSM892355     1  0.0964      0.959 0.968 0.000 0.004 0.016 NA 0.012
#&gt; GSM892361     1  0.0790      0.955 0.968 0.000 0.000 0.032 NA 0.000
#&gt; GSM892365     3  0.1296      0.797 0.000 0.000 0.948 0.004 NA 0.044
#&gt; GSM892369     3  0.3919      0.754 0.012 0.000 0.816 0.080 NA 0.056
#&gt; GSM892373     2  0.2450      0.907 0.000 0.868 0.000 0.000 NA 0.016
#&gt; GSM892377     2  0.1075      0.958 0.000 0.952 0.000 0.000 NA 0.000
#&gt; GSM892381     2  0.0858      0.962 0.000 0.968 0.000 0.000 NA 0.004
#&gt; GSM892383     2  0.1285      0.955 0.000 0.944 0.000 0.000 NA 0.004
#&gt; GSM892387     2  0.0713      0.963 0.000 0.972 0.000 0.000 NA 0.000
#&gt; GSM892344     3  0.7437      0.106 0.128 0.000 0.344 0.248 NA 0.280
#&gt; GSM892347     4  0.2764      0.876 0.100 0.000 0.028 0.864 NA 0.008
#&gt; GSM892351     4  0.3718      0.874 0.124 0.000 0.052 0.804 NA 0.020
#&gt; GSM892357     1  0.0748      0.962 0.976 0.000 0.000 0.016 NA 0.004
#&gt; GSM892359     1  0.0603      0.960 0.980 0.000 0.000 0.000 NA 0.004
#&gt; GSM892363     1  0.0291      0.963 0.992 0.000 0.000 0.000 NA 0.004
#&gt; GSM892367     3  0.1053      0.789 0.000 0.000 0.964 0.004 NA 0.020
#&gt; GSM892371     3  0.2450      0.786 0.008 0.000 0.896 0.068 NA 0.016
#&gt; GSM892375     2  0.1010      0.963 0.000 0.960 0.000 0.000 NA 0.004
#&gt; GSM892379     2  0.0260      0.963 0.000 0.992 0.000 0.000 NA 0.000
#&gt; GSM892385     2  0.1387      0.951 0.000 0.932 0.000 0.000 NA 0.000
#&gt; GSM892389     2  0.0632      0.963 0.000 0.976 0.000 0.000 NA 0.000
#&gt; GSM892341     3  0.5096      0.671 0.028 0.000 0.700 0.096 NA 0.168
#&gt; GSM892346     4  0.4988      0.809 0.180 0.000 0.012 0.712 NA 0.056
#&gt; GSM892350     4  0.2554      0.877 0.092 0.000 0.028 0.876 NA 0.004
#&gt; GSM892354     1  0.0881      0.961 0.972 0.000 0.000 0.008 NA 0.012
#&gt; GSM892356     1  0.0291      0.963 0.992 0.000 0.000 0.004 NA 0.000
#&gt; GSM892362     1  0.0547      0.961 0.980 0.000 0.000 0.020 NA 0.000
#&gt; GSM892366     3  0.1007      0.797 0.000 0.000 0.956 0.000 NA 0.044
#&gt; GSM892370     3  0.2344      0.786 0.008 0.000 0.896 0.068 NA 0.028
#&gt; GSM892374     2  0.2494      0.902 0.000 0.864 0.000 0.000 NA 0.016
#&gt; GSM892378     2  0.0972      0.962 0.000 0.964 0.000 0.000 NA 0.008
#&gt; GSM892382     2  0.0858      0.962 0.000 0.968 0.000 0.000 NA 0.004
#&gt; GSM892384     2  0.0713      0.963 0.000 0.972 0.000 0.000 NA 0.000
#&gt; GSM892388     2  0.2755      0.889 0.000 0.844 0.000 0.004 NA 0.012
#&gt; GSM892343     3  0.7272      0.290 0.164 0.000 0.468 0.264 NA 0.072
#&gt; GSM892348     4  0.3686      0.870 0.152 0.000 0.024 0.800 NA 0.012
#&gt; GSM892352     4  0.6046      0.701 0.088 0.000 0.128 0.624 NA 0.156
#&gt; GSM892358     1  0.0603      0.959 0.980 0.000 0.000 0.004 NA 0.000
#&gt; GSM892360     1  0.0551      0.963 0.984 0.000 0.000 0.004 NA 0.004
#&gt; GSM892364     1  0.1007      0.960 0.968 0.000 0.004 0.016 NA 0.008
#&gt; GSM892368     3  0.0291      0.796 0.000 0.000 0.992 0.004 NA 0.004
#&gt; GSM892372     3  0.0622      0.798 0.000 0.000 0.980 0.012 NA 0.008
#&gt; GSM892376     2  0.0632      0.964 0.000 0.976 0.000 0.000 NA 0.000
#&gt; GSM892380     2  0.0547      0.964 0.000 0.980 0.000 0.000 NA 0.000
#&gt; GSM892386     2  0.1049      0.961 0.000 0.960 0.000 0.000 NA 0.008
#&gt; GSM892390     2  0.0692      0.963 0.000 0.976 0.000 0.000 NA 0.004
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) disease.state(p) other(p) individual(p) k
#> ATC:NMF 50     1.000            0.934 2.67e-07      1.42e-03 2
#> ATC:NMF 50     1.000            0.931 1.26e-12      1.60e-05 3
#> ATC:NMF 49     0.995            0.961 2.28e-17      4.54e-07 4
#> ATC:NMF 50     1.000            0.986 6.71e-18      2.02e-07 5
#> ATC:NMF 48     1.000            0.962 7.68e-17      3.25e-07 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      parallel  stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0     ComplexHeatmap_2.1.1  markdown_1.1          knitr_1.26           
#>  [5] preprocessCore_1.46.0 cola_1.3.2            GEOquery_2.52.0       Biobase_2.44.0       
#>  [9] BiocGenerics_0.30.0   GetoptLong_0.1.7     
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6         matrixStats_0.55.0   bit64_0.9-7          doParallel_1.0.15   
#>  [5] RColorBrewer_1.1-2   httr_1.4.1           tools_3.6.0          backports_1.1.5     
#>  [9] R6_2.4.1             DBI_1.0.0            lazyeval_0.2.2       colorspace_1.4-1    
#> [13] withr_2.1.2          tidyselect_0.2.5     gridExtra_2.3        bit_1.1-14          
#> [17] compiler_3.6.0       xml2_1.2.2           microbenchmark_1.4-7 pkgmaker_0.28       
#> [21] slam_0.1-46          scales_1.1.0         readr_1.3.1          NMF_0.23.6          
#> [25] stringr_1.4.0        digest_0.6.23        pkgconfig_2.0.3      bibtex_0.4.2        
#> [29] highr_0.8            limma_3.40.6         rlang_0.4.2          GlobalOptions_0.1.1 
#> [33] RSQLite_2.1.2        impute_1.58.0        shape_1.4.4          mclust_5.4.5        
#> [37] dendextend_1.12.0    dplyr_0.8.3          RCurl_1.95-4.12      magrittr_1.5        
#> [41] Matrix_1.2-17        Rcpp_1.0.3           munsell_0.5.0        S4Vectors_0.22.1    
#> [45] viridis_0.5.1        lifecycle_0.1.0      stringi_1.4.3        plyr_1.8.4          
#> [49] blob_1.2.0           crayon_1.3.4         lattice_0.20-38      splines_3.6.0       
#> [53] annotate_1.62.0      circlize_0.4.9       hms_0.5.2            zeallot_0.1.0       
#> [57] pillar_1.4.2         rjson_0.2.20         rngtools_1.4         reshape2_1.4.3      
#> [61] codetools_0.2-16     stats4_3.6.0         XML_3.98-1.20        glue_1.3.1          
#> [65] evaluate_0.14        png_0.1-7            vctrs_0.2.0          foreach_1.4.7       
#> [69] polyclip_1.10-0      gtable_0.3.0         purrr_0.3.3          tidyr_1.0.0         
#> [73] clue_0.3-57          assertthat_0.2.1     ggplot2_3.2.1        xfun_0.11           
#> [77] gridBase_0.4-7       eulerr_6.0.0         xtable_1.8-4         skmeans_0.2-11      
#> [81] survival_2.44-1.1    viridisLite_0.3.0    tibble_2.1.3         iterators_1.0.12    
#> [85] AnnotationDbi_1.46.1 registry_0.5-1       memoise_1.1.0        IRanges_2.18.3      
#> [89] cluster_2.1.0        brew_1.0-6
```


