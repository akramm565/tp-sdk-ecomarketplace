# Starter Kit — TP Digital Ecosystems & SDK

Concevoir le SDK d'une plateforme multi-faces (EcoMarketplace) et activer les effets de réseau indirects.

## Arborescence

```
tp-sdk-ecomarketplace/
├── README.md
├── CORRIGE.md            # Réponses aux parties 1 et 3 (enseignant)
├── backend/              # API de la plateforme (FastAPI)
│   ├── main.py
│   └── requirements.txt
└── client-demo/          # Simulation du site e-commerce partenaire
    ├── app.js            # Démo : sans SDK vs avec SDK
    └── ecosdk.js         # EcoSDK (solution)
```

## Démarrage rapide

**1. Lancer le backend**
```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload --port 8000
```
API : http://localhost:8000 — Swagger : http://localhost:8000/docs

**2. Lancer la démo marchand** (Node.js >= 18)
```bash
cd client-demo
node app.js
```

## Utilisation du SDK

```js
const { EcoSDK } = require("./ecosdk");
const eco = new EcoSDK({ apiKey: "mch_live_99x821a" });
await eco.trackPurchase({ orderId: "ORD-12345", items: [{ category: "electronics", weightKg: 1.2 }] });
```

Options : `baseUrl`, `apiVersion` (défaut `v1`), `maxRetries` (3), `timeoutMs` (5000), `onError`.
Méthodes : `trackPurchase` (alias `trackCarbon`), `trackPurchaseAsync` (non bloquant), `getAnalytics`.

Clés de test : `mch_live_99x821a` (EcoShop Paris), `mch_live_44z102b` (TechStore Europe).

## Contraintes d'architecture satisfaites

- Authentification automatique (header `X-API-Key`)
- Retry avec backoff exponentiel sur erreurs réseau, 429 et 5xx (jamais sur 4xx), timeout
- Mode non bloquant `trackPurchaseAsync` pour ne pas pénaliser le site marchand
- Initialisation en une ligne
