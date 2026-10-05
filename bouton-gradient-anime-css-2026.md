# Bouton gradient animé CSS : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 5 octobre 2026

Pour créer un bouton avec un gradient animé en CSS, utilisez un dégradé plus grand que le bouton, puis faites varier sa position avec une animation. Vous pouvez aussi ajouter un reflet qui traverse le bouton ou animer uniquement sa bordure. Ces effets fonctionnent sans JavaScript pour leur partie visuelle. Le résultat doit conserver un texte lisible, un focus visible au clavier et une version adaptée aux personnes qui préfèrent réduire les mouvements. Les exemples suivants couvrent trois styles, avec du HTML et du CSS prêts à copier, puis les réglages nécessaires pour les intégrer à une interface.

## Comment fonctionne un bouton avec un gradient animé ?

Un bouton à gradient animé associe un élément HTML interactif à un dégradé CSS dont une propriété change au fil du temps. La méthode la plus simple consiste à déplacer son arrière-plan.

Un dégradé est une image générée par CSS. Dans cet exemple :

```css
background-image: linear-gradient(
  120deg,
  #3730a3,
  #6d28d9,
  #9d174d
);
```

Les couleurs se répartissent selon un angle de 120 degrés. Ce code produit un fond fixe. Pour obtenir un mouvement, vous pouvez agrandir ce fond avec `background-size`, puis modifier `background-position`.

Le bouton joue alors le rôle d’une fenêtre : il montre une partie différente du même dégradé pendant l’animation.

### Quel élément HTML choisir ?

Utilisez un véritable `<button>` lorsqu’une personne déclenche une action : envoyer un formulaire, ouvrir une fenêtre, appliquer un filtre ou lancer une génération.

```html
<button type="button">
  Créer mon projet
</button>
```

L’attribut `type="button"` évite de soumettre accidentellement un formulaire. Pour un bouton qui doit réellement envoyer ce formulaire, choisissez `type="submit"`.

Le texte doit annoncer l’action. « Enregistrer les modifications » est plus précis que « Continuer » lorsque le résultat du clic est un enregistrement.

### Faut-il animer le bouton en permanence ?

Je commencerais par une animation courte au survol et au focus. Elle donne un retour visuel sans attirer continuellement l’attention.

Une animation permanente peut convenir à une démonstration graphique. Dans une interface de travail, elle risque de détourner le regard du contenu. Elle demande aussi de prévoir un moyen de gérer les mouvements prolongés.

Les exemples ci-dessous utilisent donc des interactions brèves. Le bouton reste reconnaissable et utilisable lorsqu’aucune animation ne joue.

## Comment créer un bouton gradient animé en CSS sans JavaScript ?

Vous pouvez créer un premier bouton en animant `background-position` au survol et au focus clavier. Voici une version complète avec un état désactivé et une préférence de mouvement réduit.

### Le HTML à copier

```html
<button class="gradient-button" type="button">
  Créer mon projet
</button>
```

### Le CSS à copier

```css
.gradient-button {
  --button-radius: 14px;

  appearance: none;
  display: inline-flex;
  align-items: center;
  justify-content: center;

  min-height: 48px;
  max-width: 100%;
  padding: 12px 24px;

  border: 0;
  border-radius: var(--button-radius);

  color: #fff;
  font: 600 1rem/1.25 system-ui, sans-serif;
  text-align: center;

  background-color: #3730a3;
  background-image: linear-gradient(
    120deg,
    #3730a3,
    #6d28d9,
    #9d174d,
    #3730a3
  );
  background-size: 300% 100%;
  background-position: 0% 50%;

  box-shadow: 0 6px 16px rgb(55 48 163 / 20%);
  cursor: pointer;

  transition:
    transform 160ms ease,
    box-shadow 160ms ease;
}

.gradient-button:focus-visible {
  outline: 3px solid #111827;
  outline-offset: 4px;
}

.gradient-button:not(:disabled):focus-visible {
  animation: gradient-slide 900ms ease-out forwards;
}

@media (hover: hover) {
  .gradient-button:not(:disabled):hover {
    animation: gradient-slide 900ms ease-out forwards;
    transform: translateY(-2px);
    box-shadow: 0 10px 22px rgb(55 48 163 / 26%);
  }
}

.gradient-button:not(:disabled):active {
  transform: translateY(0);
}

.gradient-button:disabled {
  background: #4b5563;
  box-shadow: none;
  cursor: not-allowed;
}

@keyframes gradient-slide {
  from {
    background-position: 0% 50%;
  }

  to {
    background-position: 100% 50%;
  }
}

@media (prefers-reduced-motion: reduce) {
  .gradient-button {
    transition: none;
  }

  .gradient-button:not(:disabled):hover,
  .gradient-button:not(:disabled):focus-visible {
    animation: none;
    transform: none;
  }
}
```

