# Changelog — goodnet-security-null

All notable changes to this plugin are listed here. The format
follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versions track the kernel ABI through `gn_security_vtable_t`.

## [Unreleased]

No functional changes since `v1.0.0-rc1`. Post-tag activity is
limited to documentation refs (`.md` → `.en.md` sweep across
plugin-side docs).

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
