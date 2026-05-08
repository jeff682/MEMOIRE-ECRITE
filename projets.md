Voici une explication claire et technique du projet **Mémoire Écrite**, destinée aux développeurs (mais restant accessible).

---

## Mémoire Écrite – Présentation pour les développeurs

### Objectif général

Mémoire Écrite est une plateforme web de publication savante en libre accès. Elle permet de diffuser des **résultats de recherche**, des **critiques littéraires** et des **critiques cinématographiques** sur l’Afrique et les diasporas africaines. Les contenus sont évalués par les pairs (peer review), obtiennent un **DOI** et sont exposés via une **API publique** pour être réutilisés par d’autres plateformes (Google Scholar, bibliothèques numériques, agrégateurs).

---

### Cible utilisateurs

- **Visiteurs** : lecture gratuite, recherche, filtres (type, discipline, année).
- **Auteurs** (compte) : soumission d’articles, suivi du processus d’évaluation.
- **Réviseurs** : évaluation (accepter/refuser/demander modifications).
- **Éditeurs** : attribution de DOI, publication, gestion des utilisateurs et des catégories.

---

### Stack technique

| Composant | Choix |
|-----------|-------|
| Backend | **Django** (Python) + **Django REST Framework (DRF)** |
| Base de données | **PostgreSQL** (recherche plein texte, robustesse) |
| Frontend | HTML/CSS/JS (Django templates, aucune SPA lourde) – design responsive, couleurs mates |
| Stockage de fichiers | Système de fichiers local ou cloud (PDF, avatars) |
| Authentification | Sessions Django + JWT (via `djangorestframework-simplejwt` pour l’API) |
| API | DRF – endpoints publics (lecture seule) et privés (soumission, évaluation) |
| Hébergement | VPS (OVH, Scaleway, DigitalOcean) avec Gunicorn + Nginx + PostgreSQL |

---

### Architecture des applications Django

| App | Rôle |
|-----|------|
| `core` | Modèles transversaux : `TypeDocument` (recherche, littérature, cinéma), `Discipline` (histoire, géographie, etc.), fonctions utilitaires. |
| `accounts` | Gestion des utilisateurs, profils, rôles (auteur, réviseur, éditeur), authentification (login/logout/register), réinitialisation de mot de passe. |
| `publication` | Cœur du métier : modèle `Publication` (titre, résumé, fichier PDF, DOI, statut, auteur, date de soumission/publication, mots-clés), vues de liste et de détail, formulaires de soumission. |
| `review` | Processus d’évaluation : modèle `Review` (lien vers Publication, réviseur, commentaire, décision, date). Workflow de changement d’état de la publication (brouillon → en relecture → accepté/refusé). |
| `search` | Recherche avancée et filtres (utilisation du moteur de recherche plein texte de PostgreSQL ou intégration de `django‑haystack`/Elasticsearch). |
| `api` | Endpoints DRF : sérialiseurs pour `Publication`, `Review`, `User`. Authentification JWT pour les actions protégées. Documentation OpenAPI via `drf‑spectacular`. |

---

### Workflow de publication (cycle de vie d’un article)

1. **Soumission** : un auteur connecté remplit un formulaire (titre, résumé, PDF, mots‑clés, type, discipline). L’article est créé en statut `brouillon`.
2. **Envoi en relecture** : l’auteur ou l’éditeur change le statut en `en_attente`.
3. **Évaluation** : un réviseur assigné lit l’article et peut :
   - `accepter` → statut `accepte`
   - `refuser` → statut `refuse`
   - `demander_modifications` → statut `modification_demandee` (l’auteur peut soumettre une nouvelle version)
4. **Publication** : un éditeur attribue un DOI (généré automatiquement via une API Crossref ou factice) et passe le statut à `public`. L’article devient visible sur le site.

---

### Modèle de données principal (simplifié)

```python
class Publication(models.Model):
    STATUS_CHOICES = (...)
    title = models.CharField(max_length=500)
    abstract = models.TextField()
    pdf = models.FileField(upload_to='publications/')
    doi = models.CharField(max_length=100, unique=True, blank=True)
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    type_doc = models.ForeignKey(TypeDocument, on_delete=models.PROTECT)   # recherche, littérature, cinéma
    disciplines = models.ManyToManyField(Discipline)
    keywords = models.CharField(max_length=500)
    submission_date = models.DateTimeField(auto_now_add=True)
    publication_date = models.DateTimeField(null=True, blank=True)
    status = models.CharField(max_length=30, choices=STATUS_CHOICES, default='draft')
```

