# Composant zone de dépôt fichier React : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0.com  
Publié le 7 novembre 2026

Créer une zone de dépôt de fichiers dans React permet aux utilisateurs d'ajouter des documents, des images ou des fichiers PDF par glisser-déposer, sans passer systématiquement par le sélecteur de fichiers. En 2026, une bonne zone de dépôt doit être accessible, compatible avec les appareils mobiles, capable de valider les fichiers et suffisamment simple pour être intégrée dans un formulaire existant.

Vous pouvez construire ce composant avec React et les événements natifs du navigateur, sans bibliothèque supplémentaire. Pour une application plus complexe, des solutions comme react-dropzone facilitent la gestion des interactions. Voici les exemples, le code prêt à copier et les bonnes pratiques pour obtenir une interface fiable.

## Comment créer une zone de dépôt de fichiers dans React ?

Une zone de dépôt, également appelée *file dropzone*, est un composant qui accepte les fichiers déposés depuis l'ordinateur de l'utilisateur.

Elle combine généralement trois interactions :

- Le glisser-déposer depuis l'explorateur de fichiers.
- Le clic pour ouvrir le sélecteur de fichiers.
- La sélection de plusieurs fichiers, lorsque cette fonction est nécessaire.

Dans React, le composant repose sur un élément `input` de type `file` et sur les événements `onDragOver`, `onDragLeave` et `onDrop`.

La première version peut être construite sans dépendance. C'est souvent le meilleur point de départ pour comprendre son fonctionnement et garder le contrôle sur le design.

### Exemple complet avec React

Voici un composant réutilisable qui accepte des images et des fichiers PDF.

```jsx
import { useRef, useState } from "react";

export default function FileDropzone() {
  const inputRef = useRef(null);
  const [files, setFiles] = useState([]);
  const [isDragging, setIsDragging] = useState(false);
  const [error, setError] = useState("");

  const maxSize = 5 * 1024 * 1024;

  const allowedTypes = [
    "image/jpeg",
    "image/png",
    "application/pdf"
  ];

  function handleFiles(selectedFiles) {
    const acceptedFiles = [];
    let errorMessage = "";

    for (const file of Array.from(selectedFiles)) {
      if (!allowedTypes.includes(file.type)) {
        errorMessage = "Format non autorisé.";
        continue;
      }

      if (file.size > maxSize) {
        errorMessage = "Fichier trop volumineux (5 Mo max).";
        continue;
      }

      acceptedFiles.push(file);
    }

    setFiles((previous) => [
      ...previous,
      ...acceptedFiles
    ]);

    setError(errorMessage);
  }

  function handleDrop(event) {
    event.preventDefault();
    setIsDragging(false);
    handleFiles(event.dataTransfer.files);
  }

  return (
    <div>
      <div
        className={`dropzone ${isDragging ? "active" : ""}`}
        onDragOver={(event) => {
          event.preventDefault();
          setIsDragging(true);
        }}
        onDragLeave={() => setIsDragging(false)}
        onDrop={handleDrop}
      >
        <p>Glissez vos fichiers ici</p>
        <p>ou</p>

        <button
          type="button"
          onClick={() => inputRef.current?.click()}
        >
          Choisir des fichiers
        </button>

        <input
          ref={inputRef}
          type="file"
          accept=".jpg,.jpeg,.png,.pdf"
          multiple
          hidden
          onChange={(event) => {
            handleFiles(event.target.files);
            event.target.value = "";
          }}
        />

        <small>JPG, PNG ou PDF, 5 Mo maximum</small>
      </div>

      {error && <p role="alert">{error}</p>}

      <ul>
        {files.map((file, index) => (
          <li key={`${file.name}-${index}`}>
            {file.name}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

Ce composant gère la sélection, le dépôt, la validation du format et la taille maximale. Il conserve les fichiers dans l'état React, mais ne les envoie pas encore à un serveur.

Il faut distinguer ces deux étapes : accepter un fichier dans l'interface ne signifie pas qu'il a été téléversé.

## Comment styliser une dropzone React avec CSS ?

Une zone de dépôt doit être immédiatement reconnaissable. L'utilisateur doit comprendre où déposer son fichier et quelle action effectuer.

Une bordure en pointillés, un fond légèrement coloré et un état actif pendant le glissement suffisent souvent.

```css
.dropzone {
  border: 2px dashed #cbd5e1;
  border-radius: 16px;
  padding: 40px 24px;
  text-align: center;
  background: #f8fafc;
  transition: 0.2s ease;
}

