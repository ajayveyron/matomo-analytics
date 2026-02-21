# Matomo Analytics (Standalone)

Standalone self-hosted Matomo stack for tracking many projects in one place.

## 1) Start Matomo

```bash
cd /Users/ajaypawriya/Desktop/Projects/matomo-analytics
docker compose up -d
```

Open `http://localhost:8080` and complete the installer.

## 2) Configure websites

In Matomo Admin:

1. Create one website per project.
2. Copy each website's `Site ID`.

Use Matomo's **All Websites** view for the combined dashboard.

## 3) Tracking snippet for projects

Use this snippet in each project (with project-specific `siteId`):

```html
<script>
  window.__MATOMO__ = {
    url: "https://analytics.yourdomain.com/",
    siteId: "1",
  };

  (function initMatomo() {
    const config = window.__MATOMO__ || {};
    if (!config.url || !config.siteId) return;

    const baseUrl = String(config.url).endsWith("/")
      ? String(config.url)
      : `${String(config.url)}/`;

    window._paq = window._paq || [];
    window._paq.push(["trackPageView"]);
    window._paq.push(["enableLinkTracking"]);
    window._paq.push(["setTrackerUrl", `${baseUrl}matomo.php`]);
    window._paq.push(["setSiteId", String(config.siteId)]);

    const d = document;
    const g = d.createElement("script");
    const s = d.getElementsByTagName("script")[0];
    g.async = true;
    g.src = `${baseUrl}matomo.js`;
    s.parentNode.insertBefore(g, s);
  })();
</script>
```

## 4) Files in this repo

- `docker-compose.yml`: Matomo + MariaDB
- `.env`: local secrets (already generated)
- `.env.example`: template
- `.gitignore`: ignores `.env` and local DB data

## 5) Useful commands

```bash
# stop
docker compose down

# logs
docker compose logs -f matomo

# update containers
docker compose pull && docker compose up -d
```
