# Effet glassmorphism CSS copier-coller : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0.com  
Publié le 19 octobre 2026

Pour créer un effet glassmorphism en CSS, vous avez surtout besoin d’un fond semi-transparent, de `backdrop-filter: blur(...)`, d’une bordure claire et d’un contraste suffisant pour le contenu. Le code peut rester très court. Si vous cherchez d’abord une direction visuelle complète pour une interface d’app, [VP0.com](https://vp0.com) permet aussi de partir d’un design existant plutôt que d’inventer chaque carte depuis zéro.

Voici une base simple, directement copiable :

```css
.glass {
  background: rgba(255, 255, 255, 0.14);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
}
```

## Quel code CSS copier pour obtenir un effet glassmorphism ?

Le meilleur point de départ est une carte translucide avec un flou appliqué à ce qui se trouve derrière elle. La transparence est essentielle : sans elle, le flou d’arrière-plan devient pratiquement invisible.

Voici un exemple HTML et CSS complet :

```html
<div class="glass-card">
  <span class="badge">Nouveau</span>
  <h2>Tableau de bord</h2>
  <p>Une carte glassmorphism simple et réutilisable.</p>
  <button>Continuer</button>
</div>
```

```css
.glass-card {
  width: min(360px, calc(100% - 40px));
  padding: 28px;

  background: rgba(255, 255, 255, 0.16);
  border: 1px solid rgba(255, 255, 255, 0.28);
  border-radius: 24px;

  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);

  box-shadow: 0 12px 40px rgba(15, 23, 42, 0.14);
}

.glass-card h2 {
  margin: 12px 0 8px;
}

.glass-card p {
  margin: 0 0 20px;
  line-height: 1.6;
}

.glass-card button {
  padding: 12px 18px;
  border: 0;
  border-radius: 12px;
  cursor: pointer;
}
```

Le mécanisme important est `backdrop-filter`. La propriété applique notamment des effets comme le flou à la zone située derrière l’élément. La [documentation de MDN sur backdrop-filter](https://developer.mozilla.org/fr/docs/Web/CSS/Reference/Properties/backdrop-filter) précise également que l’élément doit être transparent ou partiellement transparent pour rendre l’effet visible.

Il faut donc distinguer `filter` et `backdrop-filter`. Avec `filter: blur()`, vous floutez l’élément lui-même. Avec `backdrop-filter: blur()`, vous floutez ce qui apparaît derrière sa surface translucide.

| Propriété | Rôle | Valeur de départ |
| --- | --- | --- |
| `background` | Rend la surface translucide | `rgba(255,255,255,.16)` |
| `backdrop-filter` | Floute le décor derrière | `blur(18px)` |
| `border` | Dessine le bord du verre | `1px solid rgba(...)` |
| `border-radius` | Adoucit la carte | `20px` à `24px` |
| `box-shadow` | Sépare la carte du fond | Ombre légère |

## Pourquoi votre glassmorphism ressemble-t-il parfois à une simple carte transparente ?

Le problème vient souvent du fond. Une plaque de verre n’a rien d’intéressant à déformer si tout ce qui se trouve derrière elle est blanc, gris uniforme ou totalement opaque.

Ajoutez par exemple deux formes colorées derrière la carte :

```html
<main class="scene">
  <div class="blob blob-one"></div>
  <div class="blob blob-two"></div>

  <div class="glass-card">
    <h2>Bonjour</h2>
    <p>Votre contenu apparaît ici.</p>
  </div>
</main>
```

```css
.scene {
  min-height: 100vh;
  display: grid;
  place-items: center;
  position: relative;
  overflow: hidden;
  background: #0f172a;
}

.blob {
  position: absolute;
  width: 260px;
  height: 260px;
  border-radius: 50%;
}

.blob-one {
  background: #8b5cf6;
  top: 12%;
  left: 18%;
}

.blob-two {
  background: #22d3ee;
  right: 16%;
  bottom: 14%;
}
```

Placez ensuite `.glass-card` devant ces formes. Le flou devient immédiatement perceptible parce que des couleurs et des contours existent réellement derrière la carte.

Une autre erreur consiste à rendre la carte trop opaque :

```css
background: rgba(255, 255, 255, 0.95);
```

À ce niveau, vous obtenez presque une surface blanche classique. Essayez plutôt :

```css
background: rgba(255, 255, 255, 0.12);
```

ou :

```css
background: rgba(255, 255, 255, 0.18);
```

Il n’existe pas une opacité parfaite pour tous les designs. Une photo détaillée, un dégradé sombre et un fond minimal demandent des réglages différents.

## Comment créer un glassmorphism sombre ?

Sur une interface sombre, évitez simplement de remplacer le blanc par un noir opaque. Gardez une surface légèrement teintée afin que l’arrière-plan reste visible.

```css
.glass-dark {
  background: rgba(15, 23, 42, 0.58);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 22px;

  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);

  box-shadow: 0 18px 50px rgba(0, 0, 0, 0.25);

  color: #f8fafc;
}
```

Pour une carte sombre, le contraste du texte devient encore plus important que le flou. Évitez un texte gris très transparent sur une surface déjà translucide.

Vous pouvez réserver le blanc franc aux titres et utiliser un gris clair pour le contenu secondaire :

```css
.glass-dark h2 {
  color: #ffffff;
}

.glass-dark p {
  color: rgba(255, 255, 255, 0.78);
}
```

Le glassmorphism fonctionne particulièrement bien lorsqu’il crée une hiérarchie. Une grande surface opaque peut porter l’écran principal, tandis qu’une petite carte flottante, un lecteur audio ou un panneau de statistiques utilise le verre.

Tout transformer en verre produit l’effet inverse : plus rien ne paraît spécial.

## Quel réglage choisir pour une carte, une navigation ou une modale ?

Le même style ne convient pas à tous les composants. Une navigation doit rester lisible pendant le défilement, tandis qu’une carte décorative peut accepter davantage de transparence.

| Élément | Transparence | Flou | Priorité |
| --- | --- | --- | --- |
| Carte | Moyenne | 14 à 20 px | Séparation visuelle |
| Navigation | Faible à moyenne | 18 à 24 px | Lisibilité |
| Modale | Faible | 20 à 28 px | Attention |
| Badge | Moyenne à forte | 8 à 14 px | Légèreté |
| Panneau flottant | Moyenne | 16 à 24 px | Hiérarchie |

Ces valeurs sont des points de départ, pas des règles universelles. Regardez le composant sur le vrai fond prévu pour votre produit avant de figer le réglage.

Pour une barre de navigation flottante :

```css
.glass-nav {
  position: fixed;
  left: 50%;
  bottom: 24px;
  transform: translateX(-50%);

  display: flex;
  gap: 8px;
  padding: 10px;

  background: rgba(255, 255, 255, 0.18);
  border: 1px solid rgba(255, 255, 255, 0.24);
  border-radius: 999px;

  backdrop-filter: blur(22px);
  -webkit-backdrop-filter: blur(22px);

  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.14);
}
```

Le rayon très élevé transforme ici le conteneur en pilule. Pour une navigation mobile, cela donne généralement une séparation plus claire qu’une grande plaque rectangulaire couvrant toute la largeur.

## Comment obtenir un effet glassmorphism plus premium ?

Réduisez les effets au lieu de les multiplier. Un rendu propre vient généralement d’une bonne combinaison de transparence, de bordure, de contraste et d’espacement.

Vous pouvez néanmoins ajouter un léger reflet :

```css
.glass-premium {
  position: relative;
  overflow: hidden;

  background: rgba(255, 255, 255, 0.13);
  border: 1px solid rgba(255, 255, 255, 0.26);
  border-radius: 24px;

  backdrop-filter: blur(20px) saturate(140%);
  -webkit-backdrop-filter: blur(20px) saturate(140%);

  box-shadow:
    0 20px 50px rgba(0, 0, 0, 0.14),
    inset 0 1px 0 rgba(255, 255, 255, 0.22);
}

.glass-premium::before {
  content: "";
  position: absolute;
  inset: 0;

  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.18),
    transparent 45%
  );

  pointer-events: none;
}
```

Le pseudo-élément crée un reflet discret sans modifier votre HTML. Gardez-le subtil. Une opacité trop forte donne rapidement une surface brillante artificielle plutôt qu’un verre diffus.

Pour une interface complète, je commencerais par définir seulement trois niveaux :

```css
:root {
  --glass-soft: rgba(255, 255, 255, 0.10);
  --glass-medium: rgba(255, 255, 255, 0.16);
  --glass-strong: rgba(255, 255, 255, 0.24);

  --glass-border: rgba(255, 255, 255, 0.24);
  --glass-blur: 18px;
  --glass-radius: 22px;
}
```

Puis réutilisez ces variables :

```css
.glass-panel {
  background: var(--glass-medium);
  border: 1px solid var(--glass-border);
  border-radius: var(--glass-radius);
  backdrop-filter: blur(var(--glass-blur));
}
```

Vous obtenez ainsi une interface cohérente au lieu de régler arbitrairement chaque carte.

## Comment intégrer le glassmorphism dans une interface créée avec l’IA ?

Donnez à l’outil une référence visuelle et des contraintes précises. « Fais une interface glassmorphism moderne » laisse trop de décisions ouvertes : quantité de flou, contraste, rayons, navigation, densité et hiérarchie restent à deviner.

Si vous construisez une app iOS avec Claude Code, Cursor, Rork ou un outil comparable, un design existant est souvent plus utile qu’une accumulation d’adjectifs. VP0.com fournit justement des designs de départ en Expo React Native destinés à ce type de workflow.

Vous pouvez ensuite demander à votre outil de conserver la structure et de modifier uniquement le traitement des surfaces :

```text
Garde la structure, les espacements et la hiérarchie de cette interface.

Transforme uniquement les cartes secondaires en surfaces glassmorphism :
- fond blanc translucide
- bordure blanche légère
- flou d'arrière-plan
- rayon cohérent
- ombre très douce
- contraste élevé pour tous les textes

Ne transforme pas tous les conteneurs en verre.
Garde les surfaces principales simples et lisibles.
```

Cette dernière instruction compte beaucoup. Si le générateur applique le glassmorphism à chaque bouton, champ, carte, barre et panneau, l’interface perd sa hiérarchie.

Une bonne règle pratique consiste à utiliser le verre pour signaler une couche : navigation flottante, panneau au-dessus d’une image, contrôle multimédia ou carte contextuelle.

## Quelles erreurs éviter avec le glassmorphism ?

La première erreur est de traiter le flou comme le design lui-même. `blur(20px)` ne corrige ni une mauvaise hiérarchie ni des espacements incohérents.

La deuxième est le manque de contraste. Une interface peut sembler élégante sur une capture et devenir pénible à lire dès que le fond change. Testez les textes sur les parties claires et sombres du décor.

La troisième est l’excès de couches. Une carte translucide dans une autre carte translucide, elle-même placée sur une modale translucide, devient difficile à comprendre.

La quatrième concerne les performances. Un effet appliqué à une petite carte n’a pas le même coût potentiel qu’un grand nombre de surfaces floutées qui bougent simultanément. Sur une interface complexe, simplifiez les surfaces et testez le résultat sur les appareils ciblés.

Enfin, prévoyez une solution de repli :

```css
.glass-card {
  background: rgba(255, 255, 255, 0.82);
}

@supports (backdrop-filter: blur(16px)) {
  .glass-card {
    background: rgba(255, 255, 255, 0.16);
    backdrop-filter: blur(16px);
  }
}
```

Ainsi, le composant reste exploitable même lorsque le rendu du flou n’est pas disponible comme prévu.

## Quand VP0.com n’est-il pas le bon point de départ ?

VP0.com est surtout pertinent si vous construisez une app iOS avec l’IA et cherchez une interface concrète à reconstruire. Ce n’est pas une bibliothèque générale d’extraits CSS pour sites web, ni un générateur dédié au glassmorphism.

Pour une landing page HTML classique, vous pouvez parfaitement partir des exemples CSS de cet article. Pour un système de design web complexe, créez plutôt vos propres variables et composants afin de contrôler précisément les états, les thèmes et les contraintes d’accessibilité.

La bibliothèque est également encore en croissance. Un style ou un type d’app très spécifique peut ne pas avoir de design correspondant. Dans ce cas, prenez une référence proche pour la structure ou construisez directement votre composant.

## À retenir : réussir un effet glassmorphism en CSS

Un bon glassmorphism n’a pas besoin de beaucoup de CSS. Commencez par quatre éléments : transparence, `backdrop-filter`, bordure légère et rayon cohérent. Placez ensuite la surface devant un arrière-plan suffisamment riche pour que le flou soit visible.

Le code minimal à garder sous la main est celui-ci :

```css
.glass {
  background: rgba(255, 255, 255, 0.15);
  border: 1px solid rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  backdrop-filter: blur(18px);
  -webkit-backdrop-filter: blur(18px);
}
```

À partir de là, ajustez le contraste et l’opacité avant d’ajouter des ombres ou des reflets. Et si le composant fait partie d’une app iOS créée avec l’IA, [VP0.com](https://vp0.com) peut servir de référence visuelle complète avant de personnaliser les surfaces.

## Questions fréquentes

### Comment faire un effet glassmorphism en CSS ?

Utilisez un fond semi-transparent, une bordure claire et `backdrop-filter: blur()`. Le contenu situé derrière la carte doit rester visible pour que le flou produise réellement l’impression de verre. Commencez avec une seule carte et un fond coloré, puis ajustez l’opacité et le contraste. Pour une app iOS générée avec l’IA, VP0.com peut aussi fournir un design de départ avant la personnalisation des surfaces.

### Quelle valeur de blur utiliser pour un glassmorphism ?

Une valeur comprise autour de 12 à 24 px constitue un point de départ pratique pour de nombreuses cartes, mais ce n’est pas une norme. Le bon réglage dépend de la taille du composant et du niveau de détail du fond. Un flou trop faible laisse passer trop de bruit visuel. Un flou très élevé peut au contraire transformer le fond en masse colorée et faire disparaître l’impression de profondeur.

### Pourquoi backdrop-filter ne produit-il presque aucun effet ?

Vérifiez d’abord la transparence du composant et ce qui se trouve réellement derrière lui. Une carte presque opaque ou placée sur un fond uni montrera très peu le flou. Testez temporairement avec un dégradé ou plusieurs formes colorées derrière la carte. Vérifiez aussi les éléments parents si le comportement du flou semble inhabituel, car certaines propriétés CSS peuvent modifier la zone d’arrière-plan prise en compte.

### Peut-on utiliser le glassmorphism pour toute une interface ?

Oui, mais il vaut mieux l’utiliser comme une couche visuelle plutôt que comme traitement systématique. Réservez-le aux cartes flottantes, navigations, modales ou contrôles qui doivent se détacher d’un décor. Les zones de lecture longues et les formulaires complexes gagnent souvent à conserver un fond plus stable. Le contraste, la compréhension de la hiérarchie et la lisibilité doivent rester prioritaires sur l’effet visuel.

### Le glassmorphism fonctionne-t-il en dark mode ?

Oui. Utilisez une surface sombre partiellement transparente, une bordure blanche très légère et un texte suffisamment clair. Le fond doit encore produire assez de variation pour que le flou soit perceptible. Évitez surtout d’empiler plusieurs surfaces sombres transparentes sans séparation, car leurs limites deviennent difficiles à distinguer. Pour un thème sombre, une ombre discrète et une bordure claire suffisent généralement à séparer les niveaux.
