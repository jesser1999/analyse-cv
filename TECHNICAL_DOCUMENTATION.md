# Documentation technique - CV Analyzer

## Architecture du projet

CV Analyzer est une application web développée avec Next.js, utilisant une architecture moderne et des technologies de pointe pour offrir une expérience utilisateur optimale et des fonctionnalités avancées d'analyse de CV.

### Structure des dossiers

```
cv-analyzer/
├── migrations/           # Scripts SQL pour la base de données
├── public/               # Fichiers statiques
├── src/
│   ├── app/              # Pages et routes de l'application
│   │   ├── api/          # Routes API
│   │   ├── auth/         # Pages d'authentification
│   │   ├── dashboard/    # Pages du tableau de bord
│   ├── components/       # Composants réutilisables
│   │   ├── auth/         # Composants d'authentification
│   │   ├── ui/           # Composants d'interface utilisateur
│   ├── hooks/            # Hooks React personnalisés
│   ├── lib/              # Bibliothèques et utilitaires
│   │   ├── cv-analyzer.ts # Système d'analyse de CV
│   │   ├── cv-scorer.ts   # Algorithme de notation
│   │   ├── auth.utils.ts  # Utilitaires d'authentification
├── tests/                # Tests unitaires
├── wrangler.toml         # Configuration Cloudflare
```

## Technologies utilisées

### Frontend
- **Next.js**: Framework React pour le rendu côté serveur et la génération de sites statiques
- **Tailwind CSS**: Framework CSS utilitaire pour le design
- **React**: Bibliothèque JavaScript pour construire l'interface utilisateur

### Backend
- **Next.js API Routes**: API serverless pour les fonctionnalités backend
- **NextAuth.js**: Solution d'authentification complète
- **D1 Database**: Base de données SQL légère de Cloudflare

### Analyse de CV
- **pdf-parse**: Extraction de texte à partir de fichiers PDF
- **mammoth**: Extraction de texte à partir de fichiers DOCX
- **natural**: Traitement du langage naturel pour l'analyse de texte
- **compromise**: Bibliothèque NLP pour l'extraction d'entités

## Fonctionnalités principales

### Système d'authentification
Le système d'authentification utilise NextAuth.js pour offrir plusieurs méthodes de connexion :
- Email/mot de passe
- Google OAuth
- GitHub OAuth

### Téléchargement et stockage de CV
- Validation des formats de fichiers (PDF, DOCX)
- Limitation de la taille des fichiers (5 Mo max)
- Stockage sécurisé des fichiers

### Analyse de CV
Le système d'analyse de CV est composé de deux modules principaux :
1. **CVAnalyzer**: Extrait le texte des CV et analyse le contenu pour identifier :
   - Formation et éducation
   - Expérience professionnelle
   - Compétences techniques
   - Langues
   - Projets personnels

2. **CVScorer**: Évalue le CV selon une échelle internationale avec les pondérations suivantes :
   - Formation et éducation : 20%
   - Expérience professionnelle : 30%
   - Compétences techniques : 25%
   - Langues : 10%
   - Projets personnels : 15%

## Base de données

### Schéma de la base de données

```sql
-- Table des utilisateurs
CREATE TABLE users (
  id TEXT PRIMARY KEY,
  name TEXT,
  email TEXT UNIQUE,
  password TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Table des CV
CREATE TABLE cvs (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id TEXT,
  file_name TEXT,
  file_path TEXT,
  file_type TEXT,
  file_size INTEGER,
  upload_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Table des analyses
CREATE TABLE analysis (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  cv_id INTEGER,
  total_score INTEGER,
  analysis_data TEXT,
  analysis_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (cv_id) REFERENCES cvs(id)
);

-- Table des catégories d'analyse
CREATE TABLE analysis_categories (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT,
  description TEXT
);

-- Table des résultats d'analyse par catégorie
CREATE TABLE analysis_results (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  analysis_id INTEGER,
  category_id INTEGER,
  score INTEGER,
  FOREIGN KEY (analysis_id) REFERENCES analysis(id),
  FOREIGN KEY (category_id) REFERENCES analysis_categories(id)
);
```

## API

### Routes API principales

#### Authentification
- `POST /api/auth/register`: Inscription d'un nouvel utilisateur
- `GET /api/auth/[...nextauth]`: Routes NextAuth.js pour l'authentification

#### Gestion des CV
- `POST /api/cv/upload`: Téléchargement d'un nouveau CV
- `GET /api/cv/list`: Liste des CV de l'utilisateur connecté

#### Analyse de CV
- `GET /api/cv/analyze/[id]`: Analyse d'un CV spécifique
- `GET /api/cv/score/[id]`: Notation d'un CV analysé

## Déploiement

L'application est déployée sur Cloudflare Pages, offrant :
- Performances élevées grâce au réseau CDN mondial
- Mise à l'échelle automatique
- Certificats SSL automatiques
- Intégration avec Cloudflare D1 pour la base de données

## Maintenance et évolution

### Maintenance
- Mettre à jour régulièrement les dépendances
- Surveiller les journaux d'erreurs
- Effectuer des sauvegardes régulières de la base de données

### Évolutions possibles
- Ajout de nouveaux formats de CV supportés
- Amélioration de l'algorithme d'analyse avec l'IA
- Intégration avec des plateformes de recrutement
- Fonctionnalités de comparaison entre plusieurs CV
- Suggestions d'amélioration personnalisées pour les CV
