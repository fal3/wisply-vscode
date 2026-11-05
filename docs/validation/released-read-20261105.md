# Released Mac read-only validation

Wisply for Mac 1.11.0, build `202610210220`, passed the connector's read-only checks on November 5, 2026. This establishes 1.11.0 as the tested minimum for read-only access in the extension README. The recording-version placeholder still blocks a store release.

## Build identity

- App Store Connect version `d32a0d6e-8827-481a-bdfb-bd955b59b020` was Ready for Distribution with build `d87e3134-05bd-48bb-89ba-01f0dc6e754b` / `202610210220`.
- The installed app matched that version and build, with Apple Mac OS Application Signing, Team ID `97K577ZYQT`, and CDHash `8aaf336ab4ca140208c9a89e0380a9cf7f5fb36f`. Strict/deep signature verification passed.
- Executable SHA256: `f87a5b7414a65ecb0b8b597298e6120f01364b8c1913c1d4cb8298a00e089621`.
- The identity check verified that this executable owned port 51771 before connecting. The compiled source commit was not established.

## Observed results

| Check | Result |
| --- | --- |
| Existing read-only token from the login Keychain | Connector-scope HMAC proof passed, HTTP 200, 26 ms |
| Fresh valid-nonce public identity request | HTTP 200, 2 ms; `connector` and `full` proof scopes |
| Real stdio initialization and tool listing | Passed; exactly four read-only tools |
| `list_memos`, limit 1 | Succeeded |
| `search_transcripts`, unique QA query | Succeeded |
| `get_live_transcript`, tail 1, while idle | Succeeded |
| `get_action_items` | Succeeded |
| Read-only token calling `start_recording` | Refused |
| Authenticated GET to `/mcp` | HTTP 405 |

The check retained result metadata and content-block counts. It did not save tokens, identity-proof bodies, memo text or action-item payloads.

## Remaining limits

This build advertised no `recording` identity proof, and its Settings page had no recording-only token row. Recording control stayed off throughout. A released build with that scope, bounded first-start/cancel behavior, and approved recording QA is still needed before choosing the recording minimum.

Real editor chat, quote insertion, remote-window behavior and full-access flows remain unverified. This check did not repeat port-exclusivity or offline/impostor checks from the earlier beta validation. Both MCP switches were restored off, the app was quit, and port 51771 had no listener afterward. No package or extension was published.
