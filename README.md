# Static Site Generator
A simple static site generator for blog style websites.

## Dependencies
- Python 3
- Install the required packages with `pip install -r requirements.txt`

## Usage
1. Fork this repository
2. Update the content in the `sample` directory to your liking
3. Optionally rename the `sample` directory to something else and update the 'source' entry in `config.yaml`
4. Run `py build.py` to generate the site
5. The generated site will be in the _site directory

Optionally, you can run `py watch.py` run a local server and automatically rebuild the site when changes are made. It will host at http://localhost:8000.

## Configuration
The configuration is stored in `config.yaml`. The following options are available:
- `title`: The title of the site. This can be referenced in templates as `{{ title }}`
- `description`: A description of the site. This can be referenced in templates as `{{ description }}`
- `source`: The directory where the source files are stored. Defaults to `src`
- `destination`: The directory where the generated site will be stored. Defaults to `_site`
- `base_url`: The base URL of the site. This can be referenced in templates as `{{ base_url }}`. All relative URLs will use the base URL as the root.

## Content

### HTML
HTML files are copied to the destination directory, and any mustache tags are replaced with the corresponding values from the configuration file.

### Markdown
Markdown files are converted to HTML and then processed as HTML files.

### Collections
Collections are groups of pages that can be accessed in templates. For example, a collection of blog posts could be accessed in a template as `{{ collections.posts }}`.

To create a collection, just create a new folder in the source directory and add md files to it. The folder name will be the collection name and you can access the collection in templates as `{{ collections.<collection_name> }}`.

You can create an index.html file in the collection directory to create an index page at the root of the collection folder.

You can create an _item.html file in the collection directory to create a template for each item in the collection, otherwise a simple dump of the content is used (or the _default template if provided).

#### Items
The items are markdown files in the collection directory. Each item needs to have a `title` property. Optionally, you can add a `date` property to sort the items by date. The date should be in the format `YYYY-MM-DD`. You can also add a `category` property to categorize the items. See 'sample/things' for an example.

Any other properties you add to the item will be available in the template for the item.

#### Properties
The following properties are available on collections:
- `items`: A list of all the items in the collection that you can loop through. Items are sorted by `date` in descending order if the property exists on the item, otherwise, they will be sorted by `title` in ascending order.
- `categories`: If your items contain a `category` property, this will be a list of all the unique categories. Each category contains a `name` and a list of `items` that belong to that category. See 'sample/things' for an example.

### Includes
You can create reusable HTML snippets in the `_includes` directory. These can be included in other files using the mustache syntax `{{{ filename-without-extension }}}`.

### Defaults
You can create default layouts for items by creating an `_item.html` file in the collection directory. This will be used as the template for each item in the collection rather than a simple dump of the content. This can be overridden by creating an `_item.html` file in the collection's directory.
