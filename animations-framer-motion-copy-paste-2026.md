# Animations Framer Motion copy paste : guide complet, exemples et conseils en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 8 octobre 2026

Pour ajouter des animations Framer Motion par copier-coller, commencez par un composant React simple : une carte qui apparaît, un bouton qui réagit ou un panneau qui se ferme progressivement. En 2026, la bibliothèque est présentée sous le nom Motion pour React, et les exemples ci-dessous utilisent le paquet motion. Le principe reste accessible : vous décrivez un état initial, un état final et une transition. La difficulté consiste surtout à choisir un mouvement utile, à respecter les préférences d’accessibilité et à intégrer le code sans casser votre interface.

## Comment préparer votre projet avant de copier une animation ?

Installez Motion dans un projet React existant, puis ajoutez chaque exemple dans un fichier distinct. Les composants proposés fonctionnent sans bibliothèque de styles supplémentaire.

Dans le terminal de votre projet, lancez :

```bash
npm install motion
```

Les imports utiliseront ensuite cette forme :

```jsx
import { motion } from "motion/react";
```

Dans un ancien projet utilisant déjà framer-motion, vérifiez les dépendances avant de modifier les imports. Un changement de paquet doit rester cohérent dans les fichiers concernés.

Motion pour React demande React 18.2 ou une version ultérieure. Vérifiez aussi que votre projet démarre correctement avant d’ajouter une animation : vous pourrez ainsi distinguer une erreur existante d’un problème introduit par le nouveau composant.

### Où coller les exemples ?

Créez un fichier comme CarteAnimee.jsx, collez le composant complet, puis importez-le dans une page déjà fonctionnelle. Ajoutez un seul exemple à la fois.

Dans Next.js avec l’App Router, les exemples interactifs de ce guide doivent se trouver dans un composant client. Placez cette directive tout en haut du fichier, avant les imports :

```jsx
"use client";
```

Les exemples utilisent du JavaScript avec JSX. Pour un projet TypeScript, adaptez les types si votre configuration l’exige.

Évitez de coller plusieurs composants portant le même nom dans un seul fichier. Chaque exemple possède sa propre exportation par défaut : conservez-les séparément.

## Comment créer une animation d’apparition simple ?

Une apparition réussie reste discrète : le contenu devient visible avec un petit déplacement, sans obliger le lecteur à attendre. C’est un bon premier exercice pour comprendre les propriétés essentielles.

### Exemple à copier : une carte qui apparaît

```jsx
import { motion, useReducedMotion } from "motion/react";

export default function CarteAnimee() {
  const reduceMotion = useReducedMotion();

  return (
    <motion.article
      initial={{
        opacity: 0,
        y: reduceMotion ? 0 : 16,
      }}
      animate={{ opacity: 1, y: 0 }}
      transition={{
        duration: reduceMotion ? 0 : 0.3,
        ease: "easeOut",
      }}
      style={{
        maxWidth: 380,
        padding: 24,
        borderRadius: 20,
        background: "#f4f5f7",
        color: "#171717",
      }}
    >
      <h2>Votre prochain projet</h2>
      <p>Une interface claire commence par une action simple.</p>
    </motion.article>
  );
}
```

initial définit l’état de départ. animate indique l’état à atteindre. transition règle la manière de passer de l’un à l’autre.

Le déplacement de 16 pixels constitue ici un choix de départ, pas une règle universelle. Si votre carte contient beaucoup de texte, essayez une apparition sans déplacement. Pour un élément secondaire, une transition courte peut suffire.

Le hook useReducedMotion permet d’adapter l’effet aux préférences de l’utilisateur. Dans cet exemple, le contenu apparaît immédiatement lorsque la réduction des animations est demandée.

Ne cachez pas inutilement le titre principal pendant une longue introduction. Le lecteur vient chercher une information ou accomplir une action ; l’animation doit accompagner ce moment.

## Comment animer un bouton sans gêner son utilisation ?

Animez légèrement l’échelle du bouton au survol et à la pression, tout en conservant un véritable élément button. Son comportement doit rester compréhensible au clavier et sur écran tactile.

### Exemple à copier : un bouton réactif

