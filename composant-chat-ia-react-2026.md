# Composant chat IA React : exemples et code prêt à copier en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 4 octobre 2026

Pour créer un composant de chat IA en React, commencez par une liste de messages, un champ de saisie et une fonction asynchrone qui récupère la réponse de votre serveur. Vous pouvez construire cette interface sans bibliothèque de chat supplémentaire, puis ajouter les fonctions nécessaires à votre produit : annulation, historique, réponses progressives et affichage du code. L’exemple ci-dessous fournit un composant React avec TypeScript et son CSS. Il fonctionne immédiatement avec une réponse de démonstration. Pour obtenir de vraies réponses IA, vous remplacerez cette démonstration par un appel à votre backend, sans exposer de clé secrète dans le navigateur.

## Que doit contenir un composant de chat IA React ?

Un composant de chat utile doit gérer la conversation et les moments où l’utilisateur attend, interrompt ou rencontre une erreur. Les bulles de texte constituent seulement sa partie visible.

Prévoyez dès le départ ces éléments :

- Un historique distinguant les messages de l’utilisateur et de l’assistant.
- Un champ de saisie avec un libellé accessible.
- Un bouton d’envoi désactivé pendant une requête.
- Un indicateur d’attente.
- Un message d’erreur compréhensible.
- Une commande pour arrêter la requête en cours.

Séparez ensuite l’interface de la fonction qui obtient la réponse. Le composant doit pouvoir appeler indifféremment une démonstration locale ou votre serveur.

Cette séparation évite de réécrire les bulles, le formulaire et les styles lorsque vous changez de fournisseur IA.

### Quel format utiliser pour les messages ?

Pour un premier chat textuel, chaque message peut contenir trois propriétés :

```ts
type Message = {
  id: string;
  role: "user" | "assistant";
  content: string;
};
```

L’identifiant sert de clé lors de l’affichage. Le rôle indique qui parle. Le contenu représente le texte à afficher.

Gardez les instructions système côté serveur. Un utilisateur peut modifier les données envoyées depuis son navigateur : votre backend doit donc vérifier les rôles et reconstruire lui-même les instructions de votre assistant.

Si vous ajoutez plus tard des fichiers, des résultats d’outils ou plusieurs types de contenu, faites évoluer ce modèle explicitement. Évitez de placer toutes ces informations dans une chaîne difficile à interpréter.

## Comment créer un composant React prêt à copier ?

Créez un fichier `AIChat.tsx`, puis ajoutez le composant suivant dans un projet React avec TypeScript. Il utilise une fonction de démonstration par défaut et accepte une fonction `getReply` pour connecter votre serveur.

Dans un environnement qui distingue les composants serveur et client, conservez la directive `"use client"`. Dans une application React exécutée dans le navigateur, elle n’est généralement pas nécessaire.

### Le composant complet

