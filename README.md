A project that runs a Node server and a create-react-app app via two separate containers, using Docker Compose.

## Development

```
docker-compose up
```

For development, the `server/` and `client/` directories have their own docker containers, which are configured via the `docker-compose.yml` file.

The client server is spun up at `localhost:3500` and it proxies internally to the server using the linked name as `server:3600`.

The local directories are mounted into the containers, so changes will reflect immediately. However, changes to package.json will likely need to a rebuild: `docker-compose down && docker-compose build && docker-compose up`.

### Notes

#### Adding new scss files

In a previous version of this, you needed to restart the client for new scss files to be recognized by the watch command. This may have changed (TODO: test if this still matters with react-scripts updates):

```
docker-compose restart client
```

#### Installing npm dependencies

All changes to `node_modules` should happen _inside_ the containers. Install any new dependencies by inside the container. You can do this via `docker-compose run`, but it’s easier to just upadte a running container and avoid having to rebuild everything:

```
docker-compose exec client
```

Then inside:

```
npm install --save <new_dependency>
```

## Production

```
docker-compose -f docker-compose.prod.yml up
```
For production, this uses the Dockerfile at the root of the repo. It creates a static build of the client React app and runs Express inside server, which handles both the API and serving of React files.
