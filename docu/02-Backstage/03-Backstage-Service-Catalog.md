# Backstage Service Catalog
 It lets you organize, discover, and manage all your software components in one unified platform. 🚀
Backstage Software Catalog for managing all your software (microservices, libraries, data pipelines, websites, ML models, etc.)

Backstage Software Templates for quickly spinning up new projects and standardizing your tooling with your organization’s best practices

Backstage TechDocs for making it easy to create, maintain, find, and use technical documentation, using a "docs like code" approach

 1. Go to google and search backstage docs -> https://backstage.io/docs/overview/what-is-backstage
 2. Lets begin with sfotware catalog
 3. for our case we have 1 service that we developed and we will think as component - > our http://python-app.test.com/api/v1/info
 4. the problem is that we created the ap before backstage so How do i register existing component
 5. it is simple

# Register component
1. Go to terminal open new terminal 
 run  docker ps
2. go inside continer 
        docker exec -ti 24f60955ff93 bash 
        and go to backstage
3. Check content of catalog-info.yaml 
  cat catalog-info.yaml
        apiVersion: backstage.io/v1alpha1
        kind: Component
        metadata:
            name: backstage
        description: An example of a Backstage application.
        # Example for optional annotations
        # annotations:
        #   github.com/project-slug: backstage/backstage
        #   backstage.io/techdocs-ref: dir:.
        spec:
            type: website
            owner: john@example.com
            lifecycle: experimental    
4. This is how we register component in backstage
5. in our python app repo, we need to create a file with the definition above
6. create new file in the root of repo call it catalog-info.yaml 
7. to check options go to google
    1. search for kind: Component -> https://backstage.io/docs/features/software-catalog/descriptor-format/#kind-component
    2. we want to use the types 
        service - a backend service, typically exposing an API
        website - a website
        library - a software library, such as an npm module or a Java library
    3. for our case we use service
    4. owner is who owns the component , 
       1. we are going to create later a group
## Create group development
    1. go to new terminal and login to the container
        docker exec -ti 24f60955ff93 bash 
    2. go to catalog/entities
    3. create file groups.yaml
    4. nano groups.yaml
    5. to find out what go to browser and -> https://github.com/backstage/backstage
    6. packages -> catalog-model -> examples ->acme -> https://github.com/backstage/backstage/blob/master/packages/catalog-model/examples/acme/team-a-group.yaml
    7. copy the group part
    8. go to terminal and paste it in groups.yaml 
    9. Change the name to development and descr to Development team
    10. Email to dev.. also picture can be changed
    11. remove parent and save 
    12. add the user
      1. nano users.yaml
      2. member of -> change it to development
## Register group against backstage
    1. go to app/backstage
    2. nano app-config.local.yaml
    3. add one more location
              locations:
                # Local example data, file locations are relative to the backend process, typically `packages/backend`
                - type: file
                  target: /app/backstage/catalog/entities/groups.yaml
    4. and one level above allow Group     
        - allow: [User, Group, Component, System, API, Resource, Location]
    5. under type add rules and allow only users and groups
            catalog:
            rules:
                - allow: [Component, System, API, Resource, Location]
            locations:
                # Local example data, file locations are relative to the backend process, typically `packages/backend`
                - type: file
                target: /app/backstage/catalog/entities/users.yaml
                rules:
                    - allow: [User]
                # Local example data, file locations are relative to the backend process, typically `packages/backend`
                - type: file
                target: /app/backstage/catalog/entities/groups.yaml
                rules:
                    - allow: [Group]
## Register Existing component
    1. Go to terminal 
    2. exit from second terminal and go to python app
    3. push the catalog-info.yaml
    4. stop the service backstage from terminal 1
    5. start
      yarn start
    6. check it, now new menu item My Group should appear http://localhost:3000/catalog/default/Group/development
    7. still under components there is no component check home directory
    8. create
      1. Go to Home 
      2. click create
      3. Reister existing component
      4. url -> put  https://github.com/taulantracaj-gif/python-app/blob/main/catalog-info.yaml
      5. click analyze
      6. Import 
      7. View COmponent -> it will redirect you to http://localhost:3000/catalog/default/component/python-app
      8. click on Home and u can see one component
      9. click on view source and will redirect to gh