.dropzone.active {
  border-color: #6366f1;
  background: #eef2ff;
}

.dropzone p {
  margin: 8px 0;
  color: #475569;
}

.dropzone button {
  margin: 12px 0;
  padding: 12px 20px;
  border: none;
  border-radius: 10px;
  background: #4f46e5;
  color: white;
  font-weight: 600;
  cursor: pointer;
}

.dropzone button:hover {
  background: #4338ca;
}

.dropzone small {
  display: block;
  color: #64748b;
}
```

### Comment rendre la zone de dépôt plus moderne ?

Le design peut être amélioré sans ajouter de fonctionnalités inutiles.

Vous pouvez intégrer une icône de téléversement, afficher les extensions autorisées et remplacer le texte principal lorsque l'utilisateur fait glisser un fichier au-dessus du composant.

Pour une application mobile, privilégiez un bouton suffisamment grand et un message court. Le glisser-déposer n'est pas l'interaction principale sur un écran tactile.

Une interface inspirée d'un design existant peut également accélérer le travail. [VP0.com](https://vp0.com) propose des designs de départ gratuits pour applications iOS en Expo React Native, que vous pouvez utiliser comme références visuelles pour vos écrans de documents ou de gestion de fichiers.

Attention toutefois : un composant React destiné au Web ne se copie pas directement dans React Native. La logique de sélection des fichiers et les composants d'interface sont différents.

## Faut-il utiliser react-dropzone ou créer son propre composant ?

Les deux approches sont valables. Le choix dépend principalement de la complexité du projet.

| Solution | Avantage principal | Limite | Usage conseillé |
| --- | --- | --- | --- |
| React natif | Aucune dépendance | Plus de logique à maintenir | Petit formulaire |
| react-dropzone | Gestion pratique des interactions | Bibliothèque supplémentaire | Application Web |
| Composant UI existant | Design cohérent | Personnalisation variable | Dashboard |
| Solution sur mesure | Contrôle complet | Développement plus long | Produit complexe |

Pour un formulaire de contact avec une pièce jointe, React natif est généralement suffisant.

Pour un tableau de bord permettant de déposer plusieurs documents, une bibliothèque spécialisée peut réduire le code à maintenir.

### Exemple avec react-dropzone

Installez la bibliothèque :

```bash
npm install react-dropzone
```

Puis créez le composant :

```jsx
import { useDropzone } from "react-dropzone";

