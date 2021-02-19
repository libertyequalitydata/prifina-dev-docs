# Aggregated Data API
## Introduction
The Aggregated Data API is a GraphQL API providing extensive access to Prifina user data. It is only accessible to Prifina core functions, [local data apps](../../Introduction/#local-data-apps), and [data widgets](../../Introduction/#data-widgets). In addition to querying to retrieve user data, it provides the ability to insert user data generated during the use of the app. This means that as a developer, you're able to record data about user interactions while maintaining the user's fundamental ownership and control over over that data.

This API follows the official [GraphQL specification](https://spec.graphql.org/). It uses [cursor-based pagination](https://graphql.org/learn/pagination/#end-of-list-counts-and-connections) for all lists included. In this documentation we'll go over some usage examples for this API as well as provide a reference for the schema used.

## API Reference

### Rules
To simplify usage, a few standard rules are applied across the API.
#### Pagination
All fields returning arrays use cursor-based pagination following the [Relay Connections specification](https://relay.dev/graphql/connections.htm). In short, each array field has the type of a "connections" object with the following fields:
- `count: Int` The total number of nodes returnable.
- `edges: [node]` Each node is of the type of the array queried.
- `pageInfo: PageInfo` Metadata regarding the page of query results. See [`type: PageInfo`](#type-pageinfo).

For control of this pagination, each field of a Connection type takes an argument field `pageArgs: PageArgs`. 

**Query:**
```graphql
query {
  profileInfo {
    educations(pageArgs : {
                            first: 1
                          }
              ){
      totalCount
      edges {
        node {
          institution
        }
        cursor
      }
      pageInfo {
        endCursor
        hasNextPage
      }
    }
  }
}
```

**Response:**
```json
{
  "data" : {
    "profileInfo" : {
      "educations" : {
        "totalCount" : 1,
        "edges" : [
          {
            "node" : {
              "institution" : "Greendale Community College"
            },
            "cursor" : "x12Vafg8yMg"
          }
        ],
        "pageInfo" : {
          "endCursor" : "x12Vafg8yMg",
          "hasNextPage" : true
        }
      }
    }
  }
}
```

### Queries
- `profileInfo: ProfileInfo` Information regarding a user's personal profile.

### Object Types
#### type: Address
Based on [xNAL standard], an address in generalized international format. Fields here are deliberately vague to account for the wide variety of conventions in use internationally.

#### type: PageArgs
Pagination control arguments to be passed to a Connection object.

**Fields:**
- `first: Int` Number of results to get.
- `after: String` Get results following a given cursor.
- `offset: Int` Offset of results. For example, an offset of (-1) would include the cursor specified by `after`.

#### type: PageInfo
Metadata about a page of results, used in [pagination](#pagination).

**Fields:**
- `endCursor: String` Cursor for final edge of page.
- `hasNextPage: Boolean` Whether there are more results after the position of `endCursor`.

#### type: ProfileInfo
Profile-level information on a user.

**Fields:**
- `firstName: String` Given name.
- `lastName: String` Surname.
- `middleName: String` Middle name.
- `dob: ISODateTime` Date/time of birth.
- `gender: String` Gender.
- `orientation: String` Sexual orientation.
- `residences: [ResidenceConnection]` Residences, current and former, on record. See: [`type: Residence`](#type-residence).

#### type: Residence
**Fields:**
- `address: Address`
- `isCurrent: Boolean` Whether residence is currently in use.
