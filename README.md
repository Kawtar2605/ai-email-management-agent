# ai-email-management-agent
Agent IA de gestion d’emails développé avec Make, OpenAI et Outlook permettant la classification intelligente des emails, le routage automatique et la génération de brouillons professionnels.


# AI Email Management Agent

## Présentation du projet

Ce projet consiste en la conception d’un agent IA de gestion d’emails développé avec :

- Make
- OpenAI
- Microsoft Outlook

L’objectif du système est d’automatiser intelligemment la gestion des emails professionnels grâce à l’intelligence artificielle.

L’agent est capable de :

- analyser le contenu des emails
- comprendre l’intention du message
- classifier automatiquement les emails
- détecter si une réponse est nécessaire
- générer des brouillons professionnels cohérents
- organiser automatiquement les emails dans différents dossiers Outlook

---

# Objectif du projet

Ce projet a été réalisé dans une logique de :

- transformation digitale
- automatisation des tâches administratives
- optimisation de la gestion des emails
- démonstration d’un workflow IA appliqué à un contexte métier réel

Le système a été pensé comme une première base d’assistant IA professionnel capable d’aider une entreprise dans la gestion quotidienne de sa communication email.

---

# Stack utilisée

## Automatisation

- Make (Integromat)

## Intelligence artificielle

- OpenAI API
- Prompt engineering

## Messagerie

- Microsoft Outlook 365

## Documentation & versioning

- GitHub

---

# Architecture du workflow

Le scénario fonctionne selon la logique suivante :

1. Réception d’un email via Outlook
2. Analyse complète du contenu par OpenAI
3. Classification automatique :
   - lead_potentiel
   - rdv
   - urgent
   - a_verifier
   - autre
4. Détection du besoin de réponse
5. Génération d’un brouillon professionnel
6. Routage automatique dans les dossiers Outlook

---

# Choix des modules Make

## 1. Microsoft 365 Email — Watch Emails

Ce module a été utilisé comme point d’entrée du scénario.

Son rôle est de surveiller la boîte Outlook et de détecter les nouveaux emails entrants.

Il a été choisi car il permet de déclencher automatiquement le workflow dès qu’un nouveau message arrive, sans intervention manuelle.

---

## 2. OpenAI — Transform Text to Structured Data

Ce module constitue le cœur intelligent du système.

Il permet d’analyser le contenu de l’email et de retourner une sortie structurée avec plusieurs champs :

- category
- needs_reply
- reply_type
- priority
- reason
- reply_draft

Ce module a été choisi parce qu’il permet de transformer un email non structuré en données exploitables par Make.

---

## 3. Router

Le Router permet de séparer le scénario en plusieurs chemins selon la catégorie détectée par l’IA.

Il a été choisi pour organiser automatiquement les emails selon leur intention métier :

- lead_potentiel
- rdv
- urgent
- a_verifier
- autre

---

## 4. Filtres Make

Chaque route du Router contient un filtre basé sur la valeur retournée par OpenAI.

Exemple :

- si category = lead_potentiel → route Lead_Potentiel
- si category = rdv → route RDV
- si category = urgent → route Urgent
- si category = a_verifier → route A_Verifier
- si category = autre → route Autre

Les filtres permettent de rendre le workflow logique, lisible et contrôlé.

---

## 5. Microsoft 365 Email — Move an Email to a Folder

Ce module est utilisé après chaque route pour déplacer automatiquement l’email vers le bon dossier Outlook.

Il a été choisi pour transformer l’analyse IA en action concrète dans la boîte mail.

---

## 6. Microsoft 365 Email — Create a Draft Email

Ce module permet de créer un brouillon de réponse lorsque l’IA estime qu’une réponse est nécessaire.

Il a été choisi pour garder une logique human-in-the-loop : l’agent prépare la réponse, mais l’humain garde la validation finale avant envoi.

---

# Classification des emails

## lead_potentiel

Emails montrant un intérêt professionnel ou commercial.

Exemples :
- demande d’informations
- besoin d’accompagnement
- optimisation interne
- automatisation
- transformation digitale

