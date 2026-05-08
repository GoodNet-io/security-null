# goodnet-security-null

Loopback / IntraNode pass-through security provider. Used on
trust-class connections where confidentiality and authenticity are
delegated to the underlying transport (AF_UNIX peer-cred check,
loopback IP). Refuses to upgrade Untrusted connections — production
deployments do not load this plugin on public-facing transports.

**Kind**: security provider · **Artefact**: dynamic plugin (`.so` via
dlopen) · **License**: MIT (see `LICENSE`)

## Build

This plugin lives in its own git with a flake that pulls the
kernel SDK as a Nix input. From this checkout:

```sh
nix run .#build         # release build of libgoodnet_security_null.so
nix run .#test          # vanilla ctest
nix run .#test-asan     # AddressSanitizer + UBSan
nix run .#test-tsan     # ThreadSanitizer
```

The kernel monorepo also builds this plugin in-tree through its
own `nix run .#build -- release` — operator install consumes
every bundled `.so` from there.

## Load

Manifest entry pins the SHA-256 digest; `gn_plugin_init` registers
the `null` provider. See `docs/install.en.md` and
`docs/contracts/plugin-manifest.en.md` in the kernel tree.

## Contract

- Kernel-side trust-class policy: `docs/contracts/security-trust.en.md`
- `null_allowed_trust_mask = Loopback | IntraNode`; the protocol layer
  rejects any other trust class on connect.
