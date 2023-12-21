---
title: "Advent of Supercomputing: Day 7"
date: 2023-12-07
publishDate: 2023-12-07T08:00:00Z
author: ["yuchen"]
draft: false
---

# mdBook

I keep my notes in Markdown.  Markdown is great!  It's a simple plain text format, yet you can use
most of the _basic_ **formatting** that you **_need_**.  (Did I say that this article is also
written in Markdown?)  But the problem with taking notes in plain text is that although it's
intuitive to read even in source form, it's not the best presentation format; you need a tool to
convert the Markdown files to HTML and/or PDF, so you can have the eye candy to others to look at.

## What are our options?

VS Code can print to PNG and PDF, but this means spinning up a heavyweight tool to do any rendering.
I also had troubles opening multiple VS Code in different previews; the second preview would always
break for me.

`pandoc` is the recommended tool to convert between all sorts of different document formats.  But
it doesn't look nice by default, nor is there a simple way of structuring and organizing multiple
Markdown files.
e
## So, how does mdBook help us?

> mdBook is a command line tool to create books with Markdown. It is ideal for creating product or
> API documentation, tutorials, course materials or anything that requires a clean, easily navigable
> and customizable presentation.

Take a look for yourself at the [mdBook Documentation](https://rust-lang.github.io/mdBook/)!
<iframe width="100%" src="https://rust-lang.github.io/mdBook/" style="height: 100vh; display: none;"
onload="this.style.display='block';"></iframe>

The installation is simple, and you can follow through the linked documentation.


## How do I use it?

```
mdbook init my-first-book
cd my-first-book
mdbook serve --open
```

This will make a new book and open the rendered page in your web browser and will automatically
rebuild on any edits.  I also found that using the print button would give great results too even if
you only want a PDF!

## Books written with mdBook

- [mdBook Documentation](https://rust-lang.github.io/mdBook/)
- [Helix Documentation](https://docs.helix-editor.com/)
- [The Rust Programming Language](https://doc.rust-lang.org/book/)
- [Learn Rust With Entirely Too Many Linked Lists](https://rust-unofficial.github.io/too-many-lists/)

And you can find a bigger list here: [awesome mdbooks](https://github.com/softprops/awesome-mdbook)
