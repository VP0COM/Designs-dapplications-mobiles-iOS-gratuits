# Bento grid CSS gratuit : Exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 1 octobre 2026

Les interfaces en **bento grid** sont partout en 2026. Pages SaaS, portfolios, landing pages, dashboards, pages de fonctionnalités ou vitrines produit : cette mise en page permet d’organiser beaucoup d’informations dans une composition claire, moderne et visuellement hiérarchisée.

La bonne nouvelle est qu’il n’est pas nécessaire d’utiliser une bibliothèque JavaScript ou un framework UI complet.

Avec **CSS Grid**, quelques cartes et quelques règles responsive, vous pouvez construire gratuitement votre propre bento grid.

Dans ce guide, vous trouverez plusieurs exemples de bento grid CSS avec du **code HTML et CSS prêt à copier**, depuis une version très simple jusqu’à une grille responsive plus sophistiquée.

## Qu’est-ce qu’une bento grid en CSS ?

Une bento grid est une mise en page composée de plusieurs blocs de dimensions différentes organisés dans une grille.

Le nom vient des boîtes bento japonaises, dans lesquelles plusieurs compartiments de tailles différentes partagent un même espace.

Sur une interface web, le principe est similaire.

Une carte peut occuper une seule cellule.

Une autre peut prendre deux colonnes.

Une troisième peut être plus haute et occuper plusieurs lignes.

Vous obtenez ainsi une composition de ce type :

```text
┌──────────────────┬─────────┐
│                  │         │
│   Grande carte   │ Carte 2 │
│                  │         │
├─────────┬────────┼─────────┤
│ Carte 3 │ Carte 4│         │
│         │        │ Carte 5 │
└─────────┴────────┴─────────┘
```

Le résultat paraît beaucoup plus dynamique qu’une simple grille de cartes identiques.

## Pourquoi CSS Grid est idéal pour créer un bento layout

CSS Grid permet de contrôler simultanément :

- le nombre de colonnes ;
- la largeur des cartes ;
- la hauteur des lignes ;
- l’espace entre les éléments ;
- le nombre de cellules occupées par chaque carte ;
- le comportement responsive.

Une structure minimale peut déjà ressembler à ceci :

```css
.bento-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}
```

Chaque enfant de `.bento-grid` devient automatiquement un élément de la grille.

Vous pouvez ensuite agrandir certaines cartes :

```css
.bento-large {
  grid-column: span 2;
  grid-row: span 2;
}
```

C’est essentiellement le principe sur lequel reposent la majorité des bento grids modernes.

## Exemple 1 : bento grid CSS simple

Commençons par une version extrêmement facile à réutiliser.

### HTML

```html
<section class="bento-grid">
  <article class="bento-card bento-main">
    <span>Analytics</span>
    <h2>Suivez vos performances</h2>
    <p>Centralisez vos données essentielles dans une seule interface.</p>
  </article>

  <article class="bento-card">
    <span>Rapide</span>
    <h3>Configuration simple</h3>
  </article>

  <article class="bento-card">
    <span>Automatique</span>
    <h3>Rapports intelligents</h3>
  </article>

  <article class="bento-card bento-wide">
    <span>Collaboration</span>
    <h3>Travaillez avec toute votre équipe</h3>
  </article>

  <article class="bento-card">
    <span>Sécurité</span>
    <h3>Données protégées</h3>
  </article>
</section>
```

### CSS

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Inter, Arial, sans-serif;
  background: #f5f5f7;
  color: #18181b;
}

.bento-grid {
  width: min(1100px, calc(100% - 40px));
  margin: 80px auto;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 220px;
  gap: 16px;
}

.bento-card {
  padding: 28px;
  border-radius: 24px;
  background: white;
  border: 1px solid #e8e8eb;
  overflow: hidden;
}

.bento-card span {
  display: inline-block;
  margin-bottom: 14px;
  font-size: 14px;
  color: #71717a;
}

.bento-card h2,
.bento-card h3 {
  margin: 0 0 12px;
}

.bento-card p {
  margin: 0;
  max-width: 500px;
  line-height: 1.6;
  color: #71717a;
}

.bento-main {
  grid-column: span 2;
  grid-row: span 2;
}

.bento-wide {
  grid-column: span 2;
}
```

Vous disposez déjà d’une vraie structure bento.

La carte principale occupe deux colonnes et deux lignes, tandis que les autres éléments remplissent automatiquement les espaces disponibles.

## Rendre cette première bento grid responsive

Une grille conçue uniquement pour desktop pose rapidement problème sur mobile.

Le plus simple consiste à réduire progressivement le nombre de colonnes.

Ajoutez ceci :

```css
@media (max-width: 800px) {
  .bento-grid {
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: auto;
  }

  .bento-main,
  .bento-wide {
    grid-column: span 2;
  }
}

