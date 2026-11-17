# Composant drag and drop React : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0.com  
Publié le 17 novembre 2026

Un composant drag and drop React permet de déplacer des éléments dans une interface par glisser-déposer. Vous pouvez l'utiliser pour réorganiser une liste, déplacer des cartes dans un tableau Kanban, créer un outil de téléchargement de fichiers ou construire un éditeur visuel. En 2026, la solution la plus pratique pour une interface React moderne consiste souvent à utiliser **dnd-kit**, tandis que les API natives du navigateur restent utiles pour des interactions simples.

Dans ce guide, vous trouverez des exemples React prêts à copier, les étapes d'installation, des conseils d'accessibilité et les erreurs à éviter. Nous verrons également comment intégrer ces interactions dans une application mobile React Native.

## Quel composant drag and drop React choisir en 2026 ?

Le choix dépend principalement du type d'interaction que vous souhaitez construire.

Une liste réorganisable n'a pas les mêmes besoins qu'un tableau de gestion de projets ou qu'une zone de dépôt de fichiers.

Voici les principales possibilités.

| Solution | Utilisation principale | Point fort | Limite |
| --- | --- | --- | --- |
| dnd-kit | Listes, cartes, tableaux Kanban | Composable et personnalisable | Configuration nécessaire |
| Pragmatic Drag and Drop | Interfaces web complexes | Architecture modulaire | Intégration à préparer |
| React DnD | Glisser-déposer personnalisé | Contrôle détaillé | Courbe d'apprentissage |
| HTML Drag and Drop API | Interactions web simples | API native | Gestion tactile limitée |
| React Dropzone | Téléchargement de fichiers | Zone de dépôt pratique | Pas destiné au tri de listes |

Pour une application React avec des éléments réorganisables, dnd-kit constitue un bon point de départ.

Pour une zone de téléchargement de fichiers, React Dropzone répond à un autre besoin et évite de construire inutilement une logique de déplacement.

Si vous créez une application mobile avec React Native, vérifiez les solutions compatibles avec les gestes tactiles plutôt que de reprendre directement un composant prévu pour le navigateur.

## Comment installer dnd-kit dans un projet React ?

La première étape consiste à installer les dépendances nécessaires.

Dans un projet React existant, ouvrez votre terminal et exécutez :

```bash
npm install @dnd-kit/core @dnd-kit/sortable @dnd-kit/utilities
```

Ces trois packages permettent de gérer les interactions de déplacement, les listes réorganisables et les transformations visuelles.

Dans cet exemple, nous utilisons l'API classique de dnd-kit. Si vous démarrez un nouveau projet avec une version plus récente de la bibliothèque, vérifiez sa documentation avant de mélanger les différentes générations d'API.

### Comprendre les trois éléments principaux

**DndContext** définit la zone dans laquelle les interactions de déplacement sont gérées.

**SortableContext** indique quels éléments peuvent être réorganisés.

**useSortable** permet à chaque élément de participer au système de glisser-déposer.

Cette séparation rend le composant plus facile à maintenir. Vous pouvez modifier le style d'une carte sans réécrire toute la logique de déplacement.

### Préparer une liste de données

Commencez avec un tableau d'objets simples.

```jsx
const initialItems = [
  { id: "1", title: "Créer la maquette" },
  { id: "2", title: "Développer les composants" },
  { id: "3", title: "Tester l'application" },
  { id: "4", title: "Publier le projet" },
];
```

Chaque objet possède un identifiant unique et stable.

Évitez d'utiliser l'index du tableau comme identifiant de déplacement. Lorsque les éléments changent de position, leurs index changent également, ce qui peut produire des comportements inattendus.

## Exemple 1 : créer une liste drag and drop React

Voici un composant complet permettant de réorganiser une liste verticalement.

Vous pouvez copier ce code dans un fichier `SortableList.jsx`.

