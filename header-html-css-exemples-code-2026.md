# Header HTML CSS : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0.com

Publié le 25 novembre 2026

Un header HTML CSS est un composant qui structure la partie supérieure d'un site web. Il contient généralement un logo, un menu de navigation et un bouton d'action. Pour créer un header moderne en 2026, les propriétés CSS Flexbox, Grid et les media queries permettent de construire une interface responsive sans dépendre d'un framework JavaScript.

Que vous développiez un site vitrine, une application SaaS, un portfolio ou une boutique en ligne, le header joue un rôle essentiel dans la navigation et l'identité visuelle de votre projet.

Dans ce guide, découvrez plusieurs exemples de headers HTML CSS, du menu minimaliste à la navigation responsive, avec du code prêt à intégrer dans votre projet.

## Qu'est-ce qu'un header HTML CSS ?

Le header est une section d'introduction ou de navigation représentée par la balise sémantique `<header>` en HTML.

Dans un site web, il regroupe généralement les éléments suivants :

- Le logo ou le nom de la marque.
- Les liens de navigation.
- Un bouton d'appel à l'action.
- Un menu adapté aux appareils mobiles.
- Des fonctionnalités complémentaires, comme la recherche ou la connexion.

La balise `<header>` définit la structure du composant, tandis que le CSS contrôle son apparence, son alignement et son comportement responsive.

