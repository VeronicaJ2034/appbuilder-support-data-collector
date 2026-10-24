# `@sil/support-data`

Shared support-data definitions for the desktop collector and the Scriptoria
portal. The package is framework-independent except for
`SupportDataSelector.svelte`, which requires Svelte 5 as a peer dependency.

## Request contract

Collection requests are JSON objects with schema version `1`:

```json
{
  "requestId": "portal-request-id",
  "schemaVersion": 1,
  "supportUrl": "https://app.scriptoria.io/support/...",
  "requestedItems": ["project-config", "build-diagnostics"]
}
```

Use `parseCollectionRequest` or `validateCollectionRequest` for untrusted
input. Unsupported schema versions and unknown item IDs are rejected. Duplicate
known IDs are normalized by validation. Callers must treat the user's final
selection as authoritative; the request's `requestedItems` is only an initial
recommendation.

The package does not map IDs to files, collect credentials, or perform uploads.
The desktop application owns those operations and must maintain its own
allowlist. In particular, Android keystores and passwords are not represented
by this catalog.

## UI compatibility

`SupportDataSelector.svelte` uses DaisyUI semantic classes and does not bundle
Tailwind or DaisyUI CSS. Consumers must scan this package's Svelte files with
Tailwind CSS 4 and provide DaisyUI 5. This application uses the `dy-` DaisyUI
prefix, so the component follows that prefix for component classes.
