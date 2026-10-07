# Senior Healthcare Advisors — Home Healthcare Funnel (form variant)

A new landing page being tried alongside [`senior-healthcare-advisors-home-healthcare`](https://github.com/MohitJain-SHA/senior-healthcare-advisors-home-healthcare). Same advertorial format and shared images/logo/favicon, but a different `index.html`:

- **Static phone number**, not CallGrid-tracked. `var STATIC_PHONE` (~L1689) is `+19546974538` — (954) 697-4538. It's the only phone number in the file.
- **Medicaid gate.** The quiz asks whether the visitor has Medicaid; answering "yes" routes to a not-eligible screen (`NOT-ELIGIBLE SCREEN`, ~L1198) instead of the normal result screen, and the phone number, live counter, and activity toast are all hidden on that screen (`body.dq`, ~L1271).
- **No hold timer / reference number, no Netlify Forms backup** — both present in the main repo's current version, removed here.

## Callback form → LeadConduit

Same flow as the main repo: the callback form posts client-side to
`LEADCONDUIT_URL` (~L1839, `…/flows/66994a8f23597656567d0072/sources/6a95afc78b064357c8d72ef8/submit`),
mapping name/phone/email/ZIP plus ad click IDs (`fbclid`, `msclkid`, `gclid`) to
LeadConduit's field names. TrustedForm is wired up: the cert URL is read from the
hidden `xxTrustedFormCertUrl` field with a short retry.

## Meta Pixel

`autoConfig` is off, so only explicit events fire (~L1861):

- `PageView` on load
- `Contact` on a call-button click
- `SubmitApplication` on a successful callback form submit (matched to a server-side Conversions API event via a shared event ID)

`Lead` is intentionally not fired from this page — it's reserved for real inbound calls reported server-side by the call-tracking stack.

## Seeded display content — READ BEFORE RUNNING TRAFFIC

`SOCIAL_PROOF` (~L1666) holds values typed into the file rather than read from a live system:

| What | Notes |
|---|---|
| Rotating activity toast (bottom-right) | `SOCIAL_PROOF.activity` — **placeholder entries**. Replace with real, substantiated data or set to `[]` to disable. |
| Results-screen testimonial | `SOCIAL_PROOF.testimonial` — **placeholder quote**. Replace with a real documented customer quote, or set to `null` to hide. |

**"N visitors viewing this page now"** (`.live-counter`, header) is a random drifting number, not real traffic. Delete the block to remove it.

## Editing the hero image

Replace the file named `hero-photo.jpg` with a new image of the same name — no code changes needed.

## Deployment

Not yet connected to Netlify or Vercel — ask before assuming either.
