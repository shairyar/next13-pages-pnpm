# Installation Notes

## Install

```bash
pnpx create-next-app@latest
cd next13-pages-pnpm
pnpm add @appsignal/nodejs
```

## Create appsignal.cjs

```
const { Appsignal } = require("@appsignal/nodejs");

new Appsignal({
  active: true,
  name: "next13-pages-pnpm",
  pushApiKey: "820b07cb-8c9c-4748-abaa-1fd7954e9172",
});
```

## Create server.js

```js
const { createServer } = require("http");
const { parse } = require("url");
const next = require("next");

const dev = process.env.NODE_ENV !== "production";
const app = next({ dev });
const handle = app.getRequestHandler();
const port = parseInt(process.env.PORT, 10) || 3000;

app.prepare().then(() => {
  createServer((req, res) => {
    // Be sure to pass `true` as the second argument to `url.parse`.
    // This tells it to parse the query portion of the URL.
    const parsedUrl = parse(req.url, true);

    // You might want to handle other routes here too, see
    // https://nextjs.org/docs/advanced-features/custom-server
    handle(req, res, parsedUrl);
  }).listen(port, () => {
    console.log(`> Ready on http://localhost:${port}`);
  });
});
```

## Update package.json

```json
{
  "dev": "node server.js",
  "start": "NODE_ENV=production node --require ./appsignal.cjs server.js"
}
```

## Build & Run

```bash
pnpm build && pnpm start
```

http://localhost:3000/