@media (max-width: 560px) {
  .bento-grid {
    grid-template-columns: 1fr;
  }

  .bento-main,
  .bento-wide {
    grid-column: span 1;
    grid-row: span 1;
  }

  .bento-card {
    min-height: 200px;
  }
}
```

À partir de 560 pixels, toutes les cartes sont placées dans une seule colonne.

Cette solution est généralement préférable à une reproduction miniature du layout desktop.

Sur smartphone, la lisibilité reste prioritaire.

## Exemple 2 : bento grid moderne pour une landing page SaaS

Pour une landing page, les cartes peuvent contenir davantage de contraste visuel.

### HTML

```html
<section class="features">
  <div class="feature-card feature-primary">
    <div>
      <span class="label">Analyse</span>
      <h2>Comprenez vos données en quelques secondes</h2>
      <p>
        Transformez des informations complexes en insights simples
        et directement exploitables.
      </p>
    </div>

    <div class="chart-preview">
      <div class="chart-bar bar-1"></div>
      <div class="chart-bar bar-2"></div>
      <div class="chart-bar bar-3"></div>
      <div class="chart-bar bar-4"></div>
      <div class="chart-bar bar-5"></div>
    </div>
  </div>

  <div class="feature-card">
    <span class="label">Automatisation</span>
    <h3>Moins de tâches manuelles</h3>
    <div class="metric">−42%</div>
  </div>

  <div class="feature-card dark-card">
    <span class="label">Temps réel</span>
    <h3>Vos résultats toujours à jour</h3>
  </div>

  <div class="feature-card feature-horizontal">
    <div>
      <span class="label">Équipe</span>
      <h3>Un espace partagé pour avancer plus vite</h3>
    </div>

    <div class="avatars">
      <span>A</span>
      <span>B</span>
      <span>C</span>
    </div>
  </div>

  <div class="feature-card">
    <span class="label">Performance</span>
    <h3>Chargement rapide</h3>
    <div class="metric">98</div>
  </div>
</section>
```

### CSS

```css
.features {
  max-width: 1200px;
  margin: 80px auto;
  padding: 0 24px;

  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 240px;
  gap: 18px;
}

.feature-card {
  background: #ffffff;
  border: 1px solid #e9e9ed;
  border-radius: 28px;
  padding: 28px;
  overflow: hidden;
}

.feature-primary {
  grid-column: span 2;
  grid-row: span 2;

  display: flex;
  flex-direction: column;
  justify-content: space-between;

  background:
    radial-gradient(
      circle at 80% 100%,
      rgba(99, 102, 241, 0.18),
      transparent 40%
    ),
    #ffffff;
}

.feature-horizontal {
  grid-column: span 2;

  display: flex;
  justify-content: space-between;
  align-items: center;
}

.label {
  display: block;
  margin-bottom: 14px;
  color: #777780;
  font-size: 14px;
}

.feature-card h2,
.feature-card h3 {
  margin: 0;
  line-height: 1.15;
}

.feature-card h2 {
  max-width: 520px;
  font-size: clamp(30px, 4vw, 48px);
}

.feature-card h3 {
  font-size: 24px;
}

.feature-card p {
  max-width: 520px;
  color: #71717a;
  line-height: 1.6;
}

.dark-card {
  color: white;
  background: #18181b;
  border-color: #18181b;
}

.dark-card .label {
  color: #a1a1aa;
}

.metric {
  margin-top: 40px;
  font-size: 56px;
  font-weight: 700;
}

.chart-preview {
  height: 150px;

  display: flex;
  align-items: flex-end;
  gap: 12px;
}

.chart-bar {
  flex: 1;
  border-radius: 10px 10px 4px 4px;
  background: #6366f1;
}

.bar-1 {
  height: 35%;
}

.bar-2 {
  height: 55%;
}

.bar-3 {
  height: 48%;
}

.bar-4 {
  height: 80%;
}

.bar-5 {
  height: 100%;
}

.avatars {
  display: flex;
}

.avatars span {
  width: 44px;
  height: 44px;
  margin-left: -8px;

  display: grid;
  place-items: center;

  border-radius: 50%;
  border: 3px solid white;
  background: #eeeeef;
  font-weight: 600;
}
```

Cette approche convient particulièrement aux sections présentant quatre à huit fonctionnalités importantes.

## Responsive du bento SaaS

Pour les tablettes :

```css
@media (max-width: 900px) {
  .features {
    grid-template-columns: repeat(2, 1fr);
  }

  .feature-primary {
    grid-column: span 2;
  }
}
```

Pour les smartphones :

```css
@media (max-width: 600px) {
  .features {
    grid-template-columns: 1fr;
    grid-auto-rows: auto;
  }

  .feature-card,
  .feature-primary,
  .feature-horizontal {
    grid-column: span 1;
    grid-row: span 1;
    min-height: 230px;
  }

  .feature-horizontal {
    align-items: flex-start;
    flex-direction: column;
  }
}
```

La logique reste volontairement simple.

Le layout devient sophistiqué lorsque l’espace le permet et linéaire lorsque la largeur devient limitée.

## Exemple 3 : bento grid avec grid-template-areas

Pour une composition fixe, `grid-template-areas` peut rendre le CSS encore plus lisible.

### HTML

```html
<div class="bento-layout">
  <div class="box box-a">A</div>
  <div class="box box-b">B</div>
  <div class="box box-c">C</div>
  <div class="box box-d">D</div>
  <div class="box box-e">E</div>
