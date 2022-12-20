# NX introduction at Enterprise level

Within an Nx workspace, you gain many capabilities that help you build applications and libraries using a monorepo approach. If you are currently using an Angular CLI workspace, you can transform it into an Nx workspace.

![](<../.gitbook/assets/image (3).png>)

### Creating a New Workspace <a href="#creating-a-new-workspace" id="creating-a-new-workspace"></a>

```
~❯npx create-nx-workspace@latest


 >  NX   Let's create a new workspace [https://nx.dev/getting-started/intro]

✔ Choose what to create                 · angular
✔ Repository name                       · store
✔ Application name                      · store
✔ Default stylesheet format             · css
✔ Enable distributed caching to make your CI faster · No
```

### Creating the application/library

This is an example of CLI commands

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Code Organization & Naming Conventions <a href="#code-organization-and-naming-conventions" id="code-organization-and-naming-conventions"></a>

#### Apps and Libs <a href="#apps-and-libs" id="apps-and-libs"></a>

* Apps configure dependency injection and wire up libraries. They should not contain any components, services, or business logic.
* Libs contain services, components, utilities, etc. They have well-defined public API.

### Scope (Where a library lives, who owns it)

Many enterprise applications are portals: slim shells loading different modules at runtime. If this is what you are building, the following might be a better starting point:



