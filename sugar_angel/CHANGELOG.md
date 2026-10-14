# Changelog

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
