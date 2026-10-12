# Sidebar Tailwind CSS copy paste : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 11 octobre 2026

Une bonne sidebar Tailwind CSS n’a pas besoin d’un composant compliqué. Pour la plupart des interfaces, il suffit d’une colonne fixe sur desktop, d’un menu mobile qui s’ouvre au-dessus du contenu, d’une navigation claire et de quelques états actifs bien visibles. Le vrai gain vient du fait que Tailwind permet de construire tout cela directement avec des classes utilitaires, puis d’adapter la même structure à React, Next.js, Vue ou une interface générée avec l’IA.

Voici plusieurs exemples prêts à copier, du plus simple au plus complet.

## Quel code utiliser pour une sidebar Tailwind CSS simple ?

Commencez par une structure minimale. Elle convient à un dashboard, un outil SaaS, un panneau d’administration ou une application interne.

```html
<div class="flex min-h-screen bg-gray-50">
  <aside class="w-64 border-r border-gray-200 bg-white">
    <div class="flex h-16 items-center border-b border-gray-200 px-6">
      <span class="text-lg font-semibold text-gray-900">Dashboard</span>
    </div>

    <nav class="space-y-1 p-4">
      <a
        href="#"
        class="flex items-center rounded-lg bg-gray-100 px-3 py-2 text-sm font-medium text-gray-900"
      >
        Accueil
      </a>

      <a
        href="#"
        class="flex items-center rounded-lg px-3 py-2 text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900"
      >
        Projets
      </a>

      <a
        href="#"
        class="flex items-center rounded-lg px-3 py-2 text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900"
      >
        Équipe
      </a>

      <a
        href="#"
        class="flex items-center rounded-lg px-3 py-2 text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900"
      >
        Paramètres
      </a>
    </nav>
  </aside>

  <main class="flex-1 p-8">
    <h1 class="text-2xl font-bold text-gray-900">Accueil</h1>
    <p class="mt-2 text-gray-600">
      Votre contenu principal apparaît ici.
    </p>
  </main>
</div>
```

Le principe est simple : `flex` place la sidebar et le contenu côte à côte, `w-64` donne une largeur constante au menu et `flex-1` laisse le reste de l’espace au contenu principal.

Cette version est volontairement sobre. C’est généralement préférable à une sidebar remplie de bordures, d’ombres, de badges et de couleurs différentes.

## Comment créer une sidebar Tailwind CSS responsive ?

Une sidebar desktop ne doit pas simplement devenir plus étroite sur mobile. Sur un petit écran, elle prend trop de place. Une approche plus propre consiste à masquer la navigation latérale et à afficher un bouton de menu.

Tailwind fonctionne très bien pour cette logique grâce à ses variantes responsives. La documentation officielle de Tailwind explique notamment son fonctionnement mobile-first et l’utilisation des préfixes comme `md:` et `lg:`.

Voici une structure pratique :

```html
<div class="min-h-screen bg-gray-50">
  <header class="flex h-16 items-center border-b border-gray-200 bg-white px-4 lg:hidden">
    <button
      type="button"
      class="rounded-lg p-2 text-gray-600 hover:bg-gray-100"
      aria-label="Ouvrir le menu"
    >
      <svg
        class="h-6 w-6"
        fill="none"
        stroke="currentColor"
        viewBox="0 0 24 24"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M4 6h16M4 12h16M4 18h16"
        />
      </svg>
    </button>

    <span class="ml-3 font-semibold text-gray-900">Mon application</span>
  </header>

  <aside
    class="fixed inset-y-0 left-0 hidden w-64 border-r border-gray-200 bg-white lg:block"
  >
    <div class="flex h-16 items-center px-6">
      <span class="text-lg font-semibold">Mon application</span>
    </div>

    <nav class="space-y-1 px-4">
      <a
        href="#"
        class="block rounded-lg bg-gray-100 px-3 py-2 text-sm font-medium text-gray-900"
      >
        Tableau de bord
      </a>

      <a
        href="#"
        class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100"
      >
        Utilisateurs
      </a>

      <a
        href="#"
        class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100"
      >
        Analyses
      </a>

      <a
        href="#"
        class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100"
      >
        Paramètres
      </a>
    </nav>
  </aside>

  <main class="p-6 lg:ml-64 lg:p-8">
    <h1 class="text-2xl font-bold text-gray-900">
      Tableau de bord
    </h1>
  </main>
</div>
```

