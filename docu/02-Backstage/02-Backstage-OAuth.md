# Backstage OAUTH
1. make sure the container is not up
2. recreate with exposed port 7007 used for the backend (3000 is for the frontend)
    docker run --rm -p 3001:3000 -ti -p 7007:7007 -v /home/tara/backstage-app:/app -w /app node:20-bookworm-slim bash
3. cd backstage and ls
4. more app-config.yaml
5. cat app-config.yaml and check the section auth
    auth:
    # see https://backstage.io/docs/auth/ to learn about auth providers
    providers:
        # See https://backstage.io/docs/auth/guest/provider
        guest: {}
6. go to this link https://backstage.io/docs/auth/
7. check github
8. create oauth app on GH
 1. Go to GH -> https://github.com/settings/profile
 2. developer settings 
 3. Oauth Apps
 4. create new app
 5. app name -> 
 6. homepage url -> http://localhost:3000
 7. callback url -> http://localhost:7007/api/auth/github/handler/frame
 8. register application
 9. Generate new client secret
    1. Client id: Ov23lip85bL3ak0yFrts
    2. Client Secret: 556c0715840b84806c09166e177588092f333df8
9. Go now to app-config.local.yaml - used for local
    app:
    listen:
        host: 0.0.0.0
10. Go now to app-config.yaml - and remove the changes before for listen
11. now include auth changes  from https://backstage.io/docs/auth/github/provider#configuration
12. export AUTH_GITHUB_CLIENT_ID and AUTH_GITHUB_CLIENT_SECRET
    1. exit from container
    2. docker run --rm -e AUTH_GITHUB_CLIENT_ID=Ov23lip85bL3ak0yFrts -e AUTH_GITHUB_CLIENT_SECRET=556c0715840b84806c09166e177588092f333df8 -p 3000:3000 -ti -p 7007:7007 -v /home/tara/backstage-app:/app -w /app node:20-bookworm-slim bash
    3. WRITE echo $AUTH_GITHUB_CLIENT_ID or echo $AUTH_GITHUB_CLIENT_SECRET
13. Donwload Backstage plugins - backstage does not provide providers by default 
    1. Go to Backend Installation -> https://backstage.io/docs/auth/github/provider#backend-installation
    2. to download plugin auth for github -> 
        yarn --cwd packages/backend add @backstage/plugin-auth-backend-module-github-provider
    3. it will take time to execute
14. How to use the plugin
    1. go here and see the backend.add https://backstage.io/docs/auth/github/provider#backend-installation
    2. go to this file in packages/backend/src/index.ts
         nano packages/backend/src/index.ts
    3. add it under //auth provider
15. Add plugin to the frontend
    1. go to this https://backstage.io/docs/auth/#sign-in-configuration
    2. go to packages/app/src/App.tsx
        nano packages/app/src/App.tsx
    3. add the belowmentiond before import { Navigate, Route } from 'react-router-dom';
        import { githubAuthApiRef } from '@backstage/core-plugin-api';
        import { SignInPage } from '@backstage/core-components';
    4. We can remove this as below its already in place import { SignInPage } from '@backstage/core-components';
    5. copy the components part
          components: {
            SignInPage: props => (
            <SignInPage
                {...props}
                auto
                provider={{
                id: 'github-auth-provider',
                title: 'GitHub',
                message: 'Sign in using GitHub',
                apiRef: githubAuthApiRef,
                }}
            />
            ),
        },
    6. and paste it instead of components
    7. save it
16. Backend Resolvers
    1. Go to https://backstage.io/docs/auth/github/provider#resolvers
    2. resolver is basically who is allowed to login or not kind of whitelist
    3. go to google and type backstage github - >https://github.com/backstage/backstage
    4. go to packages -> catalog-model -> examples ->acme
    5. Click on team-a-group.yaml and copy the user part
            apiVersion: backstage.io/v1alpha1
            kind: User
            metadata:
                name: breanna.davison
            spec:
              profile:
                # Intentional no displayName for testing
                email: breanna-davison@example.com
                picture: https://api.dicebear.com/7.x/avataaars/svg?seed=Luna&backgroundColor=transparent
                memberOf: [team-a]
    6. go to terminal under /app/backstage create 
        mkdir catalog/entities -p
    7. create files users.yaml
        nano catalog/entities/users.yaml 
    8. paste the content under 5
    9. under name section make sure u match the exact gh username mine is taulantracaj-gif
    10. Why to put the exact username as we are using the resolver usernameMatchingUserEntityName
    11. if u want more users then u do --- then again the same block
    12. save it and now we created the whitelist or users that are allowed to backstage
    13. We want to tell the backstage to use the file team-a-group.yaml
        1. open cat app-config.yaml
        2. look for section catalog and copy it
           catalog:
            import:
                entityFilename: catalog-info.yaml
                pullRequestBranchName: backstage-integration
            rules:
                - allow: [Component, System, API, Resource, Location]
            locations:
                # Local example data, file locations are relative to the backend process, typically `packages/backend`
                - type: file
                  target: ../../examples/entities.yaml
        3. open local.yaml
            nano app-config.local.yaml
        4. add the extra config and add User in allows, remove the import part and chagne the target to /app/backstage/catalog/entities/users.yaml
                catalog:
                    rules:
                        - allow: [User, Component, System, API, Resource, Location]
                    locations:
                        # Local example data, file locations are relative to the backend process, typically `packages/backend`
                        - type: file
                          target: /app/backstage/catalog/entities/users.yaml

# RECAP 
1. We created OAUTH app on gh
2. we modified our local file to include client and secret and set the vars as env var
3. downloaded the plugin
4. add plugin to the backend and frontend
5. we made changes for the resolvers for usernameMatchingUserEntityName
  we created a file with users that are allowed to login to backstage

# TEST
1. open new ubuntu and do docker ps
2. make sure port 7007 is exposed as this
    6bb0052a0e8b   node:20-bookworm-slim   "docker-entrypoint.s…"   42 minutes ago   Up 41 minutes   0.0.0.0:7007->7007/tcp, [::]:7007->7007/tcp, 0.0.0.0:3001->3000/tcp, [::]:3001->3000/tcp   lucid_pasteur
3. 



