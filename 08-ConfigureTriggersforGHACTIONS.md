1. Imaging to change the format of date or change anything
2. change smth in the code
3. go to your repo then settings -> https://github.com/taulantracaj-gif/python-app/settings
4. Acions -> general and make sure this is selected
   Allow all actions and reusable workflows
5. search in google docker build push github action -> https://github.com/docker/build-push-action
6. create a folder .github and then workflows
7. create ci.yaml
8. set paths to be src/**
9. set branch main
10. leave only login and build push steps so 2 
11. Docker USER NAME AND PASS tracaj 
12. GO TO GH REPO SETTINGS -> SECRETS -> ACTIONS -> REPOSITORY SECRETS
13. in ci.yaml change tags tracaj/python-app
14. put as tag as unique identifier
   1. go to goolge search for github actions commit id -> https://stackoverflow.com/questions/58886293/getting-current-branch-and-commit-hash-in-github-action
   2. ${GITHUB_SHA::6}
15. go back to terminal