Ici, `hidden lg:block` masque la sidebar avant le breakpoint `lg`. Le contenu utilise `lg:ml-64`, ce qui réserve l’espace nécessaire uniquement lorsque la navigation latérale est visible.

Pour une vraie application, le bouton mobile devra ensuite contrôler un état JavaScript qui ouvre et ferme le menu.

## Comment faire une sidebar moderne avec icônes ?

Une sidebar devient plus facile à parcourir lorsque chaque destination importante possède une icône identifiable. Il faut toutefois éviter de transformer chaque ligne en mini-carte.

Voici un exemple plus proche des interfaces SaaS actuelles :

```html
<aside class="flex h-screen w-64 flex-col border-r border-gray-200 bg-white">
  <div class="flex h-16 items-center px-5">
    <div class="flex h-9 w-9 items-center justify-center rounded-xl bg-gray-900 text-sm font-bold text-white">
      A
    </div>

    <span class="ml-3 font-semibold text-gray-900">
      Acme
    </span>
  </div>

  <nav class="flex-1 space-y-1 px-3 py-4">
    <a
      href="#"
      class="flex items-center gap-3 rounded-lg bg-gray-100 px-3 py-2 text-sm font-medium text-gray-900"
    >
      <svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-width="1.8" d="M3 12l9-9 9 9v9H3z" />
      </svg>
      Accueil
    </a>

    <a
      href="#"
      class="flex items-center gap-3 rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100 hover:text-gray-900"
    >
      <svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-width="1.8" d="M4 5h16v14H4z" />
      </svg>
      Projets
    </a>

    <a
      href="#"
      class="flex items-center gap-3 rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100 hover:text-gray-900"
    >
      <svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <circle cx="12" cy="8" r="3" stroke-width="1.8" />
        <path stroke-width="1.8" d="M5 21c0-4 3-7 7-7s7 3 7 7" />
      </svg>
      Profil
    </a>
  </nav>

  <div class="border-t border-gray-200 p-3">
    <button
      class="flex w-full items-center gap-3 rounded-lg px-3 py-2 text-left hover:bg-gray-100"
    >
      <div class="h-8 w-8 rounded-full bg-gray-200"></div>

      <div class="min-w-0">
        <p class="truncate text-sm font-medium text-gray-900">
          Marie Dupont
        </p>
        <p class="truncate text-xs text-gray-500">
          Compte personnel
        </p>
      </div>
    </button>
  </div>
</aside>
```

Le détail important est la hiérarchie. L’élément actif utilise un fond légèrement différent, alors que les éléments inactifs restent discrets.

Vous n’avez généralement pas besoin d’une couleur vive pour signaler la page actuelle.

## Comment créer une sidebar sombre avec Tailwind CSS ?

Une sidebar sombre fonctionne particulièrement bien pour les outils techniques, dashboards et applications où la navigation doit être clairement séparée du contenu.

```html
<aside class="flex h-screen w-64 flex-col bg-gray-950 text-gray-300">
  <div class="flex h-16 items-center px-5">
    <span class="text-lg font-semibold text-white">
      Workspace
    </span>
  </div>

  <nav class="flex-1 space-y-1 px-3 py-4">
    <p class="px-3 pb-2 text-xs font-medium uppercase tracking-wider text-gray-500">
      Workspace
    </p>

    <a
      href="#"
      class="block rounded-lg bg-white/10 px-3 py-2 text-sm font-medium text-white"
    >
      Overview
    </a>

    <a
      href="#"
      class="block rounded-lg px-3 py-2 text-sm hover:bg-white/5 hover:text-white"
    >
      Projects
    </a>

    <a
      href="#"
      class="block rounded-lg px-3 py-2 text-sm hover:bg-white/5 hover:text-white"
    >
      Activity
    </a>

    <p class="px-3 pb-2 pt-6 text-xs font-medium uppercase tracking-wider text-gray-500">
      Account
    </p>

    <a
      href="#"
      class="block rounded-lg px-3 py-2 text-sm hover:bg-white/5 hover:text-white"
    >
      Settings
    </a>
  </nav>
</aside>
```

