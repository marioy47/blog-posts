# Dynamic content on GatsbyJS

you have to edit the `gatsby-config.js` file and add the key `siteMetadata`

```js
module.exports = {
  siteMetadata: {
    title: "Personal blog using Gatsby",
    author: "Mario Yepes"
  }
};
```

Go to the URL `http://localhost:8000/___graphql` (is a triple \_)

Works like _Postman_ or _Insomnia_

Is autodocumente

- Queries (the only one we use)
- Notations
- Subscriptions

Click on the right **< DOCS** and then on _Query_ . Well se the complete list of availble queries

Click on `site`, the the `Type` definition

```graphql
query {
  site {
    siteMetadata {
      title
    }
  }
}
```

The structure of the response if defined by the strcuture of the query

Change the header component

```jsx
import { graphql, useStaticQuery} from "gatsby"
...

export default () => {
    const data = useStaticQuery(graphql`
        query { 
            site { 
                siteMetadata { 
                    title 
                }
            }
        }
    `)
    ...
    <Header content={data.site.siteMetadata.tile} />
    ...
}
```

### New interface for Graphql

Create `.env` file
Contents: `GATSBY_GRAPHQL_IDE=playground`
`npm install --save-dev env-cmd`
In `package.json` change the `script>development` for `env-cmd .env.development gatsby develop` 
And then `npm run develop`
Reload GraphyQL