---

## rdv

Emails liés à :
- disponibilités
- réunions
- appels
- planification

---

## urgent

Emails signalant :
- incident
- blocage
- dysfonctionnement
- problème critique

---

## a_verifier

Emails :
- ambigus
- incomplets
- nécessitant une validation humaine

---

## autre

Emails :
- newsletters
- publicités
- notifications automatiques
- messages sans action réelle

---

# Prompt Engineering

Une grande partie du projet a consisté à améliorer progressivement le prompt IA afin de :

- éviter les réponses incohérentes
- empêcher les hallucinations
- éviter les réponses contradictoires
- améliorer la logique conversationnelle
- rendre les réponses plus naturelles
- améliorer le formatage email
- éviter les répétitions inutiles

Le prompt a été continuellement optimisé après chaque phase de test.

---

# Difficultés rencontrées

## 1. Compréhension du fonctionnement de Make

Au début du projet, une difficulté importante a été de comprendre la logique de Make :

- déclencheur
- modules
- routes
- filtres
- mapping des variables
- exécution progressive du scénario

Il a fallu comprendre que chaque module reçoit les données du module précédent et que les champs doivent être correctement mappés pour que le workflow fonctionne.

---

## 2. Gestion du module Outlook Watch Emails

Le premier module devait récupérer correctement les emails entrants.

Plusieurs points ont dû être clarifiés :

- le scénario ne traite pas forcément tous les emails d’un coup
- le nombre d’emails traités dépend du paramètre de limite
- Run once permet de tester manuellement
- l’activation planifiée permet ensuite d’automatiser l’exécution

---

## 3. Choix du bon contenu à envoyer à OpenAI

Une difficulté importante a été de choisir quelles données transmettre à l’IA.

Au départ, l’analyse pouvait être trop limitée si seul l’objet du mail était envoyé.

Le workflow a ensuite été amélioré en envoyant :

- l’objet du mail
- le contenu du mail
- le corps du message disponible dans Outlook

Cela permet à l’IA d’analyser le contexte global et pas uniquement des mots-clés.

---

## 4. Définition des catégories

Il a fallu construire une logique de classification claire.

Les catégories devaient être suffisamment simples pour rester stables, mais assez précises pour être utiles :

- lead_potentiel
- rdv
- urgent
- a_verifier
- autre

Une difficulté a été d’éviter d’ajouter trop de catégories, ce qui aurait rendu le système plus complexe et moins fiable.

---

## 5. Création d’un prompt stable

Le prompt a nécessité plusieurs itérations.

Les premières versions produisaient parfois :

- des réponses trop génériques
- des classifications trop simples
- des réponses incohérentes
- des formulations trop robotiques
- des actions non confirmées

Le prompt a ensuite été renforcé avec des règles précises sur :

- la compréhension du contexte global
- l’interdiction d’inventer des informations
- la cohérence conversationnelle
- la prudence dans les réponses
- le formatage des emails

---

## 6. Gestion du structured output

Une autre difficulté a été de définir les bons champs de sortie dans OpenAI.

Le système devait retourner une structure claire pour que Make puisse ensuite utiliser les données.

Les champs retenus sont :

- category
- needs_reply
- reply_type
- priority
- reason
- reply_draft

Cette étape était importante car le Router et les filtres Make dépendent directement de ces valeurs.

---

## 7. Configuration du Router et des filtres

Le Router a posé plusieurs difficultés.

Chaque route devait être configurée avec une condition précise.

Exemple :

```text
category = urgent
```

ou :

```text
category = rdv
```

Une erreur de casse, d’espace ou de valeur pouvait empêcher la bonne route de se déclencher.

---

## 8. Mapping des dossiers Outlook

Une difficulté concrète a été de sélectionner les bons dossiers Outlook dans Make.

Les dossiers ne s’affichaient pas toujours avec un nom très clair, mais parfois avec un identifiant technique.

Il a fallu vérifier que chaque route déplaçait bien l’email dans le dossier correspondant :

