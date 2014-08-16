PoiLocator
==========

A tutorial that shows how to visualize and search points of interest with JavaScript and PTV xServer internet

http://oliverheilig.github.io/PoiLocator/

Unlike other tutorials (e.g. https://github.com/oliverheilig/SpatialTutorial/wiki), it doen't need any  middleware, but runs with only xServer and JavaScript. To run the sample you need an xServer internet token. 
You can get a trial access [here](http://xserver.ptvgroup.com/en-uk/products/ptv-xserver-internet/test/). 

The JavaScript Libraries i'm using:

* [Leaflet](http://leafletjs.com/) - a JavaScript library for mobile-friendly interactive maps 
* [Leaflet.NonTiledLayer](https://github.com/ptv-logistics/Leaflet.NonTiledLayer) - a Leaflet layer for single-tile WNS layers, needed for the xServer internet BaseMap.
* [Leaflet-pip](https://github.com/mapbox/leaflet-pip) - a simple point-in-polygon function in JavaScript to find all POIs within an isochrone 
* [lunr.js](http://lunrjs.com/) - a full text search engine in JavaScript to find all POIs matching a text.

## Set-up the base map

    // returns a layer group for xmap back- and foreground layers
    function getLayers(url, style, token, attribution) {
        var background = new L.TileLayer.WMS(url, {
            maxZoom: 19, minZoom: 0, opacity: 1.0,
            noWrap: true,
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


## Prepare your data (in progess...)

## Add your data to the map (in progess...)

## Search by proximity (in progess...)

## Search by text (in progess...)

