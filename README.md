xml4js
------
#### JavaScript XML Writer Library
The aim of the project is to create a lightweight library that enables the user to create XML documents with a low level complexity.

### Usage
You must include the library into your web application, so simply add the Javascript file into your HTML.

```html
<script type="text/javascript" src="xml4js.js"></script>
```
 
After that, you should initialize your writer object on your JavaScript Code.
```javascript
var xml = new xml4js(options);
```
##### Options
 - **declaration:** _true_ or _false_ - if you desired to add (or not) the declaration line to your XML document (version, encoding, ...);
 - **encoding:** _true_ or _false_ -  specification of the desired encoding if need one different from the default (UTF-8);
 - **tabs:** _true_ or _false_ - chooses between _tabs_ or _spaces_ on indentation.

The only method available for writing purposes, is called `write` and you can use through the following signatures:
```javascript
xml.write(name, attributes, value);
xml.write(name, attributes, callback);
```

To write an element (with/without attributes):
```javascript
xml.write("movie");
xml.write("movie", {category: "thriller"});
xml.write("movie", "The Lord of Rings: The Fellowship of the Ring");
xml.write("movie", {category: "fantasy"}, "The Lord of Rings: The Two Towers");
```

To write elements nested in other elements:
```javascript
xml.write("movie", function() {
    xml.write("title", "Titanic");
    xml.write("year", "1997");
});
// OR
xml.write("movie", {category: "Drama"}, function() {
    xml.write("title", "Titanic");
    xml.write("year", "1997");
});
 ```

To write _unparsed data_, you need to create a special element called **CDATA**:
```javascript
xml.write("cdata", "getAllMovies();");
 ```
To print/extract XML document, at any moment, invoke `toString()` method.

### Demo
Example:
```javascript
var xml = new xml4js({encoding: "ISO-8859-1"});
xml.write("bookstore", function() {
    xml.write("book");
    xml.write("book", {category: "thriller"});
    xml.write("book", "someValue");
    xml.write("book", {category: "cooking"}, function() {
        xml.write("title", {lang: "en"}, "Every Italian");
        xml.write("author",  "Giada de Laurentilis");
        xml.write("year", "2005");
    });
    xml.write("book", {category: "children"}, function() {
        xml.write("title", {lang: "en"}, "Harry Potter");
        xml.write("author",  "J. K. Rowling");
        xml.write("year", "2005");
    });
    xml.write("book", function() {
        xml.write("title", "Fear");
        xml.write("author", "Jeff Abbott");
        xml.write("year", "2006");
    });
    xml.write("data", {elem: "cdata"}, function() {
        xml.write("cdata", "getAllBooks()");
    });
});
var doc = xml.toString();
```
Output:
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<bookstore>
    <book />
    <book category="thriller" />
    <book>someValue</book>
    <book category="cooking">
        <title lang="en">Every Italian</title>
        <author>Giada de Laurentilis</author>
        <year>2005</year>
    </book>
    <book category="children">
        <title lang="en">Harry Potter</title>
        <author>J. K. Rowling</author>
        <year>2005</year>
    </book>
    <book>
        <title>Fear</title>
        <author>Jeff Abbott</author>
        <year>2006</year>
    </book>
    <data elem="cdata">
        <![CDATA[getAllBooks()]]>
    </data>
</bookstore>
```

## License
Copyright © 2013 Paulo Oliveira

Licensed under the MIT license.