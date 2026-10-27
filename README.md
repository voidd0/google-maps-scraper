# Google Maps Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape Google Maps business listings by search and place: name, category, address, coordinates, rating, review count, phone, website, opening hours, place ID and Maps link. Search by keyword and city, or paste Maps search links.

This repository has ready scripts for the [Google Maps Scraper: Places, Phones, Hours | $1.50 per 1,000](https://apify.com/v0iddo/google-maps-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/google-maps-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "searchQueries": [
    "coffee shop"
  ],
  "location": "Berlin",
  "language": "en",
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `searchQueries` | array | What to look for on Google Maps, one per line, for example: coffee shop, plumber, dentist. |
| `location` | string | Optional city, area or address added to every query as 'query in location', for example Berlin or Austin, TX. |
| `startUrls` | array | Optional Google Maps search links, for example https://www.google.com/maps/search/coffee+in+berlin. |
| `language` | string | Language of the Google Maps page: category names, days and hours. |
| `maxPlacesPerSearch` | integer | Stop each search after this many places (0 = as many as Google Maps lists, usually 100 to 200). |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "searchQuery": "coffee shop in Berlin",
  "position": 1,
  "name": "Refinery High End Coffee",
  "category": "Coffee shop",
  "categories": [
    "Coffee shop"
  ],
  "address": "Albrechtstraße 11B, 10117 Berlin, Germany",
  "street": "Albrechtstraße 11B",
  "city": "Berlin",
  "postalCode": "10117",
  "countryCode": "DE",
  "neighborhood": "Mitte",
  "latitude": 52.5213117,
  "longitude": 13.3849173,
  "rating": 4.6,
  "reviewsCount": 1221,
  "website": "http://refinerycoffee.de/",
  "openingHours": [
    {
      "day": "Tuesday",
      "hours": "8 AM-6 PM"
    },
    {
      "day": "Wednesday",
      "hours": "8 AM-6 PM"
    },
    {
      "day": "Thursday",
      "hours": "8 AM-6 PM"
    },
    {
      "day": "Friday",
      "hours": "8 AM-6 PM"
    },
    {
      "day": "Saturday",
      "hours": "9 AM-6 PM"
    }
  ],
  "placeId": "ChIJ9UK3L8JRqEcRidYSi6cycYo",
  "featureId": "0x47a851c22fb742f5:0x8a7132a78b12d689",
  "cid": "9975810344776554121",
  "url": "https://www.google.com/maps/place/?q=place_id:ChIJ9UK3L8JRqEcRidYSi6cycYo"
}
```

Fields: `searchQuery`, `position`, `name`, `category`, `categories`, `address`, `street`, `city`, `postalCode`, `countryCode`, `neighborhood`, `latitude`, `longitude`, `rating`, `reviewsCount`, `website`, `openingHours`, `placeId`, `featureId`, `cid`, `url`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/google-maps-scraper').call({"searchQueries": ["coffee shop"], "location": "Berlin", "language": "en", "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~google-maps-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"searchQueries": ["coffee shop"], "location": "Berlin", "language": "en", "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$1.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
