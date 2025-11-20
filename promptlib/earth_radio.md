Recreating Radio Garden Webradio Webapp

Create a web application that reproduces
the look, feel and functionality of the Radio
Garden website. The site should allow users
to explore and listen to live radio stations
worldwide via an interactive 3D globe.Overall Concept

Base the core experience around a
rotatable 3D globe. The globe should use
high-resolution satellite imagery rather than
political maps to mirror Radio Garden's
emphasis on physical geography over
borders. Users should be able to rotate and zoom
this globe seamlessly across continents and
oceans.Radio stations appear as green glowing
dots (spheres) superimposed on the globe
en.wikipedia.org

The size of each dot scales with the
number of broadcasters in that area.
Densely populated areas show larger dots;
clicking a dot in such a cluster zooms into
the biggest city

Keep the UI minimalistic: remove

-----------------

Recreating Radio Garden WebsiteCreate a web application that reproduces the look, feel and functionality of the Radio Garden website. The site should allow users to explore and listen to live radio stations worldwide via an interactive 3D globe.Overall Concept

Base the core experience around a rotatable 3D globe. The globe should use high-resolution satellite imagery rather than political maps to mirror Radio Garden's emphasis on physical geography over borders.Users should be able to rotate and zoom this globe seamlessly across continents and oceans.Radio stations appear as green glowing dots (spheres) superimposed on the globe. The size of each dot scales with the number of broadcasters in that area. Densely populated areas show larger dots; clicking a dot in such a cluster zooms into the biggest city in that region, revealing smaller individual station markers.Data Sources and Integration

Source radio station data from open APIs like Radio Browser (https://www.radio-browser.info/) or similar public directories. Include at least 10,000+ stations worldwide, with metadata for each: name, frequency, location (lat/long), genre, language, and live stream URL.Fetch and cache station data on app load for fast performance. Handle errors gracefully (e.g., offline stations show as grayed-out). Update the dataset periodically via a background cron job if deploying to a server.For the globe texture, use NASA's Blue Marble or equivalent free high-res satellite imagery tiles (e.g., from https://visibleearth.nasa.gov/). Ensure seamless tiling for zoom levels from global to city-scale.User Interactions and Controls

Globe Controls: Mouse/touch drag to rotate, scroll/pinch to zoom. Add subtle inertia for smooth panning. Center the view on user location via geolocation API on first load (with permission prompt).
Station Selection: Hover over a dot to show a tooltip with station name, genre, and current track (if available via API). Click to select: animate a smooth zoom/fly-to on the station's location, then auto-play the stream.
Playback: Embed an HTML5 audio player for live streams. Support play/pause, volume slider, and mute. Show a now-playing bar at the bottom with station info, waveform visualizer, and "Visit Website" link if available. Allow background play (no pause on tab switch).
Search and Filters: Minimal top bar with search input (by city, country, genre) that filters/highlights dots on the globe. Add genre tags (e.g., news, music, talk) as clickable chips to recolor dots temporarily.
Additional Features: Double-click to "jump" to a location. Keyboard shortcuts (e.g., spacebar for play/pause). Share current station via URL params (e.g., ?station=bbc-london).

UI/Design Guidelines

Keep the UI minimalistic: remove all unnecessary elements to evoke wonder and immersion. Use a dark theme with subtle gradients (black space background fading to globe glow). No ads, sidebars, or footers—full-screen globe by default.Fonts: Clean sans-serif (e.g., Inter or system defaults) for tooltips and controls. Animations: Smooth 60fps transitions using CSS or WebGL. Ensure mobile responsiveness (touch-friendly, landscape/portrait).Accessibility: ARIA labels for interactive elements, high-contrast mode toggle, keyboard navigation, and captions for audio (if metadata supports).Technical Stack and Implementation

Build with vanilla HTML/CSS/JS for simplicity, or a lightweight framework like Three.js for 3D rendering (essential for globe and particle effects). Use WebGL for glowing dots (shader-based spheres with bloom effect).Host audio streams via CORS-enabled proxies if needed. Optimize for performance: LOD (level of detail) for distant dots, lazy-load high-res textures. Target 60fps on mid-range devices.Deploy as a static site (e.g., GitHub Pages or Vercel) with a simple index.html entry point. Include a README with setup instructions.Output the full source code in a single ZIP file or GitHub repo link, ready to run locally via a local server (e.g., python -m http.server). Test for cross-browser compatibility (Chrome, Firefox, Safari, Edge) and mobile (iOS/Android).Make it indistinguishable from the original in spirit—evoke the magic of discovering distant voices on a spinning world.

