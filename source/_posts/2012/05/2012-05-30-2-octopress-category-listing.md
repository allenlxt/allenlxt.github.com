---
layout: post
title: "Octopress Category Listing"
date: 2012-05-30 16:18
comments: true
categories: octopress
---

While setting up my side bar for this site I wanted to easily list all
categories from all posts. Unfortunately, this is non-trivial from what I learned by
skimming the Octopress, Jekyll, and Liquid docs. Fortunately, there is an easy
fix. Here are the diffs of the changes I made:

{% include_code lang:diff category_generator.rb.diff %}

{% include_code lang:diff recent_posts.html.diff %}

This allows the `category_links` liquid filter to process the
`site.categories` hash. The line added to `category_generator.rb` file gets
the keys of `site.categories` which corresponds to the names of every
category.
