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

### Install cand configure techdocs 
    1. we do not ahve by default mkdocs in backstage and we have to make sure that we install mkdocs
    2. Go to docu -> https://backstage.io/docs/features/techdocs/ -> getting started -> https://backstage.io/docs/features/techdocs/getting-started
    3. Adding the frontend plugin first - if u use npx it might be that is present. lets check
    4. lets check this file packages/app/src/App.tsx  , it should have smth like this

            import {
                TechDocsIndexPage,
                techdocsPlugin,
                TechDocsReaderPage,
                } from '@backstage/plugin-techdocs';
    6. then no need to add 
    7. setting the configuration
    8. we need this
            techdocs:
                builder: 'local'
                publisher:
                    type: 'local'
                generator:
                    runIn: local
    9. the important thing about generatore is where the container for mkdocs will be installed
    10. go to container appconfig.local.yaml
        nano app-config.local.yaml
    11. add the section from point 8
    12. save it - what we did is we told that we want to run mkdocs localy from the container
    13. we need to download mkdocs 
    14. how to do it   -> https://backstage.io/docs/features/techdocs/getting-started#disabling-docker-in-docker-situation-optional
        1. we need  to run this inside container
            apt-get update && apt-get install -y python3 python3-pip python3-venv 
        2. wait for some time to finish
        3. verify
            python3 
        4. export VIRTUAL_ENV=/opt/venv
        5. python3 -m venv $VIRTUAL_ENV
        6. export PATH="$VIRTUAL_ENV/bin:$PATH"
        7. pip3 install mkdocs-techdocs-core
        8. this will run installation of mkdocs 
        9. once done we can test
        10. yarn start
        11. we need to readd the component beacuse we do not have database
    15. after start the server
    16. readd the component 
    17. go to component and click view techdocs

## NOTE
But if you want a permanent solution now, you can create a docker image from this dockerfile:

FROM node:18-bookworm-slim
 
RUN apt-get update && apt-get install -y python3 python3-pip python3-venv
 
ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
 
RUN pip3 install mkdocs-techdocs-core


and use it the next time you want to start your backstage server!



You could also refer to the video lecture where we fix this, which is in the Backstage Software Templates section, and the name of the lecture is: Final fixes to templates and TechDocs