Évitez le noir absolu associé à du blanc pur sur chaque élément. Des niveaux comme `gray-950`, `gray-300`, `gray-500` et `white/10` créent une hiérarchie plus douce.

## Comment faire une sidebar rétractable ?

Une sidebar rétractable utilise généralement deux états : environ 240 à 280 pixels lorsqu’elle est ouverte, puis 64 à 80 pixels lorsqu’elle est réduite.

Le HTML peut rester presque identique. C’est surtout la largeur et la visibilité des labels qui changent.

Version ouverte :

```html
<aside class="w-64 transition-all duration-200">
  <a href="#" class="flex items-center gap-3 px-4 py-2">
    <svg class="h-5 w-5"></svg>
    <span>Dashboard</span>
  </a>
</aside>
```

Version réduite :

```html
<aside class="w-20 transition-all duration-200">
  <a href="#" class="flex items-center justify-center px-4 py-2">
    <svg class="h-5 w-5"></svg>
    <span class="sr-only">Dashboard</span>
  </a>
</aside>
```

Le `sr-only` est utile ici : le texte disparaît visuellement mais reste disponible pour les technologies d’assistance.

Dans React, vous pouvez simplement basculer les classes selon un état `collapsed`.

```jsx
<aside
  className={`h-screen border-r bg-white transition-all duration-200 ${
    collapsed ? "w-20" : "w-64"
  }`}
>
  <nav className="p-3">
    <a
      href="/"
      className={`flex items-center rounded-lg px-3 py-2 hover:bg-gray-100 ${
        collapsed ? "justify-center" : "gap-3"
      }`}
    >
      <HomeIcon className="h-5 w-5" />

      {collapsed ? (
        <span className="sr-only">Accueil</span>
      ) : (
        <span>Accueil</span>
      )}
    </a>
  </nav>
</aside>
```

Cette méthode est plus propre que de créer deux sidebars différentes.

## Comment ajouter des groupes et sous-menus ?

Pour une application plus grande, une seule liste devient rapidement difficile à parcourir. Regroupez les destinations selon leur fonction.

```html
<nav class="space-y-6 p-4">
  <div>
    <p class="mb-2 px-3 text-xs font-semibold uppercase tracking-wider text-gray-400">
      Général
    </p>

    <div class="space-y-1">
      <a href="#" class="block rounded-lg bg-gray-100 px-3 py-2 text-sm font-medium">
        Accueil
      </a>

      <a href="#" class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100">
        Activité
      </a>
    </div>
  </div>

  <div>
    <p class="mb-2 px-3 text-xs font-semibold uppercase tracking-wider text-gray-400">
      Gestion
    </p>

    <div class="space-y-1">
      <a href="#" class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100">
        Utilisateurs
      </a>

      <a href="#" class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100">
        Facturation
      </a>

      <a href="#" class="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100">
        Paramètres
      </a>
    </div>
  </div>
</nav>
```

Les groupes sont particulièrement utiles quand la navigation dépasse six ou sept destinations principales.

Pour les sous-menus, n’en abusez pas. Deux niveaux suffisent généralement. Une navigation latérale avec trois ou quatre niveaux devient plus difficile à comprendre qu’une structure de pages mieux organisée.

## Quelle largeur choisir pour une sidebar ?

Il n’existe pas une largeur parfaite pour tous les projets.

