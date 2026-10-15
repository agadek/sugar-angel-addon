# Changelog

## 0.5.0

No database changes since 0.4.0; a backup before updating is still a good habit. Nothing to do after updating.

- Forecast: four new methods that count fat and protein, by the Warsaw method (fat-protein units) and by Dr Bernstein's (a share of the protein), each added to the physiological method and to the ridge regression. They only make a difference for Meals logged with fat or protein. They are scored beside the others in Settings > Forecast methods and can be chosen as the active method; on Home they are hidden under "Fat and protein variants" until you show one.
- Log an Activity: Moderate intensity is preselected, and Save stays in sight on a small phone.
- Log a Dose: the form shows at once and fills in the Insulin when it arrives.
- Settings asks before you leave with unsaved changes, and "Forget this device" asks once more.
- Durations of Action profiles read in hours where that helps (for example 24 h).
- Home: the latest reading's value stands on a line from its point; the band shows better in dark mode; the Calibration line keeps up while Home is open.

## 0.4.0

**Before updating, make a Home Assistant backup that includes this add-on** (the update dialog offers one). This version adds five changes to the database; each only adds new tables or an optional column, and nothing already stored changes.

Nothing to do after updating: the Forecast trains itself, and Calibration makes its first check straight away.

- Forecast: three methods side by side on Home (ridge regression, a physiological method like OpenAPS and Loop, and flat). The active one is drawn with a band covering about 80% of its past errors; the others are lighter, each with its own line and marker, and can be hidden.
- Settings > Forecast methods: each method's scores from the last 1, 7 or 14 days beside its back-test, Lows warned of in time, false warnings, a recommendation with its rule, and your choice of the active method. The ridge regression stays active until you choose.
- Every forecast is recorded and scored when the readings arrive (kept 28 days). The Forecast trains itself daily and after a gap in the readings.
- Home's "Insulin acting" lane follows the active method's insulin curve.
- Calibration: proposals for Insulin sensitivity, Carb ratio and rapid-acting Insulins' Action profiles from the last 3, 7 and 14 days, each with its evidence; starting values from your total daily dose (new in Settings) while they are not set. Accept, adjust or dismiss them in Settings > Calibration; nothing changes until you do. Once a week by itself, or with "Check my ratios". Home shows a line while a proposal waits.

## 0.3.0

**Before updating, make a Home Assistant backup that includes this add-on** (the update dialog offers one). This version changes the database twice, and going back to an older version afterwards means restoring that backup:

- Insulin sensitivity and Carb ratio are now kept in time-of-day blocks. Your current figures become one block covering the whole day, and the old single-value columns are removed; tests check that nothing is lost.
- New tables for Calibration's proposals and the history of your values. They only add; nothing already stored changes.

**After updating, train the Forecast again** (Settings > Forecast > Train the forecast): it now forecasts every 15 minutes up to 4 hours ahead, and the old trained Forecast is not used.

- Forecast: every 15 minutes from 15 minutes to 4 hours ahead, drawn on Home as a dashed line with a dot every half hour.
- Home: under the graph, "Carbohydrates absorbing" and "Insulin acting" show each Meal and Dose as a curve over its whole span, into the hours ahead, with the totals on board. A Basal is hatched, a Planned Meal hollow.
- Home: Today's Meals, the sums of what you have eaten since midnight (carbohydrates as a span, energy, fat, sugars, fibre, protein, salt), below the log buttons.
- Settings: Insulin sensitivity and Carb ratio can differ by time of day ("Split the day"); the blocks' times are your phone's local time, stored when you save them. One block covering the day works as before.
- Forecast research, not yet visible in the web app: a back-test bench that scores each Forecast method on your stored data, and a second method in the style of OpenAPS and Loop, which needs Insulin sensitivity and Carb ratio.
- Calibration, not yet visible in the web app: the store for proposals to change your figures, which change nothing until you accept them, and the history of every change to Insulin sensitivity, Carb ratio and Action profiles, including your own edits.

## 0.2.0

**Before updating, make a Home Assistant backup that includes this add-on** (the update dialog offers one). This version adds one change to the database (Nutrients on Meals): it only adds optional columns, and everything already stored is kept as it is.

- Meal estimates: the Estimate button on Log a Meal asks an AI service for a proposal from the Meal's description, which you check and change before saving. Off until you set a key: new optional options for Claude (`anthropic_api_key`), Gemini (`gemini_api_key`, free tier available) and any OpenAI-compatible service such as Groq (`openai_compatible_api_key` with its address and model). Only the description is sent. See the documentation's "Meal estimates" section.
- Nutrients on Meals: energy, fat, saturated fat, sugars, fibre, protein and salt, each optional. Meals logged before have none.
- Service log: Settings > Service log shows what the service has written to its log lately (Dexcom sessions, each sync of Glucose readings, errors).
- Home graph: 3 hours of room ahead of now, a line marking now, and Meals, Doses and Activities marked on the graph itself.
- Fixed: a failed Dexcom poll could write the Dexcom session to the log.
- Your data is kept: the one database change adds optional columns to Meals and rewrites no row. The Forecast does not need training again.

## 0.1.0

- First version of the add-on: the service, the web app and PostgreSQL 16 in one container.
- The web app and the API on port 8787.
- The Forecast is trained from the web app's Settings screen.
- Home Assistant backups carry a dump of the database, written just before each backup.
- An optional database port for debugging.
