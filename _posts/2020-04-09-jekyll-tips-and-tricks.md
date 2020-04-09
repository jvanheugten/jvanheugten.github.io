---
layout: post
title: Jekyll Tips and Tricks
image: https://lh3.googleusercontent.com/HdphyRMlxBxO1ODIFiYPI8OjjtEQBsc2MUfwL5OC0msX84uje3rJ4Zed-zJXT_VkvfdudF5oMp44wF3swWR3zuiPWrAMBDcnRmDjRl_lvkq4837xmmjXPnl0N2z9jNWHlR7uz4W2fcs3qhsQrPFTu8zLbp36kflEaf7yJ8L6lp71GtyWG3Anu3_Ct7oVzEJHKZ5rKYhBoGA8ap_nq9eJbkxD0TSdyms7_07WQojQuh2ocK25pSb-ohYD-s6MNNteve0n8xCHyE-qspkKZrMZai4j_p1ExP_XcqSz4GYaISqiba9ZokmNoLzfY509Jh92pxWFHrzQCraBldCRHvD9AX48JIK-guw1WScsqZzz-NOi0lDfj2p9M1rtwF-BbhGm71_mZNGaxwrhPPUUVeEigMnNqOVRzBsxp_ZIHs_N1ujJbq1arBeDy61nzg4C8CDVIeM0DNRTLmOPwXM3GmuJpM_fNV0w2ZlMYssFg_RuA7Zm7KpFXzjzqSRc6PBZGtB7IskewK-lgY6lAAUDRHl3SJsrsDQ1YLrK-Jr-5d7a_xGDJgn9JQ5PH12RK-wrFTIXUZ-xdZ-cGgFKFkfvoOHtY4gPEK9nXaDz-ZMB0wPWmcQXrvjwlxFC9Q8-kw23kcZ4GkHMk5Dgb2avcXwpEjVYvRwH6QSO0YCjGmo3UxpK4x2xgoyf_m9_d3fUi1_pxiyoulD9e6mRcus7VkgqVpBRQdCT6g=w960-h489-no
categories: [Web]
tags: [Jekyll]
---

## Write raw Liquid (Jekyll syntax) in posts
To blog about Jekyll's Liquid syntax without it being parsed, use the `raw` and `endraw` tags.  
Reference: 
- [https://shopify.github.io/liquid/tags/raw/]()

## Rendering Latex Math in posts
Latex Math is by default not rendered in Jekyll. To add MathJax support in Jekyll posts, create the following in `_includes/scripts.html`:

```html
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ TeX: { equationNumbers: { autoNumber: "all" } } }); </script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [ ['$','$'], ["\\(","\\)"] ],
        processEscapes: true
    }
});
</script>
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

Then include the scripts at the top of the necessary page templates, such as `_layouts/post.html`:
```html
{% raw %}{% include scripts.html %}{% endraw %}
```

There are some quirks with rendering math in Jekyll  as it parses the markdown also for Jekyll scripts.
For example:
- `${y}_x$` will not render the subscript x, however, `${y} _x$` will
- "`|`" can't be used, however, `\lVert` and `\rVert` should be used

## Post images
To easily add an image to the top of posts, add the following to `post.html` before `content`:

```html
{% raw %}{\%- if page.image -%}
<div style="text-align:center">
    <img src="{{ page.image | prepend: site.baseurl }}" alt="{{ page.title }}" title="{{ page.title }}" style="border-radius: {{ page.image-border-radius | default: 0 }}%">
</div>
{\%- endif -%}{% endraw %}
```

Images can now be added by specifying their link in the post metadata: `image: http://link-to-my-image`.  
It is also easy to add further customization options, such as the optional `image-border-radius: 20`.

The images will also show up on your homepage if you add the same line to `_layouts/home.html` after the `post.title` block with all `page`'s replaced by `post`.

## Embedding responsive YouTube videos in posts
First, create a YouTube embed script in `_includes/youtubePlayer.html`: 
```html
<div style="text-align:center">
    <iframe src="https://www.youtube.com/embed/{{ include.id }}" frameborder="0" allowfullscreen></iframe>
    <br>
</div>
```
Second, add the following to `_sass/custom.scss`:
```css
.embed-container { 
	position: relative; 
	padding-bottom: 56.25%; 
	height: 0; 
	overflow: hidden; 
	max-width: 100%;
	text-align: center
} 

.embed-container iframe, .embed-container object, .embed-container embed { 
	position: absolute; 
	top: 0; 
	left: 0; 
	width: 100%; 
	height: 100%; 
}
```
Finally, to embed a YouTube video in a post write:
```html
{% raw %}{% include youtubePlayer.html id="tvTRZJ-4EyI" %}{% endraw %}
```

## "Read more" link on the homepage
To add a "Read more" link if a post excerpt is not full content, add the following to `_layouts/home.html` after the `site.show_excerpts` block:
```html
{% raw %}{% if post.excerpt != post.content %}
    <a href="{{ site.baseurl }}{{ post.url }}">Read more</a>
{% endif %}{% endraw %}
```

# References
- [https://muffinman.io/jekyll-read-more-link/]()