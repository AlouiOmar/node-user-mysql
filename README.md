# Suggestions API

API REST construite avec Express.js, MySQL et architecture MVC pour gérer des suggestions et des utilisateurs.

## 📋 Table des matières

- [Cloner le projet](#cloner-le-projet)
- [Configuration de la base de données](#configuration-de-la-base-de-données)
- [Installation](#installation)
- [Base de données](#base-de-données)
- [Démarrage](#démarrage)
- [API Endpoints](#api-endpoints)
  - [Suggestions](#suggestions)
  - [Utilisateurs](#utilisateurs)
- [Exemples de requêtes](#exemples-de-requêtes)
- [Gestion des erreurs](#gestion-des-erreurs)

## 📦 Cloner le projet

```bash
git clone https://github.com/AlouiOmar/node-user-mysql.git
cd node-user-mysql
```

## ⚙️ Configuration de la base de données

- **1. Démarrer MySQL avec XAMPP ou WAMP**  
  - Ouvrez **XAMPP** ou **WAMP**  
  - Démarrez **Apache**  
  - Démarrez **MySQL**

- **2. Créer la base de données avec phpMyAdmin**  
  - Ouvrez **phpMyAdmin** (`http://localhost/phpmyadmin`)  
  - Cliquez sur **Nouvelle base de données**  
  - Créez une base de données nommée : **`suggestions_db`**  
  - Aucune table n'est nécessaire : elles seront créées **automatiquement** au démarrage du projet.

- **3. Configurer la connexion dans le fichier `.env` à la racine du projet :**

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=votre_mot_de_passe
DB_NAME=suggestions_db
DB_PORT=3306
PORT=3000
NODE_ENV=development
```

## 🚀 Installation

```bash
# Installer les dépendances
npm install
```

## 🗄️ Base de données

### Création de la base de données

Si vous avez suivi la section **Configuration de la base de données**, la base `suggestions_db` existe déjà.
Les **tables seront créées automatiquement** au premier démarrage de l'application grâce aux modèles Sequelize/MySQL.

> Optionnel : vous pouvez également importer manuellement le fichier `database.sql` via phpMyAdmin ou la ligne de commande MySQL si vous souhaitez pré-remplir la base.

### Structure des tables

#### Table `suggestions`
- `id` (INT, AUTO_INCREMENT, PRIMARY KEY)
- `title` (VARCHAR(255), NOT NULL)
- `description` (TEXT)
- `category` (VARCHAR(100))
- `date` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP)
- `status` (VARCHAR(50), DEFAULT 'en attente')
- `nbLikes` (INT, DEFAULT 0)

#### Table `users`
- `id` (INT, AUTO_INCREMENT, PRIMARY KEY)
- `name` (VARCHAR(255), NOT NULL)
- `email` (VARCHAR(255), NOT NULL, UNIQUE)
- `role` (VARCHAR(50), DEFAULT 'user')
- `status` (VARCHAR(50), DEFAULT 'active')
- `created_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP)
- `updated_at` (TIMESTAMP, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP)

## ▶️ Démarrage

```bash
# Mode développement (avec nodemon)
npm run dev

# Mode production
npm start

# Production (version buildée)
npm run start:prod
```

Le serveur démarre sur `http://localhost:3000`

## 📡 API Endpoints

### Suggestions

#### GET `/suggestions`
Récupère toutes les suggestions.

**Réponse :**
```json
[
  {
    "id": 1,
    "title": "Nouvelle fonctionnalité",
    "description": "Description de la suggestion",
    "category": "feature",
    "date": "2024-01-15T10:30:00.000Z",
    "status": "en attente",
    "nbLikes": 5
  }
]
```

#### GET `/suggestions/:id`
Récupère une suggestion par son ID.

**Paramètres :**
- `id` (number) - ID de la suggestion

**Réponse :**
```json
{
  "success": true,
  "suggestion": {
    "id": 1,
    "title": "Nouvelle fonctionnalité",
    "description": "Description de la suggestion",
    "category": "feature",
    "date": "2024-01-15T10:30:00.000Z",
    "status": "en attente",
    "nbLikes": 5
  }
}
```

#### POST `/suggestions`
Crée une nouvelle suggestion.

**Body (JSON) :**
```json
{
  "title": "Nouvelle fonctionnalité",
  "description": "Description de la suggestion",
  "category": "feature",
  "status": "en attente"
}
```

**Champs requis :**
- `title` (string) - Titre de la suggestion

**Champs optionnels :**
- `description` (string) - Description de la suggestion
- `category` (string) - Catégorie de la suggestion
- `status` (string) - Statut (défaut: "en attente")

**Réponse :**
```json
{
  "success": true,
  "message": "Suggestion créée avec succès",
  "id": 1
}
```

#### PUT `/suggestions/:id`
Met à jour une suggestion existante.

**Paramètres :**
- `id` (number) - ID de la suggestion

**Body (JSON) :**
```json
{
  "title": "Titre mis à jour",
  "description": "Nouvelle description",
  "category": "bugfix",
  "status": "approuvée",
  "nbLikes": 10
}
```

**Champs requis :**
- `title` (string) - Titre de la suggestion

**Réponse :**
```json
{
  "success": true,
  "message": "Suggestion mise à jour avec succès"
}
```

#### DELETE `/suggestions/:id`
Supprime une suggestion.

**Paramètres :**
- `id` (number) - ID de la suggestion

**Réponse :**
```json
{
  "success": true,
  "message": "Suggestion supprimée avec succès"
}
```

#### POST `/suggestions/:id/like`
Incrémente le nombre de likes d'une suggestion.

**Paramètres :**
- `id` (number) - ID de la suggestion

**Réponse :**
```json
{
  "success": true,
  "message": "Like ajouté avec succès"
}
```

#### GET `/suggestions/category/:category`
Récupère les suggestions par catégorie.

**Paramètres :**
- `category` (string) - Catégorie à filtrer

**Réponse :**
```json
{
  "success": true,
  "count": 2,
  "suggestions": [
    {
      "id": 1,
      "title": "Suggestion 1",
      "category": "feature",
      ...
    }
  ]
}
```

#### GET `/suggestions/status/:status`
Récupère les suggestions par statut.

**Paramètres :**
- `status` (string) - Statut à filtrer

**Réponse :**
```json
{
  "success": true,
  "count": 3,
  "suggestions": [
    {
      "id": 1,
      "title": "Suggestion 1",
      "status": "en attente",
      ...
    }
  ]
}
```

---

### Utilisateurs

#### GET `/users`
Récupère tous les utilisateurs.

**Réponse :**
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user",
    "status": "active",
    "created_at": "2024-01-15T10:30:00.000Z",
    "updated_at": "2024-01-15T10:30:00.000Z"
  }
]
```

#### GET `/users/:id`
Récupère un utilisateur par son ID.

**Paramètres :**
- `id` (number) - ID de l'utilisateur

**Réponse :**
```json
{
  "success": true,
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user",
    "status": "active",
    "created_at": "2024-01-15T10:30:00.000Z",
    "updated_at": "2024-01-15T10:30:00.000Z"
  }
}
```

#### POST `/users`
Crée un nouvel utilisateur.

**Body (JSON) :**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "role": "user",
  "status": "active"
}
```

**Champs requis :**
- `name` (string) - Nom de l'utilisateur
- `email` (string) - Email de l'utilisateur (doit être unique)

**Champs optionnels :**
- `role` (string) - Rôle de l'utilisateur (défaut: "user")
- `status` (string) - Statut (défaut: "active")

**Réponse :**
```json
{
  "success": true,
  "message": "Utilisateur créé avec succès",
  "id": 1
}
```

#### PUT `/users/:id`
Met à jour un utilisateur existant.

**Paramètres :**
- `id` (number) - ID de l'utilisateur

**Body (JSON) :**
```json
{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "role": "admin",
  "status": "active"
}
```

**Champs requis :**
- `name` (string) - Nom de l'utilisateur
- `email` (string) - Email de l'utilisateur (doit être unique)

**Réponse :**
```json
{
  "success": true,
  "message": "Utilisateur mis à jour avec succès"
}
```

#### DELETE `/users/:id`
Supprime un utilisateur.

**Paramètres :**
- `id` (number) - ID de l'utilisateur

**Réponse :**
```json
{
  "success": true,
  "message": "Utilisateur supprimé avec succès"
}
```

#### GET `/users/role/:role`
Récupère les utilisateurs par rôle.

**Paramètres :**
- `role` (string) - Rôle à filtrer

**Réponse :**
```json
{
  "success": true,
  "count": 2,
  "users": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com",
      "role": "admin",
      ...
    }
  ]
}
```

#### GET `/users/status/:status`
Récupère les utilisateurs par statut.

**Paramètres :**
- `status` (string) - Statut à filtrer

**Réponse :**
```json
{
  "success": true,
  "count": 3,
  "users": [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com",
      "status": "active",
      ...
    }
  ]
}
```

---

## 💡 Exemples de requêtes

### Avec cURL

#### Créer une suggestion
```bash
curl -X POST http://localhost:3000/suggestions \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Nouvelle fonctionnalité",
    "description": "Description de la suggestion",
    "category": "feature"
  }'
```

#### Récupérer toutes les suggestions
```bash
curl http://localhost:3000/suggestions
```

#### Créer un utilisateur
```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user"
  }'
```

#### Mettre à jour un utilisateur
```bash
curl -X PUT http://localhost:3000/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Doe",
    "email": "jane@example.com",
    "role": "admin",
    "status": "active"
  }'
```

### Avec JavaScript (Fetch API)

```javascript
// Créer une suggestion
const response = await fetch('http://localhost:3000/suggestions', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    title: 'Nouvelle fonctionnalité',
    description: 'Description de la suggestion',
    category: 'feature'
  })
});

const data = await response.json();
console.log(data);
```

## ⚠️ Gestion des erreurs

L'API retourne des réponses d'erreur standardisées :

### Erreur 400 - Bad Request
```json
{
  "success": false,
  "error": "Le titre est requis"
}
```

### Erreur 404 - Not Found
```json
{
  "success": false,
  "error": "Suggestion non trouvée"
}
```

### Erreur 500 - Internal Server Error
```json
{
  "success": false,
  "error": "Erreur serveur interne"
}
```

### Erreur 404 - Route non trouvée
```json
{
  "success": false,
  "error": "Route non trouvée"
}
```

## 🛠️ Technologies utilisées

- **Node.js** - Runtime JavaScript
- **Express.js** - Framework web
- **MySQL2** - Driver MySQL avec support des promesses
- **CORS** - Middleware pour gérer les requêtes cross-origin
- **dotenv** - Gestion des variables d'environnement

## 📝 Notes

- Toutes les dates sont retournées au format ISO 8601
- Les emails doivent être uniques dans la table `users`
- Le champ `title` est requis pour les suggestions
- Les champs `name` et `email` sont requis pour les utilisateurs
- Les routes sont sensibles à la casse

