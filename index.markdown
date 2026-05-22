---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
---

### <span class="cmd-prompt">user@stephenogg:~#</span> cat blog_posts.txt

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
  .terminal-nav { margin: 20px 0; font-family: monospace; font-size: 1.1em; }
</style>
