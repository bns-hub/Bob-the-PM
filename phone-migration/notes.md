# Project notes — phone migration (Android foldable → iPhone)

Add new entries at the top, do not delete old ones.

> [!note] This repository is public
> Specific account names, carrier, bank and work application names are deliberately **not** recorded here. They live in the user's Obsidian vault at `01 Inbox/Fold5 to iPhone Migration Plan.md` and `01 Inbox/Fold5 to iPhone Runbook.md`. Read those on the day. Everything below is the procedure and the settled decisions, which is what a future session actually needs.

---

## 2026-09-20 Planning phase — plan and runbook complete, awaiting hardware

**What we already know, confirmed, not guessed:**

- Moving from a Samsung Galaxy Z Fold 5 to an iPhone. Hardware not yet in hand; **no migration steps have been executed.**
- Two finished documents live in the vault at `01 Inbox/`:
  - `Fold5 to iPhone Migration Plan.md` — chronological, plain language, what to do and when. **This is the one to walk through on the day.**
  - `Fold5 to iPhone Runbook.md` — reference only, exact taps per step. Do not read it linearly.
- **Carrier: already on an eSIM, on a 4G plan.** Confirmed after three contradictory answers, so trust this over anything earlier. No 5G upgrade needed — that requirement applies only to converting a *physical* SIM, which is not the situation.
- The eSIM profile can be installed a **maximum of 4 times ever** (first 2 free, small fee after). One install is already consumed by the old phone. Do not experiment with installs.
- The new iPhone is **eSIM-only** — no SIM tray. Apple's automatic Android→iPhone eSIM transfer **supports no Singapore carrier**, so the move is manual, via a QR code from the carrier.
- **Contacts are stored locally on the old phone**, not in any cloud. This is the single largest loss risk in the whole migration.
- **Microsoft Authenticator holds only three accounts.** Not a long list — do not rebuild a generic 2FA inventory.
  - MS Authenticator has **no Android→iOS restore path** — backup/restore is same-platform only. All three are being re-enrolled by hand into **Google Authenticator**.
  - Because re-enrolment is unavoidable regardless, switching apps costs nothing extra. This deliberately inverts the usual "don't change apps mid-migration" advice.
  - TOTP codes are time-based, so the old phone keeps producing valid codes on Wi-Fi with no SIM for the entire safety period. **There is no deadline on this work.**
  - One of the three has **no self-service 2FA recovery** — lockout means a support ticket and a wait. It is named in the vault note; handle it deliberately while the old phone still works.
- **Okta Verify is not in use.** Do not reinstall it and do not raise an IT ticket for it.
- Microsoft Authenticator will **not** be installed on the iPhone — none of the three accounts is a Microsoft work/Entra account.
- The Obsidian vault is a plain local folder on the laptop with no git. The user intends to enable a **Drive sync plugin**; one full sync must complete before the iPhone is set up, or the iPhone will create an empty vault.
- Move to iOS runs **during first boot only**. Tapping past that screen means erasing the iPhone to retry. Wired USB-C transfer, not wireless — roughly 30 minutes versus several hours, and it does not drop.
- **Correct day order, and the order is the point:** big copy → verify → eSIM → apps that text you a code → work.
  - Rationale: activating the eSIM on the iPhone instantly kills the old phone's line, so every SMS-dependent re-verification must come *after* the eSIM move, and the big copy must come *before* it.

**What we are choosing to leave open, or unsure of, for now:**

- Whether the work CRM admin permits self-service 2FA reset, and whether the work learning platform sits behind company SSO. Two emails were recommended; answers not yet received.
- Whether Authy is still in use or dormant like Okta Verify. If dormant, retire rather than migrate.
- Whether to upgrade the mobile plan tier. Recommendation: only after the migration settles, never stacked on top of it.
- Whether the Drive sync plugin has been configured yet.
- Exact iPhone arrival date.

**The one goal for this phase:**

Be ready to run the port in a single unhurried session, with nothing irreplaceable existing only on the old phone at the moment it is wiped.

**Anything the next session should NOT re-ask, because it is already settled:**

- Carrier is already eSIM on a 4G plan. Do not re-litigate 4G vs 5G — it is irrelevant to migration.
- The authenticator list is three accounts. Do not rebuild a generic inventory.
- Google Authenticator is the chosen destination.
- Okta Verify is out of scope.
- Contacts are local-only; the triple-backup approach (`.vcf` export + Google Contacts copy + count check) is decided.
- **The user wants plain, non-technical language.** Earlier drafts were rejected for reading like an architecture document. No tiers, no gates, no jargon. Conversational, in time order.

**How to guide on the day:**

Open `01 Inbox/Fold5 to iPhone Migration Plan.md` and walk it top to bottom — it is already written in the order things happen. Use the runbook only when the user is standing at a step and wants exact taps. **Do not reorder the day.** The sequence exists to avoid the trap where the line moves to the new phone before the data copy and before SMS-dependent apps are re-verified.
