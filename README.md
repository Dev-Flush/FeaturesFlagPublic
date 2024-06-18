# Documentation

> The communication with the server is fully encrypted using the keys that can be automatically generated or entered manually when registering an app with the application.

How to install and use the feature flags app from dev flush

### Docker Compose File

```yaml
services:
  feature-flags-frontend:
    container_name: feature-flags-frontend
    image: shiv4nk4r/devflush-features:latest
    ports:
      - "4444:8080"
    environment:
      - VITE_BACKBACKEND_URL=http://localhost:3001
  feature-flags-backend:
    container_name: feature-flags-backend
    image: shiv4nk4r/devflush-features-backend:latest
    ports:
      - "3001:3000"
    environment:
      - MONGO_URL=mongodb://mongodb:27017/features-flag
      - ADMIN_USER=shivankar
      - ADMIN_PASS=securepass
  mongodb:
    image: mongo:6-jammy
    ports:
      - "27017:27017"
    volumes:
      - dbdata:/data/db
volumes:
  dbdata:
```

### Environment values for [shiv4nk4r/devflush-features](https://hub.docker.com/repository/docker/shiv4nk4r/devflush-features)

| key              | value                                         | example               |
| ---------------- | --------------------------------------------- | --------------------- |
| VITE_BACKEND_URL | url for `shiv4nk4r/devflush-features-backend` | http://localhost:3001 |

### Environment values for [shiv4nk4r/devflush-features-backend](https://hub.docker.com/repository/docker/shiv4nk4r/devflush-features-backend)

| key        | value                           | example                               |
| ---------- | ------------------------------- | ------------------------------------- |
| MONGO_URL  | url for `mongo`                 | mongodb://mongodb:27017/features-flag |
| ADMIN_USER | username to login in the webapp | admin                                 |
| ADMIN_PASS | password to login in the webapp | password                              |

Copy this `docker-compose.yaml` file in your project directory and use `docker compose up -d` command to run the application stack.

Now you can open the UI on the browser and enter your Admin credentails you configured in the docker compose file.

From here proceed to configure your apps and flags.

Now this was the first half of the installation now you are required to actually connect your app with the service. To do this simply follow the steps mentioned below.

### NPM Package

First steps install the npm package from the link below in your app

Npm link for the package [FeatureFlags](https://www.npmjs.com/package/@shiv4nk4r/feature-flags).

```js
// Import the package
const DevFlushFeatureFlags = require("@shiv4nk4r/feature-flags");
// Initialize the package
const flags = new DevFlushFeatureFlags(
  "http://localhost:3001", // Url for the backend server
  "manager", //  app name registered on DevFlush feature flag's app
  "23d4d846-fd9f-49c6-bbb1-4d0b6e6b8c9b" // key registered on the app
);
```

Now there are 3 functions which are exposed from the package you can call them as.

1. `getFlag()` - this function is used to fetch the flag that you pass default value is null for all flags, returns a promise which resolves the value

```ts
flags.getFlag("addUser").then((value: boolean) => {});
```

2. `getAllFlags` - this function gives you all the flags present for the app

```ts
flags.getAllFlagsA("addUser").then((values:{name:string, value:boolean}[]) => {});
```

3. `getApp` - returns the app data which is registered

```ts
flags.getApp("addUser").then((value: {app: object, flags: object[]}) => {});
```

Now your are set to use the app.
