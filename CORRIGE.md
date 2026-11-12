# Corrigé — Parties 1 et 3

## Partie 1 — Modélisation économique

### 1. Cartographie de la plateforme

```
Consommateurs  <── mesure CO2, comparaison, transparence ──  EcoMarketplace  <── SDK gratuit, analytics ──  E-commerçants
               ── audience, usage de l'app (données) ──────>                   ── données d'émissions produits ──>
```

- **Effet de réseau direct (intra-versant)** : plus il y a de consommateurs, plus les avis et comparaisons de l'application sont pertinents, et plus les données agrégées sont fiables.
- **Effet de réseau indirect (inter-versants)** : plus de marchands connectés, plus de produits mesurables, donc plus d'utilité pour le consommateur ; plus de consommateurs, plus les marchands ont intérêt à s'intégrer (visibilité, image « responsable »).

### 2. Rôle du SDK : le versant subventionné

Problème de la poule et de l'œuf : sans marchands, pas de consommateurs, et inversement. EcoMarketplace subventionne le versant **marchands** : SDK gratuit, quelques lignes de code, authentification, erreurs et retries déjà gérés. Le coût d'entrée (temps d'intégration, risque technique) tend vers zéro. Cela amorce le versant offre, puis les effets indirects attirent les consommateurs, qui à leur tour attirent d'autres marchands.

## Partie 3 — Friction et analyse critique

### 1. DX et TTFC

- Passer de ~40 lignes (headers, JSON, gestion d'erreurs, retries) à 3 lignes supprime les décisions et les erreurs possibles : le **TTFC** passe de jours à minutes.
- Un TTFC bas augmente le taux de conversion « découverte → intégration », donc la vitesse d'arrivée des marchands. C'est une boucle de rétroaction : plus de marchands → plus de valeur pour les consommateurs → plus de pression pour intégrer.

### 2. Gouvernance et lock-in

- **Breaking changes** : le SDK expose une interface stable (`trackPurchase`) et masque le chemin et la version (`/v1`, `/v2`). La plateforme peut migrer la couche interne vers la v2 (mapping des champs, nouveau calcul de CO2) dans une version mineure du SDK ; le code marchand ne change pas. Complément : versionnement sémantique, dépréciation annoncée, `apiVersion` épinglable.
- **Lock-in** : le marchand dépend du SDK, de son format de données et de la plateforme (historique d'émissions, analytics). Risques : coût de migration, dépendance à la politique tarifaire ou à la disparition de la plateforme, données difficiles à exporter. Atténuations : API REST documentée et ouverte, export des données, SDK open source.
