# Welcome to 3D-kartan:s documentation

3D-kartan is a 3D-WebMap application, built on Cesium JS. It is fully working and the UI is built to look like the Origo Map Open Source Project. To make users interactions with the 3D-map more convenient.

Key features for the 3D-Map:

- Toolbar with a lot of easy to use tools:
    - Info
    - Find-north
    - Zoom in
    - Zoom out
    - Home
    - Forms (requrie backend to be setup)
    - Sun study
    - Pedestrian mode
    - Hide buildings
    - Draw 3D
    - Measure (lenght, area & height)
    - Placement 
    - Terrain-section
- Project menu to display ongoing or planned constructuin sites in the city, featuring:
    - Text and image description
    - Project related layers that can be toggled
    - Zooming to project site
    - Minimizing project info
    - Pins on the map for the sites, and ability to toggle them
    - Clipping of both layer menu tilesets and terrain
- Layer menu featuring:
    - Easy toggling of both WMS- and 3D-tilesets layers.
    - Zooming to 3D-tileset layers
    - Layer abstracts
    - Ability to change a layers opacity
    - Search control on layers
- Adress search, searching againt db table
- Menu with:
    - Share map
    - Print
    - Coordinates on mouse move toggle
    - Option to toggling visual selection
- Copy coordinates on right click
- Option to display your own logo


## Important notes

- If Cesium Ion is used, you are required to give proper contribution, i.e. the Cesium Ion logo has to be present in the model. Change the visiblity of the `.cesium-viewer-bottom` configured in `src/css/main.css` before building.

- It is recommended to use a non write access token, since the the token is not secure i.e. hidden from the user.



## Requirements
To be able to install the application you need the following installed: `node`, `npm` and `webpack`.

## Available scripts

* `npm start` - Runs a webpack build with `webpack.config.js` and starts a development at `localhost:4000`.
* `npm run build` - Runs a webpack build with `webpack.config.js`.
* `npm run start:built` - Start a small static server using `http-server` to demonstrate hosting the built version.

## Simple project layout

    webpack.config.js   # The configuration file for dev and build.
    buildings/          # Folder for building tilesets.
        kommunNamn/
            Omrade/
                tileset.json  
    projects/           # Project structure.
        project_1/
            img/
                examplebild.png
            ClipMaskProject1.geojson
    public/             
        index.json      # Main config file that inits the application.
    src/
        config/
            images/
                icons/  # Folder for icons used in the application.
                material-icons-svg/             # Material icons folder containing icons
                    black-icons/        
                    white-grey-icons/
                    material-icons-svg.css      # Mapping for css variables
                png/    # Folder for instruction png and logo to be displayed in the application.
            ui/         # Folder for ui code.
                menuFunctions/                  # Folder for menu functions.
            css/        # Folder for css to ui.
            tools/      # Folder that contain subfolders for every tool and their css.
        index.html      # Main html file.
        index.js        # Main js file, intis the application.
    package.json        # Holds dependencies 
    package-lock.json   # Holds dependencies
