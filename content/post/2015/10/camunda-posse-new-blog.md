---
title: "A new blog for Camunda BPM posse"
date: "2015-10-14T13:28:00+02:00"
author: "Valentin Vago"

categories:
  - "Release"

tags:
  - Blog
  - Camunda
  - Design
  - Hugo
---

I was delighted when [Daniel](//twitter.com/meyerdan) asked me to *valify* (as Robert says) the **Camunda BPM Team Blog**
and make it fit our **CI** (nope, for once, this ain't meaning Continuous Integration). Here's the little story about that work… erm… fun and some showoff about the new blog features.
<!--more-->

## Who, nowadways, wants to write HTML?

*None of my colleagues, that's pretty much a fact.*

And since we rebuild our [documentation website](//docs.camunda.org/latest) using [Hugo](//gohugo.io)
and liked it, it seemed legit to continue with that stack.

## One cannot just drop the older posts

*That's kind of obvious.*

So how would you import the posts written on blogger?  
The nerd would probably start writing a client which would pump the posts using the blogger API and stuff.

I'm more of a creative guy. Which means that I don't make a fuss about the way,
I want to reach my goals (which, by the way, might not always be the {{< bpmn-icon name="end-event-none" >}},
a bit like when Jakob put a `style` attribute in its HTML).  
I went to the Google API console and got myself all the posts from there. Tada.

After that, it's piece of cake, a [grunt task](//gruntjs.com/api/grunt.task) consumes the JSON and
creates the mardown files and it looks more or less like that:

```js
grunt.registerTask('import', function () {
  var jsonContent = grunt.file.readJSON('./camunda-blog-posts.json');
  var postTemplate = [
    '---',
    'title: "<%= title.split(\'\\"\').join(\'\\\\"\') %>"',
    'date: "<%= published %>"',
    'author: "<%= author.displayName %>"',
    '',
    'categories:',
    '  - "<%= category %>"',
    'tags: <%= labels.map(function (tag) { return \'\\n  - "\'+ tag +\'"\'; }).join(\'\') %>',
    '',
    'aliases:',
    '  - "<%= alias %>"',
    '',
    '---',
    '',
    '<%= content %>'
  ].join('\n');

  jsonContent.items.forEach(function (post) {
    var nameParts = post.url.split('/');
    var name = 'content/post/' + nameParts[3] + '/' + nameParts[4] + '/' + nameParts[5].split('.html')[0] + '.md';

    post.category = post.title.toLowerCase().indexOf('release') < 0 ? 'Development' : 'Release';
    post.labels = post.labels || [];
    post.alias = post.url.split('blog.camunda.org').pop();

    post.content
      .split('href="http://blog.camunda.org/')
      .join('href="/')
      .split('href="http://camundabpm.blogspot.de/')
      .join('href="/')
    ;

    grunt.file.write(name, grunt.template.process(postTemplate, {data: post}));
  });

});
```

## New features?

*Sure, got plenty.*

### BPMN diagrams

Based on [Hugo shortcodes](http://gohugo.io) and [bpmn.io](http://bpmn.io), authors can now add their diagrams in a post in a breeze like that:

```
{{</* bpmn-viewer path="/2015/10/order-process" */>}}
```

And it renders something like that when the :

{{< bpmn-viewer path="/2015/10/order-process" >}}



### BPMN symbols

Also base on Hugo shortcodes, you can add all the symbols contained in the [bpmn-font project](//github.com/bpmn-io/bpmn-font).

```
{{</* bpmn-icon name="end-event-none" */>}}
```
produces

{{< bpmn-icon name="end-event-none" >}}



### Code highlighting

You saw it above in action :)

### Emoji

Na. Not yet

### Responsive layout

Yep. And soon responsive youtube video player.
