[README.md](https://github.com/user-attachments/files/28430913/README.md)
# InvoiceFlow

Two-stage invoice approval workflow: upload → PM annotates with code + signature → LP countersigns → approved PDF downloaded.

---

## Quick start (local)

```bash
npm install
npm start
```

Open http://localhost:3000

---

## How it works

| Role | Action |
|------|--------|
| **Uploader** | Upload a PDF invoice → receive a `PM-XXXXXXXX` token to share |
| **PM** | Enter token → see invoice → add project code + draw signature → receive `LP-XXXXXXXX` token |
| **LP** | Enter token → see invoice + PM approval → draw countersignature → approved PDF downloads |

Invoices are stored in the `data/` folder as JSON files. The archive sidebar shows live status for all invoices.

---

## Deploy to Railway (recommended — free tier)

1. Push this folder to a GitHub repo
2. Go to [railway.app](https://railway.app) → New Project → Deploy from GitHub
3. Select your repo — Railway auto-detects Node.js and runs `npm start`
4. Your app gets a public URL (e.g. `https://invoiceflow-production.up.railway.app`)

All three parties (uploader, PM, LP) visit the **same URL**. The PM and LP enter their token on the home screen.

---

## Deploy to Render (also free)

1. Push to GitHub
2. [render.com](https://render.com) → New Web Service → connect repo
3. Build command: `npm install`
4. Start command: `node server.js`
5. Done — Render gives you a public URL

---

## Deploy anywhere with Node 18+

```bash
# On your server:
git clone <your-repo>
cd invoiceflow
npm install
PORT=3000 node server.js

# Or with PM2 for persistence:
npm install -g pm2
pm2 start server.js --name invoiceflow
pm2 save
```

---

## File structure

```
invoiceflow/
├── server.js          Express backend (API + static serving)
├── package.json
├── public/
│   └── index.html     Full frontend (self-contained)
└── data/              Invoice store (auto-created on first run)
    ├── _index.json    Invoice metadata list
    └── ABCD1234.json  One file per invoice (includes PDF as base64)
```

---

## Notes

- **PDF size limit**: 8 MB per invoice (configurable in `server.js` — change `12mb` in `express.json` limit)
- **Storage**: Invoices are stored as base64 JSON on disk. For production with many large files, swap `data/` for S3 or similar.
- **No auth**: Tokens are the only access control. Add your own auth layer (e.g. Clerk, Auth0) if needed.
- **HTTPS**: Railway and Render provide HTTPS automatically. If self-hosting, add a reverse proxy (nginx + certbot).
