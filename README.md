# ligato.io [![Netlify Status](https://api.netlify.com/api/v1/badges/3cafe370-cf40-4fba-97f1-1451bf0a22d6/deploy-status)](https://app.netlify.com/sites/ligato/deploys)

Contains the code for the Hugo-based Ligato.io website. Upon changes detected in the github repo, it will be deployed using Netlify to replace the existing Ligato.io site.

## Instructions to Clone/Run

### Step 1 - Install Hugo

Follow the instruction located [here](https://gohugo.io/getting-started/quick-start/).

Don't worry about the theme in the quickstart guide. It is used just to get you going. 

Yhis site is using modified [Hugo-Fresh theme](https://github.com/StefMa/hugo-fresh)

Make sure you have the correct version of Hugo.
```
USER:ligato.io $ hugo version
Hugo Static Site Generator v0.54.0/extended darwin/amd64 BuildDate: unknown
```


### Step 2 - Clone this site

The hugo-fresh theme is treated as a `submodule`. This is required by `Netlify` for automatic deployment.

Be sure to use `THIS COMMAND`:

```
git clone --recurse-submodules git@github.com:ligato/ligato.io.git 
```


### Step 3 - Run it locally

```
cd ligato.io
hugo server -D
```
Go to localhost:1313 on your favorite browser and check it out.

This uses live reload so any changes will be instantaneous.

### Some Comments

* all content is written in markdown. For images, use markdown links or hugo shortcodes for images to work.

* all content is contained in the content/ folder. You can see how sub-folders under content/ relate to specific content and corresponding templates in the layouts/ on the site.

* all the content here is high-level. Detailed technical documentation is automatically retrieved from the [Ligato docs repo](https://github.com/ligato/docs) and hosted on a readthedocs.io site which in turn is reachable by the `documentation` menu option at the top and various topic-specific pointers throughout the site.

* Check the `front matter` of new content/ md files. Sidebar, templates, title, etc. may change depending on location in website. 

* `config.yaml` is the main config file. 

