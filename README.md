# gcloud utils

A set of tools for working with gcloud. This assumes you have this respository synced to the gcloud repo `gcloud`.


### Server bootstrapping (`bootstrap`)
Add this script as 
This script will `gcloud clone` any repositories listed in the environment variable `REPOS` and will run the repo
For automatic server bootstrapping, add this script to the `startup-script` metadata for all Google Compute Engines
```bash
mkdir -p /srv &>/var/log/bootstrap.log
gcloud source repos clone gcloud /srv/ &>/var/log/bootstrap.log
/srv/bootstrap &>/var/log/bootstrap.log
```

This will bootstrap all repos in the environment variable `REPOS`. First it will clone the repos, then it will run `scripts/prod/bootstrap` in each repo.

### Metadata usage (`get_metadata`, `use_env`)

Metadata can be stored on instances, and this can be used to create Heroku-like environment variables. `get_metadata` will pull the current metadata into `/srv/metadata.sh`. You can then use the metadata like so,
```bash
$ echo "export ENV=PRODUCTION" > /srv/metadata.sh
$ /srv/use_env python -c "import os; print(os.environ['ENV'])"
PRODUCTION
```
