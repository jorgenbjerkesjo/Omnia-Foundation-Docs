Localization
============================

In Omnia, localization strings are stored in JSON tenant resources. You can create new localization file using the template from Omnia toolings

.. image:: /images/toolings-item-templates-localization.png

.. note:: 
    - **Public Localization** - Used to store localization strings for UI in SharePoint
    - **Admin Localization** - Used to store localization strings for UI in Omnia admin app    

Each localization file contains the localization strings for only one language, and it need to follow this naming convention:

- ***.loc.json** for default language (English) localization. Example: sample.loc.json
- ***.loc.[culture code].json** for other langauge localization. Example: sample.loc.sv-se.json

Most of the cases, localization files with the same name but different culture code should contain the same JSON structure with different string values. 

.. note:: 
    The first level of the JSON will aways be "Public" or "Admin" depend on the localization target. You do not need to include "Public" or "Admin" part when getting localized strings.

**sample.loc.json**

.. code-block:: json

    {
        "Public": {
            "MyOmniaExtension": {
                "Sample": {
                     "SiteCollection": "Site Collection",
                     "Site": "Site"
                }
            }
        }    
    }

**sample.loc.sv-se.json**

.. code-block:: json

    {
        "Public": {
            "MyOmniaExtension": {
                "Sample": {
                    "SiteCollection": "Webbplatssamling",
                    "Site": "Webbplats"
                }
            }
        }    
    }

Localization in client-side code
--------------------------------------------------

Once the localization resources have been deployed, your client-side code can get the localized strings in three ways:

- Using the global variable **$localize**. This should only be used if you are writing non-Angular code. For Angular code you should use the other ways for better support.
- Using the filter **omfLocalization** for Angular

 .. code-block:: html

    <span ng-bind="'MyOmniaExtension.Sample.SiteCollection' | omfLocalization">   

- Using the service **localizationService** for Angular    

.. code-block:: javascript

    constructor(private $scope: ISystemLogScope,
        private localizationService: Omnia.Foundation.Services.LocalizationService) {
            this.init();
    }

    private init = () => {
        this.$scope.logTypes = new Array<Log>();
        this.$scope.logTypes.push({
            source: this.localizationService.getText("System.SystemLogs.LogTypes.Info"),
            logType: LogTypes.Info
        });
        this.$scope.logTypes.push({
            source: this.localizationService.getText("System.SystemLogs.LogTypes.Warning"),
            logType: LogTypes.Warning
        });
        this.$scope.logTypes.push({
            source: this.localizationService.getText("System.SystemLogs.LogTypes.Error"),
            logType: LogTypes.Error
        });
    }

Localization in server-side code
--------------------------------------------------

Server-side code can also use localized strings. Typical examples are localized email content in Omnia timer jobs and localized title of SharePoint fields and content types.

.. note:: 
    Currently the title of Omnia features cannot be localized. This may become possible in future version.

**Example**: Localized SharePoint content type

.. code-block:: c#

    [ContentType(id: "78FBA358-10D6-459A-ABD9-6E1539EFF8C0", 
        name: "$Localize:MyOmniaExtension.Sample.ContentTypes.SampleContentType.Name;",
        Group = "Sample Content Type Group", 
        Description = "$Localize:MyOmniaExtension.Sample.ContentTypes.SampleContentType.Description;")]
    public class SampleContentType : Omnia.Foundation.Extensibility.ContentTypes.BuiltIn.Item
    {
        [FieldRef(typeof(SampleField))]
        public string SampleField { get; set; }
    }

**Example**: Get localized strings in timer jobs

.. code-block:: c#

    public void SampleJobTimer([TimerTrigger("01:00:00")] TimerInfo timerInfo)
    {
        try
        {
            string language = "en-US";
                            
            string[] localizationKeys = new string[] { 
                "$Localize:MyOmniaExtension.Sample.EmailSubject;",
                "$Localize:MyOmniaExtension.Sample.EmailContent;" };

            ILocalizationService localizationService = WorkWith().Localization();
            Dictionary<string, string> localizationsResult = 
                localizationService.GetLocalization(localizationKeys, language); 
            
            string localizedEmailSubject = ""; 
            localizationResult.TryGetValue(localizationKeys[0], out localizedEmailSubject);

            string localizedEmailContent = ""; 
            localizationResult.TryGetValue(localizationKeys[1], out localizedEmailContent);
        }
        catch (Exception ex)
        {
            WorkWith().Logging().AddLog("SampleJobTimer", ex.Message, DefaultLogTypes.Error, ex);
        }
    }

Customize localization from Omnia admin app
--------------------------------------------------

End users can change the localized strings using Omnia admin app at **System > Localization**

.. note:: 
    Once a localized string has been changed in the admin app it will not be updated when a newer version of extension package is deployed. To make get the latest version of the localization users need to undo the customization. On the otherhand, when an extension package is removed all customization will also be removed.

.. image:: /images/omnia-admin-localization.png