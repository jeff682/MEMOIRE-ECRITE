
Cahier des charges – Projet « Mémoire Ecrite »
Plateforme web de publication, critique et échanges autour des œuvres culturelles

1. Contexte et objectif du projet
Nous souhaitons créer une plateforme numérique dédiée à la valorisation des œuvres culturelles (littérature, cinéma, théâtre, arts de la scène).
Sur cette plateforme, les administrateurs publient des articles (annonces d’événements, expressions libres).
Les utilisateurs inscrits peuvent à leur tour proposer des articles – obligatoirement attachés à une œuvre existante – ainsi que des critiques et des commentaires.

L’objectif est de disposer d’un outil modéré, interactif et facile à administrer, qui permette à la fois une éditorialisation officielle et une contribution citoyenne structurée.
2. Périmètre fonctionnel (ce que la plateforme doit faire)
2.1 Gestion des œuvres
Une œuvre est définie par : titre, auteur/réalisateur/metteur en scène, année, catégorie (littérature, cinéma, théâtre…), résumé, image (couverture/affiche).
Seuls les administrateurs créent, modifient ou suppriment des fiches œuvres.
Chaque œuvre a une page publique dédiée, qui rassemble les articles contributeurs, critiques et commentaires associés.
2.2 Rôles et permissions
Rôle
Capacités
Visiteur non inscrit
Consultation de tout le contenu, pas d’interaction.
Utilisateur inscrit
Proposer des articles contributeurs (liés à une œuvre), déposer des critiques et commentaires, gérer son espace personnel.
Contributeur (statut automatique)
Idem utilisateur, mais ses articles sont soumis à modération avant publication.
Administrateur
CRUD complet sur œuvres, articles officiels, modération de tous les contenus utilisateurs, gestion des utilisateurs.

2.3 Contenus publiés par les administrateurs
Deux formats :

Annonce d’événement : titre, date, lieu, programme, lien externe éventuel.
Expression libre : éditorial, billet personnel, sans rattachement obligatoire à une œuvre (mais un lien facultatif vers une œuvre est possible).
2.4 Contenus proposés par les utilisateurs (article contributeur)
Obligatoirement lié à une œuvre existante (sélection dans un menu déroulant).
Formulaire avec : titre, corps de texte (WYSIWYG), case à cocher « spoiler ».
Une fois soumis, l’article passe en modération (publié uniquement après validation admin).
L’utilisateur reçoit une notification par email du résultat.
2.5 Critiques (notes et avis)
Tout utilisateur inscrit peut déposer une critique sur une œuvre.
La critique contient : une note (ex. 0 à 5 étoiles) et un texte d’au moins 50 caractères.
Pas de modération a priori possible, mais :
Un bouton « Signaler » pour les lecteurs.
Après 3 signalements, la critique est masquée automatiquement et notifiée aux admins.
2.6 Commentaires
Commentaires possibles sur :
Tout article (admin ou contributeur)
Toute critique
Le commentaire est un texte simple avec pseudo et date.
Modération : publication immédiate pour les utilisateurs ayant déjà 3 commentaires approuvés ; sinon modération a priori (validation admin avant affichage).
2.7 Flux entrants : récupération de commentaires / critiques externes
La plateforme doit offrir deux moyens d’importer des commentaires/critiques depuis des sources externes :

API REST sécurisée (endpoint /api/comments et /api/reviews) permettant à des partenaires (réseaux sociaux, presse, etc.) de soumettre des contenus.
Import ponctuel par fichier CSV (interface admin) pour migrer des données existantes.

Chaque commentaire ou critique importé doit être attribué à un utilisateur existant ou à un compte générique « invité validé ».
3. Spécifications techniques
3.1 Stack technique imposée 
Back-end
Framework unique : Django (version LTS, par exemple 4.2 ou 5.0).
Django REST Framework (DRF) pour la création de l’API REST (endpoints /api/comments, /api/reviews, etc.).
Authentification : gestion des sessions Django + tokens d’API (DRF TokenAuthentication ou Simple JWT) pour les soumissions externes.
Base de données
PostgreSQL (recommandé pour la fiabilité et les performances) ou MySQL/MariaDB (acceptable).
L’agence utilisera l’ORM Django pour toutes les interactions.
Front-end
Templates Django pour les pages publiques (administration, modération, espace utilisateur) – solution intégrée rapide.
CSS : framework léger comme Tailwind CSS ou Bootstrap (au choix de l’agence, validé par le client).
JavaScript : vanilla JS pour les interactions simples (affichage des spoilers, signalements). Pour les composants riches (éditeur WYSIWYG, prévisualisation), utilisation de HTMX ou Alpine.js – pas de framework lourd type React, sauf justification forte (l’agence devra alors proposer un découpage SPA partiel).
Hébergement
L’application doit pouvoir tourner sur un hébergement mutualisé compatible Python/Django (via WSGI).
L’agence conseillera une solution adaptée au budget et au trafic prévu. Le client reste libre du choix final, mais l’agence livrera une configuration prête à déployer.

