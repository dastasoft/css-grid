# CSS Grid

CSS Grid brings **outside-in**, **two-dimensional** layouts to the web browser.

## Terminology

### Grid Container

- The element containing a grid, defined by setting `display: grid;`
- Subgrids are not supported.
- Embrace nested grids.
- Are defined using pure CSS.
- Can be nested inside media queries.
- Can be applied conditionally.
- Can be added, changed, or removed without affecting the HTML.
- Allows us to simplify HTML by reducing container nesting and separating semantic order from displayed order.

### Grid Item

- The element that is a direct descendant of the grid container.
- Second-level descendants need their own grid.
- Grid items adhere to strict columns and rows.
- Any grid item can be placed anywhere on the grid, this includes placing one item on top of another.
- Items are stacked according to the HTML source order.
- Items can be reordered using the z-index property.
- Manual placement is not impacted by the source order.
- Make sure the visible content order matches the source content order to preserve meaningful communication.

### Grid Line

- The horizontal (row) or vertical (column) line separating the grid into sections.
- Referenced by number, starting and ending with the other borers of the grid.
- Can also be named for easier reference.

### Grid Cell

- The intersection between a grid row and a grid column--effectively, the same as a table cell.

### Grid Track

- The space between two or more adjacent grid lines
- Row tracks -> horizontal
- Column tracks -> vertical

### Grid Area

- The rectangular area between four specified grid lines.
- Can cover one or more cells.

### Grid Gap

- The empty space between grid tracks.
- Commonly called a guter.

## Mesurement Units

Besides **px**, **rem**, **em** or **%** CSS Grid have two new mesurement units:

- Fraction (**fr**): The fr unit reppresents a fraction of the available space in the grid container.
- `minmax()`: The minmax function defines as the first value, the minimun measurement and as the scond value, the maximum value.
- `repeat()`: The repeat function repeats the provided pattern a specified number of times.

Let's put an example of the exact same grid, with the different mesurement units explained above:

```css
.my-grid-container {
    display: grid;
    grid-template-columns: 50% 50%;
    grid-template-columns: 1fr 1fr;
    grid-templat-columns: minmax(50%, 50%) minmax(50%, 50%)
    grid-template-columns: repeat(2, 1fr);
}
```

## Placing items on th Grid

What makes CSS Grid such a powerful layout tool is it allows us to specify where on the grid any of the grid itms should appear.

The most basic method for placing grid items on th grid is to declare the `grid-column` and `grid-row` values for each item.

```css
.my-grid-item {
    grid-column: 2/4;
    grid-row: 2/3;
}
```

The numbers represents the start and end lines of the grid.

### Implicit Lines

If grid item placement requires additional columns or rows to be created, the browser adds implicit lines to keep the grid structurally sound.

If you try to put some item outside of the grid the browser will generate the new lines (rows and columns) in order to work.

For example, with a grid of 4 by 4:

```css
.my-grid-item {
    grid-column: 1/4;
    grid-row: 4;
}
```

The `grid-column: 1/4;` will expand the item all across over the grid horizontally. For the `grid-row: 4;` you're telling the browser that the item will start on row 4 which is the last, then the browser needs to create a new implicit row.

### span Keyword

The span keyword is used to define how many grid track an element should span.

You can achieve the same result with this lines of code:

```css
.my-grid-item {
    grid-column: 1/4
    grid-column: 3 span;
}
```

The `grid-column: 3 span;` mean that starting from column 1 it will be expanded 3 additionals columns, exactly the same as from 1 to 4.

When using the `span` keyword if you don't tell the browser the starting point (like in the example above) it will start wherever the item is defined, you can define a custom start with `grid-column: 2/2 span;` this will place the item at the starting point of 2 and span 2 additional columns.

## Naming lines

You can put custom names to the lines and place items using that names instead of the number lines.

```css
.my-grid-container {
    display: grid;
    grid-template-columns: [main-start] 2fr [main-end] 1fr [sidebar-start] 1fr [sidebar-end];
}

.my-grid-item {
    grid-column: 2/4;
    grid-column: main-end/sidebar-end;
}
```

In the `my-grid-item` class the two `grid-column` values are equivalent now.

### Shorthand naming

In the example above you can see two different "areas" the `main` area and the `sidebar` area, given the names with `-` we can use a shorthand naming convention when we want to cover the all area.

The following examples generates exactly the same effects:

```css
.my-grid-container {
    display: grid;
    grid-template-columns: [main-start] 2fr [main-end] 1fr [sidebar-start] 1fr [sidebar-end];
}

.my-grid-item {
  grid-column: 2/4
  grid-column: main-end/sidebar-end;
  grid-column: main-end/sidebar;
}

.my-grid-item2{
  grid-column: 1/4;
  grid-column: main-start/sidebar-end;
  grid-column: main/sidebar;
}

.my-grid-item3 {
  grid-column: 1/2;
  grid-column: main-start/main-end;
  grid-column: main;
}
```

## Grid Areas

If the naming lines shorthand was like a workaround of getting some kind of areas, now let's get into the real areas property `grid-template-areas`.

The `grid-template-areas` is applied to a grid container. Grid areas allows us to define rectangular areas spanning one or more cells within the grid, and give them names, later we can use those names to place items within the grid. With this we will turn the grid into a map.

```css
.my-grid-container {
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-areas:
        'title title title'
        'main masthead masthead'
        'main sidebar footer';
}


.masthead {
  grid-area: masthead;
}

.page-title {
  grid-area: title;
}

.main-content {
  grid-area: main;
}

.sidebar {
  grid-area: sidebar;
}

.footer-content {
  grid-area: footer;
}
```

So now, we can just play with the area, without changing anything from the items and have a real responsive design:

```css
.site {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto 1fr 3fr;
  grid-template-areas:
    'title title'
    'main masthead '
    'main sidebar'
    'footer footer';
}

@media screen and (min-width: 600px) {
  .site {
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-areas:
      'title title title'
      'main masthead masthead'
      'main sidebar footer';
  }
}

.masthead {
  grid-area: masthead;
}

.page-title {
  grid-area: title;
}

.main-content {
  grid-area: main;
}

.sidebar {
  grid-area: sidebar;
}

.footer-content {
  grid-area: footer;
}
```

## Grid Gap example

You can define a "margin" between columns and rows.

```css
.my-grid-container {
    display: grid;
    grid-gap: 1em 2em;
    grid-row-gap: 1em;
    grid-colum-gap: 2em;
}
```

The `grid-gap` is a shorthand for `grid-row-gap` and `grid-column-gap`.

## Check browser compatibility

In order to check if the browser supports CSS Grids add the following code to your CSS file:

```css
@supports (grid-area: auto) {
  /* Grid classes here */
}
```

This will test if the browser supports `grid-area` property.

Don't use supports with `display: grid` because older browsers like IE10 and IE11 have legacy support of an older spec of CSS Grids and may not all features you write be present, `grid-area` instead it is a part of the new spec.

## EXTRA: Design CSS Grid Layouts

- Start with pen and paper
- Make mobile-first layout drafts to map out the relationships between items.
- Draw grid lines to identify columns, rows, cells, and grid areas.
- Break grid apart to identify where nesting is needed.
- Repeat the process for each content model/view.
