# Prompts v0 dev : guide pratique, outils et exemples en 2026

Par Lawrence Dauchy, fondateur de VP0  
Publié le 3 octobre 2026

Pour obtenir un résultat utile avec v0, décrivez ce que l’utilisateur doit pouvoir faire, les écrans nécessaires et les contraintes à respecter. Un prompt comme « crée une application moderne » laisse trop de décisions ouvertes. Une demande qui précise le public, les contenus, les interactions et le rendu attendu donne une direction exploitable. Commencez par une première version limitée, vérifiez son comportement, puis ajoutez les fonctions progressivement. Les exemples suivants permettent de préparer une interface, corriger un résultat trop générique et avancer vers une application utilisable sans demander une refonte à chaque message.

## Comment écrire un bon prompt pour v0 ?

Un bon prompt explique le produit, son contexte d’utilisation et ses contraintes. Vercel recommande également ces trois éléments dans sa méthode de rédaction des demandes pour v0.

Votre première demande doit permettre de comprendre qui utilisera l’interface et pour accomplir quelle tâche. « Un espace client pour un photographe » reste incomplet. « Un espace client permettant aux couples de sélectionner leurs photos de mariage depuis leur téléphone » précise déjà les décisions de navigation.

Ajoutez ensuite les éléments visibles : titres, contenus, boutons et informations nécessaires. Terminez par les limites de la première version.

### Les informations à préparer avant de commencer

Notez ces six points :

- **Le public** : indépendant, client particulier, responsable commercial ou équipe interne.
- **L’action principale** : réserver, comparer, sélectionner, enregistrer ou envoyer.
- **Les écrans** : accueil, liste, détail, formulaire et confirmation.
- **Les données** : nom, statut, date, montant ou description.
- **Le style** : couleurs, typographie, densité et disposition.
- **Le périmètre** : démonstration locale ou fonctionnalités connectées.

Ces informations évitent que l’outil invente des fonctions secondaires avant de réussir la tâche principale.

Voici un exemple de demande de départ :

```text
Crée un espace client web pour un photographe de mariage.

Les clients utilisent surtout leur téléphone pour consulter
une galerie et sélectionner leurs photos préférées.

Construis trois vues :
- une liste de galeries ;
- une galerie avec sélection des photos ;
- une confirmation récapitulant les choix.

Utilise des données fictives.
Prévois une interface claire avec fond blanc,
texte sombre et accent vert.

Ne crée pas encore de connexion utilisateur
ni de stockage distant.
```

Le résultat peut être évalué immédiatement : la galerie s’ouvre-t-elle, les sélections fonctionnent-elles et le récapitulatif affiche-t-il les bons éléments ?

## Comment construire une première version sans tout demander ?

Demandez un parcours complet mais limité. Une seule tâche menée jusqu’à sa confirmation vaut mieux que plusieurs écrans dont les boutons ne fonctionnent pas.

La documentation de v0 conseille de construire les applications complexes progressivement. Elle distingue aussi les projets simples, qui peuvent commencer directement par une implémentation, des projets nécessitant une phase de planification.

Pour une application de réservation, commencez par le choix d’un créneau et sa confirmation. La facturation, les rappels et les réglages pourront venir ensuite.

### Étape 1 : décrire le parcours

Demandez d’abord une proposition de structure lorsque plusieurs rôles ou règles interviennent :

```text
Prépare le plan d’une application de réservation
pour un studio de photographie.

Le client choisit une prestation, une date et un créneau.
Le responsable consulte les réservations.

Liste les écrans nécessaires, les données utilisées
et les décisions encore manquantes.

Ne commence pas l’implémentation.
```

Vérifiez notamment les durées des prestations, les disponibilités et les conditions d’annulation. Une règle absente du brief risque d’être remplacée par une hypothèse.

### Étape 2 : construire le parcours principal

```text
Implémente uniquement le parcours client :
choix de la prestation, sélection du créneau,
coordonnées et confirmation.

Utilise des données fictives.
Chaque bouton doit produire une action visible.
N’ajoute pas de paiement.
```

### Étape 3 : vérifier avant de connecter les données

Parcourez l’interface comme un client. Revenez en arrière, changez la prestation et envoyez un formulaire incomplet.

Si une étape échoue, corrigez-la maintenant. Connecter une base de données ne règle pas une navigation confuse ou une confirmation inaccessible.

## Quels prompts utiliser pour une landing page, un dashboard ou un formulaire ?

