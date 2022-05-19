## [Syncing Your Fork with Directus](https://docs.directus.io/contributing/introduction/#syncing-your-fork-with-directus)

### 1. Add Directus as a Remote

While your fork is your main remote or origin, you will add Directus as the upstream, which generally refers to the
original repo that you have forked.

```
git remote add upstream git@github.com:directus/directus.git
```

### 2. Fetch the Latest Changes

Depending on your setup, you will need to get the latest commits from the main branch of Directus by doing a pull,
reset, rebase or fetch.

```
git pull upstream main

```

## Build

```
cd docker
make build-images
```

then publish to dockerhub
