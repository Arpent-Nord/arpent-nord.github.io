# Cahier de l'application — M-003 Réception des documents

Module de la page **Administration** (`/admin/#reception`) de la boîte à outils. Version 0.1 (2026-11-11), livrée avec Administration v0.9.
Référence : cahier des charges M-003 v0.1, NDA-001 V03 (§5, §6.1, §6.7, §11, §17). En cas de conflit, NDA-001 prime.

## 1. À quoi sert le module
Recevoir les documents d'un contrat (courriels, plans PDF/DWG/DXF, zip, DMT, photos, réseaux de stations), les inscrire au **registre des documents reçus**, les classer selon la norme et signaler les révisions. Utilisateurs : chargé de projet / administration (dépôt), calculateur (dépôt, classement, registre), chef arpenteur (consultation, plans en vigueur). Ordinateur, Chrome ou Edge.

## 2. Utilisation pas à pas
1. Administration → onglet **Réception des documents**.
2. **Entité** : bouton ARPENT-NORD ou MECATECH (case « Mode rodage » pour 99_EN RODAGE). Le chemin imposé s'affiche (lu dans `reference.json`, non modifiable). Première fois sur un poste : « Connecter ce dossier » ; un dossier d'un autre nom est refusé.
3. **Contrat** et **Votre nom** (noté au registre).
4. **Déposer un envoi** : glisser le courriel `.eml` / `.msg` et les fichiers ou dossiers. Le `.eml` est analysé (date, expéditeur, objet, pièces jointes extraites, logos de signature ignorés). Les zip sont extraits (case « Extraire »), le zip d'origine est gardé. Les fichiers de soutien DWG vont dans `_SUPPORT`. Un fichier déjà reçu (même nom, même taille) est signalé et décoché.
5. Compléter date de réception, émetteur, objet, remarque ; répondre à la question du statut des courriels/photos/divers ; **Enregistrer la réception**.
6. **Registre** : pour chaque document « À classer », vérifier type, discipline, identifiant, révision ; cocher ; **Classer la sélection**. Les fichiers sont copiés (rien n'est écrasé ni renommé) ; le lot reste comme trace.
7. **Lots de réception** : rejeter un lot (motif), inscrire un dossier créé à la main (sans recopie), regrouper les fichiers déposés hors lot à la racine de 02_RECEPTION.

## 3. Données en entrée et en sortie (dans le dossier du contrat)
| Élément | Emplacement |
|---|---|
| Lot de réception | `08_PARTAGE\02_RECEPTION\AAMMJJ_EMETTEUR_OBJET\` (noms d'origine conservés) |
| Manifeste du lot | `…\<lot>\_reception.json` |
| Lot rejeté | `…\<lot>\_REJETE.txt` + statut dans le manifeste |
| Registre | `08_PARTAGE\REGISTRE_DOCUMENTS.csv` (`;`, UTF-8 avec BOM, ouvrable dans Excel) |
| Lignes retirées | `08_PARTAGE\REGISTRE_DOCUMENTS_RETIRES.csv` (+ Retire_par, Retire_le, Motif_retrait) |
| Journal | `08_PARTAGE\REGISTRE_DOCUMENTS_JOURNAL.csv` (Date;Utilisateur;Action;Contrat;Details) |

### Colonnes du registre
`No` (D0001…) · `Statut` (À classer / Classé / Remplacé / Rejeté) · `Statut_fichier` (VV / VC, NDA §11) · `Date_Reception` · `Emetteur` · `Objet` · `Lot_Reception` · `Fichier` (chemin dans le lot) · `Taille` · `Type` · `Discipline` (ARC, STR, CIV, GEO, MEC) · `Identifiant` (NDA §5 ou DMT-###, si déductible) · `Revision` · `Cle` (clé de comparaison des révisions) · `Remplace` · `Remplace_par` · `Destination` (chemin du fichier dans le contrat) · `Recu_par` · `Date_Depot` · `Classe_par` · `Date_Classement` · `Remarques`.

### Manifeste `_reception.json`
```json
{ "module": "M-003", "version": "0.1", "lot": "260519_MTQ_DMT-008_MODIFICATION_COTES_SEMELLES",
  "contrat": "AN-26-005 - …", "entite": "MA", "date_reception": "2026-05-19", "emetteur": "MTQ", "objet": "…",
  "remarque": "", "depose_par": "…", "depose_le": "2026-11-11 14:48:25", "statut": "Reçu",
  "inscription_sur_place": false,
  "courriels": [{ "fichier": "DMT-008.eml", "de": "…", "a": "…", "date": "…", "objet": "…" }],
  "documents": [{ "no": "D0006", "fichier": "DMT-008 Modification cotes semelles.pdf", "taille": 180, "type": "DMT",
                  "discipline": "", "identifiant": "DMT-008", "revision": "", "cle": "…", "origine": "pièce jointe de DMT-008.eml",
                  "statut": "Classé", "statut_fichier": "VV", "destination": "08_PARTAGE\\02_RECEPTION\\…", "classe_par": "…", "classe_le": "…" }],
  "rejet": { "motif": "…", "par": "…", "le": "…" } }
```
Le manifeste est la source de secours du registre : **Réindexer depuis les lots** réinscrit toute ligne présente dans un manifeste mais absente du registre (sauf si elle a été retirée volontairement).

## 4. Installation / mise en ligne
Fichier `admin/index.html` du dépôt `arpent-nord.github.io` (GitHub Pages). Publier : décompresser la livraison, Add file → Upload files dans le dossier `admin`, Commit, Ctrl+F5. Aucune bibliothèque externe ajoutée (lecture des zip par `DecompressionStream`, analyse des courriels intégrée).

## 5. Paramètres
`01_TRAITEMENT\_SYSTEME\reference.json`, clé `chemins` (saisie dans Administration → Tables de référence → « Chemins des dossiers de contrats ») : `AN`, `MA`, `AN_RODAGE`, `MA_RODAGE` (chemins Windows complets). Clé `sharepoint` (AN, MA) : si présente, le bouton « Dossier » ouvre le dossier dans SharePoint (hors rodage) ; sinon il copie le chemin Windows. Aucun secret. Aucun nom ni chemin dans le code publié.

## 6. Règles métier
- Destinations proposées (cahier §5) : Plan PDF → `03_PLANS\0X_<DISC>\01_PDF` ; Plan DWG/DXF → `03_PLANS\0X_<DISC>\02_DWG` ; Fichier d'implantation → `02_IMPLANTATION\00_A_INTEGRER` ; Réseau de stations → `02_IMPLANTATION\01_STATIONS` ; DMT, courriel, photo, modèle 3D, divers, soutien, zip → restent dans le lot.
- Révision : même `Cle` (nom sans suffixe `_R4`, `-REV03`, ` rev B`… ou identifiant NDA sans REV) et même dossier de destination. Le nouveau document est VV ; l'ancien passe **Remplacé / VC**, sans être déplacé (archivage = M-005, NDA §17). Une révision antérieure à celle en vigueur est demandée en confirmation et classée VC. Un fichier de même document déjà présent à la destination hors registre est signalé.
- Même nom déjà présent à la destination : identique (même taille) → classé sans recopie ; différent → refusé, rien n'est écrasé.
- Registre ouvert dans Excel : message clair, bouton « Réessayer » ; les fichiers et le manifeste sont déjà écrits.

## 7. Choix provisoires à confirmer (cahier §9) — constantes en tête du script `m003`
| Question | Choix v0.1 | Constante |
|---|---|---|
| Q1 DMT | restent dans leur lot de 02_RECEPTION | `DMT_DEST = null` |
| Q2 Registre | `08_PARTAGE\REGISTRE_DOCUMENTS.csv` | `REGISTRE` |
| Q3 Plans remplacés | VC au registre seulement | — |
| Q4 Identifiant NDA §5 | lu s'il est dans le nom, sinon nom d'origine + clé | `RE_NDA` |
| Q5 Contrats des deux entités | reçus dans le dossier de l'entité choisie | — |
| Q6 Courriels | `.eml` analysé + pièces jointes extraites ; `.msg` gardé tel quel | — |

## 8. Limites connues
- `.msg` (Outlook) non analysé. Zip chiffrés et ZIP64 (> 4 Go) non extraits (message). Zip dans un zip non extrait.
- Discipline proposée seulement si le nom ou le dossier l'indique ; sinon à choisir au classement.
- Lecture seule pour l'arpenteur non encore prévue (consultation depuis Pcodes, plus tard).

## 9. Journal des versions
| Version | Date | Changement |
|---|---|---|
| M-003 v0.1 (Administration v0.9) | 2026-11-11 | Première version : entité imposée, dépôt (eml, zip, _SUPPORT), registre, classement, révisions VC, rejet de lot, inscription de dossiers existants, réindexation. |
