---
title: "HTTP Signatures prove identity in federation without shared secrets"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "federation", "security", "cryptography"]
summary: "HTTP Signatures use public-key cryptography to verify that a federated message actually came from the server it claims to be from."
status: "growing"
---

In a [federated](/notes/federation/) network, when Instance B receives a message claiming to be from Instance A, how does it verify the claim? There are no shared passwords, no central authority, no OAuth provider. The answer is **HTTP Signatures** — a mechanism based on public-key cryptography.

## The Mechanism

Every instance generates a keypair. The private key stays secret; the public key is published in the actor document for anyone to read.

When Instance A sends a POST to Instance B's inbox:

1. **Select headers to sign**: typically `(request-target)`, `host`, `date`, `digest`, and `content-type`. The `digest` is a SHA-256 hash of the request body, ensuring body integrity.
2. **Create a signing string**: concatenate the selected headers into a canonical format.
3. **Sign** the string with the private key (RSA-SHA256), producing a base64-encoded signature.
4. **Attach a `Signature` header** containing the `keyId` (URL to the public key), the list of signed headers, and the signature.

On the receiving side, Instance B:
1. Parses the `Signature` header to extract `keyId` and signed headers
2. Fetches the public key from the `keyId` URL (cached after first fetch)
3. Reconstructs the signing string from the actual received headers
4. Verifies the signature against the signing string using the public key
5. Verifies the `Digest` header matches the body hash
6. Checks the `Date` header is within an acceptable window (~12 hours) to prevent replay attacks

Any failure means reject with 401 Unauthorized.

## Why This Works

- **No shared secrets needed** between instances (unlike API keys)
- **No central authority needed** (unlike OAuth with a central identity provider)
- **Each instance manages only its own keypair**
- **On-demand key discovery**: public keys are fetched and cached, so first contact with a new instance costs one extra HTTP call; subsequent interactions are zero-overhead
- **Body integrity**: the `Digest` header prevents tampering even through proxies

## Key Rotation

When rotating keys: generate new keypair, update the actor document, send an `Update` activity to followers. Remote instances with stale cached keys will fail verification, re-fetch the actor document, get the new key, and retry. This means signature verification failure should trigger one cache-refresh retry before permanent rejection.

## Security Considerations

- **Replay attacks**: prevented by the `Date` header — receivers reject requests older than ~12 hours
- **Actor spoofing**: prevented by fetching the public key from the claimed actor's domain and verifying the `keyId` domain matches the `actor` field's domain
- **All federation must use HTTPS** — HTTP Signatures protect integrity, but only HTTPS protects confidentiality

## Related Concepts
- [ActivityPub](/notes/activitypub/) — the protocol that relies on HTTP Signatures for authentication
- [Federation](/notes/federation/) — the network model where identity verification without central authority is essential
