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

```shell
po template -p aaa94297acfc episode
```

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

#### validate the feed

Test to see if the `feed.xml` document can be accessed:

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
