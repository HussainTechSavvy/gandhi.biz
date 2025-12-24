# Gandhi.biz Welcome Page

A professional, minimalist welcome page for the domain **gandhi.biz**, owned by Gandhi Family.

## Features

- **Premium Design**: Dark mode aesthetic with glassmorphism effects and subtle animated gradients.
- **Dynamic Content**: Fetches and displays a random quote on page load.
- **Responsive**: Fully optimized for mobile and desktop devices.
- **Custom Favicon**: SVG favicon with a generic "G" logo matching the site's theme.
- **Optimized Performance**: Served via Nginx with gzip compression and security headers.

## Tech Stack

- **Frontend**: HTML5, CSS3 (Vanilla), JavaScript (Vanilla)
- **Server**: Nginx (Alpine Linux)
- **Infrastructure**: Docker & Docker Compose

## Getting Started

### Prerequisites

- Docker
- Docker Compose

### Running the Project

1. Clone the repository (if applicable).
2. Start the container:

```bash
docker-compose up -d
```

3. Visit `http://localhost:8080` in your browser.

## Project Structure

```
.
├── docker-compose.yaml  # Docker service configuration
├── nginx.conf           # Optimized Nginx configuration
├── public/              # Static assets
│   ├── favicon.svg      # Site favicon
│   ├── index.html       # Main HTML file
│   └── style.css        # Stylesheets
└── .env                 # Environment variables
```

## License

Private property of Gandhi Family.
