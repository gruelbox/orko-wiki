## Local installs/self-managed

If you installed using the [local/self managed instructions](/Local-installation), simply download the [latest version](../releases/latest) and replace it on your machine.  Restart the application. That's it!

## Heroku

### After a manual install

If you installed manually using [these instructions](Manual-installation-on-Heroku), it's easy.  Simply do the following:
```
> git pull origin stable
> git checkout stable
> git push heroku stable:master
```
The application will be upgraded and automatically restart.

### After a quick deploy

If you deployed using the quick installer using [these instructions](One-click-installation-on-Heroku), unfortunately, Heroku provides no easy way of upgrading such an environment without getting your hands dirty.

**You may find it easiest, if you are not a particularly technical user, to simply delete the app on Heroku and re-install it.**

If you prefer, you can take full control of the environment as if you had performed a manual install, as follows:

