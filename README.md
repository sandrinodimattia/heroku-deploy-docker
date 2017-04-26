# Heroku Deploy Docker Image

This images makes it easier to create deploy jobs in GitLab CI which need to interact with Heroku.

It includes:

 - [The Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) which allows you to execute scripts (eg: database migrations), update settings, ...
 - [Dpl](https://github.com/travis-ci/dpl) the deploy tool by the team at Travis-CI, which includes support for Heroku

## Usage

You can get the Heroku API Key from the [Heroku Dashboard](https://dashboard.heroku.com/account). This key will be used by both the **Heroku CLI** and **Dpl**. This is why you should set it as a secret variable `HEROKU_APP_NAME` in GitLab

Then you can define a new job that deploys to Heroku:

```yaml
deploy-to-heroku-staging:
  stage: deploy
  image: sandrinodimattia/heroku-deploy-docker:0.1.0
  environment:
    name: heroku-staging
    url: https://my-app.herokuapp.com/
  variables:
    HEROKU_APP_NAME: my-app
  script:
    - dpl --skip_cleanup --provider=heroku --app=$HEROKU_APP_NAME --api-key=$HEROKU_API_KEY
    - heroku run "npm run db:up" --exit-code --app $HEROKU_APP_NAME
  only:
    - master
```
