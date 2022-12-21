# Nx basics

### What is a workspace?

A workspace is a folder created using Nx. The folder consists of a single git repository, with folders for `apps` (applications) and `libs` (libraries); along with some scaffolding to help with building, linting, and testing.

### What is an app?

An app produces a binary. It contains the minimal amount of code required to package many libs to create an artifact that is deployed.

### What is a lib?&#x20;

A lib is a set of files packaged together that is consumed by apps. Libs are similar to node modules or nuget packages. They can be published to NPM or bundled with a deployed application as-is.

Nx is configured for a workspace with these configuration files:&#x20;

• nx.json which is specific to Nx&#x20;

• package.json which is provided by npm&#x20;

• angular.json which is provided by the Angular CLI

### Types of libraries&#x20;

There are many different types of libraries in a workspace. In order to maintain a certain sense of order, we recommend having only the below four (4) types of libraries:&#x20;

• Logic libraries: Developers should consider feature libraries as libraries that implement smart UI (with injected services) for Business Use Cases specific to that feature.&#x20;

• UI libraries: A UI library contains only presentational components.&#x20;

• Data-access libraries: A data-access library contains services and utilities for interacting with a back-end system.

• Utility libraries: A utility library contains common utilities and services used by

#### Logic libraries&#x20;

Logic libraries contain router configurations for a particular application section. Most of the UI components in such a library are smart components that interact with the NgRx Store. This type of library also contains most of the application logic, validation etc. Feature libraries are almost always app-specific and are often lazy-loaded.

UI libraries&#x20;

A UI component library contains only presentational components.&#x20;

• An app-specific component library contains the components specific to a particular application (or a few very closely connected applications).&#x20;

• A shared component library contains components used in multiple applications or across many business functions.&#x20;

Naming Convention: ui or ui-\* (e.g., components-buttons).



Smart Components&#x20;

◦ manage or delegate business logic and use DI to inject services.&#x20;

◦ have child instances of presentational components.&#x20;

Presentational Components (aka dumb)&#x20;

◦ no or very little business logic&#x20;

◦ only rely on Inputs and Outputs to communicate with the outside world.&#x20;

◦ have Smart component parents ◦ announce user interactions using outputs&#x20;

◦ are highly reusable and are the easiest to test ◦ render data in a presentational format (e.g., display a user ticket), and&#x20;

◦ may be fully generic/domain-agnostic (e.g,. data-table) or have a domain-context (e.g, log-book-table).

### Data-access libraries&#x20;

Data-access libraries contain REST or webSocket services that function as client-side delegate layers to server tier APIs. All files related to State management also reside in a data-access folder.

### Utility libraries&#x20;

A utility contains common utilities/services used by many libraries.
