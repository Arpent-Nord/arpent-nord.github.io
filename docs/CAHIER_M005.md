# Cahier de l'application — M-005 Documentation du contrat

Page **`/documentation/`** de la boîte à outils. Version 0.1 (2026-11-12) : étape 1 du cahier des charges M-005 v0.5 (onglets **Consulter** et **Liste de consultation**).
Référence : cahier des charges M-005 v0.5, NDA-001 V03 (§6, §11, §17, §19). En cas de conflit, NDA-001 prime.

## 1. À quoi sert le module
Montrer les documents **en vigueur** (VV) d'un contrat et tenir à jour la **liste de consultation** que les arpenteurs ouvrent au téléphone par **un seul lien SharePoint fixe**. Les nouveautés et les remplacements sont mis en évidence en tête de liste. Utilisateurs : responsable des registres du contrat (calculateur, chef arpenteur ou arpenteur selon la taille du contrat). Ordinateur, Chrome ou Edge. Sur le téléphone, la page explique seulement comment ouvrir la liste.

## 2. Utilisation pas à pas
1. Boîte à outils → **Documentation du contrat**.
2. **Entité** : ARPENT-NORD ou MECATECH (case « Mode rodage »). Le chemin imposé vient de `reference.json`. Le dossier connecté est **le même que celui de la Réception des documents (M-003)** : déjà connecté dans l'Administration, il est reconnu ici sans rien refaire.
3. **Contrat** et **Votre nom**.
4. **Consulter** : liste des documents en vigueur, le plus récent en premier, filtres Catégorie / Discipline / Format / Changement et recherche. Boutons Ouvrir, Chemin, SharePoint. Un bandeau signale les changements **pas encore publiés** dans la liste (repère `*`).
5. **Liste de consultation** → **Mettre à jour la liste de consultation**. Rien n'est automatique : on clique après un classement, un remplacement ou une repromotion.
6. La première fois : copier le **lien fixe** (ou le message Teams prêt à coller) et l'envoyer une seule fois aux arpenteurs. QR et fiche imprimable pour la roulotte au besoin.

## 3. Sources des documents en vigueur
| Catégorie | Source | Règle |
|---|---|---|
| PLAN (par discipline) | `08_PARTAGE\REGISTRE_DOCUMENTS.csv` (M-003) | Type « Plan PDF » ou « Plan DWG/DXF », Statut Classé, VV |
| DMT | registre | Type DMT, Classé, VV |
| CORRESPONDANCE | registre | Type Courriel, Classé, VV (ou document divers dont le nom contient LETTRE, AVIS, CORRESPOND) |
| DEVIS / RAPPORT / DOCUMENT | registre | Type « Document divers », Classé, VV ; DEVIS ou RAPPORT si le nom ou l'objet le contient |
| IMPLANTATION | dossier `02_IMPLANTATION\02_FICHIERS` | tous les fichiers (géré par M-006, lu ici) |
| STATIONS | dossier `02_IMPLANTATION\01_STATIONS` | tous les fichiers |

Non listés : photos, modèles 3D, fichiers de soutien, zip ; sous-dossiers commençant par `_` ; fichiers temporaires (`~$…`). Un document inscrit au registre mais introuvable est signalé (« À vérifier ») et n'entre pas dans la liste.

## 4. Fichiers écrits (dans le dossier du contrat)
| Élément | Emplacement |
|---|---|
| Liste de consultation | `08_PARTAGE\M005_<contrat>_CONSULTATION.xlsx` (réécrite sous le même nom à chaque mise à jour) |
| État de la liste | `08_PARTAGE\M005_<contrat>_CONSULTATION.json` (documents publiés, fichiers déjà vus, historique des changements) |
| Journal | `08_PARTAGE\REGISTRE_DOCUMENTS_JOURNAL.csv` (commun avec M-003, action « M-005 Liste de consultation ») |

Le registre M-003 n'est **jamais modifié** par cette version.

