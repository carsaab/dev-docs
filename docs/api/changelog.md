---
title: Flat REST API Changelog
description: Discover the latest changes of Flat's REST API
permalink: api/changelog.html
pid: api-changelog
---

We regularly update our API and services, you can discover all the changes to our public specification. While we try to avoid breaking changes during the Beta Period, they will be written below in bold if any.

## v2.5.1 (2018-01-16)

* fix(user): Add missing escape in `pattern` (`UserCreation.username`).

## v2.5.0 (2017-10-22)

* feat(scores): Add video & audio tracks support for scores: `/v2/scores/{score}/tracks`.

## v2.4.0 (2017-10-02)

* feat(scores): New metadata and update of `PUT /v2/scores/{score}`:
  * Added metadata in API `subtitle`, `lyricist`, `composer`, `description`, `tags`, `creationType`, `license`, `licenseText`, `durationTime`, `numberMeasures`, `mainTempoQpm`, `publicationDate`.
  * `PUT /v2/scores/{score}`: Remove `title` property, this one can be updated by saving a new revision of the score data.
  * `PUT /v2/scores/{score}`: New settable properties: `description`, `tags`, `creationType`, `license`.

## v2.3.0 (2017-08-28)

* feat(user): Add profile theme and instruments played.
* feat(edu): Add new cursor-based pagination for `GET /v2/organizations/users` and `GET /v2/organizations/invitations`.
* feat(edu): Add new methods:
  * `PUT /v2/organizations/users/{user}`: Admin endpoint to update managed accounts.
  * `DELETE /v2/organizations/users/{user}`: Admin endpoint to delete or convert edu accounts to consumer accounts.
* feat(edu): Classes have a new state `inactive` that can be activated using the new method `POST /v2/classes/{class}/activate`.
* feat(edu): Assignments have a new state `draft` and can have a new attachment type `exercise`.
* feat(edu): Return Canvas LMS Instance domain in classes details
* feat(edu): Return Clever.com section information in classes details

## v2.2.0 (2017-07-02)

* feat(edu): Public release of the first education APIs:
  * `/v2/classes`: Classes management
  * `/v2/classes/{class}/assignments`: Flat Assignments and Submissions
  * `/v2/organizations/users`: Organization accounts management
  * `/v2/organizations/invitations`: Organization invitations for admins and teachers
  * `/v2/organizations/lti/credentials`: LTI credentials management
  * `/v2/groups/{group}` and `/groups/{group}/users`: List of groups and users part of groups
  * `/v2/scores/{score}/submissions`: Submissions linked to a score
* feat(edu): New OAuth2 scopes:
  * `edu.classes`: Full, permissive scope to manage the classes.
  * `edu.classes.readonly`: Read-only access to the classes.
  * `edu.assignments`: Read-write access to the assignments and submissions.
  * `edu.assignments.readonly`: Read-only access to the assignments and submissions.
  * `edu.admin`: Full, permissive scope to manage all the admin of an organization.
  * `edu.admin.lti`: Access and manage the LTI Credentials for an organization.
  * `edu.admin.lti.readonly`: Read-only access to the LTI Credentials of an organization.
  * `edu.admin.users`: Access and manage the users and invitations of the organization.
  * `edu.admin.users.readonly`: Read-only access to the users and invitations of the organization.
* fix(spec): Add missing scopes in specification for `GET /v2/scores/{score}/revisions/{revision}` and `GET /v2/scores/{score}/revisions/{revision}/{format}`

## v2.1.0 (2017-04-17)

* feat(scores): Add support of private links sharing with `sharingKey`.
* feat(comments): Make "revision" optional when creating comments and support of "last" keyword.
* fix(revisions): Missing `id` property in `ScoreRevision`.
* update(spec): Specify `binary` response type for `GET /v2/scores/{score}/revisions/{revision}/{format}`

## v2.0.0 (2017-04-10)

* chore(api): First API public release with `/v2/me`, `/v2/scores`, `/v2/users` and `/v2/groups`.