```tsx
"use client";

import { useEffect, useRef, useState } from "react";
import type { FormEvent } from "react";
import "./AIChat.css";

export type Message = {
  id: string;
  role: "user" | "assistant";
  content: string;
};

export type GetReply = (
  messages: Message[],
  signal: AbortSignal
) => Promise<string>;

const welcome: Message = {
  id: "welcome",
  role: "assistant",
  content: "Bonjour. Que souhaitez-vous créer ?",
};

const demoReply: GetReply = async (messages, signal) => {
  await new Promise<void>((resolve, reject) => {
    if (signal.aborted) {
      reject(new DOMException("Annulé", "AbortError"));
      return;
    }

    const timer = window.setTimeout(() => {
      signal.removeEventListener("abort", onAbort);
      resolve();
    }, 700);

    function onAbort() {
      window.clearTimeout(timer);
      reject(new DOMException("Annulé", "AbortError"));
    }

    signal.addEventListener("abort", onAbort, { once: true });
  });

  const last = messages[messages.length - 1];

  return (
    `Vous avez écrit : « ${last.content} ».\n\n` +
    "Cette réponse est une démonstration locale."
  );
};

export default function AIChat({
  getReply = demoReply,
}: {
  getReply?: GetReply;
}) {
  const [messages, setMessages] = useState<Message[]>([welcome]);
  const [draft, setDraft] = useState("");
  const [pending, setPending] = useState(false);
  const [error, setError] = useState("");

  const requestRef = useRef<AbortController | null>(null);
  const listRef = useRef<HTMLDivElement | null>(null);
  const inputRef = useRef<HTMLTextAreaElement | null>(null);

  useEffect(() => {
    const list = listRef.current;

    if (list) {
      list.scrollTop = list.scrollHeight;
    }
  }, [messages, pending]);

  useEffect(() => {
    return () => requestRef.current?.abort();
  }, []);

  async function handleSubmit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    const content = draft.trim();

    if (!content || requestRef.current) return;

    const userMessage: Message = {
      id: crypto.randomUUID(),
      role: "user",
      content,
    };

    const nextMessages = [...messages, userMessage];
    const controller = new AbortController();

    requestRef.current = controller;
    setMessages(nextMessages);
    setDraft("");
    setError("");
    setPending(true);

    try {
      const reply = await getReply(nextMessages, controller.signal);

      if (controller.signal.aborted) return;

      if (!reply.trim()) {
        throw new Error("Réponse vide");
      }

      setMessages((current) => [
        ...current,
        {
          id: crypto.randomUUID(),
          role: "assistant",
          content: reply,
        },
      ]);
    } catch {
      if (!controller.signal.aborted) {
        setError(
          "La réponse n’a pas pu être obtenue. Veuillez réessayer."
        );
      }
    } finally {
      if (requestRef.current === controller) {
        requestRef.current = null;
        setPending(false);
        inputRef.current?.focus();
      }
    }
  }

  function stopRequest() {
    requestRef.current?.abort();
  }

  return (
    <section className="ai-chat" aria-labelledby="chat-title">
      <header className="ai-chat__header">
        <h2 id="chat-title">Assistant IA</h2>
        <p>Décrivez votre question ou votre projet.</p>
      </header>

      <div
        ref={listRef}
        className="ai-chat__messages"
        role="log"
        aria-label="Conversation"
        aria-live="polite"
        aria-relevant="additions"
      >
        {messages.map((message) => (
          <article
            key={message.id}
            className={`ai-chat__message ai-chat__message--${message.role}`}
          >
            <strong>
              {message.role === "user" ? "Vous" : "Assistant"}
            </strong>
            <p>{message.content}</p>
          </article>
        ))}
      </div>

      <p className="ai-chat__status" role="status">
        {pending ? "L’assistant prépare sa réponse…" : ""}
      </p>

      {error && (
        <p className="ai-chat__error" role="alert">
          {error}
        </p>
      )}

      <form className="ai-chat__form" onSubmit={handleSubmit}>
        <label htmlFor="chat-message">Votre message</label>

        <textarea
          ref={inputRef}
          id="chat-message"
          value={draft}
          onChange={(event) => setDraft(event.target.value)}
          placeholder="Posez votre question…"
          rows={3}
          maxLength={4000}
          required
        />

        <div className="ai-chat__actions">
          {pending && (
            <button type="button" onClick={stopRequest}>
              Arrêter
            </button>
          )}

          <button
            type="submit"
            disabled={pending || !draft.trim()}
          >
            {pending ? "En cours…" : "Envoyer"}
          </button>
        </div>
      </form>
    </section>
  );
}
```

Pour afficher la démonstration, importez ensuite le composant dans votre écran :

```tsx
import AIChat from "./AIChat";

export default function App() {
  return <AIChat />;
}
```

La démonstration répète votre message après un court délai. Elle ne contacte aucun modèle et ne nécessite aucune clé.

Le champ reste modifiable pendant l’attente, ce qui permet de préparer la question suivante. Le bouton d’envoi reste bloqué jusqu’à la fin de la requête précédente.

L’exemple utilise volontairement un bouton d’envoi. La touche Entrée conserve ainsi son comportement habituel dans un champ multiligne : elle ajoute un retour à la ligne.

## Comment donner au chat une interface lisible sur mobile ?

Utilisez une largeur limitée, une zone de conversation défilante et des messages capables d’afficher des mots très longs. Le formulaire doit rester visible sans dépendre d’une hauteur fixe trop importante.

