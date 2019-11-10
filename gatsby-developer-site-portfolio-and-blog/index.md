---
title: Create a Developer Portfolio Site (with a blog) with Gatsby
date: 2019-11-10
tags: gatsby, react, javascript, graphql, npx
---

# Create a Developer Portfolio Site (with a blog) with Gatsby

Creating a [Gatsby](https://gatsbyjs.org) site is not an easy task even though the creators of it say it is.

It requires you to be familiar with a bunch of technologies like [graphql](https://developer.facebook.com/graphql), [React](https://reactjs.org), [Markdown](https://jamstack.org), etc. And then you need to research and test a bunch of plugins to get a minimal site up and running.

But after you pass the first configuration and you have your site configured, you'll have the more performant site you'll ever create. So the advantages compensate the drawbacks.

Here I'll list the steps I took to create my personal blog and portfolio site without going too much into detail since Gatsby already has a [great tutorial](https://gastbyjs.org/tutorial) and the [docs](https://gastbyjs.org/docs) are exceptional.

## Create the site itself

This part is actually easy since its just executing a couple of commands

```shell
mkdir blog-site
cd blog-site
npx gatsby new .
code --install-extension esbenp.prettier-vscode
npm start
```

If you know a little about Gatsby, notice that I didn't provided an starter site. That means that I'm using the default one.

If you want to build an take a peek at what will be deployed, then issue the `build` command and open a local https server:

```shell
npm run build
cd build
python -m SimpleHTTPServer 9000
```

## Configure the blog metadata to make it more SEO friendly

Edit `gatsby-config.js` file and change the metadata. Basically change:

- The `siteMetadata.title`
- The `siteMetadata.description`
- The `gatsby-plugin-manifest` so it works as a PWA
  - Add a `name`
  - Add a `shortname`

```gastby-config.js
{
    //...
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `mario-yepes-portfolio`,
        short_name: `portfolio`,
        start_url: `/`,
        background_color: `#f50057`,
        theme_color: `#f50057`,
        display: `minimal-ui`,
        icon: `src/images/svg/icon-developer-round.svg`, // This path is relative to the root of the site.
      },
    },
}
```

## SASS

```sass-and-fonts.sh
npm install --save boostrap gastby-plugin-sass node-sass
npm install --save gatsby-plugin-google-fonts
```

```gatsby-config.js
    `gatsby-plugin-sass`,
    {
      resolve: `gatsby-plugin-google-fonts`,
      options: {
        fonts: [`Fira Sans\:300, 400,700`],
        display: "swap",
      },
    },
```

## SVG embed

    npm install --save gatsby-plugin-react-svg

```gatsby-config.js
    {
      resolve: "gatsby-plugin-react-svg",
      options: {
        rule: {
          include: /images\/svg\/.*svg$/,
        },
      },
    },
```

## Split Layout in Header and Footer

```src/components/header.js
import { useStaticQuery, graphql, Link } from "gatsby"
import React from "react"

import DeveloperIcon from "../images/svg/icon-developer-round.svg"
import styles from "../styles/header.module.scss"

const Header = () => {
  const query = useStaticQuery(graphql`
    {
      site {
        meta: siteMetadata {
          author
          title
        }
      }
    }
  `)
  return (
    <header className={styles.siteHeader}>
      <nav>
        <h4>
          <Link to="/">
            <DeveloperIcon />
          </Link>
        </h4>
        <div>
          <ul>
            <li>
              <Link to="portfolio">Portfolio</Link>
            </li>
            <li>
              <Link to="blog">Blog</Link>
            </li>
            <li>
              <Link to="about">About</Link>
            </li>
          </ul>
        </div>
      </nav>
    </header>
  )
}

export default Header
```

```src/components/footer.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby"

import GithubIcon from "../images/svg/github-brands.svg"
import TwitterIcon from "../images/svg/twitter-brands.svg"
import LinkedinIcon from "../images/svg/linkedin-brands.svg"

import styles from "../styles/footer.module.scss"

const Footer = () => {
  const data = useStaticQuery(graphql`
    query MetaQuery {
      site {
        meta: siteMetadata {
          author
        }
      }
    }
  `)

  return (
    <footer>
      <div>
        <div>
          <div>
            © {new Date().getFullYear()}, {data.site.meta.author}
          </div>
          <div>
            <nav>
              <ul>
                <li>
                  <a href="#">
                    <GithubIcon class={styles.footerIcon} />
                  </a>
                </li>
                <li>
                  <a href="#">
                    <TwitterIcon />
                  </a>
                </li>
                <li>
                  <a href="#">
                    <LinkedinIcon />
                  </a>
                </li>
              </ul>
            </nav>
          </div>
        </div>
      </div>
    </footer>
  )
}