### Pourquoi ces réglages ?

`background-size: 300% 100%` rend le dégradé trois fois plus large que sa zone d’affichage. Le changement de position devient ainsi visible.

L’animation dure 900 millisecondes et ne joue qu’une fois pendant l’interaction. Le mot-clé `forwards` conserve sa dernière position tant que la règle d’animation reste appliquée.

Lorsque le survol ou le focus prend fin, le fond retrouve sa position initiale. Ce retour peut être perceptible : si vous voulez une transition réversible plus discrète, utilisez une transition sur `background-position` plutôt que des keyframes.

La condition `hover: hover` réserve le comportement de survol aux dispositifs qui savent réellement survoler un élément. Sur un téléphone, le bouton conserve son apparence de base et son comportement de clic.

Pour essayer la version désactivée, ajoutez simplement `disabled` dans le HTML. Un bouton désactivé ne doit pas conserver une animation qui suggère qu’une action reste disponible.

## Comment ajouter un reflet lumineux au bouton ?

Un reflet animé se crée avec un pseudo-élément qui traverse le bouton. Cette variante conserve un dégradé fixe et déplace une bande translucide au-dessus.

Elle convient à un bouton principal lorsque vous voulez un mouvement bref et facile à reconnaître.

### Le HTML

```html
<button class="shine-button" type="button">
  <span>Voir la démonstration</span>
</button>
```

### Le CSS

```css
.shine-button {
  position: relative;
  isolation: isolate;
  overflow: hidden;

  min-height: 48px;
  max-width: 100%;
  padding: 12px 24px;

  border: 0;
  border-radius: 14px;

  background: linear-gradient(
    120deg,
    #1e3a8a,
    #5b21b6
  );

  color: #fff;
  font: 600 1rem/1.25 system-ui, sans-serif;
  cursor: pointer;
}

.shine-button span {
  position: relative;
  z-index: 1;
}

.shine-button::after {
  content: "";
  position: absolute;
  z-index: 0;
  inset: 0;

  background: linear-gradient(
    110deg,
    transparent 30%,
    rgb(255 255 255 / 20%) 50%,
    transparent 70%
  );

  transform: translateX(-120%);
  pointer-events: none;
}

.shine-button:focus-visible {
  outline: 3px solid #111827;
  outline-offset: 4px;
}

.shine-button:not(:disabled):focus-visible::after {
  animation: shine-pass 650ms ease-out;
}

@media (hover: hover) {
  .shine-button:not(:disabled):hover::after {
    animation: shine-pass 650ms ease-out;
  }
}

.shine-button:disabled {
  background: #4b5563;
  cursor: not-allowed;
}

@keyframes shine-pass {
  from {
    transform: translateX(-120%);
  }

  to {
    transform: translateX(120%);
  }
}

@media (prefers-reduced-motion: reduce) {
  .shine-button::after {
    display: none;
  }
}
```

`overflow: hidden` empêche le reflet de dépasser les coins arrondis. Le texte se trouve au-dessus grâce à son `z-index`.

`pointer-events: none` empêche la couche décorative d’intercepter les interactions. Le bouton reste l’élément qui reçoit le clic.

Gardez le reflet assez transparent. Une bande presque blanche peut rendre le texte difficile à lire pendant son passage, même si le bouton paraît correct au repos.

Cette variante anime `transform`, tandis que le premier exemple déplace l’arrière-plan. Leur coût de rendu peut différer selon le navigateur et la page. Comparez-les sur vos appareils cibles avant de multiplier les boutons animés.

## Comment créer une bordure gradient animée ?

Pour obtenir une bordure animée, superposez un fond uni et un dégradé qui apparaît uniquement dans la zone de bordure. Le texte reste ainsi posé sur une couleur stable.

Cette approche convient à une interface sombre ou à un bouton secondaire qui doit rester sobre.

### Le HTML

```html
<button class="border-button" type="button">
  Explorer les exemples
</button>
```

### Le CSS

