---
title: Flat REST API Changelog
description: Discover the latest changes of Flat's REST API
permalink: api/changelog.html
pid: api-changelog
---

We regularly update our API and services, you can discover all the changes to our public specification. While we try to avoid breaking changes during the Beta Period, they will be written below in bold if any.

## 2017-04-11

* feat(comments): Make "revision" optional when creating comments and support of "last" keyword.
* fix(revisions): Missing `id` property in `ScoreRevision`.
* update(spec): Specify `binary` response type for `GET /scores/{score}/revisions/{revision}/{format}`

## 2017-04-10

* chore(api): First API public release with /v2/me, /v2/scores, /v2/users and /v2/groups.