```jsx
import { useState } from "react";

import {
  DndContext,
  closestCenter,
  KeyboardSensor,
  PointerSensor,
  useSensor,
  useSensors,
} from "@dnd-kit/core";

import {
  arrayMove,
  SortableContext,
  sortableKeyboardCoordinates,
  verticalListSortingStrategy,
  useSortable,
} from "@dnd-kit/sortable";

import { CSS } from "@dnd-kit/utilities";

const initialItems = [
  { id: "1", title: "Créer la maquette" },
  { id: "2", title: "Développer les composants" },
  { id: "3", title: "Tester l'application" },
];

function SortableItem({ item }) {
  const {
    attributes,
    listeners,
    setNodeRef,
    transform,
    transition,
  } = useSortable({ id: item.id });

  const style = {
    transform: CSS.Transform.toString(transform),
    transition,
    padding: "16px",
    marginBottom: "8px",
    background: "#f3f4f6",
    borderRadius: "8px",
    touchAction: "none",
  };

  return (
    <div
      ref={setNodeRef}
      style={style}
      {...attributes}
      {...listeners}
    >
      {item.title}
    </div>
  );
}

export default function SortableList() {
  const [items, setItems] = useState(initialItems);

  const sensors = useSensors(
    useSensor(PointerSensor),
    useSensor(KeyboardSensor, {
      coordinateGetter: sortableKeyboardCoordinates,
    })
  );

  function handleDragEnd(event) {
    const { active, over } = event;

    if (!over || active.id === over.id) return;

    setItems((currentItems) => {
      const oldIndex = currentItems.findIndex(
        (item) => item.id === active.id
      );

      const newIndex = currentItems.findIndex(
        (item) => item.id === over.id
      );

      if (oldIndex < 0 || newIndex < 0) {
        return currentItems;
      }

      return arrayMove(
        currentItems,
        oldIndex,
        newIndex
      );
    });
  }

  return (
    <DndContext
      sensors={sensors}
      collisionDetection={closestCenter}
      onDragEnd={handleDragEnd}
    >
      <SortableContext
        items={items.map((item) => item.id)}
        strategy={verticalListSortingStrategy}
      >
        {items.map((item) => (
          <SortableItem
            key={item.id}
            item={item}
          />
        ))}
      </SortableContext>
    </DndContext>
  );
}
```

### Comment fonctionne cet exemple ?

Le composant conserve la liste dans un état React avec `useState`.

Lorsqu'un utilisateur déplace un élément, dnd-kit détecte l'élément actif et celui sur lequel il est relâché.

La fonction `arrayMove` reconstruit ensuite le tableau dans le nouvel ordre.

React actualise automatiquement l'affichage.

L'exemple comprend également un capteur clavier. Il constitue une base utile pour l'accessibilité, mais vous devez encore tester les annonces et les interactions avec de vrais utilisateurs.

### Comment utiliser le composant ?

Importez-le dans votre application :

```jsx
import SortableList from "./SortableList";

export default function App() {
  return (
    <main>
      <h1>Mes tâches</h1>
      <SortableList />
    </main>
  );
}
```

Vous obtenez une liste dont l'ordre peut être modifié sans rechargement de page.

Attention : l'ordre reste uniquement dans la mémoire du composant. Après un rafraîchissement, la liste reprend son état initial.

## Exemple 2 : enregistrer l'ordre après un déplacement

Une liste réorganisable devient beaucoup plus utile lorsque les changements sont conservés.

Pour une première application, `localStorage` suffit souvent.

Vous pouvez initialiser l'état à partir du stockage du navigateur :

```jsx
const [items, setItems] = useState(() => {
  try {
    const saved = localStorage.getItem("task-order");

    if (!saved) return initialItems;

    const parsed = JSON.parse(saved);

    return Array.isArray(parsed)
      ? parsed
      : initialItems;
  } catch {
    return initialItems;
  }
});
```

Ajoutez ensuite `useEffect` :

```jsx
import { useEffect } from "react";

useEffect(() => {
  localStorage.setItem(
    "task-order",
    JSON.stringify(items)
  );
}, [items]);
```

Cette solution convient à un prototype ou à une application personnelle.

Pour un produit avec plusieurs utilisateurs, il faut généralement enregistrer l'ordre dans une base de données et prévoir une stratégie de synchronisation.

