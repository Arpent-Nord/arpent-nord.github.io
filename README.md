# Boîte à outils – Arpent-Nord et MECATECH Arpentage

Page d'accueil unique des applications des arpenteurs : **https://arpent-nord.github.io/**

La boîte à outils sert les deux entités, Arpent-Nord construction et MECATECH Arpentage. Elles sont régies par les mêmes méthodes (norme NDA-001) et ont la même arborescence de projets.

## Contenu du dépôt

Ce dépôt contient la page d'accueil et les modules du système de gestion des contrats. Les autres outils ont chacun leur propre dépôt public.

| Chemin | Rôle |
|---|---|
| `/index.html` | Accueil : menu des outils, bandeau du jour (nom de levé et de point) |
| `/gestion-leves/` | Gestion des levés (calculateur) : boîte de dépôt, répartition, index, contrats, bilan |
| `/gestion-leves/#depot-leves` | Dépôt des levés (arpenteur), présenté à part dans l'accueil |
| `/gestion-leves/lib/` | Bibliothèque heic-to (conversion des photos iPhone) et sa licence |
| `/admin/` | Administration des contrats : codes arpenteurs (depuis Monday), tables de référence |
| `/voie-ferree/` | Menu des outils spécialisés de voie ferrée |
| `logo-*.svg`, `logo-*.png` | Logos originaux des deux entités (voir « Logos ») |

## Outils dans d'autres dépôts

| Outil | Ouvrir | Documentation |
|---|---|---|
| Pcodes et Liaisons | /APP-PCodes/ | github.com/Arpent-Nord/APP-PCodes |
| Validation de levé | /validation-leve/ | /validation-leve/guide.html |
| Contrôle d'implantation | /controle-implantation-app/ | /controle-implantation-app/guide.html |
| GEDO REC – rapport de voie | /APP-Gedo-Rec/ | (voie ferrée) |
| Vérification nez de quai | /APP-Verification-Nez-de-Quai/ | (voie ferrée) |

## Règles communes à toutes les applications

- **Aucune donnée de mandat** (plan, levé, nom de client) dans un dépôt public. Les fichiers restent sur l'appareil de l'utilisateur ou dans OneDrive / SharePoint.
- **Bouton de retour** « ‹ Boîte à outils » dans l'en-tête de chaque application.
- **Logos** : dans l'en-tête des modules, Arpent-Nord puis MECATECH. Dans les rapports et fiches d'un contrat, le logo de l'entité du contrat (AN → Arpent-Nord, MA → MECATECH). Les logos sont intégrés dans chaque page (WebP en base64) selon le standard des logos (v4).
- **Numéro de version** et date dans le pied de page de chaque module, augmentés à chaque livraison.

## Logos

- `logo-mecatech_horizontal.svg` / `.png` (2400 px) : usage courant (en-têtes, rapports).
- `logo-mecatech_vertical.svg` / `.png` (2400 px) : pages couvertures, papeterie, documents Word.
- `logo-arpent-nord.svg` / `.png` (2400 px) : originaux tirés de `logo.ai`.

## Publier une livraison

1. Décompresser le zip de livraison sur le poste.
2. Dans ce dépôt : **Add file → Upload files**, puis glisser le **contenu** du zip (dossiers et fichiers), jamais le zip lui-même.
3. **Commit changes**, attendre 1 à 2 minutes, puis vérifier avec **Ctrl+F5** (deux fois sur téléphone si la page est installée).

## Ajouter un outil

1. Créer le dépôt public de l'outil et activer Pages (main / root), avec un `guide.html`.
2. Copier une carte `<article class="tool">` dans `index.html` et changer le nom, la description et les liens.
3. Ajouter une ligne dans « Quel outil prendre ? » et dans le tableau ci-dessus.
4. Ajouter le bouton de retour et les logos, selon le standard des logos.
