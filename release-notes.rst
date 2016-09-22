Release Notes
===============

1.0.1.1681 (2016-09-20)
--------------------------------------------------

**What`s new**

- Its now possible in the API for a extension to change the configuration created by another extension.

- New feature called "Omnia Statistics Provider" makes it possible to register Third-party analytic scripts in the Admin UI to be included in all sites where Omnia Masterpage is activated.

- Targeting api that makes it possible to specify targeting definitions in Omnia Admin using Taxonomy / Profile Properites or Security Groups and then use the api in extensions to filter content based on selected targeting.

- Omnia Jobs is a new feature that makes it possible to create Jobs in Extensions that can be based on a Timer or by listening on a Queue

- OmniaApi support to work with queues 

- Updated font awesome to version 4.6.3

- Omnia Console now has support to listen to commands. Currently we support two commands that enables support for the built in webserver in Omnia Tooling for example

  - hosting enable https://localhost:9930
  - hosting disable

- A new section in Omnia Admin > System called Developers was added that lists the version of Omnia Foundation and also displays the nuget version to be used for Extensions.

- OmniaApi now has a SitesService that has the following methods GetSiteRequestById, GetRecentSites, AddOrUpdateRecentSite

**Bug fixes**

- When provisioning a Field without a description an error was thrown

- Left navigation in admin missing scroll support

- Slow performance in Features view in Admin

- Fixed an performance issue where the JobHost was loading all extension packages from database at startup

- When adding new properties to a site template and performing a migration the migration would stop if a site was deleted

- When using approval flow in site requests there was sometimes a timeout error when approving the request

- When editing site templates deployed by extensions the save button was always disabled

- The announcements control had a problem with overflow when long text without space was in the message

- When uploading a extension package with configuration we now always sort it in alphabetic order

- The customized localization made in the localization editor in Omnia Admin for an extension package was removed when a feature was upgraded.

- Selected features in site templates was not checked in UI when the template was loaded

- When specifying a FieldRef using [FieldRef(typeof(Field1), Required = true)] it didnt listen on the Required property.