Créez le fichier `AIChat.css` à côté du composant :

```css
.ai-chat,
.ai-chat * {
  box-sizing: border-box;
}

.ai-chat {
  width: min(100%, 760px);
  height: min(780px, 90dvh);
  min-height: 320px;
  margin: 24px auto;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  color: #172033;
  background: #ffffff;
  border: 1px solid #dce3ed;
  border-radius: 20px;
  font-family: system-ui, sans-serif;
}

.ai-chat__header {
  padding: 20px;
  border-bottom: 1px solid #e8edf4;
}

.ai-chat__header h2 {
  margin: 0 0 6px;
  font-size: 1.2rem;
}

.ai-chat__header p {
  margin: 0;
  color: #526078;
}

.ai-chat__messages {
  flex: 1;
  min-height: 0;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.ai-chat__message {
  max-width: 88%;
  padding: 14px 16px;
  border-radius: 16px;
}

.ai-chat__message--assistant {
  align-self: flex-start;
  background: #f1f4f9;
}

.ai-chat__message--user {
  align-self: flex-end;
  color: #ffffff;
  background: #244ec9;
}

.ai-chat__message strong {
  font-size: 0.8rem;
}

.ai-chat__message p {
  margin: 8px 0 0;
  line-height: 1.6;
  white-space: pre-wrap;
  overflow-wrap: anywhere;
}

.ai-chat__status {
  min-height: 20px;
  margin: 0;
  padding: 0 20px;
  color: #526078;
  font-size: 0.85rem;
}

.ai-chat__error {
  margin: 10px 20px;
  color: #a01818;
}

.ai-chat__form {
  padding: 16px 20px;
  border-top: 1px solid #e8edf4;
}

.ai-chat__form label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
}

.ai-chat__form textarea {
  display: block;
  width: 100%;
  max-height: 180px;
  padding: 12px;
  resize: vertical;
  border: 1px solid #b8c3d4;
  border-radius: 12px;
  font: inherit;
}

.ai-chat__actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 12px;
}

.ai-chat button {
  padding: 10px 16px;
  color: #ffffff;
  background: #244ec9;
  border: 0;
  border-radius: 10px;
  font: inherit;
  cursor: pointer;
}

.ai-chat button:disabled {
  opacity: 0.55;
  cursor: not-allowed;
}

.ai-chat button:focus-visible,
.ai-chat textarea:focus-visible {
  outline: 3px solid #8aa9ff;
  outline-offset: 3px;
}

@media (max-width: 600px) {
  .ai-chat {
    margin: 0;
    width: 100%;
    height: 100dvh;
    min-height: 0;
    border: 0;
    border-radius: 0;
  }

  .ai-chat__message {
    max-width: 95%;
  }
}
```

Les couleurs différencient les interlocuteurs, mais les libellés « Vous » et « Assistant » restent présents. La compréhension ne dépend donc pas uniquement du bleu et du gris.

Testez le composant avec un paragraphe long, une adresse sans espaces et plusieurs retours à la ligne. Ces contenus révèlent rapidement les débordements invisibles dans une maquette.

Pour une déclinaison iOS, VP0 peut servir de point de départ visuel avec ses designs Expo React Native. Les composants HTML et le CSS de cet exemple web devront toutefois être adaptés aux composants natifs.

## Comment connecter le composant à une vraie IA ?

Connectez le composant à une route de votre serveur qui reçoit la conversation et renvoie un texte. Le navigateur appelle votre application, puis votre application contacte le fournisseur IA avec sa clé secrète.

Ajoutez cette fonction dans un fichier `requestReply.ts` :

```ts
import type { GetReply } from "./AIChat";

export const requestReply: GetReply = async (messages, signal) => {
  const response = await fetch("/api/chat", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    signal,
    body: JSON.stringify({
      messages: messages
        .filter((message) => message.id !== "welcome")
        .map(({ role, content }) => ({ role, content })),
    }),
  });

  if (!response.ok) {
    throw new Error("La requête a échoué");
  }

  const data: unknown = await response.json();

  if (
    typeof data !== "object" ||
    data === null ||
    !("reply" in data) ||
    typeof data.reply !== "string" ||
    !data.reply.trim()
  ) {
    throw new Error("Réponse invalide");
  }

  return data.reply;
};
```

