### 样式选择器（Selectors）

_译注：[原文地址](https://www.mapbox.com/mapbox-studio/styling-selectors/)_

CartoCSS styles are constructed by applying blocks of style rules to groups of objects. Style blocks are bounded by curly braces {} and contain style properties and values. Selectors are what allow you restrict these styles to specific layers or groups of objects within layers.

在CartoCSS中，地图样式通过一系列样式规则来表达。这些样式规则作用于地图上的各种要素对象，并且以模块化的形式进行组织。每一个**样式块**都由一对花括号`{}`包围，其中包含了若干条用于描述样式的属性和值。**样式选择器**的作用就是指明某个样式块是作用于哪个图层，或者进一步限定这些样式在特定图层中的作用范围是哪些要素对象。（译注：因此从本质上说，样式选择器其实就是描述了样式块的作用域）

样式选择器可以有三种不同的形式：图层标识、图层类别，以及过滤器。其中过滤器还可以分为缩放级别、数值、文本和正则表达式三种类型。

#### 图层标识（By layer ID）

Select all of the objects from a single layer by the layer’s ID. Separate multiple layer IDs with commas to select them for a single style.

通过图层的唯一标识（ID）来将样式块的作用范围限定在某一个或几个图层上。对于多个图层使用同一样式块的情况，应该将多个图层标识以逗号隔开。

	
	#layer_name {
	 // 样式描述
	}
	#layer_1,
	#layer_2 {
	 // 这里的样式将被应用于layer_1和layer_2两个图层
	}
	

#### 图层类别（By layer class）

You can also assign classes to layers to select multiple layers more simply. In Mapbox Studio (unlike TileMill) layer classes are only available for advanced usage.

当需要对多个图层定义样式时，除了可以采用之前提到的将图层标识全部列出的方法以外，还可以使用图层类别来实现（译注：也就是在这些图层标识的后面都加上一个类别名后缀，还以上面的两个图层为例，可以为它们增加一个同一个类别得到`layer_1.sample_class`和`layer_2.sample_class`两个新的标识，然后通过对`.sample_class`进行样式定义来实现与之前同样的效果。）

	
	.roads {
	 // 这里定义的样式会被应用到所有
	 // 类别后缀为'roads'的图层上
	}
	

#### 过滤器（Filter selectors）

You can modify selections with filters that reduce the number of objects a style applies to based on certain criteria. Filters let your style read into the various text and numeric properties attached to each object in a layer. For example, you might have all your roads in a single layer, but you could use filters to specify different line colors for different road classifications.

除了用图层的标识或类别来限定样式块的作用范围以外，还可以进一步用基于条件表达式的过滤器在图层内部筛选出需要应用样式的那些要素对象。这些过滤器使样式块与图层内要素对象的各种文本或数值类型的属性数据建立起关联（译注：从而实现了_条件样式_）。过滤器在制图过程中非常实用，举个例子：一个内容为道路网的线要素图层中通常是将各种不同等级和分类的道路都包含在内，而利用过滤器，我们就可以为不同等级或类别的道路配置不同的样式。

Filters should be written inside square brackets after a layer selector or nested inside a larger style block.

过滤器需要被置于一对中括号`[]`中，跟在图层选择器（标识或类别）的后面，或者还可以嵌套的写在一个更大的样式块中。

##### 缩放级别过滤器（Zoom level filters）

Restrict styles to certain zoom levels. This style will only apply when your map is zoomed all the way out to zoom level 0:

将样式的作用范围限制在特定的地图缩放级别上。在下面的例子中，样式块中定义的样式只在地图缩放到0级时发挥作用。

	
	#layer[zoom=0] { /* style */ }
	

You can specify ranges of zoom levels using two filters:
还可以定义缩放级别的范围：

	
	#layer[zoom>=4][zoom<=10] { /* style */ }
	

Valid operators for zoom filters are = (equal to), \> (greater than), \< (less than), \>= (greater than or equal to), \<= (less than or equal to), != (not equal to).

缩放级别过滤器支持6种关系运算符：`=`（等于）、`>`（大于）、`<`（小于）、`>=`（大于等于）、`<=`（小于等于）和`!=`（不等于）。

You can nest filters to better organize your styles. For example, this style will draw red lines from zoom levels 4 through 10, but the lines will be thicker for zoom levels 8, 9, and 10.

