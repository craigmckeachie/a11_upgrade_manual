# Upgrading

This tutorial walks you through upgrading the `Phonecat` app written in AngularJS to Angular using the Angular CLI.

- [Upgrading](#upgrading)
  - [Lab 0: The AngularJS Project](#lab-0-the-angularjs-project)
  - [Lab 1: Generate New Angular Project](#lab-1-generate-new-angular-project)
  - [Lab 2: Create Angular directory and configure](#lab-2-create-angular-directory-and-configure)
    - [`tsconfig.app.json`](#tsconfigappjson)
    - [`tsconfig.spec.json`](#tsconfigspecjson)
    - [`angular.json`](#angularjson)
  - [Lab 3: Create AngularJS directory and configure](#lab-3-create-angularjs-directory-and-configure)
  - [Lab 4: Bootstrap AngularJS & Angular Hybrid Application](#lab-4-bootstrap-angularjs--angular-hybrid-application)
  - [Lab 5: Downgrading An Angular Component](#lab-5-downgrading-an-angular-component)
  - [Lab 6: Upgrading an AngularJS Service](#lab-6-upgrading-an-angularjs-service)
  - [Lab 7: Routing to Angular Components (using the AngularJS Router)](#lab-7-routing-to-angular-components-using-the-angularjs-router)
  - [Lab 8: AngularJS Views inside the Root Angular Component](#lab-8-angularjs-views-inside-the-root-angular-component)
  - [Lab 9: Sibling Routers](#lab-9-sibling-routers)

## Lab 0: The AngularJS Project

1. Create an `working` directory on your computer for these labs.

2. Open a command prompt or terminal in that directory
   ```shell
   cd working
   ```
3. Clone the AngularJS Project we will be upgrading into the working directory.

   ```shell
   git clone https://github.com/craigmckeachie/angular-phonecat.git
   ```

4. Change directory to:

   ```shell
   cd angular-phonecat/app
   ```

5. Install the project dependencies:

   ```shell
   npm install
   ```

6. Install a web server:

   ```bash
   npm install serve -g
   ```

   > Note: If you already installed the `serve` web server in an earlier lab you can skip this step.

7. Run the web server (make sure you are still in the app directory):

   ```bash
   serve .
   ```

8. Click on the link to open `http://localhost:5000` and see the AngularJS Phonecat application.

   > This is the AngularJS application we will be running in Hybrid mode and slowly upgrading

   > It is the application built in the official AngularJS tutorial

9. Shut down the web server Ctrl+C.

## Lab 1: Generate New Angular Project

1. Open a command-prompt or terminal in the `working` directory.
1. Run the command
   ```shell
   ng new upgrading
   ```
1. Choose `n` no for stricter type checking and bundle budgets.
1. Choose `n` no for Angular routing.
1. Choose `CSS` for stylesheet format by hitting enter.

1. In the command-promt or terminal, change directory to the `working\upgrading` directory.
   ```
   cd upgrading
   ```
1. Serve the Angular CLI project to verify it is working

   ```bash
   ng serve -o
   ```

   A browser should display the `angular-cli` generated application.

1. Shut down the web server Ctrl+C.

## Lab 2: Create Angular directory and configure

1. Open the directory `working\upgrading` in your editor

1. In `src` folder create two folders `src\angular` and `src\angularjs`.

   > Verify the angularjs directory is not inside the angular directory.

1. Move everything (all directories and files) inside `src` except the `angular` and `angularjs` folders created in the prior step into the `angular` folder.

1. Copy two files from `src\angular\app` back out into `src`.
   - favicon.ico
   - index.html
1. Modify the `tsconfig` files to recognize the new directory structure.

   #### `tsconfig.app.json`

   ```diff
   {
     "extends": "./tsconfig.json",
     "compilerOptions": {
       "outDir": "./out-tsc/app",
       "types": []
     },
     "files": [
   -   "src/main.ts",
   +   "src/angular/main.ts",
   -   "src/polyfills.ts"
   +   "src/angular/polyfills.ts"
     ],
     "files": [
       "src/angular/main.ts",
       "src/angular/polyfills.ts"
     ],
     "include": [
       "src/**/*.d.ts"
     ]
   }
   ```

   #### `tsconfig.spec.json`

   ```diff
   {
     "extends": "./tsconfig.json",
     "compilerOptions": {
       "outDir": "./out-tsc/spec",
       "types": [
         "jasmine"
       ]
     },
     "files": [
   -    "src/test.ts",
   +    "src/angular/test.ts",
   -    "src/polyfills.ts"
   +    "src/angular/polyfills.ts"
     ],
     "include": [
       "src/**/*.spec.ts",
       "src/**/*.d.ts"
     ]
   }

   ```

1. Modify `angular.json` to recognize the new directory structure.

   In most cases, you would need to replace `src/` with `src/angular/`. Then go back and set the paths for index.html, favicon.ico to remove the /angular part of the path since these items are still directly inside of the root `src` directory.

   If time is tight just replace the contents of the `angular.json` file with the following.

   #### `angular.json`

   ```json
   {
     "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
     "version": 1,
     "newProjectRoot": "projects",
     "projects": {
       "upgrading": {
         "projectType": "application",
         "schematics": {},
         "root": "",
         "sourceRoot": "src",
         "prefix": "app",
         "architect": {
           "build": {
             "builder": "@angular-devkit/build-angular:browser",
             "options": {
               "outputPath": "dist/upgrading",
               "index": "src/index.html",
               "main": "src/angular/main.ts",
               "polyfills": "src/angular/polyfills.ts",
               "tsConfig": "tsconfig.app.json",
               "aot": true,
               "assets": ["src/favicon.ico", "src/angular/assets"],
               "styles": ["src/angular/styles.css"],
               "scripts": []
             },
             "configurations": {
               "production": {
                 "fileReplacements": [
                   {
                     "replace": "src/angular/environments/environment.ts",
                     "with": "src/angular/environments/environment.prod.ts"
                   }
                 ],
                 "optimization": true,
                 "outputHashing": "all",
                 "sourceMap": false,
                 "namedChunks": false,
                 "extractLicenses": true,
                 "vendorChunk": false,
                 "buildOptimizer": true,
                 "budgets": [
                   {
                     "type": "initial",
                     "maximumWarning": "2mb",
                     "maximumError": "5mb"
                   },
                   {
                     "type": "anyComponentStyle",
                     "maximumWarning": "6kb",
                     "maximumError": "10kb"
                   }
                 ]
               }
             }
           },
           "serve": {
             "builder": "@angular-devkit/build-angular:dev-server",
             "options": {
               "browserTarget": "upgrading:build"
             },
             "configurations": {
               "production": {
                 "browserTarget": "upgrading:build:production"
               }
             }
           },
           "extract-i18n": {
             "builder": "@angular-devkit/build-angular:extract-i18n",
             "options": {
               "browserTarget": "upgrading:build"
             }
           },
           "test": {
             "builder": "@angular-devkit/build-angular:karma",
             "options": {
               "main": "src/angular/test.ts",
               "polyfills": "src/angular/polyfills.ts",
               "tsConfig": "tsconfig.spec.json",
               "karmaConfig": "karma.conf.js",
               "assets": ["src/angular/favicon.ico", "src/angular/assets"],
               "styles": ["src/angular/styles.css"],
               "scripts": []
             }
           },
           "lint": {
             "builder": "@angular-devkit/build-angular:tslint",
             "options": {
               "tsConfig": [
                 "tsconfig.app.json",
                 "tsconfig.spec.json",
                 "e2e/tsconfig.json"
               ],
               "exclude": ["**/node_modules/**"]
             }
           },
           "e2e": {
             "builder": "@angular-devkit/build-angular:protractor",
             "options": {
               "protractorConfig": "e2e/protractor.conf.js",
               "devServerTarget": "upgrading:serve"
             },
             "configurations": {
               "production": {
                 "devServerTarget": "upgrading:serve:production"
               }
             }
           }
         }
       }
     },
     "defaultProject": "upgrading"
   }
   ```

1. In the command-promt or terminal, change directory to the `working\upgrading` directory.
   ```
   cd upgrading
   ```
1. Serve the Angular CLI project to verify it is still working

   ```bash
   ng serve -o
   ```

   A browser should display the `angular-cli` generated application as it did in the previous lab.

1. Shut down the web server `Ctrl+C`.

1. Optional: Commit your code to source control.

## Lab 3: Create AngularJS directory and configure

> Note that there is already a `bower_components` folder in `angular-phonecat\app` repository we cloned. If you were to download your own copy of angular-phonecat you would need to first run `npm install` to create this folder.

1.  Create the directory `upgrading\src\angularjs`
2.  Copy **the contents** of `angular-phonecat\app` (including the bower_components subdirectory) into `upgrading\src\angularjs`.
3.  Merge `upgrading\src\angularjs\index.html` into `upgrading\src\index.html` as shown below:

    ```diff
    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Upgrading</title>
      <base href="/">

      <meta name="viewport" content="width=device-width, initial-scale=1">
      <link rel="icon" type="image/x-icon" href="favicon.ico">

    +  <link rel="stylesheet" href="angularjs/bower_components/bootstrap/dist/css/bootstrap.css" />
    +  <link rel="stylesheet" href="angularjs/app.css" />
    +  <link rel="stylesheet" href="angularjs/app.animations.css" />
    +
    +  <script src="angularjs/bower_components/jquery/dist/jquery.js"></script>
    +  <script src="angularjs/bower_components/angular/angular.js"></script>
    +  <script src="angularjs/bower_components/angular-animate/angular-animate.js"></script>
    +  <script src="angularjs/bower_components/angular-resource/angular-resource.js"></script>
    +  <script src="angularjs/bower_components/angular-route/angular-route.js"></script>
    +  <script src="angularjs/app.module.js"></script>
    +  <script src="angularjs/app.config.js"></script>
    +  <script src="angularjs/app.animations.js"></script>
    +  <script src="angularjs/core/core.module.js"></script>
    +  <script src="angularjs/core/checkmark/checkmark.filter.js"></script>
    +  <script src="angularjs/core/phone/phone.module.js"></script>
    +  <script src="angularjs/core/phone/phone.service.js"></script>
    +  <script src="angularjs/phone-list/phone-list.module.js"></script>
    +  <script src="angularjs/phone-list/phone-list.component.js"></script>
    +  <script src="angularjs/phone-detail/phone-detail.module.js"></script>
    +  <script src="angularjs/phone-detail/phone-detail.component.js"></script>

    </head>
    -<body>
    +<body ng-app="phonecatApp">

    +  <div class="view-container">
    +    <div ng-view class="view-frame"></div>
    +  </div>

      <app-root></app-root>
    </body>
    </html>
    ```

4.  When finished with the merge delete `upgrading\src\angularjs\index.html`.

5.  Add `src\angularjs` to the list of `assets`

    #### `angular.json`

    ```diff
    {
      "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
      "version": 1,
      "newProjectRoot": "projects",
      "projects": {
        "upgrading": {
          "projectType": "application",
          "schematics": {},
          "root": "",
          "sourceRoot": "src",
          "prefix": "app",
          "architect": {
            "build": {
              "builder": "@angular-devkit/build-angular:browser",
              "options": {
                "outputPath": "dist/upgrading",
                "index": "src/index.html",
                "main": "src/angular/main.ts",
                "polyfills": "src/angular/polyfills.ts",
                "tsConfig": "tsconfig.app.json",
                "aot": true,
                "assets": [
                  "src/favicon.ico",
                  "src/angular/assets",
    +              "src/angularjs"
                ],
                "styles": ["src/angular/styles.css"],
                "scripts": []
              },

     ...
    ```

6.  Fix paths throughout the application by appending `angularjs/` to the paths in these files in the `src\angularjs` directory.

    - `core\phone\phone.service.js`
      - change ~~`"phones/:phoneId.json",`~~ to `"angularjs/phones/:phoneId.json",`
    - `phone-list\phone-list.component.js`
      - change ~~`templateUrl: "phone-list/phone-list.template.html",`~~ to `templateUrl: "angularjs/phone-list/phone-list.template.html",`
    - `phone-list\phone-list.template.html`
      - change ~~`<img ng-src="{{phone.imageUrl}}" alt="{{phone.name}}" />`~~ to `<img ng-src="angularjs/{{phone.imageUrl}}" alt="{{phone.name}}" />`
    - `phone-detail\phone-detail.component.js`
      - change ~~`templateUrl: "phone-detail/phone-detail.template.html",`~~ to `templateUrl: "angularjs/phone-detail/phone-detail.template.html",`
    - `phone-detail.template.html`

      - change ~~`{{img}}`~~ to `angularjs/{{img}}`
        _Note there are two instances of `{{img}}` in the template on line 2 and 12._

7.  Stop the application if it is running `Ctrl+C`, then run the application again using the command below.

    ```shell
    ng serve -o
    ```

    You should see both the Angular and AngularJS application rendered when the page loads as shown below.

    - _Note: You will need to scroll down the page to see the Angular application_

    ![Figure 1][figure-1]

    [figure-1]: readme-assets/together.png

    _As those two applications are bootstrapped separately, they are not able to communicate with each other or to exchange services and components. To make this work, we have to bootstrap them together as a hybrid application. The next section show how to do this._

8.  Optional: Commit your code to source control.

## Lab 4: Bootstrap AngularJS & Angular Hybrid Application

    1.  To bootstrap one application with both AngularJS and Angular we can leverage `ngUpgrade` which is part of Angular:

    Open another command-prompt or terminal to the `upgrading` directory and run this command.

    ```shell
    npm install @angular/upgrade@11 --save
    ```

2.  In `index.html`

    We don't want to bootstrap the Angular application on its own at this point, so we will comment out it's root component in `src\index.html`.

    ```html
    <!-- <app-root></app-root> -->
    ```

3.  In `angular\app\app.module.ts` comment out the bootstrap of the `AppComponent`

    ```ts
    // bootstrap: [AppComponent]
    ```

4.  To prevent bootstrapping the AngularJS application remove the `ng-app` directive.

    - Remove: <body ~~ng-app="phonecatApp"~~>

5.  Add the `UpgradeModule` to the list of Angular module imports.

    #### `angular\app\app.module.ts`

    ```diff
    + import { UpgradeModule } from "@angular/upgrade/static";

    @NgModule({
      declarations: [AppComponent],
      imports: [BrowserModule,
    + UpgradeModule],
      providers: []
      // bootstrap: [AppComponent]
    })
    ```

    ...and bootstrap the hybrid application in the `AppModule` class.

    ```ts
    ...
    export class AppModule {

      constructor(private upgrade: UpgradeModule) {}

      ngDoBootstrap() {
        this.upgrade.bootstrap(document.body, ["phonecatApp"], { strictDi: true });
      }
    }
    ```

6.  The Angular application may need to be restarted to recognize the `UpgradeModule` and bootstrap itself appropriately.
    Use Ctrl+C (Windows) or Cmd+C (Mac) to stop the web server.

7.  Run the application.

    ```shell
    ng serve -o
    ```

    You should see just AngularJS application rendered when the page loads as shown below.

    ![Figure 2][figure-2]

    [figure-2]: readme-assets/justangularjs.png

    > Note: Even though we only see the AngularJS application it is a hybrid application. To prove this, the next section shows how to use an Angular component within the shown AngularJS component.

8.  Optional: Commit your code to source control.

## Lab 5: Downgrading An Angular Component

1.  Create the widget directory: `src\angular\app\widget`
2.  Create the file `src\angular\app\widget\widget.component.ts`.
3.  Add this code to create an `Angular` component

    #### `src\angular\app\widget\widget.component.ts`

    ```ts
    import { Component } from "@angular/core";
    @Component({
      selector: "app-widget",
      template: ` <h3>Angular Widget</h3> `
    })
    export class WidgetComponent {}
    ```

4.  Downgrade the _Angular_ `WidgetComponent` to an _AngularJS_ component

    #### `src\app\angular\app.module.ts`

    ```ts
    import { UpgradeModule, downgradeComponent } from "@angular/upgrade/static";
    import { WidgetComponent } from "./widget/widget.component";

    declare var angular: any;
    angular
      .module("phonecatApp")
      .directive(
        "appWidget",
        downgradeComponent({ component: WidgetComponent })
      );
    ```

    _Be sure to use camel-case the `appWidget` when registering the directive name above. This translates to the \_dasherized_ syntax: `<app-widget></app-widget` in the template.\_

    ...and add the `WidgetComponent` to the `AppModule` declarations and as an entry component.

    ```diff
    @NgModule({
      declarations: [AppComponent,
    + WidgetComponent],
      imports: [BrowserModule, UpgradeModule],
    + entryComponents: [WidgetComponent],
      providers: []
      // bootstrap: [AppComponent]
    })
    export class AppModule {
      ...
    ```

    _As you see in this sample, the downgraded component is registered as a `directive` within the AngularJS module. For this, we can leverage the global angular variable. In order to tell TypeScript about this preexisting variable, we have to use the `declare` keyword._

5.  After this, we can call the Angular Component within an AngularJS template. Place the widget below the select list for sorting phones.

    #### `src/angularjs/phone-list/phone-list.template.html`

    ```html
    ...
    </select>

    <p>
            <!-- Angular Component -->
            <app-widget></app-widget>
    </p>
    ```

6.  Stop and restart `ng serve -o`.
7.  Open the application in the browser and you should see the _Angular_ widget hosted inside the _AngularJS_ component as shown below. Note that the orange box will not actually show on the page it is just there in the screenshot to show where the widget renders.

    > Note the orange box is just there to highlight what you are looking for and will not appear in your application.

    ![Angular Widget][widget]

    [widget]: readme-assets/widget.png

8.  Optional: Commit your code to source control.

## Lab 6: Upgrading an AngularJS Service

In order to use an existing AngularJS service within a new Angular Component, we have to upgrade it.

1.  We have to create an Angular service provider which uses a factory. This factory gets a reference to the AngularJS injector (\$injector) and uses it to obtain the service in question. We do this in a new file `phone.service.ts`. Create the file at the path shown below and add the code to the new file. You will need to create the `phones` and `shared` directories.

    #### `src\angular\app\phones\shared\phone.service.ts`

    ```ts
    import { InjectionToken } from "@angular/core";

    export const PHONE_SERVICE = new InjectionToken<any>("PHONE_SERVICE");

    function createPhoneService(injector) {
      return injector.get("Phone");
    }

    export const phoneServiceProvider = {
      provide: PHONE_SERVICE,
      useFactory: createPhoneService,
      deps: ["$injector"]
    };
    ```

    _Normally, we could use the service's type as the dependency injection token defined by the provide property. But in this case we explicitly decided to not upgrade the existing AngularJS 1.x code to TypeScript and so we don't have any type for it. Because of this, this sample uses a constant based Token called `PHONE_SERVICE`. For such tokens Angular 4+ provides the type `InjectionToken`. In Angular 2 we would use `OpaqueToken` instead. The `InjectionToken` takes a type parameter which identifies the type of the service it is pointing to. As mentioned, we don't have a type for this service and so we are just going with `any`. Note: the `injector` is getting the phone from `src\angularjs\core\phone\phone.service.js`._

2.  Register the service provider with the Angular module.

    #### `src\angular\app\app.module.ts`

    ```ts
    [...]
    import { phoneServiceProvider } from './phones/shared/phone.service';

    [...]

    @NgModule({
      [...],
      providers: [
        phoneServiceProvider
      ]
    })
    export class AppModule {
      [...]
    }
    ```

3.  Inject the upgraded `phoneService` into the Angular `WidgetComponent` and show a count of the phones.

    #### `src\angular\app\widget\widget.component.ts`

    ```ts
    import { Component, Inject, OnInit } from "@angular/core";
    import { PHONE_SERVICE } from "../phones/shared/phone.service";
    @Component({
      selector: "app-widget",
      template: `
        <h3>Angular Widget</h3>
        <p>{{ phones.length }} phones found.</p>
      `
    })
    export class WidgetComponent implements OnInit {
      phones: any[] = [];

      constructor(@Inject(PHONE_SERVICE) private phoneService: any) {}

      ngOnInit(): void {
        this.phones = this.phoneService.query();
      }
    }
    ```

4.  Run the application and you should see a count of phones in the Angular Widget as shown below.

    ![Upgrade Service][upgrade-service]

    [upgrade-service]: readme-assets/upgrade-service.png

    _We now have an AngularJS component with an Angular Component that is displaying data from an AngularJS service._

    Instead of nesting AngularJS and Angular stuff, we also need the possibility to activate routes from both versions. We will explore this in the next couple sections.

## Lab 7: Routing to Angular Components (using the AngularJS Router)

Making the AngularJS Router activate Angular components is quite easy.

1.  Configure a route with a template that points to the component in question.

    #### `src\angularjs\app.config.js`

    ```diff
    angular.module("phonecatApp").config([
      "$locationProvider",
      "$routeProvider",
      function config($locationProvider, $routeProvider) {
        $locationProvider.hashPrefix("!");

        $routeProvider
          .when("/phones", {
            template: "<phone-list></phone-list>"
          })
          .when("/phones/:phoneId", {
            template: "<phone-detail></phone-detail>"
          })
    +      .when("/widget", {
    +        template: "<app-widget></app-widget>"
    +      })
          .otherwise("/phones");
      }
    ]);
    ```

    > Be sure there is a period `.` before the `otherwise` method call.

2.  Open the application and change the url from: http://localhost:4200/#!/phones to: http://localhost:4200/#!/widget
    and you should see just the widget rendered as shown below.

    ![widget-via-route][widget-via-route]

    [widget-via-route]: readme-assets/widget-via-route.png

    While this solution is very easy, it also comes with a drawback. We cannot leverage the new Angular Router for the newly written components. We will explore how we can use the Angular Router in the next sections.

3.  Optional: Commit your code to source control.

## Lab 8: AngularJS Views inside the Root Angular Component

1.  In `src\index.html` and the Angular root component (`AppComponent`) and remove the `ng-view` directive.

    #### `src\index.html`

    ```diff
    <body>
    -  <div class="view-container">
    -    <div ng-view class="view-frame"></div>
    -  </div>
    +  <app-root></app-root>
    </body>
    ```

2.  Open `src\angular\app\app.component.html`

    - delete everything in the file
    - add the `ng-view` directive as shown below

    ```html
    <div class="view-container"><div ng-view class="view-frame"></div></div>
    ```

3.  Bootstrap `AppComponent` in `AppModule`

    #### `src\angular\app\app.module.ts`

    ```diff
    @NgModule({
      declarations: [AppComponent, WidgetComponent],
      imports: [BrowserModule, UpgradeModule],
      entryComponents: [WidgetComponent],
      providers: [phoneServiceProvider],
    +  bootstrap: [AppComponent]
    })
    export class AppModule {}
    ```

4.  Remove the following code from AppModule

    - inject `upgradeModule`
    - bootstrap application manually

    #### `src\angular\app\app.module.ts`

    ```diff
      export class AppModule {
    -   constructor(private upgrade: UpgradeModule) {}
    -   ngDoBootstrap() {
    -     this.upgrade.bootstrap(document.body, ["phonecatApp"], { strictDi: true -});
    -   }
      }
    ```

5.  Add code to bootstrap the application in the `ngOnInit` lifecycle hook of the `AppComponent` to manually bootstrap the application.

    #### `src\angular\app\app.component.ts`

    ```ts
    import { Component, OnInit } from "@angular/core";
    import { UpgradeModule } from "@angular/upgrade/static";

    @Component({
      selector: "app-root",
      templateUrl: "./app.component.html",
      styleUrls: ["./app.component.css"]
    })
    export class AppComponent implements OnInit {
      constructor(private upgrade: UpgradeModule) {}

      ngOnInit(): void {
        this.upgrade.bootstrap(document.body, ["phonecatApp"]);
      }
    }
    ```

    > Be sure you are implementing the component lifecycle hook `ngOnInit` and NOT `ngDoBoostrap` as we did in the module.

6.  Reload the application and see the Angular application component contains the `ng-view` and is hosting the AngularJS application.

    > Note that there are no changes in the appearance of the application from the prior step we just now have an AngularJS component running inside the root Angular component.

    ![App Root][app-root]

    [app-root]: readme-assets/app-root-parent.png

7.  Optional: Commit your code to source control.

## Lab 9: Sibling Routers

1.  Add the Angular directive `router-outlet`

    #### `src/angular/app/app.component.html`

    ```diff
    <div class="view-container">
        <div ng-view class="view-frame"></div>
    +    <router-outlet></router-outlet>
    </div>
    ```

    > You can ignore the error message 'router-outlet' is not a known element. It will be gone after we complete the next few steps.

2.  Configure the Angular Router.

    #### `src\angular\app\app.module.ts`

    ```diff
    + import { WidgetComponent } from "./widget/widget.component";
    + import { RouterModule } from "@angular/router";

    angular.module('phonecatApp')
      .directive(
        'ng2Demo',angular.module('phonecatApp')
      ],
      imports: [
        BrowserModule,
        UpgradeModule,
    +    RouterModule.forRoot([
    +      {
    +        path: "widget",
    +        component: WidgetComponent
    +      }
    +    ],
    +    {
    +      useHash: true
    +    }
    +    )
      ],
      entryComponents: [
        Ng2DemoComponent // Don't forget this!!!
      ],
      providers: [
        phoneServiceProvider
      ],
      bootstrap: [AppComponent]
    })
    ```

3.  Create a custom url handling strategy class to inform the router which routes should be handled by the AngularJS router and which should be handled by the Angular router.

    #### `src\angular\app\app.module.ts`

    ```ts
    ...
    import { RouterModule, UrlHandlingStrategy } from "@angular/router";

    class CustomUrlHandlingStrategy implements UrlHandlingStrategy {
      shouldProcessUrl(url) {
        return url.toString().startsWith("/widget") || url.toString() === "/";
      }
      extract(url) {
        return url;
      }
      merge(url, whole) {
        return url;
      }
    }
    ```

4.  Provide the `CustomUrlHandlingStrategy`.

    #### `src\angular\app\app.module.ts`

    ```diff
    ...
    entryComponents: [
        Ng2DemoComponent // Don't forget this!!!
      ],
      providers: [
        phoneServiceProvider,
    +    { provide: UrlHandlingStrategy, useClass: CustomUrlHandlingStrategy }
      ],
    ...
    ```

5.  Remove the hashPrefix ! from AngularJS router configuration as well as the route for the widget (since it is now configured in the Angular router) and have the default route redirect to an empty template.

    #### `src\angularjs\app.config.js`

    ```ts
    angular.module("phonecatApp").config([
      "$locationProvider",
      "$routeProvider",
      function config($locationProvider, $routeProvider) {
        //$locationProvider.hashPrefix("!");

        $routeProvider
          .when("/phones", {
            template: "<phone-list></phone-list>"
          })
          .when("/phones/:phoneId", {
            template: "<phone-detail></phone-detail>"
          })
          // .when("/widget", {
          //   template: "<app-widget></app-widget>"
          // })
          //.otherwise("/phones");
          .otherwise({ template: "" });
      }
    ]);
    ```

6.  Add a navigation menu to the `AppComponent` template at the top of the page that allows you to switch between AngularJS and Angular routes. Leave the existing html content including the `router-outlet` in the html template.

    #### `src\angular\app\app.component.html`

    ```diff
    + <a routerLink="widget">Widget</a> |
    + <a href="#/phones">Phones</a>

    <div class="view-container">
      <div ng-view class="view-frame"></div>
      <router-outlet></router-outlet>
    </div>
    ```

7.  Remove the `!` in the `ProjectList` component `href(s)`.

    #### `src\angularjs\phone-list\phone-list.component.template.html`

    ```diff
    -          <a href="#!/phones/{{ phone.id }}" class="thumb">
              <a href="#/phones/{{ phone.id }}" class="thumb">
                <img
                  ng-src="angularjs/{{ phone.imageUrl }}"
                  alt="{{ phone.name }}"
                />
              </a>
    -          <a href="#!/phones/{{ phone.id }}">{{ phone.name }}</a>
              <a href="#/phones/{{ phone.id }}">{{ phone.name }}</a>
    ```

8.  The application should initially show just the navigation and route to either an AngularJS (Phones) or an Angular (Widget) component depending on which you choose.

    ![nav]
    ![nav-widget]
    ![nav-phones]

    [nav]: readme-assets/navigation.png
    [nav-widget]: readme-assets/nav-widget.png
    [nav-phones]: readme-assets/nav-phones.png

9.  Optional: Commit your code to source control.
