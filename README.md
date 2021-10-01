NOTE: this is still under construction. Check back later to see the full JSDocFile format article.
# The JSDoc file format
The JSDoc file format (Jonas' Simple Document) is a simple way to make, edit and share documents between different applications.

It was developped for the app *'t Voetje* in order to share documents between different platformss and easily read, edit and export them on these different platforms. Afterwards, users can export these documents to Word or PDF.

Contributions are very welcome here!

## Table of contents
- [File format](#file-format)
	- [document.xml](#detailed-explanation-of-documentxml)
	- [resource folder](#resource-folder)
- [Libraries](#libraries)
	- [Installation](#installation)
	- [Library guidelines](#library-guidelines) (for developing your own library)
- [Uses](#uses)
- [Version notes](#version-notes)

## File format
The JSDoc file format is a zip file containing:

    > doc
      > document.xml
      > res (optional)
        > img.png (optional)
        > file.mp3 (optional)
So, all files are stored in a folder called *doc*. 
The only required document inside of this folder is *document.xml*, this contains the structure of the document. The xsd file for this xml format can be found at [my website](https://jonaseveraert.be/js-doc/jsdml-language/simpleDoc.xsd). This is not required to read the jsdoc format, but it can be useful when manually writing this xml file.

The doc folder is zipped. The extension of the resulting zip file will be replaced with *.jsdoc*.

### Detailed explanation of *document.xml*
#### Structure

    <simple-doc>
	    <head>
		    <simple-doc-version>2.0</simple-doc-version> (required)
		    <title>Sample Document</title> (required)
		    <author>Jonas Everaert</author>
		    <dateCreated>2021-09-30T16:34:00</dateCreated>
	    </head>
	    <document>
		    <text type="1">This is a headline of type 1</text>
		    <text type="2" color="red">This is a subheadline in a red color</text>
		    <text bold="true">This text is bold, but can also be both bold and *italic* using markdown support</text>
		    <text fontSize="24">This text is bigger</text>
			
			<text underline="true">This image will display img.png inside of the res folder</text
			<img name="img.png"/>

			<text italic="true">The implementation of "file" has not yet been added and is up to the person developping the libraries for now</text>
			<file name:"file.mp3" type="audio"/>
	    </document>
    <simple-doc/>
    
#### Structure explained
##### Head
The head part contains information about the document.
###### simple-doc-version
The version of JSDoc the document is using (latest: 2.0)
###### title
The title of the document
###### author (optional)
The author of the document
###### dateCreated (optional)
The date on which the document was created
##### document
This part contains the document structure.
###### text
This is a text object, inside of the tag you put the text that will be displayed. This can contain markdown 
- color
	- The color of the text
- fontSize
	- the text size, default is 12
- bold 
	- makes all text inside the text tag bold
- italic 
	- makes all text inside the text tag italic
- underline 
	- makes all text inside the text tag underline
- type
	- The type of the text (style of the text) (1 = title, 2 = subheadline, ...)
- disableMd (since 2.0)
	- Disables MarkDown support for the text inside of the tag

###### img
Displays an image from the  *res* folder.
- name
	- The name of the image inside of the res folder (e.g. *image.png*)

###### file
File has not yet been implemented officially. One way to implement this is when clicked it will open in the default application, or it will be displayed inside of the document.
- name
	- the name of the file inside of the res folder (e.g. *file.mp3*)
- type 
	- The file type (e.g. *audio*, or *audio/mp3*)

### Resource folder
Next to document.xml, there is also a folder called *res*. This is where all the resource files are stored. In our exampleaboe, the img named *img.png* would be in *doc > res > img.png*
        
## Libraries
The JSDoc library is available for both Java and Swift. I encourage everyone to develop their own JSDoc libraries for different languages, a detailed explanation on how these libraries are structured can be found under **Library  guidelines**.

### Installation
Detailed explanations can be found on the individual library's project pages.
- Swift
- Java

### Todo
#### JSDoc format

 - [ ] Add support for fonts:
	 - [ ] Font attribute for text
	 - [ ] Include non-system fonts in the resource folder

#### Java

 - [ ] Add Markdown support
 - [ ] Add dateCreated support

### Library guidelines
If you want to use the JSDoc format in other programming languages, you will have to implement it. If you do develop a library for JSDoc, please notify me so I can add your library to the list for others to use.

This guide is only suitable for Object Oriented Programming Languages as I do not have any experience with other programming languages. That being said, this guide might  still be useful for other programming languages.

Finally, if you think you could simplify this system, please let me know as well as I am open to feedback.

#### Objects
All of the JSDoc-tags are saved in JSDocObjects. Every JSDocObject should adopt a JSDocObject protocol.
This protocol (and thus each JSDocObject) should contain these variables: *attributes* (an array or list of [JSDocAttributes](#attributes), which are the attributes of the tag), *value* (the value inside of the tag, which is a(n optional) String and a *type* (to determine the object type).

Here is an example in *Swift*:

```
protocol JSDocObject {
    /// A list of the attributes of this JSDoc object
    var attributes: [JSDocAttribute] { get set }
    
    /// the value inside the tag.
    var value: String? { get set }
    
    /// Used to compare objects
    var type: JSDocObjectType { get }
}

enum JSDocObjectType {
    case text
    case img
    case file
    case author
    case title
    case date
}
```

#### Attributes
The attributes are part of the JSDocObjects. These are for example the fontSize inside, color or bold of Text.

```
<document>
	<text fontSize="24" color="black" bold="true">Attributes</text>
</document>
```

Programmatically, these have two variables: the attribute's *name* and the attribute's *value*. In our example: we would have a JSDocAttribute object with *name = "fontSize"* and *value = "24"*. 

To illustrate: here is an example in Swift (this is one way to implement this, but other way could also be done):
```
struct JSDocAttribute {
    enum JSDocAttributeName {...}
	
    let name: JSDocAttributeName
    var value: String?
    
    init(name: JSDocAttributeName, value: String) {
        self.name = name
        self.value = value
    }
    
    init(name: JSDocAttributeName) {
        self.name = name
        self.value = nil
    }
}
```
*I have left out any irrelevant information*

#### Reading, writing and editing
Now that we have our objects and attributes, we can start making and reading documents

##### JSDocReader
The JSDocReader class should be capable of reading the contents of a JSDoc file given a certain file path. In my Swift library, this class takes a url as input and has a function `func  readDocumentStruct() -> [JSDocObject]`.  
The *readDocumentStruct()* function will read the xml file and return a list or array of *JSDocObjects* that are in the *document.xml*. This includes elements in the *head* and *document* part. The only unimportant tag is the *simple-doc-version*, because this is only used to determine if your *JSDocReader* can handle the document (if the version is lower, it should first be converted to a newer version).

Lastly, this class should als contain a function/method to get a list of all the resource files, so that they can be used later.

##### JSDocFileConstructor
This class should contain an array or list of JSDocObjects, an array or list of the resources, the fileName and possibly some other methods. It should also contain a way to write this file, in the swift  example below, this is with the *writeFile* function.

Here is the structure of the *Swift* version:
```
class JSDocFileConstructor: Equatable {
    var objectsList: [JSDocObject]
    let res: [Entry]
    var fileName: String?
    
    init()
    
    init(objects: [JSDocObject])
    
    init(objects: [JSDocObject], resources: [Entry])
    
    func addObject(object: JSDocObject)
    
    func removeObject(index:  Int)
    
    func removeAllObjects(object: JSDocObject)
    
    func removeFirstObject(object: JSDocObject)
    
    static func == (lhs: JSDocFileConstructor, rhs: JSDocFileConstructor) -> Bool
    
    func writeFile(destDir: URL) throws {
        let writer = JSDocWriter(objects: self.objectsList, newDocName: fileName ?? nil)
        try writer.write(dir: destDir)
    }
}
```

##### JSDocWriter
Lastly, we need a way to write our JSDoc. This is done with the *JSDocWriter* class. This class should take in the list of *JSDocObject*s, the *documentName*, possibly the *author* if ther is one and the *date* if there is one. There is also a constant *simpleDocVersion* which is set to the latest version the library supports.

If there is no document name or author passed to this class, the class will search the list of *JSDocObject*s for these. If no document name has been found, it  will give it a name like "Untitled document".

Here is the structure of the *Swift* example:
```
class JSDocWriter {
    var objects: [JSDocObject]
    
    var documentName: String
    let simpleDocVersion = JSDocInfo.JSDocVersion
    var author: String?
    /// Don't forget to format YYYY-MM-DDThh-mm-ss
    var date: String?
    
    init(objects: [JSDocObject], newDocName docName: String?)
    
    init(objects: [JSDocObject])
    
    /// Writes the JSDoc file to the `savingDir`
    /// - Parameters
    ///   - savingDir: the directory where the JSDoc file should be saved to
    func write(dir savingDir: URL) throws
    
    /// Generates the xml for the write function
    private func generateXML() -> AEXMLDocument
```

#### Displaying the document
This is currently on the *todo* list. I welcome feedback on how to do this.

#### Others
##### JSDocInfo
This is just a file containing info about the library and JSDoc. In *Swift* it looks like this:
```
public class JSDocInfo {
    public static let JSDocVersion = "2.0"
    public static let JSDocVersion_Int = 20
    public static let JSDML_LINK = "https://www.jonaseveraert.be/js-doc/jsdml-language/simpleDocVersion1.xsd"
}
```
It just contains the latest version of JSDoc the library supports.

##### JSDocManager
This is something that is useful in my use case for *'t Voetje*. It is created at the start of the application and has two variables:  
- A list of JSDocFileConstructors
- An integer called *index*

The user can select between different *JSDocFileConstructor*s to use, the *index* indicatates the seleted file constructor.

This class isn't really required to be part of the library (it isn't in the Java version). It is just an extra feature that is useful in some cases.

## Uses
### 't Voetje
This is my main project. It is an app for a youth movement. JSDoc is used here to save encoded text the user has inputted. The user can then send the jsdoc to other users so they can continue working on it. When they are done, they can export it to Word or PDF.

You can download the app on [Google Play](https://play.google.com/store/apps/details?id=be.ksa.voetje) (Belgium only)

Or view it on [my website](https://voetje.jonaseveraert.be).

## Version Notes
#### 2.0 
* Markdown support using the *"enableMd"* attribute on text elements
* Added new head element: dateCreated to store the date the file was created
#### 1.1
* The folder in which JSDoc's files are stored is now a called "doc" to make it easier to read on different platforms
#### 1.0
* First version!
* Text, img, file elements
* ...
