<div align="center">
  <img src="docs/favicon.svg" width="96" alt="gandhi.biz logo">

  # gandhi.biz

  *A minimalist welcome page with glassmorphism design and dynamic content*

</div>

Static landing page for the **gandhi.biz** domain, owned by Gandhi Family. Features a dark-mode glassmorphism aesthetic with animated ambient gradients, staggered entrance animations, and a dynamically fetched quote on every visit.

## Features

- **Glassmorphism UI** — Frosted glass card with backdrop blur, subtle borders, and soft shadow depth
- **Staggered entrance** — Elements animate in sequence (scale + fade-up) with `cubic-bezier` easing
- **Ambient glow** — Slow-moving radial gradients behind the card for a living, cinematic feel
- **Dynamic quotes** — Fetches a random quote on each visit with a crossfade transition, falls back gracefully
- **Responsive** — Adapts layout and typography across mobile (375px), tablet (768px), and desktop
- **Reduced motion** — Respects `prefers-reduced-motion` by disabling all animations and transitions
- **Security headers** — Nginx configured with CSP, X-Frame-Options, X-Content-Type-Options, and Referrer-Policy
- **Static asset caching** — 1-year cache for CSS/JS/images with `no-transform`

## Tech Stack

| Layer       | Technology                                          |
|-------------|-----------------------------------------------------|
| Frontend    | HTML5, CSS3 (Custom Properties), Vanilla JavaScript |
| Fonts       | [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts |
| Web Server  | Nginx with gzip, security headers, static caching   |
| Tunnel      | Cloudflare Tunnel (`cloudflared`)                   |
| Ops         | Docker Compose, Kubernetes (Flux CD)                |

## Project Structure

```
.
├── docs/                    # Static assets served by Nginx
│   ├── favicon.svg          # Custom "G" logo SVG
│   ├── index.html           # Main HTML page
│   └── style.css            # Design tokens + all styles
├── k8s/                     # Kubernetes manifests
│   ├── _namespace.yaml      # cloudflare-tunnel namespace
│   ├── deployment.yaml      # Cloudflare Tunnel pod
│   ├── secrets.yaml         # Tunnel token secret
│   ├── flux-crds.yaml       # Flux CD CRDs (v2.7.5)
│   └── cluster-admin.yaml   # RBAC for cluster admin
├── docker-compose.yaml      # Docker Compose stack (Nginx + cloudflared)
├── nginx.conf               # Nginx configuration
├── .editorconfig            # Editor settings
└── .env                     # TUNNEL_TOKEN environment variable
```

## Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and Docker Compose
- A Cloudflare Tunnel token (set in `.env` as `TUNNEL_TOKEN`)

### Local Development

Serve the static files directly for quick iteration:

```bash
python3 -m http.server 8080 --directory docs
# or
npx serve docs
```

Open `http://localhost:8080` in your browser.

### Docker Compose

```bash
# Set your Cloudflare Tunnel token
echo "TUNNEL_TOKEN=your-token-here" > .env

# Start the stack
docker compose up -d

# Check health
docker compose ps
```

The page is served at `http://localhost:8080` and tunneled through Cloudflare.

### Kubernetes

```bash
# Create namespace and apply resources
kubectl apply -f k8s/_namespace.yaml
kubectl apply -f k8s/secrets.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/cluster-admin.yaml

# Apply Flux CD CRDs for GitOps
kubectl apply -f k8s/flux-crds.yaml
```

> [!NOTE]
> The K8s deployment runs only the Cloudflare Tunnel pod. The Nginx + static files stack runs via Docker Compose on the same node. Flux CD CRDs are included for GitOps reconciliation of the manifests.

## Design Tokens

The design system is defined as CSS custom properties in `:root`:

```css
--color-background:          #0f0f0f
--color-foreground:          #ffffff
--color-foreground-secondary: rgba(255,255,255,0.7)
--color-surface:             rgba(255,255,255,0.03)
--color-border:              rgba(255,255,255,0.08)
--color-primary:             #4facfe          /* Cyan-blue */
--color-accent:              #00f2fe          /* Teal */
--font-family:               'Inter', system-ui, sans-serif
--radius-lg:                 24px
--easing-out:                cubic-bezier(0, 0, 0.2, 1)
```

| Token                       | Contrast Ratio |
|-----------------------------|:-------------:|
| Foreground on Background    | **21:1** (AAA) |
| Secondary on Background     | **7.6:1** (AAA) |
| Surface border on Background| **1.5:1** (decorative) |
