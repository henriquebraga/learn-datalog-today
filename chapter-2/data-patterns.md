# Summary

[Explanation and exercises here](http://www.learndatalogtoday.org/chapter/2)

Just recapping the datom structure to perform pattern matching:

`[entity-id attribute value transaction-id]`

# Problem 0

Find movie titles made in 1985

## Explanation

* The query needs to return title from movie, so we create a pattern variable `?title` **[:find ?title**
* Filter datoms who have attribute `:movie/year` and value `1985` and bind entity-id to pattern variable `?movie` [?movie :movie/year 1985]
* We need to get all entities that have attribute `:movie/year` and value `1985`. We can bind them to pattern variable `?movie`. [?m :movie/year 1985]
* Now, we can bind the pattern variable `?title` that contains the result we want from query. We need to use the entity ids from previous data pattern. [?m :movie/title ?title]
* Basically whe say: "Give me all `?movie` (entity-id) that have attribute `:movie/year` with value `1985`. Now, with `?movie` (entity-id) find all datoms that has attribute `:movie/title` and bind values to variable `?title`"

## Solution

```clojure
[:find ?title
 :where
 [?movie :movie/year 1985]
 [?movie :movie/title ?title]]
```

# Problem 1

What year was "Alien" released?

## Explanation

* Query needs to return the year from the movie `"Alien"`, so we create a pattern variable `?year` 
* Filter datoms who have attribute `:movie/title` and value `"Alien"` and bind entity-id to the pattern variable `?movie` [?movie :movie/title "Alien"]
* Filter datoms with entity-id bind to `?movie` and attribute `:movie/year` and bind values to data pattern variable `?year`  [?movie :movie/year ?year]

### Pattern Variables

```
?movie: Entity ids that have attribute :movie/title and value "Alien"
?year: Values that have entity ids binded in ?movie and attribute :movie/year
```

## Solution

```clojure
[:find ?year
 :where
 [?movie :movie/title "Alien"]
 [?movie :movie/year ?year]]
```

# Problem 2

Who directed RoboCop? You will need to use [<movie-eid> :movie/director <person-eid>] to find the director for a movie.

## Explanation

* Query needs to return the director name,  so we create a pattern variable `?name` **[:find ?name**
* Create a pattern variable to bind all movies `?movie` and filter datoms which have attribute `:movie/title` with value `"Robocop"` [?movie :movie/title "RoboCop"]
* Filter datoms with entity-id bind to `?movie`, attribute `:movie/director` and bind values to `?director` pattern variable (don't forget that `:movie/director` is a reference to one person) [?movie :movie/director ?director]
* Filter datoms with entity-id bind to `?director`, attribute `:person/name` and bind values to `?name` (tells: "Give me the names from entity-ids bind to `?director`") [?director :person/name ?name]

### Pattern Variables

```
?movie: Entity-ids that have attribute :movie/title with value "RoboCop"
?director: All values (which is a reference for a person) from datoms who have entity-id binded in ?movie (contains entity-ids that have attribute :movie/title and value RoboCop)
?name: Values from datoms that have attribute :person/name and entity-id binded in ?director pattern variable 
```

## Solution

```clojure
[:find ?name
 :where
 [?movie :movie/title "RoboCop"]
 [?movie :movie/director ?director]
 [?director :person/name ?name]]
```

# Problem 3

Find directors who have directed Arnold Schwarzenegger in a movie.

## Explanation

* Query needs to return directors, so we create a pattern variable `?name` [:find ?name]
* Filter datoms with attribute `:person/name` that has value `"Arnold Schwarzenegger"` and bind entity-id to `?actor` pattern variable (tells: "Give me all entity-ids from people that have name Arnold Schwarzenegger") [?actor :person/name "Arnold Schwarzenegger"]
* Filter datoms with attribute `:movie/cast`, value bind to `?actor`, and bind entity-ids to `?movie` pattern variable (tells: "Give me all entity ids from movies that Arnold Schwarzenegger acted"  [?movie :movie/cast ?actor]
* Filter datoms with entity-id bind to `?movie` and attribute `:movie/director` and bind values to `?director` pattern variable (tells: "Give me all entity ids from directors from movies that Arnold Schwarzenegger acted and bind to `?director`"). [?movie :movie/director ?director]
* Filter datoms with entity-id bind to `?director` and attribute `:person/name` and bind values to `?name` pattern variable. (tells: "Give me all names from entity-ids in `?director`")

### Pattern Variables

```
?name: result binded with all names who have directed Arnold Schwarzenegger
?actor: all entities 
?director: all directors from wit
?name: Values from datoms that have entity-id binded in ?director and attribute :person/name
```

## Solution

```clojure
[:find ?name
 :where
 [?actor :person/name "Arnold Schwarzenegger"]
 [?movie :movie/cast ?actor]
 [?movie :movie/director ?director]
 [?director :person/name ?name]]
```