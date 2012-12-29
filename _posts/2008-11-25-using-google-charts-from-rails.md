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

*   [Googlecharts](http://googlecharts.rubyforge.org/) (gem)
*   [gchartrb](http://code.google.com/p/gchartrb/) (gem)
*   [google-charts-on-rails](http://code.google.com/p/google-charts-on-rails/) (rails plugin)

To save you the time, here is a quick comparison between the three:

**Googlecharts**
install: `gem install googlecharts`

Example:

{% highlight2 ruby %}
Gchart.bar(:data => google_data,
  :size => "#{options[:width]}x#{options[:height]}", // SIZE DEFINED AS STRING WITH BOTH WIDTH AND HEIGHT, NOT GOOD
  :axis_with_labels => 'x,y',
  :axis_labels => [labels, [, (max/2).to_i, max.to_i]],
  :format => 'image_tag',
  :bar_colors => 'E04300',
  :max_value => max,
  :bar_width_and_spacing => "#{options[:bar_width]},#{options[:bar_spacing]}")
{% endhighlight2 %}

Pros:
- it’s possible to set a custom key with stuff that are not included in the wrapper(grids for example)
- nice and clean syntax with options-hash like arguments
- possible to specify ouput format such as file or image_tag

Cons:
- not implemented grid, line_thickness, and ranges (to my knowledge). This is fairly easy to implement by yourself using the custom option though.

**gchartrb**
install: `gem install gchartrb`

Example:

{% highlight2 ruby %}
GoogleChart::BarChart.new("#{options[:width]}x#{options[:height]}", options[:name]) do |lc| // SIZE DEFINED AS STRING WITH BOTH WIDTH AND HEIGHT, NOT GOOD
  lc.show_legend = false
  lc.data 'Visningar', google_data, 'E04300'
  lc.axis :x, :color => '333333', :font_size => 10, :alignment => :center, :labels => labels
  lc.axis :y, :range => [,max], :color => '333333', :font_size => 10, :alignment => :center
  lc.width_spacing_options(:bar_width => options[:bar_width], :bar_spacing => options[:bar_spacing])
end.to_url
image_tag(url)
{% endhighlight2 %}


Pros:
- easy handling of axises (above)
- block syntax is always nice

Cons:

**google-charts-on-rails**
install: `script/plugin install http://google-charts-on-rails.googlecode.com/svn/google_charts_on_rails/ `

Example: (i have none of my own)

{% highlight2 ruby %}
GoogleChart.100x200_pie(10,20,40,30).to_url
{% endhighlight2 %}
or
{% highlight2 ruby %}
sales_chart = GoogleChart.newsales_chart.type = :pie
sales_chart.height = 200
sales_chart.width = 150
{% endhighlight2 %}

Pros:
- it’s not nothing

Cons:

- weird method_missing -like syntax
- poor doc
- much of Google Charts API not implemented

**Conclusion:
**You should use either gchartrb or googlecharts depending on code taste and exact needs.
Google Charts API is not a good solution for exact and scientificly correct graphs since it seems to render the data, labels and grids independently. Labels for example are spaced evenly along a certain axis and have no connection to the actual data points in the graph.