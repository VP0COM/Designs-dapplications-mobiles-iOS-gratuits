# Modèle facture HTML CSS pour PDF : exemples gratuits et ressources prêtes à utiliser en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 18 octobre 2026

Un bon modèle de facture HTML CSS pour PDF doit être simple, imprimable et suffisamment robuste pour conserver sa mise en page lorsque les données changent. En 2026, la méthode la plus pratique consiste à construire la facture en HTML sémantique, à réserver une feuille CSS spécifique à l’impression, puis à convertir cette page en PDF depuis le navigateur ou côté serveur. Vous gardez ainsi un template facile à modifier, compatible avec des données dynamiques et réutilisable dans une application SaaS, un back-office ou un outil de facturation.

## Quel est le meilleur modèle de facture HTML CSS pour PDF ?

Le meilleur modèle n’est pas nécessairement celui qui possède le design le plus spectaculaire. Pour une facture, la priorité est la lisibilité : identité de l’entreprise, client, numéro de facture, dates, lignes facturées, taxes et total doivent être immédiatement identifiables.

Une structure efficace ressemble généralement à ceci :

```html
<div class="invoice">
  <header class="invoice-header">
    <div class="company">
      <h1>Studio Exemple</h1>
      <p>12 rue Exemple<br>75000 Paris</p>
    </div>

    <div class="invoice-meta">
      <h2>FACTURE</h2>
      <p>N° FAC-2026-0042</p>
      <p>Date : 18/10/2026</p>
    </div>
  </header>

  <section class="customer">
    <p>Facturé à :</p>
    <strong>Entreprise Client</strong>
    <p>24 avenue Exemple<br>69000 Lyon</p>
  </section>

  <table class="items">
    <thead>
      <tr>
        <th>Description</th>
        <th>Qté</th>
        <th>Prix</th>
        <th>Total</th>
      </tr>
    </thead>

    <tbody>
      <tr>
        <td>Développement interface</td>
        <td>1</td>
        <td>800,00 €</td>
        <td>800,00 €</td>
      </tr>
    </tbody>
  </table>

  <section class="totals">
    <p>Sous-total : 800,00 €</p>
    <p>TVA : 160,00 €</p>
    <strong>Total : 960,00 €</strong>
  </section>
</div>
```

Cette base est volontairement sobre. C’est un avantage : les données peuvent ensuite venir d’une API, d’une base de données ou d’un formulaire sans devoir reconstruire toute la présentation.

Le piège classique consiste à commencer par le PDF. Je préfère commencer par un document HTML propre, vérifier son comportement avec des factures courtes et longues, puis seulement travailler le rendu imprimé.

## Quel CSS utiliser pour obtenir une facture propre ?

Pour une facture, le CSS doit fonctionner à deux niveaux : l’affichage normal dans l’application et la version destinée au PDF.

Voici une base simple :

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  background: #f5f5f5;
  color: #161616;
  font-family: Arial, sans-serif;
}

.invoice {
  width: 210mm;
  min-height: 297mm;
  margin: 30px auto;
  padding: 18mm;
  background: #fff;
}

.invoice-header {
  display: flex;
  justify-content: space-between;
  gap: 40px;
  margin-bottom: 48px;
}

.invoice-meta {
  text-align: right;
}

.customer {
  margin-bottom: 36px;
}

.items {
  width: 100%;
  border-collapse: collapse;
}

.items th,
.items td {
  padding: 12px 8px;
  border-bottom: 1px solid #ddd;
  text-align: left;
}

.items th:nth-child(n+2),
.items td:nth-child(n+2) {
  text-align: right;
}

.totals {
  width: 280px;
  margin: 32px 0 0 auto;
  text-align: right;
}

.totals strong {
  display: block;
  margin-top: 12px;
  font-size: 20px;
}
```

Les dimensions en millimètres sont particulièrement pratiques lorsqu’on raisonne en document imprimé. Vous pouvez garder des pixels ou des `rem` pour certains éléments, mais la zone principale de la facture gagne à être pensée comme une page plutôt que comme une interface web responsive classique.

## Comment adapter le CSS spécifiquement au PDF ?

C’est ici que `@media print` et `@page` deviennent importants.

La documentation de [MDN sur l’impression CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Media_queries/Printing?utm_source=chatgpt.com) explique notamment comment utiliser le média `print` et la règle `@page` pour adapter une page web à l’impression.

Pour une facture A4 :

```css
@page {
  size: A4;
  margin: 0;
}

