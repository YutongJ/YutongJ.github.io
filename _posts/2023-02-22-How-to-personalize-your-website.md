---
title: 'Customize your personal website'
date: 2023-02-22
<!-- permalink: /posts/2012/08/blog-post-4/ -->
category: "How to build a website"
tags:
  - personal website
  - customize
  - html
---
<!-- This post will show up by default. To disable scheduling of future posts, edit `config.yml` and set `future: false`.  -->


I create my personal webpage using a very basic template from Jekyll by folking from [academicpages](https://academicpages.github.io/). It is a free service that we can host our personal webpage and store all data in a GitHub repository. Following the instructions on its website, we can reach the first stage - we are not nobody anymore!

However, I think there is still room to improve and personalize the website to make it feel like my own. I am going to walk you through some minor changes that I did for my website and will keep update this post if time allows. I would like to thank all the previous contributors and their innovative posts. In regards to the modifications I have mentioned below, a list of the most relevant posts is provided for your reference.


## How to change the favicon?

"Favicon" is a shortened expression for 'favorite icon', and a favicon is a icon displayed on the tab next to a particular webpage's address. Most websites usually use their own logo as a favicon. So for a personal webpage, we do want a logo! 

To have such a logo, here are the steps we need to do:

1. Prepare your favorite image.  
	- a simple icon is enough
	- remove its background color to make is compatible with both bright/dark mode (Otherwise, you will have a ugly white square background when view in dark mode).
2. Upload the image to [https://realfavicongenerator.net/](https://realfavicongenerator.net/), follow the instructions there to generate appropriate files accordingly.
3. Download and extract everything to a new folder named "favicon".
4. Copy this "favicon" folder to `/images/` folder.
5. Modify the html code in `_includes/head/custom.html` as follows:

- locate  `...` in `custom.html`:

```html
<!-- start custom head snippets -->
...
<link rel="stylesheet" href="{{ base_path }}/assets/css/academicons.css"/>
```

- we need to further modify `href=` by adding something before and after the current path copied directly from [https://realfavicongenerator.net/](https://realfavicongenerator.net/).
	- before: `{{ base_path }}/images/favicon/`
	- after: `?v=M44lzPylqQ` 
	
This is what my `_includes/head/custom.html` looks like:

``` html
<!-- start custom head snippets -->

<link rel="apple-touch-icon" sizes="180x180" href="{{ base_path }}/images/favicon/apple-touch-icon.png?v=M44lzPylqQ">
<link rel="icon" type="image/png" sizes="32x32" href="{{ base_path }}/images/favicon/favicon-32x32.png?v=M44lzPylqQ">
<link rel="icon" type="image/png" sizes="16x16" href="{{ base_path }}/images/favicon/favicon-16x16.png?v=M44lzPylqQ">
<link rel="manifest" href="{{ base_path }}/images/favicon/site.webmanifest?v=M44lzPylqQ">
<link rel="mask-icon" href="{{ base_path }}/images/favicon/safari-pinned-tab.svg?v=M44lzPylqQ" color="#5bbad5">
<meta name="msapplication-TileColor" content="#da532c">
<meta name="theme-color" content="#ffffff">

<link rel="stylesheet" href="{{ base_path }}/assets/css/academicons.css"/>

...
```




