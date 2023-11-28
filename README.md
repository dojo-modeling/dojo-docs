# Dojo Documentation

This is a Jekyll App using the [Just the Docs](https://github.com/just-the-docs/just-the-docs) theme. It is hosted with Github Pages.

## Running Locally

If you want to contribute to the documentation, the easiest way to do so is to run the Jekyll app locally. You can run it by pulling down the project, installing [Jekyll](https://jekyllrb.com/docs/installation/), and running
- `bundle install` to install dependencies
- `bundle exec jekyll start -l` to run the server

## Adding New Pages

New top level pages go in `docs`. Child pages go in `docs/details`. Any new pages need to be added to the navigation structure in `docs/_data/navigation.yml` in order to show up in the sidebar. This is then rendered by `docs/_includes/components/sidebar.html`, which overrides the default Just the Docs sidebar.html. This is an alternative to the Just the Docs default of adding `nav_order` to the front matter block, as it allows us to much more easily see and change the nav order/structure.

> Make sure to add new pages to `docs/_data/navigation.yml`
