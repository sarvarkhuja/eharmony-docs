# Creating application

`@nrwl/angular:application` Creates an Angular application. You can specify the collection explicitly as follows:

\


<figure><img src="../.gitbook/assets/Screen Shot 2022-12-20 at 02.47.00.png" alt=""><figcaption></figcaption></figure>

### Options <a href="#options" id="options"></a>

#### name <a href="#name" id="name"></a>

REQUIRED stringPattern: `^[a-zA-Z].*$`

The name of the application.

#### prefix <a href="#prefix" id="prefix"></a>

pstringFormat: `html-selector`

The prefix to apply to generated selectors.

#### routing <a href="#routing" id="routing"></a>

booleanDefault: `false`

Generate a routing module.

#### tags <a href="#tags" id="tags"></a>

string

Add tags to the application (used for linting).

#### port <a href="#port" id="port"></a>

number

The port at which the remote application should be served.

\


#### addTailwind <a href="#addtailwind" id="addtailwind"></a>

booleanDefault: `false`

Whether to configure Tailwind CSS for the application.
