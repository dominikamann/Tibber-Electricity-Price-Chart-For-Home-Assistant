# Tibber-Electricity-Price-Chart-For-Home-Assistant
A way how you can view the current and future electricity price of your Tibber.com contract in Home Assistant.

![grafik](https://github.com/dominikamann/Tibber-Electricity-Price-Chart-For-Home-Assistant/assets/29973737/d9213327-889f-41d5-8e90-c5fbcce6707d)

- **15-minute prices** (`QUARTER_HOURLY`), matching the quarter-hourly day-ahead market. Every price is
  plotted at its own `startsAt` time, so the chart also stays correct on daylight-saving days.
- **Colours relative to the day**: cheap (at least 10 % below the day's average), normal, expensive
  (at least 10 % above). Fixed cent thresholds no longer fit since taxes and fees make up most of the price.
- **Cheapest window**: the cheapest coherent block from now on (default 2 hours) is highlighted and
  available as `sensor.tibber_cheapest_window` for automations, e.g. to start the dishwasher.
- **Price limit line**: set `input_number.tibber_price_limit` (0 = off).
- **Rolling view**: 6 hours back, 30 hours ahead.
- **Light on the API and the database**: the price list is fetched every 15 minutes and kept out of
  the recorder; the current price is calculated locally every minute.

## How to?

 1. Enable packages in your `configuration.yaml` (skip if you already have this):
    ```yaml
    homeassistant:
      packages: !include_dir_named packages
    ```
 2. Copy `packages/tibber_prices.yaml` of this repository into the `packages` folder of your Home Assistant
    config (create it next to `configuration.yaml` if it does not exist)
 3. Add your Tibber Developer Token (https://developer.tibber.com/) to `secrets.yaml`, with `Bearer ` in front of it:
    ```yaml
    tibber_token: "Bearer YOUR_TIBBER_DEVELOPER_TOKEN"
    ```
 4. Install https://github.com/RomRider/apexcharts-card over HACS
 5. Restart your Home Assistant instance
 6. Go to any dashboard you wish, add a new card and paste in the code of the `card.yaml` file

If your `configuration.yaml` already has a `recorder:` section with `exclude:`, Home Assistant reports a
conflict. Then move the `recorder:` part of the package into your existing `exclude:`.

![grafik](https://github.com/dominikamann/Tibber-Electricity-Price-Chart-For-Home-Assistant/assets/29973737/6d7381ab-cca9-4efa-9425-faa2ad59eed0)

### Entities

| Entity | Description |
|---|---|
| `sensor.tibber_price` | Current price in EUR/kWh |
| `sensor.tibber_cheapest_window` | Start of the cheapest window; attributes `end` and `average` (ct/kWh) |
| `input_number.tibber_cheapest_window_hours` | Length of the cheapest window (2 h after every restart; change `initial` in the package to keep another value) |
| `input_number.tibber_price_limit` | Limit line in the chart in ct/kWh (0 = off) |
| `sensor.tibber_price_data` | All prices of today and tomorrow (attributes `today`, `tomorrow`), used by the chart |

**Hourly prices instead?** Change `resolution: QUARTER_HOURLY` to `resolution: HOURLY` in the package.
Everything else works with both.

**Multiple homes?** The package reads the first home (`homes[0]`). Change the index in
`json_attributes_path` and `value_template` if needed.

### Upgrading from the old `sensors.yaml`

Remove the old `Tibber Prices` REST sensor (and the `sensor: !include sensors.yaml` line if nothing else
uses it), then follow the steps above. The old entity `sensor.tibber_prices` is replaced by
`sensor.tibber_price` and `sensor.tibber_price_data`; update automations that used it.

## Variant: price + PV forecast

`card-pv.yaml` shows the estimated PV production next to the price (right axis, kW). An expensive hour is
no problem when plenty of sun is coming.

- PV forecast (median) as an area, the optimistic forecast (P90) as a pale area behind it and the
  pessimistic one (P10) as a dashed line
- Actual PV production of the last hours on top, so you see how well the forecast fits today
- Optional: a base load line for your house (commented out at the end of the card)

It uses [Machine-Learning-Based PV Forecast](https://github.com/dominikamann/ha-machine-learning-based-pv-forecast):

 1. Set up the price chart as described above
 2. Install and configure [Machine-Learning-Based PV Forecast](https://github.com/dominikamann/ha-machine-learning-based-pv-forecast)
 3. Paste the code of `card-pv.yaml` into a new card
 4. Replace `sensor.YOUR_PV_POWER` with your PV power sensor (in W; if it is in kW, remove the `transform` line)
 5. If your forecast entity is not called `sensor.pv_forecast_total_forecast_curve` (it depends on your Home
    Assistant language), adjust it. Use the curve of a single plant instead of the total if you like.

---

Many thanks to the community of https://community.home-assistant.io/t/tibber-schedul-prices-upcoming-24-hours-prices/391565 who did all the initial work to achieve this. Especially to BerriJ (https://community.home-assistant.io/u/berrij/summary) who published the original code for it.

---


<p align="center">Proudly provided by <a href="https://amannlabs.eu">amannlabs.eu</a></p>
