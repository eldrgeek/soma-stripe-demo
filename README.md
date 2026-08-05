# soma-stripe-demo

Standalone, live proof of the `soma-stripe` portable component
(`~/Projects/SOMA/standards/soma-stripe/`) — a one-time donation via Stripe Checkout, **test
mode only**. This repo is just that package's files vendored as-is into a small static site +
two Netlify Functions, so there's one URL to click instead of a folder to read.

Built 2026-08-05 as a demo for Mark Inarai / James Crook of how a SOMA app stands up a
donation endpoint — not a sale, a "here's the mechanism working" click-through.

## Files

Everything under `netlify/functions/` and `public/vendor/soma-stripe/` is a verbatim copy of
`SOMA/standards/soma-stripe/`. If you're adopting the pattern for a real app, read that
package's README, not this one — this repo is the reference deployment, not the source of
truth.

- `public/index.html` — the donate button + explanation
- `public/thank-you.html` — Checkout success redirect target
- `netlify/functions/soma-stripe-checkout.ts` — creates the Checkout Session
- `netlify/functions/soma-stripe-webhook.ts` — signature-verified webhook skeleton (not wired
  up for this demo — v1 donation flow doesn't need it)

## The one Mike-gated step

Everything above is machine-built and deployable without any human click. **The one
irreducible step is a Stripe account.** Even Stripe's *test-mode* keys are account-scoped —
there is no publicly shared secret key that can create Checkout Sessions on anyone's behalf.
Signing up for that account (email + password, no business verification needed for test mode)
is exactly the "connecting an actual Stripe account" action reserved for Mike per this task's
constraint, tracked as Pulse Zero card `connect-soma-relay-stripe`.

Once that account exists:

```bash
cd ~/Projects/soma-stripe-demo
netlify link          # first time only — connects this dir to the Netlify site
netlify env:set STRIPE_SECRET_KEY sk_test_xxxxxxxx --context production
netlify deploy --prod
```

No code changes. The donate button starts redirecting to real Stripe Checkout in test mode
immediately.

## Test cards (Stripe-published, safe, no real money)

- `4242 4242 4242 4242` — succeeds
- `4000 0000 0000 0002` — declines

## Deploy

Netlify, `publish = "public"`, functions via `netlify/functions` (`node_bundler = "esbuild"`).
See `netlify.toml`.

---

_Authored 2026-08-05 by Mike Wolf (direction) + Dee (Sonnet 5, as COO)._
