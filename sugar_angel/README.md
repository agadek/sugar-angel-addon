# Sugar Angel

Support for one person managing diabetes with insulin pens.

- Fetches Glucose readings from a Dexcom continuous glucose monitor (through Dexcom Share).
- Keeps a Logbook of Meals, Doses and Activities, entered in a web app.
- Shows a Forecast of glucose 30, 60 and 90 minutes ahead, trained on your own data.

**A proof of concept, not a medical device.** Do not base treatment decisions on it.

The web app is at `http://<home-assistant-address>:8787`. It speaks plain HTTP and is protected by one token, so keep it on your home network. The Documentation tab has the details.

64-bit ARM only (for example a Raspberry Pi 5).
