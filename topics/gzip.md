# gzip

This package creates a `.gz` compressed file, much like `.rar` or `.zip`.  

## Compression
`gzip <file>` - Compress a file. *Deletes original file*.  
`gzip -k <file>` - Compress without deletion.  

`gzip -9 <file>` - Compression quality 1-9. *Default is 5*.


## Extraction
`gzip -d <file>` or `gunzip <file>`- Extract.  