</div>
```

### CSS

```css
.bento-layout {
  max-width: 1000px;
  margin: 60px auto;

  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: 220px 220px 220px;
  grid-template-areas:
    "a a b c"
    "a a d d"
    "e e d d";
  gap: 16px;
}

.box {
  display: grid;
  place-items: center;

  border-radius: 24px;
  background: #f4f4f5;

  font-size: 32px;
  font-weight: 700;
}

.box-a {
  grid-area: a;
}

.box-b {
  grid-area: b;
}

.box-c {
  grid-area: c;
}

.box-d {
  grid-area: d;
}

.box-e {
  grid-area: e;
}
```

L'avantage est immédiatement visible dans cette partie :

```css
grid-template-areas:
  "a a b c"
  "a a d d"
  "e e d d";
```

Vous pouvez presque visualiser votre interface simplement en lisant le CSS.

Les zones nommées doivent former des surfaces rectangulaires, ce qui permet de garder une structure prévisible.

## Changer complètement le layout sur mobile

L’autre avantage des zones nommées est de pouvoir redessiner très facilement la grille.

```css
@media (max-width: 700px) {
  .bento-layout {
    grid-template-columns: 1fr;
    grid-template-rows: auto;
    grid-template-areas:
      "a"
      "b"
      "c"
      "d"
      "e";
  }

  .box {
    min-height: 220px;
  }
}
```

Vous conservez exactement le même HTML.

Seul le CSS change.

## Exemple 4 : bento grid automatique

Toutes les grilles bento ne nécessitent pas un placement manuel de chaque carte.

Vous pouvez utiliser l’auto-placement de CSS Grid.

### HTML

```html
<section class="auto-bento">
  <article class="auto-card large">01</article>
  <article class="auto-card">02</article>
  <article class="auto-card">03</article>
  <article class="auto-card wide">04</article>
  <article class="auto-card">05</article>
  <article class="auto-card tall">06</article>
  <article class="auto-card">07</article>
  <article class="auto-card">08</article>
</section>
```

### CSS

```css
.auto-bento {
  max-width: 1100px;
  margin: 80px auto;

  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 180px;
  grid-auto-flow: dense;
  gap: 16px;
}

.auto-card {
  padding: 24px;
  border-radius: 24px;
  background: #f1f1f3;

  font-size: 24px;
  font-weight: 700;
}

.auto-card.large {
  grid-column: span 2;
  grid-row: span 2;
}

.auto-card.wide {
  grid-column: span 2;
}

.auto-card.tall {
  grid-row: span 2;
}
```

La propriété intéressante ici est :

```css
grid-auto-flow: dense;
```

Elle autorise l’algorithme de placement de Grid à exploiter certains espaces laissés libres par les éléments plus grands.

Elle est particulièrement pratique lorsqu’une page contient beaucoup de cartes de dimensions variables.

Il faut cependant garder à l’esprit qu’un réarrangement visuel peut différer de l’ordre du contenu dans le document. Pour une interface importante sur le plan sémantique ou accessible au clavier, l’ordre HTML doit donc rester cohérent.

## Exemple 5 : bento grid entièrement fluide

Vous n’êtes pas obligé d’utiliser des breakpoints pour tout.

Une grille peut déjà s’adapter grâce à `minmax()`.

```css
.fluid-bento {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(240px, 1fr));
  gap: 16px;
}
```

Voici une version complète.

### HTML

```html
<section class="fluid-bento">
  <article class="fluid-card">
    <h3>Design</h3>
    <p>Construisez une interface claire.</p>
  </article>

  <article class="fluid-card">
    <h3>Développement</h3>
    <p>Transformez rapidement votre concept en produit.</p>
  </article>

  <article class="fluid-card">
    <h3>Performance</h3>
    <p>Gardez une interface rapide et légère.</p>
  </article>

  <article class="fluid-card">
    <h3>Responsive</h3>
    <p>Adaptez automatiquement la grille à l'écran.</p>
  </article>
</section>
```

### CSS

```css
.fluid-bento {
  max-width: 1100px;
  margin: 80px auto;
  padding: 0 24px;

  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(240px, 1fr));

  gap: 16px;
}

.fluid-card {
  min-height: 240px;
  padding: 28px;

  border-radius: 24px;
  background: #f5f5f7;
}

.fluid-card h3 {
  margin-top: 0;
  font-size: 24px;
}

