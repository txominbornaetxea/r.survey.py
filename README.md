# r.survey.py


<html>


<h2>NAME</h2>
<em><b>r.survey.py</b></em>  - Returns maps of visibility indexes (3d distance, view angle and solid angle) from multiple survey points
<h2>KEYWORDS</h2>
<a href="visibility.html">visibility</a>, <a href="topic_survey.html">survey</a>
<h2>SYNOPSIS</h2>
<div id="name"><b>r.survey.py</b><br></div>
<b>r.survey.py --help</b><br>
<div id="synopsis"><b>r.survey.py</b> [-<b>bcd</b>] <b>points</b>=<em>name</em> <b>dem</b>=<em>name</em> <b>output</b>=<em>name</em> <b>maxdist</b>=<em>float</em>  [<b>treesmap</b>=<em>name</em>]   [<b>buildingsmap</b>=<em>name</em>]  <b>obs_heigh</b>=<em>string</em>  [<b>treesheigh</b>=<em>string</em>]   [<b>buildingsheigh</b>=<em>string</em>]   [<b>obsabselev</b>=<em>string</em>]   [<b>layer</b>=<em>string</em>]  <b>viewangle_threshold</b>=<em>string</em>  [<b>object_radius</b>=<em>string</em>]   [<b>nprocs</b>=<em>integer</em>]   [<b>memory</b>=<em>integer</em>]   [--<b>overwrite</b>]  [--<b>help</b>]  [--<b>verbose</b>]  [--<b>quiet</b>]  [--<b>ui</b>] 
</div>

<div id="flags">
<h3>Flags:</h3>
<dl>
<dt><b>-b</b></dt>
<dd>Create a simple visible-not visible raster map</dd>

<dt><b>-c</b></dt>
<dd>Consider the curvature of the earth (current ellipsoid)</dd>

<dt><b>-d</b></dt>
<dd>allow only downward view direction (can be used for drone view)</dd>

<dt><b>--overwrite</b></dt>
<dd>Allow output files to overwrite existing files</dd>
<dt><b>--help</b></dt>
<dd>Print usage summary</dd>
<dt><b>--verbose</b></dt>
<dd>Verbose module output</dd>
<dt><b>--quiet</b></dt>
<dd>Quiet module output</dd>
<dt><b>--ui</b></dt>
<dd>Force launching GUI dialog</dd>
</dl>
</div>

<div id="parameters">
<h3>Parameters:</h3>
<dl>
<dt><b>points</b>=<em>name</em>&nbsp;<b>[required]</b></dt>
<dd>Name of input vector map</dd>
<dd>Name of the input points map  (representing the survey location)</dd>

<dt><b>dem</b>=<em>name</em>&nbsp;<b>[required]</b></dt>
<dd>Name of the input DEM layer</dd>

<dt><b>output</b>=<em>name</em>&nbsp;<b>[required]</b></dt>
<dd>Prefix for the output visibility layers</dd>

<dt><b>maxdist</b>=<em>float</em>&nbsp;<b>[required]</b></dt>
<dd>max distance from the input points</dd>
<dd>Default: <em>1000</em></dd>

<dt><b>treesmap</b>=<em>name</em></dt>
<dd>Name of input vector map</dd>
<dd>Name of the vector layer representing the forested areas</dd>

<dt><b>buildingsmap</b>=<em>name</em></dt>
<dd>Name of input vector map</dd>
<dd>Name of the vector layer representing the buildings</dd>

<dt><b>obs_heigh</b>=<em>string</em>&nbsp;<b>[required]</b></dt>
<dd>observer_elevation</dd>
<dd>Default: <em>1.75</em></dd>

<dt><b>treesheigh</b>=<em>string</em></dt>
<dd>field of the attribute table containing information about threes heigh</dd>

<dt><b>buildingsheigh</b>=<em>string</em></dt>
<dd>field of the attribute table containing information about buildings heigh</dd>

<dt><b>obsabselev</b>=<em>string</em></dt>
<dd>field of the attribute table containing information about observer absolute elevation above the same datum used by the DEM (e.g. geodetic elevation)</dd>

<dt><b>layer</b>=<em>string</em></dt>
<dd>layer of the attribute table of the point map (can be usefull for obsabselev option)</dd>
<dd>Default: <em>1</em></dd>

<dt><b>viewangle_threshold</b>=<em>string</em>&nbsp;<b>[required]</b></dt>
<dd>cut the output layers at a given threshold value (between 90 and 180 degrees)</dd>
<dd>Default: <em>90.01</em></dd>

<dt><b>object_radius</b>=<em>string</em></dt>
<dd>radius of the surveyed object in unit of map (default is half the DEM resolution)</dd>

<dt><b>nprocs</b>=<em>integer</em></dt>
<dd>Number of processes</dd>
<dd>Default: <em>1</em></dd>

<dt><b>memory</b>=<em>integer</em></dt>
<dd>Amount of memory to use in MB (for r.viewshed analysis)</dd>
<dd>Default: <em>500</em></dd>

</dl>

<h2>DESCRIPTION</h2>

