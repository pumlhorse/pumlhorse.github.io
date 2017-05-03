---
title: Pumlhorse
layout: default
---

<div class="col-md-12">
<div class="box">

# Pumlhorse
Pumlhorse is a scripting tool for quickly creating powerful, readable, and reusable scripts.
    
Some practical situations include:

* Test cases
* DevOps support scripts
* Load tests
    
Pumlhorse is built on top of the powerful [Node.js](https://www.nodejs.org) platform and comes with a wide array of built in features. 
It also supports custom modules. 
    
## Blurring the lines of DEV and QA

Too often, there is a separation of responsibilities between developers and testers. Developers are responsible for writing unit tests,
and testers do the integration testing. Pumlhorse helps blur that line by providing a lightweight engine for developers to use, as well
as the ability to create helper modules to simplify testing efforts.

For example, many test cases involve logging into the system somehow. Rather than requiring a long list of precondition steps, 
a developer (or tester, for that matter) can create a "login" function that performs all the steps. This also improves the readability
of the script.

## How do I get it?

After installing Node.js, run the following command:
`npm install -g pumlhorse`
    
See the [documentation](/reference/engine/v2) for more information for running Pumlhorse.

## What else ya got?

Well...

* [Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=mdickin.pumlhorse-vscode) to write and run scripts in the IDE
* [ExpressJs plugin](https://www.npmjs.com/package/pumlhorse-express) to use Pumlhorse as a controller engine
* [Headless browser](https://www.npmjs.com/package/pumlhorse-browser) support, so you can manipulate websites through Pumlhorse scripts
* Modules for interacting with [Microsoft SQL](https://www.npmjs.com/package/pumlhorse-mssql) and [Postgres](https://www.npmjs.com/package/pumlhorse-postgres)

And don't forget the

* [Sandbox for Pumlhorse scripts](https://pumlhorse.glitch.me/)
* [Sample API test suite](https://github.com/pumlhorse/sample-test-suite)
* [Blog post](http://mdickin.com/2017/04/30/writing-weasley-clock-with-alexa/) about using Pumlhorse to make a Weasley Clock for Amazon Alexa!

</div>
</div>