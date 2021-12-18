# SpreadSheet

## Introduction:
A spreadsheet is an application for organization, analysis, and storage of data in tabular form. The first part of the project will focus on the core features and skeleton of the application . In part two we’ll take care of text-formatting ( like copy, delete, select  ). And in part three, we’ll add some useful extensions like a find and go cell dialog.

**This is what it’ll look like at the end of this project**

![image](https://user-images.githubusercontent.com/75392302/146654959-2cd2a571-2b7d-4541-9a8c-defcb5f1ff1e.png)

Before we get started, we need to add resources files for icons.

### Resource Files
The text editor uses several icons to represent various actions. The icons are in the resources directory which is directly under the TextEditor project directory.

To register the image files, in the Edit mode, right-click the icons.qrc file and select Open in Editor.

Click the Add button and select Add Files.

In the file manager, select the files to be added.

## Part One:

 the SpreadSheet Application is in the MainWindow class, which inherits QMainWindow. QMainWindow provides the framework for windows that have menus, toolbars, dock windows, and a status bar. The application provides File, Edit, Tools, Options, and Help entries in the menu bar, with the following Actions:
 
 ![image](https://user-images.githubusercontent.com/75392302/146655230-ac274206-414c-4c58-b7f4-518acc59c4b2.png)



![Screenshot (14)](https://user-images.githubusercontent.com/75392302/146655196-b34b9e5b-1f3e-4334-9336-82d4878420af.png)
![Screenshot (15)](https://user-images.githubusercontent.com/75392302/146655239-78f7e04e-5abd-4fca-80ed-840416b2b0cd.png)