过滤器可以以嵌套的方式出现在样式块内部。例如，在下面这段CartoCSS代码中，线要素会在4级到10级之间被绘制成红色，而在8级、9级和10级，线宽会更宽。

	
	#layer[zoom>=4][zoom<=10] {
	 line-color: red;
	 line-width: 2;
	 [zoom=8] { line-width: 3; }
	 [zoom=9] { line-width: 4; }
	 [zoom=10] { line-width: 5; }
	}
	

##### 数值型过滤器（Numeric value comparison filters）

The same comparison operators available for the zoom filter can also be used for any numeric column in your data. For example, you might have a population field in a source full of city points. You could create a style that only labels cities with a population of more than 1 million.

关系运算符还可以用于对图层中的数值型属性字段进行过滤。举个例子，在一个表示城市信息的点要素图层中，有一个用于记录每个城市人口数据的字段，那么我们就可以在该字段上应用数值型过滤器，实现“只有人口在一百万以上的城市才被标注在地图上”的效果：

	
	#cities[population>1000000] {
	 text-name: [name];
	 text-face-name: 'Open Sans Regular';
	}
	

You could also combine multiple numeric filters with zoom level filters to gradually bring in more populated cities as you zoom in.

而这种数值型过滤器可以和之前介绍的缩放级别过滤器结合，从而实现在不同的缩放级别上显示不同人口规模的城市。

	
	#cities {
	 [zoom>=4][population>1000000],
	 [zoom>=5][population>500000],
	 [zoom>=6][population>100000] {
		text-name: [name];
		text-face-name: 'Open Sans Regular';
	 }
	}
	

As with zoom levels, you can select data based on numeric ranges.

与缩放级别过滤器相同，数值型过滤器中也同样支持数值范围过滤：

	
	#cities[population>100000][population<2000000] { /* styles */ }
	

##### 文本型过滤器（Text comparison filters）

You can also filter on columns that contain text. Filter on exact matches with the equals operator (=) or get the inverse results with the not-equal operator (!=). Unlike zoom and numeric values, text values must be quoted with either double or single quotes.

对于文本类型的属性字段，同样也可以应用过滤器。通过使用等号运算符`=`，可以实现精确匹配，或者通过不等号运算符`!=`来得到相反的结果。与前两种过滤器不同的是，文本型过滤器关系表达式中的文本值必须要有双引号或单引号。

As an example, look at the roads layer in Mapbox Streets (the default vector tile source in Mapbox Studio). It contains a field called class, and each value for this field is one of just a few options such as “motorway”, “main”, and “street”. This makes it a good column to filter on for styling.

举个例子（译注：这里隐去了Mapbox相关内容），在一个表示道路网的线要素图层中包含了一个名为`class`的文本类型属性字段，该字段取值为`"motoway"`，`"main"`或者`"street"`，用于表示每条道路的类型。那么在制图过程中，用户就可以在该字段上应用文本型过滤器，从而实现对不同类型的道路使用不同的样式进行渲染：

	
	#roads {
	 [class='motorway'] {
		   line-width: 4;
	 }
	 [class='main'] {
		   line-width: 2;
	 }
	 [class='street'] {
		   line-width: 1;
	 }
	}
	

To select everything that is not a motorway you could use the != (“not equal”) operator in the filter:

如果要对所有不是`motoway`的道路进行样式配置，那么还可以使用不等号操作符：

	
	#roads[class!='motorway'] { /* style */ }
	

##### 正则表达式过滤器（Regular expression filters）

_Note: This is an advanced feature that may have negative performance implications._

_注意：正则表达式过滤器作为一种高级过滤功能，可能会对制图渲染性能带来负面影响。_

You can match text in filters based on a pattern using the regular expression operator (=~). This filter will match any text starting with ‘motorway’ (ie, both ‘motorway’ and ‘motorway\_link’).

用户还可以通过基于模式匹配的正则表达式进行过滤，正则表达式过滤器的运算符是`=~`。在下面的例子中，正则表达式过滤器会匹配所有`class`字段中以`motoway`开头的要素记录，像`motoway`、`motoway_link`都会被匹配上。

	
	#roads[class=~'motorway.*'] { /* style */ }
	

The . represents ‘any character’, and the \* means ‘any number of occurrences of the preceding expression. So .\* used in combination means ‘any number of any characters’.

在上面的例子中，`.`表示任意字符，`*`表示出现任意多次，因而`.*`合在一起就表示由任意字符组成的任意长度的字符串。