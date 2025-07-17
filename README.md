
# 🛡️ Simulation de fuite de clé AWS avec GitGuardian Honeytoken

Ce projet démontre comment simuler une fuite d'un secret AWS à l'aide d'un honeytoken GitGuardian placé dans un dépôt GitHub public.

## 🎯 Objectif

- Comprendre comment les bots réagissent aux fuites de secrets
- Tester GitGuardian en condition réelle
- Former les équipes à la gestion de secrets

## 🧪 Étapes principales

1. Création d'un honeytoken sur GitGuardian
2. Insertion du token dans un fichier `.env` crédible
3. Publication dans un dépôt public GitHub
4. Observation des alertes générées (100+ en moins de 30 minutes)


## 🧠 Auteur

Antonio Ferreira – Spécialiste sécurité IAM & Cloud

🔗 [linkedin.com/in/antonio-ferreira](https://www.linkedin.com/in/antoniofos/)

---

⚠️ Ce projet est uniquement à des fins de démonstration éducative.

📄 Télécharger la documentation PDF

👉 [Télécharger le guide complet en PDF](docs/Documentation_honeytoken-guide.pdf)

## 📸 Captures d'écran (Étapes clés)

Voici les principales étapes illustrées avec des captures réelles du test.

### Étape 1 – Créer un compte GitGuardian
1.	Rendez-vous sur https://dashboard.gitguardian.com
2.	Créez un compte gratuit ou connectez-vous avec votre compte GitHub.
 
![Étape 1](screenshots/1.png)

3.	Une fois connecté, GitGuardian commencera automatiquement à surveiller vos dépôts publics si l'intégration GitHub est activée.
![Étape 2](screenshots/2.png)

### Étape 2 - Créer un honeytoken
1.	Cliquez sur Honeytokens dans le menu latéral.
2.	Cliquez sur "Create Honeytoken".

![Étape 3](screenshots/3.png)

3.	Donnez un nom et une description à votre token (ex. : aws-dev-key-test).
4.	Cliquez sur “Create”.

![Étape 4](screenshots/4.png)

5. GitGuardian générera une fausse clé AWS sous forme de :
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXEMPLE
AWS_SECRET_ACCESS_KEY=gg-honey-xxxxxxxxxxxxxxxxxxxxxxxx

![Étape 5](screenshots/5.png)

### Étape 3 – Injecter le honeytoken dans un dépôt GitHub
1.	Créez un vrai dépôt public sur GitHub (ou utilisez un existant).
Exemple : https://github.com/iamsecuritec/azure-ad-demo-public
2.	Créez un fichier .env 

![Étape 6](screenshots/6.png)

3.	Collez le contenu du honeytoken en le rendant crédible :
4.	Faites un commit :
Astuce : ajoutez un commentaire dans le fichier pour tromper les bots, comme “clé temporaire pour test S3”.

![Étape 7](screenshots/7.png)

### Étape 4 – Surveillance par GitGuardian
Dès que le dépôt est mis à jour, GitGuardian commence à le surveiller.
Si un acteur externe tente d'utiliser la clé, une alerte immédiate sera déclenchée avec :
•	IP de l’attaquant
•	Pays d’origine
•	Outil utilisé (user-agent)
•	Appel AWS effectué

![Étape 8](screenshots/8.png)

### Vue d'ensemble des événements
![Étape 9](screenshots/9.png)

### Tentatives de privilèges
![Étape 10](screenshots/10.png)

### Étape 11 – Révocation du token
![Étape 11](screenshots/11.png)

### Étape 12 – Clôture et bilan du test
![Étape 12](screenshots/12.png)
