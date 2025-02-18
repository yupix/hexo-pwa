# hexo-pwa

[![npm version](https://badge.fury.io/js/%40teampimcserver%2Fhexo-pwa.svg)](https://badge.fury.io/js/%40teampimcserver%2Fhexo-pwa)


Progressive Web Apps (PWA) plugin for [Hexo](https://hexo.io/).

`hexo-pwa` let Hexo sites have these two  capabilities.

- [Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest) - Users can add your site to mobile home screen
- [Service Worker](https://developers.google.com/web/fundamentals/primers/service-workers/) - Make your site available offline


## CHANGELOG

- update README.md
- update yarn.lock
- update package.json


## Installation

```shell
# yarn
$ yarn add @teampimcserver/hexo-pwa
```

## Options

You can configure this plugin in `_config.yml`.

```yaml
pwa:
  manifest:
    path: /manifest.json
    body:
      name: hexo
      short_name: hexo
      icons:
        - src: /images/android-chrome-192x192.png
          sizes: 192x192
          type: image/png
        - src: /images/android-chrome-512x512.png
          sizes: 512x512
          type: image/png
      start_url: /index.html
      theme_color: '#ffffff'
      background_color: '#ffffff'
      display: standalone
  serviceWorker:
    path: /sw.js
    preload:
      urls:
        - /
      posts: 5
    opts:
      networkTimeoutSeconds: 5
    routes:
      - pattern: /hm.baidu.com/
        strategy: networkOnly
      - pattern: /.*\.(js|css|jpg|jpeg|png|gif)$/
        strategy: cacheFirst
      - pattern: /\//
        strategy: networkFirst
  priority: 5
```

- manifest - manifest configuration
	- path - the path of `manifest.json`, eg: `/manifest.json`
	- body - the content of `manifest.json`,  [manifest.json example](https://developer.mozilla.org/en-US/docs/Web/Manifest). `body` can be null, if not null, `hexo-pwa` will generate `manifest.json` with `JSON.stringify(body)`
- serviceWorker - service worker configuration
	- path: the path of `sw.js`, eg: `/sw.js`, you shouldn't put sw.js in subdirectory because of the [service worker scope](https://developers.google.com/web/ilt/pwa/introduction-to-service-worker#registration_and_scope)
	- preload - urls or posts that you want to preload
		- urls: an array of the preload urls
		- posts: the count of preload posts
    - opts: the options for [sw-toolbox](https://googlechromelabs.github.io/sw-toolbox/api.html#options)
	- routes - request routes and strategies, based on [sw-toolbox](https://googlechromelabs.github.io/sw-toolbox/#main). **The routes order does matter**.
		- pattern: url pattern, this config can be express-style or RegExp
		- strategy: the strategy you want to choose. [All strategies](https://googlechromelabs.github.io/sw-toolbox/api.html#options): `cacheFirst`, `networkFirst`, `cacheOnly`, `networkOnly`, `fastest`. Caution: Log requests should use `networkOnly` strategy.
- priority - [plugin priority](https://hexo.io/api/filter.html) (default value is 10)

## License

MIT