export default Footer
```

```src/components/layout.js
import React from "react"

import Header from "./header"
import Footer from "./footer"
import "../styles/layout.scss"

const Layout = ({ children, className }) => {
  return (
    <div className={className}>
      <Header />
      <div className="site-inner">{children}</div>
      <Footer />
    </div>
  )
}

export default Layout

```

## Remark

`npm install --save gatsby-transformer-remark`

```gatsby-config.js
    `gatsby-transformer-remark`
```

```src/pages/blog.js
import React from "react"
import Layout from "../components/layout"
import SEO from "../components/seo"

const BlogPage = ({ data }) => {
  return (
    <Layout>
      <SEO title="Some notes and articles" />
      <h1>Blog</h1>
      {data.posts.edges.map(data => (
        <div className="entry">
          <header>
            <h2 className="entry-title" itemprop="headline">
              {data.node.frontmatter.title}
            </h2>
            <time itemprop="datePublished" datetime="2019">
              {data.node.frontmatter.dateString}
            </time>
          </header>
        </div>
      ))}
    </Layout>
  )
}

export const query = graphql`
  {
    posts: allMarkdownRemark(
      sort: { fields: [frontmatter___date], order: DESC }
    ) {
      edges {
        node {
          frontmatter {
            title
            tags
            dateString: date(formatString: "YYYY-mm")
            date
          }
        }
      }
    }
  }
`

export default BlogPage
```

### Add new field (slug) to markdown files

The posts are in `/src/posts` and each post is in a dir:

- src/posts/my-first-post/index.md
- src/posts/second-post/index.md

```gastsy-node.js
const { createFilePath } = require("gatsby-source-filesystem")
const path = require("path")

exports.onCreateNode = ({ node, getNode, actions }) => {
  // Create the blog posts
  if (node.internal.type === `MarkdownRemark`) {
    const relPath = createFilePath({ node, getNode, basePath: "posts" })
    actions.createNodeField({
      node,
      name: `slug`,
      value: path.dirname(relPath),
    })
  }
}
```

Now update the `src/pages/blog.js` to add the slug on each item

## Create the blog posts programmatically

Until now we just added the slug to the `MarkdownReamark` nodes. We still have to create those pages but programatically. so we wont need to creat a `.js` file for each blogpost

```gatsby-node.js
// ...
exports.createPages = async ({ graphql, actions }) => {
  const result = await graphql(`
    query PostsQuery {
      posts: allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  result.data.posts.edges.forEach(({ node }) => {
    actions.createPage({
      path: node.fields.slug,
      component: path.resolve("./src/components/posts-template.js"),
      context: {
        slug: node.fields.slug,
      },
    })
  })
}
```

```src/blog.js
import React from "react"
import { Link, graphql } from "gatsby"

import Layout from "../components/layout"
import SEO from "../components/seo"

const BlogPage = ({ data }) => {
  return (
    <Layout>
      <SEO title="Some notes and articles" />
      <h1>Blog</h1>
      {data.posts.edges.map(({ node }) => (
        <div className="entry">
          {console.log(node)}
          <header>
            <Link to={node.fields.slug}>
              <h2 className="entry-title" itemprop="headline">
                {node.frontmatter.title}
              </h2>
            </Link>
            <time
              itemprop="datePublished"
              datetime={node.frontmatter.dateString}
            >
              {node.frontmatter.dateString}
            </time>
          </header>
        </div>
      ))}
    </Layout>
  )
}

export const query = graphql`
  {
    posts: allMarkdownRemark(
      sort: { fields: [frontmatter___date], order: DESC }
    ) {
      edges {
        node {
          frontmatter {
            title
            tags
            dateString: date(formatString: "MM/DD, YYYY")
            date
          }
          fields {
            slug
          }
        }
      }
    }
  }
`

export default BlogPage

```

### Add images to blog posts

```shell
npm install --save gatsby-remark-images
```

```
{
  //...
    // `gatsby-transformer-remark`,
    {
      resolve: `gatsby-transformer-remark`,
      options: {
        plugins: [
          {
            resolve: `gatsby-remark-images`,
            options: {
              maxWidth: 1024,
              linkImagesToOriginal: false,
              withWebp: true,
            },
          },
        ],
      },
    },
  // ...
}

