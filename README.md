# scanerator
Because real WMS pros don't just scan — they generate.
---

## 🇫🇷 Français

### Présentation

**SCANERATOR** est un outil autonome de génération de codes-barres conçu pour les environnements WMS (Warehouse Management System). Il s'agit d'un fichier HTML unique, sans installation, qui s'ouvre directement dans un navigateur web.

### Fonctionnalités

- **3 formats de codes-barres** : Code 128 (linéaire), QR Code, Data Matrix
- **Organisation par familles** : regroupez vos codes par catégorie (Emplacement, Support, Article, Numéro de série…)
- **Paramètres par famille** : format et dimensions personnalisables famille par famille
- **Générateur aléatoire** : injectez des codes générés automatiquement dans une famille existante
- **2 scénarios WMS préconfigurés** :
  - 📥 **Réception** : génère automatiquement les emplacements `QUAI_REC`, `POUMON_REC`, supports et articles
  - 📤 **Préparation** : génère les emplacements de picking, `POUMON_EXP`, supports et articles
- **Impression A4** : mise en page optimisée, regroupée par famille, prête à imprimer
- **Bilingue FR / EN** : toute l'interface bascule en un clic
- **Responsive** : utilisable sur desktop, tablette et mobile
- **Aucune installation** : un seul fichier `.html`, fonctionne hors-ligne après le premier chargement

### Prérequis

- Un navigateur web récent : **Chrome 80+**, **Edge 80+** ou **Firefox 75+**
- Une connexion internet au **premier lancement** uniquement (pour charger les bibliothèques JsBarcode et bwip-js depuis un CDN — mises en cache ensuite)

### Lancement