.fluid-card p {
  color: #71717a;
  line-height: 1.6;
}
```

Cette variante est moins spectaculaire puisqu’elle contient principalement des cartes de même taille.

Elle constitue cependant une excellente base responsive.

## Exemple 6 : bento grid avec effet glassmorphism

Une grille bento fonctionne également très bien avec des cartes translucides.

### HTML

```html
<div class="glass-section">
  <div class="glass-bento">
    <article class="glass-card glass-main">
      <span>Workspace</span>
      <h2>Tout votre travail au même endroit</h2>
    </article>

    <article class="glass-card">
      <span>Cloud</span>
      <h3>Synchronisation</h3>
    </article>

    <article class="glass-card">
      <span>AI</span>
      <h3>Automatisation</h3>
    </article>

    <article class="glass-card glass-wide">
      <span>Collaboration</span>
      <h3>Travail en temps réel</h3>
    </article>
  </div>
</div>
```

### CSS

```css
.glass-section {
  min-height: 100vh;
  padding: 80px 24px;

  background:
    radial-gradient(
      circle at 20% 20%,
      #8b5cf6,
      transparent 35%
    ),
    radial-gradient(
      circle at 80% 80%,
      #06b6d4,
      transparent 35%
    ),
    #111827;
}

.glass-bento {
  max-width: 1100px;
  margin: auto;

  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 220px;
  gap: 18px;
}

.glass-card {
  padding: 28px;

  border: 1px solid rgba(255, 255, 255, 0.18);
  border-radius: 28px;

  background: rgba(255, 255, 255, 0.09);
  backdrop-filter: blur(18px);

  color: white;
}

.glass-card span {
  color: rgba(255, 255, 255, 0.6);
}

.glass-card h2,
.glass-card h3 {
  margin-top: 14px;
}

.glass-main {
  grid-column: span 2;
  grid-row: span 2;
}

.glass-wide {
  grid-column: span 2;
}
```

L’effet peut être très réussi, mais il doit rester secondaire.

Une interface remplie de transparence, de flou, d’ombres et de gradients peut rapidement devenir difficile à lire.

## Exemple 7 : bento grid sombre minimaliste

Pour une esthétique plus proche des interfaces développeur :

```html
<section class="dark-bento">
  <article class="dark-item hero">
    <small>Performance</small>
    <h2>Build faster.</h2>
    <p>Une interface pensée pour la vitesse.</p>
  </article>

  <article class="dark-item">
    <small>API</small>
    <h3>Simple</h3>
  </article>

  <article class="dark-item">
    <small>Deploy</small>
    <h3>Instantané</h3>
  </article>

  <article class="dark-item wide">
    <small>Infrastructure</small>
    <h3>Prête pour passer à l'échelle</h3>
  </article>
</section>
```

```css
body {
  background: #09090b;
}

.dark-bento {
  max-width: 1100px;
  margin: 80px auto;

  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 220px;
  gap: 12px;
}

.dark-item {
  padding: 28px;

  background: #111113;
  border: 1px solid #27272a;
  border-radius: 20px;

  color: #fafafa;
}

.dark-item small {
  color: #71717a;
}

.dark-item h2,
.dark-item h3 {
  margin: 14px 0 8px;
}

.dark-item p {
  color: #a1a1aa;
}

.dark-item.hero {
  grid-column: span 2;
  grid-row: span 2;
}

.dark-item.wide {
  grid-column: span 2;
}
```

Ici, la hiérarchie repose davantage sur les dimensions que sur les couleurs.

C’est souvent une bonne approche pour éviter une interface trop chargée.

## Exemple 8 : bento grid avec animation au survol

Une petite interaction peut rendre les cartes plus vivantes.

```css
.bento-card {
  transition:
    transform 250ms ease,
    box-shadow 250ms ease,
    border-color 250ms ease;
}

.bento-card:hover {
  transform: translateY(-4px);

  border-color: #d4d4d8;

  box-shadow:
    0 20px 50px
    rgba(0, 0, 0, 0.08);
}
```

Pour un effet interne :

```css
.bento-card .visual {
  transition: transform 350ms ease;
}

.bento-card:hover .visual {
  transform: scale(1.04);
}
```

Les animations doivent rester discrètes.

Une bento grid contient déjà beaucoup d’éléments visuels. Si chaque carte effectue une rotation, un zoom important ou une animation différente, l’ensemble devient rapidement chaotique.

## Ajouter un effet de lumière suivant la souris

Pour une carte plus interactive, vous pouvez créer un halo avec des variables CSS.

### HTML

```html
<div class="spotlight-card">
  <h3>AI Workspace</h3>
  <p>Une carte avec un effet lumineux interactif.</p>
