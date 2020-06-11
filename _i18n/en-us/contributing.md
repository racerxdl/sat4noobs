## Site Structure

This is a multi-language site from Jekyll. That means you can write language alternatives for your text. The default language for this tutorial is `pt-br` (Brazillian Portuguese)

New texts must be added in `_i18n/<language>/_posts` with the following name:

* **YYYY-MM-DD-title-without-spaces.md**
  * **YYYY** => Year with 4 digits
  * **MM** => Month with 2 digits
  * **DD** => Day with 2 digits
  * **title-without-spaces** => The title without spaces

The file should contain the following basic structure:

```markdown
---
title: <Title of your post, here it can have spaces!>
date: 2020-06-10T23:48:29-03:00
author: <Your name>
translated-by: <Your name, if you're translating>
layout: post
image: <Path to post presentation image>
tags:
  - <Tags to find your post>
---

Content
```

Where:

* The field delimited by `---` is the **header** where the metadata for construction of the page are.
  * **title** => Title of your post. Here you can put spaces!
  * **date** => Date / Time in ISO Format: `YYYY-MM-DDTHH:mm:ss-03:00`
    * **YYYY** => Year with 4 digits
    * **MM** => Month with 2 digits
    * **DD** => Day with 2 digits
    * **HH** => Hour with 2 digits
    * **mm** => Minutes with 2 digits
    * **ss** => Seconds with 2 digits
    * **-03:00** => Timezone (in this case, GMT-3)
  * **author** => Name of the author(s) of the post
  * **translated-by** => Name of the author(s) of the translation -- *Optional Field*
  * **layout** => Layout of the page (always use `post`!)
  * **image** => One image to present your post -- *Optional Field*
  * **tags** => List of keywords to help find your post over the internet! *Optional Field*
* **Conteúdo** => Markdown / HTML content of your post

### Images

If you want to add a image to your post, put it inside the folder `assets/posts/<title of your post>` and you will be able to link it with markdown or html:

##### Markdown
```markdown
![Image Description](/assets/posts/<title of your post>/<your image>.jpg)
```

##### HTML
```html
<img
  src="/assets/posts/<title of your post>/<your image>.jpg"
  alt="Image Description"
/>
```


## Testing the site locally

This is a [Jekyll](https://jekyllrb.com/) website! All normal instructions for jekyll applies for this tutorial. To run locally you need to have [Ruby](https://www.ruby-lang.org/) installed in your computer (which is by default for most Linux distributions and MacOSX)

```bash
# Install bundler and jekyll
gem install bundler jekyll

# Clone the repository
git clone https://github.com/racerxdl/sat4noobs
cd sat4noobs

# Install dependencies
bundle install

# Run the website
bundle exec jekyll serve --host 0.0.0.0 --watch
```

After that, you should see a messaged saying the site is ready for access:

```
Server address: http://0.0.0.0:4000
```

The site should be available in your browser with the address [http://localhost:4000](http://localhost:4000)
