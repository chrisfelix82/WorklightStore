WorklightStore
==============

A dojo/store implementation for IBM Worklight.  It makes use of JSONStore as a native cache where possible.

The store treats the server as the master store, with the local cache only referenced if a connection to the backend is not possible.
Here is an example of a WorklightStore used in a dojox/app config.json file:

```
"stores": {
		"userStore": {
			"type": "commonapp/utils/WorklightStore",
			"observable": true,
			"params": {
				"resourceName": "User",
				"searchFields": {
					"id": "string",
					"name": "string"
				},
				"idProperty": "id"
			}
		}
	},
```

The above code assumes that we have created a Worklight adpater with the name User.  "User" translates to a RESTful resource, and 
the default procedures on the adapter are the following, which can be overriden if needed.

```

		adapterOptions : {
			get    :  "get",
			query  :  "query",
			add    :  "add",
			put    :  "put",
			remove :  "remove"
		},
```

You can use the store like any other dojo/store.  For example, userStore.get(id) would call the User.get(id) adapter procedure.  
The same parameters that are sent to the store, are sent to the adapter.  Here is a sample adapter:

```

function get(id){
	WL.Logger.debug("Getting item with id" + id);
	return {"statusCode" : 200,item: {"id": "100","name": "Chris from server"}};
}

function query(query,options){
	WL.Logger.debug("Query" + JSON.stringify(query));
	WL.Logger.debug("options" + JSON.stringify(options));
	return {"statusCode" : 200, "items" : [{"name" : "Chris Felix","age": 30,"id" : "3"},{"name" : "John Doe", "age" : 31,"id" : "4"},{"name" : "Mary Jane","id" : "5"}]};
}

function put(object,options){
	WL.Logger.debug("Object" + JSON.stringify(object));
	WL.Logger.debug("options" + JSON.stringify(options));
	return {"isSuccessful" : true,statusCode : 200};
}

function add(object,options){
	WL.Logger.debug("Object" + JSON.stringify(object));
	WL.Logger.debug("options" + JSON.stringify(options));
	return {statusCode : 201, item: {"id": 100}};
}

function remove(id){
	WL.Logger.debug("Removing item with id" + id);
	return {"statusCode" : 200};
}
```
