# Deploy DAN Metadata Catalog

Deploy NADA with Docker for DAN Metadata Catalog.
Complete rewriting of official IHSN docker for Nada : https://github.com/mah0001/nada-docker


## Prerequisites
- Docker installed
- MariaDB or MySQL database 
- SSH keys for Github configured (and have access rights)

## Installation

### Get Github files
From your base install folder,

```
git clone https://github.com/vbenoit-dan/dan-metadata-catalog-deploy.git
```

Will get the deploy code in `./dan-metadata-catalog-deploy` folder.

```
cd dan-metadata-catalog-deploy
```

Get the NADA source code in the `nada_source_code` folder.

```
git clone https://github.com/ihsn/nada.git nada_source_code
```

### Build the container image
Folder : `./dan-metadata-catalog-deploy`


First, check your uid and gid

```
id
```
Should output something like this

```
uid=1000(vbenoit) gid=1000(vbenoit) .......
```

Edit the `Dockerfile-nada` to reflect those uid and gid.

```
ARG USER_UID=1000
ARG USER_GID=1000
```

Then, create a `.env` file from the provided `.env.template` file.
```
cp .env.template .env
```

Edit the `.env` file to match your database settings

```
DB_HOST: host.docker.internal
DB_DATABASE: nada_database
DB_USERNAME: your_db_username_here
DB_PASSWORD: your_db_password_here
```

Build your container image

```
docker compose build --no-cache
```

Should finish with no error.

### Fix persistent storage user rights
Folder : `./dan-metadata-catalog-deploy`

Note : change with your username
```
sudo chown -R myusername:docker ./persistent_data/
sudo chmod -R 755 ./persistent_data/

```

### Start the container
Folder : `./dan-metadata-catalog-deploy`

```
docker compose up -d
```

Should run with no error.

With your browser, open `localhost:8383`.

The NADA installation page should appear.

Check that every check is cleared at `yes`.


### Optional : apply patches for branding and custom pages

Folder : `./dan-metadata-catalog-deploy`

Get the patch files from github into the `nada_patch` folder.

```
git clone git@github.com:benoit-vincent/nada_tweaks-for-dan.git nada_patch
```

Get the container id for NADA.

```
docker ps
```

Go to folder : `./dan-metadata-catalog-deploy/nada_patch`

```
./patch_container.sh yourcontainerid
```

E.g
```
./patch_container.sh ed0e7c6f7920
```



