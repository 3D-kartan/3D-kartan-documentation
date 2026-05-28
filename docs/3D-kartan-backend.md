# 3D-kartan-backend set up guide 

The backend only need to be set up if you wish to use the searchbar that searches adresses in a Postgis db and/or use of the `forms tool`.

## Set up the backend

1. Clone the repo
2. CD app
3. `npm install`
4. Configure proper PostgreSQL + PostGIS setup (schema, tables and user)
5. Set the proper `.env` variables (you also have to run `hash-admin-password.js` to get your hashed password)
6. If you wish to deply the service with auto start in an server environment. 
Set up a task, running the `.bat` script triggering `node server.js`
7. If you wish to skip auto start, then just run `node server.js` and the service will be up an running

The backend runs on `localhost:4001` and can be accesible by the frontend with proper cors in the `.env`file and URL rewrites in IIS.

---

## Admin interface for forms management

The backend comes with three html sites one for forms creating, one for viewing forms submissions and one that handles the login. The admin interface is protected by a cookie-based basic encryption.

They are locally reached from:

- `localhost:4001/admin/forms.html`
- `localhost:4001/admin/submissions.html`
- `localhost:4001/admin/login.html`

or public (via URL Rewrite)

- `https://your-URL/admin/forms.html`
- `https://your-URL/admin/forms.html`
- `https://your-URL/admin/forms.html`

---

## Recommended requirements: 

- PostgreSQL + PostGIS (DB to store forms and sumbission and search addresses).
- Deployment on a Microsoft server with IIS.
- Good knowledge of DB, Microsoft server + IIS and the basics of coding.
- To have a PostgreSQL + PostGIS base set up, then you can take a look at the psql script - as a guide for how the backend is set up. You can create your own schema, tables and roles but then you have to change the code to match your set up.



---
