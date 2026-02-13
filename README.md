# Star Explorer Web Application

## This is the Public repo created to link GitHub Issues.
There is no code in this repo.

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

## Project Structure

```
stars/
├── backend/               # Node.js/Express backend
│   ├── config/           # Database configuration and schema
│   ├── controllers/      # API endpoint controllers
│   ├── routes/           # Express routes
│   ├── scripts/          # Utility scripts (CSV import, DB init, catalog imports)
│   └── services/         # External API integrations
├── frontend/             # React frontend
│   ├── src/
│   │   ├── components/   # React components (StarVisualization, SEO)
│   │   ├── pages/        # Page components (Home, List, Detail, Compare)
│   │   ├── services/     # API service layer
│   │   └── styles/       # CSS stylesheets
│   └── public/
├── database/             # Star catalog data
│   └── athyg_v33.csv    # 2.5M+ stars dataset (ATHYG v3.3)
└── initial_design/       # Design mockups (SVG/PDF)
```

## Getting Started

### Prerequisites

- Node.js 18 or higher
- PostgreSQL 14 or higher
- 2GB+ free disk space (for database)

### 1. Clone the Repository

```bash
git clone <repository-url>
cd stars
```

### 2. Backend Setup

```bash
cd backend
npm install
```

Create a `.env` file from the example:

```bash
cp .env.example .env
```

Edit `.env` with your PostgreSQL credentials:

```env
DB_HOST=localhost
DB_PORT=5432
DB_NAME=stars_db
DB_USER=postgres
DB_PASSWORD=your_password
PORT=3001
```

Initialize the database:

```bash
node scripts/initDB.js
```

Import the star catalog (takes ~5-10 minutes):

```bash
npm run import-csv
```

Start the backend server:

```bash
npm run dev  # Development mode with auto-reload
# or
npm start    # Production mode
```

The API will be available at `http://localhost:3001`

### 3. Frontend Setup

```bash
cd ../frontend
npm install
```

Create a `.env` file:

```bash
cp .env.example .env
```

The default configuration should work:

```env
VITE_API_URL=http://localhost:3001/api
```

