# Transverse


Guide d’entretien technique
DBA SQL Server & DevOps | Questions, réponses attendues, cas pratique et grille d’évaluation
Objectif : évaluer les compétences réelles en administration SQL Server, diagnostic de production, automatisation, CI/CD et sécurité. Durée cible : 75 minutes. Profil : confirmé à senior.
1. Déroulement recommandé
Durée
Domaine
Objectif
10 min
Parcours
Autonomie et réalisations
20 min
SQL Server
Administration et disponibilité
15 min
Performance
Diagnostic et incidents
15 min
DevOps
Git, CI/CD, IaC
10 min
Cas pratique
Architecture et raisonnement
5 min
Échange
Questions et synthèse

2. Administration SQL Server
Q1. Index clustered et nonclustered : différence ?
Réponse attendue : Clustered : structure qui organise les lignes selon la clé, un seul par table rowstore. Nonclustered : structure séparée référençant les lignes via clé de clustering ou RID.
Signal senior : INCLUDE, key lookup, impact en écriture.
Q2. Comment diagnostiquer une requête lente ?
Réponse attendue : Collecter contexte, plan réel, Query Store, lectures logiques, CPU, durée, waits, statistiques, index et blocages ; comparer les plans avant/après.
Signal senior : Différencie plan, contention, I/O et paramétrage.
Q3. Blocking versus deadlock ?
Réponse attendue : Blocking : attente d’un verrou détenu par une autre session. Deadlock : cycle de dépendances ; SQL Server choisit une victime, erreur 1205.
Signal senior : Extended Events, xml_deadlock_report, transactions courtes et retry.
Q4. À quoi sert tempdb ? Comment l’optimiser ?
Réponse attendue : Objets temporaires, tris/hash, worktables et version store selon les fonctions. Analyser contention PAGELATCH, volumes et fichiers ; préallouer et ajuster le nombre de fichiers.
Signal senior : Ne recommande pas systématiquement huit fichiers.
Q5. Comment fonctionne Always On Availability Groups ?
Réponse attendue : Réplicas et bases du groupe de disponibilité, modes synchrone/asynchrone ; bascule automatique sous conditions de cluster et synchronisation ; les AG ne remplacent pas les backups.
Signal senior : Listener, quorum, RPO/RTO et différences d’édition.
Q6. FULL, SIMPLE et BULK_LOGGED ?
Réponse attendue : FULL : sauvegardes de journaux et restauration ponctuelle si chaîne valide ; SIMPLE : pas de sauvegardes de journaux ; BULK_LOGGED : journalisation minimale possible et limitations de point-in-time.
Signal senior : Chaîne de sauvegardes et test de restauration.
Q7. Comment préparer une montée de version ?
Réponse attendue : Compatibilité, pilotes, éditions, dépendances, tests applicatifs, sauvegardes/restaurations, plan de migration, bascule et retour arrière.
Signal senior : In-place versus side-by-side ; rollback réaliste.
Q8. Comment sécuriser SQL Server ?
Réponse attendue : Moindre privilège, comptes dédiés, TLS, TDE selon besoin, audit, correctifs, coffre de secrets, segmentation et accès contrôlés.
Signal senior : Limites TDE, certificats et récupération des clés.
Performance et incidents
Q9. Requête passée de 2 secondes à 3 minutes : démarche ?
Réponse attendue : Qualifier la portée, comparer les plans Query Store, vérifier statistiques/cardinalité, parameter sniffing, waits, CPU, I/O et blocages ; mesurer avant/après.
Signal senior : Ne crée pas un index sans preuve.
Q10. PAGEIOLATCH versus PAGELATCH ?
Réponse attendue : PAGEIOLATCH : attente de chargement d’une page depuis le stockage ; PAGELATCH : contention sur page déjà en mémoire.
Signal senior : Sait que PAGELATCH ne signifie pas stockage lent.
Q11. CPU SQL Server à 95 % : diagnostic ?
Réponse attendue : Identifier consommateurs, distinguer CPU instance et système, analyser plans, compilations, parallélisme et waits ; valider cause avant ajout de vCPU.
Signal senior : Utilise mesures et chronologie d’incident.
Q12. Comment benchmarker une baie SAN ?
Réponse attendue : Baseline avant/après ; mêmes profils de charge et configurations ; mesurer IOPS, latence, débit, profondeur de file, lecture/écriture avec DiskSpd puis corréler aux métriques SQL.
Signal senior : Distingue test synthétique et charge applicative.
DevOps et automatisation
Q13. Que signifie DevOps pour SQL Server ?
Réponse attendue : Versionnement, collaboration, tests, automatisation, CI/CD, observabilité, sécurité et changements traçables.
Signal senior : Évoque migrations de données et contraintes de production.
Q14. Différence CI et CD ?
Réponse attendue : CI : intégration, compilation et tests continus. Continuous Delivery : déploiement prêt après validation. Continuous Deployment : mise en production automatique.
Signal senior : Sait positionner gates et approbations.
Q15. Comment versionner le schéma SQL ?
Réponse attendue : Git, projet SQL/DACPAC ou migrations versionnées Flyway/Liquibase ; revues et tests.
Signal senior : Gère changements incompatibles et migrations de données.
Q16. Comment déployer un changement SQL automatiquement ?
Réponse attendue : Pull request, validation, build DACPAC ou migrations, tests recette, revue du plan, approbation, déploiement, vérification.
Signal senior : SqlPackage, contrôle du destructif et secrets.
Q17. Comment automatiser l’installation SQL Server ?
Réponse attendue : PowerShell et installation silencieuse ; Ansible/DSC selon contexte ; comptes, stockage, patchs, sauvegardes et supervision.
Signal senior : Idempotence, redémarrages et post-checks.
Q18. Terraform/OpenTofu versus Ansible ?
Réponse attendue : Terraform/OpenTofu provisionne les ressources et gère leur état ; Ansible configure les systèmes et exécute des tâches.
Signal senior : State, providers, idempotence et drift.
Q19. Comment protéger les secrets dans un pipeline ?
Réponse attendue : Vault/OpenBao ou coffre cloud, identité de service à privilèges minimaux, secrets injectés à l’exécution, jamais dans Git/logs.
Signal senior : Rotation et authentification sans secret statique.
Q20. Que faire si un déploiement SQL échoue ?
Réponse attendue : Arrêter, journaliser, qualifier les modifications, contrôler l’état, appliquer procédure testée de correction ou récupération ; parfois roll-forward préférable.
Signal senior : Comprend les limites d’un rollback transactionnel.
3. Cas pratique : provisioning VMware → SQL Server
Scénario à soumettre : une équipe demande une VM Windows Server sur VMware, SQL Server installé, une base créée, des comptes techniques configurés, des sauvegardes et une supervision opérationnelles. Comment industrialiser le processus de bout en bout ?
1. Formulaire ou portail : valider environnement, sizing, version, conformité et approbations.
2. Git : versionner IaC, configuration et migrations ; revue par pull request.
3. Orchestration : GitLab CI, Azure DevOps, Jenkins ou orchestrateur adapté.
4. OpenTofu/Terraform : provisionner VM, réseau et stockage via vCenter.
5. PowerShell/Ansible : préparer Windows, installer et configurer SQL Server.
6. Coffre de secrets : récupérer identifiants et certificats au runtime.
7. SQL : créer base, logins, rôles et permissions minimales.
8. Exploitation : sauvegardes, supervision, alertes, durcissement et tests de restauration.
9. Vérifications : tests de connexion, configuration, sauvegarde, sécurité et livraison traçable.
Question piège : le pipeline échoue après création de la VM mais avant installation SQL Server. Que faire ?
Attendu : reprise idempotente, contrôle de l’état réel et du state IaC, absence de doublons, journalisation, reprise ciblée ou compensation maîtrisée ; ne pas détruire aveuglément la VM.
4. Signaux d’alerte
Redémarre SQL Server sans diagnostic.
Crée un index automatiquement dès qu’une requête ralentit.
Stocke les mots de passe dans les scripts ou Git.
Confond Terraform/OpenTofu et Ansible.
Promet un rollback automatique pour toute migration de données.
N’a jamais testé de restauration de sauvegarde.
5. Grille de notation
Critère
Poids
Note /5
Points pondérés
Administration SQL Server
25 %
____
____
Performance et troubleshooting
20 %
____
____
HA et sauvegardes
15 %
____
____
DevOps / CI/CD / Git
20 %
____
____
Automatisation / IaC
15 %
____
____
Sécurité et communication
5 %
____
____

Total : ______ / 100. Calcul : somme des (note / 5 × poids). Seuil indicatif : 70/100 ; à adapter aux exigences du poste.
Échelle : 0 aucune connaissance ; 1 notions ; 2 pratique assistée ; 3 autonome ; 4 confirmé en production ; 5 expert et référent.
Candidat : __________________  Date : ______________  Évaluateur : __________________
Forces : __________________________________________________________________________
Risques / points à vérifier : __________________________________________________________
Décision : ☐ Favorable  ☐ À approfondir  ☐ Défavorable
6. Conseils à l’évaluateur
Demander pour chaque sujet une réalisation concrète : contexte, rôle exact, outils, difficultés, décisions, preuves et résultats. Prioriser le diagnostic de performance, la chaîne CI/CD de base et le provisionnement automatisé. Ajuster la pondération selon que le poste est principalement DBA ou principalement DevOps.
