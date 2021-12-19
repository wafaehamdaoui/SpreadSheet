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

 the SpreadSheet Application is in the MainWindow class, which inherits QMainWindow. QMainWindow provides the framework for windows that have menus, toolbars, dock windows, and a status bar. 
 
 The application provides File, Edit, Tools, Options, and Help entries in the menu bar, with the following Actions:
 
 ![image](https://user-images.githubusercontent.com/75392302/146655230-ac274206-414c-4c58-b7f4-518acc59c4b2.png)
 ![image](https://user-images.githubusercontent.com/75392302/146655254-3de283a3-9e3c-49d3-a896-15ef6f87b0f4.png)
 ![image](https://user-images.githubusercontent.com/75392302/146655290-d5113004-fc74-4a92-94d2-a202320980a1.png)
 ![image](https://user-images.githubusercontent.com/75392302/146655326-16892a1f-fac2-4db0-87e3-8fb16ec8d7e7.png)
 ![image](https://user-images.githubusercontent.com/75392302/146655340-97e8755a-1587-48ec-b67c-5d7c88e45187.png)

### Actions:
The createActions() private function, which is called from the MainWindow constructor, creates QActions .
```cpp
void SpreadSheet::createActions()
{
    // New File
   QPixmap newIcon(":/New.bmp");
   newFile = new QAction(newIcon, "&New", this);
   newFile->setShortcut(tr("Ctrl+N"));


    // open file
   QPixmap openIcon(":/Open.bmp");
   open = new QAction(openIcon,"&Open", this);
   open->setShortcut(tr("Ctrl+O"));

   // open file
  QPixmap csvIcon(":/csv.jfif");
  opencsv = new QAction(csvIcon,"&Open csv", this);
  opencsv->setShortcut(tr("Ctrl+O+C"));

    //  save file
   QPixmap saveIcon(":/Save.bmp");
   save = new QAction(saveIcon,"&Save", this);
   save->setShortcut(tr("Ctrl+S"));

    //  open file
   saveAs = new QAction("save &As", this);


    // open file
   QPixmap cutIcon(":/Cut.bmp");
   cut = new QAction(newIcon, "Cu&t", this);
   cut->setShortcut(tr("Ctrl+X"));

   // Cut menu
   QPixmap copyIcon(":/Copy.bmp");
   copy = new QAction(copyIcon,"&Copy", this);
   copy->setShortcut(tr("Ctrl+C"));

   QPixmap pasteIcon(":/Paste.bmp");
   paste = new QAction(pasteIcon,"&Paste", this);
   paste->setShortcut(tr("Ctrl+V"));

   QPixmap deleteIcon(":/DeleteSticky.bmp");
   deleteAction = new QAction(deleteIcon,"&Delete", this);
   deleteAction->setShortcut(tr("Ctrl+D"));


   row  = new QAction("&Row", this);
   Column = new QAction("&Column", this);
   all = new QAction("&All", this);
   all->setShortcut(tr("Ctrl+A"));



   QPixmap findIcon(":/search_icon.png");
   find= new QAction(findIcon, "&Find", this);
   find->setShortcut(tr("Ctrl+F"));

   QPixmap goCellIcon(":/go_to_icon.png");
   goCell = new QAction( goCellIcon, "&Go to Cell", this);
   deleteAction->setShortcut(tr("f5"));


   recalculate = new QAction("&Recalculate",this);
   recalculate->setShortcut(tr("F9"));


   sort = new QAction("&Sort");



   showGrid = new QAction("&Show Grid");
   showGrid->setCheckable(true);
   showGrid->setChecked(spreadsheet->showGrid());

   auto_recalculate = new QAction("&Auto-recalculate");
   auto_recalculate->setCheckable(true);
   auto_recalculate->setChecked(true);

   //recent files
   for (int i = 0; i < MaxRecentFiles; ++i){
       if (i < recentFiles.count()){
       recentFileActs[i]= new QAction;}
   }

   about =  new QAction("&About");
   aboutQt = new QAction("About &Qt");

    // exit
   QPixmap exitIcon(":/quit_icon.png");
   exit = new QAction(exitIcon,"E&xit", this);
   exit->setShortcut(tr("Ctrl+Q"));
}


void SpreadSheet::makeConnexions()
{
    //Connection for new file action
    connect(newFile, &QAction::triggered,
            this, &SpreadSheet::NewFile);


   //  Connexion for the  select all/row/column action
   connect(all, &QAction::triggered,
           spreadsheet, &QTableWidget::selectAll);

   connect(row, &QAction::triggered,
           spreadsheet, &QTableWidget::selectRow);

   connect(Column, &QAction::triggered,
           spreadsheet, &QTableWidget::selectColumn);




   // Connection for the  show grid
   connect(showGrid, &QAction::triggered,
           spreadsheet, &QTableWidget::setShowGrid);

   //Connection for the exit button
   connect(exit, &QAction::triggered, this, &SpreadSheet::close);


   //connectting the chane of any element in the spreadsheet with the update status bar
   connect(spreadsheet, &QTableWidget::cellClicked, this,  &SpreadSheet::updateStatusBar);

   // Connection for about
   connect(about, &QAction::triggered, this,&SpreadSheet::about_me);

   // Connection for the  aboutQt
   connect(aboutQt, &QAction::triggered, this,&SpreadSheet::about_qt);

   //Connextion between the gocell action and the gocell slot
   connect(goCell, &QAction::triggered, this, &SpreadSheet::goCellSlot);

   //Connextion between the find action and the find_funtion slot
   connect(find, &QAction::triggered, this, &SpreadSheet::find_function);

   //Connexion for the saveFile
      connect(save, &QAction::triggered, this, &SpreadSheet::saveSlot);


      //Connexion for the openCsvFile
         connect(opencsv, &QAction::triggered, this, &SpreadSheet::OpenCsvFile);

         //Connexion for the open
                  connect(open, &QAction::triggered, this, &SpreadSheet::openFile);


         //connexion for recentFiles
         for (int i = 0; i < MaxRecentFiles; ++i) {
             if (i < recentFiles.count()){
                connect(recentFileActs[i], SIGNAL(triggered()),
                         this, SLOT(openRecentFile()));}
             }
//         connect(recentFile, &QAction::triggered, this, &SpreadSheet::RecentFiles);

         //connexion for copy action
         connect(copy, &QAction::triggered, this, &SpreadSheet::Copy);

         //connexion for paste action
         connect(paste, &QAction::triggered, this, &SpreadSheet::Paste);

         //connexion for delete action
         connect(deleteAction, &QAction::triggered, this, &SpreadSheet::Delete);

         //connexion for cut action
         connect(cut, &QAction::triggered, this, &SpreadSheet::Cut);
}

```
As we might have noticed, all the actions we’ll be creating for our text editor follow the same code pattern:

Create a QAction and pass it an icon and a name

Create a status tip, which will display a message in the status bar .

Create a shortcut.

Connect the QAction’s triggered signal to a slot function.

Then we add those actions to the appropriate menu.
```cpp
void SpreadSheet::createMenus()
{
    //  File menu
    FileMenu = menuBar()->addMenu("&File");
    FileMenu->addAction(newFile);
    FileMenu->addAction(open);
    FileMenu->addAction(opencsv);
    FileMenu->addAction(recentFile);
    RecentFile = FileMenu->addSeparator();
        for (int i = 0; i < MaxRecentFiles; ++i){
            FileMenu->addAction(recentFileActions[i]);
        }
    FileMenu->addAction(save);
    FileMenu->addAction(saveAs);
    FileMenu->addAction(exit);

    RecentFile = FileMenu->addSeparator();
    for (int i = 0; i < MaxRecentFiles; ++i){
        if (i < recentFiles.count()){
        FileMenu->addAction(recentFileActs[i]);}
    }
    updateRecentFile();

    // Edit menu
    editMenu = menuBar()->addMenu("&Edit");
    editMenu->addAction(cut);
    editMenu->addAction(copy);
    editMenu->addAction(paste);
    editMenu->addAction(deleteAction);
    editMenu->addSeparator();
    auto select = editMenu->addMenu("&Select");
    select->addAction(row);
    select->addAction(Column);
    select->addAction(all);

    editMenu->addAction(find);
    editMenu->addAction(goCell);



    // Toosl menu
    toolsMenu = menuBar()->addMenu("&Tools");
    toolsMenu->addAction(recalculate);
    toolsMenu->addAction(sort);



    //Optins menus
    optionsMenu = menuBar()->addMenu("&Options");
    optionsMenu->addAction(showGrid);
    optionsMenu->addAction(auto_recalculate);


    // Help menu
    helpMenu = menuBar()->addMenu("&Help");
    helpMenu->addAction(about);
    helpMenu->addAction(aboutQt);
}
```
**ToolBar**

Creating toolbars is very similar to creating menus. The same actions that we put in the menus can be reused in the toolbars. After creating the action, we add it to the toolbar using QToolBar::addAction().
```cpp
void SpreadSheet::createToolBars()
{

    //Crer une bare d'outils
    auto toolbar1 = addToolBar("File");


    //Ajouter des actions acette bar
    toolbar1->addAction(newFile);
    toolbar1->addAction(save);
    toolbar1->addSeparator();
    toolbar1->addAction(exit);


    //Creer une autre tool bar
    auto toolbar2  = addToolBar("ToolS");
    toolbar2->addAction(goCell);
}
```
**StatusBar**

The status bar at the bottom of the main window shows a description of the menu item or toolbar button 

For our application, we will create two permanent QLabels to hold the current cell and current Formula.

```cpp
void creatStatusBar()
{
    //Creating the labels
    cellLocation = new QLabel("A45");
    cellFormula = new QLabel("B1 + C12");

    //Adding the labels to the current status bar
    statusBar()->addPermanentWidget(cellLocation);
    statusBar()->addPermanentWidget(cellFormula);

    //Just for use proposes: showing a temporary message
    statusBar()->showMessage("Opening a new file", 2000);
}
```

## Part two:

This part is about functions and slots implimentation 

***Close***
When the user attempts to close the window, we call the private function Close().
```cpp
void SpreadSheet::close()
{

    auto reply = QMessageBox::question(this, "Exit",
                                       "Do you really want to quit?");
    if(reply == QMessageBox::Yes)
        qApp->exit();
}
```
***NewFile***
When the user wants to create a new window, we call the private function Close()
```cpp
void SpreadSheet::NewFile()
{
    SpreadSheet *other = new SpreadSheet;
    other->show();
}
```
The NewFile() method is very easy, all it does is create a new instance of our window and call its show() method to display it.

***OpenFile***
```cpp
void SpreadSheet::openFile()
           {

                   QString fileName = QFileDialog::getOpenFileName(this, tr("Open Spreadsheet"),
                                                                   ".",
                                                                   tr("Spreadsheet files (*.sp)\n"
                                                                      "Comma-separated values files (*.csv)\n"
                                                                      "Lotus 1-2-3 files (*.wk1 *.wks)"));
                   if(!fileName.isEmpty()){
                       loadFile(fileName);
                   }

           }

```
 The open() slot is invoked when the user clicks File|Open. We pop up a QFileDialog asking the user to choose a file. If the user chooses a file , we call the private function loadFile() to actually load the file.
 
 ***Open CSV File***
 ```cpp
 void SpreadSheet::OpenCsvFile(){
    QString fileName = QFileDialog::getOpenFileName(this, ("Open File"));
    QString data;
    QFile file(fileName);


    QStringList rowOfData;
    QStringList columnData;
    data.clear();
    rowOfData.clear();
    columnData.clear();

    if (file.open(QFile::ReadOnly))
    {
        data = file.readAll();
        rowOfData = data.split("\n");           //Value on each row
        file.close();
    }

    for (int x = 0; x < rowOfData.size(); x++)  //rowOfData.size() gives the number of row
    {
        columnData = rowOfData.at(x).split(",");   //Number of collumn

//               int r=columnData.size();
        for (int y = 0; y < columnData.size()-2; y++)
//                {
            spreadsheet->setItem(columnData[y].toInt(),columnData[y+1].toInt(),new QTableWidgetItem(columnData[y+2]));

    }

   statusBar()->showMessage(tr("File loaded"), 2000);

}
 ```
 First we pop up a QFileDialog asking the user to choose a file. 
 
 If the user chooses a file we read this file using readAll() function .
 
 Then we store Value of each row in rowOfData list and Value of each column in columnData list.
 
 finally we show the contenent in SpreadSheet.
 
 ***Open Recent File**
 ```cpp
 void SpreadSheet::OpenRecentFiles()
{
    QAction *action = qobject_cast<QAction *>(sender());
    if (action)
        loadFile(action->data().toString());
}
 ```
 In openRecentFile() we first identify the QAction that called the slot and then load the corresponding file. The entire file path is stored in action->data(), the name of the file without the path is stored in action->text().
 
 And we need to update the list of reccentFiles time to time, we do that using updateRecentFile() function
 ```cpp
 void SpreadSheet::updateRecentFile()
{
    QSettings settings;
    QStringList files = settings.value("recentFileList").toStringList();

    int numRecentFiles = qMin(files.size(), (int)MaxRecentFiles);

    for (int i = 0; i < numRecentFiles; ++i) {
        QString text = tr("&%1 %2").arg(i + 1).arg(QFileInfo(files[i]).fileName());
        recentFileActs[i]->setText(text);
        recentFileActs[i]->setData(files[i]);
        recentFileActs[i]->setVisible(true);
    }
    for (int j = numRecentFiles; j < numRecentFiles; ++j)
        recentFileActs[j]->setVisible(false);

      RecentFile->setVisible(numRecentFiles > 0);
}
 ```
 
 ***Save***
 ```cpp
 void SpreadSheet::saveSlot()
    {
        //Creating a file dialog to choose a file graphically
        auto dialog = new QFileDialog(this);

        //Check if the current file has a name or not
        if(currentFile == "")
        {
           currentFile = dialog->getSaveFileName(this,"choose your file");

           //Update the window title with the file name
           setWindowTitle(currentFile);
        }

       //If we have a name simply save the content
       if( currentFile != "")
       {
               saveContent(currentFile);
       }
   }    
 ```
 Creating a file dialog to choose a file graphically.
 
 Check if the current file has a name or not.
 
 If we have a name simply save the content.
 
 If the user clicks Cancel, the returned file name is empty, and we do nothing.
 
 ***About***
 The application's About box is done using one statement, using the QMessageBox::about() static function 
 ```cpp
 void SpreadSheet::about_me(){
    QMessageBox::about(this, "about me","I'm trying to create a spreadSheet applicaton ");
}
 ```
 
 ***Copy***
 ```cpp
 void SpreadSheet::Copy(){
    Pastetxt = new QTableWidgetItem(spreadsheet->currentItem()->text());

 ```
 The copy() slot is invoked when the user clicks Edit|Copy.
 So we just store the contenent of the current cell in a variable that will called in paste slots.
 
 ***Paste***
 ```cpp
 void SpreadSheet::Paste(){
    i=spreadsheet->currentRow();
    j=spreadsheet->currentColumn();
    spreadsheet->setItem(i,j,Pastetxt);
}
 ```
 The Paste() slot is invoked when the user clicks Edit|Paste.
 So we just set a copied cell in the current cell .
 
 ***Delete**
 ```cpp
 void SpreadSheet::Delete(){
    i=spreadsheet->currentRow();
    j=spreadsheet->currentColumn();
    spreadsheet->item(i,j)->setText("");
}

 ```
 The Delete() slot is invoked when the user clicks Edit|Copy.
 So we just set an impty String in the current cell .
 
 ***Cut**
 ```cpp
 void SpreadSheet::Cut(){
    Copy();
    Delete();
}
 ```
 The Cut() slot is invoked when the user clicks Edit|Cut.
 So we just copy the contenent of the current cell and Then delete it .
 
 
 ## Part three:
 
 ### Go Cell
 
 This function point to an enter cell .
 
 First we Create a Form Class,
 
 Using the designer obtain the following the form:

![image](https://user-images.githubusercontent.com/75392302/146659155-7a88f2bf-1296-4247-8bf4-bc2c90ab9eb0.png)

Then we add the regular expression validator for the lineEdit:

```cpp
//Validating the regular expression
 QRegExp regCell{"[A-Z][1-9][0-9]{0,2}"};

 //Validating the regular expression
 ui->lineEdit->setValidator(new QRegExpValidator(regCell));

```

And we add a public Getter for the line edit Text to get the cell address:
```cpp
 QString GoCellDialog::cell() const
     {
         return ui->lineEdit->text();
     }
```

**Now we are setup to create the interesting connexion between the goCell action:**

First we will create the proper slot called goCellSlot to respond to the action trigger.

 ```cpp
 private slots:
 void goCellSlot();           
 ```
Then we connect the action to its proper slot in the makeConnexions function:
```cpp
connect(goCell, &QAction::triggered, this, &SpreadSheet::goCellSlot);
```

Here is the implementation of goCellSlot() function:

```cpp
 void SpreadSheet::goCellSlot()
 {
     //Creating the dialog
     GoCellDialog D;

     //Executing the dialog and storing the user response
     auto reply = D.exec();

     //Checking if the dialog is accepted
     if(reply == GoCellDialog::Accepted)
     {

         //Getting the cell text
         auto cell = D.cell();

         //letter distance
         int row = cell[0].toLatin1() - 'A';
         cell.remove(0,1);

         //second coordinate
         int col =  cell.toInt();


         //changing the current cell
         spreadsheet->setCurrentCell(row, col-1);
     }
 }
 ```
**Here is a simple test:**

![image](https://user-images.githubusercontent.com/75392302/146659282-e5b18f3c-ebfb-49e1-820d-e73ab6afd8eb.png)
![image](https://user-images.githubusercontent.com/75392302/146659292-8c6c4aa2-a873-4a14-a53b-47f5c84ea7e3.png)


### ♥Find Dialog




 
 

