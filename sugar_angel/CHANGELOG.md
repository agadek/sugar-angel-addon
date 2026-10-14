# Changelog

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
