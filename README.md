# RétroCalc

Calculatrice scientifique au look rétro, avec écran LCD vert, clavier type Casio, mémoire, historique et fonctionnement hors-ligne.

![Aperçu](assets/icons/icon-512.png)

## Sommaire

- [Lancer l'application](#lancer-lapplication)
- [Fonctionnalités](#fonctionnalités)
- [Utilisation](#utilisation)
- [Raccourcis clavier](#raccourcis-clavier)
- [Limites et messages d'erreur](#limites-et-messages-derreur)
- [Structure du projet](#structure-du-projet)
- [Tests](#tests)
- [Personnalisation](#personnalisation)
- [Vie privée](#vie-privée)
- [Polices et licences](#polices-et-licences)

## Lancer l'application

RétroCalc est une application web sans dépendance ni étape de compilation. Elle utilise toutefois des modules JavaScript et un service worker : il faut donc la servir avec un petit serveur local (un simple double-clic sur `index.html` ne suffit pas).

### Windows — méthode la plus simple

Double-cliquez sur **`demarrer.bat`**. Le script démarre un serveur Python sur le port 8080 et ouvre votre navigateur sur <http://localhost:8080>.

### Avec Python

```powershell
cd C:\Users\hp\Desktop\FORMATION_IA_CODE
python -m http.server 8080
```

Puis ouvrez <http://localhost:8080> dans Chrome, Edge ou Firefox.

### Avec VS Code

Installez l'extension **Live Server**, faites un clic droit sur `index.html` puis **Open with Live Server**.

### Avec Node.js

```powershell
npx serve .
```

## Fonctionnalités

- Opérations de base : addition, soustraction, multiplication, division, puissances, parenthèses.
- Trigonométrie : `sin`, `cos`, `tan` et fonctions inverses via la touche **INV**.
- Logarithmes : `ln` (népérien) et `log` (décimal), plus `eˣ` et `10ˣ` via INV.
- Racines carrée et cubique, carré, cube, inverse `1/x`, factorielle `n!`, pourcentage.
- Constantes `π` et `e`, constante `ANS` (dernier résultat).
- Modes d'angle **DEG / RAD / GRAD**.
- Résultats décimaux avec 0 à 12 décimales réglables, notation scientifique automatique.
- Mémoire `M+`, `M−`, `MR`, `MC` conservée entre les sessions.
- Historique de 50 calculs, visible sur l'écran LCD et dans un panneau latéral.
- Interface entièrement en français, responsive, utilisable au doigt, à la souris ou au clavier.
- Sons synthétisés et animations, désactivables.
- Installation PWA et utilisation hors-ligne après la première visite.

## Utilisation

### Saisie

- Les touches du clavier virtuel ajoutent leur contenu à l'expression.
- Le clavier physique fonctionne aussi : chiffres, opérateurs, parenthèses, lettres des fonctions.
- `Entrée` ou `=` calcule. `Retour arrière` efface un caractère. `Échap` efface tout.
- Les parenthèses ouvertes sont fermées automatiquement au moment du calcul : `sin(30` devient `sin(30)`.

### Touche INV

**INV** active les fonctions secondaires pour la prochaine touche :

| Touche | Normale | Avec INV |
|---|---|---|
| sin | `sin` | `sin⁻¹` |
| cos | `cos` | `cos⁻¹` |
| tan | `tan` | `tan⁻¹` |
| ln | `ln` | `eˣ` |
| log | `log` | `10ˣ` |
| √ | racine carrée | racine cubique |
| x² | carré | cube |
| π | `π` | `e` |

Après utilisation, INV se désactive automatiquement, comme sur une calculatrice Casio.

### Mémoire et ANS

- `M+` ajoute la valeur affichée à la mémoire, `M−` la soustrait.
- `MR` rappelle la mémoire, `MC` l'efface.
- Le badge **M** apparaît sur l'écran lorsque la mémoire contient une valeur non nulle.
- `ANS` réinsère le dernier résultat calculé.

### Historique

- Les trois derniers calculs apparaissent en petit sur l'écran LCD.
- Le panneau de droite conserve les 50 derniers calculs.
- Cliquez sur une expression pour la réutiliser, ou sur son résultat pour réinsérer la valeur.
- Le bouton **Vider** efface l'historique.

### Réglages

- **Décimales** : de 0 à 12 chiffres après la virgule.
- **Son** : active ou coupe les clics synthétisés.
- **DEG / RAD / GRAD** : la touche `DEG` du clavier fait défiler les trois modes. Si un résultat est affiché, il est recalculé dans le nouveau mode.

### Pourcentages

- `50%` vaut `0,5`.
- `200 + 10%` vaut `220` (pourcentage relatif, comportement type Casio).
- `200 − 10%` vaut `180`.
- `200 × 10%` vaut `20`.

## Raccourcis clavier

| Touche | Action |
|---|---|
| `0` à `9`, `.`, `,` | Chiffres et virgule décimale |
| `+`, `-`, `*`, `/`, `^` | Opérateurs |
| `(`, `)` | Parenthèses |
| `%`, `!` | Pourcentage, factorielle |
| `Entrée` ou `=` | Calculer |
| `Retour arrière` | Effacer un caractère |
| `Échap` ou `Suppr` | Tout effacer |
| Lettres | Saisie directe des fonctions (`sin`, `cos`, `ln`, `sqrt`, `pi`, `ans`...) |

La virgule et le point sont acceptés à la saisie ; l'affichage utilise la virgule française.

## Limites et messages d'erreur

RétroCalc affiche un message explicite plutôt qu'un simple « Erreur » :

- division par zéro ;
- tangente non définie (par exemple `tan(90)` en degrés) ;
- racine carrée d'un nombre négatif ;
- logarithme d'un nombre nul ou négatif ;
- argument hors du domaine des fonctions inverses ;
- factorielle réservée aux entiers de 0 à 170 ;
- résultat trop grand ou non réel ;
- fonction ou caractère inconnu.

Après une erreur, il suffit de modifier l'expression ou d'appuyer sur `C` pour continuer.

## Structure du projet

```
index.html                  Interface (écran, clavier, historique)
css/style.css               Thème LCD, responsive, animations
js/app.js                   Logique principale et assemblage
js/format.js                Formatage des nombres et expressions
js/storage.js               Accès sécurisé au localStorage
js/settings.js              Réglages persistants
js/history.js               Historique et rendu du panneau
js/keypad.js                Clavier virtuel et clavier physique
js/engine/tokenizer.js      Analyse lexicale
js/engine/parser.js         Analyse syntaxique (priorités, implicite)
js/engine/evaluator.js      Évaluation, trigonométrie, erreurs
manifest.webmanifest        Manifeste PWA
sw.js                       Service worker hors-ligne
tests.html                  Tests du moteur dans le navigateur
assets/fonts/               Polices LCD et licences
assets/icons/               Icônes de l'application
demarrer.bat                Lancement local sous Windows
```

## Tests

Ouvrez <http://localhost:8080/tests.html> une fois le serveur lancé.

La page exécute une quarantaine de vérifications sur les priorités, la trigonométrie, les fonctions, les pourcentages, les erreurs et le formatage. Chaque test est affiché avec son résultat et un récapitulatif en haut de page.

## Personnalisation

- **Couleurs** : modifiez les variables CSS dans `:root` (`--lcd-bg`, `--accent`, `--shell`, etc.) au début de `css/style.css`.
- **Nombre de décimales par défaut** : `DEFAULTS.decimals` dans `js/settings.js`.
- **Taille de l'historique** : `limit` lors de la création de `History` dans `js/app.js` (50 par défaut).
- **Durée de vie du cache** : changez `CACHE_NAME` dans `sw.js` après une mise à jour pour forcer le renouvellement des fichiers hors-ligne.

## Vie privée

RétroCalc fonctionne entièrement dans le navigateur. Aucune donnée n'est envoyée à un serveur : réglages, mémoire, dernier résultat et historique sont stockés uniquement dans le `localStorage` de votre navigateur. Le bouton **Vider** efface l'historique ; les réglages peuvent être réinitialisés en supprimant les clés `retrocalc.*` du stockage local.

## Polices et licences

- **DSEG7 Classic** — police à sept segments par Keshikan, licence SIL Open Font License 1.1. Voir `assets/fonts/LICENSE-DSEG.txt`.
- **VT323** — police terminal par Peter Hull, licence SIL Open Font License 1.1. Voir `assets/fonts/OFL-VT323.txt`.

Le code de l'application est fourni tel quel pour un usage personnel et pédagogique.
