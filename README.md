# PRESYVA — boutique

Site vitrine et page de vente pour la collection de guides PRESYVA. Slogan : « Maîtrisez votre présence en ligne. »
HTML/CSS/JS autonome, aucune dépendance à installer, prêt pour GitHub Pages, Netlify ou Vercel.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | La boutique : hero, méthode, bénéfices, produit, sommaire, collection, achat, FAQ, footer |
| `mockup.html` | Générateur des visuels produit (voir plus bas) |
| `legal.css` | Style commun aux 3 pages légales |
| `mentions-legales.html` | Mentions légales — **à compléter** |
| `cgv.html` | Conditions générales de vente — **à compléter** |
| `confidentialite.html` | Politique de confidentialité — **à compléter** |
| `media/produit-1..3.png` | Visuels produit 1600 × 1200 pour la fiche boutique |

Le guide lui-même (source + PDF) est dans `Documents\presyva-guide-01\`.

## À faire avant la mise en ligne

1. **Brancher le paiement.** Dans `index.html`, deux liens portent l'attribut `data-checkout` et `href="#"`.
   Remplacer par l'URL de checkout (Stripe). Tant que `href="#"`, un clic affiche une alerte de rappel.
2. **Prix : 49,99 €.** Il apparaît à deux endroits dans `index.html` — le bloc `.price` (section produit) et la ligne mono de l'outro. Pensez aux deux si vous le changez.
3. **Compléter les pages légales.** Tous les champs entre crochets (`[NOM]`, `[SIRET]`…) sont surlignés visuellement
   et doivent être remplis. Un encadré orange en haut de chaque page rappelle les points à ne pas rater.
4. **Adresse e-mail.** `contact@presyva.com` est un placeholder — le domaine n'est pas enregistré. Présent dans le footer et les pages légales.
5. **Domaine.** Mettre à jour la balise `<link rel="canonical">` dans `index.html`.
6. **Immatriculation.** Les mentions légales sont en état « préversion » tant qu'il n'y a pas de SIRET, et les ventes ne doivent pas être ouvertes avant.

## Générer les visuels produit

`mockup.html` produit trois scènes 1600 × 1200, sélectionnées par le hash de l'URL (`#s1`, `#s2`, `#s3`).

```bash
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --hide-scrollbars --virtual-time-budget=4000 --window-size=1600,1200 --screenshot="C:\Users\bilel\Documents\avisreset\media\produit-1.png" "file:///C:/Users/bilel/Documents/presyva/mockup.html#s1"
```

Répéter avec `#s2` / `produit-2.png` et `#s3` / `produit-3.png`.

## Régénérer le PDF du guide

Après toute modification de `Documents\presyva-guide-01\ebook.html` :

```bash
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --no-pdf-header-footer --print-to-pdf="C:\Users\bilel\Documents\presyva-guide-01\PRESYVA-Guide-01-Maitriser-sa-presence-sur-Internet.pdf" "file:///C:/Users/bilel/Documents/presyva-guide-01/ebook.html"
```

**Chrome, pas Edge** : Edge headless perd les fonds de couleur à l'impression.

## Direction artistique

Version sombre, calquée sur les codes des sites de studio (référence : alche.studio), avec la palette PRESYVA.

**Les règles à ne pas casser si vous modifiez le site :**

| Élément | Règle |
|---|---|
| Fond | `#060b16` quasi noir, teinté navy. Le blanc n'est utilisé que pour le texte. |
| Accents | Bleu `#2563EB` / cyan `#22D3EE`, **uniquement** en dégradé de CTA, en soulignés et en points d'état. Jamais en aplat de fond. |
| Titres | **Montserrat** (la typo du logo), weight 600 (pas 800), `line-height: 1`, `letter-spacing: -.035em`. C'est ce qui donne le côté éditorial plutôt que « SaaS ». |
| Méta-texte | **Tout** en IBM Plex Mono, majuscules, `letter-spacing: .14em` : numéros de section, labels, nav, tags, durées, prix. C'est la signature. |
| Séparateurs | Hairlines `rgba(255,255,255,.10)`. Pas d'ombre, pas de carte remplie : les blocs sont délimités par des filets. |
| Boutons | Pilules `border-radius: 999px`, contour par défaut, remplissage en dégradé qui **monte du bas** au survol. |
| Grain | Un calque de bruit SVG à 3,6 % couvre la page (`#grain`). Il casse le banding des grands dégradés — ne le retirez pas. |

## La couche animée

Inspirée de reactbits.dev, mais écrite en **canvas 2D et CSS natifs** : aucune dépendance, aucun CDN, rien à installer.

| Effet | Où | Comment |
|---|---|---|
| **Aurora** | `#aurora`, fixe derrière toute la page | 4 blobs radiaux sur trajectoires de Lissajous, canvas rendu en demi-résolution puis `filter: blur(70px)`. Coût : 4 arcs par frame. |
| **Dot Field** | `#dots`, dans le hero | Grille de points qui grossissent, s'éclairent en cyan et fuient le curseur dans un rayon de 150 px. |
| **Halo curseur** | `#glow` | Dégradé radial qui suit la souris avec une inertie de 8,5 %. |
| **Shiny Text** | `.shiny` | Balayage lumineux en boucle sur le texte en dégradé. |
| **Scramble** | `.scramble` | Les titres de section se décodent à l'entrée dans le viewport, verrouillage de gauche à droite en ~0,6 s. |
| **Boutons magnétiques** | `.magnet` | Le bouton se décale vers le curseur au survol. |
| **Outro sticky** | `.outro` | Écran plein format qui reste collé pendant 190 vh, avec anneaux concentriques respirants. |

Garde-fous à conserver :

- **`prefers-reduced-motion`** désactive tout (canvas retirés du DOM visuel, outro déroulé normalement).
- **Pointeur grossier** (mobile/tablette) : dot field, halo et magnétisme sont désactivés — inutiles et coûteux en batterie.
- Le dot field **coupe sa boucle** dès que le hero sort du viewport (IntersectionObserver), l'aurora se met en pause quand l'onglet passe en arrière-plan.
- Le scramble pose un `aria-label` avec le texte réel pendant le décodage, pour que les lecteurs d'écran ne lisent jamais le charabia.

Notes techniques :

- Le livre en 3D est du CSS pur (`transform-style: preserve-3d`), pas une image : il suit automatiquement
  tout changement de titre ou de couverture, et reste net à toutes les résolutions.
- Attention dans `mockup.html` : chaque scène déclare `#sX > * { position:relative }`, ce qui écrase la
  position absolue du calque de grain. La règle `#s1 > .grain, #s2 > .grain, #s3 > .grain` existe pour
  repasser devant en spécificité — ne la supprimez pas.
- Le ton évite toute promesse de résultat. La FAQ traite la question frontalement (dernière réponse) :
  c'est un argument de crédibilité, pas une faiblesse.
- Accessibilité : `prefers-reduced-motion` coupe toutes les animations, la FAQ utilise des `<details>` natifs
  (fonctionne sans JavaScript), et la barre de progression est purement décorative.

## Ajouter un guide à la collection

1. Dupliquer `Documents\presyva-guide-01\ebook.html`, changer le numéro de guide et le titre de couverture.
2. Dans `index.html`, section « Collection » : passer la carte concernée en `class="cc live"`
   et remplacer le tag `en_préparation` par `disponible`.
3. Dupliquer la section produit si vous vendez plusieurs guides sur la même page,
   ou créer une page dédiée par guide.
