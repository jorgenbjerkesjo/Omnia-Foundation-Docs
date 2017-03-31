Lists
============================

The Lists API contains methods that makes it easier to work with lists and libraries in SharePoint.

You reach the Lists API through the following service

.. code-block:: c#

	OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx);
	

.. contents:: The API contains the following methods:
  :local:
  :depth: 1

AddFileToList
#######

Use this method to create a file in a library based on a supplied byte array containing the file data.

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.AddFileToList(Guid listId, string folderServerRelativeUrl, string fileName, string fileType, byte[] data);	

Pass in the GUID of the library, a server relative Url to a folder (or empty string to add in root folder), a file name, and a file type (empty string)
	
Optionally, you can also supply a boolean indicating if the file should be published or not

.. code-block:: c#

	void OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.AddFileToList(Guid listId, string folderServerRelativeUrl, string fileName, string fileType, byte[] data, bool publish);

GetAllDocumentLibraries
#######

To get information about all document libraries on the current web, use the following method

.. code-block:: c#

	IEnumerable<ListIdentifier> OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.GetAllDocumentLibraries();
		
The returned array contains **ListIdentifier** objects containing for example *Title*, *Id* and *ListUrl* for the document libraries.

GetAllImageLibraries
#######

To get information about all image libraries on the current web, use the following method

.. code-block:: c#

	IEnumerable<ListIdentifier> OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.GetAllImageLibraries();
		
The returned array contains **ListIdentifier** objects containing for example *Title*, *Id* and *ListUrl* for the image libraries.

GetDocuments
#######

To get documents from a library, you can use the following method

.. code-block:: c#

	IEnumerable<DocumentIdentifier> OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.GetDocuments(Guid listId, ListQuery listQuery, bool recursive = true, string folderServerRelativeUrl = "");
		
Supply a **ListQuery** object to filter the results.
		
To get a paged subset of the documents, you can instead use the following method

.. code-block:: c#

	IEnumerable<DocumentIdentifier> OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		GetDocuments(Guid listId, string searchString = null, int skipId = 0, int take = -1, string orderBy = null, bool ascending = true, bool isGetAbsoluteUrl = false);
		
Passing in a **skipId** decides from which item to start fetching, and **take** sets the number of documents to return. You can also supply *orderBy* and *ascending* parameters to decide the sort order.

The returned **DocumentIdentifier** class contains basic information about the documents in the library, for example *Id*, *Title*, *FileName*, *DocumentUrl* and more.

GetDocumentsByFolder
#######

Much like the **GetDocuments** method, you can use this method to get a paged subset of documents, but in a specific folder

.. code-block:: c#

	IEnumerable<DocumentIdentifier> OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		GetDocumentsByFolder(Guid listId, string folderUrl, string searchString = null, int skipId = 0, int take = -1, string orderBy = null, bool ascending = true);

Pass in site relative **folderUrl**

GetListItems
#######

Use this method to get list items from a list

.. code-block:: c#

	IEnumerable<ListItem> OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.GetListItems(Guid listId, ListQuery listQuery);
		
Note that this returns the full **ListItem** objects. Use the **listQuery** parameter to filter what items are returned.

GetPageList
#######

.. note:: Publishing webs only

To get the **Pages** list of the current web use one of the following methods

.. code-block:: c#

	List OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.GetPageList(Web web, string webUrl);
		
or (to target a specific list)

.. code-block:: c#

	List OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		.GetPageList(Web web, string webUrl, string listId);
		


GetPageListId
#######

.. note:: Publishing webs only

To get the **Guid** of the **Pages** library on a publishing web, use the following method

.. code-block:: c#

	Guid OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		GetPageListId(Web web, string webUrl);

GetPageListUrl
#######

.. note:: Publishing webs only

To get the **URL** of the **Pages** library on a publishing web, use the following method

.. code-block:: c#

	string OmniaApi.WorkWith(Ctx.Omnia()).Lists(Ctx)
		GetPageListUrl(Web web, string webUrl);

