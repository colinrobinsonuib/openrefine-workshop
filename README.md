# Openrefine Workshop 20 November 2024

## Further Reading:

Intro to Google Refine
- https://www.youtube.com/watch?v=B70J_H_zAWM
- https://www.youtube.com/watch?v=cO8NVCs_Ba0
- https://www.youtube.com/watch?v=5tsyz3ibYzk

Workshop by John Little
- https://libjohn.github.io/openrefine/index.html


---

# Hands On - Part 1

## Import Data

1. Create Project > Web Addresses (URLs) > https://raw.githubusercontent.com/colinrobinsonuib/openrefine-workshop/refs/heads/main/data/building-permits.csv
2.	Click `Next >>`
3.	Select: Columns are separated by `commas (CSV)`
4.	Change the Project Name to `Building Permits` and click `Create Project >>` (top-right)

## Facets & Cluster

1. How many rows are in this data set?
   - <details><summary>Answer</summary>21,982</details>
2. Click the column header: `authorized_work > Facet > Text facet`
3. How many facets are there?
   - You may see a program warning, change the maximum number of choices show in the text facet to 8000 and proceed.
   - <details><summary>Answer</summary>7633</details>
4. In the Facet box, click count
   - What is the 4th most popular type of authorized work?
      - <details><summary>Answer</summary>SCREEN PORCH</details>
   - How many permits (rows) exist for that type fo authorized work??
      - <details><summary>Answer</summary>233</details>
5. To find spelling clusters, click the `Cluster` button in the facet box. Click `Cluster` again.
6. Click the Select All button, then the Merge Selected & Re-Cluster button, to merge all terms (accepting the default: Method = `key collision` ; Keying Function = `fingerprint`)
7. Repeat previous step using the `ngram-fingerprint` Keying Function, then close the Cluster & Edit dialog box
8. How many SCREEN PORCH facets now exist? 
   - <details><summary>Answer</summary>238</details>

## Compound Facets

1. Select the DECK Facet
2. Select the SCREEN PORCH facet combined with the DECK Facet. (Hover your mouse over the facet, click include)
3. How many matching rows exist?
   - <details><summary>Answer</summary>578</details>
4. On the "county" column, make a text facet and limit to Durham County (DURH)
5. How many "Single Family" homes received permits in Durham County for Screen Porches or Decks?
   - <details><summary>Answer</summary>3</details>

## Mass Editing

1. Make a Text Facet on the `work_type_description` column
2. There are two facets for new buildings: `NEW BUILDING` and `New Building`
3. How many `NEW BUILDING` rows exist?
   - <details><summary>Answer</summary>3</details>
4. How many `New Building` rows exist?
   - <details><summary>Answer</summary>9668</details>
5. Select `NEW BUILDING` facet, limiting to 3 matching rows. To the right of the `NEW BUILDING` facet, hover your mouse over the `edit` feature; click `edit` and alter the text to: `New Building`
6. How many `New Building` rows exist?
   - <details><summary>Answer</summary>9671</details>
7. Mass edit `OTHER` & `Other` so they have the same value
8. Mass edit `ALTERATIONS/REPAIRS` and `Alterations/repairs` so they have the same value
9. How many Facet choices exist?
   - <details><summary>Answer</summary>15</details>

## Split data in cells

1. `address` > Edit column > Split into several columns…
   - Separator = `(`
2. `address 2` > Edit column > Split into several columns…
   - Separator = `,`
3. `address 2 1` > Edit column > rename this column > `latitude`
4. `address 2 2` > Edit column > rename this column > `longitude`
5. `longitude` > Edit cells > Transform > `value.replace(")", "")`
6. `longitude` > Edit cells > Commons transforms > To number

---

# Hands On - Part 2

## Import Data

1. Create Project > Web Addresses (URLs) > https://raw.githubusercontent.com/colinrobinsonuib/openrefine-workshop/refs/heads/main/data/police-incidents.csv
2.	Click `Next >>`
3.	Select: Columns are separated by `commas (CSV)`
4.	Change the Project Name to `Police Indicents` and click `Create Project >>` (top-right)

## Custom Facets

*Question: Which year is missing the most location data?*

1. Isolate data: you have incident data without location information
   - Location > Facet > Customized facets > Facet by blank
   - Click true (True = cell is blank ; False = cell has data)
   - How many matching rows have no location data?
