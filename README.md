# Tibber-Electricity-Price-Chart-For-Home-Assistant
A way how you can view the current and future electricity price of your Tibber.com contract in Home Assistant.

![grafik](https://github.com/dominikamann/Tibber-Electricity-Price-Chart-For-Home-Assistant/assets/29973737/d9213327-889f-41d5-8e90-c5fbcce6707d)

**How to?**

 1. Add `sensor: !include sensors.yaml` to your Home Assistant `configuration.yaml`
 2. Create a new file `sensors.yaml` in your HA root folder (same level as the configuration.yaml file)
 3. Copy in the content of the  `sensors.yaml` file of this repository into your newly created file
 4. Replace `ADD_YOUR_TOKEN_HERE` with your Tibber Developer Token (https://developer.tibber.com/)
 5. Install https://github.com/RomRider/apexcharts-card over HACS
 6. Restart your Home Assistant instance
 7. Go to any dashboard you wish, add a new card and paste in the code of the `card.yaml`file

![grafik](https://github.com/dominikamann/Tibber-Electricity-Price-Chart-For-Home-Assistant/assets/29973737/6d7381ab-cca9-4efa-9425-faa2ad59eed0)


Many thanks to the community of https://community.home-assistant.io/t/tibber-schedul-prices-upcoming-24-hours-prices/391565 who did all the work. I only documented the result in git to make it easily accessible for everyone.

