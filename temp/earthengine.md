# earth engine

## keywords
- bands
- collections
- geometry (polygons,lines, points, ..)
- masks?
- map
- layer
- imagecollections / image
- filter
- visualization

## example procedure
- `ee.ImageCollection` A collection of images, often used to work with satellite data over time.
  - `var collection = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR');` crete var with dataset
  - `.filterDate` Method to filter images or features by date range.
    - `var filteredCollection = collection.filterDate('2020-01-01', '2020-12-31');` apply filter / create var
- `ee.Geometry` Represents geometric shapes (e.g., points, lines, polygons) used to define areas of interest.
  - `var point = ee.Geometry.Point(-122.262, 37.8719);` crete a point var
  - `Map.centerObject(ee.Geometry.Point(-122.262, 37.8719), 10)` center round point on map
  - `.filterBounds` Method to filter images or features by spatial bounds (geometry).
    - `var spatialFilteredCollection = filteredCollection.filterBounds(point);`
  - `var medianImage = spatialFilteredCollection.median();`
  - `.filterBounds` Method to filter images or features by spatial bounds (geometry).

- `ee.Image` A single raster image, which can be composed of multiple bands.
- `ee.FeatureCollection` A collection of features, each with a geometry and associated properties.


## examples


## Map functions
- `Map.centerObject` Method to center the map on a specific object or geometry.
- `Map.`, e.g.
  - `Map.centerObject(ee.Geometry.Point(-60.0, -3.0), 6);`
  - `Map.addLayer(treeLoss, lossViz, 'Tree Cover Loss 2001-2022'); // Add the tree cover loss layer to the map`
  - `Map.addLayer(fireComposite, fireViz, 'MODIS Fire Detections 2023'); // add fireComposite as layer to map`

## visualization params
```js
var lossViz = {
  min: 1,
  max: 22,
  palette: ['red'] // Red for tree cover loss
};

Map.addLayer(treeLoss, lossViz, 'Tree Cover Loss 2001-2022');
```

- `palette` Color scheme used to visualize single-band images.
- `mask` Method to mask out certain pixels in an image.`
- `merge` Color scheme used to visualize single-band images.
- `palette` Color scheme used to visualize single-band images.



```js
// Load the MODIS fire collection (MOD14A1)
var fireCollection = ee.ImageCollection('MODIS/006/MOD14A1')
                      .filterDate('2023-01-01', '2023-12-31');

// Select the 'FireMask' band, where value 7 indicates fire
var fireMask = fireCollection.select('FireMask');

// Create a composite image using the maximum value for each pixel
var fireComposite = fireMask.max();

// Define fire visualization parameters
var fireViz = {
  min: 0,
  max: 7,
  palette: ['000000', 'FF0000'] // black to red color ramp
};

// Load the Hansen Global Forest Change dataset
var gfc = ee.Image('UMD/hansen/global_forest_change_2022_v1_10');

// Select the tree cover loss band
var treeLoss = gfc.select('lossyear');

// Define tree loss visualization parameters
var lossViz = {
  min: 1,
  max: 22,
  palette: ['red'] // Red for tree cover loss
};

// Center the map on an area with significant deforestation and fires (e.g., Amazon rainforest)
Map.centerObject(ee.Geometry.Point(-60.0, -3.0), 6);

// Add the tree cover loss layer to the map
Map.addLayer(treeLoss, lossViz, 'Tree Cover Loss 2001-2022');

// Add the fire composite layer to the map
Map.addLayer(fireComposite, fireViz, 'MODIS Fire Detections 2023');
```
