# JHipster-Ionic (Work in Progress)

This is an attempt to show how to build an [Ionic](http://ionicframework.com/) app using [JHipster](http://jhipster.github.io/) as backend

The project is structured in 2 modules:
- server: the JHipster backend
- mobile: the Ionic app

The guiding principle is be to generate the mobile app using Yeoman [Generator-M](https://github.com/mwaylabs/generator-m)
to benefit from its great tooling while following JHipster angular code conventions so that
JHipster users feel at home.

If the prototype is successful, JHipster could be extended with a sub generator that would generate CRUD screens for each entity in mobile app or it could be just documented in our [Tips'n tricks section](http://jhipster.github.io/tips.html)

## Environment

- Ubuntu
- java 8
- node 0.12.4
- Android SDK

## Generating modules

### Server

    mkdir server
    cd server
    yo jhipster

I chose the default options except for:

- gradle to be consistent with Android and [Apache Cordova](https://cordova.apache.org/)
- gulp to be consistent with Generator-M and Ionic
- no translation

Test that app works:

    ./gradlew bootRun

Open http://127.0.0.1:8080

### Mobile App

The Ionic app was generated by Yeoman Generator-M

    mkdir mobile
    cd mobile
    yo m

I chose the default options except for:

- appName: "jhipsterApp" to be consistent with JHipster angular module
- appId: "com.mycompany.myapp" to be consistent with JHipster
- no i18n/l10n
- no iOS

Test that app works in browser:

    gulp watch

Open http://127.0.0.1:9000

## Differences in angular projects

### Project structure

- JHipster: organized by feature, a folder can contain a state, a controller and a template. Generic components (filters, directives, services) are stored in 'components' folder.
- Generator-M: organized by module and type, all states are defined in one file per module (e.g. main.js)

### Modules

- JHipster: single module
- Generator-M: Multi modules but at least app and main

### Naming conventions

- JHipster: MainController defined in main.controller.js
- Generator-M: MainCtrl defined in main-ctrl.js

## Generator-M pull requests

I proposed some PR to Generator-M to make it easier for JHipster.

- 220: [Add templates path to gulpfile.js config](https://github.com/mwaylabs/generator-m/pull/220)

# API

## Generating

JHipster uses Swagger to document its API, it's available at http://localhost/v2/api-docs but it requires authentication.

JHiposter's [swagger specification file](https://github.com/wordnik/swagger-spec) could be generated statically by build process and processed to generate some client code like [swagger-js-codegen](https://github.com/wcandillon/swagger-js-codegen). JHipster's API controllers should probably be better annotated.

An alternative is to use dynamic clients: [swagger-js](https://github.com/swagger-api/swagger-js) or [swagger-client](https://github.com/signalfx/swagger-client)

## API versioning

Currently in JHipster, the server and angular codes are coupled so there's no problem of compatibility.

When building a mobile app, this problem can occur because you don't manage when users will update the mobile app.

So, it could mean:

- server and mobile app must have a version number
- server API URL may include verison  (e.g. /api/v1)
- mobile client should send its version number upon each request using an HTTP header. This can be implemented by using [$http interceptors](https://docs.angularjs.org/api/ng/service/$http#interceptors) and pre-defining some standard responses like "client too old" that the mobile app can use to notify the user that she must update the app.

## CORS

- Servlet filter 
- [Spring 4.2 @CrossOrigin](https://spring.io/blog/2015/06/08/cors-support-in-spring-framework)