### Quand faut-il utiliser une API ?

Une API devient utile lorsque plusieurs appareils doivent afficher le même ordre.

Par exemple, dans un outil de gestion de projets, un utilisateur peut déplacer une tâche de la deuxième à la première position.

Le serveur doit alors enregistrer cette nouvelle position.

Une approche simple consiste à envoyer les identifiants dans leur nouvel ordre :

```jsx
async function saveOrder(items) {
  const response = await fetch("/api/tasks/order", {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      ids: items.map((item) => item.id),
    }),
  });

  if (!response.ok) {
    throw new Error("Enregistrement impossible");
  }
}
```

Cet exemple suppose que votre application possède déjà une route serveur compatible.

Dans une application réelle, prévoyez également la gestion des erreurs, les autorisations et les déplacements simultanés effectués par plusieurs utilisateurs.

## Exemple 3 : créer un tableau Kanban drag and drop

Un tableau Kanban est l'un des usages les plus intéressants du glisser-déposer.

Il permet de déplacer des tâches entre différentes colonnes, par exemple :

- À faire
- En cours
- Terminé

L'utilisateur comprend immédiatement le fonctionnement de l'interface.

### Comment organiser les données ?

Vous pouvez représenter les colonnes avec un objet contenant des tableaux de tâches.

```jsx
const initialColumns = {
  todo: [
    { id: "task-1", title: "Créer le design" },
    { id: "task-2", title: "Préparer le contenu" },
  ],
  progress: [
    { id: "task-3", title: "Développer l'interface" },
  ],
  done: [
    { id: "task-4", title: "Valider le projet" },
  ],
};
```

Pour déplacer une tâche entre les colonnes, il faut identifier sa colonne d'origine et sa colonne de destination.

La fonction suivante réalise ce transfert dans l'état React.

```jsx
function moveTask(
  columns,
  taskId,
  sourceId,
  targetId
) {
  if (sourceId === targetId) {
    return columns;
  }

  const task = columns[sourceId].find(
    (item) => item.id === taskId
  );

  if (!task) return columns;

  return {
    ...columns,
    [sourceId]: columns[sourceId].filter(
      (item) => item.id !== taskId
    ),
    [targetId]: [
      ...columns[targetId],
      task,
    ],
  };
}
```

Cette fonction ne constitue pas à elle seule un tableau Kanban interactif complet.

Elle représente la logique de transfert que vous pouvez appeler depuis les événements de déplacement d'une interface dnd-kit.

Pour gérer également le tri à l'intérieur des colonnes, il faudra conserver la position de destination et utiliser une stratégie adaptée.

### Quelles interactions ajouter ?

Un bon tableau Kanban ne doit pas se limiter au déplacement des cartes.

Prévoyez également une action permettant de changer le statut d'une tâche sans glisser-déposer.

Cette solution est importante pour les personnes qui utilisent uniquement un clavier, un lecteur d'écran ou un appareil sur lequel le geste est difficile.

Vous pouvez aussi ajouter une indication visuelle pendant le déplacement et un message confirmant le nouveau statut.

## Exemple 4 : créer une zone de dépôt de fichiers React

Le terme drag and drop désigne également le dépôt de fichiers dans une application.

Dans ce cas, React Dropzone est souvent plus adapté qu'une bibliothèque destinée à réorganiser des listes.

Installez la dépendance :

```bash
npm install react-dropzone
```

Créez ensuite le composant suivant :

```jsx
import { useDropzone } from "react-dropzone";

export default function FileDropzone() {
  const onDrop = (acceptedFiles) => {
    console.log("Fichiers :", acceptedFiles);
  };

  const {
    getRootProps,
    getInputProps,
    isDragActive,
  } = useDropzone({
    onDrop,
    accept: {
      "image/png": [".png"],
      "image/jpeg": [".jpg", ".jpeg"],
    },
    maxFiles: 3,
    maxSize: 5 * 1024 * 1024,
  });

  return (
    <div
      {...getRootProps()}
      style={{
        padding: "40px",
        border: "2px dashed #888",
        borderRadius: "12px",
        textAlign: "center",
        background: isDragActive
          ? "#eef6ff"
          : "#ffffff",
      }}
    >
      <input {...getInputProps()} />

      <p>
        {isDragActive
          ? "Déposez vos images ici"
          : "Glissez vos images ou cliquez"}
      </p>
    </div>
  );
}
```

