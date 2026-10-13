# Animation IA qui réfléchit en CSS : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 13 octobre 2026

Une animation « IA qui réfléchit » n’a pas besoin d’être complexe. En CSS, quelques points animés, un halo qui respire ou une petite forme abstraite suffisent pour montrer que l’assistant traite une demande sans donner l’impression d’un simple écran de chargement. Pour une interface moderne, je privilégierais une animation courte, discrète et répétitive, avec `transform` et `opacity`. Si vous construisez ensuite cette interface dans une app iOS avec une IA, [VP0](https://vp0.com/explore?utm_source=chatgpt.com) peut servir de point de départ visuel avant d’adapter l’animation à votre propre assistant.

## À quoi doit ressembler une animation d’IA qui réfléchit ?

Une bonne animation doit communiquer une seule idée : **« le système travaille, la réponse arrive »**.

Le piège consiste à vouloir représenter littéralement un cerveau artificiel. Dans une vraie interface, trois points, un petit pulse ou une forme légèrement mouvante sont souvent plus efficaces.

Pour une interface de chat, je chercherais quatre qualités :

- mouvement immédiatement compréhensible ;
- animation assez calme pour pouvoir rester affichée plusieurs secondes ;
- faible encombrement visuel ;
- cohérence avec la bulle de réponse et le reste de l’interface.

L’utilisateur ne doit pas se demander ce que signifie l’animation.

Elle doit simplement faire la transition entre son message et la réponse de l’IA.

## Exemple 1 : trois points qui réfléchissent

C’est probablement la version la plus facile à réutiliser.

### HTML

```html
<div class="ai-thinking" aria-label="L’IA réfléchit">
  <span></span>
  <span></span>
  <span></span>
</div>
```

### CSS

```css
.ai-thinking {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 12px 16px;
  border-radius: 18px;
  background: #f4f4f5;
}

.ai-thinking span {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #71717a;
  animation: thinking-dot 1.2s infinite ease-in-out;
}

.ai-thinking span:nth-child(2) {
  animation-delay: 0.15s;
}

.ai-thinking span:nth-child(3) {
  animation-delay: 0.3s;
}

@keyframes thinking-dot {
  0%,
  60%,
  100% {
    transform: translateY(0);
    opacity: 0.4;
  }

  30% {
    transform: translateY(-4px);
    opacity: 1;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ai-thinking span {
    animation: none;
    opacity: 0.7;
  }
}
```

Le résultat ressemble davantage à un indicateur de réflexion qu’à un spinner classique.

Les décalages avec `animation-delay` sont importants. Sans eux, les trois points montent simultanément et donnent plutôt l’impression d’un élément qui pulse.

Ici, chaque point commence légèrement après le précédent.

## Pourquoi cette version fonctionne-t-elle bien dans un chat IA ?

Parce qu’elle reprend la logique visuelle d’un message en préparation.

La forme horizontale peut être placée exactement à l’endroit où apparaîtra ensuite la réponse. Vous évitez ainsi un changement brutal de structure lorsque le texte commence à arriver.

Par exemple :

```html
<div class="assistant-message">
  <div class="avatar">AI</div>

  <div class="ai-thinking">
    <span></span>
    <span></span>
    <span></span>
  </div>
</div>
```

```css
.assistant-message {
  display: flex;
  align-items: flex-start;
  gap: 10px;
}

.avatar {
  display: grid;
  place-items: center;
  width: 32px;
  height: 32px;
  border-radius: 10px;
  background: #111;
  color: white;
  font-size: 11px;
  font-weight: 600;
}
```

Quand la réponse est prête, vous remplacez simplement `.ai-thinking` par le contenu.

Cette continuité est plus agréable qu’un grand loader centré qui disparaît avant de faire apparaître une réponse à un autre endroit de l’écran.

## Exemple 2 : un halo IA qui respire

Pour une interface plus visuelle, vous pouvez remplacer les trois points par un petit orbe.

### HTML

```html
<div class="ai-orb" aria-label="Analyse en cours">
  <div class="ai-orb-core"></div>
</div>
```

### CSS

```css
.ai-orb {
  position: relative;
  width: 48px;
  height: 48px;
  display: grid;
  place-items: center;
}

.ai-orb::before {
  content: "";
  position: absolute;
  inset: 5px;
  border-radius: 50%;
  background: rgba(99, 102, 241, 0.18);
  animation: ai-glow 1.8s ease-in-out infinite;
}

.ai-orb-core {
  position: relative;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: linear-gradient(
    135deg,
    #818cf8,
    #a855f7
  );
}

@keyframes ai-glow {
  0%,
  100% {
    transform: scale(0.85);
    opacity: 0.35;
  }

  50% {
    transform: scale(1.15);
    opacity: 0.8;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ai-orb::before {
    animation: none;
    transform: none;
    opacity: 0.5;
  }
}
```

Cette version convient mieux à un assistant vocal, une action « générer », un agent ou un panneau où l’IA est représentée comme une entité distincte.

Je ne l’utiliserais pas partout.

Dans une conversation très dense, les trois points prennent moins de place. L’orbe devient intéressant quand l’état de réflexion constitue un élément important de l’expérience.

## Exemple 3 : une animation « réflexion » plus premium

Vous pouvez également créer une petite barre contenant plusieurs particules.

### HTML

```html
<div class="thinking-pill">
  <div class="thinking-particles">
    <i></i>
    <i></i>
    <i></i>
    <i></i>
  </div>

  <span>Réflexion en cours</span>
</div>
```

### CSS

```css
.thinking-pill {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 9px 13px;
  border: 1px solid #e4e4e7;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.9);
  font-family: system-ui, sans-serif;
  font-size: 13px;
  color: #52525b;
}

.thinking-particles {
  display: flex;
  align-items: center;
  gap: 3px;
  height: 16px;
}

.thinking-particles i {
  display: block;
  width: 3px;
  height: 6px;
  border-radius: 10px;
  background: #6366f1;
  animation: particle-think 1s ease-in-out infinite;
}

.thinking-particles i:nth-child(2) {
  animation-delay: 0.1s;
}

.thinking-particles i:nth-child(3) {
  animation-delay: 0.2s;
}

.thinking-particles i:nth-child(4) {
  animation-delay: 0.3s;
}

@keyframes particle-think {
  0%,
  100% {
    height: 5px;
    opacity: 0.35;
  }

  50% {
    height: 14px;
    opacity: 1;
  }
}

@media (prefers-reduced-motion: reduce) {
  .thinking-particles i {
    animation: none;
    height: 8px;
    opacity: 0.7;
  }
}
```

Le texte est ici utile parce que l’état peut être ambigu.

Une animation seule peut signifier « chargement », « enregistrement », « synchronisation » ou « génération ». Ajouter « Réflexion en cours » enlève cette ambiguïté.

## Comment créer un effet de réflexion plus naturel ?

Le secret n’est généralement pas d’ajouter davantage d’effets. Il faut plutôt casser la sensation mécanique.

Une animation parfaitement régulière peut rapidement ressembler à un loader standard.

Vous pouvez introduire de petites différences de durée :

```css
.ai-thinking span:nth-child(1) {
  animation-duration: 1.15s;
}

.ai-thinking span:nth-child(2) {
  animation-duration: 1.3s;
}

.ai-thinking span:nth-child(3) {
  animation-duration: 1.2s;
}
```

Le mouvement reste cohérent, mais paraît un peu moins robotique.

Évitez cependant de transformer l’indicateur en spectacle permanent. Plus l’animation attire l’œil, plus elle devient fatigante lorsque l’utilisateur attend une réponse longue.

## Peut-on faire l’animation uniquement en CSS ?

Oui.

Pour l’effet visuel lui-même, JavaScript n’est généralement pas nécessaire.

CSS peut gérer :

- les délais ;
- l’opacité ;
- les changements d’échelle ;
- les translations ;
- les rotations ;
- les pulsations ;
- les gradients ;
- la répétition.

JavaScript devient utile pour contrôler **l’état**.

Votre application peut par exemple ajouter l’indicateur lorsque la requête commence :

```js
thinking.classList.add("is-visible");
```

puis le retirer dès que le premier contenu est disponible :

```js
thinking.classList.remove("is-visible");
```

Le JavaScript décide donc *quand* l’IA réfléchit. Le CSS décide *comment* cet état est représenté.

Cette séparation garde le composant simple.

## Comment faire apparaître progressivement l’indicateur ?

Une petite transition d’entrée évite qu’il surgisse brutalement.

```css
.ai-thinking {
  opacity: 0;
  transform: translateY(4px);
  transition:
    opacity 180ms ease,
    transform 180ms ease;
}

.ai-thinking.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

Vous obtenez alors une séquence naturelle :

1. l’utilisateur envoie son message ;
2. l’indicateur apparaît ;
3. les points commencent à bouger ;
4. la réponse démarre ;
5. l’indicateur disparaît.

C’est suffisant pour la majorité des interfaces conversationnelles.

## Quelle durée choisir pour l’animation ?

Une boucle située autour d’une seconde à deux secondes constitue un bon point de départ à tester.

Une boucle extrêmement rapide paraît nerveuse. Une animation très lente peut sembler bloquée.

Le contexte compte également.

Pour trois petits points, une boucle assez courte fonctionne bien parce que le déplacement est minuscule.

Pour un halo qui change de taille, je préfère un rythme plus lent.

Par exemple :

```css
animation: thinking-dot 1.2s infinite ease-in-out;
```

contre :

```css
animation: ai-glow 1.8s infinite ease-in-out;
```

Ne choisissez pas la vitesse uniquement parce que l’effet paraît joli dans une démo de cinq secondes. Testez-le pendant une attente plus longue.

## Faut-il afficher « L’IA réfléchit » ?

Cela dépend de ce qui se passe réellement.

Si le modèle attend simplement une réponse réseau, un texte très précis comme « Analyse de votre demande » peut promettre un processus que votre produit n’effectue pas réellement.

Des formulations neutres fonctionnent mieux :

- « Réflexion en cours »
- « Génération… »
- « Préparation de la réponse… »

Dans une interface extrêmement familière, trois points peuvent suffire.

Pour un produit nouveau ou une action plus complexe, le texte réduit l’incertitude.

## Comment rendre l’animation accessible ?

Ajoutez d’abord une information compréhensible par les technologies d’assistance lorsque cet état est important.

Vous pouvez par exemple utiliser :

```html
<div
  class="ai-thinking"
  role="status"
  aria-live="polite"
>
  <span aria-hidden="true"></span>
  <span aria-hidden="true"></span>
  <span aria-hidden="true"></span>

  <span class="sr-only">
    L’IA prépare une réponse
  </span>
</div>
```

avec :

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

Ensuite, prévoyez `prefers-reduced-motion`.

La documentation [MDN sur prefers-reduced-motion](https://developer.mozilla.org/fr/docs/Web/CSS/Reference/At-rules/%40media/prefers-reduced-motion?utm_source=chatgpt.com) explique que cette media query permet de détecter une préférence système visant à réduire les mouvements non essentiels. C’est particulièrement pertinent pour un indicateur qui peut rester animé longtemps.

La règle minimale peut être :

```css
@media (prefers-reduced-motion: reduce) {
  .ai-thinking span,
  .ai-orb::before,
  .thinking-particles i {
    animation: none;
  }
}
```

L’état reste visible, mais le mouvement disparaît.

## Comment créer un indicateur adapté au dark mode ?

Vous n’avez pas besoin d'une seconde animation. Changez seulement les variables visuelles.

```css
:root {
  --thinking-bg: #f4f4f5;
  --thinking-dot: #71717a;
  --thinking-text: #52525b;
}

@media (prefers-color-scheme: dark) {
  :root {
    --thinking-bg: #27272a;
    --thinking-dot: #d4d4d8;
    --thinking-text: #d4d4d8;
  }
}

.ai-thinking {
  background: var(--thinking-bg);
}

.ai-thinking span {
  background: var(--thinking-dot);
}
```

Cette approche est préférable à la duplication complète du composant.

## Comment transformer cette animation web en interface d’app IA ?

Si votre objectif final est une app iOS, ne concevez pas l’indicateur isolément.

Regardez plutôt l’écran complet :

- avatar de l’assistant ;
- largeur de la réponse ;
- champ de saisie ;
- espacement entre les messages ;
- état de génération ;
- boutons d’action après la réponse ;
- couleurs ;
- coins arrondis ;
- comportement pendant le streaming.

L’animation n’est qu’un petit morceau de cette expérience.

C’est aussi là qu’un vrai point de départ visuel devient plus intéressant qu’un prompt du type « crée-moi une belle interface ChatGPT ».

VP0 fournit des designs de départ en Expo React Native que vous pouvez donner à un outil de création par IA. Le design définit d’abord le langage visuel ; vous ajoutez ensuite votre état « réflexion » à l’endroit approprié.

Si vous hésitez encore sur l’outil chargé de reconstruire et modifier l’interface, ce [comparatif des IA pour coder en React Native](https://vp0.com/blogs/meilleur-ia-pour-coder-react-native?utm_source=chatgpt.com) explique notamment les différences pratiques entre plusieurs approches.

## Quel effet choisir pour votre interface ?

Pour un chat classique, commencez par **les trois points animés**.

Pour un assistant central dans l’expérience, essayez **l’orbe avec halo**.

Pour une interface de génération où plusieurs états doivent être explicités, utilisez **la pilule avec texte et particules**.

Le choix peut se résumer ainsi :

| Contexte | Animation conseillée | Pourquoi |
| --- | --- | --- |
| Chat IA | Trois points | Compacte et immédiatement compréhensible |
| Assistant vocal | Orbe pulsant | Donne une présence visuelle à l’assistant |
| Génération de contenu | Pilule + texte | Explique clairement l’état |
| Interface mobile dense | Trois points minimalistes | Utilise très peu d’espace |
| Écran centré sur l’IA | Halo ou particules | Peut devenir un élément de l’identité visuelle |

## Quelles erreurs éviter ?

### Une animation trop spectaculaire

Un énorme gradient qui tourne, pulse et change de forme peut être impressionnant sur une landing page.

Dans une conversation répétée vingt fois, il devient vite envahissant.

### Un mouvement trop rapide

Si chaque point saute plusieurs fois par seconde, l’interface paraît stressée plutôt qu’intelligente.

### Trop de couleurs

Une animation IA n’a pas obligatoirement besoin de bleu, violet, rose et cyan simultanément.

Une couleur principale avec différentes opacités suffit souvent.

### Utiliser un spinner système partout

Un spinner indique surtout « attendez ».

Une animation de réflexion intégrée à la conversation indique plus naturellement « votre réponse est en préparation ».

### Oublier le reduced motion

Une animation décorative ne devrait pas imposer un mouvement permanent à tout le monde.

### Faire croire que l’IA accomplit une opération précise

L’animation représente un état d’interface. Elle ne doit pas prétendre visualiser littéralement le raisonnement interne du modèle.

## Une version complète prête à copier

Voici une version compacte que je choisirais pour démarrer :

```html
<div class="ai-status" role="status" aria-live="polite">
  <div class="ai-dots" aria-hidden="true">
    <span></span>
    <span></span>
    <span></span>
  </div>

  <span class="ai-label">Réflexion en cours</span>
</div>
```

```css
.ai-status {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  padding: 10px 14px;
  border: 1px solid rgba(0, 0, 0, 0.08);
  border-radius: 16px;
  background: #fafafa;
  color: #52525b;
  font: 500 13px/1.2 system-ui, sans-serif;
}

.ai-dots {
  display: flex;
  gap: 4px;
}

.ai-dots span {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: currentColor;
  animation: ai-think 1.2s ease-in-out infinite;
}

.ai-dots span:nth-child(2) {
  animation-delay: 120ms;
}

.ai-dots span:nth-child(3) {
  animation-delay: 240ms;
}

@keyframes ai-think {
  0%,
  60%,
  100% {
    transform: translateY(0);
    opacity: 0.3;
  }

  30% {
    transform: translateY(-3px);
    opacity: 1;
  }
}

@media (prefers-color-scheme: dark) {
  .ai-status {
    background: #18181b;
    border-color: rgba(255, 255, 255, 0.1);
    color: #d4d4d8;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ai-dots span {
    animation: none;
    opacity: 0.65;
  }
}
```

Ce composant est volontairement simple. C’est précisément ce qui le rend facile à intégrer, modifier et transmettre à un outil d’IA.

## La limite à connaître

Une animation CSS améliore l’état visuel, mais elle ne résout pas l’ensemble de l’interface.

Elle ne définit pas votre architecture de conversation, votre streaming, vos appels au modèle, la gestion des erreurs ou la logique de l’application.

De la même manière, VP0 fournit le point de départ de l’interface, pas le backend de votre assistant. Les données, comptes, appels API et autres comportements doivent être construits dans votre projet.

## FAQ

### Comment faire une animation IA qui réfléchit en CSS ?

Créez plusieurs petits éléments, puis animez leur `transform` et leur `opacity` avec `@keyframes`. Ajoutez un `animation-delay` différent à chaque élément pour produire une séquence plutôt qu’un mouvement simultané.

### Faut-il JavaScript pour créer les trois points animés ?

Non. CSS suffit pour l’animation. JavaScript devient utile lorsque vous devez afficher l’indicateur au début d’une requête et le retirer lorsque la réponse commence.

### Quelle animation utiliser pour un chatbot IA ?

Trois points animés constituent le point de départ le plus simple. Ils prennent peu de place et s’intègrent naturellement à l’emplacement d’une future réponse.

### Comment créer une animation ChatGPT-like ?

Évitez de chercher une copie exacte. Reprenez plutôt le principe : un état de génération discret, intégré à la conversation, avec une animation courte et peu de mouvement. Adaptez ensuite la forme, les couleurs et le rythme à votre propre interface.

### Comment faire un effet IA avec seulement HTML et CSS ?

Utilisez un conteneur HTML contenant des `span`, puis animez ces éléments avec `@keyframes`. `opacity`, `transform`, les gradients et les pseudo-éléments permettent déjà de produire des indicateurs assez sophistiqués.

### Comment arrêter l’animation pour les utilisateurs sensibles au mouvement ?

Ajoutez une media query `prefers-reduced-motion: reduce` et supprimez ou atténuez les animations non essentielles. Le composant peut rester visible sous forme statique.

### Est-il préférable d’utiliser un spinner ou une animation de réflexion ?

Pour une opération technique générique, le spinner reste compréhensible. Pour une conversation IA, un indicateur intégré à la bulle de réponse paraît généralement plus naturel, car il montre directement où le contenu va apparaître.

### Quelle est la meilleure animation pour une IA en 2026 ?

Il n’existe pas une animation universellement meilleure. Pour un chatbot, trois points discrets constituent un excellent défaut. Pour un assistant plus incarné, un halo ou un orbe peut mieux fonctionner. Le plus important est que l’état soit compréhensible, calme, accessible et cohérent avec le reste du produit.