---

### API publique (DRF)

Tous les endpoints sont sous `/api/v1/`. Quelques exemples :

| Endpoint | Méthode | Accès | Description |
|----------|---------|-------|-------------|
| `/api/v1/publications/` | GET | public | Liste paginée des publications (statut `public`) avec filtres (type, discipline, année) |
| `/api/v1/publications/{id}/` | GET | public | Détail complet d’une publication (métadonnées + résumé) |
| `/api/v1/publications/{id}/download/` | GET | public | Téléchargement du PDF |
| `/api/v1/soumettre/` | POST | auteur (JWT) | Soumission d’un nouvel article (multipart/form-data) |
| `/api/v1/evaluations/` | POST | réviseur (JWT) | Ajout d’une évaluation (commentaire, décision) |
| `/api/v1/mes-publications/` | GET | auteur (JWT) | Liste des publications de l’auteur connecté |

**Documentation** : `/api/schema/swagger-ui/` (via `drf-spectacular`).

---

### Authentification

- **Frontend** : sessions Django classiques (`LoginView`, `LogoutView`).
- **API** : JWT (`djangorestframework-simplejwt`). Un endpoint `/api/token/` fournit un *access* token et un *refresh* token.

---

### Exigences non‑fonctionnelles (pour les développeurs)

- **Responsive / Mobile‑first** : le HTML/CSS doit s’adapter aux téléphones, tablettes et desktop.
- **Accessibilité** : contraste suffisant, balises sémantiques, attributs `alt`.
- **Performance** : les requêtes sur la liste des publications doivent être paginées (défaut : 20 par page). Index sur `status`, `type_doc`, `discipline`.
- **Sécurité** : validation des fichiers uploadés (PDF), protection CSRF, `HttpOnly` cookies, `X-Frame-Options`, etc.
- **Tests** : couverture minimale des modèles, formulaires, vues critiques (soumission, évaluation). Utiliser `pytest-django`.

---

### Livrables attendus (pour une équipe de développement)

1. **Code source** : dépôt Git (GitHub ou GitLab) avec `requirements.txt`, `README.md` (installation, variables d’environnement, migrations).
2. **Base de données** : script de migration initiale et fixtures (exemples de types, disciplines).
3. **Fichiers statiques** : CSS compilé, JS, images.
4. **API** : documentation interactive (Swagger/ReDoc).
5. **Fichier de configuration** : exemple `.env` (SECRET_KEY, DB_URL, EMAIL_HOST, etc.).
6. **Procédure de déploiement** (optionnelle) : Dockerfile + docker‑compose, ou playbook Ansible.

---

### Environnement de développement (recommandé)

```bash
git clone https://github.com/memoire-ecrite/memoire-ecrite.git
cd memoire-ecrite
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # puis éditer
python manage.py migrate
python manage.py loaddata initial_data.json   # types, disciplines, superuser
python manage.py runserver
```

Le site est accessible sur `http://localhost:8000`.

---

### Points d’attention techniques

- **Stockage des PDF** : attention à la taille maximale (configurer `DATA_UPLOAD_MAX_NUMBER_FIELDS` et `MAX_UPLOAD_SIZE`). Utiliser `FileField` avec un répertoire par année/mois.
- **DOI** : en première version, on peut générer un DOI factice (ex. `10.5678/memoire.{année}.{id}`). L’intégration réelle avec Crossref nécessite un compte et une requête HTTPS.
- **Recherche** : commencer avec la recherche simple (`icontains`) sur `title`, `abstract`, `keywords`. Pour la v2, passer à PostgreSQL `SearchVector`.
- **Emails** : configurer un backend SMTP (SendGrid, Mailgun, ou console en développement).

---

### Conclusion pour l’équipe de développement

Mémoire Écrite est un projet Django classique mais complet : authentification complexes (rôles), workflow d’évaluation, API publique et stockage de fichiers. L’accent doit être mis sur la **clarté du code**, la **sécurité** et la **documentation** pour permettre à d’autres développeurs de reprendre le projet. Le frontend reste volontairement simple (Django templates) pour faciliter la maintenance.

Toute question relative à l’architecture ou aux choix techniques peut être discutée dans les issues du dépôt. Bon développement !