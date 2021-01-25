---
repo: wq.app
date: 2016-03-21
---

# wq.app 1.0 alpha

**wq.app 1.0.0a1** is an alpha release of the upcoming 1.0 version of [wq.app](https://wq.io/wq.app).  A number of components and configuration options were refactored as part of the overall "patterns API cleanup" discussed in the [wq.db 0.8 release notes](./wq.db-0.8.0.md).

## `wq.db.patterns` API Refactor
- Update to support the new [XLSForm](http://xlsform.org)-style configuration object generated by the serializers in [wq.db 1.0.0a1](./wq.db-1.0.0a1.md) (#38)
- Remove references to `wq.db.patterns` models (wq/wq.db#35)
- Better handling of nested forms/attachments in outbox (#56)

## API Cleanup
- Simplify plugin `run()` arguments (#58)
- Add testing framework and Travis CI (#13)
- Update `Leaflet` and `Leaflet.Draw` versions