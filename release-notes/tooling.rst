Omnia Tooling
===============
Here you can find the releases of the Omnia Tooling for Visual Studio 2013/2015

.. contents:: Sections:
  :local:
  :depth: 1

Visual Studio 2017 1.0.5943 Beta 1
--------------------------------------------------

.. note:: 
    - To be able to use this Tooling you need to target a Tenant with Omnia Foundation version 1.0.5862 or higher

**What`s new**

- Completely new Project Wizard and Item Template Wizard that uses Github as source
- Angular 4 support
- Extensible TaskRunner (angular aot, less etc)
- We now use npm for Foundation (https://www.npmjs.com/package/@omnia/foundation)

Please report issues with tooling on our `Github repo <https://github.com/preciofishbone/Omnia-Foundation>`_

`Download vsix <http://nuget.preciofishbone.se/omniatoolings/prod/omniatooling.1.0.5932-beta.vsix>`_

Visual Studio 2017 Preview
--------------------------------------------------

**What`s new**

- Added support for Visual Studio 2017

`Download vsix <http://nuget.preciofishbone.se/omniatoolings/dev/omniatooling.1.0.1.3965-vs2017.vsix>`_

Stable 1.0.1.3965
--------------------------------------------------

**What`s new**

- New extension projects can now build without errors

`Download vsix <http://nuget.preciofishbone.se/omniatoolings/prod/omniatooling.1.0.1.3965.vsix>`_

Stable 1.0.1.2305-62
--------------------------------------------------

**What`s new**

- Update Omnia Control Item Templates for Angular 2 to comply with Angular 2.0.0
- Update Omnia Extension Sample Project Template with new samples of Angular 2

`Download vsix <http://nuget.preciofishbone.se/omniatoolings/prod/omniatooling.1.0.1.2305-62.vsix>`_


Stable 1.0.1.1699-48
--------------------------------------------------

**What`s new**

- Omnia Control Item Templates for Angular 2
- Built in websever for hosting Tenant bundles locally
- Live Reload support for Tenant bundles

.. note:: The Omnia Control Templates for Angular 2 is only for preview purposes since the Angular 2 RTM was just released we removed the bootstrapping for Angular 2 in Foundation until we have a working version running on Angular 2 RTM

**Bug fixes**

- The item template for Field contained a space in the internalname which could cause problems in provisioning
- Item Template for Omnia Control without settings should have enableSettings value set to false and the constructor should not have the ControlConfigService injected

`Download vsix <http://nuget.preciofishbone.se/omniatoolings/prod/omniatooling.1.0.1.1699-48.vsix>`_


Stable 1.0.1.1115-38
--------------------------------------------------

`Download vsix <http://nuget.preciofishbone.se/omniatoolings/prod/omniatooling.1.0.1.1115-38.vsix>`_