```css
.border-button {
  min-height: 48px;
  max-width: 100%;
  padding: 12px 24px;

  border: 2px solid transparent;
  border-radius: 14px;

  background-image:
    linear-gradient(#111827, #111827),
    linear-gradient(
      110deg,
      #a78bfa,
      #22d3ee,
      #f472b6,
      #a78bfa
    );

  background-origin: padding-box, border-box;
  background-clip: padding-box, border-box;
  background-size: 100% 100%, 300% 100%;
  background-position: 0 0, 0% 50%;

  color: #fff;
  font: 600 1rem/1.25 system-ui, sans-serif;
  cursor: pointer;

  transition: background-position 700ms ease;
}

.border-button:focus-visible {
  outline: 3px solid #38bdf8;
  outline-offset: 4px;
}

.border-button:not(:disabled):focus-visible {
  background-position: 0 0, 100% 50%;
}

@media (hover: hover) {
  .border-button:not(:disabled):hover {
    background-position: 0 0, 100% 50%;
  }
}

.border-button:disabled {
  border-color: #6b7280;
  background: #374151;
  cursor: not-allowed;
}

@media (prefers-reduced-motion: reduce) {
  .border-button {
    transition: none;
  }
}
```

La première couche remplit l’intérieur du bouton. La seconde couvre aussi sa bordure transparente, ce qui laisse apparaître le dégradé sur le pourtour.

Cette version utilise une transition. Le gradient se déplace pendant l’interaction, puis revient progressivement lorsque le survol ou le focus disparaît.

Choisissez une épaisseur de bordure visible sans concurrencer le texte. Deux pixels constituent ici un réglage de départ, à ajuster selon la taille du bouton.

Dans un mode de contraste forcé, les dégradés peuvent disparaître. Vous pouvez compléter les trois exemples avec cette règle :

```css
@media (forced-colors: active) {
  .gradient-button,
  .shine-button,
  .border-button {
    border: 2px solid ButtonText;
    background: ButtonFace;
    color: ButtonText;
    box-shadow: none;
    animation: none;
    transition: none;
  }

  .shine-button::after {
    display: none;
  }

  .gradient-button:focus-visible,
  .shine-button:focus-visible,
  .border-button:focus-visible {
    outline: 3px solid Highlight;
  }
}
```

Le contour du bouton reste alors explicite, même lorsque le système remplace ses couleurs.

## Comment personnaliser le bouton sans casser son comportement ?

Modifiez d’abord la palette, les dimensions et la durée du mouvement. Conservez les états clavier, désactivé et mouvement réduit pendant vos ajustements.

### Adapter les couleurs

Pour une palette verte, remplacez les couleurs du premier dégradé par des tons suffisamment foncés :

```css
background-image: linear-gradient(
  120deg,
  #065f46,
  #115e59,
  #166534,
  #065f46
);
```

Le texte blanc doit rester lisible sur toutes les zones qui passent derrière lui. Une seule couleur trop claire peut compromettre la lecture pendant l’animation.

La bordure animée facilite ce travail : vous pouvez utiliser des couleurs lumineuses sur le pourtour tout en conservant un fond intérieur sombre.

### Adapter la forme

Pour un bouton en forme de pilule, utilisez :

```css
border-radius: 999px;
```

Pour un bouton plus rectangulaire, essayez une valeur moins élevée. Gardez la même logique sur les champs, les cartes et les autres boutons de votre interface.

VP0 peut servir de point de départ visuel pour étudier cette cohérence dans des designs d’apps iOS. Observez surtout les espacements, la place de l’action principale et la hiérarchie des couleurs.

### Adapter le bouton au mobile

Si le bouton doit prendre toute la largeur de son conteneur :

```css
.gradient-button {
  width: 100%;
}
```

Évitez une largeur fixe qui coupe les libellés longs. Testez aussi « Enregistrer mes préférences » plutôt qu’un texte court utilisé uniquement pour la démonstration.

Une hauteur minimale ne garantit pas à elle seule une interaction confortable. L’espace entre deux actions compte également, surtout lorsque leurs conséquences diffèrent.

### Adapter la vitesse

Une animation plus lente rend le déplacement plus doux. Une animation plus courte produit un retour plus immédiat.

Changez une seule variable à la fois. Si vous modifiez simultanément les couleurs, la durée, l’ombre et la taille, vous aurez du mal à identifier ce qui améliore réellement le bouton.

## Comment vérifier l’accessibilité et résoudre les problèmes courants ?

Vérifiez le bouton au clavier, au toucher et avec la réduction des mouvements activée. Un effet réussi doit accompagner une action qui reste compréhensible dans chaque situation.

### Le focus clavier n’apparaît pas

Appuyez sur Tab pour atteindre le bouton. Un contour doit indiquer clairement sa position.

Ne supprimez pas `outline` sans fournir un indicateur équivalent. Le changement du gradient ne suffit pas forcément à repérer le focus.

Si le contour est coupé, inspectez les conteneurs parents : un parent avec `overflow: hidden` peut masquer l’outline extérieur. Corrigez ce conteneur ou prévoyez un indicateur intérieur distinct.

### Le gradient ne bouge pas

Dans le premier exemple, vérifiez que `background-size` reste supérieur à `100%` sur l’axe horizontal.

