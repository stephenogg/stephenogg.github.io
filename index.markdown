---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
---

Text can be **bold**, _italic_, ~~strikethrough~~ or `keyword`.

[Link to another page](./another-page.html).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

## My Blog Posts
## <span class="cmd-prompt">user@stephenogg:~#</span> cat blog_posts.txt

{% if site.posts.size > 0 %}
  <ul class="terminal-list">
    {% for post in site.posts %}
      <li class="terminal-item">
        <span class="terminal-date">[{{ post.date | date: "%Y-%m-%d" }}]</span>
        <span class="terminal-arrow"> -> </span>
        <a class="terminal-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
{% else %}
  <p class="terminal-error"> [ERROR] No log files found in directory '_posts/'.</p>
{% endif %}

```
The final element.
```


<style>
  .cmd-prompt { color: #151515; background: #60ff60; padding: 0 4px; font-family: Monaco, Bitstream Vera Sans Mono, Lucida Console, Terminal, monospace; }
  .terminal-list { list-style-type: none; padding-left: 0; }
  .terminal-item { font-family: Monaco, Bitstream Vera Sans Mono, Lucida Console, Terminal, monospace; margin-bottom: 8px; }
  .terminal-date { color: #888; }
  .terminal-arrow { color: #ff6060; font-weight: bold; }
  .terminal-link { color: #60ff60 !important; text-decoration: none; border-bottom: 1px dashed #60ff60; }
  .terminal-link:hover { color: #fff !important; background: #222; }
  .terminal-error { color: #ff6060; font-family: monospace; }
</style>
