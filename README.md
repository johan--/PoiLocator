PoiLocator
==========

A tutorial that shows how to visualize and search points of interest with JavaScript and PTV xServer internet

http://oliverheilig.github.io/PoiLocator/

This tutorial requires only xServer and JavaScript, you don't need any database or middleware. One restriction is that this type solution only works for a limited number of POIs that can be completely loaded into the browser. I you have thousands or millions POIs, you need another set of tools, usually including a spatial database and a middleware. You should take a look at this tutorial then https://github.com/oliverheilig/SpatialTutorial/wiki

To run the sample you need an xServer internet token. 
You can get a trial access [here](http://xserver.ptvgroup.com/en-uk/products/ptv-xserver-internet/test/). 

The JavaScript Libraries i'm using:

* [Leaflet](http://leafletjs.com/) - a JavaScript library for mobile-friendly interactive maps 
* [Leaflet.NonTiledLayer](https://github.com/ptv-logistics/Leaflet.NonTiledLayer) - a Leaflet layer for single-tile WMS layers, needed for the xServer internet BaseMap.
* [Leaflet-pip](https://github.com/mapbox/leaflet-pip) - a simple point-in-polygon function in JavaScript to find all POIs within an isochrone 
* [lunr.js](http://lunrjs.com/) - a full text search engine in JavaScript to find all POIs matching a text.

## Set-up the base map
First, you need to setup-up your html to include a Leaflet map. This quick-start-guide shows the required steps http://leafletjs.com/examples/quick-start.html. To display the xServer basemap, you'll also have to add the NonTiledLayer and NonTiledLayer.WMS.js to your includes. The initial setup which displays the basemap in Hamburg:

```js
// set up the map
var attribution = '<a href="http://www.ptvgroup.com">PTV</a>, TOMTOM';
var mapLocation = new L.LatLng(53.550556, 9.993333); // HH
var xMapWmsUrl = 'https://xmap-eu-n-test.cloud.ptvgroup.com/WMS/WMS';
    
// create a map in the "map" div, set the view to a given place and zoom
var map = new L.Map('map').setView(mapLocation, 14);

// insert xMap back- and forground layers with sandbox-style
getXMapBaseLayers(xMapWmsUrl, "sandbox", token, attribution).addTo(map);
```

The helper function returns two leaflet layers that build-up the xServer basemap

```js
// returns a layer group for xmap back- and foreground layers
function getXMapBaseLayers(url, style, token, attribution) {
    var background = new L.TileLayer.WMS(url, {
        maxZoom: 19, minZoom: 0, opacity: 1.0, noWrap: true,
        layers: style? 'xmap-ajaxbg-' + style : 'xmap-ajaxbg',
        format: 'image/png', transparent: false,
        attribution: attribution
    });

    var foreground = new L.NonTiledLayer.WMS(url + "?xtok=" + token, {
        minZoom: 0, opacity: 1.0,
        layers: style ? 'xmap-ajaxfg-' + style : 'xmap-ajaxfg',
        format: 'image/png', transparent: true,
        attribution: attribution,
    });

    return L.layerGroup([background, foreground]);
}
```

## Prepare your data

Now we want to display our locations on the map. The easiest way for Leaflet is to provide the data as [GeoJson](http://geojson.org/). I'm having some old location data stored in a Microsoft Access database (.mdb). So i've written  a tool which reads the .mdb and writes it as text-file as GeoJson.

1. **If your soucrce table has a Longitude- and Latitude-field (or Lon,Lat or WGS_x,WGS_y or similar)** - Then you're fine. This is what Leaflet expects.
2. **If your data uses PTV coordinate formats (PTV_GEODECIMAL, PTV_MERCATOR, ...)** - Then you can use my little [GeoTransform](https://gist.github.com/oliverheilig/7029947) code snippet, which does the converstion for the various PTV formats. You can also try it online [here](http://jsil.org/try/#7029947). Before writing the point, you can convert it to Wgs84 with the Trans() function.
3. **If you have coordinates in other spatial reference systems** - Then you should try to find out what kind of coordinates these are and use some 3rd-party tools to transform into WGS84. Or just jump to step 4.
4. **If your data isn't geocoded (i.e. you only have addresses without coordinates)** - Then you can use PTV xLocate which is part of your xServer intenet subscription.

## Add your data to the map (in progess...))

## Search by proximity (in progess...)

## Search by text (in progess...)

