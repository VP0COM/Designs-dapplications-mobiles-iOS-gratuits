# Effet machine à écrire CSS : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0.com  
Publié le 23 novembre 2026

L'effet machine à écrire en CSS permet de faire apparaître un texte progressivement, comme si quelqu'un le tapait sur un clavier. Vous pouvez le créer avec quelques lignes de CSS, sans JavaScript, en combinant les animations `@keyframes`, la propriété `width`, la fonction `steps()` et un curseur clignotant. Cet effet convient particulièrement aux titres de pages d'accueil, aux portfolios de développeurs, aux interfaces de chat et aux applications générées avec l'intelligence artificielle.

Dans ce guide, vous trouverez plusieurs exemples prêts à copier, des variantes modernes et des conseils pour intégrer cette animation sans dégrader l'accessibilité ni les performances de votre interface.

## Comment créer un effet machine à écrire avec CSS ?

La méthode la plus simple consiste à masquer le texte qui dépasse d'un élément, puis à augmenter progressivement sa largeur. La fonction `steps()` transforme cette progression en étapes discrètes, ce qui donne l'impression que les caractères apparaissent un par un.

Voici un exemple complet en HTML et CSS.

### Exemple 1 : effet machine à écrire simple

**HTML**

```html
<div class="typing-container">
  <h1 class="typing-text">
    Bonjour, bienvenue !
  </h1>
</div>
```

**CSS**

```css
.typing-container {
  display: inline-block;
  max-width: 100%;
}

.typing-text {
  font-family: monospace;
  font-size: 2rem;
  white-space: nowrap;
  overflow: hidden;
  width: 0;
  max-width: 100%;
  border-right: 3px solid #2563eb;
  animation:
    typing 3s steps(20, end) forwards,
    blink 0.7s step-end infinite;
}

@keyframes typing {
  from {
    width: 0;
  }

  to {
    width: 20ch;
  }
}

@keyframes blink {
  50% {
    border-color: transparent;
  }
}

@media (prefers-reduced-motion: reduce) {
  .typing-text {
    width: auto;
    animation: none;
    border-right: none;
  }
}
```

Dans cet exemple, la largeur passe de zéro à `20ch`, une valeur adaptée approximativement à une chaîne de 20 caractères avec une police monospace.

Le paramètre `steps(20, end)` découpe l'animation en 20 étapes. Pour obtenir un résultat propre, adaptez le nombre d'étapes et la largeur au texte utilisé.

La propriété `white-space: nowrap` empêche les retours à la ligne pendant l'animation, tandis que `overflow: hidden` masque les caractères qui ne sont pas encore visibles.

**À retenir :** la technique fonctionne particulièrement bien pour les textes courts utilisant une police à chasse fixe. Les textes longs et les polices proportionnelles demandent davantage de précautions.

## Quels sont les meilleurs exemples d'effet machine à écrire CSS en 2026 ?

Un effet machine à écrire peut servir plusieurs objectifs : présenter une profession, attirer l'attention sur une proposition de valeur ou simuler une réponse dans une interface conversationnelle.

Le choix de l'animation dépend surtout du contexte d'utilisation.

| Exemple | Utilisation recommandée | Difficulté |
| --- | --- | --- |
| Texte progressif simple | Titres et introductions | Facile |
| Curseur clignotant | Portfolios et pages d'accueil | Facile |
| Texte qui s'efface | Présentation de plusieurs messages | Moyenne |
| Animation en boucle | Accroches courtes | Moyenne |
| Réponse de chatbot | Interfaces conversationnelles | Moyenne |
| Plusieurs lignes successives | Tutoriels et présentations | Avancée |

### Exemple 2 : ajouter un curseur clignotant

Le curseur est l'un des éléments visuels qui rendent l'effet immédiatement reconnaissable.

Vous pouvez utiliser une bordure droite pour simuler un curseur vertical.

```css
.cursor {
  display: inline-block;
  width: 2px;
  height: 1em;
  background: currentColor;
  vertical-align: -0.15em;
  animation: cursor-blink 0.8s step-end infinite;
}

@keyframes cursor-blink {
  50% {
    opacity: 0;
  }
}

@media (prefers-reduced-motion: reduce) {
  .cursor {
    animation: none;
  }
}
```

