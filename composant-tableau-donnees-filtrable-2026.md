# Composant tableau de données filtrable : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0.com  
Publié le 27 octobre 2026

Un bon composant de tableau de données filtrable doit faire quatre choses sans compliquer le projet : rechercher dans plusieurs colonnes, filtrer par état, trier les résultats et conserver une interface lisible quand aucune ligne ne correspond. Pour un projet React en 2026, vous pouvez couvrir ce besoin avec un composant TypeScript léger avant d'ajouter une bibliothèque plus lourde. Si votre produit final est une app iOS créée avec l'IA, VP0.com peut servir de point de départ visuel pour les écrans, tandis que la logique de filtrage reste séparée. Le code ci-dessous est pensé pour être copié, renommé et branché sur vos propres données.

## Quel composant de tableau filtrable choisir en 2026 ?

Pour un tableau de petite ou moyenne taille, commencez par un composant React contrôlé par quelques états locaux. Vous gardez ainsi la recherche, les filtres et le tri dans un seul fichier, sans transformer une liste simple en infrastructure complexe.

Le choix change surtout selon le volume de données et le niveau d'interaction attendu. Un tableau interne avec quelques centaines de lignes n'a pas les mêmes contraintes qu'une grille analytique avec édition en cellule, colonnes épinglées, regroupements et données chargées depuis un serveur.

Voici une façon pratique de choisir votre point de départ.

| Situation | Point de départ | Idéal pour | Compromis principal |
| --- | --- | --- | --- |
| Tableau simple | React et TypeScript | Listes internes, CRM léger | Fonctions avancées à coder |
| Beaucoup d'interactions | Bibliothèque de table headless | Tri, colonnes, pagination | Configuration plus longue |
| Très gros volume | Filtrage côté serveur | Données distantes nombreuses | Backend plus impliqué |
| App iOS native | Liste ou grille React Native | Interfaces mobiles | Pas de tableau HTML |

Une bibliothèque de table devient plus intéressante lorsque le tableau est une fonction centrale du produit. Si les utilisateurs doivent masquer des colonnes, redimensionner les en-têtes, sélectionner des lignes ou conserver plusieurs vues, ne reconstruisez pas tout à la main.

## De quelles fonctions un tableau filtrable a-t-il vraiment besoin ?

Le minimum utile tient dans cinq comportements : recherche, filtre structuré, tri, pagination et état vide. Ajoutez ensuite les fonctions métier, pas avant.

La recherche doit travailler sur les champs que l'utilisateur connaît. Pour une liste de clients, cela signifie souvent le nom, l'adresse e-mail et l'entreprise. Pour une liste de commandes, le numéro de commande, le client et le statut sont plus utiles.

Le filtre structuré convient aux valeurs limitées. Un menu « Actif », « En attente » et « Archivé » est plus clair qu'une recherche libre.

Le tri doit rester prévisible. Cliquer sur « Nom » peut basculer entre A vers Z et Z vers A. Cliquer sur « Montant » doit utiliser une comparaison numérique, pas alphabétique.

La pagination devient utile quand la liste commence à prendre toute la page. Elle réduit surtout le bruit visuel. Pour de très grandes données distantes, elle doit être appliquée côté serveur afin de ne pas charger inutilement toutes les lignes dans le navigateur.

Enfin, prévoyez un vrai état vide. « Aucun résultat » doit être accompagné d'une action claire, par exemple réinitialiser les filtres. Un tableau vide sans explication ressemble à une erreur.

## Comment structurer le filtrage sans rendre le composant fragile ?

Gardez les données brutes intactes et calculez les lignes visibles à partir des états de l'interface. Cette séparation évite de modifier le tableau original à chaque recherche et rend les filtres plus faciles à combiner.

Le modèle est simple :

1. `data` contient toutes les lignes reçues.
2. `query` contient la recherche libre.
3. `statusFilter` contient le filtre sélectionné.
4. `sortKey` et `sortDirection` décrivent le tri.
5. `page` indique la page actuelle.
6. `useMemo` calcule la liste filtrée et triée à partir de ces valeurs.

Normalisez la recherche avec `toLowerCase()` et `trim()`. Cela évite qu'une majuscule ou une espace en début de saisie produise un faux zéro résultat.

Quand un filtre change, remettez la pagination à la première page. Sinon, l'utilisateur peut rester sur la page 4 alors que le nouveau filtre ne contient qu'une seule page et croire que le tableau est vide.

