# Sugar Angel

Sugar Angel supports one person who manages diabetes with insulin pens. It fetches Glucose readings from a Dexcom continuous glucose monitor, keeps a Logbook of Meals, Doses and Activities that you enter in a web app, and shows a Forecast of glucose 30, 60 and 90 minutes ahead.

**This is a proof of concept. It is not a medical device.** Nothing it shows has been tested or approved for treatment decisions. The Forecast is an experiment trained on your own data and can be wrong, sometimes by a lot. Decide about insulin and food the way you and your care team have agreed, with your glucose monitor and meter.

This add-on holds everything in one container: the service, the web app and its own PostgreSQL database. It runs on 64-bit ARM machines only (for example a Raspberry Pi 5).

## First start

1. Open the **Configuration** tab and set the **API token**. Nothing else is required. See [Options](#options) below.
2. Include the add-on in Home Assistant's backups before you rely on it. See [Backups](#backups).
3. Open the **Info** tab and press **Start**. The first start creates the database, which takes up to a minute or two on a Raspberry Pi. The **Log** tab shows the progress.
4. On the Info tab, switch on **Watchdog** if you want Home Assistant to restart the add-on when it stops answering. "Start on boot" is on by default.
5. Open the web app (next section), type the API token, and set up your Insulins and Settings there.

The add-on does not start without an API token: Home Assistant refuses to save or start it and says the option is missing.

## Reaching the web app

The web app is at

```
http://<home-assistant-address>:8787
```

for example `http://homeassistant.local:8787`. The **Open Web UI** button on the Info tab opens the same address. It is not shown inside Home Assistant's own pages or sidebar.

The first time you open it on a phone or computer it asks for the API token. It remembers the token on that device until you choose "Forget this device" on its Settings screen.

**Keep it on your home network.** The web app speaks plain HTTP, without encryption, and one token is all that protects it. Anyone who can listen on the network between your device and Home Assistant can read the token and your data. Do not forward port 8787 from your router, and do not publish it to the internet as it is. To use it away from home, go through something that adds encryption and access control, such as a VPN into your home network.

You can change the port, or close it, under **Network** on the Configuration tab.

## Options

Optional settings that are empty may be hidden: switch on "Show unused optional configuration options" on the Configuration tab to see them. Changes take effect when the add-on is restarted.

| Option | Meaning |
|---|---|
| **API token** (`api_token`) | Required, at least 16 characters. The one secret that protects the web app and the API. Use a long random value, for example from a password manager. If you change it, every device has to type the new one. |
| **Dexcom username** (`dexcom_username`) | The Dexcom account whose Glucose readings are fetched through Dexcom Share. Leave it empty and the add-on runs without fetching Glucose readings. |
| **Dexcom password** (`dexcom_password`) | The password of that account. |
| **Dexcom region** (`dexcom_region`) | `US` for an account made in the United States, `OUS` (the default) for anywhere else. |
| **Poll interval** (`poll_interval_seconds`) | How often to ask Dexcom for new Glucose readings, in seconds. The default is 300; Dexcom has a new one every 5 minutes. |
| **Log level** (`log_level`) | How much the service writes to the log. The default is `info,sqlx=warn`. Use `debug,sqlx=warn` when looking for a problem. |
| **Database password** (`db_password`) | Leave it empty in normal use. See [The database port](#the-database-port-for-debugging). |
| **Claude API key** (`anthropic_api_key`) | Optional. Lets Claude propose Meal estimates. See [Meal estimates](#meal-estimates). |
| **Claude model** (`anthropic_model`) | Optional. Leave it empty for the service's default. |
| **Gemini API key** (`gemini_api_key`) | Optional. Lets Gemini propose Meal estimates. |
| **Gemini model** (`gemini_model`) | Optional. Leave it empty for the service's default. |
| **Gemini fallback model** (`gemini_fallback_model`) | Optional. Asked when the Gemini model is busy. Leave it empty for the service's default, or write `off` to ask the same model once more instead. |
| **OpenAI-compatible API key** (`openai_compatible_api_key`) | Optional. Lets a service such as Groq, OpenRouter, Mistral or OpenAI propose Meal estimates. With it, the address and the model are required. |
| **OpenAI-compatible address** (`openai_compatible_base_url`) | That service's address, for Groq `https://api.groq.com/openai/v1`. |
| **OpenAI-compatible model** (`openai_compatible_model`) | The model to ask there, for Groq for example `openai/gpt-oss-20b`. |
| **OpenAI-compatible name** (`openai_compatible_name`) | Optional. What its proposals are labelled with, for example `groq`: lowercase letters, digits and dashes. Leave it empty for `openai-compatible`. |

Home Assistant keeps these values, including the API token, both passwords and the API keys, in its own storage and in every backup that includes the add-on.

## The Forecast

There is no Forecast until it has been trained, and training needs about a day of Glucose readings. Until then the Home screen says there is no Forecast.

To train it, open the web app's **Settings** screen, go to the **Forecast** section and press **Train the forecast**. The web app uses the result straight away; no restart is needed. Train it again now and then as more of your data comes in. After an update the add-on may say that the Forecast must be trained again; do it the same way.

The Forecast is hidden when the latest Glucose reading is more than 15 minutes old or when readings are missing.

## Meal estimates

When you log a Meal, the web app's **Estimate** button can ask an AI service for a proposal from the Meal's description, for example "two slices of wholemeal toast with butter and a banana": a span of carbohydrates, how fast they are absorbed, the nutrients and a short explanation. It is only a proposal. Nothing counts until you have read it, changed what you disagree with and saved the Meal yourself. The service can be wrong, and it cannot see the food.

Only the description you type is sent, together with the fixed instructions for the service. No Glucose readings, Doses, other Logbook entries or anything about you are sent.

Meal estimates are off until you set at least one key. Without a key the Estimate button is shown greyed out, with a line saying that no provider's key is set. With more than one key, every service set up is asked at once and the proposals are shown side by side.

| Provider | Where to get a key | Cost |
|---|---|---|
| **Gemini** (Google) | Google AI Studio (aistudio.google.com), "Get API key". | A free tier with a daily limit, plenty for one person's meals. **On the free tier Google may use the meal descriptions to improve its products**, and people at Google may read them. Use a paid key if you do not want that. |
| **Claude** (Anthropic) | The Anthropic Console (console.anthropic.com), after adding credit. | Paid per use. Each estimate is a small request; a month of meals costs little, but not nothing. |
| **OpenAI-compatible** | For Groq: the Groq console (console.groq.com), "API Keys". Other services such as OpenRouter, Mistral or OpenAI have their own. | Groq has a free tier with limits; others vary from free tiers to pay per use. Check the service's own terms on what it may do with what is sent. |

To set one up:

1. Get a key from the provider (table above).
2. On the add-on's **Configuration** tab, switch on "Show unused optional configuration options" and paste the key into its option. For an OpenAI-compatible service also set its address and model; for Groq the address is `https://api.groq.com/openai/v1` and a model is `openai/gpt-oss-20b`. The model must support structured answers (`response_format` `json_schema` with `strict`).
3. Save and restart the add-on. The Log tab then says which providers will be asked, never the key.

Leave the model options empty unless you have a reason: the service then uses its own default, and a newer default arrives with an update of the add-on.

When a service is busy, it is asked once more after a short pause. Gemini instead asks its fallback model straight away, a lighter model on the same key. If it is still busy, the proposal says so; try again in a moment. A key that is wrong, out of credit or over its free limit gives a clear message in place of that provider's proposal; the other providers still answer.

The keys are options like the others: Home Assistant keeps them in its storage and in every backup that includes the add-on, so they are protected the same way as the API token. To stop using a provider, clear its key and restart.

## Backups

The add-on does not back itself up. **Include it in Home Assistant's backups** (Settings > System > Backups: make sure the automatic backup covers add-ons, or select Sugar Angel when you make a backup by hand). Keep copies somewhere other than the Home Assistant machine: its storage is the likeliest thing to fail.

What happens when Home Assistant makes a backup:

- Just before, the add-on writes a fresh dump of its whole database (Glucose readings, the Logbook, Insulins and Settings). If writing the dump fails, the backup of this add-on fails too, and the Log tab says why.
- The backup then holds that dump, the trained Forecast and the add-on's options. **The options include the API token, the Dexcom password and any API keys for Meal estimates**, so treat a backup as a secret and keep Home Assistant's backup encryption on.
- The raw database files are left out on purpose. They cannot be copied safely while the database is running, and the dump replaces them.

What a restore does: Home Assistant stops the add-on, puts back the options and data from the backup, and starts it again. The add-on finds no database files, creates a new database and loads the dump. Everything logged or fetched after that backup is gone. Dexcom only gives back the last 24 hours of Glucose readings. The first start after a restore takes longer than usual; the Log tab shows "loading the dump".

Two things to know:

- **A backup made while the add-on is stopped carries the dump from the last backup made while it was running**, not the current data. If no backup was ever made while it was running, there is no dump in it at all, and a restore gives an empty database. Make backups while the add-on is running.
- There is no dump at all until Home Assistant has made a backup that includes the add-on. Check once, after the first backup, that the Log tab shows a line like `wrote /data/backup/sugar_angel.dump`.

Uninstalling the add-on deletes its data. A backup is the only way back.

## The database port, for debugging

In normal use the database cannot be reached from outside the add-on. To look at the data with a database tool:

1. On the Configuration tab set the **Database password** to a long random value.
2. Under **Network**, give port `5432` a host port, for example 5432.
3. Save and restart the add-on. Both settings are needed, and both take effect only on a restart.

Then connect from a computer on your home network to host `<home-assistant-address>`, the port you chose, database `sugar_angel`, with one of two logins, both using the Database password:

| Login | What it may do |
|---|---|
| `sugar_readonly` | Read only. Use this one unless you mean to change something. |
| `sugar_admin` | Read and change everything. A mistake here can break the add-on; make a backup first. |

For example: `psql "host=homeassistant.local port=5432 dbname=sugar_angel user=sugar_readonly"`.

Connections to the database are **not encrypted**: the password and the data cross the network in the clear. Use this on your home network only and never forward the port from your router.

**Switch it off again when you are done:** clear the Database password, clear the host port of 5432, save and restart. With the Database password empty nobody can log in from outside, even if the port is still open.

## Updating

When a new version is published, Home Assistant shows an update for the add-on. Make a backup first (the update dialog offers one), then press **Update**. Your data and options stay; the service brings its database up to date by itself when it starts. Read the changelog first: it says when the Forecast has to be trained again.

There is no way to go back to an older version other than restoring a backup made with it.

## Logs and problems

Everything the add-on writes is on its **Log** tab: the start-up steps, the service, the database and the backup hook.

| What you see | What to do |
|---|---|
| The add-on will not start; Home Assistant says the option `api_token` is missing or too short | Set the API token on the Configuration tab. |
| The log says `the option api_token is empty` | The same. |
| The web app keeps asking for the token | The token typed does not match the API token option. |
| No Glucose readings arrive | Check the Dexcom username, password and region, then the log. With no Dexcom account set, nothing is fetched. |
| The Home screen says there is no Forecast | Train it from the Settings screen. It needs about a day of Glucose readings. |
| The OpenAI-compatible service gives no proposals; the Log tab has an error line about `OPENAI_COMPATIBLE_…` | With its key set, the address (starting with `http://` or `https://`) and the model are required, and the name, if given, must be lowercase letters, digits and dashes. The add-on keeps running without that service until it is fixed or the key cleared. |
| The Estimate button is greyed out | No provider's key is set. See [Meal estimates](#meal-estimates). |
| An estimate says the provider is busy, out of quota or the key was refused | Busy: try again in a moment. Quota: wait for the free tier's daily limit to reset, or add credit. Refused key: check the key on the Configuration tab. |
| The add-on stops by itself | When the service or the database fails five times within two minutes the add-on stops instead of running half-broken. The Log tab has the reason. |

## About

The add-on is built from a private repository; this repository holds only its configuration and documentation. It is made for one person's own use and comes with no support and no warranty.
