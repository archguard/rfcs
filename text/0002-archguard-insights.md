- Start Date: 2022-06-15
- RFC PR: (leave this empty)
- ArchGuard Issue: (leave this empty)

# Summary

As a developer, we can custom Insights/Graph for some specifical usage. Like workbench, but it has more components.

We can:

 - track legacy code. like js => ts
 - manage dependencies. like Dockerfile
 - search for dep bugs. like Maven, Gradle, NPM
 - find Deprecated API.
 - tracking API numbers in History?

it can be:

- regexp-based, it will be simple. 
- Kotlin DSL, it can works with workbench.

# Basic example

## select

```
# context
context: contextName

# all
system: @all

# system by name
system: "systemName"

# system by id
system: ${number}
```

## type

### count files

```
type:sourcecode lang:ts file:xxx 
```

### scene: SCA

Log4j

need lookup for all group

```
type:sca group:org\.apache\.logging version?regexp
```

count deps with version?

```
type:sca group:
```

### count: API

```
type:api     patterntype:regexp
```

## expression

like

```
artifact.name + "$artifact.id" ..
```

# Motivation

In order to manage/custom all codes with ArchGuard.

# Detailed design

- wrapper data API for query. with MongoDB or MySQL
- insights API for save system.
- online DSL verify?

# Implementation

## Implementation Patterns

1. filter before query.  manual filter by developer in .
2. filter with query.
3. query after filter.

### Filter before Query

todo

### Filter with Query

### Query after Filter.

Code Query Exression?

```
SELECT COUNT (DISTINCT id) WHERE lang=ts file=like?
```

class="FileController"

method=""

version >= ""? `select all and then filter` ?

to AST and validate?

# Drawbacks

none.

# Alternatives

## Rule

use Rule to count with issue numbers. But also, need to make rule with graph.

## Like sourcegraph

Sourcegraph Insights: [https://sourcegraph.com/insights](https://sourcegraph.com/insights)

Migration to CSS modules

```
select:file lang:scss -file:module.scss patterntype:regexp
```

Alpine versions over all repos

```
patterntype:regexp FROM\s+alpine:([\d\.]+) file:Dockerfile
```

Updated log4j

```
lang:gradle org\.apache\.logging\.log4j['"] 2\.(17)(\.[0-9]+) patterntype:regexp
```

Vulnerable log4j

```
lang:gradle org\.apache\.logging\.log4j['"] 2\.(0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16)(\.[0-9]+) patterntype:regexp
```

# Adoption strategy

no need.

# How we teach this

custom insights

# Unresolved questions

How to make insights with MongoDB?