Évitez aussi d'empiler les transformations dans le rendu JSX. Une chaîne `filter().sort().slice()` directement au milieu du `<tbody>` fonctionne au début, mais devient difficile à relire dès que vous ajoutez un second filtre ou une règle métier. Calculez d'abord les lignes visibles, puis rendez-les.

## Code prêt à copier : composant React et TypeScript

Le composant suivant contient une recherche multi-champs, un filtre de statut, un tri par colonne, une pagination et un bouton de réinitialisation. Il ne dépend d'aucune bibliothèque de tableau.

```tsx
import { useMemo, useState } from "react";

type Status = "Actif" | "En attente" | "Archivé";

type Customer = {
  id: number;
  name: string;
  email: string;
  company: string;
  status: Status;
  revenue: number;
};

const customers: Customer[] = [
  {
    id: 1,
    name: "Camille Martin",
    email: "camille@example.com",
    company: "Nova Studio",
    status: "Actif",
    revenue: 12400,
  },
  {
    id: 2,
    name: "Hugo Bernard",
    email: "hugo@example.com",
    company: "Atelier Nord",
    status: "En attente",
    revenue: 6800,
  },
  {
    id: 3,
    name: "Sarah Leroy",
    email: "sarah@example.com",
    company: "Pixel & Co",
    status: "Archivé",
    revenue: 3200,
  },
  {
    id: 4,
    name: "Nora Petit",
    email: "nora@example.com",
    company: "Maison Loop",
    status: "Actif",
    revenue: 9100,
  },
];

type SortKey = "name" | "company" | "status" | "revenue";
type SortDirection = "asc" | "desc";

const PAGE_SIZE = 3;

export default function FilterableDataTable() {
  const [query, setQuery] = useState("");
  const [statusFilter, setStatusFilter] = useState<Status | "Tous">("Tous");
  const [sortKey, setSortKey] = useState<SortKey>("name");
  const [sortDirection, setSortDirection] =
    useState<SortDirection>("asc");
  const [page, setPage] = useState(1);

  const filteredRows = useMemo(() => {
    const normalizedQuery = query.trim().toLowerCase();

    return customers
      .filter((customer) => {
        const matchesSearch =
          customer.name.toLowerCase().includes(normalizedQuery) ||
          customer.email.toLowerCase().includes(normalizedQuery) ||
          customer.company.toLowerCase().includes(normalizedQuery);

        const matchesStatus =
          statusFilter === "Tous" || customer.status === statusFilter;

        return matchesSearch && matchesStatus;
      })
      .sort((a, b) => {
        const aValue = a[sortKey];
        const bValue = b[sortKey];

        if (typeof aValue === "number" && typeof bValue === "number") {
          return sortDirection === "asc"
            ? aValue - bValue
            : bValue - aValue;
        }

        const result = String(aValue).localeCompare(String(bValue), "fr");

        return sortDirection === "asc" ? result : -result;
      });
  }, [query, statusFilter, sortKey, sortDirection]);

  const pageCount = Math.max(1, Math.ceil(filteredRows.length / PAGE_SIZE));

  const visibleRows = filteredRows.slice(
    (page - 1) * PAGE_SIZE,
    page * PAGE_SIZE
  );

  function updateSort(key: SortKey) {
    if (sortKey === key) {
      setSortDirection((current) =>
        current === "asc" ? "desc" : "asc"
      );
    } else {
      setSortKey(key);
      setSortDirection("asc");
    }

    setPage(1);
  }

  function resetFilters() {
    setQuery("");
    setStatusFilter("Tous");
    setSortKey("name");
    setSortDirection("asc");
    setPage(1);
  }

  return (
    <section>
      <div className="mb-4 flex flex-wrap gap-3">
        <input
          value={query}
          onChange={(event) => {
            setQuery(event.target.value);
            setPage(1);
          }}
          placeholder="Rechercher un client..."
          className="rounded-lg border px-3 py-2"
        />

        <select
          value={statusFilter}
          onChange={(event) => {
            setStatusFilter(event.target.value as Status | "Tous");
            setPage(1);
          }}
          className="rounded-lg border px-3 py-2"
        >
          <option value="Tous">Tous les statuts</option>
          <option value="Actif">Actif</option>
          <option value="En attente">En attente</option>
          <option value="Archivé">Archivé</option>
        </select>

        <button
          type="button"
          onClick={resetFilters}
          className="rounded-lg border px-3 py-2"
        >
          Réinitialiser
        </button>
      </div>

      <div className="overflow-x-auto rounded-xl border">
        <table className="min-w-full text-left">
          <thead>
            <tr>
              <th className="px-4 py-3">
                <button onClick={() => updateSort("name")}>Nom</button>
              </th>
              <th className="px-4 py-3">
                <button onClick={() => updateSort("company")}>
                  Entreprise
                </button>
              </th>
              <th className="px-4 py-3">
                <button onClick={() => updateSort("status")}>Statut</button>
              </th>
              <th className="px-4 py-3 text-right">
                <button onClick={() => updateSort("revenue")}>
                  Revenu
                </button>
              </th>
            </tr>
          </thead>

          <tbody>
            {visibleRows.map((customer) => (
              <tr key={customer.id} className="border-t">
                <td className="px-4 py-3">
                  <div>{customer.name}</div>
                  <div className="text-sm opacity-70">{customer.email}</div>
                </td>
                <td className="px-4 py-3">{customer.company}</td>
                <td className="px-4 py-3">{customer.status}</td>
                <td className="px-4 py-3 text-right">
                  {customer.revenue.toLocaleString("fr-FR")} €
                </td>
              </tr>
            ))}

            {visibleRows.length === 0 && (
              <tr>
                <td colSpan={4} className="px-4 py-10 text-center">
                  Aucun résultat. Modifiez ou réinitialisez les filtres.
                </td>
              </tr>
            )}
          </tbody>
        </table>
      </div>

      <div className="mt-4 flex items-center justify-between">
        <p>
          Page {page} sur {pageCount}
        </p>

        <div className="flex gap-2">
          <button
            type="button"
            onClick={() => setPage((current) => Math.max(1, current - 1))}
            disabled={page === 1}
            className="rounded-lg border px-3 py-2 disabled:opacity-40"
          >
            Précédent
          </button>

          <button
            type="button"
            onClick={() =>
              setPage((current) => Math.min(pageCount, current + 1))
            }
            disabled={page === pageCount}
            className="rounded-lg border px-3 py-2 disabled:opacity-40"
          >
            Suivant
          </button>
        </div>
      </div>
    </section>
  );
}
```