</div>
```

### CSS

```css
.spotlight-card {
  --x: 50%;
  --y: 50%;

  position: relative;
  overflow: hidden;

  padding: 32px;
  min-height: 260px;

  border-radius: 24px;
  border: 1px solid #27272a;

  background:
    radial-gradient(
      400px circle at var(--x) var(--y),
      rgba(255, 255, 255, 0.12),
      transparent 40%
    ),
    #111113;

  color: white;
}
```

### JavaScript

```html
<script>
  const card = document.querySelector(".spotlight-card");

  card.addEventListener("mousemove", (event) => {
    const rect = card.getBoundingClientRect();

    const x = event.clientX - rect.left;
    const y = event.clientY - rect.top;

    card.style.setProperty("--x", `${x}px`);
    card.style.setProperty("--y", `${y}px`);
  });
</script>
```

Cette interaction est intéressante pour une ou deux cartes principales.

Elle devient beaucoup plus coûteuse visuellement lorsqu’elle est appliquée partout.

## Exemple 9 : bento grid sans JavaScript

Pour la majorité des landing pages, JavaScript n’est absolument pas indispensable.

Voici une structure complète utilisant uniquement HTML et CSS.

```html
<section class="pure-bento">
  <div class="pure-card main">
    <h2>Design moderne</h2>
    <p>
      Une grande carte pour votre proposition de valeur principale.
    </p>
  </div>

  <div class="pure-card stats">
    <strong>10K+</strong>
    <span>utilisateurs</span>
  </div>

  <div class="pure-card speed">
    <strong>120 ms</strong>
    <span>temps de réponse</span>
  </div>

  <div class="pure-card integrations">
    <h3>Intégrations</h3>
    <p>Connectez vos outils existants.</p>
  </div>

  <div class="pure-card security">
    <h3>Sécurité</h3>
    <p>Une infrastructure conçue pour protéger vos données.</p>
  </div>
</section>
```

```css
.pure-bento {
  max-width: 1100px;
  margin: 80px auto;
  padding: 0 24px;

  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 200px;

  gap: 16px;
}

.pure-card {
  padding: 28px;

  border-radius: 24px;
  background: #f4f4f5;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.pure-card.main {
  grid-column: span 2;
  grid-row: span 2;
}

.pure-card.integrations {
  grid-column: span 2;
}

.pure-card.security {
  grid-column: span 2;
}

.pure-card strong {
  font-size: clamp(36px, 5vw, 64px);
}

.pure-card span,
.pure-card p {
  color: #71717a;
}

