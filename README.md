# Pixel Art Clock — OpenWeatherMap edition

A port of the [Pixel Art Clock](https://github.com/tronbyt/apps/tree/main/apps/pixelartclock)
Tidbyt/Tronbyt app from AccuWeather to **OpenWeatherMap**, so it works again on a
plain free API key.

Clock, the coming 24 hours' high and low temperatures, and one of nine pixel-art
scenes matching the local forecast — capybaras included.

## Credits

- **App**: [JavierM42](https://github.com/JavierM42), based on the AccuWeather
  Forecast app by sudeepban
- **Pixel art**: [@abipixel](https://www.instagram.com/abipixel/)
- **This port**: swaps the data layer to OpenWeatherMap's free
  [5 day / 3 hour forecast API](https://openweathermap.org/forecast5); rendering
  and artwork are unchanged

## What changed vs. the original

| | AccuWeather original | This port |
|---|---|---|
| Data source | Daily 1-day forecast (needs an AccuWeather key — free tier discontinued) | OWM 5 day / 3 hour forecast (standard free key) |
| Location | Manual "location key" copied from accuweather.com URLs | Native location picker (`schema.Location`) |
| Timezone | Separate text field | From the picked location, falling back to the device timezone (`$tz`) |
| Temps shown | Today's forecast max/min | Coming 24 hours' high/low (the free endpoint only carries future 3-hour slices, so a calendar-day range isn't available) |
| Condition art | AccuWeather icon numbers 1–44 | OWM condition ids ([full table](https://openweathermap.org/weather-conditions)), incl. freezing rain → snow art per OWM's own taxonomy |
| Bad/inactive API key | Hard render error | Falls back to the sample display (new OWM keys take up to ~2 h to activate) |

The API key schema field id changed (`apiKey` → `owmApiKey`) on purpose:
installs configured in the AccuWeather era degrade gracefully to the sample
display instead of sending a dead key to the wrong API and hard-failing.

## Setup

1. Get a free API key: https://home.openweathermap.org/api_keys (no credit
   card; the standard free tier is sufficient — this app makes at most one
   call per hour per device)
2. Add the app, paste the key, pick your location. Until the key activates
   (up to ~2 hours for brand-new keys) the app shows sample data with a red
   SAMPLE marker.

## Repo layout

The `apps/pixelartclock/` layout mirrors the [tronbyt/apps](https://github.com/tronbyt/apps)
repo, so this repo can be used directly as a
[tronbyt-server](https://github.com/tronbyt/server) **user app repo** (Settings →
App Repo URL), and the directory can be copied as-is into a fork of
`tronbyt/apps` for an upstream pull request.

## Development

Render locally with the [tronbyt pixlet fork](https://github.com/tronbyt/pixlet):

```
pixlet render apps/pixelartclock/pixel_art_clock.star            # sample mode
pixlet render apps/pixelartclock/pixel_art_clock.star --2x       # 128×64 devices
pixlet render apps/pixelartclock/pixel_art_clock.star owmApiKey=<key>
```
