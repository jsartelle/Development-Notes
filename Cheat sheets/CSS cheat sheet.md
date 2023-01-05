#todo write about [[The Future of CSS Cascade Layers (CSS @layer)|layers]]

# Selectors

## :has

- lets you target elements based on their children or subsequent elements

```css
div:has(+ span) {
    /* matches divs immediately followed by a span */
}

div:has(> span) {
    /* matches divs containing a span */
}
```

## :is

- matches any element that can be matched by any of its arguments
- takes the specificity of its most specific argument
- invalid arguments are ignored

```css
:is(ol, ul) :is(ol, ul, :banana /* invalid selector is ignored */) {
    /* matches any nested list, ordered or unordered */
}
```

## :where

- same as [[#:is]] but with specificity 0

## Attribute selectors

| Operator | Meaning                                                                           |
| -------- | --------------------------------------------------------------------------------- |
| ~=       | a whitespace-separated list of words, one of which is exactly *value*             |
| \|=      | can be exactly *value* or can begin with *value* immediately followed by a hyphen |
| ^=       | prefixed by *value*                                                               |
| $=       | suffixed by *value*                                                               |
| \*=      | contains *value*                                                                  |
| [... i]  | case insensitive                                                                  |

# Properties

## gap

- sets spacing between rows and columns in flex or grid views

```css
/* first value is gap between rows, second is gap between columns */
/* if only one value is given, it applies to both */
gap: 10px 5px;
```

## animation (shorthand)

**duration** | **easing-function** | **delay** | **iteration-count** | **direction** | **fill-mode** | **play-state** | **name**

```css
animation: 3s ease-in 1s infinite reverse both running slidein;
```

# Other

## Font weights

| Value | Common weight name        |
| ----- | ------------------------- |
| 100   | Thin (Hairline)           |
| 200   | Extra Light (Ultra Light) |
| 300   | Light                     |
| 400   | Normal (Regular)          |
| 500   | Medium                    |
| 600   | Semi Bold (Demi Bold)     |
| 700   | Bold                      |
| 800   | Extra Bold (Ultra Bold)   |
| 900   | Black (Heavy)             |
| 950   | Extra Black (Ultra Black) |

# See also

- [[Useful nth-child Recipes]]
- [[The Future of CSS Cascade Layers (CSS @layer)]]
