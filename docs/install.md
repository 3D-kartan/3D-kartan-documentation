# Install

## Frontend

1. Clone the repo
2. CD webpack-5
3. `npm install`
4. Pre build configs:

    - If you wish to use the `placement tool` you need to configure the models to be used pre build. The `placement tool` requires frontend.

    - For the `searchbar` you may set the appropriate `apiBaseUrl` in `webpack.config.js`. The searchbar and the backend expects the following columns:

        - td_adress
        - td_kommund
        - Geometry (points in WGS 84)

        - schema.table setup is expected to be addresses.addresses_p

        The above parameter may be changed to fit your set-up. Future updates will hopefully move these configs to the config files for easier set-up. The `searchbar` requires frontend and backend.

    - The `forms tool` require the backend to be set up

5. Run: `npm run build` and now the application is built and should be ready to be hosted
6. Configure the `index.json` file
7. Host the app

---

## Backend
Backend installation guide can be found [here](3D-kartan-backend.md)