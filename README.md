# Beach Forecast by ZIP Code

A single-file, client-side weather web app that creates a **Beach Forecast** from a 5-digit U.S. ZIP Code.

The app converts a ZIP Code into latitude/longitude, pulls the official National Weather Service forecast for that point, checks active alerts and the latest nearby observation, then builds a beach usability score. It also adds nearby coastal information, including water temperature and today’s high/low tide schedule when a relevant NOAA CO-OPS station is close enough.

## Features

- ZIP Code search for U.S. locations
- Official NWS point forecast
- Latest nearby observation when available
- Active NWS alerts
- Daily **Beach Score Forecast**
- Calendar-style score view
- Forecast table with daily high, precipitation risk, score, label, and beach note
- Daytime hourly forecast only, no overnight clutter
- Closest coastal water temperature from NOAA CO-OPS when available
- Today’s high/low tide schedule from NOAA CO-OPS when available
- Mobile-friendly responsive layout
- No API keys
- No backend
- No login
- Safe for GitHub Pages hosting

## Data Sources

This app uses public, no-key endpoints:

### ZIP Code Lookup

- `https://api.zippopotam.us/us/{zip}`

Used to convert a 5-digit ZIP Code into latitude and longitude.

### National Weather Service API

- `https://api.weather.gov/points/{lat},{lon}`
- NWS daily forecast endpoint returned by `/points`
- NWS hourly forecast endpoint returned by `/points`
- `https://api.weather.gov/alerts/active?point={lat},{lon}`
- NWS observation stations endpoint returned by `/points`
- Latest observation from the nearest available NWS station

Used for the main weather forecast, current observation, alerts, and hourly beach score calculations.

### NOAA CO-OPS API

- `https://api.tidesandcurrents.noaa.gov/mdapi/prod/webapi/stations.json`
- `https://api.tidesandcurrents.noaa.gov/api/prod/datagetter`

Used for nearby coastal station metadata, latest water temperature, and today’s tide predictions.

## How the Beach Score Works

The Beach Score runs from **0.0 to 10.0**.

Temperature is the main driver. The app also considers sky conditions, precipitation chance, and whether rain or storms are likely during the main beach-use window.

### Daily Score Logic

Daily scores use the forecast high temperature.

If today’s observed temperature is higher than today’s forecast high, the app updates today’s high to the observed temperature and recalculates the score.

### Beach Windows

- Weekdays emphasize evening use, roughly **5 PM–9 PM**
- Weekends use a broader beach window, roughly **11 AM–9 PM**
- The hourly table displays daytime, afternoon, and evening hours only

### Labels

| Label | Score Range | Color |
| --- | ---: | --- |
| Excellent | 10.0–8.5 | Red |
| Good | 8.4–7.0 | Orange |
| Fine | 6.9–5.5 | Green |
| Poor | 5.4–3.5 | Blue |
| Awful | 3.4–0.0 | Purple |

## Coastal Conditions Behavior

The app attempts to find the closest useful NOAA CO-OPS station for:

1. Water temperature
2. Tide predictions

A distance cutoff is used so inland ZIP Codes do not show misleading coastal data. If no suitable station is close enough, the coastal card displays a friendly unavailable message instead of forcing bad data into the forecast.

Water temperature is best understood as the closest available **coastal station water temperature**, not a gridded satellite sea surface temperature product.

## Running Locally

Because this is a single-file app, you can open `index.html` directly in a browser.

For the most reliable testing, run a local server from the project folder:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## Hosting on GitHub Pages

1. Create a new GitHub repository.
2. Add the app file as `index.html`.
3. Add this `README.md` file.
4. Commit and push the files.
5. Go to **Settings → Pages**.
6. Set the source to your main branch.
7. Save and wait for GitHub Pages to publish the site.

## Project Structure

```text
/
├── index.html
└── README.md
```

The entire app is contained in `index.html`, including HTML, CSS, and JavaScript.

## Browser Support

The app is intended for modern browsers, including:

- Chrome
- Edge
- Firefox
- Safari

## Known Limitations

- Forecast quality depends on the public NWS API and the selected point forecast grid.
- ZIP Codes are converted to one representative latitude/longitude point, not every address within the ZIP Code.
- Coastal observations and tides depend on nearby NOAA CO-OPS station coverage.
- Some beaches may have nearby water temperature data but no ideal tide station, or vice versa.
- Water temperature may be unavailable if the closest station is temporarily offline or does not report that sensor.
- This app does not currently include surf height, rip current risk, UV index, beach closures, water quality, or lifeguard information.

## Recommended Future Improvements

- Add surf height where reliable public data is available
- Add rip current risk from NWS beach forecasts where available
- Add UV index
- Add sunrise and sunset times
- Add a better coastal-only ZIP Code filter
- Add saved favorite ZIP Codes using local storage
- Add a printable or shareable daily beach forecast card
- Add clearer messaging for inland ZIP Codes

## License

Use, modify, and host this app as needed for your own project. Add a formal license if you plan to publish it publicly or allow reuse by others.