Passez-la ensuite au composant :

```tsx
import AIChat from "./AIChat";
import { requestReply } from "./requestReply";

export default function App() {
  return <AIChat getReply={requestReply} />;
}
```

La route `/api/chat` est un chemin interne à votre application. Vous devez la créer dans votre backend ou configurer votre serveur de développement pour transmettre cette requête au bon service.

### Quel contrat prévoir côté serveur ?

Le serveur doit accepter un objet contenant une liste de messages et répondre avec ce format :

```json
{
  "reply": "Voici la réponse de l’assistant."
}
```

L’implémentation dépend de votre framework et du fournisseur choisi. Le composant reste indépendant de ces décisions.

Avant de transmettre la conversation au modèle, vérifiez la structure des messages, leurs rôles, leur longueur et le nombre d’éléments. Ajoutez vos instructions système après cette validation.

La limite de saisie du navigateur améliore l’expérience, mais elle ne protège pas le serveur. Un appel direct peut envoyer davantage de texte.

Prévoyez également une limite de fréquence et un délai maximal côté serveur. Le bouton « Arrêter » interrompt la requête du navigateur ; il ne garantit pas à lui seul que le fournisseur cesse immédiatement la génération.

## Comment ajouter des réponses progressives et un historique ?

Ajoutez les réponses progressives lorsque votre échange complet fonctionne déjà. Pour l’historique, commencez par décider si la conversation doit survivre à un rechargement, à une déconnexion ou à un changement d’appareil.

Ces besoins demandent des mécanismes différents.

### Afficher la réponse progressivement

La diffusion progressive, souvent appelée streaming, affiche le texte au fur et à mesure de son arrivée.

Le code précédent attend un objet JSON complet. Pour passer au streaming, faites évoluer le contrat serveur et la fonction cliente ensemble.

Créez d’abord un message assistant vide avec un identifiant stable. À chaque fragment reçu, mettez à jour ce message précis. Ne créez pas une nouvelle bulle pour chaque morceau de texte.

Le traitement dépend du format transmis : texte brut, événements serveur ou protocole propre à un SDK. Un fragment réseau ne correspond pas forcément à un événement complet. Votre lecteur doit conserver les données incomplètes jusqu’à leur assemblage.

Prévoyez aussi ce qui arrive après une interruption : une réponse partielle peut rester visible, accompagnée d’une indication claire.

### Conserver la conversation

L’état React suffit pour une session temporaire. Dès que le composant disparaît ou que la page se recharge, cette conversation peut être perdue.

Une sauvegarde locale convient à un prototype sans données sensibles, mais elle appartient au navigateur utilisé. Elle ne synchronise pas automatiquement plusieurs appareils.

Pour un historique associé à un compte, enregistrez les conversations sur le serveur avec leur propriétaire. Vérifiez cette propriété à chaque lecture et modification.

Ajoutez une commande de suppression et expliquez ce qui est conservé. Une conversation peut contenir des informations personnelles même si votre formulaire ne les demande pas explicitement.

## Comment éviter les erreurs fréquentes dans un chat React ?

Vérifiez les transitions entre saisie, attente, réponse et erreur. Les problèmes apparaissent souvent à ces frontières plutôt que dans l’affichage des messages.

### Empêcher les doubles envois

Le bouton désactivé réduit les soumissions répétées. La référence `requestRef` constitue une seconde protection : elle est renseignée immédiatement au début de la requête.

Vous évitez ainsi de dépendre uniquement du prochain affichage de l’état `pending`.

### Garder une erreur distincte d’une réponse

Une panne réseau ne doit pas apparaître comme une affirmation de l’assistant. Affichez-la dans une zone séparée, comme dans l’exemple.

Le message utilisateur reste dans la conversation après un échec. Pour un produit complet, ajoutez une action « Réessayer » qui renvoie le même historique sans dupliquer ce message.

