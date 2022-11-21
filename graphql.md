# GRAPHQL

## Introduction

- GraphQL is a query language for your API
- A GraphQL service is created by defining types and fields on those types, then providing functions for each field on each type
- **field**

  ```js
    {
      hero {
        name
      }
    }
  ```

- **arguments**

  ```js
    {
      hero(id: '10') {
        name
        height(unit: FOOT)
      }
    }
  ```

- **aliases**

  ```js
    {
      empireHero: hero(episode: EMPIRE) {
        name
      }
      jediHero: hero(episode: JEDI) {
        name
      }
    }
  ```

- **fragments**

  ```js
  {
    leftComparison: hero(episode: EMPIRE) {
      ...comparisonFields
    }
    rightComparison: hero(episode: JEDI) {
      ...comparisonFields
    }

  }

  fragment comparisonFields on Character {
    name
    appearsIn
    friends {
      name
    }
  }
  ```

- Fragment with variable

  ```js
  query HeroComparison($first: Int = 3) {
    leftComparison: hero(episode: EMPIRE) {
      ...comparisonFields
    }
    rightComparison: hero(episode: JEDI) {
      ...comparisonFields
    }

  }

  fragment comparisonFields on Character {
    name
    friendsConnection(first: $first) {
      totalCount
      edges {
        node {
          name
        }
      }
    }
  }
  ```

- **Operation**

  Syntax: `[operation type] [operation name] { }`

  Operation type:
  - `query`: fetching data

  - `mutation`: modify server-side data

  - `subcription`: stream data

- **Directives**

  ```js
  query Hero($episode: Episode, $withFriends: Boolean!) {
    hero(episode: $episode) {
      name
      friends @include(if: $withFriends) {
        name
      }
    }
  }
  ```

  ```js
  {
    "episode": "JEDI",
    "withFriends": true
  }
  ```

## References

- <https://graphql.org/learn/>
