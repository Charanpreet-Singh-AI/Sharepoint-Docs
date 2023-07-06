Sharepoint DOCS 
===============

Here’s the documentation for performing CRUD operations (Read, Insert,
Update, and Delete) on a SharePoint list in SharePoint Online using
JavaScript in the Modern Script Editor web part:

Performing CRUD Operations on SharePoint List using JavaScript in Modern Script Editor
======================================================================================

This documentation provides a step-by-step guide on how to perform
Create, Read, Update, and Delete (CRUD) operations on a SharePoint list
in SharePoint Online using JavaScript in the Modern Script Editor web
part.

Prerequisites
-------------

-  SharePoint Online site with appropriate permissions to access and
   manipulate lists.
-  Modern SharePoint page where you can add and configure the Script
   Editor web part.
-  Basic understanding of JavaScript programming.

Step 1: Set up the SharePoint environment
-----------------------------------------

1. Open your SharePoint Online site.
2. Navigate to the list where you want to perform CRUD operations.
3. Make note of the list name.
4. Add Script in head tag 

.. code:: javascript
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
   <script type="text/javascript" src="https://ajax.aspnetcdn.com/ajax/4.0/1/MicrosoftAjax.js"></script>
   <script type="text/javascript" src="/_layouts/15/sp.runtime.js"></script>
   <script type="text/javascript" src="/_layouts/15/sp.js"></script>

Step 2: Add the Modern Script Editor web part to the page
---------------------------------------------------------

1. Navigate to the SharePoint page where you want to perform CRUD
   operations.
2. Edit the page.
3. Click on the “+” button to add a new web part.
4. Search for “Script Editor” in the web part toolbox and add the
   “Script Editor” web part to the page.

Step 3: Add JavaScript code to the Modern Script Editor
-------------------------------------------------------

1. Edit the added Script Editor web part.
2. Add the following JavaScript code within the web part:

.. code:: javascript

   <script type="text/javascript">
       // SharePoint site URL
       var siteUrl = _spPageContextInfo.siteAbsoluteUrl;

       // SharePoint list name
       var listName = "YourListName";

       // Read list items
       function readListItems() {
           var context = new SP.ClientContext(siteUrl);
           var list = context.get_web().get_lists().getByTitle(listName);
           var camlQuery = new SP.CamlQuery();
           camlQuery.set_viewXml("<View><Query><OrderBy><FieldRef Name='Title' Ascending='TRUE'/></OrderBy></Query></View>");
           var items = list.getItems(camlQuery);

           context.load(items);
           context.executeQueryAsync(
               function () {
                   var listItemEnumerator = items.getEnumerator();
                   while (listItemEnumerator.moveNext()) {
                       var listItem = listItemEnumerator.get_current();
                       var itemId = listItem.get_id();
                       var title = listItem.get_item("Title");
                       var description = listItem.get_item("Description");

                       console.log("Item ID: " + itemId);
                       console.log("Title: " + title);
                       console.log("Description: " + description);
                   }
               },
               function (sender, args) {
                   console.log("Error reading items: " + args.get_message());
               }
           );
       }

       // Create a new list item
       function createListItem() {
           var context = new SP.ClientContext(siteUrl);
           var list = context.get_web().get_lists().getByTitle(listName);

           var itemCreateInfo = new SP.ListItemCreationInformation();
           var newItem = list.addItem(itemCreateInfo);

           newItem.set_item("Title", "New Item");
           newItem.set_item("Description", "Sample description");

           newItem.update();
           context.load(newItem);

           context.executeQueryAsync(
               function () {
                   console.log("Item created successfully. Item ID: " + newItem.get_id());
               },
               function (sender, args) {
                   console.log("Error creating item: " + args.get_message());
               }
           );
       }

       // Update a list item
       function updateListItem(itemId, title, description) {
           var context = new SP.ClientContext(siteUrl);
           var list = context.get_web().get_lists().getByTitle(listName);

           var item = list.getItemById(itemId);


           item.set_item("Title", title);
           item.set_item("Description", description);

           item.update();
           context.executeQueryAsync(
               function () {
                   console.log("Item updated successfully.");
               },
               function (sender, args) {
                   console.log("Error updating item: " + args.get_message());
               }
           );
       }

       // Delete a list item
       function deleteListItem(itemId) {
           var context = new SP.ClientContext(siteUrl);
           var list = context.get_web().get_lists().getByTitle(listName);

           var item = list.getItemById(itemId);
           item.deleteObject();

           context.executeQueryAsync(
               function () {
                   console.log("Item deleted successfully.");
               },
               function (sender, args) {
                   console.log("Error deleting item: " + args.get_message());
               }
           );
       }

       // Call the functions for CRUD operations
       readListItems(); // Read items
       createListItem(); // Create an item
       updateListItem(1, "Updated Item", "Updated description"); // Update an item with ID 1
       deleteListItem(1); // Delete an item with ID 1
   </script>

Make sure to replace ``"YourListName"`` with the name of your SharePoint
list.

3. Save the changes to the Script Editor web part.

Step 4: Test the CRUD operations
--------------------------------

1. Save and publish the SharePoint page.
2. Open the page in a web browser.
3. Check the browser console for the results of the performed CRUD
   operations.

Congratulations! You have successfully performed CRUD operations (Read,
Insert, Update, Delete) on a SharePoint list in SharePoint Online using
JavaScript in the Modern Script Editor web part. Feel free to customize
the provided code according to your specific requirements.
