# WebCut — éditeur vidéo gratuit dans le navigateur

WebCut est un éditeur vidéo qui fonctionne **entièrement dans le navigateur** :
pas de téléchargement, pas d'inscription, pas de filigrane.
Les vidéos ne quittent jamais l'appareil de l'utilisateur.

Toute l'application tient dans un seul fichier : `index.html`.

## Fonctions

- Import par glisser-déposer : vidéos, musiques, images
- Timeline multipiste (Texte, Overlay, Vidéo, Audio) avec aimantation, zoom et barre de défilement
- Couper, dupliquer, supprimer, annuler / rétablir
- Textes (taille, couleurs, contour), incrustations (échelle, position, rotation, opacité)
- Volume et vitesse (×0,25 à ×4) par clip
- Extraction du son d'une vidéo sur une piste audio séparée
- Téléchargement du son d'un média ou d'un clip en WAV
- Formats 16:9, 9:16 et 1:1
- Export en vrai MP4 (H.264 + AAC) lisible partout : Windows, Mac, téléphones, réseaux sociaux

## Mettre en ligne avec GitHub Pages

Le site est en ligne ici : **https://troplife56-source.github.io/webcut/**

Pour le republier ailleurs :

1. Créez un dépôt sur GitHub (par exemple `webcut`) et ajoutez-y `index.html` et `README.md`.
2. Dans le dépôt : **Settings → Pages**.
3. Sous **Build and deployment**, choisissez **Deploy from a branch**, la branche `main` et le dossier `/ (root)`, puis **Save**.
4. Après une à deux minutes, le site est disponible à l'adresse `https://<votre-pseudo>.github.io/webcut/`.

## Mesure d'audience (Google Analytics 4)

- Identifiant : `G-KG78WJGE6V` (constante `GA_ID` dans `index.html`).
- Google Analytics n'est chargé **qu'après acceptation** dans le bandeau de consentement,
  comme l'exige la CNIL. Le choix est mémorisé dans le navigateur et modifiable à tout moment
  avec le bouton 🍪 de l'en-tête. Refuser supprime les cookies `_ga` déjà posés.
- Rien n'est mesuré quand le fichier est ouvert en local (`file://`).
- Aucune donnée de média n'est envoyée : pas de nom de fichier, pas de contenu.
  Seuls les pages vues et ces événements sont transmis :

| Événement        | Paramètres                          | Quand                                |
|------------------|-------------------------------------|--------------------------------------|
| `import_media`   | `media_type` (video, audio, image)  | un média est importé                 |
| `add_text`       | —                                   | un texte est ajouté                  |
| `split_clip`     | —                                   | un clip est coupé                    |
| `extract_audio`  | —                                   | le son d'une vidéo est extrait       |
| `download_audio` | `scope` (full, clip)                | un son est téléchargé en WAV         |
| `export_video`   | `format`, `method`, `duration_s`, `aspect` | une vidéo est exportée        |

Ces événements apparaissent dans Google Analytics sous **Rapports → Engagement → Événements**
(compter jusqu'à 24 h avant les premiers chiffres ; l'onglet **Temps réel** les montre tout de suite).

## Comment fonctionne l'export

1. Le son de toute la timeline est mixé d'un coup (volume, vitesse, pistes coupées, son extrait),
   avec un limiteur pour éviter la saturation.
2. La timeline est lue du début à la fin et chaque image (30 par seconde) est encodée en H.264
   avec WebCodecs, horodatée sur la timeline : pas de décalage même si l'ordinateur ralentit.
3. Un petit assembleur intégré (sans dépendance) produit un MP4 classique, avec l'index au début
   du fichier, puis vérifie que la vidéo contient bien une image.

Si le navigateur ne propose pas WebCodecs, ou si un média est trop lourd (plus de 600 Mo ou 20 min),
WebCut passe automatiquement à une méthode de secours (enregistrement en temps réel, MP4 ou WebM).

L'export se met en pause si l'onglet est masqué et reprend quand on y revient.

## Compatibilité

Navigateurs récents sur ordinateur : Chrome et Edge (recommandés pour l'export MP4), Firefox, Safari.
Les formats lisibles dépendent du navigateur (MP4 H.264 et WebM sont les plus sûrs).