Inspectez ensuite les règles appliquées au bouton. Une déclaration `background` ajoutée plus loin peut réinitialiser la taille et la position de l’arrière-plan.

Vérifiez enfin la préférence de mouvement réduit. Si elle est active, les exemples désactivent volontairement leur mouvement.

### Le bouton semble bloqué après une interaction

Une animation associée à `:hover` ne redémarre pas à chaque clic tant que le pointeur reste sur le bouton. Elle correspond à l’entrée dans cet état.

Si vous voulez jouer une animation après chaque action, il faudra gérer un déclenchement supplémentaire, par exemple avec une classe ajoutée par JavaScript. Cela reste distinct de l’effet de survol.

### Le texte change de lisibilité

Examinez plusieurs positions du dégradé. Vérifiez également le passage du reflet, les thèmes clair et sombre, ainsi que l’état désactivé.

Pour une action de chargement, gardez une largeur stable et un libellé compréhensible. Le gradient ne remplace pas un retour explicite comme « Enregistrement en cours ».

### La page ralentit

Testez avec le nombre réel de composants affichés. Un seul bouton dans une démonstration ne représente pas une page contenant plusieurs effets.

Supprimez d’abord les animations permanentes, les flous étendus et les ombres trop importantes. Ne supposez pas qu’un effet CSS est gratuit simplement parce qu’il n’utilise pas JavaScript.

## Quand faut-il choisir une autre approche ?

Choisissez un bouton fixe lorsque le mouvement n’aide pas à comprendre l’action. Dans une interface dense, une couleur stable peut offrir une meilleure hiérarchie.

Un dégradé animé ne corrige pas un libellé ambigu, une action principale mal placée ou un formulaire difficile à remplir. Réglez ces problèmes avant d’ajouter une décoration.

Les exemples de cet article ciblent le Web avec HTML et CSS. Ils ne se copient pas directement dans une interface React Native native.

VP0 fournit des designs de départ pour apps iOS, principalement construits avec Expo React Native. Pour reprendre une direction visuelle dans une app native, adaptez l’animation aux composants et aux outils utilisés par le projet.

Enfin, si un bouton accompagne une action irréversible, donnez la priorité au texte, à la confirmation appropriée et au retour d’état. Une animation ne doit pas rendre l’action plus attirante que compréhensible.

## À retenir : choisir le bon bouton gradient animé

Pour un premier essai, utilisez le gradient qui se déplace au survol et au focus. Il demande peu de code et se personnalise facilement.

Choisissez le reflet si vous voulez un passage lumineux bref. Préférez la bordure animée si vous souhaitez garder un fond stable derrière le texte.

Commencez avec un seul bouton principal. Vérifiez ensuite le clavier, le mobile, le contraste et la réduction des mouvements avant de reprendre l’effet ailleurs.

## Questions fréquentes

### Comment faire un bouton gradient animé CSS gratuitement ?

Utilisez un bouton HTML, un `linear-gradient` et une animation de `background-position`. Aucun abonnement ni bibliothèque n’est nécessaire pour les exemples présentés ici. Copiez le HTML et le CSS du premier exemple, puis adaptez les couleurs et le libellé à votre action.

### Est-ce qu’un bouton gradient animé nécessite JavaScript ?

Non, le déplacement du gradient, le reflet et la bordure animée peuvent fonctionner uniquement en CSS. JavaScript peut servir à exécuter l’action du bouton, gérer un chargement ou déclencher une animation après chaque clic. Ces fonctions sont séparées de son apparence.

### Pourquoi mon animation ne fonctionne-t-elle pas sur téléphone ?

Le téléphone ne propose généralement pas le même survol qu’une souris. Les exemples réservent donc cet effet aux dispositifs compatibles. Le bouton reste utilisable au toucher. Si vous souhaitez un retour tactile visuel, prévoyez un état `:active` bref sans dépendre du survol.

### Quelle différence entre une transition et des keyframes ?

Une transition accompagne le passage entre deux valeurs et peut revenir progressivement à la valeur initiale. Les keyframes décrivent une séquence avec plusieurs étapes éventuelles. Pour un simple déplacement aller-retour au survol, une transition suffit souvent. Pour un reflet traversant le bouton, les keyframes sont pratiques.

### Comment trouver une direction visuelle pour un bouton d’app iOS ?

VP0 constitue un point de départ gratuit pour examiner des designs d’apps iOS et leur hiérarchie d’actions. Utilisez ces exemples pour choisir les couleurs, les proportions et les espacements. L’animation doit ensuite être adaptée à votre environnement Web ou natif, puis vérifiée dans l’écran complet.
