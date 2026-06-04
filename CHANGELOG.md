# Changelog — goodnet-security-null

All notable changes to this plugin are listed here. The format
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versions track the kernel ABI through `gn_security_vtable_t`.

## [Unreleased]

### `provides_flags` vtable slot + `link-only` provider

Added `provides_flags` lambda to the null vtable (returns `0` —
no cryptographic properties). Added a second full provider,
`link-only` (`kLinkOnlyProviderId = "link-only"`), whose
`allowed_trust_mask` is `1u << GN_TRUST_LINK_ENCRYPTED`;
registered as `"gn.security.link-only"` in `kProvidesList`. The
link-only provider covers transports that deliver link-layer
confidentiality (e.g. DTLS-offload) where upper-layer encryption
would be redundant but the trust class must still be constrained.

## [1.0.0-rc1] — 2026-05-08

Initial release. Loopback / IntraNode pass-through security
provider, extracted from the legacy in-tree `security/null` into
its own plugin git.

### Added

- `null` security provider — no-op handshake, no per-frame
  authentication, no rekey. Used on connections whose
  confidentiality and authenticity are already delegated to the
  underlying transport (AF_UNIX peer-cred check, loopback IP).
- `null_allowed_trust_mask = Loopback | IntraNode`. The
  protocol layer rejects every other trust class on connect, so
  an operator misconfiguration that loaded the `null` provider
  on a WAN transport cannot silently downgrade the connection.
- Entry symbols routed through `GN_PLUGIN_*_NAME` macros for
  static-link suffix consistency with the rest of the plugin
  surface.
