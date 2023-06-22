Sample documentation for performing CRUD operations on a SharePoint list in SharePoint Online using JavaScript in the Modern Script Editor web part:

## Performing CRUD Operations on SharePoint List using JavaScript in Modern Script Editor

This documentation provides a step-by-step guide on how to perform Create, Read, Update, and Delete (CRUD) operations on a SharePoint list in SharePoint Online using JavaScript in the Modern Script Editor web part.

### Prerequisites

- SharePoint Online site with appropriate permissions to access and manipulate lists.
- Modern SharePoint page where you can add and configure the Script Editor web part.
- Basic understanding of JavaScript programming.

### Step 1: Set up the SharePoint environment

1. Open your SharePoint Online site.
2. Navigate to the list where you want to perform CRUD operations.
3. Make note of the list name.

### Step 2: Add the Modern Script Editor web part to the page

1. Navigate to the SharePoint page where you want to perform CRUD operations.
2. Edit the page.
3. Click on the "+" button to add a new web part.
4. Search for "Script Editor" in the web part toolbox and add the "Script Editor" web part to the page.

### Step 3: Add JavaScript code to the Modern Script Editor

1. Edit the added Script Editor web part.
2. Add the following JavaScript code within the web part:

```javascript
<script type="text/javascript">
    // SharePoint site URL
    var siteUrl = _spPageContextInfo.siteAbsoluteUrl;

    // SharePoint list name
    var listName = "YourListName";

    // Create a new list item
    function createListItem() {
        var itemProperties = {
            Title: "New Item",  // Replace with your own column names and values
            Description: "Sample description"
        };

        var context = new SP.ClientContext(siteUrl);
        var list = context.get_web().get_lists().getByTitle(listName);
        var itemCreateInfo = new SP.ListItemCreationInformation();
        var newItem = list.addItem(itemCreateInfo);

        for (var propName in itemProperties) {
            newItem.set_item(propName, itemProperties[propName]);
        }

        newItem.update();
        context.load(newItem);

        context.executeQueryAsync(
            function () {
                console.log("Item created successfully.");
            },
            function (sender, args) {
                console.log("Error creating item: " + args.get_message());
            }
        );
    }

    // Call the createListItem function
    createListItem();
</script>
```

Make sure to replace `"YourListName"` with the name of your SharePoint list.

3. Save the changes to the Script Editor web part.

### Step 4: Test the create operation

1. Save and publish the SharePoint page.
2. Open the page in a web browser.
3. Check the browser console for the success or error message.

### Step 5: Perform read operation

To retrieve items from the SharePoint list, follow these steps:

1. Edit the Script Editor web part on the SharePoint page.
2. Add the following JavaScript code within the web part:

```javascript
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
        var

 items = list.getItems(camlQuery);

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

    // Call the readListItems function
    readListItems();
</script>
```

Make sure to replace `"YourListName"` with the name of your SharePoint list.

3. Save the changes to the Script Editor web part.

### Step 6: Test the read operation

1. Save and publish the SharePoint page.
2. Open the page in a web browser.
3. Check the browser console for the retrieved list items.

You can follow similar steps to add JavaScript code for update and delete operations within the Script Editor web part, using the appropriate SharePoint client object model (JSOM) methods.

Congratulations! You have successfully performed CRUD operations (Create, Read, Update, Delete) on a SharePoint list in SharePoint Online using JavaScript in the Modern Script Editor web part. Feel free to customize the provided code according to your specific requirements.
