# Layouts and Styling with Gatsby
In part 1 of this series, we started a new Gatsby Project, gave it basic styling and publish it to github pages.

In this part (part 2) we'll be using layouts to apply global structure to our site, we'll create complex components for better code reuse and use sass instead of css to apply global and local styling.

All the concept of this tuturial well build from what we did in part 1.

## Global styling
- Create `src/styles/global.css`
- Add some css to that file
- Create `gatsby-browser.js`
- Add `import "./src/styles/global.css` to that file
- Restart `gatsby develop`

## Component Scoped CSS
The tool automatically generates unique class and animation names, so you don’t have to worry about selector name collisions.

Create `src/components/container.module.css` (the `module` part is required)
Create `src/components/container.js`


