# apollo

Modern and minimalistic blog theme powered by [Zola](https://getzola.org). See a live preview [here](https://not-matthias.github.io/apollo).

<sub><sup>Named after the greek god of knowledge, wisdom and intellect</sup></sub>

<details open>
  <summary>Dark theme</summary>

  ![blog-dark](https://user-images.githubusercontent.com/26800596/168986771-4ed049e2-e123-4d0e-8a24-7bf43f47551f.png)
</details>

<details>
  <summary>Light theme</summary>

![blog-light](https://user-images.githubusercontent.com/26800596/168986766-72a48517-7122-465d-8108-3ae33e1e88b1.png)
</details>

## Features

- [X] Pagination
- [X] Themes (light, dark, auto)
- [X] Projects page
- [X] Analytics using [GoatCounter](https://www.goatcounter.com/)
- [x] Social Links
- [x] MathJax Rendering
- [x] Meta Tags For Individual Pages
- [ ] Search
- [ ] Categories

## Installation

1. Download the theme
```
git submodule add https://github.com/not-matthias/apollo themes/apollo
```

2. Add `theme = "apollo"` to your `config.toml`
3. Copy the example content

```
cp -r themes/apollo/content content
```

## Options

### Additional stylesheets

You can add stylesheets to override the theme:

```toml
[extra]
stylesheets = [
    "override.css",
    "something_else.css"
]
```

These filenames are relative to the root of the site. In this example, the two CSS files would be in the `static` folder.

### MathJax

To enable MathJax equation rendering, set the variable `mathjax` to `true` in
the `extra` section of your config.toml. Set `mathjax_dollar_inline_enable` to 
`true` to render inline math by surrounding them inside $..$.

```toml
[extra]
mathjax = true
mathjax_dollar_inline_enable = true
```

## Config

 ### Customize `<meta/>` tags 

 The following TOML and YAML code will yiled two `<meta/>` tags, `<meta property="og:title" content="the og title"/>`, `<meta property="og:description" content="the og description"/>`. 

 TOML: 

 ```toml
 title = "post title"
 description = "post desc"
 date = "2023-01-01"

 [extra]
 meta = [
     {property = "og:title", content = "the og title"},
     {property = "og:description", content = "the og description"},
 ]
 ```

 YAML: 

 ```yaml
 title: "post title"
 description: "post desc"
 date: "2023-01-01"
 extra: 
     meta: 
         - property: "og:title"
           content: "the og title"
         - property: "og:description"
           content: "the og description"
 ```

 If the `og:title`, the `og:description`, or the "description" are not set, the page's title and description will be used. That is, the following TOML code generates `<meta property="og:title" content="post title"/>`, `<meta property="og:description" content="post desc"/>`, and `<meta property="og:description" content="post desc"/>` as default values. 

 ```toml
 title = "post title"
 description = "post desc"
 date = "2023-01-01"
 ```

## References

This theme is based on [archie-zola](https://github.com/XXXMrG/archie-zola/).
