# Roblox Analytics Tracker

This project provides a simple analytics tracker for Roblox games.  It polls
Roblox’s public **Games API** at regular intervals to record the total number
of visits and current concurrent players (CCU) for a list of universe IDs.
Each data point is stored on disk as JSON.  An Express.js server exposes
REST endpoints to retrieve the data and serves a small web dashboard that
displays the history using Chart.js.

## Features

* **Track multiple games** – configure as many universe IDs as you wish in
  `config.js`.  Universe IDs uniquely identify a game on Roblox; see the
  developer forum discussion for details【698823226887793†L118-L125】.
* **Periodic polling** – the tracker queries
  `https://games.roblox.com/v1/games?universeIds=<id>` to obtain the
  current visit count and CCU for each universe【105150575459631†L5-L8】.  The interval
  is configurable (10 minutes by default).
* **Growth metrics** – each data point includes the percentage growth of
  visits and CCU compared to the previous sample.  This makes it easy to
  visualise spikes and trends over time.
* **REST API** – access the collected data via `/api/data` (all games),
  `/api/data/:id` (single game) and `/api/growth/:id` (growth only).
* **Dashboard** – a lightweight web UI built with Chart.js lets you
  explore the data interactively.  Simply select a universe ID from the
  drop–down menu to display its charts.

## Getting Started

1. **Install dependencies**

   ```sh
   cd roblox-analytics
   npm install
   ```

2. **Configure the tracker**

   Edit `config.js` and populate the `universeIds` array with the
   universe IDs of the games you want to monitor.  You can also adjust
   the polling interval by changing `fetchIntervalMinutes`.

3. **Run the server**

   ```sh
   node app.js
   ```

   The server listens on port `3000` by default.  You can override this
   by setting the `PORT` environment variable.

4. **Open the dashboard**

   Navigate to `http://localhost:3000` in your browser.  Choose a
   universe from the drop–down list to view its historical visits and
   concurrent players, along with the growth percentages between
   samples.

## Caveats

* The script must run on a machine that can reach `games.roblox.com`.
  The environment used to develop this project could not connect to
  Roblox’s servers directly; however, the API endpoint described on
  the developer forum【698823226887793†L118-L125】 returns JSON data with `visits` and
  `playing` fields【105150575459631†L5-L8】 when accessed over the internet.
* The tracker is unauthenticated and only uses public endpoints.  You
  may need a Roblox security cookie (.ROBLOSECURITY) to access some
  restricted data, but visits and CCU are publicly available.

## License

This project is licensed under the MIT license.