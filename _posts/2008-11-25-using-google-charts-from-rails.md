---
title: Using Google Charts from rails
author: marten
layout: post
categories:
  - Ruby on Rails
tags:
  - developer
  - gchartrb
  - Google Charts API
  - google charts on rails
  - googlecharts
  - newsdesk
  - ruby
  - Ruby on Rails
---
A week ago i got an interesting ticket – update the Newsdesk statistics charts for printing and sending via email. Although people here are quite satisfied whith the look and feel of our current solutions <a href="http://www.fusioncharts.com/" target="_self">Fusion Charts</a>, we feel that not being able to print the whole statistics page in one job is a huge drawback. Browsing through the different alternatives for graphic charting, once again I stumbled upon a free of charge, easy to use google service called <a href="http://http://code.google.com/apis/chart/" target="_self">Google Charts</a>.

Google Charts seemed pretty straight forward and fairly easy to implement so it was an easy decision to go for it. My goal was to substitute the Fusion Charts with Google Charts in the event of printing or emailing the stats (we keep FC for online viewing since, after all, it provide a huge amount of fluff and motional joy for the end user).

I found no less than three different ruby wrappers for the Google Charts API:

*   <a title="Googlecharts" href="http://googlecharts.rubyforge.org/" target="_blank">Googlecharts</a> (gem)
*   <a title="gchartrb" href="http://code.google.com/p/gchartrb/" target="_blank">gchartrb</a> (gem)
*   <a title="google-charts-on-rails" href="http://code.google.com/p/google-charts-on-rails/" target="_blank">google-charts-on-rails</a> (rails plugin)

To save you the time, here is a quick comparison between the three:

**Googlecharts**  
install: `gem install googlecharts`

Example:

