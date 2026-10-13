# Sugar Angel add-on repository for Home Assistant

This repository lets Home Assistant offer the Sugar Angel add-on: support for one person managing diabetes with insulin pens, with Glucose readings from Dexcom, a Logbook and a glucose Forecast.

**Sugar Angel is a proof of concept made for one person's own use. It is not a medical device.**

## Install

You need Home Assistant OS (or another installation with the Supervisor) on a 64-bit ARM machine, for example a Raspberry Pi 5.

1. In Home Assistant open **Settings > Add-ons > Add-on store** (newer versions say "Apps").
2. Open the menu at the top right, choose **Repositories** and add:

   ```
   https://github.com/agadek/sugar-angel-addon
   ```

3. Find **Sugar Angel** in the store and press **Install**.
4. Follow the add-on's **Documentation** tab: set the API token, include the add-on in your backups, start it.

## What is here

| Path | What it is |
|---|---|
| `repository.yaml` | The description of this repository. |
| `sugar_angel/config.yaml` | The add-on's configuration: version, image, ports, options. |
| `sugar_angel/DOCS.md` | The add-on's documentation, shown in Home Assistant. |
| `sugar_angel/translations/` | The names and descriptions of the options and ports. |
| `sugar_angel/CHANGELOG.md` | What changed in each version. |

There is no source code here. The add-on's image, `ghcr.io/agadek/sugar-angel-addon`, is built elsewhere and Home Assistant downloads it. The `version` in `config.yaml` is the tag of the image it downloads.