r.survey is aimed at earth surface analysis. It is useful for evaluating the visibility (in terms of 3D Distance, Solid Angle and orientation of the terrain respect to the line of sight) of  features lying on the terrain slope, like landslides, road cuts, minor vegetation, mining activity, geological outcrops, surface pipelines, parking areas, solar panels plants, burned areas, etc. 
<p>
Depending on the purpose of the study, one can be interested in searching the closest position to observe a given territory while for others, having a frontal view can be more relevant. In some cases, the objective could be the combined balance between the closest and the most frontal view, in order to evaluate how much of the human field of view is occupied by a given object (solid angle). 
<p>
r.survey is deeply based on the powerfull r.viewshed GRASS GIS module. 
<p>
r.survey wants to provide the user the necessary information to answer questions such as:
<p>
In a survey trip, from how many places is something visible?
<p>
During a field work, which place would be the best position to observe a given point/area (according to the distance, the angle or both)?
<p>
Which portion of the territory is visible along a survey path/flight?
<p>
Are objects of a given size visible from a survey path/flight?
<p>
<p>
Three principal outputs (visibility indexes) are given, based on trigonometric calculations in order to provide qualitative and quantitative information about how the terrain is perceived from the selected observation point (or viewpoint). 
This information concerns the three-dimensional distance, the relative orientation (View Angle) and the Solid Angle, of each target pixel with respect to the viewpoint.
<p>
A map containing a set of points (having different categories - cat values) describing the locations of the observer and a digital elevation model (DEM) are the mandatory inputs for r.survey. 
In addition, maps of buildings and trees, portraying height information, can be used to alter the DEM.  
<p>
Many other options can be set according to the aim of the analysis, such as: observer height respect to the ground or respect to an elevation datum (absolute elevation), maximum distance to perform the calculations, a view-angle threshold to exclude, a priori, cell oriented almost perpendicularly to the lines of sight, the average size of the observed object
The input points map must be provided to the tool as a vector layer. In case it represents the positions of a UAV, an helicopter or of a satellite, it must include an attribute field containing absolute elevation values with respect to a vertical datum (the same datum used by the input DEM). Observer height (respect to the ground, the default is 1.75 m) and maximum distance of observation (default is 1000 m)  are parameters needed by the r.viewshed module. The object radius is used to approximate, with an equivalent circle, the minimum size of an object centered in the cells center and oriented according to the slope and aspect of the cells. Since r.survey produces different types of outputs, a name to be used for the prefix of the output maps is requested.

<p>
<p>
	
3D Distance is the three dimensional linear distance between a viewpoint and a target pixel. The min3dDistance map portrays  the value of the minimum three-dimensional distance between each pixel and the closest viewpoint, in meters. 
<p>
Given a viewpoint, r.survey calculate the unit vector describing direction and sense of the line of sight between that viewpoint and each visible pixel. We define View Angle the the angle between the line of sight unit vector the and the versor normal to the terrain surface in each pixel. The maxViewAngle map shows the value of the maximum View Angle between each pixel and the viewpoints, in degrees. It is a measure of the most frontal view each single cell is visible from.  View angle output is always larger than 90° and smaller than 180°.
<p>
Solid Angle is one of the best and most objective indicators for quantifying the visibility. The idea is that any observed object observed will be progressively less apreciable the more far away and the more tilted it is with regard to the viewpoint. As a consequence Solid Angle depends on the size of the observed object as well as on the distance and the orientation from where this object is observed.
Solid Angle of a surface is, by definition, equal to the projected spherical surface in the evolving sphere divided by the square of the radius of the sphere. Here comes an approximation made by r.survey: we use the ellipse surface area in place of the projected spherical surface in the evolving sphere. The maxSolidAngle map shows the value of the maximum Solid Angle among those measured from the different viewpoints. In this map, the closest pixels to the observation points have to be interpreted with special attention, considering that the error, respect to the real Solid Angle, can reach until the 10% in the immediate neighbor pixels
<p>
Other three maps (pointOfViewWithMin3dDistance, pointOfViewWithMmaxAngle and pointOfViewWithMmaxSolidAngle) are used to register, in each cell, the identifier of the viewpoints from where an observer can get, respectively, (i) the minimum values of 3D Distance, (ii) the maximum values of View Angle and (iii) the maximum value of the Solid Angle. Another relevant output is the numberOfViews map which portraits the number of viewpoints from where each pixel is visible.


<h2>NOTES</h2>
The software was designed as a GRASS GIS python module whose source code was  written using the GRASS Scripting library and the pyGRASS library
<p>
Multi-core processing is used by r.survey for reducing the computational time. When the aim is to derive the values of the visibility indexes along a given path (e.g. a road or a UAV track) viewpoints can be very dense in terms of number per unit of distance and the more the viewpoints are,  the longer the computational time becomes.  
<p>
Parallel computation was implemented exploiting the Python Multiprocessing library and the ability of GRASS GIS to set a temporary spatial region centered on the considered point without affecting the parallel computation of the other points.


<h2>EXAMPLES</h2>

In the following example we show how to run r.survey calculating the solid angle for an object of radius equal to 20 m and using 4 parallel processes.

<br>
<div class="code"><pre>
g.region raster=dtm 
r.survey25_parallel points=pt_map dem=dtm output=example maxdist=500 object_radius=20 nprocs=4
</pre></div>



<h2><a name="authors">AUTHORS</a></h2>


Ivan Marchesini, CNR IRPI, via della Madonna Alta 126, I 06128 Perugia, Italy 
<br>
Txomin Bornaetxea, University of the Basque Country UPV/EHU, barrio Sarriena s/n, 48940 Leioa, Spain



</div>
</body>
</html>
