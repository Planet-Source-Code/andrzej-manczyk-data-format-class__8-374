﻿<div align="center">

## Data format class


</div>

### Description

Data format class makes HTML tables from specified SQL query (MS SQL, MySQL or PostgreSQL). If you need simply table you must specified only connection parameters, databases name, query statement, and you receive it. If you need more possibility you can format table head and footer, every row, column, cells, even and odd number rows. In addition this class offer sum, count, average, minimum and maximum value which you gets in footer part of the table. If you need use more options, you can define your own template and use it any time with different data. I enclose 2 example templates (you can use them as methods, they setting some options)
 
### More Info
 
HTML table


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Andrzej Manczyk](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/andrzej-manczyk.md)
**Level**          |Beginner
**User Rating**    |5.0 (15 globes from 3 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Controls/ Forms/ Dialogs/ Menus](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/controls-forms-dialogs-menus__8-3.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/andrzej-manczyk-data-format-class__8-374/archive/master.zip)

### API Declarations

DataBases.inc - Easy database use class - it is my own class you can find it among my other projects on this Web Site


### Source Code

```
<?PHP
// MAIN PART
//
// File: dataformat.inc
// Author: Andrzej Manczyk
// email: amanczyk@poczta.onet.pl
// Project: Data format class
// Version: 1.0.0
// PHP: 4.04
//
// Methods list:
// - CreateTable: creation HTML table
//   (you must first set options or use templates)
// Templates list
// - LightGreyTemplate
// - DesertTemplate
// Returns:
// HTML table
//
// DataBases.inc - Easy database use class - it is my own class
// you can find it among my other projects on this Web Site
Require "Databases.inc";
//
Class DataFormat
{
// -------------------------------------------------------------------
// database variables: see "Databases.inc" class
// databases type: mssql, mysql, pg
var $dbtype;
// connection type: c - common connection, p - open persistent connection
var $connecttype;
// - connect: for MS SQL Server - server name,
//  for MySQL - hostname [:port] [:/path/to/socket] ,
//  for PostgreSQL - host, port, tty, options,
//    dbname (without username and password)
var $connect;
var $username;
var $password;
// database name
var $dbname;
//SQL query statement
var $query;
// -------------------------------------------------------------------
// field type variables
// auto aligns your data: texts to left, numbers to right
// and date, datetime and bit data to center
var $withdatatypeformat; // True, False (default)
// inside variable, not changeable
var $arrayfieldtype;
// table variables
// -------------------------------------------------------------------
// table TAG
// a summary of what type of data the table contains
var $summary;
// specifies the position of the table with respect to the document
var $align; // left, right, center, "" (default)
// specifies the desired width of the entire table and
// is intended for visual user agents
var $width;
// specifies the width (in pixels only) of the frame around a table
var $border;
// affect a table's external frame and internal rulestable frame
var $frame; // void, above, below, hsides, lhs, rhs ,vsides, box, border
// table
var $rules;
// specifies how much space the user agent should leave between
// the left side of the table and the left-hand side of the leftmost column,
// the top of the table and the top side of the topmost row,
// and so on for the right and bottom of the table
var $cellspacing;
// specifies the amount of space between the border of the cell and its contents
var $cellpadding;
// sets the background color
var $bgcolor;
// sets the border color
var $bordercolor;
// other table options, you can define more options inside Table tag
var $tableoption;
// caption TAG
var $caption;
// -------------------------------------------------------------------
// head TAG
// adds table head, rest head options work if you set $withhaed = True
var $withhaed; // True, False (default)
// sets array with column titles (you must specified the same number
// of elements as columns in your data) or if you sets $withrownumber = True,
// one more else)
var $coltitle;
// other head options, you can define more options for each head elements
// (inside <TH>)
var $headoption;
// other head options, you can define more options for each head elements
// (outside <TH>)
var $headoptionmore;
// other head options, you can define more options as array for each head elements
var $arrayheadoption;
// -------------------------------------------------------------------
// row TAG;
// options for each rows (inside <TD>)
var $rowoption;
// options for each rows (outside <TD>)
var $rowoptionmore;
// array options for each rows
var $arrayrowoption;
// -------------------------------------------------------------------
// special rows options for odd and even number of rows
var $oddrowoption;
var $evenrowoption;
// -------------------------------------------------------------------
// special columns options
var $arraycolumnoption;
// -------------------------------------------------------------------
// footer TAG
// adds table footer, rest footer options work if you set $withfooter = True
var $withfooter; // True, False (default)
// sets type of data in footer section (you must specified the same number
// of elements as columns in your data)
var $arrayfootertype; // type: none, avg, sum, min, max, count, text
// sets text in footer section (you must specified the same number
// of elements as columns in your data)
var $arrayfootertext; // aditional text
// other footer options, you can define more options for
// each footer elements (inside (TD>)
var $footeroption;
// other footer options, you can define more options for
// each footer elements (outside (TD>)
var $footeroptionmore;
// rest - inside variables, not changeable
var $arraysum;
var $arraycount;
var $arraymin;
var $arraymax;
// -------------------------------------------------------------------
// additional column with row number
// auto makes column with current rows (records) number,
// rest options work if you set $withfooter = True
var $withrownumber; // True (default), False
// other column number options
var $rownumberoption;
// sets text in footer section
var $rownumberfootertext;
var $rownumberfootersum;
Function DataFormat()
{
 // database variables
 $this->dbtype = "pg";
 $this->connecttype = "c";
 $this->connect = "";
 $this->username = "";
 $this->password = "";
 $this->dbname = "";
 $this->query = "";
 // field type
 $this->withdatatypeformat = True;
 $this->arrayfieldtype = "";
 // table variables
 $this->summary = "";
 $this->align = ""; //left, center, right
 $this->width = "";
 $this->border = "";
 $this->frame = ""; //void, above, below, hsides ,vsides, lhs ,rhs ,box, border
 $this->rules = ""; //none, groups, rows, cols, all
 $this->bgcolor = "";
 $this->bordercolor = "";
 $this->cellspacing = "";
 $this->cellpadding = "";
 $this->tableoption = "";
 $this->caption = "";
 $this->withhead = False;
 $this->headoption = "";
 $this->headoptionmore = "";
 $this->rowoption = "";
 $this->rowoptionmore = "";
 $this->oddrowoption = "";
 $this->evenrowoption = "";
 $this->arraycolumnoption = "";
 $this->withfooter = False;
 $this->arrayfootertype = "";
 $this->arrayfootertext = "";
 $this->footeroption = "";
 $this->footeroptionmore = "";
 $this->arraysum = "";
 $this->arraycount = "";
 $this->arraymin = "";
 $this->arraymax = "";
 $this->withrownumber = True;
 $this->rownumberoption = "";
 $this->rownumberfootertext = "";
 $this->rownumberfootercount = False;
}
Function CreateTable()
{
 // this part use another class: see "DataBases.inc"
 $dbObj = New mDatabase;
 $c = $dbObj->Open($this->dbtype, $this->connecttype, $this->connect,
 $this->username, $this->password);
 If ($this->dbtype != "pg") $dbObj->SelectDB($this->dbname);
 $r = $dbObj->Query($this->query);
 $FieldsNumber = $dbObj->FieldsNumber($r);
 // table section
 $t = "<TABLE";
 If ($this->summary != "" ) $t = "$t SUMMARY=\"$this->summary\"";
 If ($this->align != "" ) $t = "$t ALIGN=\"$this->align\"";
 If ($this->width) $t = "$t WIDTH=\"$this->width\"";
 If ($this->border != "" ) $t = "$t BORDER=\"$this->border\"";
 If ($this->frame != "" ) $t = "$t FRAME=\"$this->frame\"";
 If ($this->rules != "" ) $t = "$t RULES=\"$this->rules\"";
 If ($this->bgcolor != "" ) $t = "$t BGCOLOR=\"$this->bgcolor\"";
 If ($this->bordercolor) $t = "$t BORDERCOLOR=\"$this->bordercolor\"";
 If ($this->cellspacing != "" ) $t = "$t CELLSPACING=\"$this->cellspacing\"";
 If ($this->cellpadding != "" ) $t = "$t CELLPADDING=\"$this->cellpadding\"";
 If ($this->tableoption != "" ) $t = "$t $this->tableoption";
 $t = "$t>\n";
 // caption section
 If ($this->caption != "") $t = "$t <CAPTION>$this->caption</CAPTION>\n";
 // head section
 If ($this->withhead) {
 $t = "$t <HEAD>\n";
 $t = "$t <TR>\n";
 If (Is_Array($this->coltitle)) {
 $ae = Count($this->coltitle);
 For ($col = 0; $col < $ae; $col++) {
 $t = "$t <TH";
 If ($this->headoption != "" ) $t = "$t $this->headoption";
 If (Is_Array($this->arrayheadoption) And Count($this->arrayheadoption) > $col) {
 $t = $t . " " . $this->arrayheadoption[$col];
 }
 $t = $t . ">";
 If ($this->headoptionmore != "" ) $t = $t . $this->headoptionmore;
 $t = $t . $this->coltitle[$col] . "</TH>\n";
 }
 }
 $t = "$t </TR>\n";
 $t = "$t </HEAD>\n";
 }
 // rows section
 $d = $dbObj->MoveFirstRec($r);
 $recNumber = 1;
 While ($d) {
 // check if row has even or odd row
 If ($recNumber % 2) {
 $evenrow = False;
 } Else {
 $evenrow = True;
 }
 // additional column for rows number
 $t = "$t <TR>\n";
 If ($this->withrownumber) {
 $t = "$t <TD";
 // row number option
 If ($this->rownumberoption != "" ) $t = "$t $this->rownumberoption";
 // even and odd row opion
 If ($evenrow And $this->evenrowoption != "") $t = "$t $this->evenrowoption";
 If (!$evenrow And $this->oddrowoption != "") $t = "$t $this->oddrowoption";
 // row number
 $t = "$t>";
 If ($this->rowoptionmore != "" ) $t = $t . $this->rowoptionmore;
 $t = $t . "$recNumber</TD>\n";
 }
 // columns
 For ($col = 0; $col < $FieldsNumber; $col++) {
 $t = "$t <TD";
 // row option
 If ($this->rowoption != "" ) $t = "$t $this->rowoption";
 // even and odd row opion
 If ($evenrow And $this->evenrowoption != "") $t = "$t $this->evenrowoption";
 If (!$evenrow And $this->oddrowoption != "") $t = "$t $this->oddrowoption";
 // column option
 If (Is_Array($this->arraycolumnoption) And Count($this->arraycolumnoption[$col])) {
 $t = $t . " " . $this->arraycolumnoption[$col];
 }
 // data
 // get field type
 If ($this->withdatatypeformat) {
 If ($recNumber == 1) {
 $fieldtype = $dbObj->FieldType($r, $col);
 $this->arrayfieldtype[$col] = $this->MakeDateTypeFormat($fieldtype);
 }
 $t = $t . " " . $this->arrayfieldtype[$col];
 }
 // if field is empty put space
 If (is_null($d[$col]) Or ($d[$col] == "")) $d[$col] = " ";
 If ($this->withfooter) {
 $this->arraycount[$col]++;
 If ($recNumber == 1) {
 $this->arraysum[$col] = 0;
 $this->arraymin[$col] = $d[$col];
 $this->arraymax[$col] = $d[$col];
 }
 $this->arraysum[$col] = $this->arraysum[$col] + $d[$col];
 If ($d[$col] < $this->arraymin[$col]) $this->arraymin[$col] = $d[$col];
 If ($d[$col] > $this->arraymax[$col]) $this->arraymax[$col] = $d[$col];
 }
 $t = "$t>";
 If ($this->rowoptionmore != "" ) $t = $t . $this->rowoptionmore;
 $t = $t . "$d[$col]</TD>\n";
 }
 $d = $dbObj->MoveNextRec($r);
 $t = "$t </TR>\n";
 $recNumber++;
 }
 // footer section
 If ($this->withfooter) {
 $t = "$t </FOOT>\n";
 $t = "$t <TR>\n";
 // with rows number section
 If ($this->withrownumber) {
 $t = "$t <TD";
 If ($this->rownumberfooteroption != "") $t = "$t $this->rownumberfooteroption";
 If ($this->footeroption != "") $t = "$t $this->footeroption";
 $t = "$t>";
 If ($this->footeroptionmore != "" ) $t = $t . $this->footeroptionmore;
 If ($this->rownumberfootertext == "" And !$this->rownumberfootercount) {
 $t = "$t ";
 } Else {
 $t = $t . $this->rownumberfootertext;
 If ($this->rownumberfootercount) $t = $t . " " .$this->arraycount[0];
 }
 $t = "$t </TD>\n";
 }
 If (Is_Array($this->arrayfootertype)) {
 $fc = Count($this->arrayfootertype);
 If ($FieldsNumber != $fc) {
 Echo "Incorect footer arguments number. You need $FieldsNumber arg.";
 } Else {
 For ($col = 0; $col < $fc; $col++) {
 // additional footer text
 If (Is_Array($this->arrayfootertext)) {
 $ftc = Count($this->arrayfootertext);
 If ($col > $ftc) {
 $ftxt = "";
 } Else {
 $ftxt = $this->arrayfootertext[$col];
 }
 }
 $t = "$t <TD";
 If ($this->footeroption != "") $t = "$t $this->footeroption";
 $t = "$t>";
 If ($this->footeroptionmore != "" ) $t = $t . $this->footeroptionmore;
 Switch ($this->arrayfootertype[$col]) {
 Case "avg":
 $t = $t . $ftxt . $this->arraysum[$col] / $this->arraycount[$col] . "</TD>\n";
 Break;
 Case "sum":
 $t = $t . $ftxt . $this->arraysum[$col] . "</TD>\n";
 Break;
 Case "count":
 $t = $t . $ftxt . $this->arraycount[$col] . "</TD>\n";
 Break;
 Case "min":
 $t = $t . $ftxt . $this->arraymin[$col] . "</TD>\n";
 Break;
 Case "max":
 $t = $t . $ftxt . $this->arraymax[$col] . "</TD>\n";
 Break;
 Case "text":
 If ($ftxt == "") $ftxt = " ";
 $t = $t . $ftxt . "</TD>\n";
 Break;
 Default:
 $t = $t . " </TD>\n";
 Break;
 }
 }
 }
 }
 $t = "$t </TR>\n";
 $t = "$t </FOOT>\n";
 }
 // end table
 $t = "$t</TABLE>";
 Return $t;
}
Function MakeDateTypeFormat($type)
{
 If ($type == "double" Or $type == "integer" Or $type == "int" Or $type == "long"
 Or $type == "numeric" Or $type == "float" Or $type == "real" Or $type == "money") {
 $format = "ALIGN=\"right\"";
 } Else {
 If ($type == "bool" Or $type == "bit" Or $type == "datetime" Or $type == "date") {
 $format = "ALIGN=\"center\"";
 } Else {
 $format = "ALIGN=\"left\"";
 }
 }
 Return $format;
}
// -------------------------------------------------------------------
// TEMPLATE PART
// NOTICE: If you make interesting template and you are willing
// shares it, send me and I add it to my class.
// -------------------------------------------------------------------
Function LightGreyTemplate()
{
 // format data with special way
 $this->withdatatypeformat = True;
 // show table head
 $this->withhead = True;
 // sets "Gainsboro" color for head background
 $this->headoption = "BGCOLOR=\"#DCDCDC\"";
 // sets "Whitesmoke" color for odd rows background
 $this->oddrowoption = "BGCOLOR=\"#F5F5F5\"";
 // show table footer
 $this->withfooter = True;
 // sets "Lightgrey" color for footer background and join all footer cells
 $this->footeroption = "BGCOLOR=\"#D3D3D3\" COLSPAN=\"10\"";
 // make number of records in the footer
 $this->rownumberfootercount = True;
 $this->rownumberfootertext = "Records:";
}
Function DesertTemplate()
{
 // row number column format
 $this->withrownumber = True;
 $this->rownumberoption = "ALIGN=\"right\"";
 // format data with special way
 $this->withdatatypeformat = True;
 // sets "Lightyellow" color for border color
 $this->bordercolor = "#FFFFE0";
 // show table head
 $this->withhead = True;
 // sets "Burlywood" color for head background
 $this->headoption = "BGCOLOR=\"#DEB887\"";
 // sets "Maroon" color for head fonts
 $this->headoptionmore = "<FONT COLOR=\"#800000\" BOLD=\"1\">";
 // sets "Maroon" color for row fonts
 $this->rowoptionmore="<FONT COLOR=\"#800000\">";
 // sets "Lemonchiffon" color for odd rows background
 $this->oddrowoption = "BGCOLOR=\"#FFEACD\"";
 // sets "Lightgoldenrodyellow" color for even rows background
 $this->evenrowoption = "BGCOLOR=\"#FAFAD2\"";
 // show table footer
 $this->withfooter = True;
 // sets "Burlywood" color for footer background and join all footer cells
 $this->footeroption = "BGCOLOR=\"#DEB887\" ALIGN=\"left\"";
 $this->footeroptionmore = "<FONT COLOR=\"#A0522D\">";
 // make number of records in the footer
 $this->rownumberfooteroption = "COLSPAN=\"10\"";
 $this->rownumberfootercount = True;
 $this->rownumberfootertext = "Records:";
}
}
?>
//
//
//
//
//
//
//
<?PHP
 // example how to use this class
 Require "dataformat.inc";
 $f = New DataFormat;
 $f->dbtype = "pg";
 $f->connect = "host=127.1.1.0 port=5432 dbname=example";
 $f->username = "postgres";
 $f->password = "********";
 $f->query = "SELECT * FROM data1";
 $f->arrayheadoption = Array("WIDTH=\"20\"","WIDTH=\"100\"","WIDTH=\"80\"","WIDTH=\"80\"");
 $f->coltitle = Array("Nr", "Date", "Name", "Value");
 $f->arrayfootertype = Array("none", "none", "min");
 $f->arrayfootertext = Array("","","Min: ");
 $f->DesertTemplate();
 $tbl = $f->CreateTable();
 echo $tbl;
?>
```