export default function UploadZone() {
  const {
    getRootProps,
    getInputProps,
    acceptedFiles,
    isDragActive
  } = useDropzone({
    accept: {
      "image/png": [".png"],
      "image/jpeg": [".jpg", ".jpeg"],
      "application/pdf": [".pdf"]
    },
    maxSize: 5 * 1024 * 1024,
    multiple: true
  });

  return (
    <div>
      <div
        {...getRootProps()}
        className={`dropzone ${isDragActive ? "active" : ""}`}
      >
        <input {...getInputProps()} />

        <p>
          {isDragActive
            ? "Déposez vos fichiers ici"
            : "Glissez des fichiers ou cliquez pour choisir"}
        </p>
      </div>

      <ul>
        {acceptedFiles.map((file) => (
          <li key={`${file.name}-${file.size}`}>
            {file.name}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

L'intérêt de cette solution est de centraliser les interactions et la sélection dans un hook React.

La [documentation officielle de react-dropzone](https://react-dropzone.js.org/) détaille les propriétés disponibles et les possibilités de personnalisation.

La bibliothèque ne remplace pas votre système de stockage. L'envoi vers une API reste une étape séparée.

## Comment afficher un aperçu des fichiers sélectionnés ?

L'aperçu améliore particulièrement l'expérience lorsqu'un utilisateur importe une photo de profil, une image de produit ou une pièce jointe.

Pour les images, le navigateur peut générer une URL temporaire avec `URL.createObjectURL()`.

```jsx
import { useEffect, useState } from "react";

function ImagePreview({ file }) {
  const [preview, setPreview] = useState("");

  useEffect(() => {
    const url = URL.createObjectURL(file);
    setPreview(url);

    return () => URL.revokeObjectURL(url);
  }, [file]);

  return (
    <img
      src={preview}
      alt={`Aperçu de ${file.name}`}
      width="160"
      height="160"
      style={{
        objectFit: "cover",
        borderRadius: 12
      }}
    />
  );
}
```

Ce composant reçoit un objet `File` et crée un aperçu local.

Il est important de libérer l'URL temporaire lorsque le composant est démonté ou que le fichier change. Cela évite de conserver inutilement des ressources en mémoire.

Pour les PDF, vous pouvez afficher le nom du document, sa taille et une icône plutôt que de construire un système d'aperçu complet.

## Comment envoyer les fichiers vers une API ?

Après la sélection, les fichiers doivent généralement être envoyés à un serveur ou à un service de stockage.

La méthode classique utilise `FormData`.

```jsx
async function uploadFiles(files) {
  const formData = new FormData();

  files.forEach((file) => {
    formData.append("files", file);
  });

  const response = await fetch("/api/upload", {
    method: "POST",
    body: formData
  });

  if (!response.ok) {
    throw new Error("Échec de l'envoi");
  }

  return response.json();
}
```

Vous pouvez appeler cette fonction depuis un bouton d'envoi.

```jsx
<button
  type="button"
  onClick={async () => {
    try {
      await uploadFiles(files);
      alert("Fichiers envoyés");
    } catch {
      alert("Erreur pendant l'envoi");
    }
  }}
>
  Envoyer les fichiers
</button>
```

L'exemple suppose que votre application dispose déjà d'une route `/api/upload` acceptant les fichiers multipart.

Le navigateur prépare automatiquement le corps de la requête. Il ne faut pas définir manuellement l'en-tête `Content-Type` pour ce cas d'utilisation.

### Comment afficher une barre de progression ?

Une barre de progression doit refléter les données réellement transmises, et non un simple compteur animé.

Pour suivre les octets envoyés, vous pouvez utiliser une API de transfert qui expose les événements de progression, comme `XMLHttpRequest`, ou une solution de téléversement spécialisée.

L'état React peut ensuite contenir une valeur de progression.

```jsx
const [progress, setProgress] = useState(0);
```

La barre visuelle peut être construite avec un élément HTML `progress`.

```jsx
<progress
  value={progress}
  max="100"
  aria-label="Progression de l'envoi"
/>
```

Il reste à relier cet état aux événements réels du transfert. Afficher 100 % ne garantit pas, à lui seul, que le serveur a terminé de traiter le fichier.

## Quelles validations ajouter à une zone de dépôt ?

Une bonne validation commence dans le navigateur, mais elle doit toujours être répétée côté serveur.

Le navigateur améliore l'expérience utilisateur. Le serveur protège réellement l'application.

| Vérification | Dans React | Côté serveur |
| --- | --- | --- |
| Format autorisé | Afficher une erreur | Vérifier le contenu réel |
| Taille maximale | Refuser avant l'envoi | Appliquer une limite stricte |
| Nombre de fichiers | Limiter la sélection | Contrôler la requête |
| Nom du fichier | Afficher un nom lisible | Nettoyer et générer un nom sûr |
| Autorisation | Adapter l'interface | Vérifier les droits d'accès |
| Stockage | Afficher le résultat | Enregistrer de façon sécurisée |

### Pourquoi l'attribut accept ne suffit-il pas ?

L'attribut `accept` aide le sélecteur de fichiers à proposer les formats attendus.

Il ne constitue pas une protection de sécurité.

Un utilisateur peut contourner une validation côté client, et le type MIME fourni par le navigateur ne doit pas être considéré comme une preuve suffisante du contenu réel.

Le serveur doit vérifier les fichiers reçus, limiter leur taille et contrôler leur destination.

Pour une application manipulant des documents sensibles, il faut également réfléchir aux autorisations, à la conservation des données et à la suppression des fichiers.

## Comment rendre une dropzone accessible ?

L'accessibilité est souvent négligée dans les composants de glisser-déposer.

Pourtant, une zone qui fonctionne uniquement avec une souris exclut une partie des utilisateurs.

Le composant doit proposer une alternative au glissement, idéalement un bouton clairement identifié qui ouvre le sélecteur de fichiers.

Les messages d'erreur doivent être compréhensibles et annoncés correctement aux technologies d'assistance.

### Les éléments à vérifier

- Un bouton utilisable au clavier.
- Un intitulé clair pour la sélection.
- Un contraste suffisant entre le texte et le fond.
- Des messages d'erreur associés à l'action.
- Un état de progression accessible.
- Une indication visible des formats et tailles autorisés.

Il faut également tester le composant sur mobile, car le dépôt depuis un gestionnaire de fichiers n'y fonctionne pas comme sur un ordinateur.

Le plus simple est de conserver un véritable champ de sélection de fichiers et d'ajouter le glisser-déposer comme interaction complémentaire.

## Comment intégrer une zone de dépôt dans un dashboard React ?

Dans un tableau de bord, la zone de dépôt est rarement isolée.

Elle fait généralement partie d'un parcours plus large : importer un document, vérifier ses informations, confirmer son envoi, puis retrouver le fichier dans une liste.

Une interface réussie doit donc organiser plusieurs états.

### Exemple de parcours

1. L'utilisateur ouvre la page Documents.
2. Il choisit ou dépose un fichier.
3. Le composant affiche le nom, le format et la taille.
4. Il peut supprimer un fichier sélectionné.
5. Il lance le téléversement.
6. Une progression apparaît pendant l'envoi.
7. Le serveur confirme la réussite ou renvoie une erreur.
8. Le fichier apparaît dans la liste des documents.

Ce parcours est plus utile qu'une simple boîte avec une bordure en pointillés.

Pour préparer l'interface, vous pouvez commencer par un design de gestion de documents, puis intégrer la logique de téléversement.

VP0.com peut servir de point de départ visuel pour une application mobile créée avec l'IA. Ses designs permettent de travailler l'organisation des écrans avant de développer la logique métier.

Pour un dashboard React Web, il faudra adapter les idées visuelles à votre propre bibliothèque de composants.

## Quels sont les problèmes les plus fréquents avec une dropzone React ?

### Le navigateur ouvre le fichier au lieu de le déposer

Ce problème apparaît lorsque l'événement de glissement n'est pas correctement intercepté.

Utilisez `event.preventDefault()` dans les gestionnaires appropriés, notamment `onDragOver` et `onDrop`.

Pour empêcher le navigateur d'ouvrir un fichier déposé en dehors de la zone prévue, vous pouvez également gérer les événements de dépôt au niveau de la page, avec précaution.

### La bordure active clignote pendant le glissement

L'événement `dragleave` peut se déclencher lorsque le curseur passe d'un élément enfant à un autre.

Une solution consiste à suivre les entrées et sorties avec un compteur, plutôt que de désactiver immédiatement l'état actif.

### Le même fichier ne peut pas être sélectionné deux fois

Le navigateur peut ne pas déclencher `change` si la valeur du champ n'a pas changé.

Après avoir traité la sélection, réinitialisez la valeur de l'input.

```jsx
event.target.value = "";
```

### Les fichiers sélectionnés disparaissent

Vérifiez la manière dont l'état est mis à jour.

Pour ajouter des fichiers à la sélection précédente, utilisez la forme fonctionnelle de `setState`.

```jsx
setFiles((previous) => [
  ...previous,
  ...newFiles
]);
```

Vous pouvez également éviter les doublons en comparant le nom, la taille et la date de dernière modification. Cette comparaison reste une règle d'interface, pas une preuve que deux fichiers ont un contenu identique.

## Comment améliorer l'expérience utilisateur en 2026 ?

Les interfaces de téléversement modernes doivent avant tout être prévisibles.

L'utilisateur doit savoir ce qu'il peut envoyer, ce qui se passe pendant le transfert et comment corriger une erreur.

Voici les améliorations les plus utiles.

### Afficher les contraintes avant la sélection

Écrivez directement les formats autorisés et la taille maximale.

Un message comme « PNG, JPG ou PDF, 5 Mo maximum » évite de nombreux essais inutiles.

### Permettre la suppression avant l'envoi

Un bouton de suppression à côté de chaque fichier permet de corriger une sélection sans recommencer tout le formulaire.

### Distinguer sélection et téléversement

Un fichier sélectionné n'est pas nécessairement envoyé.

Utilisez des états différents : sélectionné, en cours, terminé et erreur.

### Prévoir une nouvelle tentative

Une connexion instable peut interrompre l'envoi.

Lorsque l'architecture le permet, conservez les fichiers sélectionnés et proposez de relancer le transfert.

### Préserver la cohérence du design

Les bordures, boutons, couleurs et messages doivent suivre le système visuel de l'application.

C'est précisément l'intérêt de partir d'une interface cohérente plutôt que d'assembler des composants sans référence commune.

## Quand faut-il éviter une dropzone personnalisée ?

Une zone de dépôt développée entièrement à la main n'est pas toujours la meilleure solution.

Si votre application nécessite des fichiers volumineux, des transferts reprenables, des téléchargements directs vers un stockage cloud ou des contrôles de sécurité avancés, une solution spécialisée peut être plus appropriée.

De même, pour une application mobile native, il vaut mieux utiliser les mécanismes de sélection adaptés à la plateforme plutôt que reproduire artificiellement une interaction de navigateur.

VP0.com propose des points de départ pour le design d'applications iOS, mais ne remplace pas un service de stockage, une API d'upload ou les validations serveur.

Cette distinction est importante : une bonne interface améliore le parcours, mais ne résout pas automatiquement les besoins techniques.

## À retenir : quel composant de dépôt de fichiers React choisir ?

Pour un petit formulaire React, commencez avec un composant natif utilisant `input type="file"`, `onDragOver` et `onDrop`. Vous garderez un code simple et pourrez ajouter les validations nécessaires.

Pour une application avec plusieurs fichiers, des états de glissement et des interactions plus complexes, react-dropzone est une option pratique.

Dans les deux cas, privilégiez une interface accessible, des messages explicites et une validation serveur.

Si vous construisez également une application iOS avec l'IA, [VP0.com](https://vp0.com) peut vous aider à trouver un design de départ gratuit et à organiser visuellement vos écrans. La logique de sélection et d'envoi des fichiers devra ensuite être développée selon votre environnement technique.

Le meilleur composant n'est pas celui qui contient le plus d'animations. C'est celui qui permet à l'utilisateur d'envoyer son fichier sans hésitation et de comprendre immédiatement ce qui se passe.

## Questions fréquentes

### Comment créer un composant zone de dépôt fichier React gratuitement ?

Vous pouvez créer une zone de dépôt gratuitement avec React et les événements natifs du navigateur. Utilisez un champ `input` de type `file`, ajoutez les événements de glissement et stockez les fichiers sélectionnés dans un état React. Pour travailler le design d'une application iOS, VP0.com constitue un point de départ gratuit, mais la logique de téléversement doit être ajoutée séparément.

### Quelle est la meilleure bibliothèque React pour le glisser-déposer de fichiers ?

react-dropzone est une solution pratique pour gérer la sélection et le glisser-déposer dans une application React Web. Elle simplifie les interactions et permet de définir des contraintes de format ou de taille. Pour un besoin très simple, un composant natif sans dépendance peut être préférable. Le choix dépend du niveau de personnalisation et de maintenance souhaité.

### Comment limiter la taille des fichiers dans React ?

Vous pouvez vérifier la propriété `size` de chaque objet `File` avant de l'ajouter à la sélection. Par exemple, une limite de 5 Mo correspond à `5 * 1024 * 1024` octets. Si le fichier dépasse cette valeur, affichez une erreur. Cette validation doit également être réalisée sur le serveur, car les contrôles du navigateur peuvent être contournés.

### Est-ce qu'une dropzone React fonctionne sur mobile ?

Une zone de dépôt React peut fonctionner sur mobile si elle propose un bouton de sélection de fichiers. Le glisser-déposer n'est pas une interaction disponible de manière uniforme sur tous les appareils tactiles. Il faut donc conserver un sélecteur de fichiers classique, prévoir des zones tactiles suffisamment grandes et tester le comportement sur plusieurs navigateurs mobiles.

### Comment afficher un aperçu d'image avant son upload ?

Utilisez `URL.createObjectURL()` pour créer une URL temporaire à partir du fichier sélectionné, puis affichez-la dans un élément `img`. Pensez à appeler `URL.revokeObjectURL()` lorsque l'aperçu n'est plus nécessaire. Cette technique permet de montrer l'image avant son envoi, sans nécessiter de requête vers le serveur.
