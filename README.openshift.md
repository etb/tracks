# Tracks on Openshift

Tracks can be run easily on [Red Hat's Openshift platform](www.openshift.com), using the following method.

## Assumptions

This guide assumes you have an Openshift account, and that you have installed and
configured the 'rhc' commandline app to create Openshift apps [as per the Openshift docs](https://developers.openshift.com/en/managing-client-tools.html).

* _\[app\]_ will refer to the name of the app (e.g. "tracks")
* _\[domain\]_ will refer to your Openshift domain namespace (e.g. "floweringcactus")

## Commands

First clone this repo to your local machine:

git clone -b openshift-quickstart  git@github.com:GregSutcliffe/tracks.git

Then create the app:

* rhc app create [app] ruby-2.0 mysql-5.5 BUNDLE_WITHOUT="development test" "RAILS_ENV=production" --no-git

Normally, `rhc` will clone the (empty) git repo that comes with a new app. We don't need that (we already have the Tracks repo), so `--no-git` stops rhc from cloning it

This command will take a while. Then (amongst other output) it will print the git URI for your new app. Add it to your tracks clone as a remote:

* git remote add \[app\] \[git uri\]

Now edit `site.yml` and configure it to your liking. The defaults will work fine if you're just testing Tracks (in which case, skip this step):

* vim site.yml
* git add site.yml
* git commit -m "Added local site config"

Finally, push the repo to your Openshift app:

* git push \[app\] HEAD:master -f

## Testing

Visit _https://\[app\]-\[yourdomain\].rhcloud.com_ to see if Tracks is running. If it is, you'll be asked to create an admin account.

*Note*: You must use at least 8 characters in the admin password or it will silently fail to create the account and give you some very odd errors.

## Troubleshooting

SSH in (using the login on your Openshift application details page) and have a look around. Useful logs:

* Rails logs: ~/app-root/repo/log/production.log
* Apache logs: ~/app-root/logs/ruby.log
* Database logs: ~/app-root/logs/mysql.log

## Todos

* Convert to Rails 4.1 secrets.yaml instead of hacking in the initializers
