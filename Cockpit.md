---
tags: documentation
share: true
---

# Collections

Collections are a bit like tables in a SQL database. The big difference is that they're document based, so there is no pre-defined schema. A document in a collection is called an entry and is stored in JSON. Collections can also define fields which are inputs that allow users to add new data to the collection.

## Fields

When editing a collection, you can add fields to add some structure to the data that is stored in the collection. They also allow you to add data to the collection without having to create API requests making them handy for non-tech savy people.

### Field properties

A field has some properties that define its behavior, but also its appearance. I will be explaining the properties using the JSON representation of a field, but the properties match closely with the UI.

```json
{
  name: "name",
  label: "What is your name?",
  type: "text",
  default: "",
  info: "We use your data responsibly",
  group: "Basic information",
  localize: false,
  options: [],
  width: "1-1",
  lst: true,
  acl: []
}
```

**name**

_Required_

This is the name of the property this will create. In the example above, we want to get the name from the user, and that name is stored in the `name` property when they add it to the collection. 

### **label**

_Optional, defaults to the value of `name`_

This is the text that shows above the input when showing the collection form to a user.

### **type**

_required_

This specifies what kind of data the user should provide. Common examples are `text`, `boolean`, `select`, and `multipleselect`. There are also some that are more tailored to layout like `set`.

### **default**

_Optional, no default_

The default value of the field. If no default is set or an incorrect value is set, this will have no effect on the resulting input.

### **info**

_Optional, no default_

This allows you to put some text below the input. Can be useful if there is some special behavior you want to comunicate to the user.

### **group**

_Optional, no default_

When you set this property, you create a new tab to the form when adding a new entry using the collection form. Adding another field to the same group adds it to the same tab. This allows the user to filter the fields and gives a better overview.

### **localize**

_Optional, `false`_ by default

A cockpit server can have support for multiple languages. The user can select a language they want the content of the form to be in, fill in the form and then select another language to fill the form in that language. By setting `localize` to true, the field is affected by this, meaning that it will ask different values for all the languages.

### **options**

_Optional or required depending on the `type`_

In options you can specify type specific settings like the items that you can select in a `select` field. The required properties per field type can be [here](notion://www.notion.so/rster2002/Cockpit-1ddb52abd0b44189bc64fc0cbe7fa75b#field-types).

### **width**

_Required, `"1-1"` by default_

Specifies how wide the input should be.

- **1-1** full width of the form;
- **1-2** half the width of the form;
- **1-3** one third of the form;
- **2-3** two thirds of the form;
- **1-4** one fourth of the form;
- **3-4** three fourths of the form.

# Field types

### **select**

A select field needs a list of options that it can present to the user. To add these options do something like this:

```json
{
  "options": [
    "A",
    "B",
    "C"
  ]
}
```

If you want some extra settings to be available you can replace the strings in the arrays for objects:

```json
{
  "options": [
    { "label": "A", "value": "valueForA", "group": "One" },
    { "label": "B", "value": "valueForB", "group": "One" },
    { "label": "C", "value": "valueForC", "group": "Two" },
  ]
}
```

The value of `label` will be displayed to the user, the `value` is that will be set in the entry, and the `group` allows you to group certain options together.

### collectionlink

# Filtering collections

Code documenation for filter functions can be found here:

[cockpit/Database.php at 494765e4f0fb9484f320aee0c6ee889b6fa789b9 · agentejo/cockpit](https://github.com/agentejo/cockpit/blob/494765e4f0fb9484f320aee0c6ee889b6fa789b9/lib/MongoLite/Database.php#L361)

Some collection requests allow you to apply a filter to operations like getting or deleting entries. A basic filter will look something like this:

```json
{
	"filter": {
		"name": "Alice",
		"age": 21
	}
}
```

Here, we search for every item that has a `name` of ‘Alice’ AND an `age` of 21.

## $in

Using `$in` allows you to specify multiple values to filter on. For example:

```json
{
	"filter": {
		"_id": {
			"$in": ["abc", "def"]
		}
	}
}
```

This will delete entries with either ‘abc’ or ‘def’ as its id.