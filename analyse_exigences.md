# Analyse des exigences et technologies pour le projet d'analyse de CV

## Exigences fonctionnelles

### Interface utilisateur
- Interface moderne, professionnelle et innovante
- Design responsive pour différents appareils
- Navigation intuitive et expérience utilisateur fluide

### Système d'authentification
- Inscription et connexion des utilisateurs
- Plusieurs méthodes d'authentification
- Stockage sécurisé des informations d'identification
- Gestion des sessions utilisateur

### Gestion des CV
- Téléchargement de CV dans différents formats
- Stockage sécurisé des documents
- Visualisation des CV téléchargés
- Historique des téléchargements

### Analyse des CV
- Extraction du contenu textuel des CV
- Analyse basée sur des critères professionnels prédéfinis
- Classification des éléments importants
- Attribution d'un score selon une échelle internationale
- Présentation visuelle des résultats d'analyse

## Technologies recommandées

### Framework principal
- **Next.js** : Framework React avec rendu côté serveur, optimisé pour le SEO et les performances
- Avantages : Structure robuste, routage intégré, support API, déploiement simplifié

### Base de données
- **Cloudflare D1** : Base de données SQL intégrée à Cloudflare Workers
- Alternative : **Prisma** avec une base de données PostgreSQL pour une solution plus traditionnelle

### Authentification
- **NextAuth.js** : Solution d'authentification complète pour Next.js
- Support pour l'authentification par email/mot de passe
- Intégration avec des fournisseurs OAuth (Google, GitHub, etc.)
- Gestion des sessions et des JWT

### Stockage de fichiers
- **Cloudflare R2** : Service de stockage compatible S3
- Alternative : **AWS S3** pour le stockage des CV

### Analyse de texte et CV
- **pdf.js** : Extraction de texte à partir de PDF
- **mammoth.js** : Conversion de documents DOCX en HTML/texte
- **natural** : Bibliothèque NLP pour l'analyse de texte
- **compromise** : Analyse syntaxique et extraction d'informations

### Interface utilisateur
- **Tailwind CSS** : Framework CSS utilitaire pour un design personnalisé
- **shadcn/ui** : Composants UI réutilisables basés sur Radix UI
- **Lucide** : Bibliothèque d'icônes
- **Recharts** : Visualisation des données et résultats d'analyse

## Structure de la base de données

### Table Users
- id (clé primaire)
- email (unique)
- password_hash
- name
- created_at
- updated_at

### Table CVs
- id (clé primaire)
- user_id (clé étrangère)
- file_name
- file_path
- file_type
- upload_date
- file_size

### Table Analysis
- id (clé primaire)
- cv_id (clé étrangère)
- analysis_date
- total_score
- analysis_data (JSON)

### Table AnalysisCategories
- id (clé primaire)
- name
- description
- weight

### Table AnalysisResults
- id (clé primaire)
- analysis_id (clé étrangère)
- category_id (clé étrangère)
- score
- extracted_content
- comments

## Critères d'analyse des CV

1. **Formation et éducation** (poids : 20%)
   - Diplômes obtenus
   - Établissements fréquentés
   - Formations complémentaires

2. **Expérience professionnelle** (poids : 30%)
   - Postes occupés
   - Durée des expériences
   - Responsabilités
   - Réalisations

3. **Compétences techniques** (poids : 25%)
   - Langages de programmation
   - Outils et technologies
   - Certifications

4. **Langues** (poids : 10%)
   - Langues maîtrisées
   - Niveau de compétence

5. **Projets personnels et contributions** (poids : 15%)
   - Projets open source
   - Publications
   - Conférences

## Échelle de notation

- 0-20 : Insuffisant
- 21-40 : Passable
- 41-60 : Moyen
- 61-80 : Bon
- 81-100 : Excellent

Cette échelle sera appliquée à chaque catégorie, puis pondérée pour obtenir un score global.
