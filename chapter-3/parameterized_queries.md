# Summary

[Explanation and exercises here](http://www.learndatalogtoday.org/chapter/3)

# Problem 0

Find movie title by year

## Explanation

## Solution

```clojure
[:find ?title
 :in $ ?year
 :where
 [?movie :movie/year ?year]
 [?movie :movie/title ?title]]
```

# Problem 1

Given a list of movie titles, find the title and the year that movie was released.

## Explanation


### Pattern Variables

```
```

## Solution

```clojure
[:find ?title ?year
 :in $ [?title ...]
 :where
 [?m :movie/title ?title]
 [?m :movie/year ?year]]
```

# Problem 2

Find all movie `?title`s where the `?actor` and the `?director` has worked together

## Explanation

### Pattern Variables

```
```

## Solution

```clojure
[:find ?title
 :in $ ?actor ?director
 :where
 [?actor-id :person/name ?actor]
 [?director-id :person/name ?director]
 [?movie :movie/director ?director-id]
 [?movie :movie/cast ?actor-id]
 [?movie :movie/title ?title]]
```

# Problem 3

Write a query that, given an actor name and a relation with movie-title/rating, finds the movie titles and corresponding rating for which that actor was a cast member.

## Explanation

### Pattern Variables

```
```

## Solution

```clojure
[:find ?title ?rating
 :in $ ?name [[?title ?rating]]
 :where
 [?actor :person/name ?name]
 [?movie :movie/title ?title]
 [?movie :movie/cast ?actor]]]
```