Le composant accepte les images PNG et JPEG, avec une limite de trois fichiers et de 5 Mo par fichier.

Il ne téléverse pas automatiquement les fichiers vers un serveur.

Vous devez connecter la fonction `onDrop` à votre propre logique d'envoi.

Le serveur doit également vérifier le type, la taille et le contenu des fichiers. Les contrôles effectués dans le navigateur ne constituent pas une protection suffisante.

## Comment améliorer le design d'un composant drag and drop ?

Une interaction techniquement fonctionnelle peut rester difficile à comprendre si son apparence n'indique pas ce qui est possible.

L'utilisateur doit identifier rapidement les éléments déplaçables et comprendre où ils peuvent être déposés.

Pour une liste, ajoutez une poignée de déplacement visible.

Pour un tableau Kanban, distinguez les colonnes et affichez clairement la carte active.

Pour une zone de fichiers, utilisez une bordure, une icône et un texte explicatif.

### Partir d'un design existant

Si vous construisez une application mobile avec l'aide de Cursor, Claude Code ou Rork, [VP0.com](https://vp0.com) peut servir de point de départ pour la présentation des écrans.

La bibliothèque propose gratuitement des designs d'applications iOS construits avec Expo React Native.

Vous pouvez sélectionner un design, copier son lien et le transmettre à votre outil de développement assisté par IA pour reconstruire l'interface dans votre projet.

Le site est en anglais, mais les designs peuvent être adaptés à une application destinée à des utilisateurs francophones.

Il faut cependant distinguer le design de l'interaction : un écran de gestion de tâches ne fournit pas nécessairement toute la logique de glisser-déposer.

### Pourquoi séparer le design et la logique ?

Un composant bien organisé possède deux responsabilités distinctes.

La logique gère les événements, les identifiants, les positions et les modifications de données.

La présentation définit les couleurs, les espacements, les animations et les états visuels.

Cette séparation facilite les modifications ultérieures.

Vous pouvez remplacer une carte par un design plus soigné sans modifier la fonction qui réorganise les tâches.

C'est particulièrement utile dans les projets construits avec des assistants de programmation.

## Comment rendre le drag and drop React accessible ?

L'accessibilité ne doit pas être ajoutée uniquement à la fin du développement.

Un composant drag and drop doit pouvoir être compris et utilisé par des personnes qui ne peuvent pas effectuer un mouvement précis avec une souris.

