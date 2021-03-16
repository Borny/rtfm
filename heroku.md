# Heroku

- login
- logout
- git push
- logs

## Login

By opening a link in the browser  
`heroku login`

In the CLI only  
`heroku login -i`

## Logout

`heroku logout`

## Clone the heroku repo

`heroku git:remote -a <appName>`

## git push

`git push heroku master`

## logs

### Displaying logs

`heroku logs`

### Displaying the logs and keeping the session open for new logs to stream in

`heroku logs --tail`