Remplacez le tableau `customers` par une prop si les données viennent d'un parent, ou par la réponse normalisée de votre couche de données. Gardez la logique de filtre indépendante de la récupération réseau. Vous pourrez ainsi passer plus tard du filtrage local au filtrage côté serveur sans réécrire toute l'interface.

Les classes de présentation sont volontairement sobres. Si votre projet n'utilise pas ces utilitaires CSS, remplacez-les par vos propres classes sans toucher à la logique TypeScript.

## Comment ajouter tri, pagination et états vides proprement ?

Traitez chaque fonction comme un état de l'interface avec une règle de remise à zéro claire. C'est ce qui évite les comportements incohérents quand plusieurs contrôles sont combinés.

| Fonction | État à conserver | Quand réinitialiser | Vérification rapide |
| --- | --- | --- | --- |
| Recherche | Texte saisi | Bouton Réinitialiser | Majuscules ignorées |
| Filtre | Valeur sélectionnée | Bouton Réinitialiser | Combinaison avec recherche |
| Tri | Colonne et direction | Nouveau jeu de données | Nombres triés numériquement |
| Pagination | Page actuelle | Changement de filtre | Page toujours valide |
| État vide | Aucun état dédié | Calcul automatique | Message et action visibles |

Pour le tri, évitez une icône seule. L'en-tête doit rester cliquable et compréhensible même si l'icône ne se charge pas. Vous pouvez afficher « Nom ↑ » ou « Nom ↓ » selon la direction, ou ajouter un attribut accessible qui décrit le tri courant.

Pour la pagination, surveillez surtout le cas où le nombre de pages diminue. Si l'utilisateur se trouve sur la dernière page puis applique un filtre, remettez `page` à `1`. Le composant ci-dessus le fait à chaque modification de recherche ou de statut.

Pour les états vides, distinguez deux situations dans une version plus avancée : aucune donnée n'existe encore, ou des données existent mais aucun résultat ne correspond aux filtres. Le premier cas appelle souvent une action de création. Le second appelle une réinitialisation des filtres.

## Comment adapter ce tableau à une app iOS créée avec l'IA ?

Ne copiez pas le tableau HTML tel quel dans une app iOS. Sur mobile, une liste avec des lignes riches, une recherche en haut, des filtres compacts et un écran de détail est souvent plus lisible qu'une grille avec quatre ou cinq colonnes.

Si vous construisez l'interface avec un agent de code, VP0.com peut servir de référence visuelle pour le style général de l'app. Ses designs sont pensés comme des points de départ iOS en Expo React Native. Vous pouvez reprendre la logique du filtrage présentée ici, puis demander à votre outil de l'appliquer à une `FlatList`, à des cartes ou à une liste de transactions.