Exigences complémentaires
Utilisation des migrations Django pour la gestion du schéma de base de données.
Environnement virtuel (pipenv, poetry ou venv) et requirements.txt fourni.
Variables d’environnement pour séparer les paramètres de développement / production (SECRET_KEY, DEBUG, DATABASE_URL, etc.).
Interface admin Django : l’agence l’utilisera pour les fonctionnalités d’administration (modération des articles, signalements) sauf si une interface sur mesure est explicitement demandée. Dans ce cas, les deux coexisteront.
3.2 Exigences non fonctionnelles
Performance : temps de chargement des pages < 2 secondes (moyenne).
Responsive : adaptation aux mobiles, tablettes et desktop.
Sécurité : protection XSS, CSRF, injection SQL, rate limiting sur les soumissions (formulaires et API).
Accessibilité : niveau RGAA (référentiel général d’amélioration de l’accessibilité) partiel – navigation clavier, contrastes.
Maintenabilité : code commenté, documentation d’administration (manuel utilisateur pour les admins).
4. Contraintes légales et éditoriales
Modération : l’agence doit prévoir un tableau de bord admin pour :
Valider/rejeter les articles contributeurs.
Consulter les signalements.
Masquer/supprimer tout commentaire ou critique.
Charte d’utilisation : à intégrer sous forme de page « Conditions générales d’utilisation » et « Charte de modération » (textes fournis par le client).
Protection des données : conformité RGPD (droit à l’oubli, export des données personnelles, consentement à la publication). L’agence devra inclure une page « Politique de confidentialité » générique.
5. Livrables attendus
Livrable
Description
Plateforme fonctionnelle
Code source déployé sur un serveur de recette, puis sur serveur de production
Documentation technique
Architecture, variables d’environnement, procédure de backup/restauration
Manuel d’administration
Gestion des œuvres, modération, import CSV, configuration de l’API
Jeu de données de test
5 œuvres fictives, 3 articles admin, 5 articles contributeurs, 10 critiques, 20 commentaires
Formation (2h)
Session en visio pour les administrateurs (client)

6. Planning indicatif
Phase
Durée
Tâches clés
Cadrage et spécifications détaillées
1 semaine
Échanges sur les cas précis, validation des écrans
Conception (UX/UI)
1 semaines
Maquettes cliquables (Figma), validation client
Développement back-end + API
3 semaines
Modèles, endpoints, modération, import
Développement front-end
2 semaines
Pages publiques, formulaires, espace utilisateur, responsive
Tests & correction
2 semaines
Tests fonctionnels, sécurité, performance
Livraison & formation
1 semaine
Déploiement, recette finale, livraison des docs
Total
10 semaines



7. Budget indicatif
Poste
Description
Coût (FCFA HT)
1. Cadrage et spécifications
Atelier en ligne (2h) pour finaliser les user stories, validation des écrans
150 000
2. Conception (UI/UX)
Maquettes simples (type wireframes) pour les pages clés : accueil, fiche œuvre, formulaire article, espace utilisateur
250 000
3. Développement back-end Django
- Modèles (œuvres, articles admin, articles contributeurs, critiques, commentaires, signalements)
- Système d’authentification (inscription/connexion)
- Tableau de bord admin personnalisé pour la modération (basé sur l’admin Django)
- Workflow de validation des articles contributeurs
- Gestion des signalements et masquage automatique
1 200 000
4. Développement front-end (templates Django)
- Intégration des maquettes (HTML/CSS, Bootstrap ou Tailwind)
- Formulaires (ajout d’article contributeur, critique, commentaire)
- Page responsive (mobile/tablette)
- Affichage des spoilers avec bouton JS
800 000
5. API REST (Django REST Framework)
- Endpoint de récupération des œuvres (GET)
- Endpoint sécurisé pour soumettre commentaire/critique (POST)
- Clé d’API simple pour les partenaires
500 000
6. Import CSV (critiques / commentaires)
Interface admin pour importer un fichier CSV (colonnes définies) et assignation à un utilisateur existant ou générique
300 000
7. Tests, correction et déploiement
- Tests fonctionnels sur l’environnement de recette
- Correction des bugs bloquants
- Déploiement initial sur serveur fourni par le client (ou aide à la configuration)
250 000
8. Documentation et formation
- Manuel d’administration (PDF)
- Formation en visio (1h) pour les administrateurs
90 000
Total HT


3 540 000


8. Critères de validation (acceptation du projet)
Le projet sera considéré comme livré et accepté lorsque les conditions suivantes seront remplies :

Fonctionnalités : tous les cas d’usage décrits sections 2.1 à 2.7 sont opérationnels.
Modération : les trois flux (articles contributeurs, commentaires, signalements) fonctionnent comme défini.
API : les endpoints POST répondent correctement avec authentification.
Import CSV : possibilité d’importer au moins 100 commentaires en une fois.
Qualité : absence de blocage ou d’erreur critique durant la recette (sur Chrome, Firefox, Safari, Edge responsive).
Documentation : le manuel d’administration permet à un non-développeur de modérer et d’exporter les données.
9. Conditions particulières pour la réponse de l’agence
L’agence devra fournir dans sa proposition :

Une proposition technique détaillée (architecture, langages, bibliothèques).
Un planning réaliste et un devis.
Des références de projets similaires (plateformes avec modération de contenu utilisateur).
La politique de maintenance post-livraison (optionnelle) : correctifs, évolutions, support.


