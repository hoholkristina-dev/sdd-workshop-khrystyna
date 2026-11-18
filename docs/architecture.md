```mermaid
flowchart TD
  scripts["package.json: dev, start"] --> memory["scripts/with-memory-mongo.js"]
  memory --> server["MongoMemoryServer"]
  memory --> seed["scripts/seed-memory-mongo.js"]
  seed --> collections["pages, foundationinfos, navigations"]
  memory -->|"MONGO_URL"| next["процес Next.js"]
  next --> config["app/config/index.ts"]
  config --> connect["infrastructure/db/connect.ts"]
  connect --> server

  repos["infrastructure/repositories"] --> connect
  repos --> models["infrastructure/models"]
  repos --> domain["domain/dto"]

  di["app/di: Awilix"] --> repos
  di --> services["app/services"]
  services --> repos

  layout["app/lang layout: Header.server"] --> di
  serverPages["сторінки через resolvePageData або resolve"] --> di
  archivePage["archive/page.tsx"] -->|"fetch /api/funds"| api["app/api: funds, page, header, compositions, scientific-works"]
  tables["MusicTableSection, WorkTableSection"] --> client["services/client/tableService.ts"]
  client --> api
  api --> di
  health["app/api/health"] --> connect
  ```
