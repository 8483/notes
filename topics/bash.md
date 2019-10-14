# Logical
`&&` - AND
`||` - OR

# Redirection
`>` - Redirect standard output and **overwrite**.  
`>>` - Redirect standard output and **append**.  
`<` - Redirect input to another command. Ex. `mail mike@somewhere.org < to_do.txt`

# Piping
`|` - Uses the **output** of one command as the **input** of another.

# Command Substitution
`$(PROGRAM_PATH)` - Use program output as variable. Ex. `echo "$(/bin/date) - Hi" >> logfile.log`

# grep
Only works for searching files or standard outputs.  

`grep <CRITERIA> <PATH>` - Returns all the lines containing the search criteria.

# File movement

```bash
# Navigate to D:
cd /mnt/d

# List all files WITH new line. Pasted in Excel gives one name per cell vertically.
ls -1 > ../pics.txt

# A && B  – Run B only if A succeeded
# One command from main folder
cd 1 && ls -1 > ../pics.txt && cd ../2 && ls -1 >> ../pics.txt && cd ..

# Wihtout .jpg extension.
cd 1 && ls -1 | tr -d ".jpg" > ../pics.txt && cd ../2 && ls -1 | tr -d ".jpg" >> ../pics.txt && cd ..

# Navigate to folder with files. List all files and replace new line with comma (,) and export one folder up into .csv file.
ls -1 | tr "\\n" "," > ../pics.csv

# Append (>>) files from other folder
ls -1 | tr "\\n" "," >> ../pics.csv

# List the different files from folder 1 vs folder 2
diff <(ls -1a folder1) <(ls -1a folder2)

# Check for different files not present in folder2 from folder1, and MOVE those in folder3
rsync -rcC --compare-dest=folder2/ folder1/ folder3/

# Find all the files with a space in the name
find ./ -name "* *"

# Find all the files with a space in the name AND move them
find ./ -name "* *" -exec mv -t ../folder {} +

# Find all the files with a dash - in the name AND move them
find ./ -name "*-*" -exec mv -t ../temp {} +

# Find all files with 4 characters
find ./ -name "????.jpg"

# Find all files with 4 characters AND move them
find ./ -name "????.jpg" -exec mv -t ../folder {} +

# Count number of files in folder
ls | wc -l

# Count number of files in folder with 6 characters.
find ./ -name "??????.jpg" | wc -l

# List just the names of the images (without the extension).
ls -1 | sed -e 's/\..*$//' > ../pics.txt
```

# Image manipulation
Requires the `imagemagick` package
```bash
sudo apt-get install imagemagick
```
### Resizing
```bash
# Resize image to specific dimensions, while trying to keep aspect ratio.
convert image.jpg -resize 492x492 resized_image.jpg

# Bulk resizing of images in a folder with conversion statuses, saved in a specified folder.
for file in *.jpg; do convert $file -resize 492x492 ../resized/$file && echo $file converted; done
```

### Resampling
```bash
sudo apt install imagemagick

# Reduce the image quality. Adjust the quality parameter 1-100.
convert image.jpg -sampling-factor 4:2:0 -strip -quality 1 -interlace JPEG -colorspace RGB resampled_image.jpg

# Bulk resampling of images in a folder with conversion statuses, saved in a specified folder.
for file in *.jpg; do convert $file -sampling-factor 4:2:0 -strip -quality 1 -interlace JPEG -colorspace RGB ../resampled/$file && echo $file resampled; done
```

### Watermarking
```bash
# Watermarking
composite -dissolve 30% -gravity center watermark2.jpg image.jpg watermarked_image.jpg

# Bulk Watermarking (Save in a specific folder)
for file in *.jpg; do composite -dissolve 30% -gravity center ../watermark.jpg $file ./watered/$file && echo $file watermarked; done
```