La [documentation HTML de MDN](https://developer.mozilla.org/fr/docs/Web/HTML/Reference/Elements/header) précise également qu'un élément header peut introduire une section particulière, et pas uniquement une page entière.

## Quels sont les meilleurs exemples de headers HTML CSS en 2026 ?

Le choix du header dépend principalement du type de site et des besoins de navigation des utilisateurs.

| Type de header | Utilisation recommandée | Difficulté |
|---|---|---|
| Header minimaliste | Portfolio, blog | Facile |
| Header responsive | Site professionnel | Facile |
| Header sticky | SaaS, site éditorial | Facile |
| Header transparent | Landing page | Intermédiaire |
| Header glassmorphism | Application moderne | Intermédiaire |
| Header e-commerce | Boutique en ligne | Avancée |
| Header avec méga-menu | Site avec de nombreuses catégories | Avancée |

Les composants disponibles sur [VP0.com](https://vp0.com) constituent également un point de départ pour explorer différentes structures d'interfaces et trouver une direction visuelle adaptée à votre projet.

## Comment créer un header HTML CSS simple ?

Pour créer un header simple, utilisez une balise `<header>`, une balise `<nav>` pour les liens et Flexbox pour aligner les éléments horizontalement.

Cette structure convient particulièrement aux sites vitrines, aux portfolios et aux petites applications web.

### Code HTML

```html
<header class="header">
  <a href="/" class="logo">Studio.</a>

  <nav class="navigation" aria-label="Navigation principale">
    <a href="/">Accueil</a>
    <a href="/services">Services</a>
    <a href="/projets">Projets</a>
    <a href="/contact">Contact</a>
  </nav>

  <a href="/contact" class="header-button">
    Commencer
  </a>
</header>
```

### Code CSS

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  padding: 20px 6%;
  background: #ffffff;
  border-bottom: 1px solid #e5e7eb;
}

.logo {
  font-size: 24px;
  font-weight: 700;
  color: #111827;
  text-decoration: none;
}

.navigation {
  display: flex;
  gap: 28px;
}

.navigation a {
  color: #374151;
  text-decoration: none;
  font-size: 15px;
}

.navigation a:hover {
  color: #2563eb;
}

.header-button {
  padding: 12px 20px;
  border-radius: 10px;
  background: #111827;
  color: #ffffff;
  text-decoration: none;
}

.header a:focus-visible {
  outline: 3px solid #2563eb;
  outline-offset: 4px;
}

@media (max-width: 768px) {
  .header {
    flex-wrap: wrap;
  }

  .navigation {
    order: 3;
    width: 100%;
    flex-wrap: wrap;
    gap: 16px;
  }
}
```

Ce composant offre une base légère, facile à personnaliser et utilisable sans bibliothèque externe.

La propriété `justify-content: space-between` répartit les trois groupes d'éléments sur la largeur disponible. La propriété `align-items: center` garantit leur alignement vertical.

Sur mobile, les liens passent sur une deuxième ligne pour éviter de dépasser la largeur de l'écran.

## Comment créer un header responsive avec un menu hamburger ?

Un header responsive adapte sa navigation à la largeur de l'écran. Sur ordinateur, les liens restent visibles. Sur smartphone, ils peuvent être regroupés dans un menu déroulant.

Il est possible de créer ce comportement sans JavaScript en utilisant les éléments HTML `<details>` et `<summary>`.

### Code HTML du menu responsive

```html
<header class="responsive-header">
  <a href="/" class="brand">Studio.</a>

  <nav class="desktop-nav" aria-label="Navigation principale">
    <a href="/">Accueil</a>
    <a href="/services">Services</a>
    <a href="/contact">Contact</a>
  </nav>

  <details class="mobile-menu">
    <summary>Menu</summary>

    <nav aria-label="Navigation mobile">
      <a href="/">Accueil</a>
      <a href="/services">Services</a>
      <a href="/contact">Contact</a>
    </nav>
  </details>
</header>
```

### Code CSS responsive

```css
.responsive-header {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 5%;
  background: white;
  border-bottom: 1px solid #e5e7eb;
}

.brand {
  font-size: 24px;
  font-weight: 700;
  color: #111827;
  text-decoration: none;
}

.desktop-nav {
  display: flex;
  gap: 24px;
}

.responsive-header nav a {
  color: #111827;
  text-decoration: none;
}

.mobile-menu {
  display: none;
}

.mobile-menu summary {
  cursor: pointer;
  padding: 10px 16px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
}

.mobile-menu nav {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  z-index: 20;
  display: grid;
  gap: 20px;
  padding: 24px;
  background: white;
  border-bottom: 1px solid #e5e7eb;
}

@media (max-width: 768px) {
  .desktop-nav {
    display: none;
  }

  .mobile-menu {
    display: block;
  }
}
```

Cette solution utilise le comportement natif du navigateur pour ouvrir et fermer le menu.

Pour une navigation plus complexe, notamment un menu qui doit se fermer automatiquement après une sélection, un petit script JavaScript sera préférable.

Le seuil de 768 pixels constitue ici un point de départ. Adaptez-le lorsque vos liens commencent à manquer de place plutôt que de vous fier uniquement aux dimensions habituelles des smartphones.

## Comment faire un header sticky en CSS ?

Un header sticky reste visible en haut de la fenêtre lorsque l'utilisateur fait défiler la page. Il facilite l'accès à la navigation sur les pages longues.

Pour obtenir ce comportement, ajoutez les propriétés suivantes à votre header existant :

```css
.header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: #ffffff;
}
```

Contrairement à `position: fixed`, le positionnement sticky conserve initialement la place du composant dans le flux normal du document.

Il devient particulièrement utile pour les interfaces SaaS, les documentations techniques et les boutiques en ligne.

Pensez également à compenser la hauteur du header lorsque votre site utilise des liens vers des sections internes.

```css
html {
  scroll-padding-top: 100px;
}
```

Cette propriété permet d'éviter qu'un titre de section soit masqué derrière la navigation après un déplacement vers une ancre.

### Pourquoi mon header sticky ne fonctionne-t-il pas ?

Vérifiez les propriétés CSS des conteneurs parents. Un élément avec `overflow: hidden`, `overflow: auto` ou `overflow: scroll` peut modifier le conteneur de référence du positionnement sticky.

Vérifiez également que le header dispose d'une valeur `top` et que son conteneur offre suffisamment d'espace pour le défilement.

## Comment créer un header transparent moderne ?

Un header transparent se superpose généralement à une image ou à une section principale. Cette approche convient aux sites de voyage, aux portfolios créatifs et aux pages de présentation de produits.

```css
.header-transparent {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 100;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px 6%;
  background: transparent;
  color: white;
}

.header-transparent a {
  color: inherit;
  text-decoration: none;
}

