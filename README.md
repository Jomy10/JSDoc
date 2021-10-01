NOTE: this is still under construction. Check back later to see the full JSDocFile format article.
# The JSDoc file format
The JSDoc file format is a simple way to make, edit and share documents between different applications.

It was developped for the app *'t Voetje* in order to share documents between different platformss and easily read, edit and export them on these different platforms. Afterwards, users can export these documents to Word or PDF.

Contributions are very welcome here!

## Table of contents
- [File format](#file-format)
	- [document.xml](#Detailed-explanation-of-*document.xml*)
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

#### Reading, writing and editing
##### JSDocReader
##### JSDocFileConstructor
##### JSDocWriter

#### Storing elements

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
