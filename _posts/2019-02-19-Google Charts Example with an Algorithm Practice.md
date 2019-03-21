---
layout: post
title: Google Charts Example with an Algorithm Practice
date: 2019-02-19
tags: [js, javascript, jquery, ajax, html, css, GoogleCharts, example]
comments: true
---

>The reason why I choose `Google Charts` as my line chart solution is because of its clean data format and a variety of options.

I have to admit that Google Charts is not the best looking one, but it is very friendly for developers. First, the data set format always looks like a table, actually arrays in an array. Having compared with other libraries, this format is the clearest one for me.

```js
  var data = google.visualization.arrayToDataTable([
          ['Year', 'Sales', 'Expenses'],
          ['2004',  1000,      400],
          ['2005',  1170,      460],
          ['2006',  660,       1120],
          ['2007',  1030,      540]
        ]);
```

As for it's `options`, Google Charts offers a large number of options including at least everything I need.

For this case, I was required to created a line chart illustrating our servers' CPU performance in the selected time range, and the CPU usage data stores each minute.

## .html

Just need to create a Google Line Chart Div with some time-span selection button in `html` file.  

*We are using .xls transforming to .html*
*Actually just one line for Google Chart at the bottom*

```html
<div id="cpudiv">
  <div style="display: inline-block">
    <h3>
      <a target="blank">CPU Performance</a>
    </h3>
  </div>
  <!--buttons above line chart-->
  <div id="cpubuttons" style="display: none" class="pull-right">
    <style>.btn:focus {outline: none;} </style>
    <button class="btn btn-primary btn-xs dim" onclick="drawCpuChart(1)">1 H</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(3)">3 H</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(6)">6 H</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(12)">12 H</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(24)">1 D</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(72)">3 D</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(168)">1 W</button>
    <button class="btn btn-success btn-xs dim" onclick="drawCpuChart(336)">2 W</button>
    <!--button click animation-->
    <script>
      document.addEventListener("DOMContentLoaded", function() {
        $("button").click(function() {
          $(this)
            .addClass("btn-primary")
            .removeClass("btn-success");
          $(this)
            .siblings()
            .addClass("btn-success")
            .removeClass("btn-primary");
        });
      });
    </script>
  </div>

  <!--Google Chart-->
  <div id="cpu_chart" style="height:200px"></div>
</div>
```

## .js

First thing before coding `Javascript` is thinking about how to get, parse and compose data.

### Fetch Data

There is one point must be aware, which is `Ajax` is an Async Function. We can't not write `return data` in an ajax function, using a function to get the callback data instead.

```js
//using ajax to fetch data then return here as globle variable
var parentcpuxml = null; 

getxml(url, function (cpuxml) {
    var cpuxml = cpuxml.getElementsByTagName("cpu");
    parentcpuxml = cpuxml;
});

//also can use jquery $.ajax function here
function getxml(url, fn) {
	var xhttp = new XMLHttpRequest();
	xhttp.onreadystatechange = function () {
		if (this.readyState == 4 && this.status == 200) {
            fn(this.responseXML);
            //cannot return data directly
		}
	};
	xhttp.open("GET", url, true);
	xhttp.send();
}
```

Part of `cpudata` currently looks like: 

[[datetime1, cpuusage1],[datetime2, cpuusage2],[datetime3, cpuusage3],......]
```
cpudata: Array(56)
0: (2) [Tue Mar 19 2019 20:35:08 GMT-0400 (Eastern Daylight Time), 17.69007]
1: (2) [Tue Mar 19 2019 20:34:06 GMT-0400 (Eastern Daylight Time), 7.689963]
2: (2) [Tue Mar 19 2019 20:33:06 GMT-0400 (Eastern Daylight Time), 14.61316]
3: (2) [Tue Mar 19 2019 20:32:06 GMT-0400 (Eastern Daylight Time), 6.920557]
4: (2) [Tue Mar 19 2019 20:31:06 GMT-0400 (Eastern Daylight Time), 46.92161]
5: (2) [Tue Mar 19 2019 20:30:06 GMT-0400 (Eastern Daylight Time), 13.84391]
.......

```