**HTML**

```html
<p>
  Développeur frontend
  <span class="cursor" aria-hidden="true"></span>
</p>
```

Cette version anime uniquement le curseur. Elle ne révèle pas progressivement le texte, mais elle constitue une solution intéressante lorsque vous souhaitez conserver un titre immédiatement lisible.

Elle est également plus simple à intégrer dans une interface responsive.

### Exemple 3 : effet machine à écrire avec suppression du texte

Pour simuler une personne qui écrit puis efface une phrase, utilisez une animation qui augmente et réduit la largeur.

**HTML**

```html
<h2 class="typing-loop">
  Je crée des interfaces
</h2>
```

**CSS**

```css
.typing-loop {
  font-family: monospace;
  white-space: nowrap;
  overflow: hidden;
  width: 0;
  max-width: 100%;
  border-right: 2px solid currentColor;

  animation:
    type-delete 6s steps(22, end) infinite,
    cursor-blink 0.7s step-end infinite;
}

@keyframes type-delete {
  0% {
    width: 0;
  }

  40% {
    width: 22ch;
  }

  60% {
    width: 22ch;
  }

  100% {
    width: 0;
  }
}

@keyframes cursor-blink {
  50% {
    border-color: transparent;
  }
}

@media (prefers-reduced-motion: reduce) {
  .typing-loop {
    width: auto;
    animation: none;
    border-right: none;
  }
}
```

Le texte apparaît pendant la première partie de l'animation, reste visible quelques instants, puis disparaît progressivement.

Vous pouvez modifier les pourcentages des étapes pour prolonger la pause avant l'effacement.

Cette approche convient aux messages courts. Pour alterner plusieurs phrases de longueurs différentes, JavaScript devient généralement plus pratique.

## Comment réaliser un effet machine à écrire CSS sans JavaScript ?

Il est tout à fait possible de créer une animation de saisie sans JavaScript lorsque le contenu est connu à l'avance.

CSS peut contrôler la vitesse, la progression, les pauses et les répétitions. Il ne peut cependant pas gérer facilement une suite arbitraire de textes dynamiques avec des longueurs variables.

### Comprendre la fonction steps()

La fonction `steps()` est au cœur de nombreuses animations de saisie.

Une animation classique évolue continuellement entre deux valeurs. Avec `steps()`, la progression se fait par sauts.

Par exemple :

```css
animation: typing 3s steps(15, end) forwards;
```

Cette déclaration signifie :

- `typing` : nom de l'animation.
- `3s` : durée totale de trois secondes.
- `steps(15, end)` : progression en quinze étapes.
- `forwards` : conservation de l'état final.

Pour un texte de quinze caractères dans une police monospace, cette configuration peut produire une révélation caractère par caractère.

Attention : la propriété `ch` représente la largeur du caractère « 0 » dans la police utilisée. Elle ne garantit pas une correspondance exacte pour toutes les polices ou tous les caractères Unicode.

### Exemple 4 : animation avec délai

Vous pouvez retarder l'apparition du texte pour laisser le temps aux autres éléments de la page de se charger visuellement.

```css
.delayed-typing {
  font-family: monospace;
  overflow: hidden;
  white-space: nowrap;
  width: 0;

  animation:
    reveal 2.5s steps(18, end) 1s forwards;
}

@keyframes reveal {
  to {
    width: 18ch;
  }
}

@media (prefers-reduced-motion: reduce) {
  .delayed-typing {
    width: auto;
    animation: none;
  }
}
```

Le délai d'une seconde est indiqué après la fonction `steps()`.

Cette technique est utile pour une page d'accueil où le titre principal doit apparaître après une courte transition.

Évitez toutefois de multiplier les délais sur les informations essentielles. Un utilisateur ne devrait pas attendre plusieurs secondes avant de comprendre ce que propose votre site.

## Comment intégrer cet effet dans une interface moderne ?

Un effet machine à écrire réussi ne dépend pas uniquement du code CSS. Il doit également correspondre au style visuel de votre application.

