# Cifra Club API — Render setup

This package updates the API so the Selenium endpoint can be configured with `SELENIUM_URL`, and the web app binds to Render's `PORT`.

## Important before deploying
The API needs a separate Selenium/Firefox service. Render does not run `docker-compose.yml` automatically for a Web Service. The free Web Service has limited RAM and may not be enough for Firefox; do not assume this can run reliably at €0/month.

## API service
- Runtime: Docker
- Repository root / Docker build context: `.`
- Dockerfile path: `./Dockerfile`
- Environment variable: `SELENIUM_URL` = internal URL of the Selenium service ending in `/wd/hub` (for example `http://<selenium-internal-host>:4444/wd/hub`)
- Health check path: `/`

## Selenium service
- Docker image is based on `selenium/standalone-firefox:4.27.0`; if creating a second Render service from this repository, set Dockerfile path to `./Dockerfile.selenium` and build context to `.`.
- The API must be able to reach port `4444` on the Selenium service over a private network.
- If Render requires a paid private service or the free plan cannot provide private networking/resources, this two-service setup will not be fully free. Do not enter payment details or upgrade without deciding first.

## Quick local check
Use Docker Compose locally. Set the app service environment variable `SELENIUM_URL` to `http://selenium:4444/wd/hub`; the provided `docker-compose.yml` already uses the service name `selenium`.

The original scraper can still return an error if Cifra Club changes its page or blocks automated access. Verify the endpoint after deployment.
