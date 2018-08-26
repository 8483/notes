# Insert pictures

```vbnet 
Sub URLPictureInsert()
    Dim xCol As Long
    Dim xRg As Range
    Dim w As Integer
    Dim h As Integer
    On Error Resume Next
    Set Rng = ActiveSheet.Range("b1:b2")
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