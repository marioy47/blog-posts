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



## Bootstrap
npm install --save boostrap gastby-plugin-sass node-sass
npm install --save gatsby-plugin-prefetch-google-fonts

```gastby-config.js
    `gatsby-plugin-sass`,
    {
      resolve: `gatsby-plugin-prefetch-google-fonts`,
      options: {
        fonts: [
          {
            family: "Fira Sans",
            variants: ["300", "400", "700"],
          },
        ],
      },
    },
```

## SASS


```/src/styles/_variables.js
$font-family-sans-serif: "Fira Sans", -apple-system, system-ui,
  BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
$font-family-serif: "Fira Sans", Georgia, "Times New Roman", Times, serif;

$pink: #f50057;
$danger: $pink;
$primary: #37474f;
$white: #fff;
$body-color: $primary;
$link-color: $primary;
$link-hover-color: none;

$font-size-base: 1.15rem;
$font-weight-base: 300;
$headings-font-weight: 700;
```

```layout.scss
@import "variables";
@import "~bootstrap/scss/bootstrap.scss";

// Type
a {
  border-bottom: 1px dotted $primary;
  text-decoration: none;

  &:hover {
    border-bottom: 1px solid $primary;
    text-decoration: none;
  }
}

// Header
.site-header {
  @extend .container;
  .wrap {
    @extend .row;
  }
}
```