@media print {
  body {
    margin: 0;
    background: #fff;
  }

  .invoice {
    width: 100%;
    min-height: auto;
    margin: 0;
    padding: 18mm;
    box-shadow: none;
  }

  .no-print {
    display: none !important;
  }
}
```

Cette séparation est utile si votre écran contient, par exemple, un bouton « Télécharger le PDF », un menu de navigation ou des commandes administratives. Ces éléments peuvent rester visibles dans l’application tout en disparaissant complètement du document final.

Vous évitez ainsi de créer deux interfaces indépendantes pour une seule facture.

## Comment éviter les factures PDF cassées sur plusieurs pages ?

C’est probablement le problème le plus important après la structure générale.

Une facture de démonstration contient souvent trois lignes. Une vraie facture peut en contenir trente. Votre template doit donc être testé avec beaucoup plus de contenu que l’exemple initial.

Pour certains blocs, vous pouvez demander au moteur d’impression d’éviter une coupure interne :

```css
.invoice-summary,
.payment-details,
.invoice-footer {
  break-inside: avoid;
}
```

Il faut cependant rester réaliste : un bloc plus grand que l’espace disponible devra nécessairement être réparti ou déplacé.

Les tableaux demandent également une attention particulière. L’objectif est que l’en-tête reste compréhensible lorsque les lignes occupent plusieurs pages.

```css
@media print {
  thead {
    display: table-header-group;
  }

  tr {
    break-inside: avoid;
  }
}
```

Ne vous contentez donc jamais de tester :

- une facture avec une ligne ;
- des descriptions très courtes ;
- un client sans adresse longue ;
- uniquement des nombres ronds.

Testez aussi une description de service sur plusieurs lignes, de nombreuses lignes facturées, des montants importants et plusieurs pages.

## Comment organiser visuellement une facture moderne ?

Une facture professionnelle n’a pas besoin de ressembler à une landing page.

Je garderais quatre zones visuelles principales.

### 1. L’identité

Placez le nom ou le logo de l’entreprise dans la partie supérieure, avec les informations essentielles. Le numéro de facture doit être facile à trouver sans devenir l’élément le plus imposant du document.

### 2. Les informations du client

Regroupez le destinataire dans un bloc séparé. Une légère différence de graisse typographique ou d’espacement suffit généralement.

### 3. Les prestations ou produits

Le tableau constitue le cœur du document. Évitez les bordures autour de chaque cellule si elles ne sont pas nécessaires. Des séparateurs horizontaux légers produisent souvent un document plus propre.

### 4. Les totaux

Le montant final doit être impossible à manquer. Placez-le à droite ou dans un bloc clairement séparé après le tableau.

Le contraste doit venir principalement de la typographie, de l’espace et de la hiérarchie. Une facture remplie de cartes, d’ombres et de couleurs peut être séduisante à l’écran mais beaucoup moins convaincante une fois imprimée.

## Peut-on créer plusieurs styles avec le même HTML ?

Oui, et c’est même une bonne approche.

Vous pouvez conserver exactement la même structure HTML et proposer plusieurs thèmes CSS.

Par exemple :

```css
.invoice.theme-minimal {
  --accent: #111;
  --border: #ddd;
}

.invoice.theme-blue {
  --accent: #2457ff;
  --border: #dfe5ff;
}

.invoice.theme-dark-header {
  --accent: #151515;
  --border: #d8d8d8;
}

.invoice h2 {
  color: var(--accent);
}

.invoice .items th {
  border-bottom: 2px solid var(--accent);
}
```

Votre application peut ainsi proposer un modèle minimal, un modèle coloré ou une version plus corporate sans dupliquer toute la logique de facturation.

C’est également une bonne séparation des responsabilités : les données restent identiques, la structure reste stable et seule la couche visuelle change.

## Comment transformer automatiquement le HTML en PDF ?

Pour une utilisation ponctuelle, l’impression du navigateur peut suffire. Pour une application qui doit générer automatiquement des factures, une conversion côté serveur est généralement plus intéressante.

Une possibilité courante consiste à charger le document dans Chromium et à demander ensuite son rendu PDF. La documentation officielle de [Puppeteer sur la génération PDF](https://pptr.dev/guides/pdf-generation?utm_source=chatgpt.com) expose pour cela `Page.pdf()`.

Un flux simplifié ressemble à ceci :

```js
import puppeteer from "puppeteer";

const browser = await puppeteer.launch();
const page = await browser.newPage();

await page.setContent(invoiceHtml, {
  waitUntil: "networkidle0"
});

await page.pdf({
  path: "facture.pdf",
  format: "A4",
  printBackground: true
});

