## Local installs/self-managed

If you installed using the [local/self managed instructions](/Local-installation), simply download the [latest version](../releases/latest) and replace it on your machine.  Restart the application. That's it!

## Heroku

### After a manual install

If you installed manually using [these instructions](Manual-installation-on-Heroku), it's easy.  First navigate to where you checked out the code during the setup process, e.g.:
```
cd orko
```
Now just update the code and push it to Heroku:
```
git pull origin stable
git push heroku stable:master
```
The application will be upgraded and automatically restart.

### After a quick deploy

If you deployed using the quick installer using [these instructions](One-click-installation-on-Heroku), unfortunately, Heroku provides no easy way of upgrading such an environment without getting your hands dirty.

**You may find it easiest, if you are not a particularly technical user, to simply delete the app on Heroku and re-install it.**

If you prefer, you can take full control of the environment as if you had performed a manual install. The following is based on [these excellent instructions](https://f-a.nz/dev/update-deploy-to-heroku-app/):

- Write down the name of your Heroku app. It's used in most places in the Heroku website, and is usually the first part of the url, e.g. if your app URL is http://kolvoord-starburst.herokuapp.com, your app name is "kolvoord-starburst".
- Install [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
- Login:
```
heroku login
```
- Create a local git repository, set it up as a remote of your heroku app, and add a remote to fetch the source code from Github:
```
mkdir orko
cd orko
git init
heroku git:remote -a name-of-your-app
git remote add origin https://github.com/gruelbox/orko
```
- Finally, push the latest code to Heroku:
```
git fetch
git checkout -t origin/stable
git push -f heroku stable:master
```
That's it! From now on, you can use the "manual install" approach [detailed above](#after-a-manual-install) to perform upgrades.