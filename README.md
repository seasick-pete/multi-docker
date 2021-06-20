
# Updates

In the next lecture, we will be adding a client and worker service. There are two required configuration updates that need to be made:

1) When adding the "Client" service, be sure to include the stdin_open: true configuration:

      client:
        stdin_open: true
        build:
          dockerfile: Dockerfile.dev
          context: ./client
        volumes:
          - /app/node_modules
          - ./client:/app

Otherwise, you will get a "React App exited with Code 0" error in your terminal when we attempt to start up the application.

2) When adding the "Worker" service, include the Redis host and port as environment variables:

      worker:
        build:
          dockerfile: Dockerfile.dev
          context: ./worker
        volumes:
          - /app/node_modules
          - ./worker:/app
        environment:
          - REDIS_HOST=redis
          - REDIS_PORT=6379

Otherwise, you will get a "Calculated Nothing Yet" error message.


```
FROM node:14.14.0-alpine
```



Nginx fix for React Router

In the next lecture, we will be adding some Nginx config to the client-side project, but I neglected to add one line that would get the Nginx server to work correctly when using React Router!

In the client/nginx/default.conf file, please add the following line to the end of the location directive:

    try_files $uri $uri/ /index.html;

The default.conf should now look like this:

    server {
      listen 3000;
     
      location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
      }
    }
    


```
Fix for Failing Travis Builds

In the upcoming lecture, we will be adding a script to our .travis.yml file. Due to a change in how the Jest library works with Create React App, we need to make a small modification:

    script:
      - docker run USERNAME/react-test npm test -- --coverage

instead should be:

    script:
      - docker run -e CI=true USERNAME/react-test npm test
      