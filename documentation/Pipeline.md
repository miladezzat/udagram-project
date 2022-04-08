## FRONT END SCRIPTS in udagram-frontend

"ng": "ng",
"start": "ng serve",
"build": "ng build",
"test": "ng test",
"lint": "ng lint",
"e2e": "ng e2e",
"deploy": "sh ./bin/deploy.sh"

## BACK END SCRIPTS in udagram-api

    "start": "node .",
    "tsc": "tsc",
    "dev": "ts-node-dev --respawn --transpileOnly ./src/server.ts",
    "prod": "tsc && node ./www/server.js",
    "clean": "rm -rf www/ || true",
    "build": "npm run clean && tsc && cp -rf src/config www/config && cp .npmrc www/.npmrc && cp package.json www/package.json && cd www && zip -r Archive.zip . && cd ..",
    "deploy": "sh ./bin/deploy.sh",
    "test": "echo \"Error: no test specified\" && exit 1"

### CircleCI scripts used in the Workflow build_and_deploy

    "frontend:install": "cd udagram-frontend && npm install",
    "frontend:build": "cd udagram-frontend && npm run build",
    "backend:install": "cd udagram-api && npm install",
    "backend:build": "cd udagram-api && npm run build",
    "frontend:deploy": "cd udagram-frontend && npm run deploy",
    "backend:deploy": "cd udagram-api && npm run deploy",
    "backend:test": "cd udagram-api && npm run test"

### PipeLine Jobs

#### Workflow: build_and_deploy

docker: - image: cimg/base:stable
steps: - node/install - checkout - aws-cli/setup - eb/setup - run:
name: Front-End Install
command: |
npm run frontend:install - run:
name: Back-End Install
command: |
npm run backend:install - run:
name: Front-End Build
command: |
npm run frontend:build - run:
name: Back-End Build
command: |
npm run backend:build - run:
name: Deploy App frontend
command: |
npm run frontend:deploy - run:
name: Deploy App backend
command: |
npm run backend:deploy

##### what is done in this pipeline

- first installing the packages in the front-end and back-end
- then building the front-end and the back-end
- then deploying the front-end (by using the s3 bucket)
- and deploying the backend (by using the eb)

##### Diagram for our Pipeline and how it works

![alt text](https://github.com/david-wagih/DeploymentProject/blob/master/images/pipelineFlow.png?raw=true)

##### ScreenShots from CircleCi with the latest build done and it is successfull

![alt text](https://github.com/david-wagih/DeploymentProject/blob/master/images/lastBuild1.jpg?raw=true)
![alt text](https://github.com/david-wagih/DeploymentProject/blob/master/images/lastBuild2.jpg?raw=true)
