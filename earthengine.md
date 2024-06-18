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
