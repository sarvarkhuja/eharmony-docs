# Creating Libraries

Like a lot of decisions in programming, deciding to make a new Nx library or not is all about trade-offs. Each organization will decide on their own conventions, but here are some trade-offs to bear in mind as you have the conversation.

<figure><img src="../.gitbook/assets/Screen Shot 2022-12-20 at 11.04.02.png" alt=""><figcaption></figcaption></figure>

screen recording to create a library

{% file src="../.gitbook/assets/Screen Recording 2023-01-18 at 04.19.44.mov" %}

Library structure is  consist of 3 parts

* UI
* Data-access
* Utility

**UI library**

**UI library** -- is a collection of related presentational components. There are generally no services injected into these components (all of the data they need should come from Inputs).

**Naming Convention**

`ui` (if nested) or `ui-*` (e.g., `ui-buttons`)

**Dependency Constraints**

A ui library can depend on ui and util libraries.



### **Data-access libraries**

**Data-access libraries** contain code that function as client-side delegate layers to server tier APIs.

All files related to state management also reside in a `data-access` folder (by convention, they can be grouped under a `+state` folder under `src/lib`).

**Naming Convention**

`data-access` (if nested) or `data-access-*` (e.g. `data-access-seatmap`)

**Dependency Constraints**

A data-access library can depend on data-access and util libraries.



### Utility library

A utility library contains low level code used by many libraries. Often there is no framework-specific code and the library is simply a collection of utilities or pure functions.

**Naming Convention**

`util` (if nested), or `util-*` (e.g., `util-testing`)

**Dependency Constraints**

A utility library can depend only on utility libraries.

An example util lib module: **libs/shared/util-formatting**
