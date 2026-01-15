# Mon Projet Symfony 8

## 📋 Prérequis

- PHP 8.4 ou supérieur
- Composer
- Docker et Docker Compose
- Git

## 🚀 Installation

### 1. Cloner le projet

```bash
git clone [URL_DU_PROJET]
cd mon-projet
```

### 2. Installer les dépendances

```bash
composer install
```

### 3. Configuration de l'environnement

Créez votre fichier de configuration local :

```bash
cp .env .env.local
```

Éditez `.env.local` avec vos paramètres :

```env
# PostgreSQL Docker
POSTGRES_DB=app
POSTGRES_USER=app
POSTGRES_PASSWORD=!ChangeMe!

# Connexion base de données avec Docker
DATABASE_URL="postgresql://app:!ChangeMe!@database:5432/app?serverVersion=16&charset=utf8"

# Mailpit pour les emails de développement
MAILER_DSN=smtp://mailer:1025
```

### 4. Démarrer Docker

```bash
# Démarrer les services (PostgreSQL + Mailpit)
docker compose up -d

# Vérifier que les services sont actifs
docker compose ps
```

### 5. Créer la base de données

```bash
# Créer la base de données
php bin/console doctrine:database:create

# Exécuter les migrations (si elles existent)
php bin/console doctrine:migrations:migrate
```

### 6. Démarrer le serveur Symfony

```bash
symfony server:start
```

Ou avec PHP :

```bash
php -S localhost:8000 -t public/
```

## 🔧 Services Docker

### PostgreSQL
- **Host** : `database` (dans Docker) ou `localhost` (depuis votre machine)
- **Port** : `5432`
- **Base de données** : `app`
- **Utilisateur** : `app`
- **Mot de passe** : `!ChangeMe!`

### Mailpit (intercepteur d'emails)
- **Interface web** : http://localhost:8025
- **SMTP** : `mailer:1025` (depuis l'application)

## 📦 Commandes utiles

### Docker

```bash
# Démarrer les services
docker compose up -d

# Arrêter les services
docker compose down

# Voir les logs
docker compose logs -f

# Réinitialiser complètement (supprime les données)
docker compose down -v

# Redémarrer un service spécifique
docker compose restart database
```

### Base de données

```bash
# Créer la base de données
php bin/console doctrine:database:create

# Supprimer la base de données
php bin/console doctrine:database:drop --force

# Créer une migration
php bin/console make:migration

# Exécuter les migrations
php bin/console doctrine:migrations:migrate

# Vider le cache
php bin/console cache:clear
```

### Accès à PostgreSQL

**Depuis un client externe (DBeaver, pgAdmin, etc.) :**
- Host : `localhost`
- Port : `5432` (ou le port mappé par Docker)
- Database : `app`
- Username : `app`
- Password : `!ChangeMe!`

**Depuis le terminal :**

```bash
# Se connecter au container PostgreSQL
docker compose exec database psql -U app -d app

# Faire un backup
docker compose exec database pg_dump -U app app > backup.sql

# Restaurer un backup
docker compose exec -T database psql -U app app < backup.sql
```

## 🧪 Tests

```bash
# Lancer les tests
php bin/phpunit

# Tests avec couverture
php bin/phpunit --coverage-html var/coverage
```

## 🐛 Dépannage

### Problème : "Connection refused" à la base de données

**Solution :**
1. Vérifiez que Docker est démarré : `docker compose ps`
2. Vérifiez votre `DATABASE_URL` dans `.env.local` (utilisez `database` au lieu de `127.0.0.1`)
3. Attendez que PostgreSQL soit complètement démarré : `docker compose logs database`

### Problème : Le port 5432