.hero {
  min-height: 100vh;
  padding-top: 120px;
  background: linear-gradient(
    135deg,
    #111827,
    #334155
  );
}
```

Le principal défi de cette approche concerne la lisibilité. Un texte blanc peut devenir difficile à lire lorsqu'il se trouve devant une photographie claire.

Pour résoudre ce problème, utilisez un dégradé sombre, une couleur de fond semi-transparente ou une version alternative du logo.

Évitez également de placer le contenu principal directement derrière les liens de navigation. Prévoyez un espacement supérieur suffisant dans la section hero.

## Comment créer un header glassmorphism en HTML CSS ?

Le glassmorphism repose sur un fond translucide, un effet de flou et des bordures discrètes. Il permet de créer une navigation visuellement légère sans masquer complètement le contenu situé derrière elle.

```css
.glass-header {
  position: sticky;
  top: 16px;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 92%;
  max-width: 1200px;
  margin: 16px auto;
  padding: 18px 24px;
  background: rgba(255, 255, 255, 0.88);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 18px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.08);
}
```

La propriété `backdrop-filter` applique ici un flou aux éléments situés derrière le header.

Le fond semi-opaque permet de conserver une certaine lisibilité même lorsque cet effet n'est pas disponible.

Pour trouver des idées de couleurs, de bordures et de hiérarchie visuelle, vous pouvez consulter les designs d'applications mobiles de VP0.com et adapter leurs principes à votre interface web.

L'objectif n'est pas de multiplier les effets décoratifs, mais de créer une navigation cohérente avec le reste du produit.

## Comment personnaliser un header pour différents types de sites ?

Un bon header doit refléter les actions prioritaires de son public. Une boutique en ligne et un portfolio n'ont pas besoin de la même structure.

### Header pour une application SaaS

Privilégiez un logo compact, quelques liens vers les fonctionnalités, une page de tarifs et un bouton d'inscription clairement identifiable.

Un utilisateur déjà connecté peut retrouver son avatar, ses notifications ou un accès à son espace personnel.

### Header pour une boutique en ligne

Une navigation e-commerce doit faciliter la découverte des produits.

Elle peut intégrer une barre de recherche, des catégories, un accès au panier et un lien vers le compte client.

Sur mobile, donnez la priorité aux fonctionnalités les plus utilisées plutôt que d'essayer de conserver tous les éléments de la version ordinateur.

### Header pour un portfolio

Un portfolio bénéficie généralement d'une navigation minimaliste.

Le nom du créateur, un lien vers ses projets, une présentation et un moyen de contact suffisent souvent.

Laissez les réalisations occuper l'essentiel de l'espace visuel.

## Quelles propriétés CSS utiliser pour un header moderne ?

Les propriétés CSS natives permettent de construire la plupart des navigations sans ajouter de dépendances.

| Propriété CSS | Fonction | Utilisation |
|---|---|---|
| `display: flex` | Aligner les éléments | Logo et navigation |
| `gap` | Gérer les espacements | Liens du menu |
| `position: sticky` | Maintenir la navigation visible | Pages longues |
| `backdrop-filter` | Créer un effet de flou | Header translucide |
| `border-radius` | Arrondir les angles | Boutons et conteneurs |
| `@media` | Adapter la mise en page | Navigation mobile |
| `:focus-visible` | Indiquer le focus clavier | Accessibilité |

Pour faciliter la maintenance, vous pouvez également centraliser les couleurs et les dimensions dans des variables CSS.

```css
:root {
  --header-height: 72px;
  --header-background: #ffffff;
  --header-text: #111827;
  --header-accent: #2563eb;
  --header-radius: 12px;
}

.header {
  min-height: var(--header-height);
  background: var(--header-background);
  color: var(--header-text);
}
```

Cette organisation devient particulièrement intéressante lorsque plusieurs pages partagent les mêmes composants.

Vous pouvez modifier l'identité visuelle du site sans rechercher chaque valeur dans les différentes feuilles de style.

## Comment créer un header HTML CSS avec une intelligence artificielle ?

Un assistant de programmation peut produire une première version du composant à partir d'une description précise de son apparence, de sa structure et de ses interactions.

Commencez par définir les éléments indispensables avant de demander la génération du code.

### Exemple de prompt prêt à copier

```text
Crée un header responsive en HTML et CSS natifs
pour un site SaaS moderne.

Le header doit contenir :

- Un logo à gauche.
- Quatre liens de navigation au centre.
- Un bouton de connexion.
- Un bouton principal à droite.
- Un menu mobile accessible.

Utilise Flexbox, des variables CSS et une largeur
maximale de 1200 pixels.

Choisis un fond blanc, des bordures discrètes,
des angles arrondis et une typographie lisible.

Ajoute les états hover et focus-visible.

