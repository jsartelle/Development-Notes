#todo look into [gap](https://developer.mozilla.org/en-US/docs/Web/CSS/gap)

#todo add info on [:has](https://developer.mozilla.org/en-US/docs/Web/CSS/:has)

#todo add stuff about [[The Future of CSS Cascade Layers (CSS @layer)|layers]]

# Animation shorthand

**duration** | **easing-function** | **delay** | **iteration-count** | **direction** | **fill-mode** | **play-state** | **name**

```css
animation: 3s ease-in 1s infinite reverse both running slidein;
```

# Attribute selectors

| Operator | Meaning                                                                           |
| -------- | --------------------------------------------------------------------------------- |
| ~=       | a whitespace-separated list of words, one of which is exactly *value*             |
| \|=      | can be exactly *value* or can begin with *value* immediately followed by a hyphen |
| ^=       | prefixed by *value*                                                               |
| $=       | suffixed by *value*                                                               |
| \*=      | contains *value*                                                                  |
| [... i]  | case insensitive                                                                  |

# Font weights

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