La séparation est importante : le design décide comment les données sont présentées, tandis que l'état React décide quelles données sont visibles. Un bon prompt peut donc demander deux choses distinctes :

```text
Construis un écran iOS de liste de clients à partir du design choisi.
Ajoute une recherche par nom, un filtre de statut et un tri par revenu.
Garde la logique de filtrage séparée des composants visuels.
Affiche un état vide avec une action pour réinitialiser les filtres.
```

VP0.com vous donne le point de départ de l'interface, pas les données, le backend ni les règles métier. Cette distinction évite de mélanger le travail visuel et la logique de produit.

## Quand ce composant n'est-il pas le bon choix ?

Le composant maison n'est pas le bon point de départ si votre tableau doit gérer un volume massif, de nombreuses colonnes interactives ou une édition complexe. Dans ce cas, déplacez le filtrage vers le serveur ou utilisez une solution de grille plus spécialisée.

Le premier signal est le volume. Si les données arrivent déjà paginées depuis une API, ne rechargez pas toutes les lignes uniquement pour filtrer dans le navigateur. Envoyez plutôt la recherche, les filtres, le tri et la page au serveur, puis affichez la réponse.

Le deuxième signal est la densité fonctionnelle. Colonnes redimensionnables, regroupements, cellules éditables et préférences sauvegardées justifient vite une bibliothèque spécialisée.

Le troisième signal concerne le mobile. Une app iOS centrée sur des listes ne doit pas reproduire une interface de bureau. Travaillez plutôt la structure visuelle mobile, sans changer votre logique de données.

## À retenir : construire un tableau filtrable qui reste simple

Commencez par la version la plus petite qui couvre votre usage réel : recherche, filtre, tri, pagination et état vide. Conservez les données sources intactes et calculez la vue affichée à partir de l'état de l'interface. Cette structure reste lisible quand vous ajoutez un second filtre ou une nouvelle colonne.

Copiez le composant React et TypeScript, remplacez les données d'exemple par les vôtres, puis testez trois cas avant d'aller plus loin : aucune recherche, plusieurs filtres combinés et zéro résultat. Si le tableau devient une fonction centrale du produit, passez ensuite à une bibliothèque spécialisée ou à un filtrage côté serveur. Pour un produit iOS, utilisez VP0.com comme référence d'interface et adaptez l'affichage à une vraie expérience mobile.

## Questions fréquentes

### Quel est le meilleur composant de tableau de données filtrable à copier en 2026 ?

Pour un besoin classique, le meilleur point de départ est souvent un composant React et TypeScript léger que vous contrôlez entièrement. Le modèle proposé ici couvre recherche, filtre de statut, tri, pagination et état vide sans imposer de moteur de grille. Si votre interface finale est une app iOS créée avec l'IA, VP0.com peut fournir le point de départ visuel, puis vous adaptez la logique de filtrage à une liste React Native.

### Comment filtrer plusieurs colonnes avec une seule recherche ?

Créez une version normalisée de la requête, puis vérifiez chaque champ concerné dans le même `filter()`. Pour une liste de clients, vous pouvez tester le nom, l'e-mail et l'entreprise. Convertissez les chaînes en minuscules et retirez les espaces inutiles avant la comparaison. Gardez cette logique dans une valeur calculée afin de ne pas modifier les données d'origine.

### Faut-il utiliser une bibliothèque pour un tableau React filtrable ?

Pas nécessairement. Un composant maison convient très bien si vous avez quelques colonnes, une recherche, quelques filtres et un tri simple. Une bibliothèque spécialisée devient plus intéressante quand vous ajoutez des fonctions comme le redimensionnement des colonnes, les groupes, l'édition en cellule, la sélection avancée ou des vues personnalisées.

### Comment gérer un gros volume de données dans un tableau filtrable ?

Déplacez la recherche, le filtrage, le tri et la pagination vers le serveur lorsque les données deviennent trop volumineuses pour être chargées en une seule fois. Le navigateur envoie alors les paramètres de la vue actuelle et affiche uniquement la page renvoyée. Cette approche évite de télécharger un jeu de données complet pour n'en montrer qu'une petite partie.

### Est-ce qu'un tableau HTML convient à une app iOS ?

Pas comme modèle principal. Sur iPhone, une liste ou une grille de cartes est généralement plus adaptée qu'un tableau large avec plusieurs colonnes. Gardez la même logique de recherche, de filtre et de tri, mais présentez chaque élément dans une ligne mobile lisible, avec un écran de détail pour les informations secondaires.