| Type de sidebar | Largeur pratique | Utilisation |
| --- | --- | --- |
| Icônes uniquement | `w-16` à `w-20` | Interface compacte |
| Navigation standard | `w-56` à `w-64` | Dashboard et SaaS |
| Labels longs | `w-72` | Outils complexes |
| Mobile | Plein écran ou drawer | Petits écrans |

Une largeur de `w-64` constitue un bon point de départ parce qu’elle laisse assez de place aux labels sans prendre une part excessive du viewport.

Ne choisissez cependant pas la largeur isolément. Testez aussi votre contenu principal sur un écran d’ordinateur portable. Une sidebar confortable sur un grand moniteur peut laisser trop peu de place à une table ou à un éditeur sur un écran plus petit.

## Quelles classes Tailwind sont les plus utiles pour une sidebar ?

Vous pouvez construire presque toutes les sidebars de cet article avec un petit ensemble de classes.

| Besoin | Classes utiles | Rôle |
| --- | --- | --- |
| Structure | `flex`, `flex-col`, `flex-1` | Organiser la mise en page |
| Dimensions | `w-64`, `h-screen`, `min-h-screen` | Définir la zone |
| Position | `fixed`, `sticky`, `inset-y-0`, `left-0` | Garder le menu en place |
| Responsive | `hidden`, `lg:block`, `lg:ml-64` | Adapter desktop et mobile |
| Espacement | `p-4`, `px-3`, `py-2`, `gap-3` | Donner de l’air |
| États | `hover:bg-gray-100`, `focus:*` | Montrer les interactions |
| Animation | `transition-all`, `duration-200` | Rétracter proprement |

Le piège est de chercher une classe spéciale « sidebar ». Tailwind ne vous impose pas ce composant. Vous assemblez les primitives nécessaires à votre propre navigation.

## Comment copier une sidebar dans React ?

La conversion vers React est directe. Remplacez `class` par `className`, transformez les éléments répétitifs en données, puis générez les liens avec `map()`.

```jsx
const navigation = [
  { name: "Accueil", href: "/" },
  { name: "Projets", href: "/projects" },
  { name: "Équipe", href: "/team" },
  { name: "Paramètres", href: "/settings" },
];

export default function Sidebar() {
  return (
    <aside className="flex h-screen w-64 flex-col border-r border-gray-200 bg-white">
      <div className="flex h-16 items-center px-5">
        <span className="font-semibold text-gray-900">
          Mon application
        </span>
      </div>

      <nav className="flex-1 space-y-1 p-3">
        {navigation.map((item) => (
          <a
            key={item.href}
            href={item.href}
            className="block rounded-lg px-3 py-2 text-sm text-gray-600 hover:bg-gray-100 hover:text-gray-900"
          >
            {item.name}
          </a>
        ))}
      </nav>
    </aside>
  );
}
```

Dans Next.js ou avec un routeur React, remplacez ensuite les balises `<a>` par le composant de navigation approprié et utilisez l’URL courante pour appliquer automatiquement le style actif.

## Comment transformer le code en vraie sidebar mobile ?

Le menu mobile demande un peu de logique. L’idée est d’afficher un overlay, puis une sidebar positionnée au-dessus de la page.

```jsx
{open && (
  <div className="fixed inset-0 z-50 lg:hidden">
    <button
      className="absolute inset-0 bg-black/40"
      onClick={() => setOpen(false)}
      aria-label="Fermer le menu"
    />

    <aside className="relative h-full w-72 bg-white shadow-xl">
      <div className="flex h-16 items-center justify-between border-b px-4">
        <span className="font-semibold">Menu</span>

        <button
          onClick={() => setOpen(false)}
          className="rounded-lg p-2 hover:bg-gray-100"
        >
          Fermer
        </button>
      </div>

      <nav className="space-y-1 p-4">
        <a href="/" className="block rounded-lg px-3 py-2 hover:bg-gray-100">
          Accueil
        </a>

        <a href="/projects" className="block rounded-lg px-3 py-2 hover:bg-gray-100">
          Projets
        </a>

        <a href="/settings" className="block rounded-lg px-3 py-2 hover:bg-gray-100">
          Paramètres
        </a>
      </nav>
    </aside>
  </div>
)}
```

