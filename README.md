# Raspberry Pi Climate Monitor

A Raspberry Pi IoT project that reads live temperature and humidity data from a DHT22 sensor and visualises it in two ways — a real-time Matplotlib chart and a Flask web dashboard with Server-Sent Events.

---

## Overview

Built on a Raspberry Pi, this system continuously polls a DHT22 temperature/humidity sensor and streams the data to your choice of display. A Tkinter launcher lets you pick between the local Matplotlib graph or the browser-based web dashboard, both of which update live every 60 seconds.

---

## Features

- **DHT22 sensor integration** — reads real temperature (°C) and humidity (%) every 60 seconds via the Adafruit CircuitPython DHT library
- **Matplotlib live chart** — animated line + scatter plot that updates in real time using `FuncAnimation`
- **Flask web dashboard** — self-hosted local web server with a live chart pushed to the browser via Server-Sent Events (SSE)
- **Statistical summary** — displays max/min values and a derived humidity-to-temperature equation on the web dashboard
- **Tkinter launcher** — simple GUI to choose between display modes at startup
- **Threaded architecture** — sensor polling, chart rendering, and web serving all run concurrently via Python's `threading` module
- **Fake data mode** — built-in test mode to develop and debug without physical hardware

---

## Tech Stack

- **Language:** Python 3
- **Hardware:** Raspberry Pi + DHT22 sensor
- **Libraries:** `Flask`, `matplotlib`, `adafruit-circuitpython-dht`, `tkinter`
- **Concurrency:** `threading` module for parallel sensor polling and display
- **Web:** Flask + Server-Sent Events (SSE) for real-time browser updates

---

## Project Structure

```
raspberry-pi-climate-monitor/
├── main.py                  # Entry point — launches the Tkinter mode selector
└── displays/
    ├── sensors.py           # DHT22 sensor polling, data storage, fake data mode
    ├── mat_plt.py           # Matplotlib animated live chart
    ├── web_graph.py         # Flask web server with SSE chart updates
    └── templates/
        ├── tk.py            # Tkinter launcher window
        └── index.html       # Web dashboard frontend
```

---

## How It Works

1. `main.py` opens a Tkinter window with two buttons — **Matplotlib** and **Web**
2. Selecting either mode spawns a background thread running the DHT22 sensor polling loop (`sensors.py`)
3. The sensor loop reads temperature and humidity every 60 seconds, appending values to shared in-memory lists
4. **Matplotlib mode** — `FuncAnimation` re-plots the full dataset every 60 seconds in a live window
5. **Web mode** — Flask serves `index.html` and streams JSON updates to the browser via `/chart-data` using SSE; the page auto-opens in the default browser

---

## Setup

**1. Clone the repo**
```bash
git clone https://github.com/candtk/raspberry-pi-climate-monitor.git
cd raspberry-pi-climate-monitor
```

**2. Install dependencies**
```bash
pip install flask matplotlib adafruit-circuitpython-dht
```

**3. Wire the DHT22**

Connect the DHT22 data pin to GPIO4 (D4) on the Raspberry Pi.

**4. Run**
```bash
python main.py
```

> To test without hardware, swap `data` for `fakedata` in `mat_plt.py` and `web_graph.py`.

---

## License

MIT
