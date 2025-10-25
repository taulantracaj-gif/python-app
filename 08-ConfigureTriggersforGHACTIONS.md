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
15. commit the changes
16. it will fail with invalid reference format GITHUB_SHA::6
17. TO FIX
   1. THIS ${GITHUB_SHA::6} works only on bash since this is a modue which is not bash then
   2. lets try this ${{ github.sha }}
   3. ACTION should be trigged and in  https://hub.docker.com/repository/docker/tracaj/python-app/general we should see the new tag
18. To shorten the commint
    1. go to stackoverflow -> https://stackoverflow.com/questions/58886293/getting-current-branch-and-commit-hash-in-github-action
    2. get this
      - name: Shorten Commit Id
        shell: bash
        run: |
          echo "COMMIT_ID=${GITHUB_SHA::6}" >> "$GITHUB_ENV"
  
    3. past it in the job and change 
         env.COMMIT_ID
19. 