- Lead_Potentiel
- RDV
- Urgent
- A_Verifier
- Autre

---

## 9. Création des brouillons de réponse

La génération de brouillons a été une étape plus complexe que le simple tri.

Il fallait que l’agent sache :

- quand répondre
- quand ne pas répondre
- quel type de réponse générer
- comment rester cohérent avec le mail reçu
- comment éviter les réponses inutiles

La logique human-in-the-loop a été conservée : l’agent crée un brouillon, mais n’envoie pas automatiquement le mail.

---

## 10. Problème de destinataire dans le brouillon

Une erreur est apparue lors de la création des brouillons : l’adresse email du destinataire n’était pas toujours correctement mappée.

Le module Outlook attendait une adresse email valide.

La correction a consisté à utiliser le bon champ du module Outlook d’origine, correspondant à l’adresse email réelle de l’expéditeur.

---

## 11. Problème de formatage des emails

Les premiers brouillons étaient parfois affichés avec un texte trop compact.

Exemple :

```text
Bonjour,Merci pour votre message...
```

Pour corriger cela, le prompt a été modifié afin de générer les réponses en HTML simple avec :

```html
<br><br>
```

Cela a permis d’obtenir des brouillons beaucoup plus lisibles et professionnels.

---

## 12. Réponses trop génériques

Même après amélioration, certaines réponses restaient génériques.

Cette limite vient principalement du fait que l’agent ne dispose pas encore :

- d’un contexte entreprise complet
- d’une base de connaissances métier
- d’un CRM
- d’un historique client
- d’un calendrier connecté

Cette limite a été identifiée comme une amélioration future plutôt qu’un bug du V1.

---

## 13. Gestion des cas ambigus

Certains emails pouvaient appartenir à plusieurs catégories.

Exemple :

- un lead qui propose aussi un rendez-vous
- un problème urgent avec demande d’échange
- une demande vague pouvant être un lead ou un email à vérifier

Ces cas ont montré que l’évaluation d’un agent IA n’est pas toujours binaire. Certaines classifications peuvent être acceptables même si elles sont discutables.

---

## 14. Maîtrise des coûts et crédits API

Une contrainte importante a été la consommation de crédits OpenAI.

Les tests ont donc été réalisés progressivement afin de valider la stabilité du système sans multiplier inutilement les appels API.

Une fois le comportement jugé suffisamment stable, les tests massifs ont été arrêtés pour éviter de consommer des crédits sans valeur ajoutée.

---

## 15. Stabilisation du MVP

La dernière difficulté a été de savoir quand arrêter d’ajouter des fonctionnalités.

Le projet aurait pu être complexifié avec :

- calendrier
- CRM
- envoi automatique
- mémoire conversationnelle
- notifications externes

Mais le choix a été fait de stabiliser d’abord un V1 clair :

- classification
- routage
- brouillons
- validation humaine

Cette décision permet de garder un projet compréhensible, démontrable et crédible.

---

# Résultats observés

Le système parvient désormais à :

- classifier correctement la majorité des emails
- détecter lorsqu’aucune réponse n’est nécessaire
- générer des brouillons cohérents
- organiser automatiquement les emails Outlook
- maintenir une logique conversationnelle simple

---

# Limites actuelles

Le système reste volontairement générique.

L’agent ne possède pas encore :

- de contexte interne d’entreprise
- de mémoire conversationnelle avancée
- d’accès CRM
- de connaissance métier spécifique
- de personnalisation avancée

Cela entraîne parfois des réponses :

- prudentes
- vagues
- très générales

---

# Perspectives d’amélioration

Les prochaines évolutions possibles :

- connexion à Google Calendar / Outlook Calendar
- suggestions automatiques de créneaux réels
- intégration CRM
- mémoire conversationnelle
- personnalisation métier
- validation humaine avant envoi
- dashboard de supervision
- scoring des leads
- agents IA multi-étapes

---

# Auteur

Projet réalisé par Kawtar Maarof.

GitHub :
https://github.com/Kawtar2605