2. Explanation for the next step:

*You’ll create a custom facet (by Year) without altering any data. To do this, you’ll use a very simple expression which takes a slice of data based on fixed position, starting in position 6 (counting begins at 0) and ending four character positions later at 10 (6 + 4=10). Given the data, find the year value within the “INC DATETIME” field.*

   - To take the year "2014", use expression: `value[6,10]`
   - To take the hour, use `value[11,13]`
   - To take PM (or AM), use `value[20,22]`

3. Create a Custom Facet to Sort by Year
   - `INC DATETIME` > Facet > `Custom text facet`
   - Expression = value[6,10]
4. Which year is missing the most location data?
    - <details><summary>Answer</summary>2005</details>
5. Keep only the data with location values, i.e. delete all rows that have no location data
   - Remove ("X out") the Year facet
   - All > Edit rows > Remove all matching rows
   - Remove( "X out") the Location facet
6. How many rows remain?
   - <details><summary>Answer</summary>75967</details>

## Regular Expressions

*Learn more about regular expressions [here](https://regexlearn.com/learn/regex101) and test them out [here](https://regexr.com/)*

Create a new column to extract the year from the `INC DATETIME` column:
- `INC DATETIME` > Edit column > Add column based on this column
   - New Column Name = YEAR
   - Expression = `value.match(/.*\/(\d\d\d\d).*/)[0]`
   - Click `OK`

## Text Filters & Facets

*Question: What is the most common bicycle incident?*

**Text Filter**: Explore your data to find how many incident descriptions involve bicycles
- `LCR DESC` > Text filter
   - Enter: `bicycle`
   - How many rows match?
      - <details><summary>Answer</summary>251</details>

**Facet**: Make a Text Facet on the LCR DESC column
- `LCR DESC` > Facet > Text Facet
   - How many bicycle categories are there?
      - <details><summary>Answer</summary>6</details>
   - What is the most common bicycle incident?
      - <details><summary>Answer</summary>LARCENY/BICYCLES ($50-$199) - 119</details>

## Cluster

*Question: How many bicycle incident were not felonies?*

1. `FELONY` > Facet > Text facet
2. `FELONY` > Edit cells > Cluster and edit
   - Method = `key collision` ; Key Function = `metaphone3`
   - For each row, check "Merge?" and change the "New Cell Value"
      - to `Felony` in the first row
      - to `Not Felony` in the second row
   - click the "Merge Selected & Re-Cluster" button
   - Try other "Methods" and “Keying Functions”. “Merge Selected & Re-Cluster” after each operation
3. How many bicycle incidents were not felonies?
   - <details><summary>Answer</summary>There are 234 items after clustering using both Methods "key collision" and "nearest neighbor"</details>

---
 
# Hands On - Part 3

## Import Data

1. Create Project > Web Addresses (URLs) > https://raw.githubusercontent.com/colinrobinsonuib/openrefine-workshop/refs/heads/main/data/authors.csv
2.	Click `Next >>`
3.	Select: Columns are separated by `commas (CSV)`
4.	Change the Project Name to `Authors` and click `Create Project >>` (top-right)

## Reconcile with Wikidata

*Question: How old were each of these authors when they died?*

`name` > Reconcile > Start reconciling

*The first time you use this feature, you will need to add a new service. Click "Add Standard Service" and enter the URL: `https://wikidata.reconci.link/en/api`*

1. Select the service > `Next >>`
2. Pick an instance type to reconcile against: `human (Q5)`
3. Start reconciling
4. Facet by `none` to see which authors were not matched
5. Select `Search for match` under each unmatched author and find a match
6. Remove all facets

## Add Columns by Fetching Wikidata values

1. `name` > Edit column > Add columns from reconciled values
2. Add property `date of birth`
3. Add property `date of death`
4. Click `OK`

## Calculate Age

1. `date of death` > Edit column > Add column based on this column
2. New Column Name = `age`
3. Expression = `diff(value, cells["date of birth"].value, "years")`
4. Click `OK`

## Bonus

Use the current time as the date of death for alive authors

1. Undo creating the `Age` column
2. Facet `date of death` by blank
3. Select all blank rows
4. `date of death` > Edit cells > Transform
5. Expression = `now()`
6. Remove all facets
7. Create the `Age` column as specified above
