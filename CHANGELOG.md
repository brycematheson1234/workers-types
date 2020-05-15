# Changelog

## 1.0.10

- ### Features

  - **Add Cache behavior modifiers to outbound Requests - [@trjstewart], [issue/22] [pull/17]**

    When constructing a request, you can now include the following cache-manipulating properties in the initializer dictionary:
    ```typescript
    // Force response to be cached for 300 seconds.
    fetch(event.request, { cf: { cacheTtl: 300 } })

    // Force response to be cached for 86400 seconds for 200 status codes, 1 second for 404, and do not cache 500 errors
    fetch(request, { cf: { cacheTtlByStatus: { '200-299': 86400, '404': 1, '500-599': 0 } } })
    ```
    Read more about these properties in the [`Request` docs](https://developers.cloudflare.com/workers/reference/apis/request/).

    [@trjstewart]: https://github.com/trjstewart
    [pull/17]: https://github.com/cloudflare/workers-types/pull/17
    [issue/22]: https://github.com/cloudflare/workers-types/issues/22

  - **Add support for `caches.default` - [@ispivey], [@ashnewmanjones], [issue/8] [pull/21]**

    The Workers runtime exposes a default global cache as `caches.default`, accessed like:
    ```typescript
    let cache = caches.default
    ```
    This is an extension to the [Service Workers spec for `CacheStorage`](https://w3c.github.io/ServiceWorker/#cachestorage), and thus needed to be added explicitly to our type definitions.

    [@ispivey]: https://github.com/@ispivey
    [@ashnewmanjones]: https://github.com/@ashnewmanjones
    [pull/21]: https://github.com/cloudflare/workers-types/pull/21
    [issue/8]: https://github.com/cloudflare/workers-types/issues/8

  - **Add missing properties to inbound `Request` `cf` object - [@ispivey], [issue/23] [pull/24]**

    Adds:
    * `clientTcpRtt`
    * `metroCode`

    Makes most geolocation properties optional, because they are not guaranteed to be set on every request.

    Changes the type of `asn` from string to number.

    [@ispivey]: https://github.com/@ispivey
    [pull/24]: https://github.com/cloudflare/workers-types/pull/24
    [issue/23]: https://github.com/cloudflare/workers-types/issues/23

  - **Adds `cacheKey` property to outbound `Request` constructor - [@ispivey], [issue/22] [pull/28]**

    Adds the `cacheKey` property.

    [@ispivey]: https://github.com/@ispivey
    [pull/28]: https://github.com/cloudflare/workers-types/pull/28
    [issue/22]: https://github.com/cloudflare/workers-types/issues/22

  - **Allow passing another Request as the `init` arg to `Request` constructor - [@ispivey], [issue/15] [pull/18]**

    Previously, this pattern wasn't allowed:
    ```typescript
    new Request(parsedUrl.toString(), request)
    ```
    This is because the `cf` object on inbound Request objects, and that expected in the `init` dictionary arg to the Request constructor, have a different shape.

    This change creates explicit `CfRequestProperties` (inbound) and `CfRequestInit` (outbound) interfaces, and updates the `Request` constructor to accept either type:
    ```typescript
    interface RequestInit {
        cf?: CfRequestInit|CfRequestProperties
    }
    ```

    Read more about the `Request` constructor in the [`Request` docs](https://developers.cloudflare.com/workers/reference/apis/request/).

    [@ispivey]: https://github.com/ispivey
    [pull/18]: https://github.com/cloudflare/workers-types/pull/18
    [issue/15]: https://github.com/cloudflare/workers-types/issues/15
  
  - **Add iterable methods to FormData, Headers, and URLSearchParams - [@ispivey], [issue/25] [pull/26]**
  
    The iterable methods `entries()`, `keys()` and `values()` are not present on these three types in `lib.webworker.d.ts`. They are instead supplied in `lib.dom.iterable.d.ts`.

    However, as discussed in this issue on the TypeScript repo, `lib.dom.d.ts` and `lib.webworker.d.ts` have conflicting type definitions, and the maintainers hope to solve this issue by refactoring shared components into a new `web.iterable.d.ts` lib: https://github.com/microsoft/TypeScript/issues/32435#issuecomment-624741120

    Until then, we will include the iterable methods supported by Workers in our own type definitions.

    [@ispivey]: https://github.com/@ispivey
    [pull/26]: https://github.com/cloudflare/workers-types/pull/26
    [issue/25]: https://github.com/cloudflare/workers-types/issues/25

- ### Maintenance

  - **Add a Releast Checklist - [@ispivey], [issue/20] [pull/27]**

    As we onboard more contributors, we're documenting release procedures.

    [@ispivey]: https://github.com/@ispivey
    [pull/27]: https://github.com/cloudflare/workers-types/pull/27
    [issue/20]: https://github.com/cloudflare/workers-types/issues/20