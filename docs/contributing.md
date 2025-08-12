# How to contribute to LiGHT docs

## Guidelines

To contribute, you can suggest changes and open pull requests at our [GitHub repo](https://github.com/LiGHT-YalEPFL/LiGHT-doc)

`LiGHT-doc` uses `mkdocs`. `mkdocs` is a tool that automatically build documentation from markdown files.

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

### Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

Every markdown files are written in the `docs` directory. 

### Workflow

* Update your relevant files in the `docs` folder

* Test your local changes by running
```
mkdocs serve
```
and accessing `localhost:8000` on your browser

* Push your changes to some branch
```
git checkout -b [branch name]
git add [files]
git commit -m [Commit message]
git push
```

* Open a pull request to the `main` branch on GitHub

* Once the pull request is merged on `main`, changes should be visible at https://light-yalepfl.github.io/LiGHT-doc/
