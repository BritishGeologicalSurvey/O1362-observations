# Viewing geotagged photos in QGIS

> Demonstration of how to view geotagged images in QGIS.

# https://bit.ly/2NOUSgJ

#### Outline:

1. Download and unzip project files from:
[https://github.com/BritishGeologicalSurvey/O1362-observations/archive/master.zip](https://github.com/BritishGeologicalSurvey/O1362-observations/archive/master.zip)
2. Watch YouTube video: [https://www.youtube.com/watch?v=BgjsfM8mUsc](https://www.youtube.com/watch?v=BgjsfM8mUsc)
3. Try it for yourself.


## Importing geotagged image locations

QGIS comes with a geotagged image importer that reads metadata from geotagged
photos and converts it into a point layer.  Use it via `Processing > Toolbox`.


### Adding plugins

Plugins are required to import basemap data and to read GPS files.  Install
those via `Plugins > Manage and install plugins...`

### Adding satellite basemaps

The QuickMapServices plugin adds background maps from internet tile servers.
Once installed, it can be accessed from `Web > QuickMapServices`.  The Bing
Satellite layer is good for Iceland.  It is in the "contributed pack", which is
added via `Web > QuickMapServices > More services > Get contributed pack`.


### Setting coordinate reference system

Web maps are best viewed in the WGS 84 / Pseudo Mercator coordinate reference
system (EPSG
3857). This can be set via `Project > Properties > CRS`.


### Adding GPS localities

GPS data are read and written via the GPS Tools plugin.  Once activated, it can
be accessed from `Vector > GPS Tools`.


### Viewing geotagged images directly in QGIS

View the images in QGIS when hovering the mouse over their location by adding
the following code to the HTML Map Tip box (`[Right click] > Properties > Display`)

**Linux / Mac**

```html
[% filename %]
<br>
<a href="file://[% photo %]" target="new">
  <img src="file://[% photo %]" width=200>
</a>
```

**Windows** (note extra `/` in file paths)

```html
[% filename %]
<br>
<a href="file:///[% photo %]" target="new">
  <img src="file:///[% photo %]" width=200>
</a>
```

Make sure that Map Tips are enabled (`View > Show Map Tips`)


## Advanced

The following steps make for an even smoother experience.


#### Relative file paths

The Geotagged Photos tool records the absolute file path for photo
locations.  By switching to relative paths, the project folder (QGIS settings
plus image files) can be moved and
copied to different locations without breaking the links.  There are three steps:

1. Save the project as a `.qgz` file (`Project > Save as...`) in a directory
   above the photos.
2. Add a `relative_path` field to the Attributes of the photos layers using the
   Field Calculator (`[Right click] > Open Attribute Table > Open field calculator`).  Then create new text (unlimited length) field called `relative_path` defined by the expression `replace("photo", 'C:\\Users\\path\\to\\project\\directory', '')`. This removes the start of the file path. Note the double backslashes.
3. Update the addresses in the Map Tips to `"file://[%
   @project_folder || relative_path %]"`


#### Fast-loading thumbnails

QGIS resizes the full image to thumbnail size on-the-fly when the mouse hovers over a point.  Try using a tool such as [ImageMagick](https://www.imagemagick.org/) to create separate thumbnail files for your images, then
update the MapTip code to use them instead.  This will
make them pop up more quickly.


#### Offset overlapping points

Where multiple photos were tagged with the same location, the mouse-over will
only read from the top marker.  The other photos cannot be accessed.
A work-around for this is to perturb the location of the images so they no
longer overlap.  Use the GRASS "perturb" tool (`Processing > Toolbox > v.perturb`) to add a uniform distribution of up to 0.0001° (equivalent to
about 10 m) to the photos layer to spread out overlapping points.

The QGIS Displacement Renderer tool for Symbology can also offset overlapping
points, but it [doesn't yet work with the Map
Tips](https://issues.qgis.org/issues/16131).

## Links

+ QGIS: [https://qgis.org](https://qgis.org)
+ QGIS Github page:
  [https://github.com/qgis/QGIS](https://github.com/qgis/QGIS)
+ Shapefile must die! (QGIS3 default is geopackage): [http://switchfromshapefile.org](http://switchfromshapefile.org)
+ GPS Prune (geotag images from normal cameras using external GPS data by
  matching time stamps with track locations):
  [https://activityworkshop.net/software/gpsprune/](https://activityworkshop.net/software/gpsprune/)
+ Demo of GPS Prune correlation:
  [https://www.youtube.com/watch?v=SLRD9MDdIF8](https://www.youtube.com/watch?v=SLRD9MDdIF8)

+ volcan01010: [https://www.twitter.com/volcan01010](https://www.twitter.com/volcan01010)
+ Fieldwork Guide For Robots:
  [http://all-geo.org/volcan01010/2014/06/a-robots-guide-to-fieldwork/](http://all-geo.org/volcan01010/2014/06/a-robots-guide-to-fieldwork/)
+ All the software a geoscientist needs. For free:
  [http://all-geo.org/volcan01010/2011/11/all-the-software-a-geoscientists-needs-for-free/](http://all-geo.org/volcan01010/2011/11/all-the-software-a-geoscientists-needs-for-free/)
