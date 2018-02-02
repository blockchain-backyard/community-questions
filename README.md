# Community-Questions

## Contributor Guidelines

1. `git clone` this repo.
2. `git submodule init`
3. `git submodule update` to obtain the theme used. Currently using [future-imperfect](https://themes.gohugo.io/future-imperfect/).

Other dependencies:
1. [hugo](http://gohugo.io/) in your path.

### Writing a new post

Create it using:
```
hugo new posts/<name of post>.md
```
Note: our theme impacts how we organize a post. Primary difference from the default created template is a missing `type = "post"` entry in the header. Adding this manually seems to work.

Local testing:
```
hugo server -D
```

Finally publishing:
```
hugo
```
Also note that before publishing, remove the line `draft = "true"`. Only then will the post show up in the final output.  Note: we publish to the `docs/` directory for gh-pages to directly pick it up; not that you have to do anything, it is configured in the `config.toml` file.
