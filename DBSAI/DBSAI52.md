# Relational Operator Evaluation

* _alternative algorithms_ can be used to implement each operator

    * performance depends on query, cardinalities, indexes, sort order, size of buffer pool, replacement policiy, …

* **Common implementation techniques**:

    * **indexing**: an index is used to only process tuples that satisfy a selection or join condition
    * **iteration**: process all tuples of an input table, one after the other or scan all index data entries (index only scan)
    * **partitioning**: decompose an operation into less expensive collection of operations by partitioning tuples on sort key (e.g. sorting and hashing)

## Access Path

* _access path_ = way of retrieving tuples from table

    * _file scan_
    * _index_ plus _selection condition_

* Relational Operator accept _one or more tables_ as input
* Access path significantly contributes to _cost of operator_
* **Conjunctive Normal Form** (CNF): A conjunction of conditions of the form ```attribute operator value```, where ```operator``` is one of the comparison operators $<, \leq, =, \not= \geq, >$, each condition is called a _conjunct_
* An index **matches** a selection criterion if it can be used to retrieve just the tuples that satisfy the condition

    * A hash index matches a CNF selection => $attribute = value$ for each attribute in the index' search key
    * A tree index matches a CNF celection => $attribute op value$ for each attribute in the index' search key
    * A search key prefix is the prefix of a search key: $<a>, <a,b>$ are prefixes of key $<a,b,c>$

        * An index can match a subset of a CNF selection: **primary conjuncts**: the conjucnts that the index matches
        * When only prefix of index is used, produced tuples need to be checked for missing conditions.
        
* **Selectivity** of an access path: number of index and data entries it retrieves

    * the _most selective_ access path is the one that retrieves fewest pages
    * selectivity depends on primary conjuncts
    
        * each conjunct acts as a filter on the table
    
* **Reduction Factor**: the fraction of tuples in the table that satisfy a conjunct

    * for several primary conjuncts, the fraction satisfying them all can be approximated by multiplying their reduction factors
    * Reduction Factor Estimation: Assume uniform distribution of data, catalog information $ILow$ and $IHigh$ can be used
    * for tree index $T$, reduction factor for $attr < value$ is: $\frac{IHigh(T) - value}{IHigh(T) - ILow(T)}$

