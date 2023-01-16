# Heroku

- [Install](#install)
- [Login](#login)
- [Logout](#logout)
- [git push](#git-push)
- [logs](#logs)

## Install

## Login

By opening a link in the browser  
`heroku login`

In the CLI only  
`heroku login -i`

## Logout

`heroku logout`

## Add remote

When switching projects, clone the corresponding remote:

`heroku git:remote -a <appName>`

## git push

`git push heroku master`

## logs

### Displaying logs

`heroku logs`

### Displaying the logs and keeping the session open for new logs to stream in

`heroku logs --tail`
