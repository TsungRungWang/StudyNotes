# Google Earth Engine

**https://earthengine.google.com/**



[Code Editor](https://code.earthengine.google.com/)

Official Get-Started Tutorial: https://developers.google.com/earth-engine/guides/getstarted

[Coding Best Practices](https://developers.google.com/earth-engine/guides/best_practices#avoid_eealgorithmsif). This page teaches how to code in GEE efficiently and avoid some bad practices.

[Task Manager](https://code.earthengine.google.com/tasks). Tasks like uploading and exporting to drive will appear here.

How to debug? https://developers.google.com/earth-engine/guides/debugging



This note is mainly written under the framework of the [official handbook](https://developers.google.com/earth-engine/guides/)



Other links and resources: 

- Google Earth (not only GEE) YTB channel: https://www.youtube.com/googleearth



## Geodata

### Landsat

Landsat collections: https://developers.google.com/earth-engine/datasets/catalog/landsat

##### Overview

| Mission   | Main page                                                    |
| --------- | ------------------------------------------------------------ |
| Landsat 9 | https://developers.google.com/earth-engine/datasets/catalog/landsat-9 |
| Landsat 8 | https://developers.google.com/earth-engine/datasets/catalog/landsat-8 |
| Landsat 7 | https://developers.google.com/earth-engine/datasets/catalog/landsat-7 |



##### Level and Collections

About Landsat collection structures: https://developers.google.com/earth-engine/guides/landsat

| Mission   | Time range                                  | Best quality imagery                                         | Notes                            |
| --------- | ------------------------------------------- | ------------------------------------------------------------ | -------------------------------- |
| Landsat 9 | 2021-10-31T00:00:00Z to present             | [Level 2, Collection 2, Tier 1](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2) |                                  |
| Landsat 8 | 2013-03-18T15:58:14Z to present             | [Level 2, Collection 2, Tier 1](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LC09_C02_T1_L2) |                                  |
| Landsat 7 | 1999-05-28T01:02:17Z to 2023-02-04T01:41:07 | [Level 2, Collection 2, Tier 1](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_LE07_C02_T1_L2) | **ETM+ failed on May 31, 2013!** |



##### Scale Factors

Note that there are scale factors in Landsat bands!!!

The following code applies for Landsat 8 & 9. There is slight difference in thermal bands of Landsat 7. 

```javascript
// Applies scaling factors.
function applyScaleFactors(image) {
  var opticalBands = image.select('SR_B.').multiply(0.0000275).add(-0.2);
  var thermalBands = image.select('ST_B.*').multiply(0.00341802).add(149.0);
  return image.addBands(opticalBands, null, true)
              .addBands(thermalBands, null, true);
}

dataset = dataset.map(applyScaleFactors);
```

|                                            | Original              | After Scaling |
| ------------------------------------------ | --------------------- | ------------- |
| `SR_B.`: $\times2.75\times10^{-5}+0.2$     | $1$ to 65455 (16 bit) | -0.2 to 1.6   |
| `SR_B10`: $\times3.41802\times10^{-3}+149$ | 0 to 65535 (16 bit)   | 149 to 373 K  |



### Sentinel-2

https://developers.google.com/earth-engine/datasets/catalog/sentinel-2, harmonized

[Sentinel-2 User Handbook](https://sentinel.esa.int/documents/247904/685211/Sentinel-2_User_Handbook) by ESA

[Level-2A data in GEE](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED)

[Sentinel-2 Bands](https://en.wikipedia.org/wiki/Sentinel-2#Spectral_bands) from Wikipedia

Repeating cycle: ~5 days

##### Scale Factors

Scale factors for `B*` bands are all 10000.







#### Harmonized Landsat Sentinel-2 (HLS)

https://hls.gsfc.nasa.gov/

https://www.earthdata.nasa.gov/esds/harmonized-landsat-sentinel-2

https://www.sciencedirect.com/science/article/pii/S0034425718304139

https://lpdaac.usgs.gov/documents/1007/HLS_User_Guide_V15_provisional.pdf





### Sentinel-1

Sentinel-1 Algorithms: https://developers.google.com/earth-engine/guides/sentinel1

| Mission    | Time range                      | Imagery                                                      |
| ---------- | ------------------------------- | ------------------------------------------------------------ |
| Sentinel-1 | 2014-10-03T00:00:00Z to present | [S1 GRD](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S1_GRD) |



Repeating cycle: 12 days

See [Sentinel-1 Orbit](https://sentinels.copernicus.eu/web/sentinel/missions/sentinel-1/satellite-description/orbit)



Common processing procedure

```javascript
var collectionName = ee.ImageCollection('COPERNICUS/S1_GRD')
  .filterBounds(aoi)
  .filterDate(time_start, time_end)
  .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VH'))
  .filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'))
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .select('VH')
  .mosaic()
  .sort('system:time_start', true);
```



Orbit number (path):

`relativeOrbitNumber_start:`

Frame information is not included in the metadata as the frames are manually separated into images.





### Elevation



### PALSAR-2

PALSAR-2 ScanSAR Level 2.2

https://developers.google.com/earth-engine/datasets/catalog/JAXA_ALOS_PALSAR-2_Level2_2_ScanSAR



---



## List

Generate blank list: 

```javascript
var myList = ee.List.sequence(1, 10);
```

Generate empty list:

```javascript
var emptyList = ee.List([]);
```



Add items to a list. Note that `=` must be applied otherwise there would be no update on the list. 

```javascript
var list = list.add(item);
```



Convert an image collection to a list:

```javascript
var imageList = imageCollection.toList(imageCollection.size());
```

Vise versa, convert a list of images to an image collection:

```javascript
var imageCollection = ee.ImageCollection.fromImages(imageList);
```

or manually: 

```javascript
var imageCollection = ee.ImageCollection.fromImages([image1, image2, image3]);
```





Visit items in a list by index:

```javascript
var item = list.get(i);
```

Note that an item selected by `.get()` will not retain its data type. So a typecast must be applied before using it as, for example, an image:

```javascript
var image = ee.Image(list.get(i));
```









## Functions

```javascript
var functionName = function(parameters) {
	// Do something
	return 0; // A return is required
};
```

Note that a function is also a variable (?)

A function can also be written in another form:

```javascript
function functionName(parameters) {
	// Do something
	return 0; // A return is required
};
```

However only the first form is recommended in GEE, whereas you can also see form two in some scripts.







![img](https://zhaoxuhui.top/assets/images/blog/content/2020-10-14-01.PNG)

## Image

If you want to access a certain property in an image, simply `imageName.propertyName`

(however I saw somewhere else that it should be `image.get()`?)

The properties of an image is stored as a dictionary





## ImageCollection





## Geometry



## Filters



Select a band out of an image: `.select(bandName)`. Note that `bandName` can contain wildcard like `.` and `*`

`mosaic()` and `qualityMosaic()`



---

## Date and Time

https://developers.google.com/earth-engine/apidocs/ee-date

A date can be either a `ee.Date` object or simply be a string(?really)



`advance(60, 'day');`, used to calculate time offset





## Assets Management















## Showing Results

### Print in Console



### Add Layers to the Map

```javascript
var visParams = {min: -3, max: 3, palette: ['blue', 'white', 'red']};
Map.addLayer(DataName, visParams, 'Layer Name');
```

### Centering

```javascript
Map.centerObject(AOI, ZoomLevel);
```



```javascript
Map.setCenter(lon, lat, ZoomLevel);
```





## Export to Drive

`Export.image.toDrive`: https://developers.google.com/earth-engine/apidocs/export-image-todrive



### Using Google Cloud

https://developers.google.com/earth-engine/guides/exporting_map_tiles



About Cloud Storage buckets: https://cloud.google.com/storage/docs/buckets/

Google Cloud main page: https://console.cloud.google.com/



## Handle with Python

https://developers.google.com/earth-engine/guides/python_install



```powershell
pip install earthengine-api
```

此外还要安装`gcloud`

```powershell
(New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe")

& $env:Temp\GoogleCloudSDKInstaller.exe
```

参考: https://cloud.google.com/sdk/docs/install



然后就是激活

```python
ee.Authentiate()
```

也可以在cmd中激活, 但是这种方式容易出现`earthengine`没有被添加到`PATH`的情况

```powershell
earthengine authenticate
```



激活后的密钥放在`%UserProfile%\.config\earthengine\credentials`中. 如果想要反激活的话只需移除密钥文件即可

> 此处`%UserProfile%`为`C:\Users\wcrpk`

### Set Python Proxy

这一步是确保python的流量也走梯子, 以访问Google服务

```python
import os
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:5555'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:5555'
```

代理的地址和端口可以打开clash或者在Windows设置中的“代理”查看. 注意clash有随机端口模式, 可以关掉. 

### Initialization

```python
import ee
ee.Initialize()
```



## Using CLI

https://cloud.google.com/sdk/auth_success





