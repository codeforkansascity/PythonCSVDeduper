**The KCDigitalDrive folder has a script to dedupe records in a csv file.**
The starting point for this code came from example scripts here:  [dedupe](https://github.com/dedupeio/dedupe), a library that uses machine learning to perform de-duplication and entity resolution quickly on structured data.

The python script is used to identify potential duplicate records in a CSV file.  It has been trained for several CSV files, but would require new training if different features are included.

As input, the program expects to receive two AWS S3 buckets, one to read files from, and the other to write output result files to.

Because files from different agencies may have different column names and orders for features (First Name, Last Name, City, etc.), the Mappings.csv file can be used to map the fields in each file to the expected pattern.

The program expects to run in an OS context that has access to an S3 environment.  It will look for files in that S3 environment under the folder name passed in followed by \input, and write files out to a folder \output.




This is copied from: https://github.com/codeforkansascity/DedupeSQSReaderService  which is forked from another library of examples. https://github.com/dedupeio/dedupe-examples.git




# Dedupe Examples
```

### [CSV example](https://dedupeio.github.io/dedupe-examples/docs/csv_example.html) - early childhood locations

This example works with a list of early childhood education sites in Chicago from 10 different sources.

```bash
cd csv_example
pip install unidecode
python csv_example.py
```
  (use 'y', 'n' and 'u' keys to flag duplicates for active learning, 'f' when you are finished)

**To see how you might use dedupe with smallish data, see the [annotated source code for csv_example.py](https://dedupeio.github.io/dedupe-examples/docs/csv_example.html).**

### [Patent example](https://dedupeio.github.io/dedupe-examples/docs/patent_example.html) -  patent holders

This example works with Dutch inventors from the PATSTAT international patent data file

```bash
cd patent_example
pip install unidecode
python patent_example.py
```
  (use 'y', 'n' and 'u' keys to flag duplicates for active learning, 'f' when you are finished)

### [Record Linkage example](https://dedupeio.github.io/dedupe-examples/docs/record_linkage_example.html) -  electronics products
This example links two spreadsheets of electronics products and links up the matching entries. Each dataset individually has no duplicates.

```bash
cd record_linkage_example
python record_linkage_example.py
```

**To see how you might use dedupe for linking datasets, see the [annotated source code for record_linkage_example.py](https://dedupeio.github.io/dedupe-examples/docs/record_linkage_example.html).**

### [Gazetteer example](https://dedupeio.github.io/dedupe-examples/docs/gazetteer_example.html) -  electronics products
This example links two spreadsheets of electronics products and links up the matching entries using the Gazetteer class

```bash
cd gazetteer_example.py
python gazetteer_example.py
```


### [MySQL example](https://dedupeio.github.io/dedupe-examples/docs/mysql_example.html) - IL campaign contributions

See `mysql_example/README.md` for details

**To see how you might use dedupe with bigish data, see the [annotated source code for mysql_example](https://dedupeio.github.io/dedupe-examples/docs/mysql_example.html).**


### [PostgreSQL big dedupe example](https://dedupeio.github.io/dedupe-examples/docs/pgsql_big_dedupe_example.html) - PostgreSQL example on large dataset

See `pgsql_big_dedupe_example/README.md` for details

This is the same example as the MySQL IL campaign contributions dataset above, but ported to run on PostgreSQL.


## Training

The _secret sauce_ of dedupe is human input. In order to figure out the best rules to deduplicate a set of data, you must give it a set of labeled examples to learn from.

The more labeled examples you give it, the better the deduplication results will be. At minimum, you should try to provide __10 positive matches__ and __10 negative matches__.

The results of your training will be saved in a JSON file for future runs of dedupe.

Here's an example labeling operation:

```bash
Phone :  2850617
Address :  3801 s. wabash
Zip :
Site name :  ada s. mckinley st. thomas cdc

Phone :  2850617
Address :  3801 s wabash ave
Zip :
Site name :  ada s. mckinley community services - mckinley - st. thomas

Do these records refer to the same thing?
(y)es / (n)o / (u)nsure / (f)inished
```