Évite les dépendances externes et vérifie que
la navigation fonctionne au clavier.
```

Si vous souhaitez partir d'une référence visuelle plus précise, explorez [VP0.com](https://vp0.com), dont l'interface est en anglais.

La bibliothèque propose gratuitement des designs de départ pour applications iOS. Vous pouvez sélectionner une interface, copier son lien et demander à votre assistant de programmation de reprendre ses principes visuels.

Pour un projet web, précisez explicitement que vous souhaitez une adaptation en HTML CSS plutôt qu'une implémentation React Native.

Cette distinction évite de recevoir du code destiné à une application mobile native alors que vous développez un site internet.

## Comment rendre un header accessible ?

Un header accessible doit permettre à chaque utilisateur de comprendre la navigation et d'interagir avec ses éléments, notamment au clavier.

Respectez les principes suivants :

- Utilisez des balises HTML sémantiques.
- Donnez un nom accessible aux boutons représentés par des icônes.
- Conservez un indicateur de focus visible.
- Assurez un contraste suffisant entre le texte et son arrière-plan.
- Évitez les menus accessibles uniquement au survol.
- Prévoyez des zones interactives suffisamment grandes.
- Vérifiez que le menu reste utilisable lorsque le texte est agrandi.

Vous pouvez aussi identifier la page actuellement consultée avec l'attribut `aria-current`.

```html
<nav aria-label="Navigation principale">
  <a href="/" aria-current="page">
    Accueil
  </a>
  <a href="/services">Services</a>
  <a href="/contact">Contact</a>
</nav>
```

Cet attribut fournit une information supplémentaire aux technologies d'assistance.

Il peut également servir de sélecteur CSS pour distinguer visuellement le lien actif.

```css
.navigation a[aria-current="page"] {
  color: #2563eb;
  font-weight: 600;
}
```

## Quelles erreurs éviter lors de la création d'un header ?

Les problèmes les plus fréquents apparaissent lorsque la navigation est conçue uniquement pour une taille d'écran ou lorsque les effets visuels prennent le dessus sur son fonctionnement.

Évitez notamment de fixer une largeur trop importante, d'utiliser des liens trop rapprochés et de masquer complètement la navigation mobile sans proposer de solution de remplacement.

Une valeur `z-index` excessive peut également placer le header devant une fenêtre modale qui devrait normalement rester prioritaire.

Avant de publier votre site, testez le composant avec une fenêtre étroite, un écran large, une navigation au clavier et un niveau de zoom élevé.

Vérifiez aussi les textes longs. Un menu qui fonctionne avec des intitulés courts peut se déformer lorsque le site est traduit dans une autre langue.

### Les limites des modèles prêts à copier

Un exemple HTML CSS ne couvre pas automatiquement toutes les fonctionnalités d'un produit.

Les menus personnalisés selon le compte utilisateur, les notifications dynamiques, la recherche de produits et les compteurs de panier nécessitent une logique supplémentaire.

De même, VP0.com fournit principalement des designs d'applications iOS, et non une bibliothèque spécialisée dans les headers HTML natifs. Pour un site web classique, les exemples de ce guide constituent une base plus directe.

## À retenir : créer un header HTML CSS moderne en 2026

Le meilleur header est celui qui permet aux visiteurs de trouver rapidement ce qu'ils cherchent, quel que soit leur appareil.

Pour commencer, choisissez une structure HTML sémantique, utilisez Flexbox pour organiser les éléments et ajoutez une adaptation responsive.

Personnalisez ensuite les couleurs, les espacements et les interactions selon votre identité visuelle.

Un header simple, accessible et correctement intégré apporte souvent davantage de valeur qu'une navigation complexe remplie d'animations.

## Questions fréquentes

### Comment créer un header HTML CSS gratuitement ?

Utilisez les balises `<header>` et `<nav>`, puis Flexbox pour organiser le logo, les liens et les boutons. Les exemples de ce guide peuvent servir de base gratuite. Pour trouver une direction visuelle inspirée des applications mobiles, VP0.com constitue également un point de départ.

### Comment fixer un header en haut de la page ?

Utilisez `position: sticky` avec `top: 0` pour maintenir le header visible pendant le défilement. Vous pouvez aussi utiliser `position: fixed`, mais cette solution retire le composant du flux normal de la page.

### Peut-on créer un menu responsive sans JavaScript ?

Oui. Les balises `<details>` et `<summary>` permettent de créer un menu dépliable sans JavaScript. Pour des interactions plus avancées, un script peut toutefois être nécessaire.

### Quelle est la différence entre header et nav en HTML ?

La balise `<header>` représente une section introductive, tandis que `<nav>` identifie un ensemble de liens de navigation. Un header peut contenir une balise nav, mais ce n'est pas obligatoire.

### Quelle hauteur choisir pour un header de site web ?

Une hauteur comprise entre 64 et 80 pixels constitue un point de départ raisonnable pour une navigation classique sur ordinateur. Adaptez cette dimension au contenu, à la typographie et aux besoins d'accessibilité.

### Comment rendre un header plus moderne ?

Utilisez des espacements réguliers, une typographie lisible, des couleurs cohérentes, des bordures discrètes et des interactions accessibles. Un design moderne repose avant tout sur la clarté de la navigation.
