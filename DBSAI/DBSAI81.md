# Query Optimization

* To evaluate given SQL query $Q$

    1. parse and analyse $Q$
    2. derive _relational algebra_ expression $E$ that computes $Q$
    3. generate set of _logical plan_ $L$ by transforming and simplifying $E$
    4. generate a set of _physical plans_ $P$ by annotating each plan in $L$ with access methods and operator algorithms
    5. for each plan in $P$, estimate quality (cost) of plan and choose the best plan as final plan
    
* Query optimizer does last three steps:

    * plan enumeration in steps 3 and 4
    * algebraic (or rewrite) query Optimization in step 3
    * non-algebraic (or cost-based) query optimization in steps 4 and 5
    
* Parser: creates internal representation of input query

    * each ``SELECT-FROM-WHERE`` clause is translated into _query block_

* Query Optimizer: consider each query block and choose evaluation plan for this block

    * focus on single block queries for now
    * optimization of _nested queries_ discussed separately
    
* To find best plan, query optimizer explores _search space_

    * _logical level_: relational algebra equivalences
    * _physical level_: access methods and operator algorithms

## Relational Algebra Equivalences:

* Each query block is a relational algebra expression consisting of cross-product, selection and projection
* rewrite optimization applies _relational algebra equivalences_ to transform query blocks

    * cross products => join
    * choose different join orders
    * push selections and projections ahead of joins

* Two relational algebra expressions $E_1$, $E_2$ are _equivalent_ if they generate the same set of tuples on every legal database instance

    * $E_1 \equiv E_2$
    * Rules may be applied in both directions

* Selections

    1. conjunctive selections can be deconstructed into sequence of individual selections (_cascading selections_) $$\sigma_{c1 \wedge c2 \wedge \dots \wedge cn} (R) \equiv \sigma_{c1}(\sigma_{c2}(\dots (\sigma_{cn}(R))\dots))$$
    2. selections are _commutative_ $$\sigma_p(\sigma_q(R)) \equiv \sigma_q(\sigma_p(R))$$

* Projections

    1. only the last projection in a sequence of projections is needed, the others can be omitted (_cascading projections_) $$\pi_{a1}(\pi_{a2}(\dots(\pi_{an}(R))\dots)) \equiv \pi_{a1}(R)$$

* Cross-Products and Natural Joins

    1. _commutative_
        \begin{align*}
        R \times S & \equiv S \times R \\
        R \Join S & \equiv S \Join R
        \end{align*}
        
    2. _associative_
        \begin{align*}
        R \times (S \times T) & \equiv (R \times S) \times T \\
        R \Join (S \Join T) & \equiv (R \Join S) \Join T
        \end{align*}

* General Joins

    1. _associative_ $$(R \Join_p S) \Join_{q \wedge r} T \equiv R \Join_{p \wedge q} (S \Join_r T)$$ where $r$ involves only attributes of $S$ and $T$

* Selections, Cross-Product, Join

    1. selections can be _combined_ with cross-product to form join
        $$\sigma_p (R \times S) \equiv R \Join_p S$$
    2. selection can be combined with join
        $$\sigma_p (R \Join_q S) \equiv R \Join_{p \wedge q} S$$
    3. selections _commute_ with cross-products and joins, if predicate $p$ involves attributes of $R$ only
        \begin{align*}
        \sigma_p( R\times S) &\equiv \sigma_p (R) \times S \\
        \sigma_p( R\Join S) &\equiv \sigma_p (R) \Join S
        \end{align*}
    4. selections _distribute_ over cross-product and join, if predicates $p$ only involves attributes of $R$ and predicate $q$ only involves attributes of $S$
        \begin{align*}
        \sigma_{p \wedge q}(R \times S) &\equiv \sigma_p(R) \times \sigma_p (S) \\
        \sigma_{p \wedge q}(R \Join_r S) &\equiv \sigma_p(R) \Join_r \sigma_p (S)
        \end{align*}

* Projections, Selections, Cross-Product, Join

    1. projection and selection _commute_, if the selection predicate $p$ only involves attributes retained by the projection list $a$
        $$ \pi_a(\sigma_p(R)) \equiv \sigma_p(\pi_p(R))$$
    2. projection _distributes_ over cross-product, where $a_1$ is the subset of attributes in $a$ that appear in $R$ and $a_2$ is the subset of attributes that appear in $S$
        $$\pi_a(R \times S) \equiv \pi_{a1}(R)\times \pi_{a2}(S)$$
    3. projection _distributes_ over join where $a1$ is the subset of attributes in $a$ that appear in $R$, $a2$ is a subset of attributes that appear in $S$ and predicate $p$ only involves attributes in $a_1\cup a_2$
        $$\pi_a(R \Join_p S) \equiv \pi_{a1}(R) \Join_p \pi_{a2}(S)$$
        
* Union and Intersection

    1. are _commutative_
        \begin{align*}
        R \cup S &\equiv S \cup R \\
        R \cap S &\equiv S \cap R
        \end{align*}
    2. are _associative_
        \begin{align*}
        (R \cup S) \cup T &\equiv R \cup (S \cup T) \\
        (R \cap S) \cup T &\equiv R \cap (S \cap T)
        \end{align*}

* Projection and Union

    1. projections _distribute_ over union
        $$\pi_a (R \cup S) \equiv \pi_a (R) \cup \pi_a (S)$$

* Selection, Union, Intersection, Difference

    1. are _distributive_
        \begin{align*}
        \sigma_p (R \cup S) &\equiv \sigma_p (S) \cup \sigma_p (R) \\
        \sigma_p (R \cap S) &\equiv \sigma_p (S) \cap \sigma_p (R) \\
        \sigma_p (R \setminus S) &\equiv \sigma_p (S) \setminus \sigma_p (R)
        \end{align*}

    2. Intersect and Difference are _commutative_ (does _not_ apply to Union)
        \begin{align*}
        \sigma_p (R \cap S) &\equiv \sigma_p (S) \cap R \\
        \sigma_p (R \setminus S) &\equiv \sigma_p (S) \setminus R
        \end{align*}








