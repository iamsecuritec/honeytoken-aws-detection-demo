
Documentation : Créer un véritable honeypot avec GitGuardian et GitHub
Objectif
Simuler une fuite de clé AWS dans un dépôt GitHub public en utilisant un honeytoken GitGuardian, afin d’observer les réactions des bots et attaquants automatisés.


 
Étape 1 : Créer un compte GitGuardian
1.	Rendez-vous sur https://dashboard.gitguardian.com
2.	Créez un compte gratuit ou connectez-vous avec votre compte GitHub.
 












3.	Une fois connecté, GitGuardian commencera automatiquement à surveiller vos dépôts publics si l'intégration GitHub est activée.
 

Étape 2 : Créer un honeytoken
1.	Cliquez sur Honeytokens dans le menu latéral.
2.	Cliquez sur "Create Honeytoken".
 
3.	Donnez un nom et une description à votre token (ex. : aws-dev-key-test).
4.	Cliquez sur “Create”.
 












GitGuardian générera une fausse clé AWS sous forme de :
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXEMPLE
AWS_SECRET_ACCESS_KEY=gg-honey-xxxxxxxxxxxxxxxxxxxxxxxx
 










Étape 3 : Injecter le honeytoken dans un dépôt GitHub
1.	Créez un vrai dépôt public sur GitHub (ou utilisez un existant).
Exemple : https://github.com/iamsecuritec/azure-ad-demo-public
2.	Créez un fichier .env 
 









3.	Collez le contenu du honeytoken en le rendant crédible :
4.	Faites un commit :
Astuce : ajoutez un commentaire dans le fichier pour tromper les bots, comme “clé temporaire pour test S3”.
 













Étape 4 : Surveillance par GitGuardian
Dès que le dépôt est mis à jour, GitGuardian commence à le surveiller.
Si un acteur externe tente d'utiliser la clé, une alerte immédiate sera déclenchée avec :
•	IP de l’attaquant
•	Pays d’origine
•	Outil utilisé (user-agent)
•	Appel AWS effectué
 







 
 



Exemple de résultats
En moins de 21 minutes, plus de 100 événements ont été déclenchés.
 
Exemples d'attaques :
Action	Nom de l’action	Signification
GetCallerIdentity	Obtenir l'identité de l'appelant	Vérifie si la clé AWS est valide et identifie l'utilisateur IAM ou le compte AWS associé
ListBackupPlans	Lister les plans de sauvegarde	Recherche les plans dans AWS Backup, pour cibler des données à restaurer ou supprimer
ListSecrets	Lister les secrets	Tente de découvrir les domaines OpenSearch/Elasticsearch disponibles dans le compte AWS

ListDomainNames	Lister les noms de domaine	Enumère tous les utilisateurs du compte AWS pour préparer une escalade de privilèges
ListServiceQuotas	Lister les quotas de service	Permet de voir les limites d’utilisation pour chaque service AWS (utile pour cartographier l’environnement)
ListBuckets	Lister les buckets S3	Enumère tous les buckets de stockage S3, souvent pour préparer l’exfiltration de données
DescribeOrganization	Décrire l'organisation AWS	Fournit des infos sur l'organisation AWS (si AWS Organizations est activé), y compris le compte root
AttachUserPolicy	Attacher une politique à un user	Tente d’accorder plus de privilèges à un utilisateur IAM en attachant une politique
ListUsers	Lister les utilisateurs IAM	Enumère tous les utilisateurs du compte AWS pour préparer une escalade de privilèges
		











Étape 5 : Clôturer le test
Révoquer le honeytoken :
Dans GitGuardian, sélectionnez le token → cliquez sur Revoke
 









Nettoyer le dépôt :
•	Supprimez le fichier. env ou remplacez le contenu par un place Holder
•	Optionnel : réécrire l’historique Git si nécessaire
  

 







Conclusion
Ce test démontre qu’un secret exposé même quelques minutes peut être :
•	Vu
•	Testé
•	Exploité
par des acteurs automatisés du monde entier.
Utiliser des honeytokens est une excellente stratégie pour :
•	Surveiller des fuites
•	Former vos équipes
•	Tester vos alertes sécurité




