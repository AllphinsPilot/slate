---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.allphins.com/'>Sign Up for a Developer Key</a>

includes:
  - authentication
  - pagination
  - limits
  - users
  - portfolios
  - policies
  - exposures
  - datasources
  - risk_exports
  - scenarios
  - assets
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Allphins API
---

# Introduction

> Base URL

```
https://api.allphins.com/api/v1/
```

The Allphins API is organized around REST. It uses resource-oriented URLs and standard HTTP verbs and status codes. It accepts JSON request bodies and returns JSON responses.

This reference describes the available endpoints and how to call them.

## Conventions

| Topic | Rule |
|-------|------|
| Base URL | Every endpoint is under `https://api.allphins.com/api/v1/`. There is no other API version. |
| Trailing slash | Every path ends with `/`. Without it, the server answers with a redirect, and most HTTP clients drop the body of a `POST` or `PATCH` when they follow it. |
| Request bodies | Send JSON with the header `Content-Type: application/json`. |
| Dates | `YYYY-MM-DD`, for example `2025-01-31`. |
| Timestamps | ISO 8601, in UTC. |
| Authentication | Every request except the token endpoints needs an `Authorization: Bearer` header. See [Authentication](#authentication). |

## Groups and lines of business

Your account belongs to one or more groups. Each group holds the data of one line of business (for example `energy_v2` treaty).

Each request uses the active group of the account. Lists, searches and computations only see the data of that group. To work on another line of business, switch the active group: see [Switch the active group](#switch-the-active-group).

The active group is a setting of the account, not of the access token. Switching it changes the scope of every later request made with that account, including requests running in parallel.

Where applicable, Allphins issues one API key per line of business.
