# podops.example

This is a canonical example with some episodes showing how to build a podcast feed from simple markdown files using the podops.dev toolchain.

*Note: for details on how to install a podops.dev CDN node and how to install the `po` command line utility, head over to the [https://github.com/podops/podops](https://github.com/podops/podops) repo!*

### TL;DR

... or how to build a podcast in 5 steps:

```shell
cd /the/podcast/location

# prepare the podcast repo
po new
<... do some yaml editing>

# create an episode
po template episode
<... more yaml editing>

# build the feed
po build

# register the podcast with the CDN
po init

# sync the podcast with the CDN
po sync
```


### Step-by-step example

#### Step 1: initialize the podcast repository

Start with an empty directory and initialize the new podcast repository:

```shell
po new
```

This creates the `show.yaml` template, some other files and assignes the podcast its unique GUID. Open the `show.yaml` and change all the CAPITALIZED attributes.

```yaml
---
# /podops.example/show.yaml
apiVersion: v1
kind: show
metadata:
    name: NAME
    guid: aaa94297acfc
    date: Wed, 16 Feb 2022 09:00:16 +0000
    labels:
        block: "no"
        complete: "no"
        explicit: "no"
        language: en_US
        type: Episodic
description:
    title: TITLE
    summary: SUMMARY
    link:
        uri: https://podops.dev/NAME
        rel: external
    category:
        - name: Technology
          subcategory:
            - Podcasting
    owner:
        name: PODCAST OWNER(S)
        email: HELLO@PODCAST
    author: PODCAST AUTHOR(S)
    copyright: PODCAST COPYRIGHT
image:
    uri: cover.png
    rel: local

```

#### Step 2: create a first episode

Each podcast should have at least one (!) episode, let's create one:

```shell
po template -p aaa94297acfc episode
```

The `-p aaa94297acfc` refers to the podcast's unique ID, which links the podcast episode template to the show template.

```yaml
---
# episode-6d1048a48504.yaml
apiVersion: v1
kind: episode
metadata:
    name: episode-6d1048a48504
    guid: 6d1048a48504
    parent: aaa94297acfc
    date: Wed, 16 Feb 2022 09:30:30 +0000
    labels:
        block: "no"
        episode: "1"
        explicit: "no"
        season: "1"
        type: Full
description:
    title: EPISODE TITLE
    summary: EPISODE SUMMARY
    episodeText: EPISODE DESCRIPTION
    link:
        uri: https://podops.dev/PARENT-NAME/episode-6d1048a48504
        rel: external
    duration: 1
image:
    uri: episode.png
    rel: local
enclosure:
    uri: episode.mp3
    rel: local
    type: audio/mpeg
```

#### Step 3: build the feed

Each podcast requires an rss feed (`feed.xml` in podops), which is created from the `show.yaml` file and all the episodes resources.

```shell
po build
Sucessfully built podcast 'minimalpodcast'
```

#### Step 4: initialize the CDN

Before pubslishing the podcast, it has to be registered:

```shell
po init
```

#### Step 5: sync the local build with the CDN

Publishing the podcast is done with the `sync` command:

```shell
po sync
Sucessfully synced all resources
```

All the podcast assets will be transfered to the CDN. In order to test that the feed can be accessed, request the `feed.xml` document from its canonical URL:

```shell
curl https://podops.dev/minimalpodcast/feed
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd">
  <channel>
    <title>podops minimal podcast example</title>
    <link>https://podops.dev/minimalpodcast</link>
    ...
    ...
````