Start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:5173`

## Usage

### Home Page
- Welcome screen with overview of the application
- Quick access to star catalog

### Star List Page
- Browse all stars in the catalog
- Switch between List, Card, and Table views
- Search by name, catalog ID, constellation, magnitude, or spectral type
- Pagination controls for navigating large datasets

### Star Detail Page
- View comprehensive information about a specific star
- Interactive 3D visualization showing the star and any orbiting exoplanets
- Expandable advanced data section with catalog IDs, kinematics, and coordinates
- Real-time orbital simulation with accurate relative speeds and distances

## API Endpoints

### Health Check
- `GET /api/health` - Check API and database status

### Stars
- `GET /api/stars` - Get all stars (paginated)
- `GET /api/stars/search?q=...` - Search stars
- `GET /api/stars/id/:id` - Get star by ID
- `GET /api/stars/name/:name` - Get star by name
- `GET /api/stars/:id` - Get detailed star information (includes exoplanets and binary companions)

### Exoplanets
- `GET /api/exoplanets/star/:starId` - Get exoplanets for a star
- `GET /api/exoplanets/search?q=...` - Search exoplanets
- `GET /api/exoplanets/:name` - Get exoplanet by name

### Binary Companions
- Binary companion data is included in star detail responses
- Includes orbital parameters, spectral types, and links to companion stars when available

## Database

**Current Database Version**: 1.3.0 (February 2026)

The PostgreSQL database contains:

- **stars table**: ~2,554,193 stars with 41 fields including:
  - Identifiers from multiple catalogs (Gaia, HIP, HD, HR, TYC, etc.)
  - Position data (RA, Dec, distance, Cartesian coordinates)
  - Photometry (magnitude, color index, spectral type)
  - Kinematics (proper motion, radial velocity, 3D velocities)
  - Stellar parameters (temperature, radius, mass, metallicity, surface gravity)

- **exoplanets table**: ~4,126 confirmed exoplanets with:
  - Planet properties (radius, mass, temperature)
  - Orbital parameters (period, semi-major axis, eccentricity)
  - Discovery information (method, year)
  - Host star linkage
  - Includes: Solar System (8), WASP (189), Kepler (2,711), and other confirmed planets

- **binary_companions table**: 9,337 binary star companions with:
  - Spectroscopic binaries (3,368 from SBX catalog)
  - Visual binaries (5,969 from WDS catalog)
  - Orbital parameters (period, semi-major axis, eccentricity)
  - Companion spectral types and cross-references

- **star_aliases table**: ~5,965,338 cross-references for searching stars by:
  - Bayer designations (e.g., "Alpha Centauri A")
  - Flamsteed numbers (e.g., "61 Cygni")
  - Catalog identifiers (TYC, HIP, HD, HR, Gl/GJ)
  - Kepler catalog aliases (KIC, KOI)

### Database Management

For detailed information on database structure, versioning, and safe update procedures:

- **[CLAUDE.md](CLAUDE.md)**: Complete technical documentation
  - Database schema and column mapping
  - Data source versions and update history
  - Detailed update procedures for each data source
  - Emergency rollback procedures

- **[DATA_SOURCE_GUIDE.md](DATA_SOURCE_GUIDE.md)**: Quick reference guide
  - Column-to-source mapping
  - Update commands for each data source
  - Version bump guidelines
  - Safety checklist

## Data Sources

### Star Catalog
- **ATHYG v3.3**: Combined star catalog (2,552,164 stars)
  - Gaia Data Release 3 (DR3) - June 2022
  - Hipparcos - Original + New Reduction
  - Tycho-2 - March 2000
  - Henry Draper (HD) catalog
  - Harvard Revised (HR) / Bright Star Catalogue
  - Gliese catalog (Gl/GJ)
  - HYG Database v4.2 (IAU-approved proper names)
  - **Source**: https://codeberg.org/astronexus/athyg

### Exoplanet Data
- **NASA Exoplanet Archive**: Confirmed exoplanets (~4,126 planets)
  - Planetary Systems Composite Parameters table
  - WASP catalog (176 host stars, 189 planets)
  - Kepler mission (1,948 host stars, 2,711 planets)
  - **Source**: https://exoplanetarchive.ipac.caltech.edu/

- **Custom Data**: Solar System planets (8 planets orbiting Sol)

### Binary Star Catalogs
- **SBX (Spectroscopic Binary Catalog)**: 3,368 companions
  - Close binary systems detected via Doppler shift
  - Orbital periods from days to months
  - **Source**: https://astro.ulb.ac.be/sbx

- **WDS (Washington Double Star Catalog)**: 5,969 companions
  - Visual binary systems (wide separation)
  - Orbital periods from years to centuries
  - **Source**: http://www.astro.gsu.edu/wds/

## Development

### Backend Development

```bash
cd backend
npm run dev  # Uses nodemon for auto-reload
```

### Frontend Development

```bash
cd frontend
npm run dev  # Vite dev server with hot reload
```

### Building for Production

Frontend:
```bash
cd frontend
npm run build
npm run preview  # Preview production build
```

## Performance Notes

- The CSV import process takes 5-10 minutes to load 2.5M+ stars
- Database indexes are created automatically for optimal query performance
- Frontend uses pagination to efficiently display large result sets
- Three.js visualizations are optimized with efficient rendering techniques

## Deployment

This application can be deployed to Render.com using the included `render.yaml` blueprint configuration.

### Quick Deploy to Render

1. Push your code to GitHub
2. Go to [Render Dashboard](https://dashboard.render.com)
3. Click "New" → "Blueprint"
4. Connect your GitHub repository
5. Render will automatically detect `render.yaml` and deploy all services

For detailed deployment instructions, database setup, and environment configuration, see **[DEPLOYMENT.md](DEPLOYMENT.md)**.

### Deployment Components

The application deploys as three services:
- **PostgreSQL Database**: Stores star and exoplanet data
- **Backend API**: Node.js/Express server
- **Frontend**: React static site (Vite build)

All environment variables and service connections are configured automatically via `render.yaml`.

## Future Enhancements

- Galaxy view showing star positions in 3D space
- User annotations and favorites
- Advanced filtering by multiple criteria
- Constellation artwork overlays
- Mobile-responsive 3D controls
- Export functionality for star data
- Integration with additional astronomical databases

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
