## Set Operations

* **Intersection** and **Cross-Product**: special case of _join_

    * _equality on all attributes_ as join condition => _intersection_
    * ``true`` as join condition computes _cross-product_

* **Union** and **Difference**: selection with complex selection condition

    * _union_: main problem is _duplicate elimination_
    * _difference_: variantion of _duplicate elimination_
    * use _sorting_ or _hashing_ for _duplicate elimination_

### Union and Difference using Sorting

* $R \cup S$

    1. sort both $R$ and $S$ using combination of _all fields_
    2. scan sorted $R$ and $S$ in parallel and merge them, eliminating duplicates
    * For difference $R \setminus S$, keep records from $R$ if they do _not appear_ in $S$
    
* integrate with _external sort operator_

### Union and Difference using Hashing

* $R \cup S$

    1. Partition both $R$ and $S$ using a hash function $h(\cdot)$ over combination of _all_ fields.
    2. Process each record $i$ as follows:
    
        * build in-memory hash table using hash function $h_2(\cdot) \not= h(\cdot)$ for $S[i]$
        * scan $R[i]$, for each tuple probe hash table for $S[i]$, if tuple is in hash table, discard it, otherwise add it to table
        * Write out hash table, clear it to prepare for next partition.
    
    * For difference $R \setminus S$, partition differently. After building in-memory hash table for $S[i]$, $R[i]$ is scanned and $S[i]$ is probed for every tuple in $R[i]$. If tuple is _not in tuple_, it is written to result.














