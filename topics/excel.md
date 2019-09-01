# Insert pictures from URL

This code gets images from URLs and inserts them in the column to the right of the URL.

1. Have a column with the image URLs, usually in range `A:A`. Leave an empty column on the right i.e. `B:B`.
2. Open the Developer/Visual Basic tab.
3. Paste the below code in a new module. (Insert/Module)
4. Edit the `Rng` value in the code i.e. the range with image URLs ex. `A1:A100`.
5. Click run and wait for the images to be populated in the adjacent column i.e. column `B:B`.

It takes around 2 seconds per image.

```vbnet 
Sub URLPictureInsert()
    Dim xCol As Long
    Dim xRg As Range
    Dim w As Integer
    Dim h As Integer
    On Error Resume Next
    Set Rng = ActiveSheet.Range("A1:A100")
    w = 100
    h = 100
    For Each cell In Rng
        xCol = cell.Column + 1
        Set xRg = Cells(cell.Row, xCol)
        xRg.ColumnWidth = 20
        xRg.RowHeight = 100
        ActiveSheet.Shapes.AddPicture Filename:=cell, LinkToFile:=msoFalse, SaveWithDocument:=msoCTrue, Left:=xRg.Left + (xRg.Width - w) / 2, Top:=xRg.Top + (xRg.Height - h) / 2, Width:=w, Height:=h
        Next
End Sub
```

# Google Sheets Cell Update Timestamp

1. Modify code with corresponding sheet, cells and update cells.
2. Go to Tools/Script Editor.
3. Paste the code and click save. 

```javascript
function onEdit(e) {
  var sh = e.source.getActiveSheet();
  var sheets = ['Sheet1']; // Which sheets to run the code.

  // Columns with the data to be tracked. 1 = A, 2 = B...
  var ind = [1, 2, 3].indexOf(e.range.columnStart); 

  // Which columns to have the timestamp, related to the data cells.
  // Data in 1 (A) will have the timestamp in 4 (D)
  var stampCols = [4, 5, 6]
  
  if(sheets.indexOf(sh.getName()) == -1 || ind == -1) return;

  // Insert/Update the timestamp.
  var timestampCell = sh.getRange(e.range.rowStart, stampCols[ind]);
  timestampCell.setValue(typeof e.value == 'object' ? null : new Date());
}
```