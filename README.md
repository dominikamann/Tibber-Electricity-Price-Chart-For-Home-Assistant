# Tibber-Electricity-Price-Chart-For-Home-Assistant
A way how you can view the current and future electricity price of your Tibber.com contract in Home Assistant.

![grafik](https://github.com/dominikamann/Tibber-Electricity-Price-Chart-For-Home-Assistant/assets/29973737/d9213327-889f-41d5-8e90-c5fbcce6707d)

Prices are fetched in **15-minute resolution** (`QUARTER_HOURLY`), matching the quarter-hourly
day-ahead market. Every price is plotted at its own `startsAt` time, so the chart also stays
correct on daylight-saving days (23 or 25 hours).

**How to?**

 1. Add `sensor: !include sensors.yaml` to your Home Assistant `configuration.yaml`
 2. Create a new file `sensors.yaml` in your HA root folder (same level as the configuration.yaml file)
 3. Copy in the content of the  `sensors.yaml` file of this repository into your newly created file
 4. Replace `ADD_YOUR_TOKEN_HERE` with your Tibber Developer Token (https://developer.tibber.com/), keep the `Bearer ` in front of it
 5. Install https://github.com/RomRider/apexcharts-card over HACS
 6. Restart your Home Assistant instance
 7. Go to any dashboard you wish, add a new card and paste in the code of the `card.yaml`file

![grafik](https://github.com/dominikamann/Tibber-Electricity-Price-Chart-For-Home-Assistant/assets/29973737/6d7381ab-cca9-4efa-9425-faa2ad59eed0)

**Hourly prices instead?** Change `resolution: QUARTER_HOURLY` to `resolution: HOURLY` in `sensors.yaml`.
The cards work with both.

**Multiple homes?** The sensor reads the first home (`homes[0]`). Change the index in
`json_attributes_path` and `value_template` if needed.

## Variant: price + PV forecast

`card-pv.yaml` shows the estimated PV production as a second curve (right axis, kW) next to the
price. An expensive hour is no problem when plenty of sun is coming.

It uses the forecast curve of
[Machine-Learning-Based PV Forecast](https://github.com/dominikamann/ha-machine-learning-based-pv-forecast):

 1. Set up the price chart as described above
 2. Install and configure [Machine-Learning-Based PV Forecast](https://github.com/dominikamann/ha-machine-learning-based-pv-forecast)
 3. Paste the code of `card-pv.yaml` into a new card
 4. If your entity is not called `sensor.pv_forecast_total_forecast_curve` (it depends on your Home
    Assistant language), adjust it. Use the curve of a single plant instead of the total if you like.

The curve shows the median forecast (P50): 15-minute values for the next 24 hours, hourly values after that.

---

Many thanks to the community of https://community.home-assistant.io/t/tibber-schedul-prices-upcoming-24-hours-prices/391565 who did all the initial work to achieve this. Especially to BerriJ (https://community.home-assistant.io/u/berrij/summary) who published the original code for it.

<p align="center">Proudly provided by <a href="https://amannlabs.eu">amannlabs.eu</a></p>