Un portfolio de développeur peut utiliser une police monospace et un curseur très marqué. Une application de productivité bénéficiera plutôt d'une animation discrète et d'un rythme plus rapide.

Pour les créateurs d'applications iOS utilisant l'IA, [VP0.com](https://vp0.com) propose des designs de départ gratuits en Expo React Native. Vous pouvez vous en inspirer pour organiser vos écrans avant d'ajouter vos propres animations.

Il faut cependant distinguer deux environnements : le CSS présenté dans cet article concerne principalement les interfaces web. Les animations d'une application React Native native utilisent d'autres mécanismes.

### Exemple 5 : titre animé dans une section hero

Une section hero est le premier grand bloc visible d'une page d'accueil.

L'effet machine à écrire peut attirer l'attention sur une phrase courte sans imposer une animation à toute la page.

```html
<section class="hero">
  <span class="hero-label">
    Portfolio développeur
  </span>

  <h1>Je transforme vos idées en</h1>

  <p class="hero-typing">
    expériences digitales
  </p>

  <p class="hero-description">
    Interfaces modernes, rapides
    et accessibles.
  </p>
</section>
```

```css
.hero {
  padding: 80px 24px;
  text-align: center;
}

.hero-label {
  color: #64748b;
  font-size: 0.875rem;
}

.hero h1 {
  font-size: clamp(2rem, 5vw, 4rem);
  margin-bottom: 16px;
}

.hero-typing {
  display: inline-block;
  max-width: 100%;
  font-family: monospace;
  font-size: clamp(1rem, 3vw, 2rem);
  color: #2563eb;

  white-space: nowrap;
  overflow: hidden;
  width: 0;

  animation: hero-type 3s steps(21, end) forwards;
}

@keyframes hero-type {
  to {
    width: 21ch;
  }
}

.hero-description {
  color: #64748b;
  margin-top: 24px;
}

@media (prefers-reduced-motion: reduce) {
  .hero-typing {
    width: auto;
    animation: none;
  }
}
```

Le contraste entre le titre statique et la phrase animée permet de guider le regard.

Pour un rendu professionnel, gardez une hiérarchie typographique claire et évitez les animations simultanées sur plusieurs éléments.

### Pourquoi partir d'un design existant ?

Lorsque vous créez une interface avec Cursor, Claude Code ou un autre outil d'IA, il peut être tentant de demander directement une page complète avec plusieurs animations.

Le résultat risque toutefois de manquer de cohérence si vous ne fournissez aucune référence visuelle.

Une méthode plus fiable consiste à :

1. Choisir une structure d'interface adaptée à votre projet.
2. Définir les couleurs, les espacements et la typographie.
3. Intégrer les composants statiques.
4. Ajouter l'effet machine à écrire uniquement aux éléments pertinents.
5. Tester le rendu sur ordinateur et sur mobile.

VP0.com peut servir de référence de design pour une application iOS. Pour un site web, vous devrez adapter les idées visuelles à votre propre structure HTML et CSS.

## Comment rendre l'effet machine à écrire responsive et accessible ?

L'animation doit rester compréhensible sur les petits écrans et respecter les préférences des utilisateurs sensibles au mouvement.

Un texte trop long associé à `white-space: nowrap` peut dépasser la largeur disponible sur mobile.

De plus, une animation répétée indéfiniment peut détourner l'attention du contenu principal.

### Adapter la taille du texte

Utilisez `clamp()` pour définir une taille fluide :

```css
.typing-responsive {
  font-size: clamp(1rem, 3vw, 2.5rem);
  max-width: 100%;
}
```

Cette technique ajuste la taille de la police selon la largeur de l'écran, tout en conservant des limites minimales et maximales.

Elle ne suffit cependant pas toujours pour les phrases longues.

Pour un texte susceptible de dépasser l'écran, privilégiez une phrase plus courte ou une animation différente, par exemple une apparition progressive avec `opacity`.

### Respecter les préférences de mouvement réduit

La media query `prefers-reduced-motion` permet d'adapter les animations aux préférences définies dans le système de l'utilisateur.

```css
@media (prefers-reduced-motion: reduce) {
  .typing-text,
  .typing-loop,
  .typing-responsive {
    animation: none;
    width: auto;
    max-width: 100%;
    white-space: normal;
    border-right: none;
  }
}
```

Vous pouvez approfondir cette fonctionnalité dans la [documentation officielle de prefers-reduced-motion](https://developer.mozilla.org/fr/docs/Web/CSS/@media/prefers-reduced-motion).

Pour une animation qui révèle du texte, la version sans mouvement doit afficher immédiatement le contenu complet.

### Éviter les problèmes avec les lecteurs d'écran

Le texte animé visuellement n'a pas besoin d'être annoncé caractère par caractère.

Pour une interface statique, gardez le texte dans le DOM et évitez de modifier continuellement son contenu accessible.

Si vous utilisez JavaScript pour simuler une saisie, vous pouvez séparer la version visuelle de la version destinée aux technologies d'assistance.

La bonne solution dépend du contexte, notamment lorsqu'il s'agit d'une vraie réponse générée par un assistant conversationnel.

### Checklist avant publication

| Élément à vérifier | Bonne pratique |
| --- | --- |
| Texte mobile | Aucune coupure involontaire |
| Animation | Durée adaptée à la longueur |
| Accessibilité | Version sans mouvement |
| Curseur | Contraste suffisant |
| Performance | Éviter les animations inutiles |
| Contenu | Texte compréhensible sans effet |
| Responsive | Tester plusieurs largeurs |

Ces vérifications sont particulièrement importantes pour une page d'accueil ou une interface utilisée quotidiennement.

## Quelles erreurs éviter avec les animations de saisie CSS ?

La première erreur consiste à utiliser le même nombre d'étapes pour toutes les phrases.

Si votre texte contient vingt caractères, mais que l'animation utilise `steps(10)`, plusieurs caractères peuvent apparaître au même moment.

Inversement, un nombre excessif d'étapes peut créer des mouvements intermédiaires peu naturels.

### Erreur 1 : utiliser une police proportionnelle

Les lettres n'ont pas toutes la même largeur dans une police proportionnelle.

La lettre « i » est généralement plus étroite que la lettre « M ». Une animation basée sur `ch` et `steps()` peut donc révéler les caractères de manière irrégulière.

Pour un résultat prévisible, utilisez une police monospace.

Si votre identité visuelle exige une police proportionnelle, envisagez une animation pilotée par JavaScript ou une révélation par mot.

### Erreur 2 : animer un texte trop long

Une animation de quinze secondes sur une phrase importante peut frustrer les visiteurs.

Le contenu essentiel doit être disponible rapidement.

Pour une phrase longue, une apparition progressive du bloc entier peut être plus appropriée qu'une simulation de saisie.

### Erreur 3 : multiplier les animations

Une page contenant plusieurs curseurs clignotants, des textes qui s'effacent et des titres qui apparaissent successivement peut devenir fatigante.

Conservez généralement un seul point focal animé par section.

### Erreur 4 : ignorer les préférences utilisateur

Une animation qui fonctionne visuellement n'est pas nécessairement confortable pour tout le monde.

Prévoyez systématiquement une alternative sans mouvement lorsque l'effet n'est pas indispensable à la compréhension.

### Erreur 5 : confondre CSS web et React Native

Le CSS classique n'est pas directement applicable à une application React Native native.

Si vous partez d'un design trouvé sur VP0.com, vous pouvez conserver l'intention visuelle de l'effet, mais son implémentation doit utiliser les mécanismes adaptés à React Native.

Cela évite de copier un exemple web qui ne fonctionnera pas dans le projet mobile.

## Comment demander à une IA de générer un effet machine à écrire ?

Les assistants de programmation peuvent accélérer la création d'animations CSS, à condition de leur donner des contraintes précises.

Une demande vague comme « ajoute une animation moderne » peut produire un effet trop complexe ou difficile à maintenir.

Précisez le type d'animation, la durée, les contraintes responsive et les exigences d'accessibilité.

### Prompt 1 : effet simple pour un portfolio

> Crée un effet machine à écrire en HTML et CSS uniquement pour le titre de mon portfolio. Utilise une police monospace, un curseur clignotant et une animation de trois secondes. Le texte doit rester visible à la fin. Ajoute une règle prefers-reduced-motion et évite tout débordement sur mobile. Fournis le HTML et le CSS séparément.

Ce prompt convient à un premier prototype.

### Prompt 2 : animation pour une page SaaS

> Crée une section hero moderne avec un titre statique et une courte phrase animée en CSS. Utilise steps() pour simuler une saisie au clavier. Le design doit être minimaliste, responsive et accessible. L'animation doit s'exécuter une seule fois. N'utilise aucune bibliothèque externe.

Cette version limite les dépendances et facilite l'intégration.

### Prompt 3 : interface de chatbot

> Construis une interface de chat en HTML, CSS et JavaScript. Lorsqu'une nouvelle réponse est disponible, affiche progressivement son texte avec un effet de saisie. Prévois une option permettant de désactiver l'animation. Assure-toi que le contenu complet reste accessible aux lecteurs d'écran et que les réponses longues peuvent passer à la ligne.

Dans ce dernier cas, JavaScript est justifié puisque le texte est dynamique.

Si vous créez une application mobile avec un outil d'IA, vous pouvez également partir d'un design de VP0.com, puis demander à l'assistant d'ajouter une animation de saisie adaptée au framework utilisé.

## À retenir : quel effet machine à écrire CSS choisir ?

Pour un titre court, l'association `width`, `overflow: hidden` et `steps()` reste une méthode simple et efficace.

Si vous souhaitez uniquement un curseur clignotant, une animation d'opacité suffit.

Pour un texte qui s'écrit puis s'efface, les keyframes permettent de créer une boucle sans JavaScript. En revanche, pour plusieurs phrases dynamiques ou une véritable interface de chat, JavaScript offre davantage de contrôle.

Le plus important est de ne pas considérer l'animation comme un élément isolé. Elle doit renforcer la lisibilité, respecter le style de l'interface et fonctionner correctement sur mobile.

Pour les applications iOS créées avec l'IA, [VP0.com](https://vp0.com) peut vous aider à choisir une base visuelle cohérente avant d'intégrer vos interactions personnalisées. La bibliothèque est gratuite et son interface est en anglais.

Commencez par un exemple court, testez son comportement sur plusieurs écrans, puis ajustez la vitesse et le curseur. Une animation discrète et maîtrisée donne généralement un meilleur résultat qu'un effet spectaculaire répété partout.

## Questions fréquentes

### Peut-on créer un effet machine à écrire uniquement avec CSS ?

Oui. Pour un texte fixe, vous pouvez utiliser `@keyframes`, `steps()`, `overflow: hidden` et une largeur progressive. JavaScript n'est pas nécessaire pour une animation simple.

### Comment faire clignoter le curseur en CSS ?

Utilisez une bordure droite ou un pseudo-élément, puis animez son opacité ou sa couleur avec `@keyframes`. Une animation `step-end` permet d'obtenir un clignotement net.

### Comment répéter un effet machine à écrire à l'infini ?

Ajoutez `infinite` à la propriété `animation`. Pour simuler l'effacement, définissez plusieurs étapes dans les keyframes afin de réduire la largeur après une pause.

### Pourquoi mon animation CSS coupe-t-elle le texte ?

Le problème vient souvent d'une largeur exprimée en `ch` qui ne correspond pas à la longueur réelle du texte, d'une police proportionnelle ou d'un conteneur trop étroit. Vérifiez également `white-space` et `overflow`.

### L'effet machine à écrire fonctionne-t-il sur mobile ?

Oui, mais il faut adapter la taille du texte et éviter les phrases trop longues sans retour à la ligne. Une version sans animation est également recommandée pour les utilisateurs qui préfèrent réduire les mouvements.

### Peut-on utiliser cet effet dans une application React Native ?

Oui, le principe visuel peut être reproduit, mais pas en copiant directement les animations CSS du navigateur. Les projets Expo React Native, comme les designs proposés sur VP0.com, nécessitent une implémentation adaptée à leur environnement.
