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








**********



Entretien DBA SQL Server & DevOps
Guide recruteur • Questions, réponses attendues et notes • 60 minutes
Candidat : ___________________________________   Date : _______________
Évaluateur technique : _________________________   DSI : ________________
Mode d’emploi
Le référent DBA conduit les questions techniques ; le DSI évalue l’autonomie, la communication et les risques. Laisser le candidat expliquer son raisonnement avant de consulter la réponse. Demander systématiquement : « Qu’avez-vous personnellement réalisé ? »
Notation : 0 = aucune réponse ; 1 = notions ; 2 = partiel/assisté ; 3 = autonome ; 4 = solide en production ; 5 = expert avec exemples vérifiables. Noter les preuves, pas seulement le vocabulaire.
Déroulé conseillé
0–10 min : présentation et expériences réelles (DSI + DBA)
10–25 min : SQL Server, performance et production (DBA)
25–40 min : DevOps, Git, pipelines et automatisation (DBA)
40–50 min : cas pratique transversal (DBA)
50–60 min : autonomie, questions du candidat et conclusion (DSI)
01 · Parcours
Question : Décrivez votre dernière mission : quel environnement SQL Server et quelles automatisations avez-vous personnellement réalisés ?
Réponse attendue : Attendu : contexte, taille/criticité, versions, rôle exact, résultats chiffrés, incidents ou livraisons concrets.
Relance : Relance : « Quel livrable avez-vous créé vous-même ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
02 · SQL Server
Question : Une application est soudainement lente. Comment diagnostiquez-vous ?
Réponse attendue : Attendu : impact et changements récents ; CPU, mémoire, I/O, blocages, waits ; Query Store et plans d’exécution ; hypothèse, action mesurée et validation.
Relance : Relance : « Si le plan a changé, comment le prouvez-vous ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
03 · SQL Server
Question : Quelle différence entre PAGEIOLATCH et PAGELATCH ?
Réponse attendue : Attendu : PAGEIOLATCH = attente de lecture de page depuis le stockage ; PAGELATCH = contention sur page en mémoire. Le diagnostic et les remèdes diffèrent.
Relance : Relance : « Que regarderiez-vous sur tempdb ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
04 · SQL Server
Question : Une base est indisponible : quelles sont vos premières actions ?
Réponse attendue : Attendu : qualifier l’impact, sécuriser et communiquer, consulter erreurs/état instance, stockage, réseau, disponibilité, RPO/RTO ; restaurer ou basculer selon architecture ; valider applicativement.
Relance : Relance : « Comment prouvez-vous que la restauration est exploitable ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
05 · SQL Server
Question : Comment concevez-vous une stratégie de sauvegarde et de restauration ?
Réponse attendue : Attendu : FULL/DIFF/LOG selon RPO et modèle de récupération, rétention, chiffrement, stockage séparé, surveillance, tests réguliers de restauration et documentation.
Relance : Relance : « Une sauvegarde réussie garantit-elle la restauration ? » (Non.)
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
06 · DevOps
Question : Expliquez simplement Git, CI et CD dans le contexte d’une base SQL Server.
Réponse attendue : Attendu : Git versionne ; CI vérifie/intègre ; CD prépare ou réalise le déploiement ; scripts ou DACPAC/migrations testés et traçables. Continuous delivery ≠ continuous deployment.
Relance : Relance : « Qui valide le passage en production ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
07 · DevOps
Question : Décrivez un pipeline SQL Server que vous avez réellement construit.
Réponse attendue : Attendu : déclencheur Git, tests, validation du changement, environnement cible, approbation, déploiement, logs, vérification et gestion des échecs ; outils précis et responsabilité personnelle.
Relance : Relance : « Montrez-moi mentalement ses étapes de bout en bout. »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
08 · DevOps
Question : Comment automatisez-vous une installation SQL Server sur Windows ?
Réponse attendue : Attendu : installation silencieuse PowerShell/Ansible/DSC, prérequis, comptes de service, disques, tempdb, patching, configuration, supervision et tests post-installation.
Relance : Relance : « Comment obtenez-vous 20 serveurs cohérents ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
09 · DevOps
Question : Différence entre Terraform/OpenTofu et Ansible ?
Réponse attendue : Attendu : Terraform/OpenTofu provisionne et suit l’état des ressources ; Ansible configure et orchestre des tâches ; rôles complémentaires, pas exclusifs.
Relance : Relance : « Que se passe-t-il si une VM existe déjà ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
10 · DevOps
Question : Comment gérez-vous les secrets dans un pipeline ?
Réponse attendue : Attendu : jamais de mot de passe en clair dans Git ; coffre de secrets, identité à privilèges minimaux, injection à l’exécution, masquage des logs et rotation.
Relance : Relance : « Comment faites-vous tourner le mot de passe ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
11 · DevOps
Question : Le pipeline échoue après trois scripts SQL sur cinq : que faites-vous ?
Réponse attendue : Attendu : arrêt, journalisation, état réel de la base, migrations traçables, idempotence si applicable, récupération ou correction en avant ; pas de promesse de rollback automatique universel.
Relance : Relance : « Comment évitez-vous de rejouer un script destructif ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
12 · Cas pratique
Question : Nous voulons provisionner une VM VMware, installer SQL Server, créer une base et activer sauvegardes et supervision. Dessinez votre solution.
Réponse attendue : Attendu : demande standardisée → Git/pipeline → Terraform/OpenTofu avec vCenter → PowerShell/Ansible → scripts SQL → coffre de secrets → tests, sauvegardes, supervision et restitution ; gestion de l’état, droits et erreurs.
Relance : Relance : « Si le pipeline tombe après la création de la VM, comment reprenez-vous sans doublon ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
13 · DSI
Question : Vous arrivez dans une équipe où tout est manuel. Que faites-vous les 90 premiers jours ?
Réponse attendue : Attendu : inventaire, cartographie des risques, priorisation, standards, petit POC, indicateurs, documentation, accompagnement des équipes et déploiement progressif.
Relance : Relance : « Quel premier résultat livrable au bout d’un mois ? »
Note :  0  /  1  /  2  /  3  /  4  /  5       Preuves / remarques : __________________________________________
_________________________________________________________________________________
Grille de synthèse
Domaine
Poids
Note /5
SQL Server / production
35 %
____ / 5
DevOps / CI-CD
25 %
____ / 5
Automatisation / IaC
20 %
____ / 5
Raisonnement / gestion des risques
10 %
____ / 5
Communication / autonomie
10 %
____ / 5

Score pondéré : ______ / 100      Avis : ☐ Favorable  ☐ Réservé  ☐ Défavorable
Forces : __________________________________________________________________________
Risques / points à vérifier : __________________________________________________________
Recommandation au DSI : __________________________________________________________
Aide-mémoire pour l’évaluateur non spécialiste DevOps
Git : historique des scripts et revue des modifications.
Pipeline CI/CD : suite d’étapes de validation et de déploiement automatisées.
Terraform/OpenTofu : provisionnement d’infrastructure et gestion de son état.
Ansible / PowerShell : configuration et automatisation des systèmes.
Idempotence : relancer une opération sans effets indésirables ni doublons.
Drapeaux rouges : secrets dans Git, absence de tests, aucun exemple concret, rollback supposé toujours automatique.
Important : cette grille mesure l’expérience déclarée et le raisonnement. Pour un poste très DevOps, compléter par un exercice pratique revu par une personne compétente dans les outils utilisés.

