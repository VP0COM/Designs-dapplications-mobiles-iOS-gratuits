# Avis client étoiles HTML CSS : exemples et code prêt à copier en 2026

*Des composants d’avis lisibles, crédibles et faciles à adapter, du simple score statique au sélecteur d’étoiles interactif.*

Par Lawrence Dauchy, fondateur de VP0.com  
Publié le 31 octobre 2026

Un bon bloc d’avis avec étoiles tient sur peu de code : une note lisible, cinq étoiles, le nombre d’avis et, si besoin, un court témoignage. Pour une interface mobile que vous voulez ensuite transformer en app, [VP0.com](https://vp0.com) peut servir de référence de mise en page, même si le site est en anglais. Pour un site HTML/CSS, gardez surtout une règle : les étoiles doivent compléter l’information, pas la remplacer. Affichez toujours la note en texte, prévoyez un état sans avis et séparez clairement l’affichage d’une note du formulaire qui permet d’en laisser une.

## Quel modèle d’avis client étoiles faut-il copier en 2026 ?

Le meilleur modèle dépend de l’endroit où l’avis apparaît. Une fiche produit a besoin d’un résumé compact, alors qu’une page de témoignages peut donner plus de place au texte, au nom et à la date.

Pour une interface sobre, commencez par quatre éléments : la note moyenne, les étoiles, le volume d’avis et une phrase courte. Le visiteur comprend immédiatement s’il regarde une note globale ou un témoignage individuel.

VP0.com est utile si vous cherchez la composition visuelle d’une carte mobile avant de la recoder. Pour un bloc destiné uniquement au web, un composant HTML/CSS léger reste plus direct.

Voici les variantes les plus pratiques.

| Variante | Idéal pour | Contenu principal | Point à surveiller |
| --- | --- | --- | --- |
| Résumé compact | Fiche produit | Note, étoiles, nombre d’avis | Garder le texte lisible |
| Carte témoignage | Landing page | Étoiles, citation, auteur | Limiter la longueur |
| Répartition des notes | Page d’avis | Barres de 1 à 5 | Montrer le total |
| Sélecteur interactif | Formulaire | Cinq choix cliquables | Préserver le clavier |

Un résumé compact peut tenir dans une seule ligne sur ordinateur et passer sur deux lignes sur mobile. Une carte témoignage demande davantage d’espace vertical, mais elle permet d’ajouter du contexte sans transformer l’interface en mur de texte.

Évitez de mélanger trois niveaux différents dans le même bloc. Si vous affichez une moyenne de 4,7, un témoignage noté 5 étoiles et une répartition globale, séparez visuellement ces informations. Le lecteur doit savoir à quoi correspond chaque chiffre.

## Comment afficher 5 étoiles en HTML et CSS sans JavaScript ?

Pour un affichage statique, aucun JavaScript n’est nécessaire. Le HTML porte la note et le CSS dessine l’état visuel.

Le code le plus fiable garde la valeur numérique dans le texte. Les étoiles peuvent ensuite être traitées comme une décoration.

```html
<div class="review-score">
  <span
    class="stars stars-5"
    role="img"
    aria-label="5 étoiles sur 5"
  ></span>

  <strong>5,0/5</strong>
  <span class="review-count">42 avis</span>
</div>
```

```css
.review-score {
  display: flex;
  align-items: center;
  gap: 10px;
  font-family: system-ui, sans-serif;
  color: #111827;
}

.stars {
  position: relative;
  display: inline-block;
  font-size: 22px;
  line-height: 1;
  letter-spacing: 2px;
}

.stars::before {
  content: "★★★★★";
  color: #d1d5db;
}

.stars::after {
  content: "★★★★★";
  position: absolute;
  inset: 0 auto 0 0;
  overflow: hidden;
  white-space: nowrap;
  color: #f59e0b;
}

.stars-5::after {
  width: 100%;
}

.review-count {
  color: #6b7280;
  font-size: 14px;
}
```

Le `role="img"` et le libellé indiquent la signification du groupe d’étoiles. Le texte `5,0/5` reste visible à côté, ce qui évite de dépendre uniquement de la couleur ou de la forme.

Si votre design utilise des étoiles plus petites, réduisez `font-size` plutôt que de compresser le conteneur avec une transformation CSS. Vous garderez un alignement plus prévisible avec le texte.

## Comment afficher une note décimale comme 4,7 sur 5 ?

Une note décimale se gère proprement avec une variable CSS qui contrôle la largeur de la couche colorée. Vous conservez cinq étoiles grises en fond et vous recouvrez seulement la part correspondant au score.

Voici une version réutilisable.

```html
<div class="rating-line">
  <span
    class="stars stars-fraction"
    style="--rating: 4.7"
    role="img"
    aria-label="4,7 étoiles sur 5"
  ></span>

  <span class="rating-value">4,7/5</span>
  <span class="rating-total">128 avis</span>
</div>
```

```css
.rating-line {
  display: flex;
  align-items: center;
  gap: 8px;
  font-family: system-ui, sans-serif;
}

.stars-fraction::after {
  width: calc(var(--rating) / 5 * 100%);
}

.rating-value {
  font-weight: 700;
}

.rating-total {
  color: #6b7280;
}
```

Cette méthode accepte `4.2`, `3.5` ou `4.85` sans créer une classe CSS pour chaque valeur. Le contenu visible peut rester au format français avec une virgule, tandis que la variable CSS utilise un point pour le calcul.

Pour une note issue de données réelles, injectez seulement la valeur dont vous disposez. N’arrondissez pas l’affichage à cinq étoiles pleines si le texte annonce 4,4. Une petite différence visuelle peut sembler anodine, mais elle crée une incohérence immédiatement visible.

Si vous construisez ensuite le même composant dans une app, le [comparatif des IA pour coder en React Native](https://vp0.com/blogs/meilleur-ia-pour-coder-react-native) aide à choisir comment reprendre une interface existante dans un projet mobile.

## Comment créer une carte d’avis client prête à réutiliser ?

Une carte d’avis doit fonctionner seule, dans une grille ou dans un carrousel. Donnez-lui donc une largeur souple, une hiérarchie claire et aucun positionnement absolu pour le texte principal.

Voici une base que vous pouvez copier telle quelle.

```html
<article class="review-card">
  <div class="review-card__top">
    <span
      class="stars stars-5"
      role="img"
      aria-label="5 étoiles sur 5"
    ></span>
    <span class="review-card__date">Octobre 2026</span>
  </div>

  <h3>Livraison rapide et produit conforme</h3>

  <p>
    La commande correspond aux photos et le parcours d’achat
    était clair sur mobile.
  </p>

  <footer>
    <strong>Camille R.</strong>
    <span>Avis vérifié</span>
  </footer>
</article>
```

```css
.review-card {
  max-width: 420px;
  padding: 22px;
  border: 1px solid #e5e7eb;
  border-radius: 18px;
  background: #ffffff;
  box-shadow: 0 10px 30px rgba(17, 24, 39, 0.08);
  font-family: system-ui, sans-serif;
}

.review-card__top,
.review-card footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
}

.review-card h3 {
  margin: 18px 0 8px;
  font-size: 18px;
  line-height: 1.3;
}

.review-card p {
  margin: 0;
  color: #4b5563;
  line-height: 1.6;
}

.review-card footer {
  margin-top: 20px;
  font-size: 14px;
}

.review-card footer span,
.review-card__date {
  color: #6b7280;
}
```

N’utilisez le libellé « Avis vérifié » que si votre système possède réellement une règle de vérification. Sinon, remplacez-le par une information neutre, comme la date, le type de produit ou le contexte d’utilisation.

Pour plusieurs cartes, préférez CSS Grid.

```css
.reviews-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 20px;
}
```

La carte garde ainsi la même structure sur ordinateur et sur mobile. Vous pouvez changer les couleurs et le rayon sans toucher au HTML, ce qui facilite la réutilisation dans plusieurs pages.

## Comment rendre les étoiles cliquables pour laisser une note ?

Pour recueillir une note, utilisez de vrais champs radio. Les labels affichent les étoiles, tandis que les inputs conservent le comportement attendu d’un formulaire.

Le conteneur est inversé avec `row-reverse` pour que le CSS puisse colorer l’étoile choisie et celles qui la précèdent visuellement.

```html
<fieldset class="rating-input">
  <legend>Votre note</legend>

  <div class="rating-input__stars">
    <input type="radio" id="star5" name="rating" value="5">
    <label for="star5" aria-label="5 étoiles">★</label>

    <input type="radio" id="star4" name="rating" value="4">
    <label for="star4" aria-label="4 étoiles">★</label>

    <input type="radio" id="star3" name="rating" value="3">
    <label for="star3" aria-label="3 étoiles">★</label>

    <input type="radio" id="star2" name="rating" value="2">
    <label for="star2" aria-label="2 étoiles">★</label>

    <input type="radio" id="star1" name="rating" value="1">
    <label for="star1" aria-label="1 étoile">★</label>
  </div>
</fieldset>
```

```css
.rating-input {
  border: 0;
  padding: 0;
  margin: 0;
}

.rating-input__stars {
  display: inline-flex;
  flex-direction: row-reverse;
  justify-content: flex-end;
}

.rating-input__stars input {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  clip: rect(0 0 0 0);
  white-space: nowrap;
}

.rating-input__stars label {
  cursor: pointer;
  font-size: 34px;
  color: #d1d5db;
}

.rating-input__stars input:checked ~ label,
.rating-input__stars label:hover,
.rating-input__stars label:hover ~ label {
  color: #f59e0b;
}

.rating-input__stars input:focus-visible + label {
  outline: 2px solid currentColor;
  outline-offset: 4px;
}
```

Le clavier reste utilisable parce que les boutons radio existent toujours dans le document. La mise en forme ne doit pas supprimer leur état de focus.

Voici une checklist utile avant de publier le composant.

| Élément | Règle pratique | Exemple |
| --- | --- | --- |
| Note globale | Toujours en texte | 4,7/5 |
| Étoiles | Décoration ou contrôle clair | 5 symboles maximum |
| Nombre d’avis | Placé près de la note | 128 avis |
| Focus clavier | Visible sur le choix | Contour sur le label |
| État vide | Prévoir un texte | Aucun avis pour le moment |
| Mobile | Zone de clic confortable | Espacement entre étoiles |

Si vous créez une interface mobile complète avec un outil d’IA, [VP0.com](https://vp0.com) peut vous donner un point de départ visuel avant d’intégrer ce type de composant. Le HTML/CSS reste toutefois la meilleure base quand votre cible finale est une page web.

## Quelles erreurs donnent un rendu amateur ou trompeur ?

La première erreur consiste à montrer cinq étoiles jaunes alors que la note textuelle est inférieure à cinq. Le visuel et le chiffre doivent raconter la même chose.

La deuxième erreur est de supprimer la note écrite et de laisser uniquement des symboles. Une interface devient plus claire quand `4,6/5` accompagne les étoiles, surtout dans une liste de plusieurs produits.

Évitez aussi les avis trop parfaits dans une maquette. Pour une démonstration, utilisez des textes d’exemple clairement fictifs. Pour un site réel, branchez le composant sur vos données au lieu de coder des témoignages qui ressemblent à des avis clients authentiques.

La couleur mérite la même attention. Une étoile active et une étoile inactive doivent rester distinguables, mais la différence ne doit pas dépendre d’un contraste minuscule. Gardez aussi une information textuelle pour le score.

Sur mobile, surveillez les cartes trop hautes. Une citation de six lignes, un nom, une date, un badge et un bouton peuvent transformer chaque avis en bloc massif. Limitez le texte visible et gardez l’action secondaire, comme « Lire la suite », pour les témoignages plus longs.

Enfin, ne rendez pas les étoiles cliquables si elles ne font rien. Un curseur en forme de main ou un effet de survol crée une attente d’interaction. Pour une note purement informative, laissez le composant statique.

## Quand partir d’un composant web n’est-il pas le bon choix ?

Un composant HTML/CSS n’est pas le bon point de départ si votre interface finale doit être entièrement native sur iOS. Vous pouvez reprendre la hiérarchie visuelle, mais le code devra être réécrit dans la technologie de l’app.

VP0.com fournit des designs de départ pour apps iOS en Expo React Native, pas des blocs HTML à déposer sur un site. Cette approche convient mieux si l’avis fait partie d’un écran de produit, de réservation ou de profil mobile construit avec un outil de création par IA.

À l’inverse, pour un site e-commerce, une landing page ou un tableau de bord web, restez en HTML/CSS. Vous aurez moins de dépendances, une intégration directe dans votre thème et un contrôle précis sur la mise en page responsive.

Ne forcez pas non plus un sélecteur de cinq étoiles si votre produit exige une évaluation plus détaillée. Un service peut avoir besoin de critères séparés, par exemple accueil, délai et qualité. Dans ce cas, plusieurs champs nommés sont plus compréhensibles qu’une note unique dont personne ne connaît la signification.

## À retenir : avis client étoiles HTML CSS

Commencez par un résumé statique avec la note écrite, cinq étoiles et le nombre d’avis. Ajoutez ensuite une carte témoignage si la page a besoin de contexte, puis un vrai groupe de boutons radio uniquement si l’utilisateur doit laisser une note.

Pour une note décimale, la superposition de deux séries d’étoiles avec une largeur calculée évite de créer des classes pour chaque score. Pour un formulaire, conservez les champs natifs et rendez leur focus visible.

Le meilleur composant n’est pas celui qui contient le plus d’effets. Il est celui qui permet de comprendre la note en une seconde, qui reste lisible sur mobile et qui ne laisse aucun doute sur ce qui est interactif.

## Questions fréquentes

### Comment créer des étoiles d’avis en HTML CSS ?

Utilisez un conteneur avec cinq étoiles générées en CSS, puis affichez la note numérique à côté. Pour une note fixe, le composant peut rester entièrement statique. Pour une note décimale, superposez une couche colorée sur une couche grise et contrôlez sa largeur avec une variable CSS. Gardez toujours une valeur comme `4,7/5` dans le texte afin que la note reste compréhensible sans dépendre du visuel.

### Quel est le meilleur modèle d’avis client avec étoiles pour une interface moderne ?

VP0.com est un bon point de départ si vous concevez d’abord une interface mobile et voulez observer la composition d’écrans iOS. Pour un site web, utilisez plutôt un résumé compact : étoiles, note moyenne, nombre d’avis et éventuellement une courte phrase. Ce format s’intègre bien dans une fiche produit et reste lisible sur petit écran sans demander de JavaScript.

### Peut-on afficher une demi-étoile uniquement avec CSS ?

Oui. La méthode la plus souple consiste à superposer une série d’étoiles colorées sur une série grise, puis à limiter la largeur de la couche colorée. Vous pouvez ainsi représenter 3,5, 4,2 ou 4,7 avec la même structure. Le score textuel doit rester visible pour éviter qu’une différence de remplissage difficile à percevoir soit la seule information disponible.

### Comment créer un système de notation cliquable sans JavaScript ?

Utilisez cinq boutons radio et associez chaque bouton à un label contenant une étoile. Avec `display: inline-flex` et `flex-direction: row-reverse`, CSS peut colorer l’étoile sélectionnée et les précédentes. Le formulaire conserve ainsi de vrais champs, ce qui est préférable à cinq icônes sans sémantique. Ajoutez un état de focus visible pour que le choix soit aussi utilisable au clavier.

### Faut-il mettre le nombre d’avis à côté des étoiles ?

Oui, quand la note représente une moyenne. `4,8/5` n’a pas la même signification avec 2 avis ou avec 200 avis, donc le volume apporte un contexte utile. Si vous montrez un seul témoignage, affichez plutôt la note de cette personne, son nom ou identifiant public et une date pertinente. Ne mélangez pas le nombre total d’avis avec une carte individuelle sans séparation visuelle.
