# System Catalog

* _two kinds of information_ in DBMS:

    * Data
    
        * alternative file structures for _tables_ and _indexes
        * files contain either _tuples of a table_ or _index entries_
        * collection of user tables and indexes represent the data of the database
    
    * Metadata

        * DBMS maintains information that describes tables and indexes
        * descriptive information => stored in _special table_
        
            => common names: catalog table, data dictionary, _system catalog_

* System Catalog

    * size of buffer pool, page size, etc
    * information about tables, views, indexes
    * statistics about tables and indexes
    * statistics are updated periodically, not every time tables are modified

* System Catalog is stored as a collection of tables itself

    * existing techniques for implementing and managing tables
    * same query language used for catalog tables
