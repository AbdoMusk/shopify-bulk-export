# shopify-data-export

A package to help you bulk export data from Shopify's async [Bulk Operation API](https://shopify.dev/docs/api/usage/bulk-operations/queries).

### Usage

Install the package:

```sh
pnpm install shopify-bulk-export
```

Then, import the package and use the default exported function to run a query:

```ts
import bulkExport from 'shopify-bulk-export'

interface Product {
  id: string
  title: string
}

// The function accepts a generic, which will be the type of the nodes returned:
const data = await bulkExport<Product>({
  query: 'query Products { products { edges { node { id title } } } }',
  store: {
    accessToken: '',
    name: ''
  }
})
```

You can also pass a `TypedDocumentNode` query if you use the GraphQL Code Generator for example:

```ts
import bulkExport from 'shopify-bulk-export'
import { ProductsQueryDoc } from './generated'

const data = await bulkExport({
  query: ProductsQueryDoc,
  store: {
    accessToken: '',
    name: ''
  }
})
```

If your query has variables (for example a search query), you can pass these in and they will be replaced in your query:

```ts
import bulkExport from 'shopify-bulk-export'

const query = `<some-runtime-query>`

const data = await bulkExport({
  query: 'query Products ($query: String!) { products (query: $query) { edges { node { id title } } } }',
  store: {
    accessToken: '',
    name: ''
  },
  variables: {
    query
  }
})
```