<pre style="overflow:auto;background-color:#2b2b2b;color:#e6e1dc;line-height:12px;font-size:12px;padding:6px;"><pre class="sunburst"><span class="Support">Gchart</span>.<span class="Entity">bar</span>(<span style="color:#da4939;"><span style="color:#da4939;">:</span>data</span> =&gt; google_data, <span style="color:#da4939;"><span style="color:#da4939;">
  :</span>size</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">"</span><span class="StringEmbeddedSource"><span class="StringEmbeddedSource">#{</span>options<span class="StringEmbeddedSource">[</span><span class="StringConstant"><span class="StringConstant">:</span>width</span><span class="StringEmbeddedSource">]</span><span class="StringEmbeddedSource">}</span></span>x<span class="StringEmbeddedSource"><span class="StringEmbeddedSource">#{</span>options<span class="StringEmbeddedSource">[</span><span class="StringConstant"><span class="StringConstant">:</span>height</span><span class="StringEmbeddedSource">]</span><span class="StringEmbeddedSource">}</span></span><span style="color:#a5c261;">"</span></span>, <span class="StringRegexp"><span class="StringRegexp">/</span></span><span class="StringRegexp"><span class="StringRegexp">/</span></span> <span style="color:#d0d0ff;">SIZE</span> <span style="color:#d0d0ff;">DEFINED</span> <span style="color:#d0d0ff;">AS</span> <span style="color:#d0d0ff;">STRING</span> <span style="color:#d0d0ff;">WITH</span> <span style="color:#d0d0ff;">BOTH</span> <span style="color:#d0d0ff;">WIDTH</span> <span style="color:#d0d0ff;">AND</span> <span style="color:#d0d0ff;">HEIGHT</span>, <span style="color:#d0d0ff;">NOT</span> <span style="color:#d0d0ff;">GOOD</span>
  <span style="color:#da4939;"><span style="color:#da4939;">:</span>axis_with_labels</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>x,y<span style="color:#a5c261;">'</span></span>, <span style="color:#da4939;"><span style="color:#da4939;">
  :</span>axis_labels</span> =&gt; [labels, [<span style="color:#da4939;"></span>, (max<span style="color:#cc7833;">/</span><span style="color:#da4939;">2</span>).<span class="Entity">to_i</span>, max.<span class="Entity">to_i</span>]],
  <span style="color:#da4939;"><span style="color:#da4939;">:</span>format</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>image_tag<span style="color:#a5c261;">'</span></span>, <span style="color:#da4939;"><span style="color:#da4939;">
  :</span>bar_colors</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>E04300<span style="color:#a5c261;">'</span></span>,
  <span style="color:#da4939;"><span style="color:#da4939;">:</span>max_value</span> =&gt; max, <span style="color:#da4939;"><span style="color:#da4939;">
  :</span>bar_width_and_spacing</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">"</span><span class="StringEmbeddedSource"><span class="StringEmbeddedSource">#{</span>options<span class="StringEmbeddedSource">[</span><span class="StringConstant"><span class="StringConstant">:</span>bar_width</span><span class="StringEmbeddedSource">]</span><span class="StringEmbeddedSource">}</span></span>,<span class="StringEmbeddedSource"><span class="StringEmbeddedSource">#{</span>options<span class="StringEmbeddedSource">[</span><span class="StringConstant"><span class="StringConstant">:</span>bar_spacing</span><span class="StringEmbeddedSource">]</span><span class="StringEmbeddedSource">}</span></span><span style="color:#a5c261;">"</span></span>)</pre>
</pre>

Pros:  
- it’s possible to set a custom key with stuff that are not included in the wrapper(grids for example)  
- nice and clean syntax with options-hash like arguments  
- possible to specify ouput format such as file or image_tag

Cons:  
- not implemented grid, line_thickness, and ranges (to my knowledge). This is fairly easy to implement by yourself using the custom option though. ****

**gchartrb**  
install: `gem install gchartrb`

Example:

<pre style="overflow:auto;background-color:#2b2b2b;color:#e6e1dc;line-height:12px;font-size:12px;padding:6px;"><pre class="sunburst"><span class="Support">GoogleChart</span>::<span class="Entity">BarChart</span>.<span class="Entity">new</span>(<span style="color:#a5c261;"><span style="color:#a5c261;">"</span><span class="StringEmbeddedSource"><span class="StringEmbeddedSource">#{</span>options<span class="StringEmbeddedSource">[</span><span class="StringConstant"><span class="StringConstant">:</span>width</span><span class="StringEmbeddedSource">]</span><span class="StringEmbeddedSource">}</span></span>x<span class="StringEmbeddedSource"><span class="StringEmbeddedSource">#{</span>options<span class="StringEmbeddedSource">[</span><span class="StringConstant"><span class="StringConstant">:</span>height</span><span class="StringEmbeddedSource">]</span><span class="StringEmbeddedSource">}</span></span><span style="color:#a5c261;">"</span></span>, options[<span style="color:#da4939;"><span style="color:#da4939;">:</span>name</span>]) <span style="color:#cc7833;">do </span>|<span style="color:#d0d0ff;">lc</span>| <span class="StringRegexp"><span class="StringRegexp">/</span></span><span class="StringRegexp"><span class="StringRegexp">/</span></span> <span style="color:#d0d0ff;">SIZE</span> <span style="color:#d0d0ff;">DEFINED</span> <span style="color:#d0d0ff;">AS</span> <span style="color:#d0d0ff;">STRING</span> <span style="color:#d0d0ff;">WITH</span> <span style="color:#d0d0ff;">BOTH</span> <span style="color:#d0d0ff;">WIDTH</span> <span style="color:#d0d0ff;">AND</span> <span style="color:#d0d0ff;">HEIGHT</span>, <span style="color:#d0d0ff;">NOT</span> <span style="color:#d0d0ff;">GOOD</span>
  lc.<span class="Entity">show_legend</span> <span style="color:#cc7833;">=</span> <span style="color:#da4939;">false</span>
  lc.<span class="Entity">data</span> <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>Visningar<span style="color:#a5c261;">'</span></span>, google_data, <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>E04300<span style="color:#a5c261;">'
</span></span>  lc.<span class="Entity">axis</span> <span style="color:#da4939;"><span style="color:#da4939;">:</span>x</span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>color</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>333333<span style="color:#a5c261;">'</span></span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>font_size</span> =&gt; <span style="color:#da4939;">10</span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>alignment</span> =&gt; <span style="color:#da4939;"><span style="color:#da4939;">:</span>center</span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>labels</span> =&gt; labels
  lc.<span class="Entity">axis</span> <span style="color:#da4939;"><span style="color:#da4939;">:</span>y</span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>range</span> =&gt; [<span style="color:#da4939;"></span>,max], <span style="color:#da4939;"><span style="color:#da4939;">:</span>color</span> =&gt; <span style="color:#a5c261;"><span style="color:#a5c261;">'</span>333333<span style="color:#a5c261;">'</span></span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>font_size</span> =&gt; <span style="color:#da4939;">10</span>, <span style="color:#da4939;"><span style="color:#da4939;">:</span>alignment</span> =&gt; <span style="color:#da4939;"><span style="color:#da4939;">:</span>center
</span>  lc.<span class="Entity">width_spacing_options</span>(<span style="color:#da4939;"><span style="color:#da4939;">:</span>bar_width</span> =&gt; options[<span style="color:#da4939;"><span style="color:#da4939;">:</span>bar_width</span>], <span style="color:#da4939;"><span style="color:#da4939;">:</span>bar_spacing</span> =&gt; options[<span style="color:#da4939;"><span style="color:#da4939;">:</span>bar_spacing</span>]) <span style="color:#cc7833;">
end</span>.<span class="Entity">to_url</span> <span class="Entity">
image_tag</span>(url)</pre>
</pre>

Pros:  
- easy handling of axises (above)  
- block syntax is always nice

Cons:****

**google-charts-on-rails**  
install: `script/plugin install http://google-charts-on-rails.googlecode.com/svn/google_charts_on_rails/ `

Example: (i have none of my own)

<pre style="overflow:auto;background-color:#2b2b2b;color:#e6e1dc;line-height:12px;font-size:12px;padding:6px;"><pre class="sunburst"><span style="color:#d0d0ff;">GoogleChart</span>.100<span class="Entity">x200_pie</span>(<span style="color:#da4939;">10</span>,<span style="color:#da4939;">20</span>,<span style="color:#da4939;">40</span>,<span style="color:#da4939;">30</span>).<span class="Entity">to_url</span></pre>
</pre>

or

<pre style="overflow:auto;background-color:#2b2b2b;color:#e6e1dc;line-height:12px;font-size:12px;padding:6px;"><pre class="sunburst">sales_chart <span style="color:#cc7833;">=</span> <span class="Support">GoogleChart</span>.<span class="Entity">new</span>sales_chart.<span class="Entity">type</span> <span style="color:#cc7833;">=</span> <span style="color:#da4939;"><span style="color:#da4939;">:</span>pie</span>
sales_chart.<span class="Entity">height</span> <span style="color:#cc7833;">=</span> <span style="color:#da4939;">200
</span>sales_chart.<span class="Entity">width</span> <span style="color:#cc7833;">=</span> <span style="color:#da4939;">150</span></pre>
</pre>

Pros:  
- it’s not nothing

Cons:

- weird method_missing -like syntax  
- poor doc  
- much of Google Charts API not implemented

**Conclusion:  
**You should use either gchartrb or googlecharts depending on code taste and exact needs.  
Google Charts API is not a good solution for exact and scientificly correct graphs since it seems to render the data, labels and grids independently. Labels for example are spaced evenly along a certain axis and have no connection to the actual data points in the graph.