Submission for the Far Away Hackathon A zero-latency, client-side mission control dashboard for ISRO telemetry and orbital tracking.

 The Concept
Most space-tech hackathon projects rely on heavily rate-limited reverse-geocoding APIs or generic dashboard templates. OrbNet was built with a different philosophy: what if we built a dashboard that a senior flight director at ISRO or JPL would actually want to use?

OrbNet is a strictly frontend, single-page application that tracks the CARTOSAT-2B satellite. It moves all the heavy computational lifting—orbital mechanics, spatial geocoding, and anomaly detection—directly to the user's browser edge.

The Design Philosophy: Restrained, technical, and confident. No glassmorphism, no generic rounded-rectangle grids, and no bloated frameworks. Just pure, dense, information-rich UI.

 Core Frontend Features
1. Optimistic UI & Zero-Wait Boot
Waiting for network requests to resolve breaks the immersion of mission control software. OrbNet boots instantly by injecting a known-good orbital cache into the rendering engine, calculating the map and telemetry on frame one. It then silently syncs with the live CelesTrak API in the background, updating the orbit model without stuttering the UI.

2. "API-less" Client-Side Geocoding
Instead of hitting external APIs to figure out what country the satellite is flying over (which causes lag and rate-limiting), OrbNet uses pure spatial mathematics. We load a lightweight TopoJSON geometry file and use D3.js point-in-polygon math to calculate the satellite's exact terrestrial region entirely within the browser.

3. Edge-Computed Orbital Mechanics
Using satellite.js, OrbNet propagates standard Two-Line Element (TLE) sets into live geodetic coordinates (Latitude, Longitude, Altitude, Velocity) 60 times a second. It also dynamically computes the next pass over the Bangalore Ground Station by checking real-time elevation angles.

4. Procedural Telemetry & Z-Score Anomaly Detection
To simulate hardware health (Voltage, Temp, SNR), the frontend utilizes a custom mathematical noise model that occasionally injects procedural anomalies. A rolling standard deviation (Z-Score) algorithm continually analyzes this stream, logging "Isolation Forest-style" alerts to the terminal when values exceed a 3.5σ threshold.

 The Tech Stack (100% Client-Side)
OrbNet requires zero build steps, zero backend servers, and zero API keys.

Architecture: Pure HTML5, CSS3, and Vanilla ES6 JavaScript.

Orbital Math: satellite.js (SGP4/SDP4 propagation).

Mapping & Spatial: d3.js & topojson-client (Equirectangular projections and graticule rendering).

Typography: Google Fonts (Roboto & Roboto Mono).

Data Source: CelesTrak (Live TLE text feeds via standard web fetch).

 Getting Started
Because this project is entirely frontend-based with zero dependencies, running it is incredibly simple.

Clone the repository:

Bash
git clone https://github.com/yourusername/orbnet.git
Navigate to the project directory:

Bash
cd orbnet
Open the file in any modern web browser:

Bash
# On Mac
open index.html

# On Windows
start index.html

# On Linux
xdg-open index.html
(Alternatively, you can host it instantly via GitHub Pages or simply drag and drop index.html into your Chrome/Firefox tab).

Future Frontend Roadmap
If we were to continue developing this post-hackathon, the next UI/UX steps would be:

WebGL 3D Globe: Swapping the 2D D3 projection for a Three.js interactive 3D earth model.

Web Workers: Moving the satellite.js array calculations (like the 90-minute historical ground track computation) into a Web Worker to ensure the main UI thread never drops below 60fps.

User-Selectable Satellites: Adding a frontend search index to filter and track any of the 20,000+ NORAD objects currently in orbit.
