# meilisearch-dokku

Deploy an instance of [Meilisearch](https://meilisearch.com/) on Dokku through a `Dockerfile`.

## Deploy

Create git remote:

```
git remote add dokku dokku@{app-server}:{my-meili-app}
```

Create dokku app and config:

```
dokku apps:create {my-meili-app}
dokku config:set MEILI_MASTER_KEY={secret}
```

Configure [persistent storage](https://dokku.com/docs/advanced-usage/persistent-storage/):

```
dokku storage:ensure-directory {my-meili-app}
dokku storage:mount /var/lib/dokku/data/storage/{my-meili-app}:/meili_data
```

Setup port redirection and enable SSL:

```
dokku proxy:ports-add http:80:7700
dokku proxy:ports-remove 7700
dokku letsencrypt:enable
```

Deploy a new version:

```
# FIXME: can't seem to start two apps concurrently, to be digged into
dokku ps:stop {my-meili-app}
git push dokku main
```

Meilisearch is available at https://{my-meili-app}.{app-server}