@media (max-width: 800px) {
  .pure-bento {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 560px) {
  .pure-bento {
    grid-template-columns: 1fr;
    grid-auto-rows: auto;
  }

  .pure-card,
  .pure-card.main,
  .pure-card.integrations,
  .pure-card.security {
    grid-column: span 1;
    grid-row: span 1;
    min-height: 200px;
  }
}
```

Vous pouvez copier cette base dans pratiquement n’importe quelle landing page.

## Utiliser `minmax()` pour une grille plus robuste

Les dimensions fixes sont pratiques pour reproduire précisément une maquette, mais une approche plus flexible peut être préférable.

Par exemple :

```css
grid-template-columns:
  repeat(4, minmax(0, 1fr));
```

Le `minmax(0, 1fr)` permet notamment aux colonnes de réellement se réduire lorsque leur contenu est volumineux.

Pour les lignes :

```css
grid-auto-rows:
  minmax(180px, auto);
```

Le contenu peut ainsi augmenter naturellement la hauteur d’une carte.

## Quand utiliser `grid-column: span 2`

C’est probablement la technique la plus utile pour une bento grid.

```css
.card-large {
  grid-column: span 2;
}
```

L’élément occupe deux colonnes.

Pour deux lignes :

```css
.card-tall {
  grid-row: span 2;
}
```

Pour les deux :

```css
.card-featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

Cela permet de construire une hiérarchie visuelle sans ajouter beaucoup de CSS.

## Créer une grille de 12 colonnes

Pour davantage de contrôle, vous pouvez utiliser le même principe que les systèmes de grille traditionnels.

```css
.bento-12 {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 16px;
}

.card-main {
  grid-column: span 8;
}

.card-side {
  grid-column: span 4;
}

.card-half {
  grid-column: span 6;
}
```

Cette approche fonctionne particulièrement bien avec des maquettes complexes.

Par exemple :

```html
<div class="bento-12">
  <article class="card card-main">
    Grande fonctionnalité
  </article>

  <article class="card card-side">
    Statistique
  </article>

  <article class="card card-half">
    Fonctionnalité A
  </article>

  <article class="card card-half">
    Fonctionnalité B
  </article>
</div>
```

Vous obtenez une grande liberté pour créer des proportions comme :

- 8 + 4 ;
- 6 + 6 ;
- 4 + 4 + 4 ;
- 3 + 3 + 6.

## Comment choisir les dimensions des cartes

Le piège consiste à rendre chaque carte différente simplement parce que le principe bento l’autorise.

Une bonne grille doit conserver une logique.

Une méthode simple consiste à définir trois dimensions.

### Petite carte

Pour une information secondaire :

```css
.small {
  grid-column: span 1;
  grid-row: span 1;
}
```

### Carte horizontale

Pour une fonctionnalité importante :

```css
.wide {
  grid-column: span 2;
}
```

### Grande carte

Pour l’élément principal :

```css
.large {
  grid-column: span 2;
  grid-row: span 2;
}
```

Ces trois variantes suffisent pour la majorité des interfaces.

## Quels border-radius utiliser en 2026 ?

Il n’existe pas une valeur parfaite.

Pour une interface moderne, vous pouvez commencer autour de :

```css
border-radius: 24px;
```

Pour de grandes cartes :

```css
border-radius: 28px;
```

Pour une esthétique plus technique :

```css
border-radius: 16px;
```

Le plus important est la cohérence.

Évitez par exemple d’utiliser 12 px sur une carte, 28 px sur une autre et 40 px sur une troisième sans raison visuelle particulière.

## Quel espace entre les cartes ?

Une base raisonnable est :

```css
gap: 16px;
```

Pour un layout plus compact :

```css
gap: 12px;
```

Pour une page très aérée :

```css
gap: 20px;
```

Le `gap` CSS est préférable aux marges individuelles entre les cartes, car la grille contrôle directement l’espacement.

## Ajouter une bordure discrète

Les cartes blanches peuvent disparaître sur un fond blanc.

Une bordure subtile aide à les séparer :

```css
border: 1px solid #e4e4e7;
```

Sur fond sombre :

```css
border: 1px solid #27272a;
```

Vous pouvez également combiner une bordure légère et une ombre :

```css
box-shadow:
  0 1px 2px rgba(0, 0, 0, 0.04),
  0 8px 30px rgba(0, 0, 0, 0.04);
```

## Une bento grid doit-elle utiliser des images ?

Pas nécessairement.

Une bonne carte peut contenir :

- du texte ;
- une métrique ;
- une mini visualisation ;
- une icône ;
- un graphique ;
- un extrait d’interface ;
- une illustration ;
- un témoignage ;
- un exemple de résultat.

Les meilleures compositions mélangent généralement plusieurs formats sans transformer chaque cellule en mini landing page.

## Bento grid pour une section de fonctionnalités

Une section classique de fonctionnalités peut suivre ce modèle :

```text
Grande fonctionnalité principale
        +
Petite métrique
        +
Petite fonctionnalité
        +
Grande démonstration produit
        +
Preuve sociale
```

La taille de chaque cellule communique implicitement son importance.

Une fonctionnalité centrale peut occuper quatre cellules.

Une statistique secondaire peut n’en occuper qu’une.

Cette hiérarchie est l’une des principales raisons pour lesquelles le format bento fonctionne si bien.

## Bento grid pour un portfolio

Le même principe peut être appliqué à des projets.

```html
<section class="portfolio-grid">
  <article class="project project-main">
    Projet principal
  </article>

  <article class="project">
    Application mobile
  </article>

  <article class="project">
    Branding
  </article>

  <article class="project project-wide">
    Dashboard SaaS
  </article>
</section>
```

```css
.portfolio-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 260px;
  gap: 16px;
}

.project {
  padding: 30px;
  border-radius: 24px;
  background: #ececef;
}

.project-main {
  grid-column: span 2;
  grid-row: span 2;
}

.project-wide {
  grid-column: span 2;
}
```

Il suffit ensuite d’utiliser des images en arrière-plan ou des éléments `<img>`.

## Bento grid pour une page personnelle

Vous pouvez également construire une homepage personnelle avec des cellules consacrées à :

- votre présentation ;
- votre projet principal ;
- vos réseaux ;
- vos compétences ;
- votre localisation ;
- vos derniers travaux ;
- vos statistiques ;
- vos disponibilités.

Par exemple :

```css
.profile {
  grid-column: span 2;
  grid-row: span 2;
}

.work {
  grid-column: span 2;
}

.social {
  grid-column: span 1;
}

.about {
  grid-column: span 1;
}
```

Le style bento est particulièrement adapté aux portfolios parce qu’il permet de présenter plusieurs facettes d’un profil sur un seul écran.

## Les erreurs fréquentes avec les bento grids

### Créer trop de tailles différentes

Si chaque carte possède une largeur et une hauteur différentes, la grille perd sa structure.

Limitez-vous généralement à deux ou trois formats.

### Mettre trop de contenu dans les petites cartes

Une cellule étroite ne devrait pas contenir quatre paragraphes.

Gardez les cartes secondaires simples.

### Reproduire exactement le desktop sur mobile

Un layout quatre colonnes n’a pas besoin de devenir une miniature quatre colonnes sur smartphone.

Transformez-le en une ou deux colonnes.

### Utiliser trop d’effets

Gradient, glow, glassmorphism, ombres, animations et textures peuvent fonctionner individuellement.

Les utiliser simultanément sur chaque carte rend l’interface beaucoup plus lourde.

### Oublier la hiérarchie

Toutes les cartes ne doivent pas attirer autant l’attention.

Votre utilisateur doit comprendre immédiatement quelle zone est la plus importante.

## Faut-il utiliser Flexbox ou Grid ?

Pour le conteneur bento lui-même, CSS Grid est généralement le choix naturel.

Pour organiser le contenu à l’intérieur d’une carte, Flexbox reste extrêmement pratique.

Vous pouvez donc combiner les deux :

```css
.bento {
  display: grid;
}

.bento-card {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
```

Grid contrôle la structure globale.

Flexbox contrôle l’organisation interne.

Ce n’est pas un choix exclusif entre les deux technologies.

## Bento grid CSS ou bibliothèque UI ?

Pour une grille relativement simple, le CSS natif suffit largement.

Une bibliothèque devient intéressante lorsque vous avez besoin de composants complets comportant déjà :

- des animations ;
- des cartes interactives ;
- des variantes ;
- des thèmes ;
- des composants React ;
- des états complexes.

Mais importer un système entier uniquement pour afficher six cartes n’est généralement pas nécessaire.

Pour prototyper rapidement différents styles d’interface, des outils comme VP0 peuvent également servir à explorer une composition avant de reprendre ou d’adapter la structure dans votre propre projet.

## Exemple final : bento grid prêt à copier

Voici une version compacte réunissant les principes les plus utiles.

### HTML

```html
<section class="bento">
  <article class="card card-main">
    <div>
      <span class="eyebrow">Plateforme</span>
      <h1>Construisez de meilleures interfaces.</h1>
      <p>
        Une bento grid responsive réalisée uniquement avec HTML et CSS.
      </p>
    </div>

    <div class="preview">
      <div class="preview-inner"></div>
    </div>
  </article>

  <article class="card">
    <span class="eyebrow">Rapide</span>
    <strong>120 ms</strong>
  </article>

  <article class="card">
    <span class="eyebrow">Utilisateurs</span>
    <strong>12K+</strong>
  </article>

  <article class="card card-wide">
    <span class="eyebrow">Collaboration</span>
    <h2>Travaillez ensemble en temps réel.</h2>
  </article>

  <article class="card">
    <span class="eyebrow">Disponibilité</span>
    <strong>99.9%</strong>
  </article>
</section>
```

### CSS

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 80px 24px;

  font-family:
    Inter,
    system-ui,
    sans-serif;

  background: #f7f7f8;
  color: #18181b;
}