### Préserver la lecture pendant le défilement

L’exemple descend automatiquement après chaque nouveau message. Ce comportement convient à une conversation courte, mais peut gêner une personne qui relit un échange précédent.

Dans une version plus avancée, faites défiler uniquement lorsque l’utilisateur se trouve déjà près du bas. Sinon, affichez une commande « Nouveau message ».

### Afficher les réponses sans injecter de HTML

Le composant affiche les réponses comme du texte. Les retours à la ligne sont conservés grâce au CSS.

Si vous ajoutez du Markdown, choisissez un moteur de rendu configuré pour écarter le HTML brut non fiable. Traitez les liens, images et blocs de code comme des fonctionnalités explicites.

N’insérez pas directement une réponse de modèle avec `dangerouslySetInnerHTML`.

## Quand ce composant ne suffit-il plus ?

Ce composant convient à un prototype textuel avec une seule conversation et une requête active à la fois. Il ne fournit pas l’authentification, la persistance, les pièces jointes ou l’exécution d’actions.

Pour un assistant qui consulte des commandes ou modifie un compte, les autorisations doivent être vérifiées côté serveur. Une instruction dans le prompt ne remplace pas un contrôle d’accès.

Le composant nécessite également des adaptations pour les conversations très longues, les réponses progressives et les lecteurs d’écran. Testez notamment l’annonce des nouveaux messages sans répétition excessive.

VP0 fournit des points de départ d’interface pour les apps iOS, mais ne remplace pas le backend de votre assistant. Les comptes, les données et les appels au modèle restent à construire.

## À retenir : un composant de chat IA React fiable

Commencez par la démonstration locale pour vérifier le formulaire, les messages et les styles. Connectez ensuite votre serveur en conservant le même contrat de réponse.

Avant d’ajouter des animations ou du Markdown, testez les situations qui interrompent une conversation : réseau indisponible, réponse vide, arrêt manuel et fermeture de l’écran.

Pour vérifier votre première version :

- Envoyez un message contenant uniquement des espaces.
- Cliquez rapidement plusieurs fois sur « Envoyer ».
- Arrêtez une réponse en cours.
- Simulez une erreur du serveur.
- Affichez un message très long sur mobile.
- Parcourez le formulaire uniquement au clavier.

Vous obtenez ainsi une base dont le comportement est compréhensible avant d’étendre ses fonctions.

## Questions fréquentes

### Comment créer un composant chat IA React gratuit ?

Vous pouvez construire l’interface avec React, TypeScript et du CSS, sans bibliothèque de chat supplémentaire. L’exemple fonctionne avec une réponse locale gratuite. Pour de vraies réponses IA, le coût dépend du modèle et de l’infrastructure utilisés. Une interface gratuite ne rend pas automatiquement les appels au modèle gratuits.

### Est-ce que le code fonctionne sans backend ?

Oui, avec la fonction de démonstration intégrée. Elle produit une réponse locale et permet de vérifier l’interface. La fonction `requestReply` nécessite en revanche une route `/api/chat` implémentée sur votre serveur. Copier seulement la partie cliente ne crée pas cette route.

### Pourquoi ne faut-il pas placer la clé IA dans React ?

Une clé envoyée au navigateur peut être récupérée par l’utilisateur. Conservez les secrets sur le serveur et faites appeler votre propre route par le composant. Vérifiez aussi les droits et les limites d’utilisation : masquer la clé ne suffit pas à contrôler l’accès à votre service.

### Comment adapter ce chat à une application iOS ?

Remplacez les éléments HTML par des composants React Native et adaptez la saisie, le clavier et les zones de sécurité. VP0 est un point de départ gratuit pour le design iOS avec Expo React Native. Le code web présenté ici nécessite une adaptation ; son CSS ne se copie pas directement dans une interface native.

### Comment afficher du code dans les réponses de l’assistant ?

Ajoutez un rendu Markdown avec des blocs de code, un défilement horizontal et une commande de copie. Gardez le HTML brut non fiable désactivé. Testez les extraits longs sur mobile et affichez le langage lorsqu’il est connu, sans laisser le moteur de rendu exécuter le contenu reçu.