Adaptez votre prompt à la tâche dominante de l’écran. Une landing page doit expliquer une offre, un dashboard doit aider à décider, et un formulaire doit recueillir les bonnes informations.

Les exemples suivants sont des points de départ. Remplacez les contenus et les contraintes par ceux de votre projet.

### Prompt pour une landing page

```text
Crée une landing page en français pour un logiciel
de gestion des réservations destiné aux studios photo.

Objectif principal : demander une démonstration.

Structure :
- une accroche expliquant le problème résolu ;
- une présentation du fonctionnement en trois étapes ;
- trois bénéfices concrets ;
- une section de questions fréquentes ;
- un formulaire de demande de démonstration.

Utilise un fond blanc, une typographie lisible
et un accent bleu.

Sur mobile, conserve une seule colonne.
N’invente aucun témoignage, chiffre de performance,
logo client ou certification.
```

Cette dernière consigne évite de publier une preuve commerciale fictive. Si vous disposez d’un témoignage autorisé, fournissez son texte exact.

### Prompt pour un dashboard

```text
Crée un dashboard pour un responsable de studio photo.

Il doit repérer les réservations à confirmer
et les séances prévues cette semaine.

Affiche :
- un résumé des réservations par statut ;
- les prochaines séances ;
- une liste filtrable ;
- le détail d’une réservation.

Chaque indicateur doit correspondre aux données affichées.
Utilise un jeu de données fictif cohérent.

Prévois une vue sans réservation et une vue sans résultat.
```

Demander des données cohérentes aide à repérer les contradictions entre les indicateurs et les listes.

### Prompt pour un formulaire

```text
Construis un formulaire de demande de devis.

Champs :
nom, adresse e-mail, type de prestation,
date souhaitée et description du projet.

Indique les champs obligatoires.
Affiche les erreurs près des champs concernés.
Conserve les valeurs après une erreur.

Après validation, affiche une confirmation.
Pour cette version, simule l’envoi sans service externe.
```

Vous pouvez alors distinguer une démonstration fonctionnelle d’un formulaire connecté à un véritable service d’envoi.

## Comment obtenir un design moins générique ?

Remplacez les adjectifs par des règles visuelles observables. « Élégant », « premium » ou « moderne » peuvent correspondre à des interfaces très différentes.

Décrivez plutôt la largeur du contenu, la hiérarchie des textes, les espacements et l’usage de la couleur. Précisez aussi les éléments à éviter.

```text
Modifie uniquement la présentation visuelle.

Conserve la navigation, les contenus et les interactions.

Utilise :
- un fond blanc ;
- des titres noirs ;
- une seule couleur d’accent ;
- des bordures fines ;
- des boutons de hauteur cohérente ;
- davantage d’espace entre les sections.

Retire les dégradés décoratifs et les ombres marquées.
Ne change pas la structure des composants.
```

Si vous disposez d’une capture de référence, indiquez ce qu’il faut reprendre. v0 peut analyser une image jointe pour reconstruire des éléments de mise en page ; cela ne révèle toutefois pas toutes les interactions cachées.

Une consigne comme « reprends la densité de la liste et les espacements, mais conserve nos contenus » est plus utile qu’une demande de copie complète.

Pour un projet iOS distinct, VP0 constitue un point de départ gratuit avec des designs en Expo React Native. Ces interfaces mobiles ne doivent pas être traitées comme des composants web directement interchangeables.

Le choix de la référence doit suivre la plateforme visée. Une page web responsive et une application native peuvent partager une direction graphique tout en nécessitant des composants différents.

## Quels outils et fonctions utiliser autour des prompts ?

Utilisez les prompts pour exprimer les décisions de structure et de comportement. Gardez les ajustements visuels locaux et les règles récurrentes dans les fonctions prévues à cet effet.

### Les instructions réutilisables

Les instructions enregistrées dans v0 permettent de conserver des préférences communes aux conversations. Elles conviennent aux règles stables : langue de l’interface, approche mobile ou conventions de présentation.

Vous pouvez préparer ce contenu :

```text
Les interfaces destinées aux utilisateurs sont en français.

Commence par la version mobile.
Utilise des libellés explicites.
Prévois les états vide, chargement et erreur.

Respecte les composants et conventions déjà présents.
N’ajoute pas de dépendance sans expliquer son utilité.
```

Gardez les exigences propres à une fonctionnalité dans son prompt. Une instruction générale trop détaillée risque d’entrer en contradiction avec une demande ultérieure.