Pour une version de production, pensez aussi au focus clavier, à la touche Échap, au blocage du scroll derrière le menu et aux attributs ARIA appropriés.

## Comment rendre une sidebar moins générique ?

Le problème n’est généralement pas Tailwind. Le problème vient plutôt d’une interface construite uniquement à partir de décisions génériques : même rayon de bordure partout, mêmes gris, même espacement, mêmes icônes et aucun rapport entre la sidebar et le reste du produit.

Commencez par définir quelques règles visuelles :

1. Une largeur constante.
2. Une seule logique d’espacement.
3. Un état actif immédiatement identifiable.
4. Deux ou trois niveaux de texte maximum.
5. Un traitement cohérent des icônes.
6. Un bloc utilisateur séparé de la navigation.
7. Un comportement mobile décidé avant d’ajouter les détails visuels.

Si vous créez une application avec Cursor, Claude Code ou un autre agent de code, une référence visuelle concrète est souvent plus efficace qu’un long prompt décrivant vaguement une interface « moderne ».

C’est précisément là qu’un point de départ comme [VP0.com](https://vp0.com) peut être utile : vous partez d’un design d’app existant, puis votre outil de création par IA reconstruit l’interface à partir de la source du design plutôt que d’interpréter uniquement des adjectifs.

VP0.com est particulièrement pertinent lorsque la sidebar n’est qu’une partie d’une interface plus grande. Vous pouvez ainsi réfléchir à la navigation avec le dashboard, les réglages, le profil et les autres écrans au lieu d’optimiser un composant isolé.

## Comment demander à une IA de créer cette sidebar ?

Évitez un prompt comme :

```text
Create a beautiful modern sidebar with Tailwind CSS.
```

Il laisse presque toutes les décisions à l’outil.

Préférez une demande structurée :

```text
Create a responsive application sidebar with Tailwind CSS.

Desktop:
- 256px width
- fixed left navigation
- white background
- subtle right border
- logo at the top
- grouped navigation
- clear active state
- user profile section at the bottom

Mobile:
- hide the desktop sidebar
- show a menu button in the header
- open the navigation as a left drawer
- add a dark overlay
- close on overlay click

Accessibility:
- visible keyboard focus
- labels for icon-only buttons
- keep navigation text available to screen readers

Keep the visual design minimal.
Do not add gradients or excessive shadows.
```

Ce prompt décrit une architecture et un comportement. L’IA doit donc prendre moins de décisions arbitraires.

Vous pouvez ensuite demander une seconde passe uniquement pour le design :

```text
Keep the structure and behavior unchanged.

Improve only:
- typography hierarchy
- spacing
- active navigation state
- icon consistency
- border treatment
- profile section

Do not change the routes or component API.
```

Séparer structure et finition évite qu’une modification visuelle casse toute la navigation.

## Quelles erreurs éviter avec une sidebar Tailwind ?

La première erreur est de concevoir uniquement la version desktop. Une navigation latérale fixe de 256 pixels n’est pas une solution mobile.

La deuxième est de dépendre uniquement de la couleur pour l’état actif. Utilisez aussi un fond, un changement de graisse ou un autre signal visuel.

La troisième est de réduire la sidebar à des icônes sans conserver de labels accessibles. Une icône évidente pour son créateur ne l’est pas forcément pour chaque utilisateur.

La quatrième est d’ajouter trop d’animations. Une transition courte lors de la réduction du menu suffit. La navigation ne devrait pas bouger en permanence.

La cinquième est de placer chaque action dans la sidebar. Les actions liées au contenu actuel peuvent rester dans la page. La sidebar sert d’abord à naviguer entre les grandes zones du produit.

Enfin, évitez de copier un composant sans vérifier ses hypothèses. Un exemple peut utiliser une largeur fixe, un routeur différent, des icônes absentes de votre projet ou une logique mobile qui ne correspond pas à votre application.

## Quelle structure choisir selon votre projet ?

Pour un petit dashboard, choisissez la première sidebar : simple, fixe et avec quatre ou cinq destinations.

Pour un SaaS plus dense, utilisez des groupes, une zone utilisateur en bas et éventuellement une version rétractable.

Pour une application très visuelle, gardez la navigation discrète afin qu’elle ne concurrence pas le contenu.

Pour un outil utilisé pendant plusieurs heures, testez également l’état réduit. Les icônes peuvent conserver l’accès aux sections tout en libérant davantage de place pour l’éditeur ou le dashboard.

Pour une application mobile, ne cherchez pas forcément à reproduire une sidebar desktop. Une navigation par onglets ou une autre structure peut être plus naturelle selon les écrans.

## Quelle est la limite du copier-coller ?

Un composant prêt à copier accélère le premier écran, mais il ne connaît pas votre architecture.

Les routes, permissions, rôles utilisateur, états actifs, sous-menus, données du profil et comportements mobiles doivent être raccordés à votre application.

Même chose pour VP0.com : un design fournit un point de départ d’interface, pas le backend complet. L’authentification, les données, les paiements et la logique métier restent à construire dans votre projet.

Considérez donc le code copié comme une base à adapter, pas comme une fonctionnalité terminée.

## À retenir : quelle sidebar Tailwind CSS copier en 2026 ?

Pour la majorité des projets, commencez avec une sidebar de `w-64`, une navigation verticale simple, un état actif visible et une version mobile séparée. Ajoutez ensuite les groupes, les icônes et le mode rétractable uniquement si votre produit en a réellement besoin.

Tailwind est particulièrement pratique ici parce que la structure, les états et les variantes responsives peuvent rester lisibles directement dans le composant. La [documentation Tailwind CSS](https://tailwindcss.com/docs/responsive-design) permet de vérifier le fonctionnement actuel des breakpoints et des variantes responsives avant d’adapter le code à votre projet.

Si votre problème dépasse la sidebar et concerne l’identité visuelle de toute l’application, partez plutôt d’une interface cohérente. VP0.com peut alors servir de référence pour donner à votre agent de code une direction plus concrète qu’un simple prompt esthétique.

## Questions fréquentes

### Comment créer une sidebar avec Tailwind CSS ?

Utilisez une structure `flex`, donnez une largeur fixe à l’élément `aside`, puis laissez le contenu principal prendre l’espace restant avec `flex-1`. Ajoutez ensuite les classes responsives nécessaires pour masquer ou transformer la sidebar sur mobile.

### Quelle largeur utiliser pour une sidebar Tailwind ?

`w-64` est un excellent point de départ pour une sidebar classique avec icônes et labels. `w-16` ou `w-20` convient mieux à une navigation compacte avec icônes, tandis que `w-72` peut être utile lorsque les intitulés sont longs.

### Comment faire une sidebar responsive avec Tailwind CSS ?

Construisez d’abord le comportement mobile, puis utilisez les variantes `md:` ou `lg:` pour le desktop. Une solution courante consiste à masquer la sidebar fixe sur mobile et à ouvrir la navigation sous forme de drawer avec un bouton de menu.

### Comment garder une sidebar fixe pendant le scroll ?

Utilisez `fixed`, `inset-y-0` et `left-0` pour une sidebar indépendante du contenu. Si vous souhaitez qu’elle suive le scroll uniquement dans son conteneur, `sticky` avec une position supérieure adaptée peut être plus approprié.

### Quelle est la meilleure sidebar Tailwind CSS à copier ?

La meilleure base est généralement la plus simple qui couvre réellement votre navigation : largeur fixe sur desktop, drawer sur mobile, état actif clair et labels accessibles. Si vous avez besoin d’une direction visuelle pour l’ensemble d’une app créée avec l’IA, VP0.com constitue aussi un point de départ pratique avant d’adapter la navigation à votre produit.
