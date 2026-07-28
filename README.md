# Portfolio — Data Engineer

> Morgan Le Gall — parcours Data Engineer (OpenClassrooms, 2025/2026).
> Profil GitHub : https://github.com/MoyLiG

Data Engineer en formation, je conçois des pipelines de données reproductibles
(Docker, orchestration) et des systèmes NLP/RAG en production. Ce portfolio
récapitule 11 projets du parcours : de la modélisation SQL aux systèmes RAG
scalables, en passant par le streaming temps réel et l'infrastructure data.

## Récapitulatif des projets

| # | Projet | Domaine | Stack clé | Code |
|---|--------|---------|-----------|------|
| P13 | MVP RAG scalable | NLP / RAG / Cloud | pgvector, PostGIS, Redis, smolagents, Langfuse, FastAPI, Scaleway | [GitHub](https://github.com/MoyLiG/P13-mvp-rag-puls-events) |
| P12 | Projet d'infrastructure (Option B) | Data infra / Qualité | Kestra, PostgreSQL (RLS), Great Expectations, Google Maps, Slack | [GitHub](https://github.com/MoyLiG/P12_projet_infrastructure) |
| P11 | Système RAG (POC) | NLP / RAG | LangChain, Mistral, FAISS, Streamlit | [GitHub](https://github.com/MoyLiG/P11) |
| P10 | BottleNeck — ETL automatisé | Orchestration ETL | Kestra, DuckDB, Python | [GitHub](https://github.com/MoyLiG/p10-bottleneck-data-engineer) |
| P9 | InduTechData — pipeline temps réel | Streaming | Redpanda/Kafka, PySpark, MySQL, Docker | [GitHub](https://github.com/MoyLiG/P9-InduTechData) |
| P8 | Infrastructure de données (Forecast 2.0) | Data infra / ELT | Airbyte, dbt, PostgreSQL, AWS | [GitHub](https://github.com/MoyLiG/P8-forecast-data-infra) |
| P7 | Base de données NoSQL (NosCités) | NoSQL | MongoDB (replica set + sharding), Tableau | [GitHub](https://github.com/MoyLiG/P7-noscites-mongodb) |
| P6 | Prédiction conso. bâtiments (Seattle) | ML deployment | BentoML, scikit-learn, Pydantic, Docker | [GitHub](https://github.com/MoyLiG/P6-seattle-energy-bentoml) |
| P5 | Migration données médicales | NoSQL / DevOps | MongoDB, Docker, RBAC, CI/CD GitHub Actions | [GitHub](https://github.com/MoyLiG/OC_P5) |
| P4 | Audit d'un environnement de données | SQL / BI | SQL, Power BI, audit | _projet local_ |
| P3 | Conception & requêtage SQL | SQL / Modélisation | SQL, modélisation relationnelle (Power Architect) | _projet local_ |
| — | Fondamentaux | Python / outils | Python, Git, exercices | — |

## Compétences démontrées

| Catégorie | Compétences | Projets |
|-----------|-------------|---------|
| Langages | Python, SQL | P3, P4, P5, P6, P9, P10, P11, P12, P13 |
| Bases de données | Modélisation relationnelle, MongoDB (NoSQL), PostgreSQL/PostGIS, pgvector, MySQL, DuckDB, indexation, sharding, RLS | P3, P5, P7, P8, P9, P10, P12, P13 |
| Data Engineering / ETL | Orchestration (Kestra), ELT (dbt), ingestion (Airbyte), streaming (Kafka/PySpark), pipelines reproductibles | P8, P9, P10, P12 |
| DevOps / Cloud | Docker / Compose, CI/CD (GitHub Actions), déploiement cloud (AWS, Scaleway), API (FastAPI, BentoML) | P5, P6, P8, P12, P13 |
| ML / NLP | RAG, embeddings, LLM (Mistral), recherche sémantique, agents (smolagents), ML supervisé | P6, P11, P13 |
| Qualité / Tests | pytest, Great Expectations, tests dbt, assertions « golden values », monitoring (Langfuse) | P5, P8, P10, P12, P13 |
| Sécurité / Données sensibles | RBAC, RLS, pseudonymisation (SHA-256 salé), hachage bcrypt, audit trail, RGPD/souveraineté | P5, P12, P13 |

## Fiches projets

### P13 — MVP RAG scalable (Puls-Events)
> Faire évoluer le POC RAG P11 en MVP production scalable et souverain.

**Le problème** — Le POC P11 était stateless, mono-instance (FAISS local),
sans contexte géographique ni recherche temps réel ni monitoring. Puls-Events
veut un MVP qui tienne la charge et personnalise les réponses.

**Ce que j'ai construit** — Une architecture RAG production couvrant 4 défis :
mémoire conversationnelle (Redis court terme + PostgreSQL long terme), contexte
géographique (PostGIS, requêtes rayon/distance), recherche web temps réel
(smolagents + Tavily) et monitoring qualité/coût (Langfuse). Vector store migré
de FAISS vers pgvector (multi-instances). API FastAPI async avec streaming SSE.

**Stack & outils** — Mistral (`mistral-small`, `mistral-embed`), PostgreSQL +
pgvector + PostGIS, Redis, LangChain, smolagents + Tavily, Langfuse, FastAPI,
Kestra (ingestion Open Agenda), Docker, Scaleway (cloud souverain FR).

**Résultats** — Étude de design MVP complète : analyse des besoins, choix cloud
(souveraineté FR/RGPD), architecture détaillée, macro-backlog, estimation des
coûts (build + OPEX), plan de projet. Retriever hybride BM25 + dense hérité du
POC, benché.

**Compétences démontrées** — Architecture de systèmes RAG scalables, choix
cloud argumenté, mémoire conversationnelle, recherche géospatiale, agents LLM,
monitoring de production, gestion de projet data.

**Valeur ajoutée** — Transforme une preuve de faisabilité en produit déployable,
avec une trajectoire de coûts maîtrisée et une conformité RGPD by design.

**Code** — [github.com/MoyLiG/P13-mvp-rag-puls-events](https://github.com/MoyLiG/P13-mvp-rag-puls-events)

### P12 — Gérez un projet d'infrastructure (Option B, Sport Data Solution)
> Pipeline ETL calculant l'impact financier de deux avantages sportifs pour
> les 162 salariés d'une entreprise (fictive).

**Le problème** — Sport Data Solution veut tester la faisabilité d'une prime
sportive + jours bien-être. Données RH sensibles, besoin de traçabilité, de
qualité et d'une démo crédible face à un sponsor business.

**Ce que j'ai construit** — Un pipeline ETL orchestré, de la génération de
données (~3-5k activités style Strava) jusqu'aux marts analytiques, avec
qualité de données automatisée, enrichissement par services externes réels et
restitution PowerBI. Schémas PostgreSQL séparés `raw`/`staging`/`marts` +
`audit` + `cache` (pattern inspiré dbt).

**Stack & outils** — Kestra (flow YAML versionné, replay), PostgreSQL (rôles,
Row Level Security, audit log), Great Expectations,
Google Maps Distance Matrix API, Slack webhook, PowerBI, Docker, Faker.

**Résultats** — Pipeline reproductible (tout en conteneurs), qualité de données
validée par suites Great Expectations déclaratives, démo live fonctionnelle avec
services externes réels. Décisions techniques tracées dans un journal de bord.

**Compétences démontrées** — Orchestration Kestra, sécurité de données sensibles
(RLS, pseudonymisation des PII, audit), qualité de données, intégration d'API
externes, gestion de projet et arbitrage technique argumenté.

**Valeur ajoutée** — Une infra data conforme aux standards entreprise (sécurité,
audit, reproductibilité) sur un cas RH réaliste, défendable face à un sponsor.

**Code** — [github.com/MoyLiG/P12_projet_infrastructure](https://github.com/MoyLiG/P12_projet_infrastructure)

### P11 — Système RAG (POC Puls-Events)
> Démontrer la faisabilité d'un chatbot de recherche sémantique sur les
> événements culturels d'Open Agenda.

**Le problème** — Permettre à un utilisateur de poser en langage naturel des
questions du type « Quels concerts à Nantes ce mois-ci ? » sur les données
événementielles d'Open Agenda.

**Ce que j'ai construit** — Un POC RAG bout-en-bout : ingestion de l'API
Opendatasoft (pagination + retry), index vectoriel, chaîne de récupération +
génération, démo CLI et Streamlit. Périmètre Pays de la Loire, événements de
moins d'un an.

**Stack & outils** — LangChain (`create_retrieval_chain` + retriever MMR),
Mistral (`mistral-small-latest`, `mistral-embed` 1024 dim), FAISS (`IndexFlatL2`),
Streamlit. Stack imposée par le brief.

**Résultats** — Chatbot fonctionnel répondant en français sur le catalogue
Open Agenda (1,1 M+ enregistrements au catalogue source). Pipeline d'évaluation
du RAG livré. Validé par les équipes produit et marketing.

**Compétences démontrées** — RAG, embeddings, intégration LLM, ingestion d'API,
évaluation de système de récupération, prototypage rapide.

**Valeur ajoutée** — A débloqué le financement du MVP (P13) en prouvant la
valeur métier du chatbot.

**Code** — [github.com/MoyLiG/P11](https://github.com/MoyLiG/P11)

### P10 — BottleNeck : ETL automatisé
> Industrialiser une chaîne d'analyse de données vin (rapprochement ERP / site
> web, détection de vins premium).

**Le problème** — Automatiser et fiabiliser un traitement manuel : nettoyer 3
sources Excel, fusionner ERP et catalogue web, calculer le CA et isoler les
vins « premium » par analyse statistique.

**Ce que j'ai construit** — Un pipeline ETL orchestré et planifié, avec tests
d'assertion à chaque étape clé (« golden values »), alerte email en cas
d'échec, et un diagnostic des produits orphelins (présents en ERP, absents du
web).

**Stack & outils** — Kestra (flow YAML, trigger cron, alerte Gmail SMTP),
DuckDB (SQL de nettoyage/fusion/CA), Python (calcul de z-score), Excel/CSV en
sortie, Docker Compose.

**Résultats** — CA total vérifié à 70 568,60 € ; 30 vins premium (z-score > 2)
et 684 vins ordinaires ; 111 produits orphelins détectés (~40 540 € de stock
dormant). 5 blocs d'assertions garantissent les valeurs attendues à chaque
étape.

**Compétences démontrées** — Orchestration ETL, SQL analytique, tests de
pipeline (assertions, fail-fast), détection d'anomalies statistiques,
reproductibilité Docker.

**Valeur ajoutée** — Transforme un traitement manuel fragile en pipeline
testé, planifié et auto-surveillé, avec un diagnostic métier exploitable.

**Code** — [github.com/MoyLiG/p10-bottleneck-data-engineer](https://github.com/MoyLiG/p10-bottleneck-data-engineer)

### P9 — InduTechData : pipeline de données temps réel
> Traiter en continu un flux de tickets clients pour produire des insights
> temps réel.

**Le problème** — Ingérer un flux continu de tickets (générés toutes les 2 s),
les enrichir et les agréger en quasi temps réel pour le support client.

**Ce que j'ai construit** — Un pipeline streaming complet : un producteur Python
publie des tickets JSON dans un topic Kafka (3 partitions), PySpark Structured
Streaming les consomme en micro-batches (30 s), les enrichit (assignation
d'équipe, score de priorité) puis écrit en MySQL + exports Parquet partitionnés
et JSON.

**Stack & outils** — Redpanda (broker compatible Kafka), PySpark Structured
Streaming (`from_json`, `foreachBatch`), MySQL (JDBC), Parquet, Docker Compose.

**Résultats** — Pipeline temps réel fonctionnel : ingestion → streaming →
traitement → stockage multi-format, avec agrégations live (par type de demande,
priorité, équipe) affichées en console. Stockage en table `tickets` +
`ticket_stats`.

**Compétences démontrées** — Streaming temps réel (Kafka/Spark), Structured
Streaming, enrichissement à la volée, exports analytiques (Parquet partitionné),
orchestration conteneurisée.

**Valeur ajoutée** — Démontre la capacité à traiter de la donnée en mouvement
(et pas seulement en batch), brique clé d'une plateforme data moderne.

**Code** — [github.com/MoyLiG/P9-InduTechData](https://github.com/MoyLiG/P9-InduTechData)

### P8 — Construisez & testez une infrastructure de données (Forecast 2.0)
> Enrichir les modèles de prévision électrique de GreenAndCoop avec des données
> météo de 6 stations.

**Le problème** — Ingérer des sources météo hétérogènes (JSON + Excel),
les unifier et les modéliser proprement pour alimenter des modèles de prévision
de demande électrique.

**Ce que j'ai construit** — Une infrastructure ELT moderne : ingestion par
Airbyte (3 sources) vers PostgreSQL, transformations dbt en couches
staging → intermediate → marts (schéma en étoile), optimisations PostgreSQL
(index, partitionnement par mois), et tests de qualité.

**Stack & outils** — Airbyte (abctl), dbt Core (adaptateur postgres),
PostgreSQL, schéma en étoile (`dim_date`, `dim_weather_stations`,
`fct_weather_observations`), Docker, déploiement AWS.

**Résultats** — Modèle dimensionnel en étoile opérationnel (`dim_date` = 2184
lignes sur 6 ans, 6 stations), 38 tests dbt définis (génériques + 6 tests métier
sur plages de valeurs météo, unicité, non-nullité), partitionnement mensuel de
la table de faits.

**Compétences démontrées** — ELT moderne (ingestion + transformation),
modélisation dimensionnelle (star schema), optimisation PostgreSQL
(index/partitionnement), tests de qualité dbt, déploiement cloud.

**Valeur ajoutée** — Une couche de données météo fiable, testée et requêtable,
prête à alimenter les Data Scientists sans retraitement.

**Code** — [github.com/MoyLiG/P8-forecast-data-infra](https://github.com/MoyLiG/P8-forecast-data-infra)

### P7 — Concevez & analysez une base de données NoSQL (NosCités)
> Concevoir une base MongoDB distribuée pour des annonces de logement (Airbnb
> Paris + Lyon) et justifier le choix NoSQL.

**Le problème** — Stocker et interroger des annonces hétérogènes (schéma
variable, listes imbriquées comme `amenities`), avec une perspective de montée
en charge multi-villes, là où un schéma relationnel imposerait colonnes NULL ou
EAV et des migrations.

**Ce que j'ai construit** — Une infrastructure MongoDB distribuée : un replica
set (haute disponibilité) et un cluster shardé par ville (`city` comme shard
key, zones dédiées), avec routeur mongos et config servers. Documentation du
choix NoSQL argumentée et restitution Tableau.

**Stack & outils** — MongoDB (replica set `noscitesRS`, cluster shardé,
config servers, mongos), WSL2, Tableau, data dictionary.

**Résultats** — 105 858 annonces réparties par sharding : 95 885 docs Paris
(90,78 %) sur `shardParis` et 9 973 docs Lyon (9,21 %) sur `shardLyon`. Choix
NoSQL justifié sur 5 axes (schéma variable, imbrication native, évolutivité sans
migration, sharding natif, alignement modèle requête/application).

**Compétences démontrées** — Modélisation NoSQL, architecture distribuée
(replica set, sharding horizontal), arbitrage SQL vs NoSQL argumenté,
visualisation Tableau.

**Valeur ajoutée** — Une base prête à la montée en charge multi-villes sans
migration de schéma, avec un raisonnement d'architecture défendable.

**Code** — [github.com/MoyLiG/P7-noscites-mongodb](https://github.com/MoyLiG/P7-noscites-mongodb)

### P6 — Anticipez les besoins en consommation des bâtiments (Seattle)
> Déployer un modèle de prédiction de consommation énergétique des bâtiments
> comme une API.

**Le problème** — Industrialiser un modèle ML de prédiction de consommation
(données bâtiments de Seattle) pour le rendre interrogeable en production, avec
validation des entrées.

**Ce que j'ai construit** — Un service de prédiction packagé et conteneurisé :
endpoints `/predict` et `/model_info`, validation stricte des entrées,
preprocessing/feature engineering reproductible, et sauvegarde du modèle dans
un model store.

**Stack & outils** — BentoML (service + Bento/Docker), scikit-learn, Pydantic
(schémas `BuildingInput`/`PredictionOutput`), pandas/numpy, Swagger UI.

**Résultats** — API ML fonctionnelle (Swagger sur `:3000`), modèle versionné
dans le model store BentoML, validation Pydantic des requêtes, tests d'API
(validation + intégration). Notebook d'entraînement + présentation livrés.

**Compétences démontrées** — Déploiement de modèles ML (MLOps), conception
d'API, validation de schémas, feature engineering, conteneurisation.

**Valeur ajoutée** — Fait passer un modèle du notebook à un service interrogeable
et validé, étape indispensable de la mise en production ML.

**Code** — [github.com/MoyLiG/P6-seattle-energy-bentoml](https://github.com/MoyLiG/P6-seattle-energy-bentoml)

### P5 — Migration de données médicales vers MongoDB
> Migrer 55 500 dossiers médicaux d'un CSV vers MongoDB, de façon sécurisée et
> reproductible.

**Le problème** — Passer un dataset médical sensible (55 500 dossiers) d'un
fichier plat vers une base NoSQL performante, sécurisée et conteneurisée, avec
garantie d'intégrité.

**Ce que j'ai construit** — Une migration complète CSV → MongoDB avec
optimisation par index, sécurité RBAC double niveau, validation automatique
d'intégrité CSV ↔ MongoDB, le tout conteneurisé et couvert par une pipeline
CI/CD.

**Stack & outils** — MongoDB, Python, Docker, bcrypt (12 rounds), GitHub Actions
(CI/CD), pytest.

**Résultats** — 55 500 documents migrés ; 5 index MongoDB (requêtes nettement
accélérées) ; RBAC à double niveau (MongoDB + application) ; 28 tests
automatisés (24 unitaires + 4 d'intégration) ; pipeline CI/CD opérationnelle.

**Compétences démontrées** — Migration de données, modélisation NoSQL et
indexation, sécurité (RBAC, hachage bcrypt), conteneurisation Docker, CI/CD,
tests automatisés, intégrité de données.

**Valeur ajoutée** — Une migration auditable et rejouable, conforme aux
exigences de sécurité d'un domaine sensible (santé).

**Code** — [github.com/MoyLiG/OC_P5](https://github.com/MoyLiG/OC_P5)

### P4 — Audit d'un environnement de données
> Auditer un environnement de données (logs, base relationnelle) et restituer
> les constats en BI.

**Le problème** — Évaluer la qualité et la structure d'un environnement de
données existant (ventes, employés, clients, produits) et produire un rapport
d'audit exploitable.

**Ce que j'ai construit** — Un audit structuré : modélisation de la base
(tables `calendrier`, `employes`, `clients`, `produits`, ventes), requêtes SQL
d'analyse, et restitution visuelle Power BI, synthétisés dans un rapport
d'audit.

**Stack & outils** — SQL (SQLite), Power BI (`.pbix`), modélisation de schéma,
rapport d'audit (DOCX).

**Résultats** — Rapport d'audit livré, modèle de données documenté et tableau de
bord Power BI de restitution.

**Compétences démontrées** — Audit de données, SQL analytique, modélisation
relationnelle, dataviz/BI (Power BI), restitution écrite.

**Valeur ajoutée** — Transforme un environnement opaque en constats chiffrés et
visuels, base d'une prise de décision.

**Code** — _projet local (livrables OpenClassrooms)_

### P3 — Conception & requêtage d'une base de données SQL
> Modéliser une base relationnelle de transactions immobilières et l'interroger.

**Le problème** — Concevoir un schéma relationnel propre (régions, communes,
biens, ventes) à partir de données immobilières, et répondre à des questions
métier par requêtes SQL.

**Ce que j'ai construit** — Un modèle relationnel normalisé avec clés étrangères
(`Region` → `Commune` → `Bien`/`Vente`), conçu sous Power Architect, puis une
batterie de requêtes analytiques sur les ventes.

**Stack & outils** — SQL (SQLite, `PRAGMA foreign_keys`), Power Architect
(modélisation MCD/MLD), données immobilières publiques.

**Résultats** — Schéma relationnel avec intégrité référentielle ; requêtes
d'analyse livrées (ex. appartements vendus au 1er semestre 2020, ventes par
région, répartition par nombre de pièces avec pourcentages).

**Compétences démontrées** — Modélisation relationnelle (normalisation, clés
étrangères), SQL analytique (jointures, agrégations, sous-requêtes).

**Valeur ajoutée** — Pose les fondations de la rigueur en modélisation de
données, socle réutilisé dans tous les projets SQL ultérieurs.

**Code** — _projet local (livrables OpenClassrooms)_

## Fondamentaux

En amont des projets d'ingénierie, le parcours a démarré par une phase de
fondamentaux : découverte du métier de Data Engineer (P1) et montée en
compétence Python via une série d'exercices (P2 — manipulation de données,
structures, algorithmique). Ces bases outillent tous les projets suivants.

---

*Parcours Data Engineer — OpenClassrooms, 2025/2026. Morgan Le Gall.*