.bento {
  width: 100%;
  max-width: 1120px;
  margin: auto;

  display: grid;
  grid-template-columns:
    repeat(3, minmax(0, 1fr));

  grid-auto-rows:
    minmax(220px, auto);

  gap: 16px;
}

.card {
  min-width: 0;
  padding: 28px;

  display: flex;
  flex-direction: column;
  justify-content: space-between;

  border: 1px solid #e4e4e7;
  border-radius: 26px;

  background: #ffffff;

  overflow: hidden;

  transition:
    transform 250ms ease,
    box-shadow 250ms ease;
}

.card:hover {
  transform: translateY(-3px);

  box-shadow:
    0 20px 60px
    rgba(0, 0, 0, 0.07);
}

.card-main {
  grid-column: span 2;
  grid-row: span 2;
}

.card-wide {
  grid-column: span 2;
}

.eyebrow {
  display: block;

  margin-bottom: 16px;

  color: #71717a;

  font-size: 14px;
  font-weight: 500;
}

.card h1 {
  max-width: 600px;

  margin: 0;

  font-size:
    clamp(36px, 5vw, 64px);

  line-height: 1;
  letter-spacing: -0.04em;
}

.card h2 {
  max-width: 500px;

  margin: 0;

  font-size: 30px;
  line-height: 1.1;
}

.card p {
  max-width: 560px;

  color: #71717a;
  line-height: 1.6;
}

.card strong {
  font-size:
    clamp(40px, 5vw, 64px);

  letter-spacing: -0.04em;
}

.preview {
  margin-top: 40px;
  height: 220px;

  padding: 16px 16px 0;

  border-radius:
    20px 20px 0 0;

  background:
    linear-gradient(
      135deg,
      #eef2ff,
      #f5f3ff
    );
}

.preview-inner {
  width: 100%;
  height: 100%;

  border-radius:
    14px 14px 0 0;

  background: white;

  box-shadow:
    0 10px 40px
    rgba(0, 0, 0, 0.08);
}

@media (max-width: 800px) {
  .bento {
    grid-template-columns:
      repeat(2, minmax(0, 1fr));
  }

  .card-main,
  .card-wide {
    grid-column: span 2;
  }
}

