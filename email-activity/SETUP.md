# Email to activity: setup (v2.5.0-email.2, test)

Option A: BCC or forward an email to a tracker address and it becomes an activity.

| Send to | Activity added |
|---|---|
| `yourname+tracker@yourdomain` | In Progress |
| `yourname+done@yourdomain` | Complete |

The activity text is the email subject (Re:/Fwd: removed), so put the SIM serial in the subject, e.g. `R-MCX-100251 revised VDR for sign-off`. For a shared serial, add the aircraft: `R-FMX-100954 DA42 …`, `FMX-0297 P2006T …`. The data file is public, so write subjects you're comfortable having visible.

## 1. Put the test tracker on the branch

Upload to the `email-activity` branch, same places as on main:

- `index.html` and `changelog.html` at the top
- `admin/project-management.html`

These read `email-activity` instead of `main`. The live site doesn't change.

## 2. Create a GitHub token for the script

GitHub → Settings → Developer settings → Fine-grained tokens → Generate new token.

- Repository access: Only select repositories → `Certification-Projects`
- Permissions → Contents: Read and write
- Pick an expiry date, and note it

Copy the token.

## 3. Create the Apps Script

1. Go to script.google.com while signed in to your work Google account → New project. Name it "Tracker email".
2. Replace everything in `Code.gs` with the provided `Code.gs`, then save.
3. Project Settings (gear icon):
   - Time zone: your local time zone (activity times use it)
   - Script Properties → Add property: `GITHUB_TOKEN` = the token from step 2

## 4. Test

1. Send yourself a test email with BCC `yourname+tracker@…` and a subject like `R-MCX-100251 test from email`.
2. In the editor, choose `dryRun` and click Run. The first time, Google asks you to allow Gmail and external requests. Approve it.
3. Check the Execution log. It should say "[dry run] would add In Progress to c-skyborne: …". Nothing is written yet.
4. Choose `processTrackerEmails` and Run. The activity appears on the branch with an Email marker. The thread gets the label Tracker/Logged.

## 5. Turn on the timer

Choose `installTrigger` and Run once. From then on it checks every 15 minutes. `removeTrigger` stops it.

## Viewing the branch

GitHub Pages only shows main. To see the branch, download its files, keeping the folders. Open `index.html` from your computer, then sign in with the gear icon. Make sure the Branch field says `email-activity`. If you've signed in before in that browser, it may still show `main` from then, so change it.

Check the footer on both pages. It should read `v2.5.0-email.2 · Branch: email-activity`. If the editor says `Branch: main`, it's reading the live data. Sign in again from the gear with `email-activity`. If the footer says v2.4.6, you've opened the live site rather than the branch files.

## When an email isn't logged

The thread gets the label **Tracker/Unmatched**. This happens when the subject has no serial, has two serials, or has a shared serial without the aircraft. Add that activity in the editor by hand. The script won't retry it.

## Merge checklist (when ready for v2.5.0)

1. In `index.html`, set `branch: 'email-activity'` back to `'main'`.
2. In `Code.gs`, set `branch: 'email-activity'` to `'main'`.
3. Reset the branch's `data/certifications.json` to match main, so test activities don't overwrite live data.
4. Change the version label to `v2.5.0`, remove the branch label next to it in the footer, and update the change log entry.
5. Merge the page files.
