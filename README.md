# Signal Marchés v4

Dashboard financier temps réel, single-file HTML, hébergé sur GitHub Pages. Agrège des données de marché depuis plusieurs API publiques, calcule des signaux techniques d'achat/vente et les enrichit avec l'actualité et un indice Fear & Greed synthétique.

🔗 **Démo** : [gonnno.github.io/Signal-Marches-v4](https://gonnno.github.io/Signal-Marches-v4)

> ⚠️ Informatif uniquement — pas un conseil en investissement. Risque très élevé sur produits à levier.

## Fonctionnalités

- **7 marchés thématiques** : semi-conducteurs, Nasdaq 100, bourse européenne, pétrole & énergie, or & métaux, crypto, matières premières — chacun avec signal ACHAT / VENTE / NEUTRE / DANGER calculé sur RSI-14, SMA5/20, momentum 5j/20j et volatilité.
- **14 indices mondiaux** (onglet Monde) : Amériques, Europe, Asie-Pacifique, approximés via ETF cotés US pour contourner les limites CORS des API gratuites sur les places non-américaines.
- **Panneau "Pourquoi ce signal ?"** : facteurs haussiers/baissiers détaillés par actif, score technique vs impact des actualités, barre de confiance.
- **Fil d'actualités filtrable** (haussier / baissier / neutre) agrégé depuis 3 sources indépendantes.
- **Radar de volatilité** : scan automatique des ETF les plus agités du jour.
- **Sélection d'instruments Trade Republic** sur signal ACHAT/VENTE.
- **Liens utiles** : bourses officielles, calendriers économiques, data, crypto.
- **Thème clair/sombre**, navigation par swipe sur mobile, auto-refresh configurable.
- Historique de prix local (60 jours glissants, `localStorage`) pour calculer les tendances 5j/20j sans dépendre d'un abonnement API premium.

## Marchés couverts

| Marché | Actifs |
|---|---|
| 🔬 Semi-conducteurs | NVDA, AMD, TSMC, Broadcom, ASML |
| 📈 Nasdaq 100 | QQQ, Microsoft, Apple, Meta, Alphabet |
| 🇪🇺 Bourse Européenne | TotalEnergies, Airbus, LVMH, Sanofi, BNP, L'Oréal |
| 🛢️ Pétrole & Énergie | USO/WTI, ExxonMobil, Chevron, Occidental |
| 🥇 Or & Métaux | GLD, Argent, Minières Or |
| ₿ Crypto | Bitcoin, Ethereum, Solana, XRP |
| 🌾 Matières premières | Blé, Café, Sucre, Gaz naturel, Cuivre |

## Sources de données

| Source | Usage |
|---|---|
| [Finnhub](https://finnhub.io) | Actions, ETF, indices mondiaux, historique de prix |
| [CoinGecko](https://coingecko.com) | Crypto (primaire) |
| [Newsdata.io](https://newsdata.io) | Actualités FR |
| [Alpha Vantage](https://alphavantage.co) | Prix café/sucre + `NEWS_SENTIMENT` (score de sentiment calculé) |
| [Twelve Data](https://twelvedata.com) | Historique café/sucre |

## Stack technique

```
Format       : Single-file HTML (~1 800 lignes)
Hébergement  : GitHub Pages
Fonts        : IBM Plex Mono + Space Grotesk (Google Fonts)
Charts       : SVG inline (sparklines custom)
Storage      : localStorage (historique, quota, thème)
Dépendances  : aucune — pas de framework, pas de build
```

## Configuration

Les clés API se trouvent dans le bloc `CONFIG` en haut du `<script>` d'`index.html` :

```js
const CONFIG = {
  FINNHUB_KEY: '...',
  NEWSDATA_KEY: '...',
  ALPHAVANTAGE_KEY: '...',
  TWELVEDATA_KEY: '...',
};
```

Elles sont en clair dans le code source — inévitable sur un site 100% statique sans backend. Pour de vraies clés privées, il faudrait un relais serveur (Cloudflare Worker / Vercel function).

## Déploiement

1. Pousser `index.html` sur la branche `main`
2. **Settings → Pages → Source : Deploy from a branch → Branch : main / (root)**
3. Le site est en ligne à `<owner>.github.io/<repo>`

Aucune étape de build requise.

## Limites connues

- **Café / Sucre** : historique parfois incomplet selon la disponibilité Twelve Data sur le plan gratuit.
- **Europe** : cotations via ADR/OTC US (Finnhub gratuit ne supporte pas les tickers Euronext natifs) — léger décalage vs le prix Euronext réel.
- **Marché fermé** : le signal peut rester influencé par la dernière séance disponible en l'absence de données intraday.
- **Quota Newsdata.io** : 200 requêtes/jour sur le plan gratuit, ~20 analyses/jour avant blocage (reset automatique à minuit UTC).
- **Alpha Vantage** : 25 requêtes/jour sur le plan gratuit — l'appel `NEWS_SENTIMENT` est groupé sur un panier de tickers pour ménager le quota.

## Roadmap

- [ ] Indicateur MACD
- [ ] Export PDF du rapport de signal
- [ ] Historique Café/Sucre plus robuste (plan Twelve Data payant ou source alternative)
- [ ] Indication explicite "données J-1" quand le marché est fermé
