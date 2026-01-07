# Configuration Synthflow/Fine-tuner - Secrétaire IA Dentiste

## Étape 1 : Déployer sur Railway

1. Créer un compte sur [Railway](https://railway.app)
2. Connecter votre GitHub
3. Créer un nouveau projet depuis ce repo
4. Railway déploiera automatiquement
5. Récupérer l'URL (ex: `https://votre-app.up.railway.app`)

## Étape 2 : Configurer les Custom Actions dans Synthflow

Dans votre dashboard Synthflow/Fine-tuner, créez les actions suivantes :

---

## Actions principales (gestion RDV)

### Action 1 : Rechercher un patient

**Nom de l'action:** `rechercher_patient`

**Description pour l'IA:**
> Utilise cette action pour rechercher un patient existant dans le système. Tu peux chercher par nom, prénom, date de naissance ou téléphone. Utilise-la quand le patient dit qu'il est déjà venu au cabinet.

**Configuration API:**
- **Méthode:** POST
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/rechercher_patient`
- **Headers:**
  - `Content-Type: application/json`
  - `X-Office-Code: VOTRE_OFFICE_CODE` (remplacer par le vrai code)
  - `X-Api-Key: VOTRE_API_KEY` (si nécessaire)

**Body (JSON):**
```json
{
  "nom": "<nom>",
  "prenom": "<prenom>",
  "date_naissance": "<date_naissance>",
  "telephone": "<telephone>"
}
```

**Variables à créer:**
| Variable | Description | Exemple |
|----------|-------------|---------|
| nom | Nom de famille du patient | "DUPONT" |
| prenom | Prénom du patient | "Marie" |
| date_naissance | Date de naissance format YYYY-MM-DD | "1985-03-15" |
| telephone | Numéro de téléphone mobile | "0612345678" |

---

### Action 2 : Consulter les disponibilités

**Nom de l'action:** `consulter_disponibilites`

**Description pour l'IA:**
> Utilise cette action pour voir les créneaux disponibles. Demande d'abord au patient quel type de rendez-vous il souhaite et à partir de quelle date.

**Configuration API:**
- **Méthode:** POST
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/consulter_disponibilites`
- **Headers:**
  - `Content-Type: application/json`
  - `X-Office-Code: VOTRE_OFFICE_CODE`
  - `X-Api-Key: VOTRE_API_KEY`

**Body (JSON):**
```json
{
  "type_rdv": "<type_rdv>",
  "date_debut": "<date_debut>",
  "date_fin": "<date_fin>",
  "nouveau_patient": <nouveau_patient>,
  "patient_id": "<patient_id>"
}
```

**Variables à créer:**
| Variable | Description | Exemple |
|----------|-------------|---------|
| type_rdv | Code du type de RDV | "CONSULTATION" |
| date_debut | Date de début de recherche YYYY-MM-DD | "2025-01-20" |
| date_fin | Date de fin (optionnel, max 14 jours) | "2025-01-27" |
| nouveau_patient | true si nouveau patient, false sinon | true |
| patient_id | ID du patient si connu | "893" |

---

### Action 3 : Créer un rendez-vous

**Nom de l'action:** `creer_rdv`

**Description pour l'IA:**
> Utilise cette action pour réserver un créneau une fois que le patient a choisi une date et une heure. Tu dois avoir toutes les informations du patient : nom, prénom, téléphone, et optionnellement email et date de naissance.

**Configuration API:**
- **Méthode:** POST
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/creer_rdv`
- **Headers:**
  - `Content-Type: application/json`
  - `X-Office-Code: VOTRE_OFFICE_CODE`
  - `X-Api-Key: VOTRE_API_KEY`

**Body (JSON):**
```json
{
  "praticien_id": "<praticien_id>",
  "type_rdv": "<type_rdv>",
  "date": "<date>",
  "heure": "<heure>",
  "nom": "<nom>",
  "prenom": "<prenom>",
  "telephone": "<telephone>",
  "email": "<email>",
  "date_naissance": "<date_naissance>",
  "nouveau_patient": <nouveau_patient>,
  "patient_id": "<patient_id>",
  "message": "<message>"
}
```

**Variables à créer:**
| Variable | Description | Exemple |
|----------|-------------|---------|
| praticien_id | ID du praticien | "schedule_123" |
| type_rdv | Code du type de RDV | "CONSULTATION" |
| date | Date du RDV YYYY-MM-DD | "2025-01-22" |
| heure | Heure au format HHMM | "0930" |
| nom | Nom du patient | "DUPONT" |
| prenom | Prénom du patient | "Marie" |
| telephone | Téléphone mobile | "+33612345678" |
| email | Email du patient | "marie@email.com" |
| date_naissance | Date de naissance | "1985-03-15" |
| nouveau_patient | true/false | true |
| patient_id | ID si patient existant | "" |
| message | Message pour le praticien | "Douleur depuis 3 jours" |

---

### Action 4 : Voir les RDV d'un patient

**Nom de l'action:** `voir_rdv_patient`

**Description pour l'IA:**
> Utilise cette action pour afficher tous les rendez-vous d'un patient. Tu dois d'abord avoir recherché le patient pour obtenir son ID.

**Configuration API:**
- **Méthode:** POST
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/voir_rdv_patient`
- **Headers:**
  - `Content-Type: application/json`
  - `X-Office-Code: VOTRE_OFFICE_CODE`
  - `X-Api-Key: VOTRE_API_KEY`

**Body (JSON):**
```json
{
  "patient_id": "<patient_id>"
}
```

**Variables à créer:**
| Variable | Description | Exemple |
|----------|-------------|---------|
| patient_id | ID du patient obtenu via rechercher_patient | "893" |

---

### Action 5 : Annuler un RDV

**Nom de l'action:** `annuler_rdv`

**Description pour l'IA:**
> Utilise cette action pour annuler un rendez-vous existant. Tu dois d'abord récupérer la liste des RDV du patient pour obtenir l'ID du RDV à annuler.

**Configuration API:**
- **Méthode:** POST
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/annuler_rdv`
- **Headers:**
  - `Content-Type: application/json`
  - `X-Office-Code: VOTRE_OFFICE_CODE`
  - `X-Api-Key: VOTRE_API_KEY`

**Body (JSON):**
```json
{
  "rdv_id": "<rdv_id>"
}
```

**Variables à créer:**
| Variable | Description | Exemple |
|----------|-------------|---------|
| rdv_id | ID du RDV à annuler | "appt_456" |

---

## Actions informatives (connaissances du cabinet)

### Action 6 : Obtenir les horaires du cabinet

**Nom de l'action:** `info_horaires`

**Description pour l'IA:**
> Utilise cette action quand le patient demande les horaires d'ouverture du cabinet ou quand il veut savoir quand il peut venir pour un certain type de soin.

**Configuration API:**
- **Méthode:** GET
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/info/horaires`

**Pas de body ni de variables nécessaires.**

---

### Action 7 : Obtenir les types de RDV disponibles

**Nom de l'action:** `info_types_rdv`

**Description pour l'IA:**
> Utilise cette action pour connaître tous les types de rendez-vous proposés par le cabinet avec leurs durées et plages horaires. Utile quand le patient demande ce que le cabinet propose.

**Configuration API:**
- **Méthode:** GET
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/info/types_rdv`

**Pas de body ni de variables nécessaires.**

---

### Action 8 : Suggérer un type de RDV

**Nom de l'action:** `suggerer_type_rdv`

**Description pour l'IA:**
> Utilise cette action quand le patient décrit son problème (mal de dents, besoin de nettoyage, etc.) mais ne sait pas quel type de RDV prendre. L'action analysera le motif et suggérera le type de RDV approprié.

**Configuration API:**
- **Méthode:** POST
- **URL:** `https://VOTRE-URL-RAILWAY.up.railway.app/info/suggerer_type_rdv?motif=<motif>`

**Variables à créer:**
| Variable | Description | Exemple |
|----------|-------------|---------|
| motif | Description du problème par le patient | "j'ai mal à une dent depuis 2 jours" |

---

## Étape 3 : Configurer le prompt système de l'agent

Voici un exemple de prompt système pour votre secrétaire IA :

```
Tu es Sophie, secrétaire virtuelle du cabinet dentaire [NOM DU CABINET].

Ton rôle :
- Accueillir chaleureusement les patients
- Les aider à prendre, modifier ou annuler des rendez-vous
- Répondre aux questions sur les horaires et types de consultations

Comportement :
- Sois professionnelle mais chaleureuse
- Pose les questions une par une, ne submerge pas le patient
- Confirme toujours les informations avant de créer un RDV
- Si tu ne peux pas aider, propose de laisser un message pour le cabinet

Flux typique pour un nouveau RDV :
1. Demande si le patient est déjà venu au cabinet
2. Si oui, recherche-le avec nom + prénom + téléphone
3. Demande le motif de consultation (utilise suggerer_type_rdv si besoin)
4. Propose les créneaux disponibles
5. Confirme les informations et crée le RDV
6. Rappelle au patient qu'il recevra une confirmation

Types de RDV disponibles (avec durées) :
- URGENCE : Urgence dentaire (20 min)
- CONSULTATION : Consultation générale (20 min)
- BILAN : Bilan complet CDC/Esthétique/Ortho/Paro (60 min)
- DETARTRAGE : Détartrage et maintenance (40 min)
- PROPHYLAXIE : Séance de prophylaxie (45 min)
- ECLAIRCISSEMENT : Blanchiment dentaire (80 min)
- EXTRACTION : Extraction dentaire (40 min)
- IMPLANT_GREFFE : Implant ou greffe (45 min)
- PROTHESES : Prothèses dentaires (60 min)
- COLLAGE_FACETTES : Facettes et inlays (30 min)
- INVISALIGN_1ER_RDV : Premier RDV Invisalign (40 min)
- INVISALIGN_TRAITEMENT : Suivi Invisalign (40 min)

Horaires du cabinet :
- Lundi : 09h30-19h30
- Mardi : 09h30-19h30
- Mercredi : FERMÉ
- Jeudi : 09h30-19h30
- Vendredi : 09h30-19h30
- Samedi : 09h00-15h00
- Dimanche : FERMÉ

Note : Certains types de RDV ne sont disponibles que sur certaines plages horaires.
Utilise l'action info_horaires pour plus de détails.
```

---

## Étape 4 : Endpoints utilitaires (pour tests)

### Lister les praticiens
```
GET https://VOTRE-URL-RAILWAY.up.railway.app/praticiens
Headers: X-Office-Code: VOTRE_CODE
```

### Lister les types de RDV (depuis l'API rdvdentiste)
```
GET https://VOTRE-URL-RAILWAY.up.railway.app/types_rdv
Headers: X-Office-Code: VOTRE_CODE
```

### Infos types RDV cabinet (local)
```
GET https://VOTRE-URL-RAILWAY.up.railway.app/info/types_rdv
```

### Infos horaires cabinet (local)
```
GET https://VOTRE-URL-RAILWAY.up.railway.app/info/horaires
```

### Catégories de RDV
```
GET https://VOTRE-URL-RAILWAY.up.railway.app/info/categories
```

---

## Test avec l'environnement de test

Pour tester, utilisez :
- **Office Code:** `07142393B9A2E58PDEPW`
- **Pas d'API Key requise**

Patients de test disponibles :
| Nom | Prénom | Téléphone | ID |
|-----|--------|-----------|-----|
| TEST | - | 3607080910 | 893 |
| ESSAI | Amélie | 0607080911 | 894 |

Page de test : https://rdvdentiste.net/jardin-sur-erdre-test/pierre-et-clement.html

---

## Référentiel des types de RDV

| Code | Nom | Durée | Jours disponibles |
|------|-----|-------|-------------------|
| URGENCE | Urgence | 20 min | Lun/Mar/Jeu/Ven/Sam |
| CONSULTATION | Consultation | 20 min | Lun/Mar/Jeu/Ven/Sam |
| BILAN | Bilan CDC/Esthétique/Ortho/Paro | 60 min | Lun/Mar/Jeu/Ven/Sam |
| DETARTRAGE | Détartrage et Maintenance | 40 min | Lun/Mar/Ven/Sam |
| PROPHYLAXIE | Séance de Prophylaxie | 45 min | Lun/Mar/Ven/Sam |
| LITHOTRITIE | Lithotritie | 40 min | Lun/Mar/Ven |
| ECLAIRCISSEMENT | Éclaircissement Fauteuil | 80 min | Lun/Mar/Jeu/Ven |
| COLLAGE_FACETTES | Collage Facettes/Inlay/Pose | 30 min | Lun/Mar/Jeu/Ven |
| SOINS_CONSERVATEURS | Soins Conservateurs Composites | 30 min | Lun/Mar/Jeu/Ven |
| PROTHESES | Prothèses | 60 min | Lun/Mar/Jeu/Ven |
| INLAY_IRM | Inlay IRM Empreinte Optique | 40 min | Lun/Mar/Jeu/Ven |
| EVALUATION_PHOTO | Évaluation/Photo/Essayage | 30 min | Lun/Mar/Jeu/Ven |
| EXTRACTION | Extraction/Résection Apicale | 40 min | Lun/Mar/Jeu/Ven/Sam |
| IMPLANT_GREFFE | Implant/Greffe | 45 min | Lun/Mar/Jeu/Ven/Sam |
| RESECTION_APICALE | Résection Apicale | 30 min | Lun/Mar/Jeu/Ven/Sam |
| INVISALIGN_1ER_RDV | Invisalign 1er RDV | 40 min | Lun/Mar/Jeu/Ven |
| INVISALIGN_TRAITEMENT | Invisalign Traitement | 40 min | Lun/Mar/Jeu/Ven |
| EMP_OPTIQUE_CONTENTION | Empreinte Optique Contention | 15 min | Lun/Mar/Jeu/Ven |
| FIN_INVISALIGN | Fin Invisalign | 60 min | Lun/Mar/Jeu/Ven |
