# Changelog

## 0.2.0

- Service log: Settings > Service log shows what the service has written to its log lately (Dexcom sessions, each sync of Glucose readings, errors).
- Home graph: 3 hours of room ahead of now, a line marking now, and Meals, Doses and Activities marked on the graph itself.
- Fixed: a failed Dexcom poll could write the Dexcom session to the log.
- Your data is kept: this version changes nothing in the database.

## 0.1.0

- First version of the add-on: the service, the web app and PostgreSQL 16 in one container.
- The web app and the API on port 8787.
- The Forecast is trained from the web app's Settings screen.
- Home Assistant backups carry a dump of the database, written just before each backup.
- An optional database port for debugging.
