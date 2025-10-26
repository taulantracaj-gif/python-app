# Backstage Tech Docs
Imagine if you could manage documentation the same way you manage code. 👩‍💻

What if you could simply write the documentation for your application as a text file (md), right inside the same repo as your code?

That would be awesome! 🙌

No more jumping between Notion, Confluence, or scattered docs.

And the best part? Your documentation would live in Git! versioned, and always in sync with your code. ✅

That’s exactly what Backstage TechDocs are. They let you render your "documentation as code" inside Backstage, with very little setup! 🚀

Let's set it up and create some md (markdown) files where we will store our app's documentation!

## Tech Docs configure 
1. Go to https://backstage.io/docs/overview/what-is-backstage
2. check for Backstage TechDocs -> https://backstage.io/docs/features/techdocs/
3. in our python-repo create a folder called docs
4. Create file index.md - use sites like stackedit.io

### turn md files to html
1. go to mkdocs -> https://example-mkdocs-basic.readthedocs.io/en/latest/ and backstage is using this to generate html from md
2. we need to create the mkdocs.yml
3. how to find example
    1. go to https://github.com/backstage/backstage/
    2. and find mkdocs.yml -> https://github.com/backstage/backstage/blob/master/mkdocs.yml
    3. copy one page 
            site_name: 'Backstage'
            site_description: 'Main documentation for Backstage features and framework APIs'
            repo_url: https://github.com/backstage/backstage
            edit_uri: edit/master/docs

            plugins:
            - techdocs-core
            - redirects:
                redirect_maps:
                    'index.md': 'overview/what-is-backstage.md'

            # For sidebar navigation on https://backstage.io/, see `microsite/sidebars.js`
            nav:
            - Overview:
                - What is Backstage?: 'overview/what-is-backstage.md'
    4. Go to your python app repo and create mkdocs.yaml and paste the file 
            site_name: 'python-app'
            site_description: 'Main documentation for the python-app'
            repo_url: https://github.com/taulantracaj-gif/python-app
            edit_uri: edit/main/docs

            plugins:
            - techdocs-core

            # For sidebar navigation on https://backstage.io/, see `microsite/sidebars.json`
            nav:
            - Overview:
                - Getting started: 'docs/index.md'
    5. Go to catalog-info.yaml in the python-app repo and add this
        backstage.io/techdocs-ref: dir:.  - we want to use techdocs,plugins and pls go to current dir and check for docs -> docs is  default by backstage
    6. sdasd