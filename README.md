# UCSD Supercomputing Club Website

## Contributing
This site is run with Hugo.

`git submodule update --init --recursive`

Once that is done simply run `hugo serve` and it should be hosted on [localhost:1313](http://localhost:1313).

To develop with drafts and publish dates use the `./setup` script
- `./setup` will build the site adding any pages that are marked as drafts and any that have pubish dates in the figure
- `./setup -l` will build the site as if it were deployed as the live site
