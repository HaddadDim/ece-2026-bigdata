# Lab 2 - Object storage with S3

## Erreurs rencontrées
- InvalidAccessKeyId lors des commandes aws s3 : les credentials temporaires avaient expiré. Solution : recoller le script S3 Profile Details dans le terminal, ou redémarrer le service.

## Questions du Job Kubernetes

**Pourquoi les credentials sont dans un Secret et pas dans la ConfigMap ?**
Une ConfigMap est stockée en clair et destinée à la configuration non sensible.
Un Secret est un objet distinct dont l'accès peut être restreint par RBAC et
qui peut être chiffré au repos. Il évite d'exposer les clés dans les manifests.

**Que se passe-t-il si le Job est relancé demain ?**
Les credentials d'Onyxia sont temporaires : le Job échoue avec une erreur
ExpiredToken. En production, on utilise une identité liée au pod (service
account avec rôle IAM) ou un gestionnaire de secrets (Vault, External Secrets)
avec rotation automatique, et des droits limités au strict nécessaire.

**Comment en faire une ingestion quotidienne ?**
Avec un CronJob Kubernetes (par exemple schedule "0 2 * * *") qui crée un Job
chaque jour, ou avec un orchestrateur comme Airflow.
