---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://www.allphins.com/'>Sign Up for a Developer Key</a>

includes:
  - assets
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

> Base url

```
https://app.allphins.com/api/v1/
```

The Allphins API is organized around REST. Our API has predictable resource-oriented URLs, accepts form-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

This API reference provides information on available endpoints and how to interact with it.

# Authentication

> To authorize, use this code:


```shell
# With shell, you can just pass the correct header with each request
curl https://app.allphins.com/api/v1/assets/
  -H "Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6"
```

> Make sure to replace `19a519fbcc3b5978f4d5a6405ca64d0344d274b6` with your API token.

Allphins uses API tokens to allow access to the API. You can register a new Allphins API token at our [developer portal](https://www.allphins.com/).

Allphins expects for the API token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Token 19a519fbcc3b5978f4d5a6405ca64d0344d274b6`

<aside class="notice">
You must replace <code>19a519fbcc3b5978f4d5a6405ca64d0344d274b6</code> with your personal API key.
</aside>
