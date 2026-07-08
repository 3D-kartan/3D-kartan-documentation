# JSON Config file parameters

The diffrent parts of the config for the application is defined in arrays.

For a full example file see: [FULL JSON CONFIG EXAMPLE](#full-json-config-example)

The following array parameters are required:

- Camera
- Proj4Defs
- Terrain
- Background Layers 
- Styles

---

### Camera

Start position of the where application initiates, should be in `EPSG:4326`. 

The `minimimZoomDistance` and `maximumZoomDistance` are required to be set.

```JSON
"camera": [
    {
      "startExtent": [
        15.671103064362,
        60.21484248223163,
        16.312895699095435,
        59.91045413122643
      ]
    },
    {
      "minimumZoomDistance": 1,
      "maximumZoomDistance": 60000
    }
  ],
```

---

### Proj4Defs

Here is the coordinate system defined. EPSG:4326 must be the first coordiante system in the array. The second one is the primary used in the model when displaying coordinates. I.e. when toggling it on in the top right menu. 

More systems can be added to the array to enable copying in diffrent systems.

Important note: for the model to work you need at least `EPSG:4326` and `EPSG:3006` specified. Since the terrain-section tool uses both systems. An ideal config is as below: 1. `EPSG:4326`, 2. `Your local Sweref EPSG`, 3. `EPSG:3006`.

```JSON
 "proj4Defs": [
    {
      "code":      "EPSG:4326",
      "alias":     "urn:ogc:def:crs:EPSG::4326",
      "projection": "+proj=longlat +datum=WGS84 +no_defs",
      "label":    "WGS 84 (lat/lon)"
    },
    {
      "code":      "EPSG:3010",
      "alias":     "urn:ogc:def:crs:EPSG::3010",
      "projection": "+proj=tmerc +lat_0=0 +lon_0=16.5 +k=1 +x_0=150000 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs",
      "label": "SWEREF 99 16 30"
    },
    {
      "code":      "EPSG:3006",
      "alias":     "urn:ogc:def:crs:EPSG::3006",
      "projection": "+proj=utm +zone=33 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs +type=crs",
      "label": "SWEREF 99 TM" 
    }
  ],
```

---
### Logo

If configured the model will display a logo. 

```JSON
  "logo": [
    {
      "useLogo": true,
      "logoName": "logo.png",
      "logoLocation": "top-right-menu"
    }
  ],
```
Possible values for `"logoLocation"` is either `"top-right-menu"` or `"bottom-middle"`.

Place your logo in the /images/png folder. Rectangular images is best suited for use.  

`"top-right-menu"` - Is more discreet and places the logo in the menu at the top right of the screen.

`"bottom-middle"` - Highlights the logo better and places it in the bottom middle of the screen.

---

### Searchbar 

Define if the application should have a searchbar that searches adresses in a postgis db. 

Values: `true` or `false`

```JSON
"searchbar" : [ 
    { 
      "active" : true
    } 
  ],
```

For more information about how to set up the 3D-kartan-backend that handles searches to the database, see section [3D-kartan-backend](3D-kartan-backend.md).

---

### ion id

If Cesium ion is wished to be used for loading terrain or 3D-tilesets, then the `ionId` array need to be included.

Possible values for `useIonId` are `true` or `false` and your id string.


```JSON
"ionId": [
    {
      "useIonId": true,
      "id": "YOUR_ION_KEY"
    }
  ],
```

---

### Terrain

Which terrain provider that should be used. Possible values for `terrainType`:

`world` - the least detaild terrain

```JSON
  "terrain": [
    {
      "terrainType": "world"  
    }
  ],
```


`ion` - use a specified terrain stored i Cesium ion, make sure to also specify `assetId`

```JSON
  "terrain": [
    {
      "terrainType": "ion",        
      "assetId": 2202832    
    }
  ],
```

`url` -  use a locally served terrain via IIS for example. Make sure to specify `url` location. The following folder structure are in the example below. 

/dist-folder containing the app

/terrain-folder contating the tiles and layer.json



```JSON
  "terrain": [
    {
      "terrainType": "url",
      "url": "./terrainUrl"
    }
  ],
```

`ellipsoid` - use the WGS 84 ellipsoid, important note: some functions in the model require terrain tiles for sampling height, these functions does not work with `ellipsoid` terrain since it does not supply terrain tiles.


```JSON
  "terrain": [
    {
      "terrainType": "ellipsoid"  
    }
  ],
```
---

### Toolbar

The toolbar includes a variety of tools that can be included or excluded in the application. 

These are the following possible tools:

`info` - Popup info, can be set to show navigation instruction image. Set your own popup text. Remeber it's json and that line break in strings is not allowed.

`find-north` - Rotates the camera so it is facing north.

`zoom-in` - Zoom in

`zoom-out` - Zoom out

`home` - Moves the camera to the initial start location.

`forms` - Adds forms functionallity, the admin configure submission forms in the forms admin backend. The user can submit answers to those forms. The forms-tool requries the backend to be set up. See [3D-kartan-backend](3D-kartan-backend.md).
`"name"` is the visible name of the form to the user.`"id"` id string of the form to be loaded from the backend.

```JSON
  {
    "toolName": "forms",
    "toolTip": "Öppna formulärvy",
    "iconVar": "--black-icon-share",
    "active": true,
    "forms": [
    { "name": "Trygghetsformulär", "id": "form-id-string" },
    { "name": "Återvinningsformulär", "id": "form-id-string" },
    { "name": "Reparationsformulär", "id": "form-id-string" }
    ]
  },
```


`bookmarks` - Quick zooms to specified locations. These locations are set in the `index.json` file. Se this example:
```JSON
{
    "toolName": "bookmarks",
    "toolTip": "Öppna snabbzooms bokmärken",
    "iconVar": "--black-icon-bookmarks",
    "active": true,
    "locations":[
      {
        "name": "Tumba",
        "position": 
          {
          "lng": 17.864741,
          "lat": 59.198933,
          "height": 900
          }
      },
      {
        "name": "Tullinge",
        "position": 
          {
          "lng": 17.886863,
          "lat": 59.200208,
          "height": 800
          }
      },
    ]
  },
```
The `"position"` object is required. It is also possible to set `"orientation"`, following this structure: 
```JSON
 "position": {
    "lng": 17.886863,
    "lat": 59.200208,
    "height": 800
    },
 "orientation": {
    "heading": 0,
    "pitch": -45,
    "roll": 0
    }
```

`sun-study` - Set the desired time and date for accurate shadows in the model. Inital time value are from your devices (pc or mobile).

`pedestrian-mode` - Move around in "first person". 

`hide-buildings` - Hide 3D-tiles objects via middle click.

`draw-3d` - Draw 3D polygons. Drawn polygon works with sun-study and casts shadows. Creates a flat floor on the lowest corner height, and a fixed "roof" on the height from the user input height above ground (calculated by the lowest cornerpoint + user input value). This means it's possible to create bridge like polygons over valleys.

`measure` - Measure line, area and height. The measurement is point (x,y,z) to point (x,y,z) based. Meaning there is no draped values. 

`placement` - Place 3D-objects in the model. Example of models that can be used: trees or buildings.

`terrain-section` - Draw terrain section with equidistant- or interval placed points. CSV exports are available in configured `Proj4Defs`

If the tool json-object is included and `active: true` then the tool button will load. If `active: false` it will not load, it will also not load if the json-object is excluded from the config file.

Here is a complete toolbar array including all tools:


```JSON
"toolbar": [ 
  {
    "toolName":   "info",
    "toolTip":    "Information",
    "iconVar":    "--black-icon-info", 
    "actionType": "popup",
    "showNavigationImg": true,
    "popupText":  "Höjderna i modellen är indikativa eftersom terrängen är något förenklad och bör inte användas i beslut där hög noggranhet krävs. Har du ett ärende som kräver höjdberäkning med hög noggranhet? Kontaka Kart & Mät på kartomat@avesta.se",
    "active":     true
  },
  {
    "toolName": "find-north",
    "toolTip": "Hitta norr",
    "iconVar": "--black-icon-navigation",
    "actionType": "rotateNorth",
    "active": true
  },
  {
    "toolName": "zoom-in",
    "toolTip": "Zooma in",
    "iconVar": "--black-icon-add",
    "actionType": "zoomIn",
    "active": true
  },
  {
    "toolName": "zoom-out",
    "toolTip": "Zooma ut",
    "iconVar": "--black-icon-remove",
    "actionType": "zoomOut",
    "active": true
  },
  {
    "toolName": "home",
    "toolTip": "Zooma till startvy",
    "iconVar": "--black-icon-home",
    "actionType": "goHome",
    "active": true
  },
  {
    "toolName": "forms",
    "toolTip": "Öppna formulärvy",
    "iconVar": "--black-icon-share",
    "active": true,
    "forms": [
    { "name": "Trygghetsformulär", "id": "form-id-string" },
    { "name": "Återvinningsformulär", "id": "form-id-string" },
    { "name": "Reparationsformulär", "id": "form-id-string" }
  ]
  },
  {
    "toolName": "bookmarks",
    "toolTip": "Öppna snabbzooms bokmärken",
    "iconVar": "--black-icon-bookmarks",
    "active": true,
    "locations":[
      {
        "name": "Tumba",
        "position": 
          {
          "lng": 17.864741,
          "lat": 59.198933,
          "height": 900
          }
      },
      {
        "name": "Tullinge",
        "position": 
          {
          "lng": 17.886863,
          "lat": 59.200208,
          "height": 800
          }
      },
    ]
  },
  {
    "toolName": "sun-study",
    "toolTip": "Solstudie",
    "iconVar":"--black-icon-sun",
    "active": true
  },
  {
    "toolName": "pedestrian-mode",
    "toolTip": "Fotgängarläge",
    "iconVar":"--black-icon-pedestrian",
    "active": true
  },
  { 
    "toolName": "hide-buildings", 
    "toolTip": "Göm byggnader", 
    "iconVar":"--black-icon-visibility-off", 
    "active": true
  },
  { 
    "toolName": "draw-3d", 
    "toolTip": "Rita 3D-polygoner", 
    "iconVar":"--black-icon-cube", 
    "active": true
  },
  {
    "toolName":   "measure",
    "toolTip":    "Mät area & distans",
    "iconVar":    "--black-icon-measure",
    "actionType": "measure",
    "active":     true
  },
  {
    "toolName":   "placement",
    "toolTip":    "Placera 3D-modeller",
    "iconVar":    "--black-icon-place-model",
    "actionType": "panel",
    "active":     true
  },
  {
    "toolName":   "terrain-section",
    "toolTip":    "Rita terrängprofil",
    "iconVar":    "--black-icon-landscape",
    "actionType": "panel",
    "active":     true
  },
],
```

  ---

### Projects

The application has support for a project menu.

To utilize the built in project menu used for displaying planned projects in the city. Simply add the `"projects"` array.

Each project requires a set of parameters to be set. These are: `"name"`, `"start-location"` and `"content"`.

`"terrainClipping"` and `"tilsetClipping"` are optional and can be set with either a geojson or a set of coordinates. See this example:

```JSON
  "terrainClipping": {
    "url": "./projects/project_1/Klippmask_Albytappan.geojson",
    "featureIndex": 0,
    "polygonIndex": 0 
  },
   "tilesetClipping": {
    "positions": [[17.82,59.20],[17.82,59.19],[17.84,59.19],[17.84,59.20]]
  },
```
If you want to use the same polygon for both clippings it´s enough to set either `"terrainClipping"` or `"tilsetClipping"`.

`"featureIndex"` and `"polygonIndex"` are optinal, if not set the first polygon is used in the geojson. These parameters can be used to select a specific feature if there are multiple features or select a specific polygon if there is a multipolygon. 

Important note: index 0 is the first object. Polygons with holes are not supported, the holes will be ignored.


The `"terrainClipping"` are optional but requries `"inverse-terrain"`, `"inverse-tileset"`, `"terrain-enableAtStart"`, `"tileset-enableAtStart"` to be set.
Possible values for these are `true` or `false`.

`"inverse-*" : true` - Everying outside the `"terrainClipping"` polygon is hidden.


`"inverse-*" : false` - Everying inside the `"terrainClipping"` polygon is hidden.

`"*-enableAtStart" : true` - The function is toggled on by default.


`"*-enableAtStart" : false` - The function is toggled off by default.

Important note: for either `terrainclipping` or `tilesetclipping` to be present. Both the `inverse`-parameter and `enableAtStart`-parameter has to be configured.

The camera can also be locked to look at the project if `"lock-camera": true`. The camera locking is taking input from the pins coordinates which mean the `"pin"` property also has to be set.

Here is an example of a configuerd project:

```JSON
"projects" : [
    {
      "name": "Projekt busstorget Avesta",
      "html-description": "<p>Här är en bild:</p><img src=\"./projects/project_1/imagery/gagata.JPG\" alt=\"Beskrivning\" >",
      "start-location": {
        "position": {
          "lng": 16.166,
          "lat": 60.128,
          "height": 5000
        },
        "orientation": {
          "heading": 0,
          "pitch": -45,
          "roll": 0
        }
      },
      "pin": { 
          "lng": 16.166,
          "lat": 60.128,
          "height": 5000
      },
      "content": [
        {
          "name": "Åsbo Dalahästen",
          "type": "tileset",
          "url": "buildings/avesta/asbo/tileset/tileset.json",
          "options": { "maximumScreenSpaceError": 16 },
          "visible-at-start": true,
          "heightOffset": 20
        },
        {
          "name": "True Ortho",
          "type": "imagery",
          "provider": "WMS",
          "url": "MY_WMS_URL",
          "layers": "trueOrtho",
          "parameters": {
            "format": "image/png",
            "transparent": true
          },
          "visible-at-start": false
        }
      ],
      "terrainClipping": {
        "positions": [[17.82,59.20],[17.82,59.19],[17.84,59.19],[17.84,59.20]]
      },
      "inverse-terrain": false,
      "inverse-tilesets": false,
      "terrain-enableAtStart": false,
      "tileset-enableAtStart": true,
      "lock-camera": true
      }
    ],
```

The `html-description` is optional and can be used to provide a description to the project. It has support for including images. Make sure to put the images in the right folder.

The `pin` is optional, if set i places a pin at the specified location. Clicking on the pin in the model opens the desired project. 

The 3D-tiles layers specified under `content` can be set to be visible at project inition via `visible-at-start: true`. It is also possible to set the the Cesium `maximumScreenSpaceError`. If the 3D-tileset needs to be offset that is also possible via the `heightOffset`, both positive and negative values are accepted for example `20.3` or `-20.3`.


---

### Background Layers

This array sets the baselayers for the model. 

Set the Key-Value pair `primary:true` on the baselayer that should be loaded as the primary baslayer

`style` sets the icon displayed in the layer menu. There are a set of icons included. It is possible to add more icons. The value of `style` refer to the array `styles` where icon paths are set. See [Styles](#styles).

```JSON
 "backgroundLayers": [
    {
      "name": "OpenStreetMap",
      "type": "OSM",
      "primary": true,
      "style": "osm"
    },
    {
      "name": "Ortofoto WMS",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "Ortofoto_0.16",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "style": "orto_farg"
    },
    {
      "name": "Topografiska kartan WMS",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "topowebbkartan",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "style": "karta_farg"
    },
    {
      "name": "Topografiska kartan nedtonad WMS",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "topowebbkartan_nedtonad",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "style": "karta_gra"
    }
  ],
```

---

### Styles

The styles array sets the icon names and paths used for the [Background layers](#background-layers). It is possible to add more icon styles.
Simply put your icon in the images/icons folder and add it in the array.

```JSON
  "styles": [
    {
      "name": "orto_farg",
      "img": "orto_farg.png"
    },
    {
      "name": "karta_farg",
      "img": "karta_farg.png"
    },
    {
      "name": "karta_gra",
      "img": "karta_gra.png"
    },
    {
      "name": "osm",
      "img": "osm.png"
    }
  ],
```

---

## Layer menu

The layer menu consits of the following types: 3D-tiles or WMS. 

Each type has its own array. [Wms Layers](#wms-layers). [3D-tilesets](#tilesets).

A layer must belong to a [Group](#groups).

---

### Wms-layers

Here are additional WMS-layer configured. They are placed above the [Background Layer](#background-layers) in the drawing order. An additional parameter here is the `group` value, see [Groups](#groups).

```JSON
  "wmsLayers": [
    {
      "name": "WMS_NAME_DISPLAYED_IN_LAYER_MENU",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "WMS_LAYER_NAME",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "group": "WMS"
    }
  ],
```

---

### Tilesets

3D-tilesets can be used in the model and can be loaded locally or via Cesium ion. Which 3D-tilesets that should be included in the application are specified in the tilsets array, except the once that belong to a project in the [Project menu](#projects).

`name` - Unique id name.

`title` - Optional title, if set the title value will be displayed as the layer name in the layer menu. If not set the `name` will be displayed.

`group` - The group the 3D-tileset should belong to.

`url` - The url to your local 3D-tileset.

`options` - Has support for setting the Cesium `maximumScreenSpaceError`.

`visble-at-start` - If the layer should be displayed from the intion of the model. Possible values `true` or `false`.

`ionAssetId` - Needed to be set if you wish to load a tileset from ion. Remeber to set the [ion id](#ion-id)!

`infoText` - If set the layer can display some info text belonging to the layer, is displayed by pressing the "three dots" button on the layer.

Example config:

```JSON
 "tilesets": [
    {
      "name": "Avesta Centrum",
      "title":"Avesta Centrum",
      "group": "avesta",
      "url": "buildings/avesta/avestaCentrum_ny2_clean/tileset/tileset.json",
      "options": { "maximumScreenSpaceError": 16 },
      "visible-at-start": true
    },
    {
      "name": "Bergsnäs",
      "group": "avesta1",
      "url": "buildings/avesta/bergsnas_clean/tileset/tileset.json",
      "options": { "maximumScreenSpaceError": 16 },
      "visible-at-start": true
    },
    {
      "name": "Centrala Krylbo",
      "group": "avesta2",
      "url": "buildings/avesta/centralaKrylbo_clean/tileset/tileset.json",
      "options": { "maximumScreenSpaceError": 16 },
      "visible-at-start": true
    },
    {
      "name": "Ion Asset",
      "group": "avesta",
      "ionAssetId": MY_ASSET_ID_IN_ION,
      "options": { "maximumScreenSpaceError": 16 },
      "infoText": "Laddat från ion."
    },
 ],

```

---

### Groups 

A layer ([WMS-Layer](#wms-layers) or [Tileset](#tilesets)) must belong to a group. Which are set up in the groups array. Groups can have subgrups and a subgrup can have a subgrup to itself. Per default three group levels are supported. If you wish to have more you have to change the core of 3D-kartan. 

`name` - Unique id name.

`title` - The group name to be displayed in the layer menu.

`groups` - Optional, used to display subgrups.

Example config: 

```JSON
  "groups": [
    {
      "name": "Buildings",
      "title": "Byggnader",
      "groups": [
        {
          "name": "avesta",
          "title": "Avesta nedtonad",
          "groups": [
            {
              "name": "avesta1",
              "title": "Avesta nedtonad1"
            }
          ]
        }
      ]
    },
    {
      "name": "avesta_textur",
      "title": "Avesta textur"
    },
    {
      "name": "WMS",
      "title": "Kartlager"
    }
  ],
```

---

## FULL JSON CONFIG EXAMPLE 

```JSON


{
  "camera": [
    {
      "startExtent": [
        15.671103064362,
        60.21484248223163,
        16.312895699095435,
        59.91045413122643
      ]
    },
    {
      "minimumZoomDistance": 1,
      "maximumZoomDistance": 60000
    }
  ],
  "proj4Defs": [
    {
      "code":      "EPSG:4326",
      "alias":     "urn:ogc:def:crs:EPSG::4326",
      "projection": "+proj=longlat +datum=WGS84 +no_defs",
      "label":    "WGS 84 (lat/lon)"
    },
    {
      "code":      "EPSG:3010",
      "alias":     "urn:ogc:def:crs:EPSG::3010",
      "projection": "+proj=tmerc +lat_0=0 +lon_0=16.5 +k=1 +x_0=150000 +y_0=0 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs",
      "label": "SWEREF 99 16 30"
    },
    {
      "code":      "EPSG:3006",
      "alias":     "urn:ogc:def:crs:EPSG::3006",
      "projection": "+proj=utm +zone=33 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs +type=crs",
      "label": "SWEREF 99 TM" 
    }
  ],
  "searchbar" : [ 
    { 
      "active" : true
    } 
  ],
  "ionId": [
    {
      "useIonId": true,
      "id": "MY_ION_ID"
    }
  ],
  "terrain": [
    {
      "terrainType": "ion",        
      "assetId": 2202832    
    }
  ],
  "logo": [
    {
      "useLogo": true,
      "logoName": "logo.png",
      "logoLocation": "top-right-menu"
    }
  ],
  "toolbar": [
    {
      "toolName":   "info",
      "toolTip":    "Information",
      "iconVar":    "--black-icon-info",
      "actionType": "popup",
      "showNavigationImg": true,
      "popupText":  "Höjderna i modellen är indikativa eftersom terrängen är något förenklad och bör inte användas i beslut där hög noggranhet krävs. Har du ett ärende som kräver höjdberäkning med hög noggranhet? Kontaka Kart & Mät på enhetsmailen@exempel.se",
      "active":     true
    },
    {
      "toolName": "find-north",
      "toolTip": "Hitta norr",
      "iconVar": "--black-icon-navigation",
      "actionType": "rotateNorth",
      "active": true
    },
    {
      "toolName": "zoom-in",
      "toolTip": "Zooma in",
      "iconVar": "--black-icon-add",
      "actionType": "zoomIn",
      "active": true
    },
    {
      "toolName": "zoom-out",
      "toolTip": "Zooma ut",
      "iconVar": "--black-icon-remove",
      "actionType": "zoomOut",
      "active": true
    },
    {
      "toolName": "home",
      "toolTip": "Zooma till startvy",
      "iconVar": "--black-icon-home",
      "actionType": "goHome",
      "active": true
    },
    {
      "toolName": "forms",
      "toolTip": "Öppna formulärvy",
      "iconVar": "--black-icon-share",
      "active": true,
      "forms": [
        { "name": "Trygghetsformulär", "id": "form-id-string" },
        { "name": "Återvinningsformulär", "id": "form-id-string" },
        { "name": "Reparationsformulär", "id": "form-id-string" }
      ]
    },
    {
      "toolName": "bookmarks",
      "toolTip": "Öppna snabbzooms bokmärken",
      "iconVar": "--black-icon-bookmarks",
      "active": true,
      "locations":[
        {
          "name": "Tumba",
          "position": 
            {
            "lng": 17.864741,
            "lat": 59.198933,
            "height": 900
            }
        },
        {
          "name": "Tullinge",
          "position": 
            {
            "lng": 17.886863,
            "lat": 59.200208,
            "height": 800
            }
        },
      ]
    },
    {
      "toolName": "sun-study",
      "toolTip": "Solstudie",
      "iconVar": "--black-icon-sun",
      "active": true
    },
    {
      "toolName": "pedestrian-mode",
      "toolTip": "Fotgängarläge",
      "iconVar": "--black-icon-pedestrian",
      "active": true
    },
    { 
      "toolName": "hide-buildings", 
      "toolTip": "Göm byggnader", 
      "iconVar": "--black-icon-visibility-off", 
      "active": true
    },
    { 
      "toolName": "draw-3d", 
      "toolTip": "Rita 3D-polygoner", 
      "iconVar": "--black-icon-cube", 
      "active": true
    },
    {
      "toolName": "measure",
      "toolTip": "Mät area & distans",
      "iconVar": "--black-icon-measure",
      "actionType": "measure",
      "active": true
    },
    {
      "toolName": "placement",
      "toolTip": "Placera 3D-modeller",
      "iconVar": "--black-icon-place-model",
      "actionType": "panel",
      "active": true
    }
  ],
  "projects" : [
    {
      "name": "Projekt busstorget Avesta",
      "html-description": "<p>Här är en bild:</p><img src=\"/projects/project_1/imagery/gagata.JPG\" alt=\"Beskrivning\" >",
      "start-location": {
        "position": {
          "lng": 16.166,
          "lat": 60.128,
          "height": 5000
        },
        "orientation": {
          "heading": 0,
          "pitch": -45,
          "roll": 0
        }
      },
      "pin": { 
          "lng": 16.166,
          "lat": 60.128,
          "height": 5000
      },
      "content": [
        {
          "name": "Avesta Centrum",
          "type": "tileset",
          "url": "buildings/avesta/avestaCentrum_ny2/tileset/tileset.json",
          "options": { "maximumScreenSpaceError": 16 },
          "visible-at-start": true
        },
        {
          "name": "Åsbo",
          "type": "tileset",
          "url": "buildings/avesta/asbo/tileset/tileset.json",
          "options": { "maximumScreenSpaceError": 16 },
          "visible-at-start": true
        },
        {
          "name": "Plankarta",
          "type": "imagery",
          "provider": "WMS",
          "url": "MY_WMS_URL",
          "layers": "trueOrthoTT",
          "parameters": {
            "format": "image/png",
            "transparent": true
          },
          "visible-at-start": false
        }
      ],
      "terrainClipping": {
      "url": "./projects/project_1/Klippmask_Terrain.geojson",
      "featureIndex": 0,
      "polygonIndex": 0 
      },
      "tilesetClipping": {
      "url": "./projects/project_1/Klippmask_Buildings.geojson",
      "featureIndex": 0,
      "polygonIndex": 0 
      },
      "inverse-terrain": false,
      "inverse-tilesets": false,
      "terrain-enableAtStart": true,
      "tileset-enableAtStart": false,
      "lock-camera": true
    },
  ],
  "backgroundLayers": [
    {
      "name": "OpenStreetMap",
      "type": "OSM",
      "primary": true,
      "style": "osm"
    },
    {
      "name": "Ortofoto WMS",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "Ortofoto_0.16",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "style": "orto_farg"
    },
    {
      "name": "Topografiska kartan WMS",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "topowebbkartan",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "style": "karta_farg"
    },
    {
      "name": "Topografiska kartan nedtonad WMS",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "topowebbkartan_nedtonad",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "style": "karta_gra"
    }
  ],
  "wmsLayers": [
    {
      "name": "True Ortofoto TT 2020",
      "type": "WMS",
      "url": "MY_WMS_URL",
      "layers": "trueOrthoTT",
      "parameters": {
        "transparent": true,
        "format": "image/png"
      },
      "group": "WMS"
    }
  ],
  "groups": [
    {
      "name": "Buildings",
      "title": "Byggnader",
      "groups": [
        {
          "name": "avesta",
          "title": "Avesta nedtonad"
        },
        {
          "name": "WMS",
          "title": "Kartlager"
        }
      ]
    }
  ],
  "styles": [
    {
      "name": "orto_farg",
      "img": "orto_farg.png"
    },
    {
      "name": "karta_farg",
      "img": "karta_farg.png"
    },
    {
      "name": "karta_gra",
      "img": "karta_gra.png"
    },
    {
      "name": "osm",
      "img": "osm.png"
    }
  ],
  "tilesets": [
    {
      "name": "Ion Asset",
      "group": "avesta",
      "ionAssetId": MY_ION_ID,
      "options": { "maximumScreenSpaceError": 16 },
      "infoText": "Laddat från ion."
    },
    {
      "name": "Åvestadal",
      "group": "avesta",
      "url": "buildings/avesta/avestadal_clean/tileset/tileset.json",
      "options": { "maximumScreenSpaceError": 16 }
    },
    {
      "name": "Bergsnäs",
      "group": "avesta",
      "url": "buildings/avesta/bergsnas_clean/tileset/tileset.json",
      "options": { "maximumScreenSpaceError": 16 }
    },
    {
      "name": "Centrala Krylbo",
      "group": "avesta",
      "url": "buildings/avesta/centralaKrylbo_clean/tileset/tileset.json",
      "options": { "maximumScreenSpaceError": 16 }
    }
  ]
}
```

