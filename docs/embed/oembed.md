---
title: Sheet music oEmbed
permalink: embed/oembed.html
pid: embed-oembed
---

[oEmbed](http://oembed.com/) is an open standard to easily embed content from oEmbed providers into your site. The Flat oEmbed endpoint serves our embed code for any Flat Score URL:

* ```https://flat.io/score/*```
* ```https://*.flat.io/score/*```

## Endpoint

Flat's oEmbed is available at the following URL:

```
https://flat.io/services/oembed
```

Basic example:

```bash
$ curl "https://flat.io/services/oembed?format=json&url=https%3A%2F%2Fflat.io%2Fscore%2F56ae21579a127715a02901a6"
{
    "version": "1.0",
    "type": "rich",
    "provider_name": "Flat",
    "provider_url": "https://flat.io",
    "author_url": "https://flat.io/flat",
    "author_name": "Flat Team",
    "width": 800,
    "height": 400,
    "title": "House of the Rising Sun",
    "html": "<iframe src=\"https://flat.io/embed/56ae21579a127715a02901a6?layout=page\" allowfullscreen height=\"400\" width=\"800\" frameBorder=\"0\"></iframe>"
}
```

## oEmbed Parameters

We support all the following standard parameters:

| Parameters | Required | Description | Default value |
|:-----------|:---------|-------------|---------------|
| `url` | required | The URL of the sheet music to be embedded ||
| `format` | optional | The returned format (`json` and `xml` are supported) | `json` |
| `maxwidth` | optional | The maximum width of a rendered score in whole pixels. This value must be at least `100`, otherwise the width will be 100px. | `800` |
| `maxheight` | optional | The maximum height of a rendered score in whole pixels. This value must be at least `100`, otherwise the width will be 100px. | `400`

## Custom [Embed URL Parameters](url-parameters.html)

If extra query parameters are specified in the oEmbed request or in the URL you are retrieving embedding information from, they will be passed to the final embed URL. This allows you to easily use all of your [Embed URL Parameters](url-parameters.html).

```
https://flat.io/services/oembed?format=json&url=https%3A%2F%2Fflat.io%2Fscore%2F56ae21579a127715a02901a6&layout=track
https://flat.io/services/oembed?format=json&url=https%3A%2F%2Fflat.io%2Fscore%2F56ae21579a127715a02901a6%3Flayout%3Dtrack
```
