# Processing of PDF files

The PDF files were processed with `science-parser` and `pdffigures2`: You can download and compile both from:
- `science-parser`: https://github.com/allenai/science-parse
- `pdffigures2`: https://github.com/allenai/pdffigures2

If you leave the pdfs in the folder `data/pdfs/data/`, then you can run the following two commands (working directory `data/pdfs`):

```
java -Xmx6g -jar path-to/science-parse-cli-assembly-2.0.2-SNAPSHOT.jar . data/ -f parsing/article_information.json
```

The file `article_information.json` will have the following schema if parsed with Spark:

```python
In [ ]: spark.read.\
    json('parsed/article_information.json').\
    printSchema()
```

```
root
 |-- metadata: struct (nullable = true)
 |    |-- abstractText: string (nullable = true)
 |    |-- authors: array (nullable = true)
 |    |    |-- element: string (containsNull = true)
 |    |-- creator: string (nullable = true)
 |    |-- emails: array (nullable = true)
 |    |    |-- element: string (containsNull = true)
 |    |-- referenceMentions: array (nullable = true)
 |    |    |-- element: struct (containsNull = true)
 |    |    |    |-- context: string (nullable = true)
 |    |    |    |-- endOffset: long (nullable = true)
 |    |    |    |-- referenceID: long (nullable = true)
 |    |    |    |-- startOffset: long (nullable = true)
 |    |-- references: array (nullable = true)
 |    |    |-- element: struct (containsNull = true)
 |    |    |    |-- author: array (nullable = true)
 |    |    |    |    |-- element: string (containsNull = true)
 |    |    |    |-- citeRegEx: string (nullable = true)
 |    |    |    |-- shortCiteRegEx: string (nullable = true)
 |    |    |    |-- title: string (nullable = true)
 |    |    |    |-- venue: string (nullable = true)
 |    |    |    |-- year: long (nullable = true)
 |    |-- sections: array (nullable = true)
 |    |    |-- element: struct (containsNull = true)
 |    |    |    |-- heading: string (nullable = true)
 |    |    |    |-- text: string (nullable = true)
 |    |-- source: string (nullable = true)
 |    |-- title: string (nullable = true)
 |    |-- year: long (nullable = true)
 |-- name: string (nullable = true)

```

To parse the figures, run the following (same working directory as before):

```
java -Xmx6g -jar path-to/pdffigures2-assembly-0.0.12-SNAPSHOT.jar data -i 300 -s parsed/figure_results.json -d parsed/ -m parsed/
```

You can read the results with Pandas:
```python
import pandas as pd
 pd.read_json('parsed/18621-271496-4-PB.json')
```
