# ee examples

## Monitoring Forest Health Using NDVI

```js
// Define the area of interest (AOI) as a polygon
var aoi = ee.Geometry.Polygon([
  [[-122.5, 37.5], [-122.5, 37.7], [-122.2, 37.7], [-122.2, 37.5]]
]);

// Load the Landsat 8 ImageCollection
var collection = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR')
  .filterDate('2022-01-01', '2022-12-31')
  .filterBounds(aoi);

// Calculate NDVI for each image in the collection
var ndviCollection = collection.map(function(image) {
  var ndvi = image.normalizedDifference(['B5', 'B4']).rename('NDVI');
  return ndvi.copyProperties(image, ['system:time_start']);
});

// Create a median composite of NDVI
var ndviMedian = ndviCollection.median();

// Define visualization parameters for NDVI
var ndviViz = {
  min: 0,
  max: 1,
  palette: ['white', 'green']
};

// Center the map and add the NDVI layer
Map.centerObject(aoi, 10);
Map.addLayer(ndviMedian.clip(aoi), ndviViz, 'Median NDVI 2022');

// Add the AOI layer to the map
Map.addLayer(aoi, {color: 'red'}, 'AOI');
```

## Generating Vegetation Health Report


```js
// Define the area of interest (AOI)
var aoi = ee.Geometry.Polygon([
  [[-122.5, 37.5], [-122.5, 37.7], [-122.2, 37.7], [-122.2, 37.5]]
]);

// Load the Landsat 8 ImageCollection
var collection = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR')
  .filterDate('2022-01-01', '2022-12-31')
  .filterBounds(aoi);

// Calculate NDVI for each image in the collection
var ndviCollection = collection.map(function(image) {
  var ndvi = image.normalizedDifference(['B5', 'B4']).rename('NDVI');
  return ndvi.copyProperties(image, ['system:time_start']);
});

// Calculate mean NDVI over the year
var meanNdvi = ndviCollection.mean();

// Calculate statistics for NDVI
var stats = meanNdvi.reduceRegion({
  reducer: ee.Reducer.mean(),
  geometry: aoi,
  scale: 30,
  maxPixels: 1e9
});

// Print NDVI statistics
print('Mean NDVI for 2022:', stats.get('NDVI'));

// Define visualization parameters for NDVI
var ndviViz = {
  min: 0,
  max: 1,
  palette: ['white', 'green']
};

// Center the map and add the NDVI layer
Map.centerObject(aoi, 10);
Map.addLayer(meanNdvi.clip(aoi), ndviViz, 'Mean NDVI 2022');

// Add the AOI layer to the map
Map.addLayer(aoi, {color: 'red'}, 'AOI');

```


## Detecting Forest Loss Over Time

```js
// Define the area of interest (AOI)
var aoi = ee.Geometry.Polygon([
  [[-122.5, 37.5], [-122.5, 37.7], [-122.2, 37.7], [-122.2, 37.5]]
]);

// Load the Hansen Global Forest Change dataset
var gfc = ee.Image('UMD/hansen/global_forest_change_2022_v1_10');

// Select the tree cover loss band
var treeLoss = gfc.select('lossyear');

// Define visualization parameters for tree cover loss
var lossViz = {
  min: 1,
  max: 22,
  palette: ['yellow', 'red']
};

// Center the map and add the tree cover loss layer
Map.centerObject(aoi, 10);
Map.addLayer(treeLoss.clip(aoi), lossViz, 'Tree Cover Loss 2001-2022');

// Add the AOI layer to the map
Map.addLayer(aoi, {color: 'red'}, 'AOI');


```
