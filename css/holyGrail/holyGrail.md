# Holy Grail

The Holy Grail layout is a famous CSS page layout that has been traditionally hard to implement.

It consists of a header, footer, and three columns. The left column contains navigation items, the middle column contains the page contents, and the right column contains ads

I should be able to implement the HolyGrail layout using just CSS. You shouldn’t need to change the HTML too much

# Requirements:

- Header
  - Stretches horizontally across the whole page
  - 60px tall
- Columns:
  - Both left & right columns have a fixed width of 100px;
  - The center column is fluid-width
  - All the columns should have the same height, regardless of which column is the tallest
- Footer
  - Stretches horizontally across the whole page
  - 100px tall
  - The footer should be at the bottom of the page even if there is not enough content to fill up the viewport height
- Code
  ```html
  <!DOCTYPE html>
  <html lang="en">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    </head>
    <body>
      <div id="root"></div>
    </body>
  </html>
  ```
  ```html
  body { font-family: sans-serif; font-size: 12px; font-weight: bold; margin: 0;
  } * { box-sizing: border-box; } header, nav, main, aside, footer { padding:
  12px; } header { background-color: tomato; } nav { background-color: coral; }
  main { background-color: moccasin; } aside { background-color: sandybrown; }
  footer { background-color: slategray; }
  ```
  ```tsx
  import "./styles.css";

  export default function App() {
    return (
      <>
        <header>Header</header>
        <div>
          <nav>Navigation</nav>
          <main>Main</main>
          <aside>Sidebar</aside>
        </div>
        <footer>Footer</footer>
      </>
    );
  }
  ```

# Thought Process

Ok there are several big things that you need to do to get this layout

### Setting up the flex container

First, you need to get the `#root` element in CSS and assign it the following in css:

```css
#root {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}
```

<aside>
💡 What is Flex?

Flex aims to provide a more efficient way to lay out, align, and distribute a space **within a container** even when their size is unknown and/or dynamic. Hence why it’s called “flex”.

The main idea behind the flex layout is to give the container the ability to alter its content’s height and width to best fill the available space (i.e. to accommodate different displays and screen sizes).

A flex container either expands items to fit the space or shrinks them to prevent overflow.

Most importantly, the flexbox layout is direction-agnostic as opposed to the regular layouts (block which is vertically-based and inline which is horizontally-based).

</aside>

So in our case, we want to organise our header, navbar, main content, ads bar, and footer into flex items that exist in the `#root` container. The direction (main-axis) defines the direction the flex items are placed in the container. The default is row but in our case we want columns (top to bottom)

Lastly, we give a minimum height of the entire view port (i.e. `min-height: 100vh`)

### Setting up the .columns class

Next, we want our navigation, main, and sidebar to be within one flex container

We wrap these three item into a larger container div with the `columns` class. We then set the columns display to flex. We want this columns container to expand as much as possible so we set it to `flex-grow: 1`

Next, we set the widths of the navigation and sidebar divs to 100px. We can then get Main to fill up the remaining space with `flex-grow: 1`

We need to add `flex-shrink: 0` to the nav and sidebar so that they don’t shrink when the content in main is too wide

```css
.columns {
  display: flex;
  flex-grow: 1;
}

main {
  flex-grow: 1;
}

nav {
  width: 100px;
  flex-shrink: 0;
}

aside {
  width: 100px;
  flex-shrink: 0;
}
```

### Footer

Since our .columns flexbox container grows, we need to set the height of the footer to 100px

We need to make it “sticky” (it should stick to the bottom of the screen when there is not enough content to fill the page).

This is covered in the “setting up the container” section where we set the `min-height` of the entire page to 100vh, set out the children to be in a vertical format, and add `display:flex` and `flex-direction: column`. We set the headers and footers to be a fixed height and then set the middle container to grow with `flex-grow: 1`

### Small stuff

Lastly, we need to just `text-align: center;` all the divs
