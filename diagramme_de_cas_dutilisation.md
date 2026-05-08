Voici le **diagramme de cas d’utilisation** du projet **Mémoire Écrite**, représenté en langage **Mermaid** (texte lisible qui peut être converti en image par n’importe quel outil supportant Mermaid, comme GitHub, Notion, Draw.io, ou un éditeur en ligne).

Vous pouvez copier le code ci-dessous dans un environnement Mermaid (par exemple [mermaid.live](https://mermaid.live)) pour obtenir une image prête à être intégrée dans votre documentation.

```mermaid
graph TD
    %% Acteurs
    Visiteur["Visiteur (non connecté)"]
    Auteur["Auteur (connecté)"]
    Relecteur["Relecteur (comité)"]
    Editeur["Éditeur (administrateur)"]

    %% Boîte système
    box "Mémoire Écrite"
        usecase_consulter["Consulter les publications"]
        usecase_recherche["Rechercher (mot-clé)"]
        usecase_filtres["Appliquer des filtres (type, discipline, année)"]
        usecase_inscription["S'inscrire"]
        usecase_connexion["Se connecter"]
        usecase_deconnexion["Se déconnecter"]
        usecase_soumettre["Soumettre un article"]
        usecase_modifier_brouillon["Modifier son brouillon"]
        usecase_suivi["Suivre l’état de sa soumission"]
        usecase_evaluer["Évaluer un article (accepter/refuser/demander modif)"]
        usecase_doi["Attribuer un DOI"]
        usecase_publier["Publier l’article (rendre public)"]
        usecase_gerer_users["Gérer les utilisateurs (rôles)"]
        usecase_gerer_categories["Gérer les catégories (types, disciplines)"]
        usecase_statistiques["Consulter les statistiques"]
    end

    %% Associations Visiteur
    Visiteur --> usecase_consulter
    Visiteur --> usecase_recherche
    Visiteur --> usecase_filtres
    Visiteur --> usecase_inscription

    %% Associations Auteur (inclut Visiteur, donc hérite)
    Auteur --> usecase_connexion
    Auteur --> usecase_soumettre
    Auteur --> usecase_modifier_brouillon
    Auteur --> usecase_suivi
    Auteur --> usecase_deconnexion

    %% Relations d'héritage (extension)
    Auteur --|> Visiteur

    %% Associations Relecteur
    Relecteur --> usecase_connexion
    Relecteur --> usecase_evaluer
    Relecteur --> usecase_deconnexion
    Relecteur --|> Auteur   (un relecteur peut aussi soumettre ? optionnel, on le laisse pour flexibilité)

    %% Associations Éditeur
    Editeur --> usecase_connexion
    Editeur --> usecase_doi
    Editeur --> usecase_publier
    Editeur --> usecase_gerer_users
    Editeur --> usecase_gerer_categories
    Editeur --> usecase_statistiques
    Editeur --> usecase_deconnexion
    Editeur --|> Relecteur
```

## Légende et explications

| Symbole | Signification |
|---------|----------------|
| **Visiteur** | Personne non authentifiée qui parcourt le site. |
| **Auteur** | Utilisateur connecté ayant le rôle « auteur ». Il hérite des droits du visiteur (flèche blanche). |
| **Relecteur** | Membre du comité de lecture. Il hérite des droits de l’auteur (il peut aussi soumettre, mais on peut restreindre si besoin). |
| **Éditeur** | Administrateur. Il hérite des droits du relecteur. |
| **Boîte système** | Délimite le périmètre de la plateforme Mémoire Écrite. |

## Cas d’utilisation détaillés

| Cas d’utilisation | Acteur principal | Description brève |
|------------------|------------------|-------------------|
| Consulter les publications | Visiteur | Voir la liste et le détail des articles publiés. |
| Rechercher | Visiteur | Effectuer une recherche par mot‑clé (titre, auteur, résumé). |
| Appliquer des filtres | Visiteur | Filtrer par type (recherche/littérature/cinéma), discipline, année. |
| S’inscrire | Visiteur | Créer un compte (devient auteur). |
| Se connecter | Auteur, Relecteur, Éditeur | Accéder à son espace personnel. |
| Soumettre un article | Auteur | Déposer un nouveau texte avec fichier PDF et métadonnées. |
| Modifier son brouillon | Auteur | Corriger ou compléter une soumission en cours (statut brouillon). |
| Suivre l’état de sa soumission | Auteur | Voir si l’article est en attente, en relecture, accepté ou refusé. |
| Évaluer un article | Relecteur | Donner un avis (accepter, refuser, demander modifications) avec commentaire. |
| Attribuer un DOI | Éditeur | Générer et enregistrer un DOI pour un article accepté. |
| Publier l’article | Éditeur | Rendre l’article visible sur le site public. |
| Gérer les utilisateurs | Éditeur | Modifier les rôles, activer/désactiver des comptes. |
| Gérer les catégories | Éditeur | Ajouter ou modifier les types de contenu et les disciplines. |
| Consulter les statistiques | Éditeur | Voir le nombre de soumissions, de publications, de visiteurs. |

Ce diagramme sert de référence pour l’équipe de développement : chaque cas d’utilisation sera implémenté dans une ou plusieurs vues Django, avec les permissions correspondantes.