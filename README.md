# PhoneFinder

A Bluetooth **proximity finder** web app to help locate a lost device. Single file, no build step.

Open `index.html` — the screen splits into:

- **Top (Finder):** the device you're tracking — a signal ring, an estimated distance, a direction arrow, and a beep that **speeds up and rises in pitch as you get closer**.
- **Bottom (Nearby devices):** a live list of Bluetooth devices around you, sorted into **📱 Phones & tablets** and **🎧 Other devices** (tabs: All / Phones / Devices). Tap one for details, rename it, or tap **Track**.

### How devices are identified
Most phones don't broadcast a readable name, so the app works out what a device is from the data it *does* broadcast:
- **Manufacturer ID** (Apple, Samsung, Google, Microsoft, Xiaomi, Huawei, Garmin, Sony…)
- **Apple Continuity type** — tells an iPhone/iPad apart from AirPods, an Apple Watch or an AirTag/Find My item
- **Service UUIDs** — e.g. Google Fast Pair accessories, Tile, Samsung SmartTag, heart-rate straps, keyboards
- **Advertised name patterns** — "Pixel", "Galaxy S…", "iPhone", "Buds", "Watch"…

The details sheet shows *what it was identified as* and *how we know*.

### Precision
- Signal strength is run through a **Kalman filter**, so the ring, distance and beep rate are steady instead of jumping around.
- The direction arrow uses a **weighted average of the strongest headings** from your full-circle turn (your body blocks the signal behind you), and shows a **confidence** rating. Hold the phone flat against your chest while turning for best results.

## What's real vs. what isn't

This app is honest about hardware/OS limits:

| You asked for | Reality |
|---|---|
| See nearby devices as you walk | ✅ Real, via Web Bluetooth scanning |
| Sort into Phones vs. Devices, identify the maker/type | ✅ From manufacturer data, Apple Continuity type, services and name |
| Rename a device | ✅ Saved in your browser |
| Beep faster the closer you get | ✅ Real, driven by signal strength (RSSI) |
| Direction arrow to the device | ⚠️ Approximate — built from your phone's **compass + signal strength** as you turn around. Not UWB-precise like a real AirTag (that needs special hardware apps can't use). |
| Get the device's **phone number / owner name** | ❌ Impossible. **No OS or web API exposes a nearby device's phone number** — it's blocked everywhere as an anti-stalking measure. The app shows only what Bluetooth actually broadcasts (advertised name, ID, signal, services). |
| Works with **no app on the other device** | ⚠️ Only if that device is **broadcasting Bluetooth** (most phones/earbuds/watches do). True "find it even when off / via a global network" is exclusive to Apple Find My and Google Find My Device. |
| Works on **all devices** | ⚠️ Real scanning needs **Chrome or Edge on Android / desktop**. **iPhone browsers don't support Bluetooth scanning at all.** A built-in **Demo mode** lets you try the full experience on any phone. |

## Running it

### Try the UI anywhere (Demo mode)
Just open the page. If your browser can't scan, it auto-switches to Demo mode with simulated devices so you can see how tracking, the arrow, and the beeping work.

### Real Bluetooth scanning (Android / desktop Chrome or Edge)
1. Serve the file over **https** (required for Web Bluetooth). Easiest: host it (e.g. GitHub Pages) or run a local https server.
2. In the browser, enable the experimental scanning API:
   - Open `chrome://flags` → search **"Experimental Web Platform features"** → **Enabled** → restart.
3. Open the page, tap **Scan**, and allow Bluetooth when prompted.

### Finding your lost phone with it
1. Make sure the lost phone's Bluetooth is **on** (and ideally paired/advertising).
2. Scan, find it in the list, **rename** it ("My phone"), tap **Track**.
3. Tap **Sound on** and walk around — the ring fills and beeps speed up as you near it.
4. Tap **Find direction** and slowly turn a full circle to lock the arrow onto the strongest signal.

## Note on intended use
Built for finding **your own** lost devices. Because phone numbers and identities are never exposed, it can't be used to identify or covertly track other people — by design.
