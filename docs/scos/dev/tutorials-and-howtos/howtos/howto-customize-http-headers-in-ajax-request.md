---
title: HowTo - Customize HTTP headers in AJAX request
last_updated: Jun 16, 2021
template: howto-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/ht-customize-http-headers-in-ajax-request
originalArticleId: 5c93f57b-4df4-40be-a588-4f288d94137b
redirect_from:
  - /2021080/docs/ht-customize-http-headers-in-ajax-request
  - /2021080/docs/en/ht-customize-http-headers-in-ajax-request
  - /docs/ht-customize-http-headers-in-ajax-request
  - /docs/en/ht-customize-http-headers-in-ajax-request
  - /v6/docs/ht-customize-http-headers-in-ajax-request
  - /v6/docs/en/ht-customize-http-headers-in-ajax-request
  - /v5/docs/ht-customize-http-headers-in-ajax-request
  - /v5/docs/en/ht-customize-http-headers-in-ajax-request
  - /v4/docs/ht-customize-http-headers-in-ajax-request
  - /v4/docs/en/ht-customize-http-headers-in-ajax-request
  - /v3/docs/ht-customize-http-headers-in-ajax-request
  - /v3/docs/en/ht-customize-http-headers-in-ajax-request
  - /v2/docs/ht-customize-http-headers-in-ajax-request
  - /v2/docs/en/ht-customize-http-headers-in-ajax-request
  - /v1/docs/ht-customize-http-headers-in-ajax-request
  - /v1/docs/en/ht-customize-http-headers-in-ajax-request
---

The `XMLHttpRequest` method `setRequestHeader()` sets the value of an HTTP request header. When using `setRequestHeader()`, you must call it after calling `open()`, but before calling `send()`. If this method is called several times with the same header, the values are merged into a single request header.

To add custom headers to the `ajax-provider.ts`, add `this.headers.forEach((value: string, key: string) => this.xhr.setRequestHeader(key, value));` into the promise of the `fetch` method.

{% info_block infoBox "Usage example" %}

```ts
this.ajaxProvider.headers.set('Accept', 'application/json'
);.
```

{% endinfo_block %}
