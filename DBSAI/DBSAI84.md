## Nested Subqueries

* consists of multiple query blocks
* Optimization options depend on

    * form of ``WHERE`` predicate
    * presence or absence of _aggregates_ in subquery
    * whether subquery is _uncorrelated_ or _correlated_
    * other characteristics

* **SQL Canonical Form**: if the ``FROM`` clause only contains table names and its ``WHERE`` clause only contains simple and join predicates
* **correlated subquery**: if it references table or tuple variable of the top-level query

* Types of Nesting:

    * **Type A**: uncorrelated, aggregated subquery
    * **Type N**: uncorrelated, not aggregated subquery
    * **Type J**: correlated, not aggregated subquery
    * **Type JA**: correlated, aggregated subquery
    * **Type D**: correlated, division subquery => no longer possible, as ``CONTAINS`` was removed

* Nested Subquery Rewrite:

    1. rewrite query to **Canonical Form**
    2. Based on type of subquery, one of two algorithms:
    
        * ``NEST-N-J``: transform nested subquery of Type N and Type J to a query in canonical form
        
            ![``Nest-N-J`` algorithm](images/NEST-NJ.png)
        
            1. combine ``FROM`` clauses of all query blocks into one ``FROM`` clause
            2. combine ``WHERE`` clauses of all query blocks using ``AND``, replacing element test (``IN``) with equality (``=``)
            3. retain ``SELECT`` clause of outermost query block
            
        * ``NEST-JA``: use temporary tables to remove one level of nesting for subqueries of type JA
        
            ![``NEST-JA`` algorithm part 1](images/NEST-JA1.png)
            
            ![``NEST-JA`` algorithm part 2](images/NEST-JA2.png)
            
            1. project join column of $R_1$ and restrict it with any simple predicates applying to $R_1$
            2. create temporary relation $R_{tmp} by joining $R_1$ and $R_2$ using same operator as join predicate in original query
            
                * if aggregate function is ``COUNT``, join must be outer join
                * if aggregate function is ``COUNT(*)``, compute aggregate over join column
                * include join column(s) and aggregated column in ``SELECT`` clause
                * include join columns(s) in ``GROUP BY`` clause
            
            3. join $R_1$ to $R_{tmp}$ according to transformed version of original query by changing join predicate to equality (``=``)
            * transform resulting Type J query with ``NEST-N-J``

