### Fichier Excel
- Feuille **En vigueur** : `Changement`, `Categorie`, `Discipline`, `Format`, `Document` (lien vers SharePoint), `Revision`, `Date`, `Detail_changement`, `Dossier`. Tri : le plus récent en premier. Filtres sur la ligne d'en-tête, en-têtes figés.
- Feuille **Changements** : changements des 14 derniers jours, le plus récent en premier.
- Repères : 🟢 NOUVEAU · 🔵 REMPLACE · 🟠 REPROMU · ⚪ RETIRÉ (fond de couleur).
- Écrit par l'outil lui-même (format Office ouvert), sans bibliothèque externe.

### Calcul des changements
Comparaison avec la dernière liste publiée (`.json`) :
- **REMPLACE** : un document disparu et un nouveau ont la même clé (même dossier, même nom sans révision : `_REV_B`, `-R3`, ` rev C`, `_02`…). Aussi : même fichier dont le contenu a changé.
- **REPROMU** : le document qui revient avait déjà été dans une liste publiée.
- **NOUVEAU** : sans prédécesseur. **RETIRÉ** : disparu sans remplaçant.
- La première mise à jour crée la liste initiale, sans repères.

## 5. Paramètres (`01_TRAITEMENT\_SYSTEME\reference.json`)
| Clé | Rôle |
|---|---|
| `chemins` (AN, MA, AN_RODAGE, MA_RODAGE) | chemin imposé du dossier des contrats (existant) |
| `sharepoint` (AN, MA) | adresse SharePoint du dossier **parent** du dossier des contrats (existant, même règle que M-003). **Nouveau** : `AN_RODAGE`, `MA_RODAGE` facultatifs pour tester le lien au téléphone pendant le rodage |
| `responsables_registres` | **nouveau**, facultatif : `{"AN-26-005": "Prénom Nom"}`. Affiché ; avertit si un autre nom met la liste à jour |
| `formats_consultation` | **nouveau**, facultatif : `{".ttm": "MNT", ".xml": "MNT"}` complète la table extension → format |

Format par défaut : PDF, DWG, DXF, TXT, CSV, RXL ; **MNT** = `.ttm`, `.dtm`, `.tin`, `.tn3`, `.xml`, `.landxml`.
Adresse d'un fichier : `<sharepoint>/<dossier des contrats>/<contrat>/<chemin>` (encodée), `?web=1` pour les fichiers Office.

## 6. Installation / mise en ligne
Dépôt `arpent-nord.github.io` : téléverser `documentation/index.html` (nouveau dossier) et `index.html` (tuile ajoutée à l'accueil et ligne « Quel outil prendre ? »). Add file → Upload files, Commit, Ctrl+F5.
⚠ L'`index.html` fourni part de la copie du 2026-11-09 (Renommage_applications\Etape_C). Si l'accueil a changé depuis, n'ajoutez que la carte `t-doc` et la ligne de « Quel outil prendre ? » (voir le README de la livraison).

## 7. À valider au rodage (AN-26-005)
1. Le lien fixe ouvre la liste dans Excel au téléphone et à la tablette ; les filtres fonctionnent en lecture.
2. Les liens des documents ouvrent PDF et DWG dans SharePoint/OneDrive ; adresses avec espaces et accents.
3. L'adresse reste la même après une mise à jour (remplacement du fichier par la synchronisation OneDrive) ; historique des versions SharePoint conservé ou non.
4. Le fichier ouvert dans Excel sur l'ordinateur bloque l'écriture : message clair, rien de changé.
5. Classement des DEVIS / RAPPORT par mot-clé suffisant, ou ajout de types dans M-003.

## 8. Limites connues
- La mise à jour se fait ici, par un clic : la Réception (M-003) ne la propose pas encore (étape 2 : déplacement de M-003 dans la tuile).
- La copie gelée des plans dans `10_ARCHIVES` (cahier §7.2 bis) n'est pas encore utilisée : la liste pointe la `Destination` du registre. Elle le sera avec l'onglet Archivage.
- Les « changements depuis votre dernière visite » ne sont pas possibles : la liste est la même pour tous (14 derniers jours).

## 9. Journal des versions
| Version | Date | Changement |
|---|---|---|
| M-005 v0.1 | 2026-11-12 | Première version : Consulter (filtres, le plus récent en premier, changements en attente) ; liste de consultation Excel à lien fixe (En vigueur + Changements, repères en couleur) ; lien, message Teams, QR et fiche ; tuile à l'accueil. |
