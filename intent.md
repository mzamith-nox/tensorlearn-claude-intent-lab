# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (PRD, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is history only: *"SUPERSEDED by approved PRD N-17 … This is historical context, not current approval."* Nothing below is approved by it alone.

## Problem

- **P-1** The clinic receives avoidable calls from patients checking whether their results are ready.
  > PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."

## Proposed outcome

- **I-1** Notify the patient when a result is ready for their review in the existing authenticated portal.
  > PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **I-2** "Ready" means a doctor has reviewed **and** released the result. A completed laboratory upload alone does not make a notification eligible.
  > PRD: "A doctor must review and release the result before any notification is eligible."
  > Discovery (product owner): "keep the PRD's review-before-notification gate. 'Ready' must not mean that the laboratory upload merely finished."
- **I-3** The notification only says that an update is available and directs the patient to the existing authenticated portal.
  > PRD: "The notification says that an update is available and directs the patient to the existing authenticated portal."

### How success will be observed

- **S-1** Results-status calls: count status-only calls separately from clinical questions, and compare them against a measured baseline.
  > PRD: "Measure reduction in results-status calls and delivery failures."
  > Discovery (support lead): "count status-only calls separately from clinical questions; compare against a measured baseline."
- **S-2** Delivery failures: count notification delivery failures.
  > PRD: "Measure reduction in results-status calls and delivery failures."
- **S-3** No numeric target and no baseline value are approved. Both are to be agreed during discovery. The early request's "90% call reduction in one week" is superseded and does not apply.
  > PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet."

## Affected users and systems

- **U-1** Patients: the only approved recipients of the notification.
- **U-2** Doctors: review and release results. Their release is the trigger for notification eligibility.
- **U-3** Support staff: handle the incoming calls and classify status-only calls versus clinical questions for the success measure.
- **U-4** Systems: the existing authenticated portal and its existing result route (engineering lead: "the existing portal path is adequate"); the result review/release status; the notification delivery path (channel still open, see OPEN-1); notification and operational logs.
- **U-5** Referring doctors are **not** approved recipients. Their notification is an open decision (OPEN-2).

## Out of scope

> PRD: "Portal redesign, appointment scheduling, clinical interpretation and notifying third parties are outside this approved scope."

- **X-1** Portal redesign.
- **X-2** Appointment scheduling.
- **X-3** Clinical interpretation.
- **X-4** Notifying third parties.
- **X-5** A new or rebuilt patient dashboard. Discovery (engineering lead): "A new patient dashboard is a separate project."
- **X-6** Superseded early-request scope: sending results as soon as the lab uploads them, including the test result in an SMS, copying the referring doctor, and the 90%-in-one-week target.

## Constraints

- **C-1** Review-before-notification: no notification is eligible until a doctor has reviewed and released the result (PRD, discovery notes, see I-2).
- **C-2** No clinical detail: no diagnosis, test result, clinical measurement or treatment detail in the message or in notification and operational logs.
  > PRD: "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs."
  > Discovery (privacy owner): "no sensitive content in messages or operational logs."
- **C-3** Authenticated access only: results are viewed in the existing authenticated portal. No attached results and no public result link.
  > PRD: "Authenticate access in the existing portal. Do not attach results or introduce a public result link."
- **C-4** Duplicate prevention: at most one notification per released result version.
  > PRD: "Avoid duplicate notifications for the same released result version"
- **C-5** Delivery-failure handling: handle delivery failures without sending duplicate patient messages.
  > PRD: "handle delivery failures without duplicate patient messages."
- **C-6** Use the existing portal result route.
  > PRD: "Use the existing portal result route."

## Open questions

- **OPEN-1 Channel** (SMS, email, portal push). Owners: **product owner** and **privacy owner**, after the consent and delivery review. Not decided here.
  > PRD: "Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review."
  > Discovery (privacy owner): "Consent and the actual channel are still under review."
- **OPEN-2 Referring-doctor notification.** Owners: **clinical owner** and **privacy owner**, who must decide whether the recipient is authorized. Not approved by the PRD, and not to be inferred from the superseded request.
  > PRD: "clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD."
  > Discovery (clinical owner): "do not infer approval for referring-doctor notifications from the old request."
- **OPEN-3 Baseline and numeric target** for the success measures: to be agreed during discovery (PRD). No owner is named in the sources, so none is assigned here.

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
