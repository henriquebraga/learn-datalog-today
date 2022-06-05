# Summary

[Explanation here and exercises here](http://www.learndatalogtoday.org/chapter/1)

Database consist of datoms.

Datom structure: [entity-id, attribute, value, transaction-id]

### Query structure

Vector starting with keyword `:find`

Pattern variables: symbols starting with ?

`:where`: filter to match datoms. It always try to match with the given `data pattern`.

Example: Find entity-ids with person name "Ridley Scott"

```clojure
[:find ?e
 :where
 [?e :person/name "Ridley Scott"]]
```

# Problem 0

Find the entity ids of movies made in 1987

## Solution

Here we want the result from pattern variable ?e

Remember the structure from a datom is [entity-id attribute value transaction-id]

Looking carefully after :where, Datomic (the query engine) will:

* Bind with every possible value the pattern ?e with all entities
* Get all datoms that match the pattern (attribute `:movie/year` and value `1987`).

```clojure
[:find ?e
 :where
 [?e :movie/year 1987]]
```

# Problem 1

Find the entity-id and titles of movies in the database

## Explanation

* There are two things the query needs to return, so we can have two pattern variables: `?e` (entity-id) `?title` (title of movies)
* Since datom structure is [entity-id attr value tx-id], we only need to place those patterns in `entity-id` and `value`, but we also need to set attribute as `:movie/title`: `[?e :movie/title ?title]` 
* Actually the restriction in where says: "Give me all `?e` (entity-id from datom) and `?title` (value from datom) that has attribute `movie/title`"

## Solution

```clojure
[:find ?e ?title
 :where
 [?e :movie/title ?title]]
```

# Problem 2

Find the name of all people in the database

## Explanation

* This time we are only interested in name from people, there's just one pattern variable: `?name`
* But since we have no interest in entity-id, we can use it a wildcard `_` that just says "Ok, we won't need it, you can ignore" `[_ :person/name ?name]`

## Solution

```clojure
[:find ?name
 :where
 [_ :person/name ?name]]
```