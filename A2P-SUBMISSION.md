# Keyferrals – A2P 10DLC go-live checklist

## 1. Confirm the legal name and address (done – taken from keyferrals.com)

The site uses **KeyFerrals LLC**, **7901 4th St N, STE 300, St. Petersburg, FL 33702**.
In the GHL brand registration, enter the same name, EIN, and address **exactly** as they appear on your IRS EIN letter
(CP 575 / 147C). If the IRS letter shows a different address, change it on the site too, since TCR rejects mismatches:

```bash
cd ~/Desktop/a2p
grep -rl --include='*.html' '7901 4th St N' . | xargs sed -i '' 's|7901 4th St N, STE 300, St. Petersburg, FL 33702|NEW ADDRESS HERE|g'
```

## 2. Connect the form (so submissions are actually captured)

Pick one:

- **GHL inbound webhook (easiest):** Automation → Workflows → new workflow → trigger "Inbound Webhook" → copy the URL →
  paste it into `data-endpoint=""` on the `<form>` in `index.html`. Fields sent: `first_name`, `last_name`, `email`, `phone`,
  `brokerage`, `service_area`, `message`, `sms_transactional_consent`, `sms_marketing_consent`, `consent_timestamp`, `source_url`.
  Map the two consent fields to custom fields so you have a record of each contact's consent.
- **GHL form embed:** build the form in Sites → Forms, add two **optional, unchecked** checkboxes using the exact consent text
  from `index.html`, then replace the `<form>…</form>` block with the GHL embed code. Keep the Privacy/Terms links under it.

## 3. Deploy (must be live on HTTPS before you submit)

The site is hosted on GitHub Pages (repo `devgrowthguild/A2PKEYFERALS`, branch `main`, folder `/ (root)`) at **keyferral.com**.
This file is excluded from the published site by `_config.yml`.

**Hostinger → Domains → keyferral.com → DNS / Nameservers → DNS records:**

1. Delete the existing `A` record for `@` (currently `195.35.39.91`), plus any `AAAA` record for `@`.
2. Add four `A` records, name `@`: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
3. Delete the existing `CNAME` for `www`, then add `CNAME` name `www` → `devgrowthguild.github.io`

**GitHub → repo Settings → Pages:** make sure the custom domain is `keyferral.com`, then click "Check again".
Once the check passes (usually 15 min to a few hours), tick **Enforce HTTPS**.

After deploying, check that these URLs load over HTTPS:

- https://keyferral.com
- https://keyferral.com/privacy-policy/
- https://keyferral.com/terms-and-conditions/

---

## 4. What to paste into the GHL A2P wizard

**Use case:** Mixed (Customer Care + Marketing). Pick *Low Volume Mixed* if you'll send under about 2,000 texts a day.

**Campaign description**
> Keyferrals is a real estate referral company that connects licensed real estate agents with verified buyer and seller opportunities. We send text messages to real estate agents and prospective partners who opt in through the form on our website (https://keyferral.com/#get-started). Messages include application follow-ups, onboarding information, referral notifications, appointment reminders, account updates and, for contacts who separately opt in to marketing, promotional offers and program updates.

**Sample message 1**
> Keyferrals: Hi {first_name}, thanks for applying to join the Keyferrals partner network! Your account specialist will reach out within 1 business day to confirm your service areas. Reply HELP for help, STOP to opt out.

**Sample message 2**
> Keyferrals: Reminder – your onboarding call with {rep_name} is scheduled for {date} at {time}. Reply to this message if you need to reschedule. Reply STOP to opt out.

**Sample message 3 (marketing)**
> Keyferrals: We've opened new referral territories in {city}. Want priority access? Reply YES and your account specialist will follow up. Msg & data rates may apply. Reply STOP to opt out.

**How do end users consent (message flow)**
> End users opt in by submitting the "Get Started" form at https://keyferral.com/#get-started. The form collects name, email, and an optional phone number. It shows two separate checkboxes, both unchecked by default and not required to submit the form: one for non-marketing messages (application follow-ups, referral notifications, appointment reminders, account updates) and one for marketing messages (promotional offers, program updates). Each checkbox names Keyferrals and states that message frequency varies, message & data rates may apply, and users can reply HELP for help or STOP to opt out. Links to our Privacy Policy (https://keyferral.com/privacy-policy/) and Terms & Conditions (https://keyferral.com/terms-and-conditions/) are shown directly below the checkboxes. SMS consent is not a condition of purchase. Opt-in data and consent are never shared with third parties.

**Privacy Policy URL:** https://keyferral.com/privacy-policy/
**Terms & Conditions URL:** https://keyferral.com/terms-and-conditions/

**Opt-in keywords:** START
**Opt-in confirmation message**
> Keyferrals: Thanks for subscribing to text updates! Msg frequency varies. Msg & data rates may apply. Reply HELP for help, STOP to opt out.

**Opt-out keywords:** STOP, UNSUBSCRIBE, CANCEL, END, QUIT
**Opt-out message**
> Keyferrals: You have been unsubscribed and will no longer receive messages from us. Reply START to resubscribe.

**Help keywords:** HELP, INFO
**Help message**
> Keyferrals: For help, email support@keyferrals.com or call (802) 416-3551. Msg & data rates may apply. Reply STOP to opt out.

**Other campaign attributes:** Embedded links: No · Embedded phone numbers: No · Age-gated: No · Direct lending: No · Affiliate marketing: No

(If you later add links or phone numbers to your messages, change those two answers to Yes.)

**Brand registration:** Use the same legal name, EIN, and address as step 1. Use an `@keyferrals.com` email (not Gmail).
