# Auto-convert Jupyter Notebooks To Posts

[`fastpages`](https://github.com/fastai/fastpages) will automatically convert [Jupyter](https://jupyter.org/) Notebooks saved into this directory as blog posts!

You must save your notebook with the naming convention `YYYY-MM-DD-*.ipynb`.  Examples of valid filenames are:

```shell
2020-01-28-My-First-Post.ipynb
2012-09-12-how-to-write-a-blog.ipynb
```

If you fail to name your file correctly, `fastpages` will automatically attempt to fix the problem by prepending the last modified date of your notebook. However, it is recommended that you name your files properly yourself for more transparency.

See [Writing Blog Posts With Jupyter](https://github.com/fastai/fastpages#writing-blog-posts-with-jupyter) for more details.

## Notes

In a notebook, front matter is defined as a markdown cell at the beginning of the notebook with the following contents:

  ```markdown
  # "Title"
  > "Awesome summary"

  - toc: false
  - branch: master
  - badges: true
  - comments: true
  - categories: [fastpages, jupyter]
  - image: images/some_folder/your_image.png
  - hide: false
  - search_exclude: true
  - metadata_key1: metadata_value1
  - metadata_key2: metadata_value2
  ```
  
Similarly, in a markdown document the same front matter would be defined like this at the beginning of the document:

  ```yaml
  ---
  title: "My Title"
  description: "Awesome description"
  layout: post
  toc: false
  comments: true
  image: images/some_folder/your_image.png
  hide: false
  search_exclude: true
  categories: [fastpages, jupyter]
  metadata_key1: metadata_value1
  metadata_key2: metadata_value2
  ---
  ```

Additional metadata is optional and allows you to set custom [front matter](https://jekyllrb.com/docs/front-matter/).  

Note that anything defined in front matter must be valid YAML.  **Failure to provide valid YAML could result in your page not rendering** in your blog.  For example, if you want a colon in your title you must escape it with double quotes like this:

` - title: "Deep learning: A tutorial"`

See this [tutorial on YAML](https://rollout.io/blog/yaml-tutorial-everything-you-need-get-started/) for more information.

### Configure Title & Summary
  - Replace `Title`, with your desired title, and `Awesome summary` with your desired summary.

**Note:** It is recommended to enclose these values in double quotes, so that you can escape colons and other characters that may break the YAML parser.

### Table of Contents
  - `fast_template` will automatically generate a table of contents for you based on [markdown headers](https://guides.github.com/features/mastering-markdown/)!  You can toggle this feature on or off by setting `toc:` to either `true` or `false`.

### Colab, Binder and GitHub Badges

This option works for **notebooks only**

  -  The `branch` field is used to optionally render a link your notebook to Colab and GitHub in your blog post post. It'll default to `master` if you don't specify it in the notebook.
  - If you do not want to show Colab / GitHub badges on your blog post (perhaps because your repo is private and the links would be broken) set `badges` to `false`.  This defaults to `true`
  - By default, when you omit this parameter from your front matter, or you set `badges: true`, **all three badges (GitHub, Binder, Colab)** will appear by default. You can adjust these defaults in with the `default_badges` parameter in [Site Wide Configuration Options](#site-wide-configuration-options).
    - If only want to hide a badge on an individual post, you can set the front matter `hide_{github,colab,binder}_badge: true`.  For example, if you wanted to hide the Binder badge for an individual notebook but you want the other badges to show up, you can set this in your front matter:

      ```yaml
      - badges: true
      - hide_binder_badge: true
      ```
  - **Note about Binder**: Binder allows you to customize the dependencies and other aspects of the Jupyter Notebook environment for your readers. The easiest way is to add a `requirements.txt` file with common packages you use for all your notebooks at the root of your repository, you can learn more [on the official Binder docs](https://mybinder.readthedocs.io/en/latest/introduction.html).

### Categories
  - You can have a comma seperated list inside square brackets of categories for a blog post, which will make the post visible on the tags page of your blog's site.  For example:

    In a notebook:

    ```
    # "My Title"
    - categories: [fastpages, jupyter]
    ```

    In a markdown document:

    ```
    ---
    title: "My Title"
    categories: [fastpages, jupyter]
    ---
    ```

  You can see a preview of what this looks like [here](https://fastpages.fast.ai/categories/).


  - You can toggle the display of tags on/off by setting `show_tags` to `true` or `false` in `_config.yml`:

```yaml
# Set this to true to display tags on each post
show_tags: true
```

### Enabling Comments

Commenting on blog posts is powered by [Utterances](https://github.com/utterance/utterances), an open-source and ad-free way of implementing comments.  All comments are stored in issues on your blog's GitHub repo.  You can turn this on setting `comments` to  `true`.  This defaults to `false`.

To enable comments with [Utterances](https://github.com/utterance/utterances) you will need to do the following:

  - Make sure the repo is public, otherwise your readers will not be able to view the issues/comments.
  - Make sure the [utterances app](https://github.com/apps/utterances) is installed on the repo, otherwise users will not be able to post comments.
  - If your repo is a fork, navigate to it's settings tab and confirm the issues feature is turned on.

### Setting an Image For Social Media

On social media sites like Twitter, an image preview can be automatically shown with your URL.  Specifying the front matter `image` provides this metadata to social media sites to render this image for you.  You can set this value as follows:

`- image: images/diagram.png`

Note: for this setting **you can only reference image files and folders in the `/images` folder of your repo.**

### Hiding A Blog Post

You may want to prevent a blog post from being listed on the home page, but still have a public url that you can preview or share discreetly.  You can hide a blog post from the home page by setting the front matter `hide` to `true`.  This is set to `false` by default.

It is recommended that you use [permalinks](https://jekyllrb.com/docs/permalinks/) in order to generate a predictable url for hidden blog posts.  You can also set the front matter `search_exclude` to `false` if you don't want users to find your hidden post in a search.

### Pinning A Blog Post

By default, posts are sorted by date on your homepage. However, you may want one or more blog posts to always appear at the very top of your homepage.  In other words, you may want certain posts to be "pinned" or "sticky".  To accomplish this, specify the `sticky_rank` front matter in the order you would like your sticky posts to appear.  Blog posts that do not set this parameter are sorted in the default way by date after the sticky posts.

For example, consider these three markdown posts (also works for notebooks).

`2020-01-01-Post-One.md`
```yaml
---
title: Post One
sticky_rank: 1
---
```

`2020-02-01-Post-Two.md`
```yaml
---
title: Post Two
sticky_rank: 2
---
```

`2020-04-01-Post-Three.md`
```yaml
---
title: Post Three
---
```

However, since `sticky_rank` is specified, blog posts will **first be sorted by sticky_rank in ascending order, then by date in descending order**, so the order of these posts will appear like so:

- Post One
- Post Two
- Post Three

_Without `sticky_rank` the above posts would actually be sorted in reverse order due to the dates associated with each post._

_Note: pinning also works for notebooks:_

```
# "My cool blog post"
> "Description of blog post"

- sticky_rank: 2
```

### Toggle Search Visibility

fastpages comes with built in keyword search powered by [lunr.js](https://lunrjs.com/).  You can prevent a blog post or page from appearing in search results by setting the front matter `search_exclude` to `false`.  This is set to `true` by default.


## Site Wide Configuration Options

**It is recommended that everyone personalizes their blogging site by setting site-wide configration options**. These options can be found in `/_config.yml`.  Below is a description of various options that are available.

- `title`: this is the title that appears on the upper left hand corner on the header of all your pages.  
- `description`: this description will show up in various places when a preview for your site is generated (for example, on social media).
- `github_username`: this allows your site to display a link to your GitHub page in the footer.
- `github_repo`: this allows your site to render links back to your repository for various features such as links to GitHub and Colab for notebooks.
- `url`: This does not need to be changed unless you have a custom domain.  **Note: leave out the trailing / from this value.**
- `baseurl`: See the comments in `/_config.yml` for instructions ( "Special Instructions for baseurl" on setting this value properly.  If you do not have a custom domain, then you can likely ignore this option.
- `email`: this is currently unused.  Ignore.
- `twitter_username`: creates a link in your footer to your twitter page.
- `use_math`: Set this to `true` to get LaTeX math equation support.  This is off by default as it otherwhise loads javascript into each page that may not be used.
- `show_description`: This shows a description under the title of your blog posts on your homepage that contains a list of your blog posts.  Set to `true` by default.
- `google_analytics`: Optionally use a [Google Analytics](http://www.google.com/analytics/) ID for tracking if desired.
- `show_image`: If set to true, this uses the `image` parameter in the front matter of your blog posts to render a preview of your blogs as shown below.  This is set to `false` by default.
  When show_image is set to `true` your homepage will look like this:

  ![home page](/_fastpages_docs/_show_image_true.png)

- `show_tags`: You can toggle the display of tags on your blog posts on or off by setting this value to `false`.  This is set to `true` by default, which which renders the following links for tags on your blog posts like this:

  ![tags](_fastpages_docs/_post_tags.png)

- `pagination`: This is the maximum number of posts to show on each page of your home page.  Any posts exceeding this amount will be paginated onto another page.  This is set to `15` by default.  When this is triggered, you will see pagination at the bottom of your home page appear like this:

  ![paginate](_fastpages_docs/_paginate.png)

  Note: if you are using an older version of fastpages, **you cannot use the automated upgrade process** to get pagination.  Instead you must follow these steps:

    1. Rename your index.md file to index.html
         > mv index.md index.html
    2. Replace the `Gemfile` and `Gemfile.lock` in the root of your repo with the files in this repo.
    3. Edit your `_config.yml` as follows (look at [_config.yml](_config.yml) for an example):
        ```yaml
        gems:
        - jekyll-paginate

        paginate: 10
        paginate_path: /page:num/
        ```

    _Alternatively, you can copy all of your posts over to a newly created  repository created from the fastpages template._

- `default_badges`: By default GitHub, Binder, and Colab badges will show up on notebook blog posts. You can adjust these defaults by setting the appropriate value in `default_badges` to false.  For example, if you wanted to turn Binder badges off by default, you would change `default_badges` to this:

  ```yaml
  default_badges:
    github: true
    binder: false
    colab: true
  ```

- `html_escape`: this allows you to toggle escaping of HTML in various components of blog posts on or off.  At this moment, you can only toggle this for the `description` field in your posts.  
This is set to `false` by default.

## Adjusting Page Width

You can adjust the page width of fastpages on various devices by editing [/_sass/minima/custom-variables.scss](_sass/minima/custom-variables.scss).

These are the default values, which can be adjusted to suit your preferences:

```scss
// width of the content area
// can be set as "px" or "%"
$content-width:    1000px;
$on-palm:          800px;
$on-laptop:        1000px;
$on-medium:        1000px;
$on-large:         1200px;
```

## Annotations and Highlighting With hypothes.is

[hypothes.is](https://web.hypothes.is/) is an open platform that provides a way to annotate and higlight pages, which can be either public or private.  When this feature is enabled, readers of your blog will be presented with the following tooltip when highlighting text:

![annotation](_fastpages_docs/annotate.png)

**This is disabled by default in fastpages.** You can enable or disable this in your [_config.yml](_config.yml) file by setting `annotations` to `true` or `false`:

```yaml
# Set this to true to turn on annotations with hypothes.is
annotations: false
```

> You can customize hypothes.is by reading [these configuration options](http://h.readthedocs.io/projects/client/en/latest/publishers/config/).  It is also a good idea to read [these docs](https://web.hypothes.is/for-publishers/#embedding) if you want to do more with hypothes.is.  However, before trying to customize this feature you should read the [customizing fastpages](#customizing-fastpages) section for important caveats.

## Subscribing with RSS

You can direct your readers to subscribe with [RSS feeds](https://en.wikipedia.org/wiki/RSS).  There are many RSS subscription services available on the internet.  Some examples include:

1. [Feedrabbit](https://feedrabbit.com/)
2. [Blogtrottr](https://blogtrottr.com/)

## Syntax Highlighting

`fastpages` overrides the default syntax highlighting of minima with the [Dracula theme](https://draculatheme.com/).  

- The default highlighting in fastpages looks like this:

  ![default-highlighting](_fastpages_docs/highlight_dracula.png)

- However, you can make the syntax highlighting to look like this, if you choose:

  ![default-highlighting](_fastpages_docs/highlight_original.png)

  If you wish to revert to the light theme above, you can remove the below line in [_sass/minima/custom-styles.scss](_sass/minima/custom-styles.scss)

  ```scss
  @import "minima/fastpages-dracula-highlight";
  ```
- If you don't like either of these themes, you can add your own CSS in [`_sass/minima/custom-styles.scss`](_sass/minima/custom-styles.scss).  See [customizing fastpages](#customizing-fastpages) for more details.

## Dark Mode

[This blog post](https://prudhvirampey.com/blog/colours/jekyll/css/fastpages/2020/10/30/hello-dark-mode.html) describes how to enable Dark Mode for fastpages.

## Adding Citations via BibTeX

Users who prefer to use the citation system BibTeX may do so; it requires enabling the [jekyll-scholar](https://github.com/inukshuk/jekyll-scholar) plugin. Please see [Citations in Fastpages via BibTeX and jekyll-scholar](https://drscotthawley.github.io/devblog4/2020/07/01/Citations-Via-Bibtex.html) for instructions on implementing this.

## Writing Blog Posts With Jupyter

### Hide Input/Output Cells

Place the comment `#hide` at the beginning of a code cell and it wil **hide both the input and the output** of that cell.

A `#hide_input` comment at the top of any cell will **only hide the input**.

Furthermore, the `hide input` [Jupyter Extension](https://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/hide_input/readme.html) can be used to hide cell inputs or outputs, which will be respected by fastpages.

### Collapsable Code Cells

You may want some code to be hidden in a collapsed element that the user can expand, rather than completely hiding the code from the reader.

- To include code in a collapsable cell that **is collapsed by default**, place the comment `#collapse` at the top of the code cell.
- To include code in a collapsable cell that **is open by default**, place the comment `#collapse_show` or `#collapse-show` at the top of the code cell.
- To include the output under a collapsable element that is closed by default, place the comment `#collapse_output` or `#collapse-output` at the top of the code cell.

### Embedded Twitter and YouTube Content
In a markdown cell in your notebook, use the following markdown shortcuts to embed Twitter cards and YouTube Videos.


  ```markdown
  > youtube: https://youtu.be/your-link
  > twitter: https://twitter.com/some-link
  ```
### Adding Footnotes

Adding footnotes in notebooks is a bit different than markdown.  Please see the [Detailed Guide To Footnotes in Notebooks](https://github.com/fastai/fastpages/blob/master/_fastpages_docs/NOTEBOOK_FOOTNOTES.md).

### Automatically Convert Notebooks To Blog Posts

1. Save your notebook with the naming convention `YYYY-MM-DD-*.` into the `/_notebooks` or `/_word` folder of this repo, respectively.  For example `2020-01-28-My-First-Post.ipynb`.  This [naming convention is required by Jekyll](https://jekyllrb.com/docs/posts/) to render your blog post.
    - Be careful to name your file correctly!  It is easy to forget the last dash in `YYYY-MM-DD-`. Furthermore, the character immediately following the dash should only be an alphabetical letter.  Examples of valid filenames are:

        ```shell
        2020-01-28-My-First-Post.ipynb
        2012-09-12-how-to-write-a-blog.ipynb
        ```

     - If you fail to name your file correctly, `fastpages` will automatically attempt to fix the problem by prepending the last modified date of your file to your generated blog post, however, it is recommended that you name your files properly yourself for more transparency.


2. [Commit and push](https://help.github.com/en/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line) your file(s) to GitHub in your repository's master branch.

3. GitHub will automatically convert your files to blog posts.  **It will take ~5 minutes for the conversion process to take place**.  You can click on the Actions tab of your repo to view the logs of this process. There will be two workflows that are triggered with each push you make to your master branch: (1) "CI" and (2) "GH Pages Status".  Both workflows must complete with a green checkmark for your latest commit before your site is updated.

4. If you wish, you can preview how your blog will look locally before commiting to GitHub. See [this section](#running-the-blog-on-your-local-machine) for a detailed guide on running the preview locally.


## Writing Blog Posts With Markdown

If you are writing your blog post in markdown, save your `.md` file into the `/_posts` folder with the same naming convention (`YYYY-MM-DD-*.md`) specified for notebooks.

## Writing Blog Posts With Microsoft Word

Save your Microsoft Word documents into the `/_word` folder with the same naming convention (`YYYY-MM-DD-*.docx`) specified for notebooks.

_Note:_ [alt text](https://support.office.com/en-us/article/add-alternative-text-to-a-shape-picture-chart-smartart-graphic-or-other-object-44989b2a-903c-4d9a-b742-6a75b451c669) in Word documents are not yet supported by fastpages, and will break links to images.

### Specifying front-matter for Word documents

`fastpages` does not have a robust way to specify [front matter](https://jekyllrb.com/docs/front-matter/) for Word documents.  At the moment, you can only specify front matter globally for all Word documents by editing [_action_files/word_front_matter.txt](_action_files/word_front_matter.txt).  

To specify unique front matter per Word document, you will need to convert Word to markdown files manually. You can follow the steps in this [blog post](https://www.fast.ai/2020/01/18/gitblog/), which walk you through how to use [pandoc](https://pandoc.org/installing.html) to do the conversion.  Note: If you wish to customize your Word generated blog post in markdown, make sure you delete your Word document from the _word directory so your markdown file doesn’t get overwritten!  

_If your primary method of writing blog posts is Word documents, and you plan on always manually editing markdown files converted from Word, you are probably better off using [fast_template](https://github.com/fastai/fast_template) instead of fastpages._

# Running the blog on your local machine

See the [development guide](_fastpages_docs/DEVELOPMENT.md).


# Using The GitHub Action & Your Own Custom Blog

The `fastpages` action allows you to convert notebooks from `/_notebooks` and word documents from `/_word` directories in your repo into [Jekyll](https://jekyllrb.com/) compliant blog post markdown files located in `/_posts`.  **Note: This directory structure is currently inflexible** for this Action, as it is designed to be used with Jekyll.

If you already have sufficient familiarity with [Jekyll](https://jekyllrb.com/) and wish to use this automation in your own theme,  you can use this GitHub Action by referencing `fastai/fastpages@master` as follows:

```yaml
...

uses: fastai/fastpages@master

...
```
An illustrative example of what a complete workflow may look like:



```yaml
jobs:
  build-site:
    runs-on: ubuntu-latest
    ...

    - name: convert notebooks and word docs to posts
      uses: fastai/fastpages@master

    ...

    - name: Jekyll build
      uses: docker://jekyll/jekyll
      with:
        args: jekyll build

    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      if: github.event_name == 'push'
      with:
        deploy_key: ${{ secrets.SSH_DEPLOY_KEY }}
        publish_branch: gh-pages
        publish_dir: ./_site
```

Note that this Action **does not have any required inputs, and has no output variables**.  

### Optional Inputs

  - `BOOL_SAVE_MARKDOWN`:  Either 'true' or 'false'.  Whether or not to commit converted markdown files from notebooks and word documents into the _posts directory in your repo.  This is useful for debugging. _default: false_
  - `SSH_DEPLOY_KEY`: a ssh deploy key is required if BOOL_SAVE_MARKDOWN = 'true'

See the API spec for this action in [action.yml](action.yml)

Detailed instructions on how to customize this blog are beyond the scope of this README.  ( We invite someone from the community to contribute a blog post on how to do this in this repo! )

# Contributing To Fastpages

Please see the [contributing guide](_fastpages_docs/CONTRIBUTING.md).

# Upgrading Fastpages

Please see the [upgrading guide](_fastpages_docs/UPGRADE.md).