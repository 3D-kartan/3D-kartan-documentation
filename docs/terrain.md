## Terrain

Terrain in 3D-Kartan has to be either Cesium World, an ion asset terrain or locally created and hosted terrain.

Ellipsoid height is also an option but is not recomended since some functionallity depens on a terrain provider that has tile availability.

### Create local terrain

Terrain can be created from a Docker image, example: tumgis/ctb-quantized-mesh.

Which can be found here: [https://hub.docker.com/r/tumgis/ctb-quantized-mesh](https://hub.docker.com/r/tumgis/ctb-quantized-mesh)


### Set up local terrain for web hosting on IIS

Create a virtual folder in IIS where you store the terrain. You may have to set the correct mime-types. 


### Tile terrain via Cesium ion

You can upload a DEM .tif file to [ion](https://cesium.com/platform/cesium-ion/) and let ion tile it. 

To note here is:

- The data goes to the cloud
- You need to specify the right settings 
- The DEM should be in WGS 84
