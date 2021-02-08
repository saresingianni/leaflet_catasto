# Visualizzare le mappe del catasto terreni con Leaflet 

Esempio con di visualizzazione mappe dal Catasto delle Agenzie delle Entrate, Satelittare, Stradale e Toponomastica


### Demo 
* [demo](https://saresingianni.github.io/leaflet_catasto/) - Pagina index.html

### Prato della Valle Padova
![alt text immagine Padova Prato della Valle](https://github.com/saresingianni/leaflet_catasto/blob/main/img/padova.jpg?raw=true)


## Per iniziare

- Cosa è Leafle è una libreria  JavaScript per creare delle mappe intereattive
- a JavaScript library for interactive maps
- questo è il link di riferimento https://leafletjs.com/download.html
Vi elenci qui le libreire che ho utilizzato nel file demo index.html
* [Lealeft](https://leafletjs.com/download.html) - Link per il donwload della libreria principaòe
* [Leaflet-Pegman](https://unpkg.com/leaflet-pegman@0.0.8/leaflet-pegman.css) - Link Dipendenze per Open Steet
* [jszip](https://unpkg.com/jszip@3.1.5/dist/jszip.min.js) - Link per utilizzo jszip
* [json](https://unpkg.com/togeojson@0.16.0/togeojson.js) - Link per utilizzo json
* [json](https://unpkg.com/geojson-vt@3.0.0/geojson-vt.js) - Link per utilizzo geojson
* [KMZ](https://unpkg.com/leaflet-kmz@0.0.6/libs/KMZParser.js) - Link per utilizzo KMZParser
* [GeoJSON](https://unpkg.com/leaflet-kmz@0.0.6/libs/GridLayer.GeoJSON.js) - Link per utilizzo GeoJson
* [leaflet-transparency](https://unpkg.com/leaflet-transparency@0.0.3/leaflet-transparency.js) - Link per utilizzo sulla traparenza
* [Coordinate per catasto](https://cdnjs.cloudflare.com/ajax/libs/proj4js/2.6.2/proj4-src.min.js ) - Link per utilizzo sulle coordinate per il Catasto
* [display](dist/leaflet.zoomdisplay-src.js) - Link per utilizzo zoomDisplay
* [controller](dist/dist/Control.Geocoder.js) - Link per utilizzo Control.Geocoder
* [font](https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css) - Link per utilizzo Control.Font


## Siti per la documentazione dove ho preso spunto
* [Paolo Mistrangelo](https://medium.com/@p.mistrangelo/visualizzare-le-mappe-del-catasto-terreni-con-google-maps-473a44872962 ) -Paolo Mistrangelo Visualizzare le mappe del catasto terreni con Google Maps
* [Andrea Borroso](https://medium.com/tantotanto/le-mappa-castali-diventano-finalmente-utilizzabili-821db2f84533) Andrea Borroso
Le mappe catastali dell’Agenzia delle Entrate diventano finalmente utilizzabil
* [Agenzia delle Entrate](https://geoportale.cartografia.agenziaentrate.gov.it/age-inspire/srv/ita/catalog.search#/home) Agenzia delle Entrate
* [INSPIRE Geoportal](https://inspire-geoportal.ec.europa.eu/) INSPIRE Geoportal
* [INSPIRE Geoportal](https://geodati.gov.it/geoportale/) Repertorio Nazionale dei Dati Nazionale

### Risorse su Git Hub

* [Leaflet](https://github.com/Leaflet/Leaflet) Risorse Leaflet su Git Hub

* [Leaflet Opacità](https://github.com/dayjournal/Leaflet.Control.Opacity) Risorse Leaflet per controllo sulla Opacità su Git Hub
* [Leaflet posizione](https://github.com/domoritz/leaflet-locatecontrol) Risorse Leaflet per controllo della posizione

* [Leaflet posizione](https://github.com/Raruto/leaflet-transparency) Risorse Leaflet per la trasparenza

### Punti Salienti

```
style>
    html,
    body,
    .map {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
    }
  </style> viene inserita la mappa a tutto schermo
```
```
proj4.defs("WGS84", "+proj=longlat +ellps=WGS84 +datum=WGS84 +units=degrees");
proj4.defs("EPSG:6706", "+proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs");
Chiamata alle coordinate per il catasto
var opts = {
      map: {                    //45.398358 11.876553﻿
        center: [45.398358, 11.876553],//45°24′23″N 11°52′40″E (Mappa)
        zoom:17,                //45°24′04″N 11°56′32″E (Mappa)
        markerZoomAnimation: false,
        zoomControl: false,
      }, le opzioni per i vari layer
```
```
L.TileLayer.Catasto = L.TileLayer.extend({
    getTileUrl: function(coords) {
      //   console.log("coords.x "+coords.x);
      //   console.log("coords.y "+coords.y);
      //   console.log("coords.z "+coords.z);
         
         return "https://wms.cartografia.agenziaentrate.gov.it/inspire/wms/ows01.php?language=ita&service=WMS&version=1.3.0&request=GetMap&bbox="+bbox(coords.x,coords.y,coords.z)+"&crs=EPSG:6706&width=256&height=256&layers=province,CP.CadastralZoning,acque,CP.CadastralParcel,fabbricati,codice_plla,simbolo_graffa&styles=default&format=image/png&DPI=96&map_resolution=96&format_options=dpi:96&transparent=true"
        },
      getAttribution: function() {
          return "<a href='https://www.agenziaentrate.gov.it/portale/it/web/guest/schede/fabbricatiterreni/consultazione-cartografia-catastale/servizio-consultazione-cartografia'>Agenzie delle Entrate</a>"
      }
    });
        L.tileLayer.Catasto = function() {
  
       return new L.TileLayer.Catasto();
    }
    chiamata funzioni per agenzia delle entrate
```
```
   var baseLayers = {};
    baseLayers["MappaToponomastica"] = new L.TileLayer(opts.otmLayer.url, opts.otmLayer.options);
    baseLayers["MappaStradale"] = new L.TileLayer(opts.osmLayer.url, opts.osmLayer.options);
    baseLayers["Mappa Catastale"] = L.tileLayer.Catasto();
   //baseLayers["Posizione Attuale"] = map.locate({setView: true, maxZoom: 16});
   //new L.tileLayer(opts.catastoLayer.url(opts.map.center[0], opts.map.center[1]), opts.catastoLayer.options);
    baseLayers["ImmagineSatellitare"] = new L.TileLayer(opts.satelliteLayer.url, opts.satelliteLayer.options)
    
    var controlZoom = new L.Control.Zoom(opts.zoomControl); costruzione tabella menu chekpoint
    var baseLayers2 = {};
    var overlays = {
    "Sovrapposizione Catasto": ibrido
    };
    var controlLayers2= L.control.layers(baseLayers2, overlays).addTo(map);

    L.Control.Layers.include({
        getOverlays: function() {
        // create hash to hold all layers
        var control, layers;
        layers = {};
        control = this;

        // loop thru all layers in control
        control._layers.forEach(function(obj) {
        var layerName;

        // check if layer is an overlay
        if (obj.overlay) {
        // get name of overlay
        layerName = obj.name;
        // store whether it's present on the map or not
        return layers[layerName] = control._map.hasLayer(obj.layer);
        }
      });

      return layers;
    }
    
    }); cattura degli eventi
```
```
  //fetch dei risultati di ritorno
      fetch(testUrl)
        .then(res => res.json())
        .then((out) => {
        //console.log('Checkout this JSON! ', out);
        allegato=out.ALLEGATO;
        codice_comune=out.COD_COMUNE;
        denominazione=out.DENOM;
        foglio=out.FOGLIO
        num_particella=out.NUM_PART;
        sezione=out.SEZIONE;
        sigla_provincia=out.SIGLA_PROV;
        sviluppo=out.SVILUPPO;
        tipologia=out.TIPOLOGIA;
        /* utilizzo fecth per reuperare le chiamate presso l'Agenxia delle entrate

```
```
    map.on('click', onMapClick);
    //L.tileLayer.Catasto().addTo(map);
  function bbox(x,y,z) {
    //console.log("sono qui x "+x);
    //console.log("sono qui y "+y);
    //console.log("sono qui z "+z);
    bl_lng=tile2long(x,z);
		tr_lng=tile2long((x+1),z);
		bl_lat=tile2lat(y+1,z);
		tr_lat=tile2lat((y),z);
		bl=proj4("WGS84","EPSG:6706",[bl_lng,bl_lat]);
		tr=proj4("WGS84","EPSG:6706",[tr_lng,tr_lat]);
    //console.log("sono qui bl[1] "+bl[1]);
    //console.log("sono qui bl[0] "+bl[0]);
    //console.log("sono qui bl[0] "+bl[0]);
    return bl[1]+","+bl[0]+","+tr[1]+","+tr[0];
	}
	function tile2long(x,z) {
		return (x/Math.pow(2,z)*360-180);
	  }
	function tile2lat(y,z) {
    	var n=Math.PI-2*Math.PI*y/Math.pow(2,z);
    	return (180/Math.PI*Math.atan(0.5*(Math.exp(n)-Math.exp(-n))));
    }
  function Ibrido(tipo, acceso) {
      this.tipo = tipo;
      this.acceso = acceso;
   }parte finale per i calcol delle coordinate
    
