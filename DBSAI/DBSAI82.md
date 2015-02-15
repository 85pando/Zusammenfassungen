## Rewrite Optimization

* Optimization is guided by heuristics:

    1. **Break apart conjunctive selections** into a sequence of simpler selections
    2. **Move selections down the query tree** to reduce the cardinality of intermediate results as early as possible
    3. **Replace Selection-Cross-Product pairs with join** to avoid large intermediate results
    4. Break lists of projection attributes apart, move them down the query tree** and **create new projections where possible** to reduce tuple widths as early as possible
    5. **Perform joins with the smallest expected result first** (cost based heuristic)

* Implicit join predicates can be turned into explicit one to enable more join orders.


* Plan enumeration