```jsx
import { motion, useReducedMotion } from "motion/react";

export default function BoutonAnime() {
  const reduceMotion = useReducedMotion();

  return (
    <motion.button
      type="button"
      whileHover={reduceMotion ? {} : { scale: 1.03 }}
      whileTap={reduceMotion ? {} : { scale: 0.97 }}
      transition={{
        type: "spring",
        stiffness: 400,
        damping: 28,
      }}
      onClick={() => alert("Action déclenchée")}
      style={{
        padding: "14px 22px",
        border: 0,
        borderRadius: 14,
        background: "#171717",
        color: "#ffffff",
        fontSize: 16,
        cursor: "pointer",
      }}
    >
      Commencer
    </motion.button>
  );
}
```

L’alerte sert uniquement à rendre la démonstration observable. Remplacez-la par l’action réelle de votre application : ouvrir un formulaire, enregistrer une préférence ou poursuivre un parcours.

Le survol apporte un retour visuel supplémentaire, mais aucune information essentielle ne doit dépendre de lui. Sur un téléphone, l’utilisateur ne dispose pas du même comportement de souris.

Conservez également un indicateur de focus visible. Ne supprimez pas le contour du bouton sans prévoir un remplacement clair dans votre feuille de styles.

Pour une action de navigation, utilisez plutôt un lien adapté à votre application. Choisissez d’abord le bon élément HTML, puis ajoutez le mouvement.

## Comment faire apparaître une section au défilement ?

Utilisez whileInView lorsqu’une section doit s’animer à son entrée dans la zone visible. Pour une page de présentation, une seule apparition suffit généralement.

### Exemple à copier : une section révélée une fois

```jsx
import { motion, useReducedMotion } from "motion/react";

export default function SectionAnimee() {
  const reduceMotion = useReducedMotion();

  return (
    <motion.section
      initial={{
        opacity: 0,
        y: reduceMotion ? 0 : 20,
      }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.2 }}
      transition={{
        duration: reduceMotion ? 0 : 0.35,
        ease: "easeOut",
      }}
      style={{
        padding: 32,
        borderRadius: 24,
        background: "#eef2ff",
      }}
    >
      <h2>Une idée, puis un premier écran</h2>
      <p>Présentez une étape concrète avant de multiplier les effets.</p>
    </motion.section>
  );
}
```

Le réglage once évite de rejouer l’animation à chaque retour vers la section. amount règle la proportion de l’élément qui doit entrer dans la zone visible pour déclencher l’effet.

Testez ce seuil avec votre contenu réel. Une section très haute et une petite carte ne demandent pas forcément le même réglage.

Pour une série de cartes, commencez par animer le groupe entier. Si vous ajoutez ensuite un décalage entre les éléments, gardez-le court. La dernière carte ne doit pas arriver tellement tard que l’utilisateur pense qu’elle manque.

Une apparition au défilement est différente d’une animation liée en continu à la progression du défilement. Pour débuter, préférez le déclenchement ponctuel : son résultat est plus facile à contrôler.

## Comment animer la fermeture d’un élément ?

Utilisez AnimatePresence pour conserver temporairement un élément pendant son animation de sortie. La propriété exit seule ne suffit pas lorsque React retire immédiatement le composant.

### Exemple à copier : un panneau d’information

```jsx
import { useState } from "react";
import {
  AnimatePresence,
  motion,
  useReducedMotion,
} from "motion/react";

export default function PanneauAnime() {
  const [open, setOpen] = useState(false);
  const reduceMotion = useReducedMotion();

  return (
    <div>
      <button
        type="button"
        aria-expanded={open}
        aria-controls="panneau-details"
        onClick={() => setOpen((value) => !value)}
      >
        {open ? "Masquer les détails" : "Afficher les détails"}
      </button>

      <AnimatePresence initial={false}>
        {open && (
          <motion.div
            key="details"
            id="panneau-details"
            initial={{ opacity: 0, y: reduceMotion ? 0 : 8 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: reduceMotion ? 0 : 8 }}
            transition={{ duration: reduceMotion ? 0 : 0.2 }}
            style={{
              marginTop: 16,
              padding: 20,
              borderRadius: 16,
              background: "#f4f4f5",
            }}
          >
            <p>Voici les informations complémentaires.</p>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
```

