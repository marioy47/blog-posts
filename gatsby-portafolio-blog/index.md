# Create a Portfolio/Blog Site with Gatsby

```bash
mkdir blog-site
npx gatsby new .
code --install-extension esbenp.prettier-vscode
npm run build
python -m SimpleHTTPServer 9000
```

## Config
Edit `gatsby-config.js`

- Edit siteMetadata.title
- Edit siteMetadata.description
- Config `gatsby-plugin-manifest` para que funcione como PWA
  - name
  - shortname

```gastby-config.js
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `mario-yepes-portfolio`,
        short_name: `starter`,
        start_url: `/`,
        background_color: `#f50057`,
        theme_color: `#f50057`,
        display: `minimal-ui`,
        icon: `src/images/svg/icon-developer-round.svg`, // This path is relative to the root of the site.
      },
    },
```

## SASS
npm install --save boostrap gastby-plugin-sass node-sass
npm install --save gatsby-plugin-google-fonts

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

