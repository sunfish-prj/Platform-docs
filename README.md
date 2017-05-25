# Documentation README
This is the documentation of the SUNFISH platform components available on [ReadTheDocs](http://sunfish-platform-docs.rtfd.io/)

The documentation describes the *key concepts* of _Federation-as-a-Service_(FaaS) and _SUNFISH Platform_. FaaS is an innovative cloud federation solution enabled by the software SUNFISH Platform. 

## Contributing to the documentation

This documentation is in written in [reStructuredText](http://docutils.sourceforge.net/rst.html) *.rst* files which are then
automatically converted to HTML using [Sphinx](http://www.sphinx-doc.org/en/stable/). 

This GitHub repository contains all the *.rst* files. When a new commit is pushed to the repository, the updates are built and published on http://sunfish-platform-docs.rtfd.io/. Any new content that goes into `docs/` triggers a new build and publication. The main entry-point is 'doc/index.rst'.


## Setup (for repository's contributors)

You can easily setup you local environment to contribute to the guide. 

### Sphinx setup

Here are the quick steps to setup Sphinx on your local machine. (You may need to adjust depending on your OS, check [documentation](http://www.sphinx-doc.org/en/stable/index.html#0))

```
sudo pip install Sphinx
sudo pip install sphinx_rtd_theme
```

The commands 'make html' to locally compile the documentation in `docs/_build/html/` where you can check from *index.html* a local version of the guide. 

```
NB: the folder '_build' is not synchronised on the repository, it is local. 
```

### Writing steps
Configure the Git repository

1. Clone this GitHub repository `git clone https://github.com/sunfish-prj/Platform-docs.git`

Write your contributions

2. Change the file in `docs` (use this [guide](http://www.sphinx-doc.org/en/stable/rest.html#rst-primer) to help in writing Sphinx. Don't worry it is almost pure text)
3. `make html` from `docs/`
4. Check locally your changes to the guide `docs/_build/html/`

When ready to push the changes online

5. `git add .`
4. `git commit -m 'Your message'`
5. `git push origin master`
6. Check [ReadTheDocs](http://sunfish-platform-docs.rtfd.io/) out! 

If you prefer to use a GUI for Git, you might use https://desktop.github.com/ 