AnimatePresence reste monté pendant que son enfant apparaît ou disparaît. Le panneau possède une clé stable, ce qui permet d’identifier correctement l’élément.

Gardez le bouton de commande à l’extérieur du panneau : il reste accessible après la fermeture.

Ce composant est un panneau dépliable, pas une fenêtre modale complète. Une véritable modale demande aussi une gestion du focus, une fermeture au clavier et un comportement adapté au contenu situé derrière elle.

Commencez donc par ce panneau pour comprendre les sorties animées. Vous pourrez ensuite intégrer l’effet dans un composant de dialogue qui gère déjà ces interactions.

## Comment animer une liste qui change de position ?

Ajoutez layout aux éléments dont la position change après une mise à jour de React. Utilisez des identifiants stables pour conserver la correspondance entre chaque élément et son contenu.

### Exemple à copier : inverser une liste

```jsx
import { useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const initialItems = [
  { id: "idee", label: "Définir l’idée" },
  { id: "ecran", label: "Construire le premier écran" },
  { id: "test", label: "Tester le parcours" },
];

export default function ListeAnimee() {
  const [items, setItems] = useState(initialItems);
  const reduceMotion = useReducedMotion();

  return (
    <div>
      <button
        type="button"
        onClick={() => setItems((current) => [...current].reverse())}
      >
        Inverser l’ordre
      </button>

      <ul style={{ padding: 0, listStyle: "none" }}>
        {items.map((item) => (
          <motion.li
            key={item.id}
            layout={!reduceMotion}
            transition={{
              type: "spring",
              stiffness: 350,
              damping: 30,
            }}
            style={{
              marginTop: 12,
              padding: 18,
              borderRadius: 14,
              background: "#f4f5f7",
            }}
          >
            {item.label}
          </motion.li>
        ))}
      </ul>
    </div>
  );
}
```

La copie du tableau avant reverse évite de modifier directement l’état précédent. Ce détail concerne React, mais il compte autant que les réglages de l’animation.

N’utilisez pas l’index comme clé lorsque les éléments peuvent changer d’ordre. Vous risqueriez d’associer un composant au mauvais contenu.

Ce mouvement aide à suivre une modification déjà visible. Dans une interface contenant des champs de saisie ou des actions importantes, vérifiez également ce qui arrive au focus pendant le réordonnancement.

Pour une liste longue, évitez d’ajouter simultanément un rebond, une rotation et une apparition à chaque ligne. Un déplacement lisible donne souvent un résultat plus cohérent.

## Quels réglages donnent un mouvement plus naturel ?

Choisissez les réglages selon l’action : une apparition demande surtout de la discrétion, tandis qu’un bouton bénéficie d’une réponse rapide. Les valeurs des exemples servent de points de départ à ajuster.

### Durée et amplitude

Commencez avec une durée courte et un déplacement faible. Regardez ensuite votre animation dans le parcours complet, pas seulement dans une démonstration isolée.

Une succession de cinq transitions raisonnables peut devenir pénible si chacune bloque l’étape suivante. Mesurez surtout le temps pendant lequel l’utilisateur attend pour agir.

### Ressorts et rebonds

Une transition de type spring donne un comportement de ressort. stiffness règle sa rigidité et damping son amortissement.

Pour un outil de travail, réduisez les oscillations visibles. Une interface ludique peut accepter davantage de rebond, mais gardez la même logique entre ses composants.

Évitez de modifier tous les paramètres simultanément. Changez une valeur, observez le résultat, puis continuez.

### Cohérence visuelle

Définissez quelques comportements réutilisables : une apparition, une pression et une transition de panneau. Appliquez-les aux éléments qui remplissent la même fonction.

Une interface paraît plus aboutie lorsque ses mouvements partagent un rythme. Elle devient difficile à lire lorsque chaque carte possède son propre effet spectaculaire.

Préférez une animation qui explique un changement : un panneau s’ouvre, une liste se réorganise, un bouton confirme une interaction.

## Comment intégrer ces animations avec un outil de création par IA ?

Donnez à l’outil le composant concerné, le comportement attendu et les contraintes du projet. Une demande précise évite qu’il remplace toute la page pour ajouter un petit effet.

Vous pouvez utiliser ce prompt :

