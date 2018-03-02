# github-issuetracker

This is a Serverless function that tracks new GitHib issues, processes them with sentiment analysis and applies `positive` and `review` labels.


## Deploy Sentiment Analysis function

In order to use this filter function, you will need to deploy the Sentiment Analysis function first.
This is a python function that provides a rating on sentiment positive/negative (polarity -1.0-1.0) and subjectivity provided to each of the sentences sent in via the TextBlob project.

You can checkout the function from [faas/sample-functions](https://github.com/openfaas/faas/tree/master/sample-functions).


## Update image with your Docker ID

You need to update `image` value in `filter.yml` and replace `docwareiy` with your Docker ID. 
You can check your images in [Docker Hub](https://hub.docker.com).


## Create Github Auth token

Go to your GitHub profile -> Settings/Developer settings/Personal access tokens and generate new token.

Copy the contents of `env.example.yml` to `env.yml` and update `auth_token` value with the new generated token.

Update `repo` in `env.yml` with the repository you'd like to use the function for.


## Create GitHub repo Webhook

Go to your github repo -> Settings/Webhooks and create a new Webhook.

Update **Payload URL** to be your OpenFaaS host and the function extension, e.g. `http://localhost:8080/function/issuetracker`.

Choose `application/json` for a **Content Type**.

Select **Let me select individual events** and then **Issues** and **Issue comment**.

Select **Active** and press **Update webhook**.


## Build and Deploy:

Use the CLI to build and deploy the function:

```
faas build -f issuetracker.yml & faas push -f issuetracker.yml & faas deploy -f issuetracker.yml
```

View the logs by executing `docker service logs -f issuetracker` on the Docker instance.

You can now test the function by creating new issues in your repo.