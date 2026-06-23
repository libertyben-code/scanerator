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
- **Génération séquentielle** : générez des codes avec un compteur incrémental (ex. `ABCD001`, `ABCD002`…)
- **Préfixe et suffixe** : encadrez chaque code généré avec du texte fixe
- **2 scénarios WMS préconfigurés** :
  - 📥 **Réception** : génère automatiquement les emplacements `QUAI_REC`, `POUMON_REC`, supports et articles
  - 📤 **Préparation** : génère les emplacements de picking, `POUMON_EXP`, supports et articles
- **Impression A4** : mise en page optimisée, regroupée par famille, prête à imprimer
- **Trilingue FR / EN / ES** : toute l'interface bascule en un clic
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

### Génération aléatoire et séquentielle

Le générateur de codes dispose de deux modes :

- **Aléatoire** : génère N codes de longueur L à partir du jeu de caractères sélectionné (A–Z, 0–9, a–z, –)
- **Séquentiel** : génère N codes composés de L caractères aléatoires suivis d'un compteur zero-paddé (ex. `ABC001`, `DEF002`…)

Dans les deux modes, un **Préfixe** et un **Suffixe** optionnels encadrent chaque code.

### Formats de codes-barres

| Format | Type | Recommandé pour |
|---|---|---|
| **Code 128** | Linéaire | Codes alphanumériques, emplacements, numéros de série. Meilleur choix polyvalent. |
| **QR Code** | 2D matriciel | Données volumineuses, URLs, texte long. Lisible par smartphone. |
| **Data Matrix** | 2D matriciel | Empreinte compacte. Idéal pour petites étiquettes et marquage industriel. |

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
- **Sequential generation**: generate codes with an incremental counter (e.g. `ABCD001`, `ABCD002`…)
- **Prefix and suffix**: wrap every generated code with fixed text
- **2 preconfigured WMS scenarios**:
  - 📥 **Reception**: auto-generates `QUAI_REC`, `POUMON_REC` locations, carriers and items
  - 📤 **Picking**: auto-generates pick locations, `POUMON_EXP`, shipping carriers and items
- **A4 printing**: optimised layout grouped by family, print-ready
- **Trilingual FR / EN / ES**: full interface switches in one click
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

### Random and sequential generation

The code generator has two modes:

- **Random**: generates N codes of length L drawn from the selected charset (A–Z, 0–9, a–z, –)
- **Sequential**: generates N codes made of L random characters followed by a zero-padded counter (e.g. `ABC001`, `DEF002`…)

In both modes, an optional **Prefix** and **Suffix** wrap each generated code.

### Barcode formats

| Format | Type | Recommended for |
|---|---|---|
| **Code 128** | Linear | Alphanumeric codes, locations, serial numbers. Best general-purpose choice. |
| **QR Code** | 2D matrix | Large payloads, URLs, long text. Scannable by any smartphone. |
| **Data Matrix** | 2D matrix | Compact footprint. Ideal for small labels and industrial marking. |

### Tech stack

| Library | Version | Purpose |
|---|---|---|
| [JsBarcode](https://github.com/lindell/JsBarcode) | 3.11.6 | Code 128 generation (SVG) |
| [bwip-js](https://github.com/metafloor/bwip-js) | 4.5.1 | QR Code and Data Matrix generation (Canvas) |

No JavaScript framework, no npm dependencies. The file is fully self-contained.

---

## 🇪🇸 Español

### Presentación

**SCANERATOR** es una herramienta autónoma de generación de códigos de barras diseñada para entornos WMS (Sistema de Gestión de Almacenes). Se trata de un único archivo HTML, sin instalación, que se abre directamente en cualquier navegador web.

### Funcionalidades

- **3 formatos de código de barras**: Code 128 (lineal), QR Code, Data Matrix
- **Organización por familias**: agrupe sus códigos por categoría (Ubicación, Soporte, Artículo, Número de serie…)
- **Ajustes por familia**: formato y dimensiones personalizables por familia
- **Generador aleatorio**: inyecte códigos generados automáticamente en cualquier familia existente
- **Generación secuencial**: genere códigos con un contador incremental (ej. `ABCD001`, `ABCD002`…)
- **Prefijo y sufijo**: encuadre cada código generado con texto fijo
- **2 escenarios WMS preconfigurados**:
  - 📥 **Recepción**: genera automáticamente ubicaciones `QUAI_REC`, `POUMON_REC`, soportes y artículos
  - 📤 **Preparación**: genera ubicaciones de picking, `POUMON_EXP`, soportes de expedición y artículos
- **Impresión A4**: maquetación optimizada, agrupada por familia, lista para imprimir
- **Trilingüe FR / EN / ES**: toda la interfaz cambia en un clic
- **Responsive**: utilizable en escritorio, tableta y móvil
- **Sin instalación**: un único archivo `.html`, funciona sin conexión tras la primera carga

### Requisitos

- Un navegador web moderno: **Chrome 80+**, **Edge 80+** o **Firefox 75+**
- Conexión a internet únicamente en el **primer inicio** (para cargar las bibliotecas JsBarcode y bwip-js desde un CDN — se almacenan en caché después)

### Inicio

Use directamente la versión desplegada:
👉 **[Abrir SCANERATOR](https://libertyben-code.github.io/scanerator/)**

### Formato de la lista

La zona de entrada principal acepta una lista simple, un elemento por línea:

```
# NombreFamilia
codigo001
codigo002

# OtraFamilia
codigo010
codigo011
```

Las líneas que comienzan por `#` definen una **familia**. Todas las demás líneas no vacías son **valores de código de barras**.

### Generación aleatoria y secuencial

El generador de códigos dispone de dos modos:

- **Aleatorio**: genera N códigos de longitud L extraídos del juego de caracteres seleccionado (A–Z, 0–9, a–z, –)
- **Secuencial**: genera N códigos compuestos de L caracteres aleatorios seguidos de un contador con ceros a la izquierda (ej. `ABC001`, `DEF002`…)

En ambos modos, un **Prefijo** y un **Sufijo** opcionales encuadran cada código generado.

### Formatos de código de barras

| Formato | Tipo | Recomendado para |
|---|---|---|
| **Code 128** | Lineal | Códigos alfanuméricos, ubicaciones, números de serie. Mejor opción polivalente. |
| **QR Code** | Matricial 2D | Datos voluminosos, URLs, texto largo. Legible con cualquier smartphone. |
| **Data Matrix** | Matricial 2D | Huella compacta. Ideal para etiquetas pequeñas y marcado industrial. |

### Tecnologías

| Biblioteca | Versión | Uso |
|---|---|---|
| [JsBarcode](https://github.com/lindell/JsBarcode) | 3.11.6 | Generación Code 128 (SVG) |
| [bwip-js](https://github.com/metafloor/bwip-js) | 4.5.1 | Generación QR Code y Data Matrix (Canvas) |

Sin framework JavaScript, sin dependencias npm. El archivo es completamente autónomo.

---

## Fichiers / Files / Archivos

```
scanerator/
├── index.html        # Application / App / Aplicación
├── README.md         # Ce fichier / This file / Este archivo
└── docs/             # Documentation développeur / Developer docs / Documentación para desarrolladores
```

---

## Compatibilité / Compatibility / Compatibilidad

| | Chrome | Edge | Firefox | Safari |
|---|---|---|---|---|
| Desktop | ✅ 80+ | ✅ 80+ | ✅ 75+ | ✅ 14+ |
| Tablet | ✅ | ✅ | ✅ | ✅ |
| Mobile | ✅ | ✅ | ✅ | ✅ |

---

*Vibe coded with ❤️*
