# Fishing Spots

An interactive map for cataloguing fishing spots — rate them, sketch the actual
stretch of river you fish, and keep the access notes and regulations with the pin.

**▶ Try it: https://robertarthurjackson.github.io/Fishing/**

It opens in any browser, phone or desktop. There is nothing to install and no
account to make.

Seeded with researched spots across the BC Kootenays (Columbia Valley, Elk Valley,
Kootenay Lake), the Calgary area, and the Methow Valley in Washington — with
season dates, access directions and the regulations that actually apply to each
water. Add your own from there.

## Features

- **Three geometry types** — drop a pin, trace a river stretch as a line, or
  outline a zone as a polygon. A spot is whichever shape fits it.
- **Four-axis ratings** — fish quality, action, ease of access and scenery, each
  0–5. Spots are coloured green through red by their average and sortable by any
  single axis, so "where's the easy one?" and "where's the best one?" are
  different questions with different answers.
- **Regulations in the card** — season windows, gear restrictions and closures
  live with the spot. BC classified waters render purple and dashed so you
  can't miss that they need a supplemental licence.
- **Map layers** — street, topographic, or satellite.
- **Import / export** — everything round-trips as JSON.
- **Works on a phone**, and dark mode follows your system setting.

## Running it

Pick whichever suits you:

**Just use it** — open the link above. That is the whole thing.

**Keep your own copy** — download `index.html` and double-click it. It works
straight off your hard drive, offline, no server. Map tiles need a connection,
but your spots do not.

**Host it yourself** — clone the repo and serve the folder:

```sh
git clone https://github.com/robertarthurjackson/Fishing.git
cd Fishing
npx serve .        # or: python3 -m http.server
```

Any static host works — GitHub Pages, a home server, an S3 bucket.

## Where your data lives

In your browser's `localStorage`, on your device. Not on a server, not shared,
not visible to anyone else — including whoever gave you the link.

Two consequences worth knowing:

- **It does not follow you between devices.** Spots you add on your phone stay on
  your phone. Use Export → Import to move them.
- **It is per-site-address.** The same app opened at a different URL keeps a
  separate set of spots.

The seeded spots are baked into `index.html`. They merge into your data the first
time you load the app, and deleting one keeps it deleted. Your ratings are yours —
updates to the seeded content never overwrite them.

## Making it your own

The whole app is one file: `index.html`, containing the HTML, CSS and vanilla JS.
No framework, no build step, no dependencies to install — [Leaflet](https://leafletjs.com/)
loads from a CDN and that is the only third-party code.

The spots are a plain array called `SEED_SPOTS`. Replace it with your own water
and you have your own app. Each entry looks like:

```js
{
  id: 'seed-example',
  name: 'Example Creek',
  detail: 'The braided section below the bridge',
  waterbody: 'Example Creek',
  region: 'other',
  geometry: { type: 'marker', coords: [[50.1234, -115.6789]] },
  species: ['cutthroat trout'],
  season: 'July–Oct once runoff clears',
  access: 'Pull-off at the bridge, walk downstream.',
  notes: 'Small fish, nobody around.',
  ratings: { fish: 3, action: 4, access: 4, scenery: 5 },
  createdAt: 1786060800000,
}
```

`geometry.type` is `marker`, `line` or `polygon`; `coords` is a list of
`[lat, lng]` pairs (one for a marker, many for a line or polygon). Add
`classified: true` for water needing a supplemental licence and it renders purple.

Regions are defined in `REGION_LABELS` and `REGION_VIEWS` near the top of the
script — adding one also means updating the two `<select>` lists and the
region-jump buttons.

## A word on the regulations

The seeded season dates, closures and gear rules were researched from primary
sources — provincial and state regulation documents rather than fishing blogs —
but **they go stale, and they are not a substitute for the current synopsis.**
Regulations change in-season. Check before you fish:

- [BC Freshwater Fishing Regulations Synopsis](https://www2.gov.bc.ca/gov/content/sports-culture/recreation/fishing-hunting/fresh-water-fishing)
- [Alberta Sportfishing Regulations](https://www.alberta.ca/fishing-regulations)
- [Washington Sport Fishing Rules](https://wdfw.wa.gov/fishing/regulations)

## Licence

MIT — see [LICENSE](LICENSE). Take it, fork it, make it about your own water.
