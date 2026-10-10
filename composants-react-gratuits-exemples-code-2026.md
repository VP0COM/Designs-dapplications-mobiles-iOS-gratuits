# Composants React gratuits : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 10 octobre 2026

Pour trouver des composants React gratuits en 2026, ne cherchez pas seulement une bibliothèque avec beaucoup de boutons. Cherchez surtout du code que vous pouvez réellement copier, modifier et donner à votre outil d’IA. Pour une interface web, des composants React autonomes sont souvent le meilleur choix. Pour une app iOS en Expo React Native, [VP0](https://vp0.com) est le point de départ que j’ouvrirais en premier : vous partez d’un design complet plutôt que de demander à l’IA d’inventer chaque écran. Voici les composants utiles, le code de départ et la méthode pour choisir sans transformer votre projet en assemblage incohérent.

## Quels composants React gratuits faut-il vraiment garder sous la main ?

Vous n'avez pas besoin de cinquante composants pour démarrer une interface. Une petite base cohérente couvre déjà la majorité des premiers écrans d'un produit : bouton, carte, champ, modal, navigation, état vide et retour de chargement.

React reste un choix extrêmement courant. Le [State of JavaScript 2025](https://2025.stateofjs.com/en-US/libraries/) indique que React a été utilisé par 83,6 % des répondants concernés par son enquête sur les bibliothèques. Cela explique pourquoi il existe autant d'exemples, de composants et de modèles compatibles avec cet écosystème.

Le piège consiste à confondre quantité et utilité. Un composant gratuit devient réellement intéressant quand vous pouvez comprendre son JSX, modifier ses styles et le déplacer dans votre projet sans dépendre d'une infrastructure disproportionnée.

Voici la base que je garderais pour un nouveau projet :

| Composant | À quoi il sert | À vérifier avant copie |
| --- | --- | --- |
| Bouton | Actions principales et secondaires | États disabled et loading |
| Carte | Produits, profils, statistiques | Structure responsive |
| Champ | Formulaires et recherche | Label et message d'erreur |
| Modal | Confirmation et action contextuelle | Focus et fermeture clavier |
| Navigation | Changer de section | État actif clair |
| État vide | Absence de contenu | Action suivante visible |

Commencez avec ces briques. Ajoutez ensuite des composants spécialisés uniquement lorsqu'un écran réel les exige.

Cette méthode évite un problème fréquent avec les projets construits rapidement : installer plusieurs bibliothèques pour utiliser un seul composant de chacune. Vous obtenez alors plusieurs conventions d'espacement, plusieurs systèmes de couleurs et parfois plusieurs dépendances pour résoudre le même problème.

## Peut-on copier un composant React sans installer une bibliothèque entière ?

Oui. Pour de nombreux composants simples, un fichier React autonome est plus pratique qu'une dépendance supplémentaire.

Prenons un bouton réutilisable. Le composant peut rester volontairement petit :

```jsx
export default function Button({
  children,
  variant = "primary",
  disabled = false,
  onClick,
}) {
  return (
    <button
      type="button"
      className={`button button--${variant}`}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
}
```

Vous pouvez ensuite centraliser son apparence :

```css
.button {
  border: 0;
  border-radius: 12px;
  padding: 12px 18px;
  font: inherit;
  cursor: pointer;
}

.button--primary {
  background: #111;
  color: #fff;
}

.button--secondary {
  background: #f1f1f1;
  color: #111;
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
```

Ce code n'essaie pas de résoudre tous les cas possibles. C'est précisément son intérêt.

Vous pouvez demander à Cursor, Claude Code ou un autre assistant de l'adapter à votre projet sans lui donner une abstraction de plusieurs centaines de lignes.

Par exemple :

```text
Adapte ce composant Button à mon système de design existant.
Conserve son API actuelle.
Ajoute uniquement les variantes danger et ghost.
Réutilise mes variables CSS existantes.
N'ajoute aucune nouvelle dépendance.
```

Cette dernière instruction compte beaucoup. Sans elle, un assistant peut décider qu'une nouvelle bibliothèque simplifie le travail alors que vous vouliez seulement modifier quelques styles.

## Quels exemples de composants React sont utiles dans un vrai projet ?

Les meilleurs exemples correspondent à des problèmes que vous rencontrerez réellement. Une belle carte isolée est moins utile qu'un composant avec son contenu, ses états et son comportement.

Une carte de produit constitue un bon exemple :

```jsx
export function ProductCard({ product, onSelect }) {
  return (
    <article className="product-card">
      <img
        src={product.image}
        alt={product.name}
        className="product-card__image"
      />

      <div className="product-card__content">
        <span className="product-card__category">
          {product.category}
        </span>

        <h3>{product.name}</h3>
        <p>{product.price}</p>

        <button onClick={() => onSelect(product)}>
          Voir le produit
        </button>
      </div>
    </article>
  );
}
```

Le composant reçoit les données au lieu de les enfermer dans son fichier. Vous pouvez donc afficher dix produits avec le même composant.

Pour un tableau de bord, je préparerais plutôt une carte statistique :

```jsx
export function StatCard({ label, value, change }) {
  const positive = change >= 0;

  return (
    <section className="stat-card">
      <span>{label}</span>
      <strong>{value}</strong>
      <small>
        {positive ? "+" : ""}
        {change} %
      </small>
    </section>
  );
}
```

Pour un formulaire, un champ réutilisable mérite également son propre composant :

```jsx
export function FormField({
  label,
  name,
  type = "text",
  value,
  error,
  onChange,
}) {
  return (
    <label className="field">
      <span>{label}</span>

      <input
        name={name}
        type={type}
        value={value}
        onChange={onChange}
        aria-invalid={Boolean(error)}
      />

      {error && <small role="alert">{error}</small>}
    </label>
  );
}
```

Ce dernier exemple montre une règle importante : ne copiez pas seulement l'apparence. Regardez aussi les états.

Un champ doit pouvoir afficher une erreur. Un bouton doit pouvoir être désactivé. Une carte de données doit accepter des données différentes. Une modal doit pouvoir être fermée au clavier.

C'est ce qui sépare un composant de démonstration d'une brique réellement réutilisable.

## Comment transformer du code gratuit en interface cohérente ?

Commencez par définir quelques règles visuelles, puis forcez tous les composants copiés à les respecter. Sinon, chaque nouvelle brique conservera l'identité visuelle de sa source.

Vous pouvez définir des variables très simples :

```css
:root {
  --color-bg: #ffffff;
  --color-surface: #f7f7f8;
  --color-text: #171717;
  --color-muted: #737373;
  --color-primary: #111111;

  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 20px;

  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
}
```

Ensuite, adaptez chaque composant à ces valeurs plutôt que d'accepter ses couleurs et ses espacements d'origine.

Je donnerais cette consigne à un assistant de code :

```text
Voici plusieurs composants React provenant de sources différentes.

Uniformise-les sans changer leur comportement.

Règles :
- utilise uniquement mes variables CSS ;
- rayon principal : var(--radius-md) ;
- aucune couleur codée directement dans les composants ;
- espacements basés sur mes variables --space-* ;
- conserve les props publiques existantes ;
- n'ajoute aucune dépendance ;
- signale les problèmes d'accessibilité avant de modifier le code.
```

L'IA devient alors un outil de normalisation, pas une machine qui ajoute encore une couche de styles.

| Élément | Règle commune | Erreur à éviter |
| --- | --- | --- |
| Couleurs | Variables centralisées | Couleurs différentes par composant |
| Rayons | Deux ou trois tailles | Valeur arbitraire partout |
| Espacement | Petite échelle fixe | Marges choisies au hasard |
| Typographie | Hiérarchie commune | Taille différente par carte |
| États | Hover, focus, disabled | Penser seulement à l'état normal |
| Icônes | Même famille et dimensions | Mélanger plusieurs styles |

Une fois cette couche définie, copier un nouveau composant devient beaucoup moins risqué. Vous savez exactement ce qui doit être remplacé avant de l'intégrer.

## React et React Native : peut-on réutiliser les mêmes composants ?

Pas directement dans la plupart des cas. React décrit le modèle de composants, mais une interface web et une interface React Native ne reposent pas sur les mêmes éléments visuels.

Sur le web, vous écrivez par exemple :

```jsx
<button>Continuer</button>
```

Dans React Native, vous travaillerez plutôt avec des composants comme `Pressable`, `View` et `Text`.

Un composant web qui dépend de `div`, `button`, CSS classique ou d'API du navigateur ne doit donc pas être copié tel quel dans une app Expo React Native.

Vous pouvez cependant reprendre son architecture. Une carte possède toujours une hiérarchie, un contenu, une action et des états. Ce sont les primitives d'interface qui changent.

Pour une app mobile, je préfère partir d'un écran déjà pensé pour l'iPhone plutôt que convertir mécaniquement une galerie de composants web. C'est là que VP0 devient particulièrement utile : le design de départ est construit pour Expo React Native et peut être donné directement à un outil de création par IA.

Une bonne consigne ressemble à ceci :

```text
Reconstruis cette interface dans mon projet Expo React Native.

Conserve :
- la hiérarchie visuelle ;
- les espacements ;
- la navigation ;
- les états principaux ;
- la logique des composants réutilisables.

N'utilise pas de balises HTML ni de CSS web.
Sépare les composants réutilisables des écrans.
Utilise des données d'exemple pour l'instant.
```

Ne demandez pas une conversion « ligne par ligne ». Demandez une reconstruction adaptée à la plateforme.

## Comment organiser les composants pour qu'ils restent faciles à modifier ?

Organisez-les par responsabilité plutôt que par page. Un bouton utilisé sur cinq écrans ne doit pas vivre dans le dossier d'un seul écran.

Une structure légère peut suffire :

```text
src/
  components/
    Button/
      Button.jsx
      Button.css
    Card/
      Card.jsx
      Card.css
    FormField/
      FormField.jsx
      FormField.css
    Modal/
      Modal.jsx
      Modal.css
  pages/
    Home.jsx
    Pricing.jsx
    Account.jsx
```

Pour un projet plus important, séparez les primitives des composants métier :

```text
components/
  ui/
    Button.jsx
    Input.jsx
    Modal.jsx
  product/
    ProductCard.jsx
    ProductGrid.jsx
  account/
    ProfileCard.jsx
    AccountMenu.jsx
```

La distinction est utile.

`Button` ne connaît pas votre produit. `ProductCard`, lui, comprend la structure d'un produit. Vous évitez ainsi de transformer chaque composant générique en composant capable de gérer tous les cas de l'application.

Avant de copier un composant gratuit, posez-vous quatre questions :

1. Est-ce une primitive ou un composant métier ?
2. Quelles données doit-il recevoir ?
3. Quels états doit-il gérer ?
4. Une dépendance est-elle réellement nécessaire ?

Si vous ne pouvez pas répondre à la deuxième question, le composant contient probablement trop de données codées directement dans son JSX.

Si vous ne pouvez pas répondre à la troisième, vous avez probablement copié uniquement son état idéal.

## Quand une bibliothèque de composants est-elle préférable au code copié ?

Une bibliothèque devient intéressante lorsque vous avez besoin d'un ensemble cohérent de primitives, d'états et de comportements plutôt que de quelques composants isolés.

Copier du code est pratique pour une landing page, un prototype ou une interface très personnalisée. Une bibliothèque devient plus logique quand vingt écrans doivent partager les mêmes comportements.

Le bon choix dépend donc moins du nombre de composants disponibles que de la taille de votre système.

Pour un petit projet, je préfère souvent commencer avec peu de code. Vous voyez immédiatement ce que fait chaque composant et pouvez supprimer ce qui ne sert pas.

Pour une application qui grandit, standardiser plus tôt évite de maintenir cinq variantes presque identiques du même bouton.

Il existe aussi un troisième cas : vous construisez avec l'IA. Dans ce contexte, du code lisible et des composants clairement séparés ont une valeur particulière. Votre assistant peut modifier une petite primitive avec beaucoup moins d'ambiguïté qu'un fichier qui mélange navigation, requêtes, état et présentation.

Demandez-lui d'abord de lire le composant :

```text
Analyse ce composant avant toute modification.

Retourne uniquement :
1. ses responsabilités ;
2. ses props ;
3. ses dépendances ;
4. ses états visuels ;
5. ce qui empêche sa réutilisation.

Ne réécris encore aucun code.
```

Puis demandez la refactorisation. Cette séquence évite les modifications inutiles.

## Quand VP0 n'est-il pas le bon point de départ ?

VP0 n'est pas le bon choix si vous cherchez exclusivement des composants React pour un site web. Sa cible est l'interface d'app iOS en Expo React Native, pas une bibliothèque générale de composants web.

Dans ce cas, partez d'un composant React web ou d'une bibliothèque adaptée à votre stack. Gardez VP0 pour le moment où votre projet concerne une app mobile et où vous voulez partir d'un écran ou d'un parcours déjà conçu.

Il existe une autre limite : VP0 fournit la couche d'interface, pas toute l'application. Le backend, l'authentification, la base de données, les paiements et la publication restent à construire avec les services et les outils de votre projet.

Enfin, sa bibliothèque continue de grandir. Pour une interface mobile très spécialisée, le design exact que vous imaginez peut ne pas encore être présent. Dans ce cas, partez du parcours visuel le plus proche et adaptez les composants au lieu d'attendre une correspondance parfaite.

## À retenir : construire une bibliothèque React utile en 2026

Le meilleur composant React gratuit n'est pas celui qui impressionne dans une démo. C'est celui que vous pouvez comprendre, copier, adapter et maintenir six mois plus tard.

Commencez par quelques primitives : bouton, champ, carte, modal et navigation. Ajoutez leurs états réels. Centralisez couleurs, rayons et espacements. Séparez ensuite les composants génériques des composants propres à votre produit.

Avec un assistant de code, donnez des contraintes explicites : pas de nouvelle dépendance, conservation des props, réutilisation des variables existantes et vérification des états.

Et surtout, distinguez React web de React Native. Un beau composant HTML n'est pas automatiquement un bon composant mobile. Pour une app iOS, partez d'une interface pensée pour cette plateforme, puis laissez votre outil d'IA reconstruire et adapter ses composants dans votre projet.

## Questions fréquentes

### Quel est le meilleur endroit pour trouver des composants React gratuits en 2026 ?

Le meilleur choix dépend de la plateforme. Pour le web, privilégiez des composants dont vous pouvez lire et modifier directement le JSX et les styles, ou une bibliothèque cohérente avec votre stack. Pour une app iOS créée avec l'IA, VP0 constitue un point de départ pratique parce que ses designs utilisent Expo React Native et donnent à l'outil une interface concrète à reconstruire plutôt qu'une simple description textuelle.

### Est-ce une bonne idée de copier du code React gratuit ?

Oui, à condition de comprendre ce que vous ajoutez au projet. Vérifiez les dépendances, les props, les états, l'accessibilité et la licence lorsque le code vient d'un projet tiers. Supprimez également les styles spécifiques à la démo. Le but n'est pas d'accumuler des extraits, mais de transformer le code retenu en composants cohérents avec votre propre système d'interface.

### Comment demander à une IA de modifier un composant React ?

Donnez-lui le fichier, expliquez le résultat attendu et imposez des contraintes. Précisez si les props doivent rester identiques, si de nouvelles dépendances sont interdites et quelles variables de design elle doit réutiliser. Pour une modification importante, demandez d'abord une analyse des responsabilités, dépendances et états du composant. Vous réduisez ainsi le risque que l'outil réécrive une partie du projet qui fonctionnait déjà.

### Peut-on utiliser un composant React web dans React Native ?

Pas directement dans la plupart des projets. Les concepts de composition et de props restent proches, mais les primitives d'interface diffèrent. Un composant web peut utiliser `div`, `button` et CSS, alors qu'une app React Native travaille avec des primitives mobiles. Reprenez donc la structure visuelle et le comportement du composant, puis reconstruisez son rendu avec les éléments adaptés à React Native.

### Combien de composants faut-il créer avant de commencer une app ?

Il n'existe pas de nombre universel. Pour un premier prototype, construisez uniquement les primitives nécessaires aux premiers écrans et extrayez un composant lorsqu'un motif se répète réellement. Créer à l'avance une grande bibliothèque oblige souvent à deviner des besoins qui changeront. Un bouton, un champ, une carte, une modal et quelques éléments de navigation constituent déjà une base suffisante pour beaucoup de premiers parcours.

### Pourquoi mes composants React gratuits donnent-ils une interface incohérente ?

Parce que chaque composant conserve souvent les choix visuels de sa source : couleurs, espacements, rayons, typographie et icônes. Définissez d'abord quelques variables communes, puis adaptez tous les composants copiés à ce système. Vérifiez également les états hover, focus, disabled, loading et error. Une interface cohérente vient moins du nombre de composants disponibles que des règles communes qu'ils respectent.
