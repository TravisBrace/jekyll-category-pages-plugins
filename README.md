# Jekyll Category Pages Plugin

This Jekyll plugin generates category pages for your collections based on the categories specified in the front matter of each document. It allows you to create dynamic category pages for different collections, customize their layouts, and set SEO metadata like title tags and descriptions.

## Installation

1. Copy the `category_pages_generator.rb` file to your Jekyll project's `_plugins` directory. If you don't have a `_plugins` directory, create one in your project's root directory.

## Usage

To use the plugin, follow these steps:

1. Define categories in the front matter of your documents. You can either use a single category or an array of categories.

Example:

```
---
category: First Category
categories:
  - Second Category
  - Third Category
---
```

2. In your `_config.yml`, add a `category_pages` key with the following settings for each collection you want to generate category pages:

- `collection`: The name of the collection (without the underscore).
- `url_structure`: The URL structure for the category pages. Use `:category` as a placeholder for the category slug.
- `layout`: The name of the layout file to use for the category pages (without the file extension).
- `seo_title`: The title tag template. Use `:category` as a placeholder for the category name.
- `seo_description`: The description tag template. Use `:category` as a placeholder for the category name.

Example:

```
category_pages:
  - collection: blogposts
    url_structure: "/blog/:category/"
    layout: blogcategoryindex
    seo_title: ":category Blog Posts | My Website"
    seo_description: "Read our latest blog posts about :category on My Website."
  - collection: thingamajigs
    url_structure: "/:category/"
    layout: thingamajigcategory
    seo_title: ":category Thingamajigs | My Website"
    seo_description: "Meet our talented :category thingamajigs at My Website."
```

3. Create the specified layout files for your category pages in your `_layouts` directory, and include the necessary HTML and Liquid code.

Example for `_layouts/blogcategoryindex.html`:

```
---
layout: default
---

{% assign category = page.category %}

<h1>{{ category }} Blog Posts</h1>

{% for post in site.blogposts %}
  {% assign post_categories = post.category | default: post.categories %}
  {% if post_categories contains category %}
    <article>
      <h2>{{ post.title }}</h2>
      <p>{{ post.date | date: "%B %d, %Y" }}</p>
      <p>{{ post.excerpt | strip_html }}</p>
      <a href="{{ post.url }}">Read More</a>
    </article>
  {% endif %}
{% endfor %}
```

With these steps, the plugin will generate category pages for the specified collections based on the categories defined in the front matter of each document.

## Displaying a list of categories and their URLs

To display a list of categories for a specific collection, you can use the following Liquid code in your template:

```
{% assign collection_categories = "" | split: "" %}

{% for item in site.<your_collection_name> %}
  {% assign item_categories = item.category | default: item.categories %}
  {% for category in item_categories %}
    {% unless collection_categories contains category %}
      {% assign collection_categories = collection_categories | push: category %}
    {% endunless %}
  {% endfor %}
{% endfor %}

{% assign collection_categories = collection_categories | sort %}

<ul>
{% for category in collection_categories %}
  {% assign category_slug = category | slugify %}
  {% assign category_url = site.category_pages | where: "collection", "<your_collection_name>" | where: "category", category | first %}
  <li>
    <a href="{{ category_url.url }}">{{ category }}</a>
  </li>
{% endfor %}
</ul>
```

Replace `<your_collection_name>` with the name of your desired collection (e.g., `blogposts` or `thingamajigs`).

This Liquid code snippet will generate a sorted list of unique categories for the specified collection and create links to their respective category pages.

## Note

This plugin assumes that you are using Jekyll's default permalink style. If you have a custom permalink style set in your `_config.yml`, make sure to update the `url_structure` setting accordingly in the `category_pages` configuration.

And that's it! You should now be able to use the custom category plugin to generate dynamic category pages for different collections.
