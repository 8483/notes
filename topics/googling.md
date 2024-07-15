# Googling

These are called advanced search operators.

```bash
# Keywords

"keyword"                # Must include keyword
"foo bar baz"            # Must include phrase

-keyword                 # Must exclude keyword
-foo -bar -baz           # Must exclude keywords

# Content

intitle: foo             # Single
allintitle: foo bar      # Multiple

inurl: foo             # Single
allinurl: foo bar      # Multiple

intext: foo             # Single
allintext: foo bar      # Multiple

# Specific website

website.com: query
site:website.com query
link:example.com        # Sites linking to example.com

# Files

filetype: pdf
ext: pdf

# Period

after: 2017
before: 2018

2017..2018               # Between dates

# Utilities

define: meaning
weather: berlin
map: place
stocks: aapl
```

Google Dorking, also known as Google Hacking, is a technique that utilizes advanced search operators to uncover information on the internet that may not be readily available through standard search queries.

Examples:

```bash
intitle: webcampxp 5

filetype:env "DB_PASSWORD"
```
