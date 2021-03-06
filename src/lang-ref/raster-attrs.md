### 栅格符号（raster）的属性

##### raster-opacity `float`


默认值： 1
_(opaque)_
不透明

The opacity of the raster symbolizer on top of other symbolizers.
设置栅格符号的透明度。
* * *

##### raster-filter-factor `float`


默认值： -1
_(Allow the datasource to choose appropriate downscaling.)_
允许数据源自行选用缩小图像尺寸（重采样？）的方法

This is used by the Raster or Gdal datasources to pre-downscale images using overviews. Higher numbers can sometimes cause much better scaled image output, at the cost of speed.
用于栅格或GDAL数据源（译注：这个是mapnik概念），对图像尺寸进行预先缩小。将该值调高可以得到更好（译注：什么叫“好”？）的缩略图效果，但相应的处理时间也会变长。
* * *

##### raster-scaling `keyword`
`near``fast``bilinear``bilinear8``bicubic``spline16``spline36``hanning``hamming``hermite``kaiser``quadric``catrom``gaussian``bessel``mitchell``sinc``lanczos``blackman`

默认值： near


The scaling algorithm used to making different resolution versions of this raster layer. Bilinear is a good compromise between speed and accuracy, while lanczos gives the highest quality.
设置对栅格数据进行重采样的算法。Bilinear可以在速度和质量方面得到不错的平衡，而lanczos则能够得到最高的绘制质量。
* * *

##### raster-mesh-size `unsigned`


默认值： 16
_(Reprojection mesh will be 1&#x2F;16 of the resolution of the source image)_
取原始图像分辨率的1/16作为栅格的重投影网格大小（译注：对于256\*256的瓦片，网格的大小就是256/16=16，也就是16\*16这么大。不知道这么理解对不对，不过至少从之前绘制过的栅格，特别是出现那些奇怪小网格的栅格数据情况来看，应该是这样的。）

A reduced resolution mesh is used for raster reprojection, and the total image size is divided by the mesh-size to determine the quality of that mesh. Values for mesh-size larger than the default will result in faster reprojection but might lead to distortion.
在对原始图像进行重投影时，是先将图像切分成若干网格，对网格中的小图像片分别重投影。如果设定该值使得网格的尺寸变大（译注：即把该属性的值调小），那么重投影的速度会加快，但可能会导致图像变形。
* * *

##### raster-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_
将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。
* * *

##### raster-colorizer-default-mode `keyword`
`discrete``linear``exact`

默认值： undefined


TODO
* * *

##### raster-colorizer-default-color `color`


默认值： undefined


TODO
* * *

##### raster-colorizer-epsilon `float`


默认值： undefined


TODO
* * *

##### raster-colorizer-stops `tags`


默认值： undefined


TODO
* * *

