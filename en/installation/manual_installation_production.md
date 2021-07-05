# Manual installation for production

Beforehand it should be noted that this method is *not recomended*, since you should use the [installer](https://github.com/consul/installer) instead.
Use this method if you can already deal with NGNIX, https (such as letsencrypt), SSL, puma or passenger, mail server and postgres. 
This assumes also that you already [installed all the necessary packages](https://github.com/consul/docs/blob/master/en/installation/debian.md) on your system.
The created directory structure herein is to be used with [capistrano structure](https://capistranorb.com/documentation/getting-started/structure/).

1. First create our main directory and enter in it, clone the repo to a repo directory, and create some necessary directories

```
mkdir consul && cd consul
git clone --mirror https://github.com/consul/consul.git repo
mkdir releases shared
mkdir shared/tmp shared/config shared/public
mkdir shared/public/assets shared/public/system shared/public/ckeditor_assets
```

2. Now extract from the repo the first release to the respective directory, and add create the symbolic link of the current release

```
cd repo
git archive 1.2.0 | tar -x -f - -C ../releases/first
cd ..
ln -s releases/first current
```

3. Now deploy the production version

```
cd releases/first
bundle install --path ../../shared/bundle --without development test
cd ../..
```

4. Now copy some files and create some links

```
cp current/config/secrets.yml.example shared/config/secrets.yml
cp current/config/database.yml.example shared/config/database.yml
cd releases/first/config
ln -s ../../../shared/config/database.yml
ln -s ../../../shared/config/secrets.yml
cd ../../..
```

5. Edit `database.yml` and `secrets.yml`

Enter the first shared directory

```
cd shared
```

Edit now therein `config/database.yml` and insert therein on `username` and `password` your credentials generated during the [setup](https://github.com/consul/docs/blob/master/en/installation/debian.md#postgresql-94) of the system.


We need now to generate a key with

```
bin/rake secret RAILS_ENV=production
```

Copy that generated key into the clipboard. 

Now edit the file `config/secrets.yml` file and inside that file under the section `production`, 
 - provide the generated key
 - provide your `server_name` (ex: example.com),
 - set `force_ssl` to `false`

6. Create a database, load seeds and compile assets.

Under the directory `releases/first` run

```
 bin/rake db:migrate RAILS_ENV=production
 bin/rake db:seed RAILS_ENV=production
 bin/rake assets:precompile RAILS_ENV=production
 ```

Start the server

```
bin/rails s -e production
```

Congratulations, your server should be now running on production mode :smile:
