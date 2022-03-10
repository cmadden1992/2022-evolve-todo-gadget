# 2022 Evolve Todo Gadget
This guide takes you through the process of building a custom gadget in Omni CMS, from scratch! We will be building a To Do List gadget to keep track of our tasks.
## Resources
-   Adding, installing, editing gadgets - [Gadget Support Documentation](https://support.moderncampus.com/learn-omni-cms/administration/setup/gadgets.htmll)
-   Config options, events, APIs, and more - [GitHub - Gadget Starter](https://github.com/omniupdate/gadget-starter)
## Considering Hosting
In order for a custom gadget to be installed in Omni CMS, we need to point to a live URL at which our gadget's files can be found. One cool trick is that you can use Omni CMS itself to publish your gadget files, which is what we will be doing here!
## Recommended File Structure
If we're going to host our files in Omni CMS, let's start by setting up the folder structure we want. Right now you are building one gadget, so you might be tempted to put all of the required files in one gadget folder. However, resource files like Bootstrap, jQuery, and the gadgetlib can be reused, so we should consider putting them in a separate folder that all future gadgets we create can share. It saves resources, and it's better architecture. Let's setup create a folder in Omni CMS that is called **custom-gadgets** 
## Upload the Starter Files to Omni CMS
- Navigate to  **/custom-gadgets** or create the folder.
- Compress the gadgets folder 
- Upload and Import the Zip File
	- Ignore Root Folder is checked
- This will create the following folder/file structure:
```
/custom-gadgets
	/lib
		bootstrap-5.1.3.min.css
		gadgetlib.js
		gadgetlib.min.js
		jquery-3.6.0.min.js
	/to-do-list (the final gadget)
		config.xml
		index.html
		noun-clipboard.svg
		noun-clipboard-dark.svg
	/to-do-list-starter (the starter gadget)
		config.xml
		index.html
		noun-clipboard.svg
		noun-clipboard-dark.svg
```
## Publish all the Files
After upload, we will select all the rows in custom-gadgets and get them published out. In the Publish confirmation, check the option for "Include Checked-Out Files" just in case we had any of the files checked out, and then Publish. You will receive the success notification publish.
## Adding the Two Gadget to Omni CMS
1. Now that our files have been published, we can add the "Gadget" to Omni CMS. Navigate to the setup screen for gadgets via **Setup --> Gadgets**. There you should see a list of the gadgets available to you on Omni CMS, a number of which are system gadgets provided by Modern Campus.
2. To add your own custom gadget, there is a new button. You will be asked for a live URL to the root of your gadget.
	- Example live URLs of the two gadgets:
		-  `https://[workshop-domain].com/custom-gadgets/to-do-list/`
		- 	`https://[workshop-domain].com/custom-gadgets/to-do-list-starter/`
3. After entering the URL,  use the Fetch button and it will look for the config.xml file at the root folder of the provided URL. Once the gadget has been fetched, use the next button to Configure Gadget" modal will pop up. Let's make sure the following Access Settings are selected before hitting "Save":
	-   Available To: Everyone
	-   Display Option: Default On
4. Save your gadget. Your custom gadget has been added to Omni CMS!
5. Repeat the steps to add the other gadget.
## Verify Our Gadgets Show Up
Go back to **Content > Pages** and open up the gadget sidebar. If you don't see the "To Do List Final" and "To Do List Starter" gadget in the sidebar, use on the "Gear" icon next to "Gadgets" at the top of the sidebar to configure the sidebar. That will open up a modal in which you should see available gadgets. Find "To Do List Final" and "To Do List Starter" and make sure it is checked, then "Save".
## Adjusting Options in config.xml
Don't forget to save and re-publish the file after making changes.
`NOTE: The config.xml is where you specify a number of configuration options for a gadget. A full explanation of the available options can be found in the ReadMe file on the gadget-starter GitHub page:` [GitHub - Gadget Starter](https://github.com/omniupdate/gadget-starter)
## Updating the Gadget in Omni CMS
After making config changes for any custom gadget, we have to let Omni CMS know to update its' configuration for the gadget:
1.  Navigate to **Setup > Gadgets**
2. Use the "Refresh" option in the row actions.
## Let's Get Started!
### Step 1) Explore what is included
Open **to-do-list-starter/index.html** in Omni CMS in the `<>Source` tab. You'll notice that the contents of the file as it comes from the starter are basic skeleton of the gadget.
`Note: From now on, any time we make changes to our HTML, we have to save & publish index.html, and then refresh the gadget to see the changes.`
	- ***Tip: You can use the Quick Publish Gadget to republish the gadget files as we go***
### Step 2) Link Your Resources
Let's explore the resources in the _head_ of the gadget's HTML file. Links and scripts added here are called "blocking" scripts because they have to be resolved before the browser can render the rest of the page, so it is best practice to only include here what is necessary for the page to render correctly. We will need make sure the Bootstrap CSS file, jQuery JS file, and GadgetLib JS file for the gadget has been added
```html
<head>
    <link rel="stylesheet" href="../lib/bootstrap-5.1.3.min.css">
    ...
    <script type="text/javascript" src="../lib/jquery-3.6.0.min.js"></script>
    ...
    <script type="text/javascript" src="../lib/gadgetlib.js"></script>
</head>
```
With this setup a direct edit link in the gadget iframe will not be visible.
`NOTE: It is a good idea to periodically save, publish, and refresh your gadget iframe to make sure things are working correctly.`
### Step 3) Reviewing Overall HTML Structure
Let's consider the overall HTML structure we would need for a "to do list" type application. We would need:
-   A text input
-   An add button
-   A list
Looking at what we need, we could also decide to group the input and submit button together at the top of the gadget, and make the list the rest of the body. This is how we accomplished this structure in the HTML:
```html
<body>
   <div id="main">
		<div class="header mb-2">
			<div class="input-group input-group-sm">
				<textarea type="text" id="todo-input" name="task-input" placeholder="Add a To Do..." maxlength="100" class="form-control"></textarea>
				<button id="add-todo" class="btn btn-success">+ Add</button>
			</div>
		</div>
		<div id="list-container">
			<ul id="todo-list" class="mb-3"></ul>
		</div>
	</div>
	<div style="display:none;">
		<!-- ouc:ob --><!-- /ouc:ob -->
	</div>
	....
	<!-- ouc:editor wysiwyg="no" --><!-- /ouc:editor -->
</body>
```
### Step 4) What are these Strange Comments?
There are two "strange" HTML comments that you may question, so here are explanations to demystify them.
- This comment takes the automatic direct edit link added to all published HTML files from Omni CMS and places it in a visually hidden div.
```html
	<div style="display:none;">
		<!-- ouc:ob --><!-- /ouc:ob -->
	</div>
```
- This comment is somewhat of an unknown feature in the CMS but allows to not have the ability to open the WYSIWYG editor for the file and will force the edit link to the source editor of the given file.
```html
	<!-- ouc:editor wysiwyg="no" --><!-- /ouc:editor -->
```
### Step 5) Basic Styling Overrides
Review the CSS in a `style` block in the document `head`. Your own styles should come **after** the Bootstrap stylesheet:
```html
<link rel="stylesheet" href="../lib/bootstrap-5.1.3.min.css">
<style type="text/css">
	html {
		box-sizing: border-box;
	}
	*, *:before, *:after {
		box-sizing: inherit;
	}
	body {
		background-color: inherit;
		overflow-y: hidden;
	}
	#main {
		height: 100%;
		width: 100%;
		position: absolute;
	}
	.header {
		width: 100%;
		padding: 0px 5px;
		margin-top: 2%;
		position: sticky;
		top: 0;
	}
	#todo-input {
		overflow-y: auto;
		resize: none;
		border: 1px solid #BBBBBB;
		border-right: none;
	}
	#add-todo {}
	#list-container {
		width: 100%;
		height: 79%;
		padding: 5px 0px 5px 5px;
	}
	#todo-list {
		margin: 0px;
		padding: 0px;
		height: 100%;
		overflow-y: auto;
	}
	.list-item {
		width: 100%;
		position: relative;
	}
	.todo {
		width: 75%;
		word-break: break-word;
		display: block;
	}
	.content-edit {
		display: inline-block;
		overflow-y: auto;
		resize: none;
		border: 1px solid #BBBBBB;
		border-radius: 3px;
	}
	.list-item .edit:not(.checked):not(.editing){
		display: block;
	}
	.act-btn {
		display: none;
		position: absolute;
		z-index: 10;
		right: 5px;
		top: 0px;
		height: 22px;
		width: 50px;
		font-size: 12px;
		padding: 0px;
	}
	.delete {}
	.delete.checked {
		display: block;
	}
	.save {
		width: 19%;
		left: 80%;
	}
	.save.editing {
		display: block;
	}
	input[type="checkbox"]:checked + label {
		text-decoration: line-through;
		color: #767676;
	}
</style>
```
### Step 7) Reviewing what JavaScript is already there
- There is a todo class, which has a ton of helper functions that are there but no functionality
	- The todo class will handle each todo as they are added/edited/deleted or initially rendered.
- The mine_type of what metadata we are storing on Omni CMS.
- Various helper methods for working with todo items
- Do Stuff after the Gadget is Ready
	 - You must wait for the gadget to be ready before doing any API calls.
```javascript
<script type="text/javascript">
    document.addEventListener('DOMContentLoaded', async () => {
		// Makes sure the global "gadget" object is instantiated with your access token for you to start making API calls
		await gadget.ready();
	});
</script>
```
### Step 8) Our goals of this custom gadgets
1. On gadget load, the list will show all todo items.
2. Ability to create a todo.
3. Ability to edit todo.
4. Ability to complete todos.
5. Ability to delete completed todo.
## Building the Gadget!
### Step 1) Selecting a "mime_type" for the Metadata API
By selecting a mime_type this allows you to group all the todos under a "key"
```javascript 
const mime_type = 'cms-gadget-metadata-todo/json';
```
### Step 2) The Event Listener and Validation for the Add button
After the document is loaded, we can use JavaScript to attach an event listener to the "+ Add" button:
```javascript
document.getElementById('add-todo').addEventListener('click', (evt) => {
	const text = inputEl.value;
	if (text) { createTodo(text); }
});
```
Method to call after the gadget is ready:
```javascript
addToDoHandler();
```
### Step 2) Defining a **createTodo** method
The metadata API methods use jQuery Ajax, so we can use async/await for the promise to complete.
```javascript
const createTodo = async (content) => {
   const metadata = { content, complete: false };
   const data = await gadget.Metadata.create({
       mime_type,
       metadata: JSON.stringify(metadata),
   });
   const newTodo = new TodoItem({
       id: JSON.parse(data.metadata),
       data: metadata,
       insertType: 'afterbegin',
   });
   
   allTodos.unshift(newTodo);
   newTodo.render();
   // new todo items goes to the top.
   inputEl.value = '';
};
```
### Step 3) Render our Saved Todos on Gadget Load
Now that we are saving Todos with the Metadata API, let's figure out how to retrieve and render all of our saved Todos.

When the _gadget_ object is ready, let's trigger a method to get our Todos, and then a method to render them:
```javascript
await fetchTodos();
```
Retrieving our Todos and rendering:
```javascript
// renders todo list when passed the array of todos
const renderTodos = (data) => {
	todoList.innerHTML = '';
	data.forEach((rawData) => {
		let todo = {
			rawData,
			id: rawData.id,
		};
		// just in case there is any errors when receiving the metadata
		try {
			todo.data = JSON.parse(rawData.metadata);
		} catch (e) {
			todo.data = { };
			todo.data.content = "Corrupted Todo. Please contact admin.";
		}
		const finalTodo = new TodoItem(todo);
		allTodos.push(finalTodo);
		finalTodo.render();
	});
};
// do the fetch for todos
const fetchTodos = async () => {
	allTodos.length = 0;
	try {
		// calls Metadata 'list' method, calls for list render method upon completion
		const data = await gadget.Metadata.list({ mime_type });
		renderTodos(data);
	} catch (error) {
		console.log(error);
	}
};
```
At this point we should have the todo's rendering on gadget load.
### Step 4) Complete the Todo on Usage of the Checkbox
We need to add to `checkboxEvent` and `updateTodo` method on the todo class to have this functionality.
```javascript
async updateTodo() {
	const metadata = JSON.stringify({ content: this.content, complete: this.complete });
	// calls Metadata 'update' method, returns deferred object
	await gadget.Metadata.update({
		action: 'update',
		mime_type,
		id: this.id,
		metadata,
	});
}
checkboxEvent() {
	this.findElement('input[type="checkbox"]').addEventListener('click', async (evt) => {
		this.complete = !this.complete;
		this.isChecked = `${this.complete ? 'checked' : ''}`;
		// update todo in database and add/remove classes as necessary
		try {
			const data = await this.updateTodo();
			this.findElement('.delete').classList.toggle('checked');
			this.findElement('.edit').classList.toggle('checked');
		} catch (error) {
			console.log(error);
		} finally {
			this.setElement();
		}
	});
}
```
### Step 5) Delete completed Todos
We only want to allow deletion of completed todos, so let's add that functionality. We need to add to the `removeTodo` and `deleteEvent` methods.

```javascript
async removeTodo() {
	// calls Metadata 'delete' method, returns deferred object
	await gadget.Metadata.delete({ mime_type, id: this.id });
}
deleteEvent() {
	this.findElement('[data-action="delete-todo"]').addEventListener('click', async (evt) => {
		try {
			const data = await this.removeTodo();
			this.el.remove();
			const index = allTodos.indexOf(getTodoFromList(this.id));
			allTodos.splice(index, 1);
		} catch (error) {
			console.log(error);
		}
	});
}
```
### Step 6) Enable Editing of Todos that are Not Completed
We only want to allow editing of todos, so let's add that functionality. We need to adjust the `removeTodo` and `deleteEvent` methods to have the functionality
```javascript
setEditingClasses() {
	this.findElement('.edit').classList.toggle('editing');
	this.findElement('.save').classList.toggle('editing');
	this.setElement();
}
removeInput() {
	this.findElement(`.${this.editTextAreaClass}`).remove();
	this.findElement('label').style.display = null;
	this.setEditingClasses();
}
editEvent() {
	this.findElement('[data-action="edit-todo"]').addEventListener('click', () => {
		// replace todo with textarea, prefill with todo content, show save button
		this.findElement('label').style.display = 'none';
		const textArea = document.createElement('textarea');
		textArea.className = this.editTextAreaClass;
		textArea.maxLength = 100;
		textArea.value = this.content;
		textArea.addEventListener('keydown', (evt) => {
			if (evt.key.toUpperCase() === "ESCAPE") { this.removeInput(); }
		});
		this.findElement('.todo').appendChild(textArea);
		textArea.select();
		this.setEditingClasses();
	});
}
saveEvent() {
	this.findElement('[data-action="save-todo"]').addEventListener('click', async (evt) => {
		const newValue = this.findElement(`.${this.editTextAreaClass}`).value.trim();
		if (!newValue || newValue === this.content.trim()) {
			this.removeInput();
			return;
		}
		this.content = newValue;
		try {
			await this.updateTodo();
			this.setContent();
			this.removeInput();
		} catch (error) {
			console.log(error);
		} finally {
			this.setElement();
		}
	});
}
```
## Additional Features!
We have created the basic functionality we need. We can create, list, edit, and delete our Todos. Now it's up to you to go further! Here are some ideas of how you can make the To Do List gadget do more for you:
-   Add a loading state for the gadget to make it more user friendly
-  Add notifications inside of the gadgets of potential errors/success or whenever needed.
-  Add sort options for your todos. Sort by most recent, or by oldest, or anything else you may want, like drag and drop.
-  Link Todos to specific files using Metadata linking.
- Group together todos under specific priority levels.
