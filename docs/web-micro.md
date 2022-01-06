---
sidebar_label: 'Web-Micro'
sidebar_position: 4
---

# Web-Micro Guiding Questions

This is my **first Docusaurus document**!

---
<!-- id: my-doc-id
title: My document title
description: My document description
slug: /my-custom-url
--- -->

## 1. Orchestrator

The orchestrator is a singleton ```(window.orchestrator = createOrchestrator())``` that lives in ```core.js```, and is the first thing to load on the page. 

For Web app : ```window.orchestrator = new WebOrchestrator()```

For Mobile app: ```window.orchestrator = new MobileOrchestrator()``

```jsx title="projects/core/src/Orchestrator.tsx"
function createOrchestrator() {
	return window.STATIC_MANIFEST ? new MobileOrchestrator() : new WebOrchestrator();
}
```


It has no dependencies. It is responsible for fetching the relevant application manifest.






, then loading all of the individual services listed therein and initiating the application render.




![Orchestrator](/img/orchestrator-1.png)

```jsx 
function HelloDocusaurus() {
    return (
        <h1>Hello, Docusaurus!</h1>
    )
}
```

## 2. What's the first method being called by Orchestrator when the app is loaded?

When the app is loaded => START is the first method being called by Orchestrator 


![Body-onload](/img/body-onload.png)

![orchestrator-start](/img/orchestrator-start.png)

## 4. What's difference between Service Manifest & Application Manifest?

Service manifest
Each service has a service manifest ```projects/[service-name/index.tsx```, which is a JS object which describes all of the stuff in the service and how it is organized.

Application manifest
The application manifest is a JSON file  created during build (in ```/target/.output/dist/_manifest```).

It listing the services that should be downloaded to run the application, the features that should be enabled from those services, and any configuration for the service (e.g. inactivity timeout duration), or more global application-level config (e.g. API url, etc.). Each region has its own application manifest (e.g. ```application.us-nj.json,``` ```application.us-in.json```, etc.).

## 7. When visiting non-existing routes (eg. pointsbet.com.au/non-existing), how does the app implement redirect?


![Error404] (/img/error404.png)


## 8. How does component injection work? Behind the scene, how are injected components imported?

A component injection is a declarative instruction in the service manifest to drop a component into a named Host. 

The host component is injected into in the service with a name pertaining to the placement of the Host. 
Then, the other service can inject any component to render into the Host by property "target" which value matching with the name of the Host`s placement.

(Component injections with same target are injected and rendered into the Host with the same placement`s name and ordered by property PRIORITY)

Component injections without property "composed" are inline component injections.

Component injections with property "composed" are composed component injections.

All composed injections always wrap all inline injections. The composition (nesting) of the composed component injections is determined by the "priority".

e.g.

Login modal component including Host component with <i>name ="auth/login-modal-body" </i>
![login-modal-body](/img/login-modal-body.png)

Registration service injected component with target "auth/login-modal-body" into the Host with the same placement above
![component-euGdpr-service](/img/component-euGdpr-service.png)

---

Behind the since  injected components imported via the hook ```useComponentHostData``` and function ```build``` in ```(/projects/common/src/component/ComponentHost/Common.tsx)``` .

Within the Orchestrator, ```FeatureManager``` and ```ComponentManager``` reshapes and caches application manifest's service and feature definitions to component injection mappings, and exposes them via ```getComponents``` method. 

![getComponents](/img/getComponents.png)

The function ```build``` manage the order(inline, wrapping) and render all injection components with same target name into the component Host.

```Orchestrator.getComponents(name)``` manage an index of components by injection target name from a collection of service manifests 


## 11. How is the index.html file generated?







1. How the Ochestrator works?
2. What's the first method being called by Orchestrator when the app is loaded?
3. Where is global routing table located & how it works?
4. What's difference between Service Manifest & Application Manifest?
5. How are services loaded and enabled?
6. How does Route wrap other components? Where to find routing table?
7. When visiting non-existing routes (eg. pointsbet.com.au/non-existing), how does the app implement redirect?
8. How does component injection work? Behind the scene, how are injected components imported?
9. How are service manifest combined?
10. Are service bundle js files created during build or run-time?
11. How is the index.html file generated?
12. How are feature toggle JSON config generated, how does it enable/disable features?
- How turn on (off) toggles in Storage Explorer
- How does it enable/disable features?
- Inline feature toggling
13. How Storybook works? How is it built?