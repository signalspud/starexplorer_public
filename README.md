# Star Explorer Web Application

## This is the Public repo created to link GitHub Issues.
### There is no code in this repo.

A full-stack web application for exploring detailed information about 2.5 million stars, their exoplanets, and binary companions. Built with React, Node.js, Express, and PostgreSQL, featuring animated visualizations.

## Features

- **Comprehensive Star Catalog**: Access data from ATHYG v3.3, combining Gaia DR3, Hipparcos, Tycho-2, and other major astronomical catalogs
- **Interactive 3D Visualization**: Real-time orbital simulations of exoplanetary systems using Three.js
- **Multiple Display Modes**: View stars in list, card, or detailed table formats
- **Advanced Search**: Search by star name, catalog IDs, constellation, magnitude, and spectral type
- **Exoplanet Integration**: Discover 4,100+ confirmed exoplanets including the complete Kepler mission catalog
- **Binary Star Systems**: Explore 9,300+ binary companions from spectroscopic (SBX) and visual (WDS) catalogs
- **Star Name Aliases**: Search stars by multiple identifications (Bayer, Flamsteed, catalog numbers)
- **Rich Data Display**: View photometry, kinematics, positions, calculated properties, and more
- **Space-Themed UI**: Beautiful dark mode interface with smooth animations

## Tech Stack

### Frontend
- React 18
- Vite
- React Router
- Three.js with React Three Fiber
- Axios

### Backend
- Node.js
- Express
- PostgreSQL
- CSV Parser

### External APIs
- NASA Exoplanet Archive
- SIMBAD Astronomical Database
- SBX Spectroscopic Binary Catalog (ULB Brussels)
- WDS Washington Double Star Catalog (VizieR)

## License

MIT

## Credits

- **Star Catalog**: ATHYG v3.3 by Astronexus (https://codeberg.org/astronexus/athyg)
  - Combines data from Gaia DR3, Hipparcos, Tycho-2, HYG, and other sources
- **Exoplanet Data**: NASA Exoplanet Archive (https://exoplanetarchive.ipac.caltech.edu/)
  - Includes Kepler mission discoveries and WASP catalog
- **Binary Star Data**:
  - SBX Spectroscopic Binary Catalog - ULB Brussels (https://astro.ulb.ac.be/sbx)
  - WDS Washington Double Star Catalog - US Naval Observatory (http://www.astro.gsu.edu/wds/)
- **External APIs**:
  - SIMBAD Astronomical Database (https://simbad.cds.unistra.fr/)
  - Wikipedia MediaWiki API