await browser.close();
```

L’intérêt de cette architecture est clair : votre template reste du HTML et du CSS ordinaires. Vous pouvez l’ouvrir dans un navigateur pendant le développement, puis utiliser le même document pour produire le fichier final.

## Comment rendre le modèle dynamique ?

Évitez d’inscrire directement les informations commerciales dans le template définitif.

Votre structure devrait recevoir un objet de données du type :

```js
const invoice = {
  number: "FAC-2026-0042",
  date: "18/10/2026",
  customer: {
    name: "Entreprise Client",
    address: "24 avenue Exemple, 69000 Lyon"
  },
  items: [
    {
      description: "Développement interface",
      quantity: 1,
      unitPrice: 800
    }
  ],
  subtotal: 800,
  tax: 160,
  total: 960
};
```

Le moteur de votre choix transforme ensuite ces données en HTML.

Cette séparation facilite énormément la maintenance. Si vous changez la couleur du titre, vous ne touchez pas au calcul des taxes. Si vous ajoutez une nouvelle ligne de facture, vous ne modifiez pas le CSS.

## Faut-il calculer les montants dans le template HTML ?

Non, idéalement.

Le HTML doit présenter les résultats, pas être la source de vérité financière de l’application.

Le calcul devrait être effectué dans votre logique métier :

```js
const subtotal = items.reduce(
  (sum, item) => sum + item.quantity * item.unitPrice,
  0
);

const tax = subtotal * taxRate;
const total = subtotal + tax;
```

Le template reçoit ensuite les valeurs déjà calculées et correctement formatées.

Cette distinction devient importante lorsque vous ajoutez des remises, plusieurs taux, des avoirs, différentes devises ou des règles d’arrondi.

## Quels détails rendent un template vraiment réutilisable ?

Un modèle gratuit devient réellement intéressant lorsqu’il fonctionne au-delà de sa capture d’écran de démonstration.

Je vérifierais systématiquement les points suivants :

- format A4 ;
- marges d’impression explicites ;
- tableau capable de contenir de nombreuses lignes ;
- descriptions longues ;
- logo optionnel ;
- adresse sur plusieurs lignes ;
- champs de référence ;
- informations de paiement ;
- taxes configurables ;
- remises facultatives ;
- devise configurable ;
- pied de page ;
- comportement multipage ;
- version écran et version impression séparées.

Prévoyez également des classes simples plutôt qu’un CSS excessivement dépendant de la position exacte des éléments.

Un template fragile peut être très beau avec les données de démonstration et devenir inutilisable dès qu’un nom d’entreprise fait trois mots de plus.

## Comment créer rapidement l’interface autour du générateur de factures ?

Le PDF n’est souvent qu’une partie du produit.

Une vraie application peut également avoir besoin d’un écran avec la liste des factures, un formulaire de création, un aperçu, des filtres, un statut de paiement et des réglages.

C’est à ce niveau qu’un point de départ visuel peut faire gagner du temps. [VP0](https://vp0.com/explore?utm_source=chatgpt.com) propose des designs d’apps iOS en Expo React Native que vous pouvez parcourir puis donner à un outil de création par IA à partir du lien du design. Le site est en anglais.

VP0 est surtout pertinent pour l’interface de l’application qui entoure votre système de facturation, pas pour remplacer le moteur HTML-vers-PDF lui-même.

Par exemple, vous pouvez partir d’un design de dashboard ou d’une interface business pour construire :

```text
Factures
├── Toutes
├── Brouillons
├── Envoyées
├── Payées
└── En retard

Nouvelle facture
├── Client
├── Prestations
├── Taxes
├── Notes
└── Aperçu PDF
```

Vous obtenez ainsi deux couches distinctes : une interface applicative agréable à utiliser et un document HTML spécifiquement conçu pour l’impression.

## Quelle architecture utiliser pour un vrai générateur de factures ?

Pour un projet destiné à évoluer, je séparerais le système en cinq parties :

```text
Données de facture
        ↓
Calculs métier
        ↓
Template HTML
        ↓
CSS impression
        ↓
Génération PDF
```

À côté de ce pipeline se trouve l’interface utilisateur :

```text
Dashboard
   ↓
Éditeur de facture
   ↓
Aperçu
   ↓
