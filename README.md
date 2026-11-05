# Digital Farm Management

**One QR scan. Every detail of every animal.**

A farm management app for dairy farmers. Every animal gets a QR tag that opens its own digital dashboard, and a farm-wide supplies dashboard tracks feed and medicine. Smart alerts warn about vaccinations, calving, dry-off dates and low stock.

Built for the National Milk Day event as a team pitch prototype.

## The problem

- **Scattered records:** animal details live in notebooks, memory and loose papers.
- **Missed schedules:** vaccination, calving and dry-off dates are easy to forget.
- **Supply surprises:** feed and medicine run out without warning.

Result: lower milk yield, higher animal health risk and avoidable costs.

## What the app does

### Animal dashboard (opens from the QR tag)
| Section | What it shows |
|---|---|
| Identity | Tag ID, name, breed, age |
| Body weight | Current weight, growth per month, trend chart |
| Feeding | Daily ration (green, dry, concentrate) and feeding times |
| Vaccination | Last shot date and next due date for each vaccine |
| Reproduction | Day of gestation, expected calving date |
| Lactation | Stage, days in milk, daily yield, dry-off date |

Each section can be updated in place: log a weight, log milk, record a shot, record a service or calving, and change a ration.

### Farm supplies dashboard
- Feed stock, daily use and **days of feed left**.
- Medicine and vaccine stock with expiry dates.
- Add and Use buttons to adjust stock.
- Herd-wide vaccination status for each vaccine.

### Screens
| Tab | Purpose |
|---|---|
| Today | Milk total, calvings this month, shortcuts, and everything that needs attention |
| Herd | Searchable list of animals, add an animal, view QR tags |
| Milk | Enter today's litres for every milking animal in one go |
| Supplies | Feed, medicine and herd-wide vaccination |
| Scan | Read a QR tag from a photo, or type a tag number |

## How the QR tags work

1. Each animal has a unique tag ID such as `DF-101`.
2. The QR code encodes the app address plus the tag, for example `https://<app-address>#df-101`.
3. Scanning opens the app directly on that animal's dashboard.

In the prototype, the Scan tab reads a photo of a tag. Live camera scanning and scanning with the phone's normal camera app need the app hosted at its own address.

## How reminders are calculated

The next due date is never typed in. It is always the **last shot date plus the interval**, so recording a shot moves the next due date forward automatically.

| Vaccine | Interval |
|---|---|
| FMD | 180 days |
| HS + BQ | 365 days |
| Deworming | 90 days |

Alert rules (amber means a warning, red means urgent):

| Alert | Amber | Red |
|---|---|---|
| Vaccination | Due within 14 days | Overdue |
| Calving | Within 30 days | Within 7 days |
| Dry-off | Within 14 days | None |
| Feed | Under 7 days left | Under 3 days left |
| Medicine stock | At or below minimum | Out of stock |
| Medicine expiry | Within 60 days | Expired |

Reproduction dates use these gestation lengths: cattle about 280 to 283 days, buffalo about 310 days. The dry-off date is 60 days before expected calving.

## Data model

| Record | Key fields |
|---|---|
| Animal | tag, name, breed, date of birth, gestation length |
| Weight log | date, kg |
| Milk log | date, litres |
| Ration | green, dry and concentrate kg, feeding times |
| Vaccination | vaccine, date given |
| Reproduction | service date, calving date |
| Feed item | name, stock, daily use, unit |
| Medicine item | name, quantity, unit, minimum, expiry date |

## Current status: prototype

- Runs as a single web page (HTML, CSS and JavaScript). No server needed.
- Uses two small libraries: `qrcode-generator` to create QR codes and `jsQR` to read them.
- Data is saved in the browser only (`localStorage`), so it is not shared between phones or people.
- The animals and stock are **sample data**, generated relative to today's date. "Reset sample data" on the QR tags page restores them.

### Known limits
- Alerts show only when the app is open. There is no WhatsApp, SMS or push message yet.
- No logins, so there is one farm and one user.
- No live camera scan.
- English only.
- No offline support.

## Roadmap

1. **Backend and logins:** a shared database (for example Supabase or Firebase) with phone OTP login.
2. **Real reminders:** a daily scheduled check that sends WhatsApp or SMS messages through a provider such as Twilio, Gupshup or MSG91.
3. **Whole-herd actions:** record a vaccine for every animal in one tap.
4. **Hosting at its own address:** so printed QR tags open the right animal from any phone camera.
5. **Languages:** Hindi and other regional languages.
6. **Offline mode:** for farms with weak signal.
7. **Roles:** owner, worker and vet.
8. **Reports:** milk per animal, feed cost, breeding performance.
9. **Printable tags:** waterproof ear or collar tags made from the generated QR codes.

## Suggested tech stack for the full version

| Part | Choice |
|---|---|
| App | Progressive web app (React or Next.js) |
| Database and login | Supabase (PostgreSQL) or Firebase |
| QR codes | `qrcode-generator` or similar |
| Scanning | Phone camera through a web QR library |
| Reminders | Scheduled daily job plus a messaging provider |
| Hosting | Vercel or Netlify |

## Team pitch summary

- **Healthier animals:** timely vaccination and breeding care.
- **Higher productivity:** better tracking of lactation and yield.
- **Lower costs:** less wastage and no emergency buying.

**Next step:** a working prototype with sample animals and live QR codes.