Directement via la version déployée :
👉 **[Ouvrir SCANERATOR](https://libertyben-code.github.io/scanerator/)**

### Structure de la liste

La zone de saisie principale accepte une liste brute, un élément par ligne :

```
# NomDeFamille
code001
code002

# AutreFamille
code010
code011
```

Les lignes commençant par `#` définissent une **famille**. Toutes les autres lignes non vides sont des **codes-barres**.

### Familles WMS par défaut

| Famille | Format exemple | Usage |
|---|---|---|
| Emplacement | `ALL-01-A-001` | Adressage allée-niveau-colonne-position |
| Support | `PAL-2024-001` | Identification palettes et bacs |
| Article | `REF-VISSERIE-M6` | Référence article avec désignation |
| Numéro de série | `SN-2024-00142` | Traçabilité unitaire avec millésime |

### Scénario Réception

L'onglet **Réception** (orange) génère automatiquement 4 familles pour le flux `QUAI_REC → POUMON_REC` :

1. **Emplacements Quai** — positions de déchargement (ex. `QUAI_REC-001`)
2. **Emplacements Poumon Réception** — tampon avant stockage (ex. `POUMON_REC-001`)
3. **Supports Réception** — palettes et bacs entrants (ex. `PAL-REC-001`)
4. **Articles Réception** — références à réceptionner (ex. `REF-REC-001`)

Paramétrable : préfixes, nombre de quais, nombre de poumons, nombre de supports, nombre d'articles.

### Scénario Préparation

L'onglet **Préparation** (violet) génère automatiquement 4 familles pour le flux `Stock → Picking → POUMON_EXP` :

1. **Emplacements Picking** — allées de prélèvement (ex. `PICK-001`)
2. **Poumon Expédition** — zone tampon sortie (ex. `POUMON_EXP-001`)
3. **Supports Expédition** — colis et cartons (ex. `CART-EXP-001`)
4. **Articles Préparation** — références à prélever (ex. `REF-PICK-001`)

### Formats de codes-barres

| Format | Type | Recommandé pour |
|---|---|---|
| **Code 128** | Linéaire | Codes alphanumériques, emplacements, numéros de série. Meilleur choix polyvalent. |
| **QR Code** | 2D matriciel | Données volumineuses, URLs, texte long. Lisible par smartphone. |
| **Data Matrix** | 2D matriciel | Empreinte compacte. Idéal pour petites étiquettes et marquage industriel. |

### Personnalisation par famille

Dans l'aperçu, chaque famille dispose d'un bouton **⚙ Personnaliser** permettant de définir un format et des dimensions spécifiques, indépendamment des paramètres globaux.

### Impression

1. Cliquer sur **🖨 Imprimer A4**
2. Dans la boîte de dialogue du navigateur, vérifier : format **A4**, orientation **Portrait**
3. Désactiver les en-têtes/pieds de page du navigateur pour un rendu plus propre
4. Pour enregistrer en PDF, sélectionner **Enregistrer en PDF** comme imprimante

### Technologies

| Bibliothèque | Version | Usage |
|---|---|---|
| [JsBarcode](https://github.com/lindell/JsBarcode) | 3.11.6 | Génération Code 128 (SVG) |
| [bwip-js](https://github.com/metafloor/bwip-js) | 4.5.1 | Génération QR Code et Data Matrix (Canvas) |

Aucun framework JavaScript, aucune dépendance npm. Le fichier est entièrement autonome.

---

## 🇬🇧 English

### Overview

**SCANERATOR** is a standalone barcode generation tool designed for WMS (Warehouse Management System) environments. It is a single HTML file — no installation required — that opens directly in any web browser.

### Features

- **3 barcode formats**: Code 128 (linear), QR Code, Data Matrix
- **Family organisation**: group barcodes by category (Location, Carrier, Item, Serial number…)
- **Per-family settings**: format and dimensions customisable per family
- **Random code generator**: inject auto-generated codes into any existing family
- **2 preconfigured WMS scenarios**:
  - 📥 **Reception**: auto-generates `QUAI_REC`, `POUMON_REC` locations, carriers and items
  - 📤 **Picking**: auto-generates pick locations, `POUMON_EXP`, shipping carriers and items
- **A4 printing**: optimised layout grouped by family, print-ready
- **Bilingual FR / EN**: full interface switches in one click
- **Responsive**: works on desktop, tablet and mobile
- **Zero installation**: single `.html` file, works offline after first load

### Requirements

- A modern web browser: **Chrome 80+**, **Edge 80+** or **Firefox 75+**
- An internet connection on **first launch only** (to load JsBarcode and bwip-js from CDN — cached afterwards)

### Getting started

Use the deployed version directly:
👉 **[Open SCANERATOR](https://libertyben-code.github.io/scanerator/)**

### List format

The main input area accepts a plain list, one item per line:

```
# FamilyName
barcode001
barcode002

# AnotherFamily
barcode010
barcode011
```

Lines starting with `#` define a **family**. All other non-empty lines are **barcode values**.

### Default WMS families

| Family | Example format | Use |
|---|---|---|
| Location | `ALL-01-A-001` | Aisle-level-column-position addressing |
| Carrier | `PAL-2024-001` | Pallet and bin identification |
| Item | `REF-VISSERIE-M6` | Item reference with description |
| Serial number | `SN-2024-00142` | Unit traceability with year |

### Reception scenario

The **Reception** tab (orange) auto-generates 4 families for the `QUAI_REC → POUMON_REC` flow:

1. **Dock Locations** — unloading positions (e.g. `QUAI_REC-001`)
2. **Reception Buffer Locations** — buffer before put-away (e.g. `POUMON_REC-001`)
3. **Reception Carriers** — inbound pallets and bins (e.g. `PAL-REC-001`)
4. **Reception Items** — items to be received (e.g. `REF-REC-001`)

Configurable: prefixes, number of docks, number of buffers, number of carriers, number of items.

### Picking scenario

The **Picking** tab (purple) auto-generates 4 families for the `Stock → Picking → POUMON_EXP` flow:

1. **Pick Locations** — pick aisles (e.g. `PICK-001`)
2. **Shipping Buffer** — outbound staging area (e.g. `POUMON_EXP-001`)
3. **Shipping Carriers** — parcels and cartons (e.g. `CART-EXP-001`)
4. **Picking Items** — items to be picked (e.g. `REF-PICK-001`)

### Barcode formats

| Format | Type | Recommended for |
|---|---|---|
| **Code 128** | Linear | Alphanumeric codes, locations, serial numbers. Best general-purpose choice. |
| **QR Code** | 2D matrix | Large payloads, URLs, long text. Scannable by any smartphone. |
| **Data Matrix** | 2D matrix | Compact footprint. Ideal for small labels and industrial marking. |

### Per-family customisation

In the preview panel, each family has a **⚙ Customise** button allowing format and dimension overrides independent of the global settings.

### Printing

1. Click **🖨 Print A4**
2. In the browser print dialog, confirm: format **A4**, orientation **Portrait**
3. Disable browser headers/footers for a cleaner result
4. To save as PDF, select **Save as PDF** as the printer

### Tech stack

| Library | Version | Purpose |
|---|---|---|
| [JsBarcode](https://github.com/lindell/JsBarcode) | 3.11.6 | Code 128 generation (SVG) |
| [bwip-js](https://github.com/metafloor/bwip-js) | 4.5.1 | QR Code and Data Matrix generation (Canvas) |

No JavaScript framework, no npm dependencies. The file is fully self-contained.

---

## Fichiers / Files

```
scanerator/
├── barcode-generator.html   # Application principale / Main application
├── README.md                # Ce fichier / This file
└── scanerator-poster.html   # Poster publicitaire / Advertising poster (optional)
```

---

## Compatibilité / Compatibility

| | Chrome | Edge | Firefox | Safari |
|---|---|---|---|---|
| Desktop | ✅ 80+ | ✅ 80+ | ✅ 75+ | ✅ 14+ |
| Tablet | ✅ | ✅ | ✅ | ✅ |
| Mobile | ✅ | ✅ | ✅ | ✅ |

---

*Vibe coded with ❤️