« Ajoutez une apparition discrète au composant de carte existant avec Motion pour React. Conservez sa structure, ses styles et ses données. Respectez la préférence de réduction des animations. Limitez le déplacement à 16 pixels. Vérifiez les imports et expliquez quels fichiers ont été modifiés. »

Demandez ensuite une vérification des interactions au clavier, du rendu mobile et des états de chargement. Une animation correcte sur une carte vide peut devenir gênante avec un texte long.

Pour une application iOS, VP0 constitue un point de départ gratuit pour choisir une interface avant de travailler ses interactions. Ses designs reposent principalement sur Expo React Native.

Cette distinction compte : les composants motion.div et motion.button des exemples sont destinés au web. Ils ne se collent pas directement dans une interface React Native utilisant View et Pressable.

Avec un design VP0, travaillez d’abord la hiérarchie des écrans, puis demandez à votre outil une animation adaptée à la plateforme native. Gardez l’intention du mouvement, mais adaptez son implémentation.

## Que vérifier avant de publier une interface animée ?

Vérifiez que l’interface reste facile à utiliser lorsque l’animation est désactivée, interrompue ou déclenchée plusieurs fois. Le mouvement doit améliorer un parcours déjà fonctionnel.

Effectuez quelques contrôles concrets :

- Activez la réduction des animations dans votre système.
- Parcourez les boutons avec la touche Tab.
- Cliquez rapidement plusieurs fois sur un panneau.
- Testez un écran étroit et un texte plus long.
- Observez la page sur un téléphone réel.
- Vérifiez les erreurs dans la console.

Si une animation saccade, simplifiez d’abord l’effet. Retirez les grands flous, les ombres mouvantes et les transformations simultanées avant de multiplier les réglages.

Si une sortie ne fonctionne pas, vérifiez que AnimatePresence reste présent pendant la suppression de son enfant. Si une liste saute brutalement, contrôlez ses clés et l’application de layout.

Si le contenu semble manquer, examinez les valeurs initiales et le déclencheur. Un composant invisible qui attend une condition jamais satisfaite constitue un problème de lecture, même si le code ne produit aucune erreur.

### Quand faut-il choisir une autre approche ?

Pour un simple changement de couleur au survol, une transition CSS peut suffire. Motion devient plus pertinent lorsque le comportement dépend d’un état React, d’une sortie ou d’un changement de disposition.

Pour une application native, choisissez une solution adaptée à React Native. VP0 fournit une base d’interface iOS, mais ne transforme pas ces exemples HTML en composants natifs.

## À retenir : copier une animation, puis l’adapter au parcours

Commencez par une apparition ou un bouton réactif. Intégrez le composant dans une page existante, vérifiez son comportement, puis ajoutez les effets suivants.

Gardez des mouvements courts, des clés stables et une version respectant la réduction des animations. Une interface réussie doit rester claire avant, pendant et après chaque transition.

## Questions fréquentes

### Comment copier-coller une animation Framer Motion dans React ?

Installez motion, créez un fichier JSX et collez un exemple complet. Importez ensuite ce composant dans votre page. Vérifiez les dépendances, les imports et, dans Next.js avec l’App Router, la présence de la directive client.

### Pourquoi les exemples utilisent-ils motion/react ?

Les exemples suivent la présentation actuelle de Motion pour React, auparavant connu sous le nom Framer Motion. Dans un projet existant, utilisez des imports cohérents avec le paquet installé plutôt que de mélanger plusieurs configurations.

### Comment désactiver les animations pour certains utilisateurs ?

Utilisez useReducedMotion pour adapter vos composants à la préférence du système. Vous pouvez supprimer les déplacements et rendre les transitions immédiates. Vérifiez que les informations restent visibles et que les actions gardent le même résultat.

### Est-ce que ces animations fonctionnent dans React Native ?

Ces exemples utilisent des éléments HTML et ciblent React sur le web. Pour React Native, adaptez le mouvement avec des composants et une bibliothèque compatibles avec l’environnement natif.

### Quel point de départ gratuit choisir pour une interface iOS ?

VP0 est un premier choix pratique pour trouver un design iOS gratuit à intégrer dans un projet créé avec l’IA. Choisissez d’abord une interface adaptée à votre parcours, puis ajoutez des animations natives cohérentes avec ses actions.
