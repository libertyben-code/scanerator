# Contributing to SCANERATOR

---

## 🇫🇷 Français

### Comment ça fonctionne

SCANERATOR est un **fichier HTML unique** (`index.html`). Il ne requiert aucun outil de compilation, aucun gestionnaire de paquets, aucun serveur. Pour lancer l'application, il suffit d'ouvrir le fichier dans un navigateur.

Le code JavaScript est entièrement contenu dans un IIFE (Immediately Invoked Function Expression) à la fin du fichier. La structure interne est la suivante :

| Partie | Description |
|---|---|
| `i18n` | Objet contenant toutes les chaînes traduites (`fr`, `en`, `es`) |
| `setLang` / `applyTranslations` | Bascule de langue et mise à jour du DOM |
| Scénarios (Réception / Picking) | Générateurs de listes WMS préconfigurées |
| `generate()` | Parse la zone de texte et rend l'aperçu + la zone d'impression |
| `makeBarcode()` | Délègue au bon renderer (JsBarcode pour Code 128, bwip-js pour QR/DataMatrix) |
| Générateur aléatoire/séquentiel | Mode Aléatoire ou Séquentiel avec préfixe/suffixe |
| Overrides par famille | Paramètres de rendu spécifiques à chaque famille |

### Ajouter une chaîne traduite

1. Ajouter la clé dans les trois objets `i18n.fr`, `i18n.en` et `i18n.es`
2. Ajouter l'attribut `data-i18n="maCle"` sur l'élément HTML correspondant
3. Les placeholders d'inputs sont gérés manuellement dans `applyTranslations()`

### Ajouter une nouvelle langue

1. Ajouter un nouvel objet `i18n.xx` (copier `i18n.en` et traduire)
2. Ajouter un bouton dans le groupe `.lang-toggle` : `<button id="lang-xx" onclick="setLang('xx')">XX</button>`
3. Ajouter le `.classList.toggle` correspondant dans `setLang()`

### Ajouter un nouveau format de code-barres

1. Ajouter l'option radio dans la section "Format & dimensions" du HTML
2. Gérer le nouveau `bcid` dans la fonction `makeBarcode()` (branche `else` via `bwipjs.toCanvas`)

### Processus de contribution

1. Forker le dépôt
2. Créer une branche : `git checkout -b feature/ma-fonctionnalite`
3. Modifier `index.html` (et les docs si nécessaire)
4. Tester dans Chrome/Edge/Firefox : génération, impression, bascule de langue
5. Ouvrir une Pull Request vers `main` avec une description claire

---

## 🇬🇧 English

### How it works

SCANERATOR is a **single HTML file** (`index.html`). It requires no build tool, no package manager, no server. To run the app, simply open the file in a browser.

All JavaScript is contained in an IIFE (Immediately Invoked Function Expression) at the end of the file. The internal structure is:

| Part | Description |
|---|---|
| `i18n` | Object holding all translated strings (`fr`, `en`, `es`) |
| `setLang` / `applyTranslations` | Language switching and DOM update |
| Scenarios (Reception / Picking) | Preconfigured WMS list generators |
| `generate()` | Parses the textarea and renders the preview + print area |
| `makeBarcode()` | Delegates to the right renderer (JsBarcode for Code 128, bwip-js for QR/DataMatrix) |
| Random / sequential generator | Random or Sequential mode with prefix/suffix |
| Per-family overrides | Per-family rendering settings |

### Adding a translated string

1. Add the key to all three objects `i18n.fr`, `i18n.en` and `i18n.es`
2. Add `data-i18n="myKey"` to the corresponding HTML element
3. Input placeholders are set manually in `applyTranslations()`

### Adding a new language

1. Add a new `i18n.xx` object (copy `i18n.en` and translate)
2. Add a button in the `.lang-toggle` group: `<button id="lang-xx" onclick="setLang('xx')">XX</button>`
3. Add the matching `.classList.toggle` inside `setLang()`

### Adding a new barcode format

1. Add the radio option in the "Format & dimensions" section of the HTML
2. Handle the new `bcid` in the `makeBarcode()` function (the `else` branch via `bwipjs.toCanvas`)

### Contribution process

1. Fork the repository
2. Create a branch: `git checkout -b feature/my-feature`
3. Edit `index.html` (and docs if needed)
4. Test in Chrome/Edge/Firefox: generation, printing, language switching
5. Open a Pull Request to `main` with a clear description

---

## 🇪🇸 Español

### Cómo funciona

SCANERATOR es un **único archivo HTML** (`index.html`). No requiere ninguna herramienta de compilación, ningún gestor de paquetes, ningún servidor. Para ejecutar la aplicación, basta con abrir el archivo en un navegador.

Todo el JavaScript está contenido en un IIFE (Immediately Invoked Function Expression) al final del archivo. La estructura interna es la siguiente:

| Parte | Descripción |
|---|---|
| `i18n` | Objeto con todas las cadenas traducidas (`fr`, `en`, `es`) |
| `setLang` / `applyTranslations` | Cambio de idioma y actualización del DOM |
| Escenarios (Recepción / Preparación) | Generadores de listas WMS preconfigurados |
| `generate()` | Analiza el área de texto y renderiza la vista previa + zona de impresión |
| `makeBarcode()` | Delega al renderizador correcto (JsBarcode para Code 128, bwip-js para QR/DataMatrix) |
| Generador aleatorio/secuencial | Modo Aleatorio o Secuencial con prefijo/sufijo |
| Overrides por familia | Ajustes de renderizado específicos por familia |

### Añadir una cadena traducida

1. Añadir la clave en los tres objetos `i18n.fr`, `i18n.en` e `i18n.es`
2. Añadir el atributo `data-i18n="miClave"` en el elemento HTML correspondiente
3. Los placeholders de inputs se gestionan manualmente en `applyTranslations()`

### Añadir un nuevo idioma

1. Añadir un nuevo objeto `i18n.xx` (copiar `i18n.en` y traducir)
2. Añadir un botón en el grupo `.lang-toggle`: `<button id="lang-xx" onclick="setLang('xx')">XX</button>`
3. Añadir el `.classList.toggle` correspondiente en `setLang()`

### Añadir un nuevo formato de código de barras

1. Añadir la opción radio en la sección "Formato y dimensiones" del HTML
2. Gestionar el nuevo `bcid` en la función `makeBarcode()` (rama `else` mediante `bwipjs.toCanvas`)

### Proceso de contribución

1. Hacer un fork del repositorio
2. Crear una rama: `git checkout -b feature/mi-funcionalidad`
3. Modificar `index.html` (y la documentación si es necesario)
4. Probar en Chrome/Edge/Firefox: generación, impresión, cambio de idioma
5. Abrir una Pull Request hacia `main` con una descripción clara