@media (max-width: 560px) {
  body {
    padding:
      40px 16px;
  }

  .bento {
    grid-template-columns: 1fr;
    grid-auto-rows: auto;
  }

  .card,
  .card-main,
  .card-wide {
    grid-column: span 1;
    grid-row: span 1;

    min-height: 220px;
  }

  .card-main {
    min-height: 500px;
  }
}
```

Ce modèle offre une bonne base pour une landing page, un produit SaaS, un portfolio ou une page de fonctionnalités.

## Comment personnaliser rapidement cette bento grid

Vous pouvez modifier son apparence sans toucher à sa structure.

### Version plus arrondie

```css
.card {
  border-radius: 32px;
}
```

### Version compacte

```css
.bento {
  gap: 10px;
}

.card {
  padding: 22px;
}
```

### Version sombre

```css
body {
  background: #09090b;
  color: #fafafa;
}

.card {
  background: #111113;
  border-color: #27272a;
}
```

### Version sans bordures

```css
.card {
  border: 0;
  background: #f0f0f2;
}
```

### Version avec davantage d'espace

```css
.bento {
  gap: 24px;
}

.card {
  padding: 36px;
}
```

## Checklist pour une bonne bento grid

Avant de publier votre interface, vérifiez quelques points.

- Une carte principale est clairement identifiable.
- Les cartes secondaires restent plus discrètes.
- Les espaces entre les cartes sont réguliers.
- Le rayon des coins est cohérent.
- Le texte reste lisible dans les petites cellules.
- Le layout fonctionne sur tablette.
- Le layout se transforme proprement sur smartphone.
- Les éléments importants conservent un ordre HTML logique.
- Les animations ne ralentissent pas l’interface.
- Les cartes n’utilisent pas toutes un effet visuel différent.

Une bonne bento grid paraît riche sans sembler désorganisée.

## FAQ

### Comment créer une bento grid en CSS ?

Utilisez un conteneur avec `display: grid`, définissez plusieurs colonnes avec `grid-template-columns`, puis donnez à certaines cartes une taille supérieure avec `grid-column: span 2` ou `grid-row: span 2`.

### CSS Grid est-il obligatoire pour un bento layout ?

Non, mais il s’agit généralement de la solution la plus pratique. Flexbox peut convenir à certaines compositions simples, mais CSS Grid est mieux adapté lorsqu’un élément doit occuper plusieurs lignes et colonnes.

### Peut-on créer une bento grid sans JavaScript ?

Oui. La structure, le responsive, les dimensions variables, les couleurs, les gradients et même la majorité des animations peuvent être réalisés uniquement avec HTML et CSS.

### Comment rendre une bento grid responsive ?

Utilisez des media queries pour réduire le nombre de colonnes. Une grille peut passer de quatre colonnes sur desktop à deux sur tablette puis une seule sur mobile.

### Comment créer une grande carte dans CSS Grid ?

Utilisez par exemple :

```css
.card-large {
  grid-column: span 2;
  grid-row: span 2;
}
```

La carte occupera alors deux colonnes et deux lignes.

### Peut-on utiliser `grid-template-areas` pour une bento grid ?

Oui. Cette technique est particulièrement adaptée lorsque vous connaissez précisément la disposition souhaitée et que vous voulez pouvoir la redéfinir facilement pour les différentes tailles d’écran.

### Quelle taille de gap utiliser ?

Une valeur située autour de 12 à 20 pixels fonctionne bien dans de nombreuses interfaces. `16px` constitue un bon point de départ.

### Quelle taille de border-radius utiliser ?

Entre 16 et 32 pixels fonctionne bien pour la plupart des styles bento modernes. L’important est surtout de conserver une valeur cohérente entre les différentes cartes.

### Faut-il donner une hauteur fixe aux cartes ?

Pas obligatoirement. `grid-auto-rows: minmax(180px, auto)` permet par exemple de conserver une hauteur minimale tout en laissant les cartes s’agrandir si leur contenu le nécessite.

### Quelle est la méthode la plus simple pour commencer ?

Commencez avec trois colonnes, un `gap` de 16 pixels et seulement trois types de cartes : normale, large et grande. Ajoutez ensuite les effets visuels uniquement une fois que la hiérarchie et le responsive fonctionnent correctement.

## Conclusion

Une bento grid impressionnante n’exige pas beaucoup de code.

Pour commencer, quatre propriétés suffisent souvent :

```css
.bento {
  display: grid;
  grid-template-columns:
    repeat(3, 1fr);
  grid-auto-rows: 220px;
  gap: 16px;
}
```

Puis donnez davantage d’espace à vos éléments importants :

```css
.featured {
  grid-column: span 2;
  grid-row: span 2;
}
```

C’est cette combinaison entre une grille régulière et quelques cellules volontairement plus grandes qui crée l’esthétique bento.

Pour un projet réel en 2026, privilégiez d’abord une hiérarchie claire, un responsive simple et un contenu lisible. Les gradients, animations, ombres ou effets glassmorphism peuvent ensuite enrichir la composition.

Le résultat sera plus moderne, mais surtout beaucoup plus facile à maintenir.