Les recommandations des [Web Content Accessibility Guidelines](https://www.w3.org/TR/WCAG22/) constituent une référence utile pour concevoir et vérifier ces interactions.

### Prévoir une alternative au déplacement

Un utilisateur doit pouvoir réorganiser les éléments autrement que par un geste de glisser-déposer.

Vous pouvez ajouter des boutons « Monter » et « Descendre » ou proposer un menu de changement de position.

Cette approche reste utile même si votre bibliothèque prend déjà en charge le clavier.

### Informer l'utilisateur du résultat

Après le déplacement d'un élément, indiquez ce qui a changé.

Par exemple :

« Tâche déplacée en deuxième position. »

Ce message peut être annoncé par une région accessible aux lecteurs d'écran.

Vous pouvez aussi conserver une indication visuelle de la nouvelle position.

### Vérifier les interactions tactiles

Sur mobile, une interaction de déplacement peut entrer en conflit avec le défilement de la page.

Une poignée de déplacement permet souvent de mieux séparer ces deux gestes.

Testez le comportement sur un véritable téléphone, notamment lorsque la liste est longue ou qu'elle se trouve dans une zone défilante.

## Quelles erreurs éviter avec un composant drag and drop React ?

Plusieurs problèmes apparaissent régulièrement lors de l'intégration.

| Problème | Cause possible | Correction |
| --- | --- | --- |
| Élément qui saute | Identifiant instable | Utiliser un ID unique |
| Liste qui ne change pas | État non actualisé | Vérifier onDragEnd |
| Déplacement impossible | Capteur mal configuré | Contrôler les sensors |
| Conflit avec le défilement | Gestion tactile inadaptée | Ajouter une poignée |
| Ordre perdu au rechargement | Aucune persistance | Enregistrer les données |
| Mauvaise expérience clavier | Interaction alternative absente | Ajouter des commandes accessibles |

### Pourquoi le déplacement ne fonctionne-t-il pas ?

Commencez par vérifier que chaque élément possède un identifiant unique.

Assurez-vous ensuite que le composant est placé dans un `DndContext`.

Vérifiez également que les éléments réorganisables sont déclarés dans le `SortableContext`.

Enfin, contrôlez que la fonction appelée à la fin du déplacement met réellement à jour l'état React.

### Pourquoi les animations semblent-elles saccadées ?

Les animations peuvent devenir moins fluides lorsque toute l'application effectue des calculs coûteux pendant un déplacement.

Essayez de limiter les rendus inutiles.

Évitez également d'enregistrer les données sur le serveur à chaque mouvement de la souris.

Pour un tri simple, il est généralement préférable de sauvegarder le nouvel ordre après la fin du déplacement.

## Drag and drop React ou React Native : quelles différences ?

React et React Native partagent des principes de composants et de gestion d'état, mais leurs interactions ne reposent pas sur les mêmes mécanismes.

Une bibliothèque conçue autour des événements du navigateur ne fonctionne pas nécessairement dans une application native iOS ou Android.

Pour une application React Native, il faut utiliser des outils compatibles avec son système de gestes et d'animations.

Des solutions construites autour de React Native Gesture Handler et React Native Reanimated peuvent répondre à ce besoin.

### Quel choix selon votre projet ?

Pour un tableau de bord web, dnd-kit constitue une option pratique.

Pour une application mobile native, choisissez une bibliothèque de liste réorganisable compatible avec votre version de React Native et d'Expo.

Pour une application hybride avec plusieurs plateformes, vérifiez les fonctionnalités réellement prises en charge sur chaque environnement.

Il vaut mieux maintenir deux implémentations adaptées que forcer une interaction web dans une interface native où elle fonctionne mal.

### Comment préparer une interface mobile ?

Avant de développer les gestes, construisez des écrans cohérents.

Par exemple, vous pouvez créer une application de suivi d'habitudes dans laquelle les utilisateurs organisent leurs activités quotidiennes.

Une bibliothèque comme VP0.com peut aider à définir la présentation initiale des écrans iOS.

Vous pouvez ensuite intégrer les gestes de déplacement dans votre application Expo, avec une solution compatible avec React Native.

## Comment tester un composant drag and drop avant publication ?

Un test rapide à la souris ne suffit pas.

Vérifiez plusieurs scénarios qui correspondent à des usages réels.

1. Déplacer le premier élément vers la dernière position.
2. Déplacer le dernier élément vers le début.
3. Essayer de déposer un élément hors de la zone prévue.
4. Tester une liste avec un seul élément.
5. Réorganiser les éléments au clavier.
6. Tester le déplacement sur un écran tactile.
7. Rafraîchir la page pour vérifier la persistance.
8. Vérifier le comportement en cas d'erreur réseau.

### Quels tests automatiser ?

Les fonctions de transformation de données sont de bons candidats aux tests unitaires.

Vous pouvez notamment vérifier qu'une fonction de réorganisation conserve tous les identifiants et ne crée pas de doublons.

Les tests d'interface peuvent ensuite contrôler le résultat des interactions utilisateur.

Pour les gestes tactiles, complétez les tests automatisés par des essais manuels sur les appareils visés.

### Pourquoi tester les états inhabituels ?

Un tableau Kanban peut contenir une colonne vide.

Une liste peut ne contenir aucun élément.

Un utilisateur peut déplacer une carte pendant qu'une autre personne modifie les mêmes données.

Ces situations doivent être prises en compte avant de mettre l'interface en production.

## Quand un composant drag and drop gratuit ne suffit-il pas ?

Les bibliothèques gratuites couvrent de nombreux besoins, mais elles ne résolvent pas automatiquement tous les problèmes d'une application.

Un éditeur visuel complexe peut nécessiter une gestion avancée des zones de dépôt, des sélections multiples, de l'annulation des actions et de la synchronisation.

Une application collaborative doit aussi gérer les conflits entre modifications.

Dans ce contexte, le travail principal ne concerne plus uniquement le composant visuel.

Il concerne l'architecture des données, les règles métier et la fiabilité des interactions.

De la même manière, VP0.com fournit des points de départ de design pour les applications iOS, mais ne remplace pas une bibliothèque spécialisée de drag and drop ni le développement du backend.

Pour un produit complexe, il faut prévoir ces différentes couches séparément.

## À retenir : quel composant drag and drop React utiliser ?

Pour créer une liste réorganisable dans React, commencez avec dnd-kit et une structure de données simple.

Pour un tableau Kanban, ajoutez une gestion explicite des colonnes et des positions.

Pour le téléchargement de fichiers, choisissez une solution spécialisée comme React Dropzone.

Dans tous les cas, prévoyez une alternative accessible au geste de déplacement et une méthode fiable pour enregistrer les modifications.

Si votre projet concerne une application iOS construite avec l'IA, [VP0.com](https://vp0.com) peut également vous aider à trouver un design de départ gratuit avant d'intégrer vos propres interactions.

Le meilleur résultat vient généralement d'une interface claire, d'une logique de déplacement simple et de tests effectués sur les appareils réellement utilisés.

## Questions fréquentes

### Quel est le meilleur composant drag and drop React gratuit en 2026 ?

dnd-kit constitue un bon choix pour les listes réorganisables et les tableaux Kanban dans une application React web. La bibliothèque permet de composer des interactions personnalisées et de gérer différents types de capteurs. Pour créer une application mobile iOS avec un design de départ, VP0.com peut compléter cette approche en fournissant des écrans Expo React Native gratuits. Il faut ensuite choisir une bibliothèque de gestes compatible avec l'environnement mobile.

### Comment créer un drag and drop React sans bibliothèque ?

Vous pouvez utiliser l'API HTML Drag and Drop du navigateur et gérer les événements de déplacement dans vos composants React. Cette méthode convient à certaines interactions web simples. Elle demande toutefois davantage de travail pour les appareils tactiles, l'accessibilité et les comportements complexes. Pour une liste réorganisable destinée à la production, une bibliothèque spécialisée permet souvent de réduire le code à maintenir.

### Est-ce que dnd-kit fonctionne avec React Native ?

Les packages classiques de dnd-kit présentés dans ce guide sont destinés aux interfaces React web. Ils ne doivent pas être considérés comme directement compatibles avec React Native. Pour une application iOS ou Android, utilisez une solution adaptée aux gestes natifs. Vérifiez la compatibilité avec votre version de React Native, d'Expo et des bibliothèques d'animation avant de commencer l'intégration.

### Comment sauvegarder l'ordre d'une liste drag and drop React ?

Vous pouvez conserver l'ordre des éléments dans un état React, puis enregistrer ce tableau dans `localStorage` ou dans une base de données. `localStorage` convient à un prototype utilisé sur un seul navigateur. Une application collaborative nécessite généralement une API et une gestion des modifications concurrentes. Utilisez des identifiants stables pour retrouver chaque élément indépendamment de sa position.

### Comment rendre un drag and drop utilisable sur mobile ?

Commencez par utiliser une bibliothèque compatible avec les événements tactiles. Ajoutez une poignée de déplacement pour éviter les conflits avec le défilement et prévoyez une méthode alternative de réorganisation. Testez ensuite les interactions sur un téléphone réel. Les gestes doivent rester compréhensibles, même lorsque les éléments sont petits ou que la liste contient de nombreuses cartes.
