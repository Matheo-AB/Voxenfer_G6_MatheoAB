# PROJET Architecture "micro-services" - **VOXENFER-G6 (Modération)**

Ce dépôt contient le code source Python, le fichier `Dockerfile`, le fichier `requirements.txt` ainsi que les documents et ressources requis pour le micro-service de Modération (G6) du projet Voxenfer.

## 👥 Auteurs

- 👑 Mathéo ALBOUY-BENALIA
- Robin RIBAUTE
- Arthur GAUSSOT

## 👨‍🏫 Professeur

- Philippe Roussille

---

## Objectifs du service

Le service **G6 - Modération** gère la sécurité et la discipline sur le serveur Luanti. Il est responsable de :
- Recueillir et lister les **signalements** effectués par les joueurs.
- Gérer les **bannissements** (ajout, suppression, consultation par le jeu).
- Maintenir une **file d'actions** (bannissements, confiscation d'objets, etc.) que le mod Luanti vient interroger et exécuter en jeu.

Ce service possède sa propre base de données SQLite gérée via l'ORM SQLAlchemy et vérifie les jetons JWT émis par le service-comptes.

---
## Comment lancer le service

### En développement local (Hors Gateway)

- Installer les dépendances : `pip install -r requirements.txt`
- Lancer le serveur Flask : `python app.py`
- Le service écoute par défaut sur le port 5000.

### En intégration (Via Docker Compose et Gateway Caddy)

- L'équipe G1 doit intégrer ce service dans le `docker-compose.yml`.
- Le service sera joignable uniquement via la gateway de G1 sur le port 8080.
- L'URL de base publique sera `http://localhost:8080/moderation/`.

---

## Exemples d'appels (curl)

**Note importante :** Tous les appels doivent passer par la gateway G1 sur le port 8080. La gateway retire le préfixe `/moderation` avant de transmettre la requête à notre service.

### 1. Créer un signalement pour un **joueur**.
```bash
curl -X POST http://localhost:8080/moderation/signalements \
  -H "Authorization: Bearer [...]" \
  -H "Content-Type: application/json" \
  -d '{"pseudo": "CanardGraffeur", "raison": "Dégradation"}'
```

### 2. Effectuer un bannissement pour un **joueur**.
```bash
curl -X POST http://localhost:8080/moderation/bannis pseudo motif duree \
  -H "Authorization: Bearer [...]" \
  -H "Content-Type: application/json" \
  -d '{"pseudo": "CanardVoleur", "motif": "Vol Avéré", "duree": 3j}'
```

### 3. Appel en boucle par le mod Luanti.
```bash
curl -X GET http://localhost:8080/moderation/bannis
```

## 📂 Organisation du Dépôt 📂

```text
service-moderation/
└───── app.py
   ├── auth.py
   ├── db.py
   ├── Dockerfile
   └── requierements.txt

Documents/
└───── 1-sujet.md
   ├── 2-contrats.md
   ├── group.md
   └── README.md
```