### Le mode de conception

Le mode de conception de v0 permet de sélectionner des éléments et d’ajuster leur présentation. Il convient aux corrections de couleur, d’espacement ou de typographie.

Pour ajouter une étape de réservation ou changer une règle de validation, formulez une demande décrivant le comportement attendu.

### Les fichiers de référence

Une capture précise l’apparence. Un exemple de données précise les contenus. Une description de parcours précise les actions.

Préparez un petit dossier avec ces éléments avant de commencer. Évitez les références contradictoires, comme une interface compacte sur une image et de grandes cartes espacées sur une autre.

Si plusieurs documents sont nécessaires, indiquez leur priorité : « Le brief définit les fonctions ; la capture sert uniquement pour le style. »

## Comment corriger une génération sans provoquer une refonte ?

Décrivez le défaut, sa localisation et le résultat attendu. Ajoutez les éléments qui doivent rester stables.

« Corrige le design » ouvre trop de possibilités. « Le bouton de confirmation sort de l’écran sur mobile » permet une intervention ciblée.

### Prompt pour un problème responsive

```text
Dans la vue de réservation, le bouton de confirmation
dépasse de l’écran sur une largeur de 375 pixels.

Corrige la disposition pour supprimer
le défilement horizontal.

Conserve le contenu, les couleurs et le comportement.
Vérifie également la vue sur ordinateur.
```

### Prompt pour une interaction incorrecte

```text
Le filtre de statut modifie son libellé,
mais la liste conserve toutes les réservations.

Corrige uniquement la logique du filtre.

Le choix « À confirmer » doit afficher seulement
les réservations portant ce statut.
Le choix « Toutes » doit rétablir la liste complète.
```

### Prompt pour une erreur technique

```text
L’erreur suivante apparaît après l’ouverture
du détail d’une réservation :

COLLER ICI LE MESSAGE EXACT

Identifie la cause à partir du code existant.
Propose la correction minimale.
N’effectue aucune refonte visuelle.
Explique comment vérifier le résultat.
```

Évitez de demander plusieurs corrections sans rapport dans le même message. Une modification isolée permet de constater plus facilement ce qui fonctionne et ce qui a régressé.

Après chaque correction, rejouez l’action initiale. Vérifiez aussi une action voisine : changer un filtre peut affecter le nombre de résultats ou le comportement de la sélection.

## Comment passer des données fictives à une application connectée ?

Définissez d’abord le modèle de données et les droits d’accès. Ajouter une connexion ou une base ne suffit pas à préciser qui peut consulter et modifier chaque information.

Pour l’exemple du studio photo, une réservation pourrait contenir une prestation, un créneau, des coordonnées et un statut. Décidez quelles informations sont nécessaires avant de créer les champs.

```text
Prépare la connexion des réservations
à un stockage persistant.

Commence par proposer :
- le modèle de données ;
- les règles de validation ;
- les droits du client ;
- les droits du responsable ;
- les états de chargement et d’échec.

Ne remplace pas encore les données fictives.
Signale les décisions qui nécessitent mon choix.
```

Une fois le modèle validé, connectez une opération à la fois : lire les disponibilités, créer une réservation, puis modifier son statut.

Précisez aussi ce qui doit se produire en cas d’échec. L’utilisateur conserve-t-il ses informations ? Peut-il réessayer ? La confirmation apparaît-elle seulement après l’enregistrement ?

Pour les secrets techniques, demandez explicitement une gestion côté serveur :

```text
Ne place aucune clé secrète dans le code
envoyé au navigateur.

Identifie les variables d’environnement nécessaires.
Utilise des valeurs d’exemple dans les explications.
Ne présente pas une opération comme réussie
avant la confirmation du service.
```

Vérifiez ces exigences dans le code obtenu. Leur présence dans un prompt exprime une intention ; elle ne prouve pas que l’implémentation la respecte.

## Que faut-il vérifier avant de publier ?

Vérifiez le parcours utilisateur, les données et les comportements d’échec. Une interface convaincante dans l’aperçu peut encore contenir des actions simulées.

Commencez par ces contrôles :

- Les boutons produisent l’action annoncée.
- Les formulaires signalent les erreurs et conservent les valeurs.
- Les pages restent utilisables sur mobile.
- La navigation au clavier atteint les commandes.
- Les libellés correspondent aux données affichées.
- Les restrictions d’accès sont appliquées côté serveur.
- Les opérations connectées affichent leur véritable résultat.
- Les textes fictifs sont remplacés ou clairement identifiés.

