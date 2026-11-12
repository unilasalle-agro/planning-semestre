# Gantt de semestre

Page web statique : on dépose le planning d'un semestre (.xlsx ou .csv), elle affiche un Gantt par module, regroupé par UE, avec les examens en rouge et les remises de rapports en losanges.
Tout se passe dans le navigateur : le fichier n'est envoyé nulle part.

## Format du fichier attendu

Une ligne par créneau, avec ces colonnes (l'ordre n'a pas d'importance, les accents et la casse non plus) :

| Colonne | Obligatoire | Remarque |
|---|---|---|
| Date | oui | `jj/mm/aaaa`, `aaaa-mm-jj` ou vraie date Excel |
| Module | oui | |
| groupe | non | champ de regroupement par défaut (voir ci-dessous) |
| autres colonnes | non | UE, type d'activité, etc. : disponibles dans « Regrouper par » |
| Début, Fin, Durée en minutes | non | |
| Type d'activité | non | CM, TD, TP, AF, EXT, EXAMEN_ECRIT, EXAMEN_ORAL… |
| Libellé | non | utilisé pour les remises |

- Les types qui commencent par `EXAMEN` sont comptés comme examens. D'autres types (DS, partiel…) se cochent dans la page.
- Une ligne de type `REMISE` (ou `RAPPORT`) est une remise de rapport. Elle peut n'avoir que la date, le module et le libellé.
- CSV : séparateur `;`, `,` ou tabulation détecté automatiquement, UTF-8 ou Windows-1252.
- Classeur à plusieurs feuilles : un menu permet de choisir la feuille qui contient le tableau.

## Regroupement des modules

Par défaut, les modules sont regroupés selon la colonne nommée `groupe` (ou `Groupe`, `groupes`). Les menus « Regrouper par » et « puis par » permettent de choisir n'importe quelle autre colonne du fichier (UE, type d'activité…), sur un ou deux niveaux. Le choix est mémorisé dans le navigateur.

- **Colonne absente :** les modules ne sont pas regroupés (un seul bloc « Tous les modules ») et la page l'indique. Le regroupement se choisit alors à la main.
- **Valeur vide :** une ligne sans valeur reprend la valeur la plus fréquente de son module. Un module sans aucune valeur va dans un bloc « Sans groupe » (placé en dernier) et la page le signale.
- **Valeurs différentes pour un même module :** le module apparaît dans chacun des groupes concernés, avec seulement les séances de ce groupe, et la page le signale.
- Les lignes `REMISE` saisies à la main n'ont pas besoin de valeur de groupe : elles suivent leur module.

## Titre

Le champ « Titre » de la barre du haut définit le titre affiché au-dessus du Gantt, repris dans l'image exportée et dans le PDF. Vide, le titre est « Gantt de semestre ». Il est mémorisé dans le navigateur pour chaque nom de fichier.

## Remises de rapports

Le formulaire de la page ajoute une remise (module, date, libellé). Elle est mémorisée dans le navigateur (`localStorage`), sur cet ordinateur seulement.
Le bouton « Télécharger le planning avec les remises » produit un .xlsx qui contient ces remises sous forme de lignes `REMISE`. Le redéposer les remet à l'écran, sur n'importe quel ordinateur.

## Mettre en ligne avec GitHub Pages

1. Créer un dépôt public sur GitHub et y envoyer tout le contenu de ce dossier (`index.html`, `vendor/`, `exemple/`, `README.md`).
2. Dans le dépôt : Settings → Pages → Source : « Deploy from a branch », branche `main`, dossier `/ (root)`.
3. Après une minute ou deux, la page est disponible à `https://<compte>.github.io/<dépôt>/`.

Pour tester en local, ouvrir `index.html` suffit : tout fonctionne hors ligne, seules les polices Google ne se chargent pas (une police système les remplace).

## Exporter

Le menu « Exporter » propose :
- **Image PNG** ou **JPEG** : le Gantt seul, avec titre, légende et date d'édition, sur fond blanc quel que soit le thème de la page.
- **PDF** : ouvre l'impression du navigateur. Une mise en page A4 paysage est prévue (Gantt réduit à la largeur de la page, sans formulaire ni boutons). Choisir « Enregistrer au format PDF » comme imprimante.
- **Planning avec les remises (.xlsx)**.

## Contenu

- `index.html` : toute l'application (HTML, CSS, JavaScript).
- `vendor/xlsx.full.min.js` : SheetJS 0.18.5 (lecture et écriture des .xlsx), licence Apache 2.0.
- `vendor/html2canvas.min.js` : html2canvas 1.4.1 (export en image), licence MIT.
- `exemple/Planning_S9_AGROECO.xlsx` : fichier d'essai.

Les deux bibliothèques sont copiées dans le dépôt : la page ne dépend d'aucun CDN.
