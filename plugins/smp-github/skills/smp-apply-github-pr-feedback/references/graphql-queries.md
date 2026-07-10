# GraphQL Queries for PR Review Thread Resolution

**Important:** All queries below use inline values (placeholder tokens in UPPERCASE) instead of GraphQL variables. Do NOT use GraphQL variable declarations (for example, `query($owner: String!)` or `mutation($threadId: ID!)`) or `-f`/`-F` flags for substituting query variables (for example, `-f owner=... -F pr=...`) — the `$` characters in variable declarations can be mangled by shell quoting, causing the API to reject the query with `Expected VAR_SIGN, actual: UNKNOWN_CHAR`. Instead, replace the placeholder tokens directly in the query string itself. It is still required to pass the query to `gh api graphql` via `-f query='...query text...'`.

## Fetch Review Thread Node IDs

Use this query to find the thread node ID for a given comment. Match the thread whose first comment has a `databaseId` equal to the target comment ID (or its parent comment ID if the target is a reply).

### First page

```graphql
query {
  repository(owner: "OWNER", name: "REPO") {
    pullRequest(number: PR_NUMBER) {
      reviewThreads(first: 100) {
        pageInfo {
          hasNextPage
          endCursor
        }
        nodes {
          id
          isResolved
          comments(first: 1) {
            nodes {
              databaseId
            }
          }
        }
      }
    }
  }
}
```

### Subsequent pages

If `pageInfo.hasNextPage` is `true`, re-run using this variant, replacing `END_CURSOR` with the raw `endCursor` string from the previous response (do not include the JSON quotes — the query template already provides the surrounding quotes). Repeat until the matching `databaseId` is found or all pages are exhausted.

```graphql
query {
  repository(owner: "OWNER", name: "REPO") {
    pullRequest(number: PR_NUMBER) {
      reviewThreads(first: 100, after: "END_CURSOR") {
        pageInfo {
          hasNextPage
          endCursor
        }
        nodes {
          id
          isResolved
          comments(first: 1) {
            nodes {
              databaseId
            }
          }
        }
      }
    }
  }
}
```

**Example invocation:**
```
gh api graphql -f query='query { repository(owner: "my-org", name: "my-repo") { pullRequest(number: 42) { reviewThreads(first: 100) { pageInfo { hasNextPage endCursor } nodes { id isResolved comments(first: 1) { nodes { databaseId } } } } } } }'
```

## Resolve a Review Thread

Once the thread node ID is known, resolve it with this mutation:

```graphql
mutation {
  resolveReviewThread(input: {threadId: "THREAD_ID"}) {
    thread {
      isResolved
    }
  }
}
```

**Example invocation:**
```
gh api graphql -f query='mutation { resolveReviewThread(input: {threadId: "PRRT_abc123"}) { thread { isResolved } } }'
```
