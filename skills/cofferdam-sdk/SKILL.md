---
name: cofferdam-sdk
description: Add "Sign in with Cofferdam" to an app with @cofferdam/sdk and verify its session attestations on a server. Use when a project imports @cofferdam/sdk or @cofferdam/sdk-react, mentions Cofferdam sign-in, a csa1 token or the Cofferdam API, or asks to integrate Cofferdam.
license: MIT
metadata:
  publisher: Cofferdam Inc.
  updated: "2026-11-23"
  docs: https://cofferdam.xyz/llms.txt
---

# Cofferdam SDK

For a coding agent working in a developer's project. One fact per bullet, each tagged `[built]` or `[planned]`: treat a planned fact as not available, and never write code that depends on it. The documentation pages win when they differ from this file.

## Read the docs first

- [built] The SDK is pre-release (version 0.0.0) and changes. Fetch `https://cofferdam.xyz/llms.txt` before writing Cofferdam code, then the Markdown pages it lists; any docs page is Markdown at its address plus `.md`.
- [built] Prefer what the pages say over what you remember about Cofferdam, and tell the user when the pages do not cover what they asked.

## Install

- [built] The packages are not on npm. Install from the release branches: `"@cofferdam/sdk": "github:cofferdamxyz/cofferdam-sdk#release-core-main"` and, for React, `"@cofferdam/sdk-react": "github:cofferdamxyz/cofferdam-sdk#release-react-main"`. Never run `npm install @cofferdam/sdk`.
- [built] `ethers` version 6 is needed by every entry point except `@cofferdam/sdk/mock`.
- [planned] Packages on npm. Until then there is no fixed version to pin: a release branch holds one commit, replaced on every publish.

## Sign-in

- [built] Start with the mock: `new MockProvider({ scope, ...mockProfiles['verified-br'] })` from `@cofferdam/sdk/mock`, then `await provider.signIn(policy)`. It needs no network, account or key.
- [built] `scope` is the app's own identifier. The response's `appPseudonym` is the user's id in that app only; use it as the user key.
- [built] Do not store `accountAddress`. It changes between contract generations and is expected to leave the response.
- [planned] A pseudonym that stays the same across contract generations. Today it is derived from the account address, so keep the user record easy to re-key.
- [built] The app never receives a name, an email, a passport or a phone number, so do not design a schema that expects them from Cofferdam.
- [built] A sign-in the policy refuses throws `SignInRejected` with a `code` (`country_blocked`, `self_verification_failed`, `cancelled`, `no_passkey`, `unknown`).
- [built] In React: `CofferdamProvider` with a stable `config`, then `SignInWithCofferdamButton` or `useSignInWithCofferdam()`. With React Server Components (the Next.js App Router), these go in a file that starts with `'use client'`.
- [built] Read a failed sign-in from the hook's `error`, not from the button's `onError`.
- [planned] Signing real users in from a third-party app. Today a real passkey sign-in runs on Cofferdam's own surfaces only, so ship integrations on the mock network and say so to the user.
- [planned] Country and claim policies on real accounts; today they can be tested with the mock profiles only.

## Verify on the server

- [built] Never trust a session on the client's word. Send `session.attestation` (a `csa1:` token) to the server and verify it there with `decodeAndVerifySessionAttestation(token, { expect: { scope }, maxAgeMs })`.
- [built] Always pass `expect.scope` and `maxAgeMs`: a version 1 token has no audience and no expiry of its own.
- [built] A valid token proves its key signed those fields, not that the key controls the account. To rely on the account, call `POST https://cofferdam.xyz/v1/session/verify-attestation` with `checkOnChain: true` and read `authority.status` (`active`, `not_found`, `account_not_deployed`, `chain_unsupported`, `check_failed`).
- [built] From a server that is not JavaScript, use that HTTP route for the whole check. Call it from a server; it does not answer cross-origin requests from web pages.
- [built] In tests, make real tokens with `signSessionAttestation({ signer: new DeterministicPasskeySigner('test-user'), fields })`. That signer is for tests only.
- [built] The mock provider's `attestation` is the placeholder `mock-attestation-v1` and does not verify; do not treat a verification failure on it as a bug.
- [built] `sessionToken` is unsigned and informational. Never authorise anything with it.

## Money

- [built] A session token never moves money. Do not build a flow that sends a user's funds from a session; sending money takes the user's passkey confirmation on a Cofferdam surface.