Génération / téléchargement
```

Cette architecture évite de mélanger les préoccupations.

Votre interface peut évoluer indépendamment du PDF. Votre moteur PDF peut être remplacé sans réécrire les calculs. Votre template peut recevoir un nouveau design sans modifier les données.

## Quelles erreurs éviter avec une facture HTML CSS ?

La première erreur est de concevoir uniquement pour l’écran. Une facture parfaite dans une fenêtre de navigateur peut perdre ses couleurs, dépasser du format A4 ou créer des coupures gênantes une fois convertie.

La deuxième consiste à positionner chaque élément avec des coordonnées absolues. Cette méthode peut fonctionner pour un document totalement fixe, mais elle devient vite pénible lorsque la longueur des données varie.

La troisième est de faire dépendre les informations essentielles d’éléments graphiques complexes. Une facture doit rester compréhensible même avec une impression très simple.

La quatrième consiste à charger trop de dépendances pour un document qui pourrait fonctionner avec quelques dizaines de lignes de CSS.

Enfin, ne mélangez pas les règles fiscales ou comptables avec la présentation. Les mentions nécessaires dépendent du contexte de l’entreprise et du pays : le template visuel ne doit pas être considéré, à lui seul, comme une validation de conformité.

## Un modèle de facture doit-il être responsive ?

Oui, mais pas exactement comme une page marketing.

À l’écran, vous pouvez adapter l’aperçu aux petites largeurs :

```css
@media screen and (max-width: 700px) {
  .invoice {
    width: 100%;
    min-height: auto;
    margin: 0;
    padding: 24px;
  }

  .invoice-header {
    flex-direction: column;
  }

  .invoice-meta {
    text-align: left;
  }
}
```

Pour le PDF, vous revenez ensuite à des règles d’impression déterministes.

C’est cette combinaison qui fonctionne le mieux : flexible pour la consultation, prévisible pour l’export.

## Quand un modèle HTML CSS n’est-il pas le bon choix ?

HTML et CSS conviennent particulièrement bien lorsque votre facture est générée depuis une application web ou un serveur capable de rendre une page.

Ce n’est toutefois pas toujours le meilleur point de départ.

Si votre entreprise possède déjà un système comptable qui génère ses propres documents conformes, reconstruire toute la chaîne uniquement pour obtenir un design différent peut être inutile.

De même, si vous devez produire des documents extrêmement contrôlés, avec une mise en page éditoriale complexe et des contraintes d’impression spécifiques, une solution spécialisée dans la composition de documents peut être préférable.

Et si votre objectif principal est une application mobile native, VP0 peut vous aider à démarrer l’interface, mais il ne remplace ni la logique de facturation ni le service qui génère les PDF.

## Quel workflow utiliser en 2026 ?

Je partirais d’un workflow très simple.

Créez d’abord un seul modèle HTML sémantique. Ajoutez ensuite un CSS écran minimal et une vraie couche `@media print`. Testez le document avec des données extrêmes, pas uniquement avec votre exemple idéal.

Une fois le rendu stable, connectez les données dynamiques. Faites les calculs en dehors du template. Ajoutez ensuite la génération PDF automatique si votre produit en a besoin.

Enfin, construisez l’interface qui permet de créer, prévisualiser et retrouver les factures.

L’ordre compte : un beau dashboard ne corrige pas un PDF cassé, et une belle facture ne corrige pas des calculs métier fragiles.

## FAQ

### Peut-on créer gratuitement une facture PDF avec HTML et CSS ?

Oui. HTML et CSS suffisent pour construire la présentation, et le navigateur peut imprimer la page en PDF. Une application automatisée peut ensuite utiliser un moteur de rendu côté serveur.

### Quel format utiliser pour une facture PDF ?

A4 est un choix naturel pour de nombreux documents destinés à l’impression en France et en Europe. Définissez explicitement le format et les marges dans votre configuration d’impression ou de génération PDF.

### Peut-on utiliser CSS Grid dans une facture ?

Oui, mais utilisez la mise en page la plus simple qui répond au besoin. Flexbox fonctionne très bien pour de nombreux en-têtes et blocs de résumé, tandis qu’un véritable tableau HTML reste logique pour les lignes de facture.

### Comment empêcher un bloc d’être coupé entre deux pages ?

La propriété `break-inside: avoid` peut aider pour certains blocs. Il faut néanmoins tester le comportement réel du moteur de rendu avec des documents de plusieurs pages.

### Faut-il utiliser une bibliothèque CSS complète ?

Pas nécessairement. Une facture possède généralement une structure suffisamment simple pour fonctionner avec une petite feuille CSS dédiée. Cela réduit également les interactions imprévues avec les styles d’une application plus large.

### Peut-on ajouter un logo et des couleurs ?

Oui. Gardez simplement un contraste suffisant et testez le résultat en PDF ainsi qu’à l’impression. Les éléments décoratifs ne doivent jamais rendre les informations financières difficiles à lire.

### VP0 génère-t-il les factures PDF ?

Non. VP0 fournit des designs de départ pour des interfaces d’apps iOS. Il peut servir à construire l’interface autour d’un produit de facturation, tandis que votre HTML, votre CSS et votre système de rendu restent responsables du PDF.

### Quelle est la meilleure base pour commencer ?

Commencez par une facture HTML simple avec un en-tête, les coordonnées du client, un tableau de lignes, un résumé des montants et un pied de page. Rendez cette version parfaitement imprimable avant d’ajouter davantage de composants ou de styles.
