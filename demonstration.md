# Demonstration

## Import Data

1. Create Project > Web Addresses (URLs) > https://raw.githubusercontent.com/colinrobinsonuib/openrefine-workshop/refs/heads/main/data/bicycle-collection.tsv
2. Columns are seperated by tabs (TSV)
3. Uncheck - "Quotation marks are used to enclose" (bottom)
4. Project Name to "bicycle categories"

## Navigating the "Project"
- Notice there are 72 rows
- Notice there are two views: rows and records
- The "records" view is for advanced manipulation
- `Description` > View > Collapse
- `Marks` > View > Collapse
- You can
   - "Show" 5-50 rows at a time
   - Navigate screens "< Previous" & "next >"

## Facets

Categories > Facet > Text facet

- Notice mashup of "pipe delimited" text facets
- Switch between the "name" and "count" sorting options.

## Rows vs. Records

`Categories` > edit cells > split multi-valued cells > separator = |

- Now how many cateogry choices exist?
- Now how many rows exist?
- Click records to switch to the records view
- Click count What is the most popular Category term?
- Click name Limit to the "Juvenilia" facet; How many matching records?

## Web Scrape & API

1. `new_link` > Edit column > Add column by fetching URLs
2. New column name = Web Data
   - Notice Throttle delay
   - 2000 milliseconds is good. Less than 2 seconds and you MIGHT get booted, blocked, cast out!
3. Click OK and wait
4. When done, you’ve got all the source data for each page associated with the link in the "Persistent Link" column

## Parsing Data

Typically, after fetching data, you’ll need to parse it.

For example, you can transform the data in the retrieved cells with a regular expression to gather only the title of each object.

1. `Web Data` > Edit column > Add column based on this column
2. New Column Name = Web Page Title
3. Expression = `value.parseHtml().select("h2")[0].htmlText()`

## Split
1. remove faceting: click Remove All in the facet sidebar
2. `Production Date` > Edit column > Split into several columns…
3. Seperator = - (i.e. "space dash space")

## Concatenate
- Height > Edit column > Add column based on this column
- New column name: Dimension
- GREL Expression = `value + " x " + cells["Width"].value`

## Find & Replace
- Dimension > Edit cells > Transform
- GREL Expression = value.replace("mm","")

## Facet by Blank
- Dimension > Facet > Customized facet > facet by blank

## Text filter
- Clear all facets
- Object Title > Text filter > bmx

## Custom Facet
- Production date 1
- Facet > custom text facet
- `value[0,2]` or `value.match(/(\d\d).*/)[0]`

# University Clustering
1. Load university-data.csv
2. Facet by country
3. Cluster by fingerprint
4. Cluster by Daitch-Mokotoff
5. Manually rename
6. Get countries down to 46

We just cleaned all the country values for 5500 rows of data!
