# podops.example

This is a canonical example with some episodes showing how to build a podcast feed from simple markdown files.

### TL;DR

... or how to build a podcast in 5 steps:

#### Step 1: initialize the podcast repository

```shell
po new
```

```yaml
---
# /podops.example/show.yaml
apiVersion: v1
kind: show
metadata:
    name: NAME
    guid: 9ab34090a145
    date: Wed, 16 Feb 2022 14:40:58 +0000
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

```shell
po template -p 9ab34090a145 episode
```

```yaml
---
# episode-29624a9f8b23.yaml
apiVersion: v1
kind: episode
metadata:
    name: episode-29624a9f8b23
    guid: 29624a9f8b23
    parent: 9ab34090a145
    date: Wed, 16 Feb 2022 14:43:30 +0000
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
        uri: https://podops.dev/PARENT-NAME/episode-29624a9f8b23
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

```shell
po build
Sucessfully built podcast 'minimalpodcast'
```

#### Step 4: initialize the CDN

```shell
po init
```

#### Step 5: sync the local build with the CDN

```shell
po sync
Sucessfully synced all resources
```