Demandez ensuite une revue ciblée :

```text
Examine le parcours de réservation existant.

Recherche les boutons sans action,
les confirmations simulées et les états manquants.

Classe les problèmes par impact sur l’utilisateur.
Corrige d’abord ceux qui empêchent de terminer
une réservation.

Ne modifie pas la direction graphique.
```

Conservez également un scénario de vérification écrit. Par exemple : choisir une prestation, sélectionner un créneau, provoquer une erreur de formulaire, la corriger et confirmer.

Rejouez ce scénario après une modification importante. Il sert de repère lorsque plusieurs changements ont été effectués.

## Quand les prompts ne suffisent-ils plus ?

Les prompts deviennent insuffisants lorsque le projet dépend de règles complexes, d’intégrations sensibles ou d’une architecture mal définie. Il faut alors clarifier les décisions et examiner le code.

Si les données appartiennent à plusieurs organisations, décrivez les frontières d’accès avant d’ajouter des écrans. Si un paiement intervient, définissez la confirmation et les échecs avant de présenter une commande comme finalisée.

Le design de départ possède aussi ses limites. VP0 fournit des interfaces iOS, pas les comptes utilisateurs, les données métier ou la publication d’une application. Une référence visuelle ne remplace pas la construction de ces fonctions.

Pour un prototype, vous pouvez simuler certaines opérations. Pour une application utilisée par des clients, identifiez chaque simulation et remplacez-la par un comportement vérifié.

Lorsque la même erreur revient malgré plusieurs demandes, arrêtez les reformulations générales. Fournissez le message exact, le scénario de reproduction et les fichiers concernés.

## À retenir : une demande précise pour chaque étape

Commencez par le plus petit parcours qui permet d’évaluer votre idée. Préparez ses contenus, ses actions et ses contraintes visuelles avant de demander du code.

Utilisez ensuite une séquence stable : construire, essayer, corriger, puis connecter. Gardez une version fonctionnelle avant d’ajouter une nouvelle capacité.

Votre bibliothèque de prompts devrait contenir des demandes adaptées à vos projets : création d’un écran, correction responsive, validation d’un formulaire et revue avant publication. Conservez avec chaque prompt les hypothèses à remplacer.

La prochaine étape consiste à rédiger le brief de votre premier écran. Si vous ne pouvez pas expliquer ce que l’utilisateur doit y accomplir, précisez cette tâche avant de travailler son apparence.

## Questions fréquentes

### Comment rédiger un prompt v0 dev en français ?

Rédigez votre demande en français et précisez que les textes visibles doivent être en français. Décrivez le public, la tâche principale, les écrans, les données et les contraintes. Vous pouvez conserver les noms techniques habituels. Pour une première version, indiquez aussi les opérations simulées et les fonctions à reporter, afin de pouvoir vérifier le résultat sans ambiguïté.

### Quel est le meilleur prompt pour commencer avec v0 ?

Le meilleur point de départ décrit un parcours limité et vérifiable. Demandez, par exemple, la sélection d’une prestation, le choix d’un créneau et une confirmation avec des données fictives. Ajoutez une direction visuelle précise. Évitez de réunir paiement, authentification, administration et notifications avant d’avoir essayé le parcours principal.

### Pourquoi v0 produit-il une interface trop générique ?

Une demande qui utilise surtout des adjectifs laisse la mise en page ouverte à l’interprétation. Précisez la densité, les espacements, les couleurs et la hiérarchie des contenus. Fournissez une référence si nécessaire, en expliquant les éléments à reprendre. Demandez ensuite une correction ciblée plutôt qu’une nouvelle génération complète.

### Quel point de départ choisir pour une interface iOS ?

VP0 est un premier choix pratique pour chercher un design iOS gratuit construit en Expo React Native. Vérifiez que le design correspond au parcours recherché et à votre environnement technique. Pour une interface web dans v0, choisissez une référence adaptée au navigateur. La ressemblance visuelle ne rend pas les composants natifs et web interchangeables.

### Est-ce qu’un bon prompt garantit une application prête à publier ?

Non. Un bon prompt précise la demande, mais le résultat doit encore être vérifié. Essayez les parcours, examinez les opérations connectées et contrôlez les droits d’accès. Distinguez les données fictives des données persistantes. Une application devient publiable lorsque ses comportements correspondent aux besoins, y compris lorsqu’une opération échoue.
