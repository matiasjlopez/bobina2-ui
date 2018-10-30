# bobina2-ui

bobina2-ui provides a set of React Views to display and interact with objects of different data structures. These views work together to provide full web apps. 

With bobina2-ui you can quickly make a web application UI by configuring views with metadata instead of hand-coding Javascript, CSS, and HTML.

bobina2-ui works hand-in-hand with [bobina2-server](https://github.com/matiasjlopez/bobina2-server) which provides the matching REST endpoints based on the same metadata.

This project is a **work in progress**. I'm still learning React.



## Installation

[**Download**](https://github.com/matiasjlopez/bobina2-ui/archive/master.zip) or **clone** from [GitHub](https://github.com/matiasjlopez/bobina2-ui/).

```bash
# To get the latest stable version, use git from the command line.
git clone https://github.com/matiasjlopez/bobina2-ui
```

In the bobina2-ui directory, use the command line to type the following:

```bash
# Install dependencies
npm install

# Run the node.js server
npm start

```

In a web browser, go to the url [http://localhost:8080/](http://localhost:8080/).

For the REST endpoints, you also need to setup [bobina2-server](https://github.com/matiasjlopez/bobina2-server).


## Views

For any object, a single model defines UI elements across views in a simple declarative way.

Evolutility-UI-React provides 2 types of view:

* Views for a model: [Browse](#browse), [Edit](#edit).
* Views for a collection: [List](#list), [Cards](#cards), [Charts](#charts).

Notes: Views for actions will come later.

A large part of the API (methods, options and events) is common to all views. Some views have additional API.

## Views for One object
### Browse
Shows all fields for viewing (read only). Fields are grouped in panels.

![Browse](https://raw.githubusercontent.com/evoluteur/evolutility-ui-react/master/public/screenshots/comics/one-browse.gif)

Code: [/src/components/views/one/Browse.js](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/components/views/one/Browse.js)

### Edit
This view shows all fields for edition to create or update records.
It automatically performs validation based on the model.
Fields are grouped in panels and tabs.

![Edit](https://raw.githubusercontent.com/evoluteur/evolutility-ui-react/master/public/screenshots/comics/one-edit.gif)

Code: [/src/components/views/one/Edit.js](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/components/views/one/Edit.js)


## Views for Many objects
### List
Gives a tabular view of a collection.

![List](https://raw.githubusercontent.com/evoluteur/evolutility-ui-react/master/public/screenshots/comics/many-list.gif)

Code: [/src/components/views/many/List.js](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/components/views/many/List.js)

### Cards
Shows records side by side as cards.

![Cards](https://raw.githubusercontent.com/evoluteur/evolutility-ui-react/master/public/screenshots/comics/many-cards.gif)

Code: [/src/components/views/many/Cards.js](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/components/views/many/Cards.js)

### Charts
Draws charts about the collection.

![Charts](https://raw.githubusercontent.com/evoluteur/evolutility-ui-react/master/public/screenshots/comics/many-charts.gif)

Code: [/src/components/views/many/Charts.js](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/components/views/many/Charts.js)




## Models

For each application, several views will be generated form a single model. 
Models describe the object and its list of fields.

### Entity

| Property     | Meaning                                 |
|--------------|-----------------------------------------|
| id           | Unique key to identify the entity (used as API parameter). |    
| icon         | Icon file name for the entity (example: "cube.gif". |  
| name         | Object name (singular).    |
| namePlural   | Object name (plural).      |
| title        | Application name.          |
| fields       | Array of fields.           |
| groups       | Array of groups. If not provided a single group will be used.   |
| titleField   | Field id for the column value used as record title. titleField can also be a function. | 

### Field

| Property     | Meaning                               |
|--------------|---------------------------------------|
| id           | Unique key for the field (can be the same as column but doesn't have to be). |
| type         | Field type to show in the UI. Possible field types: <ul><li>boolean (yes/no)</li><li>date</li><li>datetime</li><li>decimal</li><li>document</li><li>email</li><li>image</li><li>integer</li><li>lov (list of values)</li><li>money</li><li>text</li><li>textmultiline</li><li>time</li><li>url</li></ul> |
| required     | Determines if the field is required for saving.      |
| readonly     | If set to true, the field value cannot be changed.   |       
| defaultValue | Default field value for new records.                 |     
| max, min     | Maximum/Minimum value allowed (only applies to numeric fields).      |    
| maxLength, minLength | Maximum/Minimum length allowed (only applies to text fields).      |                    
| inMany       | Determines if the field is present (by default) in lists of records. |                     
| height       | For fields of type "textmultiline", number of lines used in the field (in Browse and Edit views). |                 
| width        | Percentage width in Browse and Edit views. |
| help         | Optional help on the field. |
| noCharts     | Prevent the field to have a charts (only necessary for fields of type integer, decimal, money, boolean, list of values which are "chartable"). |

### Group

| Property     | Meaning                               |
|--------------|---------------------------------------|
| id           | Unique key for the group. It is optional.            |
| type         | Only "panel" is currently supported ("tab" is next). |
| label        | Group title as displayed to the user.      |
| fields       | Array of field ids.                        |


### Sample model

The following example is the model for a simple graphic novels inventory app. 

```javascript
module.exports = {
    id: "comics",
    label: "Graphic Novels",
    name: "graphic novel serie",
    namePlural: "graphic novel series",
    icon: "comics.png",
    titleField: "title",
    searchFields: ["title", "authors", "notes"]

	fields:[
      {
          id: "title", type: "text", label: "Title", required: true, 
          maxLength: 255,
          width: 100, inMany: true
      },
      {
          id: "authors", type: "text", width: 62, inMany: true,
          label: "Authors"
      },
      {
          id: "genre", type: "lov", label: "Genre", 
          width: 38, inMany: true,
          list: [
            {id: 1, text: "Adventure"},
            {id: 2, text: "Fairy tale"},
            {id: 3, text: "Erotic"},
            {id: 4, text: "Fantastic"},
            {id: 5, text: "Heroic Fantasy"},
            {id: 6, text: "Historic"},
            {id: 7, text: "Humor"},
            {id: 8, text: "One of a kind"},
            {id: 9, text: "Youth"},
            {id: 10, text: "Thriller"},
            {id: 11, text: "Science-fiction"},
            {id: 12, text: "Super Heros"},
            {id: 13, text: "Western"} 
          ]
      },
      {
          id: "serie_nb", type: "integer", 
          width: 15, inMany: false,
          label: "Albums", noCharts: true 
      },
      {
          id: "have_nb", type: "integer", 
          width: 15, inMany: false,
          label: "Owned", noCharts: true 
      },
      {
          id: "have", type: "text", 
          width: 15, inMany: false,
          label: "Have" 
      },
      {
          id: "language", type: "lov", label: "Language", 
          width: 17, inMany: true,
          lovicon: true,
          list: [
            {id: 2, text: "French", icon:"flag_fr.gif"},
            {id: 1, text: "American", icon:"flag_us.gif"}
          ]
      },
      {
          id: "complete", type: "boolean", 
          width: 19, inMany: false,
          label: "Complete"
      },
      {
          id: "finished", type: "boolean", 
          width: 19, inMany: false,
          label: "Finished"
      },
      {
          id: "pix", type: "image", 
          width: 30, inMany: true,
          label: "Cover"
      },
      {
          id: "notes", type: "textmultiline", 
          label: "Notes", maxLength: 1000,
          width: 70, height: 7, inMany: false
      }
  ],

  groups: [
      { 
        id:"serie", type: "panel", label: "Serie", width: 70,
        fields: ["title", "authors", "genre", 
              "serie_nb", "have_nb", "have", 
              "language", "complete", "finished", "notes"]
      },
      { 
        id:"pix", type: "panel", label: "Album Cover", width: 30,
        fields: ["pix"]
      }
  ]
}


```

More sample models: [To-do list](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/models/todo.js),
 [Address book](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/models/contact.js),
  [Restaurants list](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/models/restaurant.js),
    [Wine cellar](https://github.com/evoluteur/evolutility-ui-react/blob/master/src/models/winecellar.js). 


## Other implementations of Evolutility

[Evolutility-UI-jQuery](https://github.com/evoluteur/evolutility-ui-jquery) - Model-driven Web UI for CRUD using jQuery and Backbone (for REST or localStorage).

[Evolutility-Server-Node](https://github.com/evoluteur/evolutility-server-node) - RESTful Micro-ORM for CRUD and more, written in Javascript, using Node.js, Express, and Postgres.

[Evolutility-ASP.net](https://github.com/evoluteur/evolutility-asp.net) - Lightweight CRUD framework for heavy lifting with ASP.net and Microsoft SQL-Server.

## License

Copyright (c) 2018 [Olivier Giulieri](https://evoluteur.github.io/).

Evolutility-UI-React is released under the [MIT license](http://github.com/evoluteur/evolutility-ui-react/blob/master/LICENSE.md).

To suggest a feature or report a bug: [https://github.com/evoluteur/evolutility-ui-react/issues](https://github.com/evoluteur/evolutility-ui-react/issues)

