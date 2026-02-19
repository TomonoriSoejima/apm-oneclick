Phase 1: One-click APM repro (Node.js)

Steps:
1. Create a .env file with:
   ELASTIC_APM_SERVER_URL
   ELASTIC_APM_SECRET_TOKEN
2. Run:
   docker compose up --build
3. Open http://localhost:3000 to use the UI for:
   1. choose deployment
   2. choose language (`java`, `go`, `python`, `js`)
   3. deployment auto-activates in the background, then start traffic
   4. open Kibana link to check APM data

Service name is dynamic when activating target:
- `serviceName = <deployment-name>-<language>`

Runtime behavior on this branch:
- `python` traffic is routed to `python-worker`.
- `java` traffic is routed to `java-worker`.
- `go` traffic is routed to `go-worker`.
- `js` traffic runs on the Node service.

Elastic Cloud API (dynamic extraction):
- The UI dynamically fetches deployments (`/cloud/list-deployments`) and selected deployment target config (`/cloud/deployments/:deploymentId/apm-target`).
- Set Cloud API key in `.env` using `ELASTIC_CLOUD_API_KEY`.
- Optional `.env` variables:
   - `ELASTIC_APM_KIBANA_URL=https://.../app/apm/services/demo-demo`
   - `ELASTIC_CLOUD_API_BASE_URL=https://api.elastic-cloud.com`
   - `ELASTIC_CLOUD_API_PREFIX=/api/v1`
   - `ELASTIC_CLOUD_AUTH_SCHEME=ApiKey`

Success:
- Service appears in Kibana APM within ~1 minute.