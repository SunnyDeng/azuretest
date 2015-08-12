<properties
   pageTitle="Availability checklist | Microsoft Azure"
   description="Checklist that provides guidance for availability concerns during design."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="masimms"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2015"
   ms.author="masashin"/>

# Liste de vérification de disponibilité

![](media/best-practices-availability-checklist/pnp-logo.png)

## Conception d'applications

- **Éviter tout point de défaillance unique.** Tous les composants, services, ressources et instances de calcul doivent être déployés en tant qu'instances multiples d'éviter un point de défaillance unique aux disponibilité. Cela comprend des mécanismes d'authentification. Concevoir l'application à être configuré pour utiliser plusieurs instances, automatiquement détecter les pannes et rediriger les requêtes vers les instances non pas où la plate-forme ne fait pas automatiquement.
- **Décomposer la charge de travail par différent SLA.** Si un service est composé de charge critique et moins critiques, gérer différemment et spécifiez les fonctionnalités du service et le nombre d'instances pour répondre à leurs exigences de disponibilité.
- **Réduire au minimum et comprendre les dépendances des services.** Minimiser le nombre de différents services utilisés lorsque c'est possible et s'assurer que vous comprenez toutes les dépendances et le service qui existent dans le système. Cela comprend la nature de ces dépendances et l'impact de l'échec ou réduit les performances dans chacun d'eux sur la demande globale. Microsoft garantit au moins 99,9 % de disponibilité pour la plupart des services, mais cela signifie que chaque service supplémentaire, une application s'appuie sur potentiellement réduit la disponibilité globale SLA de votre système de 0,1 %.
- **Tâches de conception et les messages à être idempotente si possible** pour qu'une copie des demandes ne causeront pas de problèmes. Par exemple, un service peut agir comme un consommateur qui gère les messages envoyés en tant que demandes par d'autres parties du système qui agissent comme des producteurs. Si le consommateur échoue après traitement du message, mais avant de reconnaître qu'il a été traité, un producteur peut soumettre une demande de répétition qui pourrait être traitée par une autre instance du consommateur. Pour cette raison, les consommateurs et les opérations qu'ils accomplissent devraient être idempotent afin que répéter une opération exécutée précédemment ne rend pas les résultats non valides. Cela peut signifier détectant dupliqué messages ou assurer la cohérence en utilisant une approche optimiste à la gestion des conflits.
- **Utiliser un agent de messages qui implémente une disponibilité élevée pour les opérations critiques.** De nombreux scénarios pour lancer des tâches ou pour accéder aux services à distance utilisent la messagerie pour transmettre des instructions entre l'application et le service cible. Pour des performances optimales, l'application doit pouvoir envoyer le message, puis retourner pour traiter plus de demandes sans avoir à attendre une réponse. Afin de garantir la livraison des messages, le système de messagerie doit fournir une haute disponibilité. Mettre en place des files d'attente de Bus Service _au moins une fois_ sémantique, auquel cas chaque message envoyé à une file d'attente ne seront pas perdue, bien que des copies peuvent être livrés dans certaines circonstances. Si le traitement des messages est idempotente (voir le point précédent) ensuite répété de livraison ne doit pas être un problème.
- **Applications de conception à gracieusement dégrader** when reaching resource limits, and take appropriate action to minimize the impact for the user. In some cases, the load on the application may exceed the capacity of one or more parts, causing reduced availability and failed connections. Scaling can help to alleviate this, but it may reach a limit imposed by factors such as resource availability or cost. Design the application so that, in this situation, it can automatically degrade gracefully. For example, in an ecommerce system, if the order-processing subsystem is under strain (or has even failed completely), that part of the system can be temporarily disabled while allowing other functionality (such as browsing the product catalog) to continue. It might be appropriate to postpone requests to a failing subsystem, for example still enabling customers to submit orders but saving them to a queue or other safe storage mechanism for later processing when the orders subsystem is available again.
- **Gérer correctement les événements de rafale rapide.** La plupart des applications ont besoin de gérer des charges de travail variables au fil du temps, tels que pics première chose le matin dans une application métier ou lorsqu'un nouveau produit est sorti en un site de commerce électronique. Mise à l'échelle automatique peut aider à gérer la charge, mais il peut prendre un certain temps pour les instances supplémentaires sont en ligne et de gérer les demandes. Empêcher rafales soudaines et inattendues de l'activité d'écrasantes de l'application de conception de requêtes de file d'attente pour les services qu'il utilise et pour dégrader gracieusement lorsque les files d'attente sont près de pleine capacité. Veiller à ce qu'il y a suffisamment de performances et de capacité disponibles dans des conditions non rafale pour vider les files d'attente et traiter les demandes en suspens. Pour plus d'informations, consultez le [Charge en fonction des file d'attente de mise à niveau modèle](https://msdn.microsoft.com/library/dn589783.aspx).

## Déploiement et maintenance

- **Déployer plusieurs instances de rôles pour chaque service.** Microsoft fait des garanties de disponibilité pour les services que vous créez et déployez, mais ces garanties ne sont valables que si vous déployez au moins deux instances de chaque rôle dans le service. Cela permet à un seul rôle ne sera pas disponible pendant que l'autre reste actif. Ceci est particulièrement important si vous devez déployer des mises à jour d'un système live sans interrompre les activités des clients ; instances peuvent être démontés et mis à jour individuellement tandis que les autres continuent en ligne.
- **Héberger des applications dans plusieurs centres de données.** Bien qu'extrêmement improbable, il est possible pour un datacenter entier aller en mode hors connexion grâce à un événement tel qu'une catastrophe naturelle ou tronc panne d'Internet. Les applications vitales doivent être hébergées dans plus d'un datacenter pour assurer une disponibilité maximale. Cela a un avantage supplémentaire en ce qu'il peut réduire la latence pour les utilisateurs locaux et offre des possibilités supplémentaires de flexibilité lors de l'actualisation des applications.
- **Automatiser et tester les tâches de déploiement et de maintenance.** Applications distribuées sont constitués de plusieurs parties qui doivent travailler ensemble. Déploiement devrait par conséquent être automatisée à l'aide de testé et éprouvé de mécanismes tels que les scripts et les applications de déploiement de mise à jour et valider la configuration et automatiser le processus de déploiement. Techniques automatisées devraient également être utilisés pour effectuer des mises à jour de tout ou partie des applications. Il est essentiel de tester tous ces processus entièrement pour s'assurer que les erreurs n'entraînent pas des interruptions de service supplémentaires. Tous les outils de déploiement doivent avoir des restrictions de sécurité appropriées pour protéger l'application déployée ; définir et appliquer des stratégies de déploiement soigneusement et réduire au minimum le besoin d'intervention humaine.
- **Pensez à utiliser des caractéristiques intermédiaires et de production de la plate-forme** où ils sont disponibles. Par exemple, en utilisant des milieux intermédiaires et de production de Services Cloud Azure permet aux applications d'être commuté de l'un à l'autre instantanément grâce à un échange d'adresse IP virtuels (VIP Swap). Toutefois, si vous préférez organiser localement, ou déployer simultanément les différentes versions de l'application et progressivement migrer les utilisateurs, vous ne pourrez pas utiliser une opération de Swap de VIP.
- **Appliquer les modifications de configuration sans recyclage** l'instance lorsque cela est possible. Dans de nombreux cas, les paramètres de configuration pour une application Azure ou un service peuvent être modifiées sans exiger le rôle doit être redémarré. Rôle exposent des événements qui peuvent être gérés pour détecter les modifications de configuration et de les appliquer aux composants dans l'application. Cependant, certains changements aux paramètres plateforme core nécessitera un rôle doit être redémarré. Lorsque vous générez des composants et services, optimiser la disponibilité et réduire les temps d'arrêt grâce à la conception d'accepter des modifications apportées aux paramètres de configuration sans nécessiter l'application dans son ensemble doit être redémarré.
- **Utilisez la mise à niveau de domaines pour aucune interruption de service lors de mises à jour.** Azure compute unités telles que web et rôles de travail sont consacrés au domaines. Mise à niveau instances de rôle pour le groupe domaines ensemble afin que, lorsqu'une mise à jour propagée a lieu, chaque rôle dans le domaine de la mise à niveau est arrêté, mis à jour et redémarré à son tour afin de minimiser l'impact sur la disponibilité de l'application. Vous pouvez spécifier combien de domaines mise à niveau doit être créé pour un service lorsque le service est déployé.

	> [AZURE. REMARQUE] Rôles sont également distribués sur plusieurs domaines faute, dont chacun est raisonnablement indépendante depuis d'autres domaines de faute en matière de serveur rack, alimentation et refroidissement disposition, afin de minimiser le risque d'un échec qui affectent toutes les instances de rôle. Cette distribution s'effectue automatiquement et vous ne pouvez pas le contrôler.

- **Configurer la disponibilité des ensembles pour les machines virtuelles d'Azur.** Placing two or more virtual machines in the same availability set guarantees that these virtual machines will not be deployed to the same fault domain. To maximize availability, you should create multiple instances of each critical virtual machine used by your system and place these instances in the same availability set. If you are running multiple virtual machines that serve different purposes, create an availability set for each virtual machine and add instances of each virtual machine to each availability set. For example, if you have created separate virtual machines to act as a web server and a reporting server, create an availability set for the web server and another availability set for the reporting server. Add instances of the web server virtual machine to the web server availability set, and add instances of the reporting server virtual machine to the reporting server availability set.

## Gestion des données

- **Profitez de la réplication des données** par le biais de redondance tant locale que géographique. Données à Azure storage sont automatiquement répliquées pour protéger contre la perte en cas de défaillance de l'infrastructure, et certains facteurs de cette réplication peuvent être configurés. Par exemple, copies en lecture seule des données peuvent être répliquées dans plusieurs régions géographiques (dénommée stockage redondant dans le monde-un accès en lecture ou RA-GRS). Notez qu'à l'aide de RA-GRS engage des frais supplémentaires, voir le [Stockage d'Azur prix](http://azure.microsoft.com/pricing/details/storage/) page sur le site Web de Microsoft pour plus de détails.
- **Utiliser l'accès concurrentiel optimiste et l'uniformité éventuelle** Lorsque c'est possible. Transactions que bloquer l'accès aux ressources par le biais de verrouillage (accès concurrentiel pessimiste) peut entraîner une performance médiocre et réduire considérablement la disponibilité. Ces problèmes peuvent devenir particulièrement aigus dans les systèmes distribués. Dans de nombreux cas, une bonne conception et techniques telles que le partitionnement peuvent réduire les risques de mises à jour conflictuelles qui se produisent. Où les données sont répliquées, ou sont lues dans un magasin séparément mis à jour, les données ne seront finalement cohérentes, mais les avantages dépassent généralement l'impact sur la disponibilité d'utiliser des transactions pour assurer cohérence immédiate.
- **Utilisation sauvegarde périodique et un point de restauration temps**, and ensure it meets the Recovery Point Objective (RPO). Regularly and automatically back up data that is not preserved elsewhere, and verify you can reliably restore both the data and the application itself should a failure occur. Data replication is not a backup feature because errors and inconsistencies introduced through failure, error, or malicious operations will be replicated across all stores. The backup process must be secure to protect the data in transit and in storage. Databases or parts of a data store can usually be recovered to a previous point in time by using transaction logs. Microsoft Azure provides a backup facility for data stored in Microsoft Azure SQL Database. The data is exported to a backup package on Microsoft Azure blob storage, and can be downloaded to a secure on-premises location for storage.
- **Activez l'option de haute disponibilité de conserver une copie secondaire d'un cache Redis.** Lorsque vous utilisez Cache Redis, choisissez l'option Standard de conserver une copie secondaire du contenu. Pour plus d'informations, consultez la page [Créer un cache dans Azure Redis Cache](https://msdn.microsoft.com/library/dn690516.aspx) sur le site Web de Microsoft.

## Erreurs et échecs

- **Introduire la notion d'un délai d'attente.** Services et ressources peuvent devenir indisponibles, provoquant des demandes à l'échec. S'assurer que les délais d'attente vous appliquez sont appropriés pour chaque service ou une ressource ainsi que le client qui accède à eux (dans certains cas, il peut être opportun d'accorder un délai plus long pour une instance particulière d'un client, selon le contexte et les autres actions que le client est en train d'effectuer). Délais d'attente très court peut provoquer des opérations réessayer excessif pour les services et les ressources qui ont une latence considérable, mais très longs délais d'attente peuvent causer blocage si un grand nombre de demandes est en attente d'attente d'un service ou une ressource pour répondre.
- **Nouvelle tentative ratées opérations causées par des anomalies transitoires.** Concevoir une stratégie de nouvelle tentative pour accéder à tous les services et ressources où ils ne supportent pas intrinsèquement de réessayer la connexion automatique. Utiliser une stratégie qui inclut un délai croissant entre les tentatives comme le nombre d'échecs augmente afin d'éviter une surcharge des ressources et lui permettre de récupérer et de traiter les demandes en file d'attente avec élégance. Des tentatives continuelles avec des délais très courts sont susceptibles d'exacerber le problème.
- **Cesser d'envoyer des demandes afin d'éviter les échecs en cascade** when remote services are unavailable. There may be situations where transient or other faults, ranging in severity from a partial loss of connectivity to the complete failure of a service, take much longer than expected to return to normal. Additionally, if a service is very busy, failure in one part of the system may lead to cascading failures, and result in many operations becoming blocked while holding onto critical system resources such as memory, threads, and database connections. Instead of continually retrying an operation that is unlikely to succeed, the application should quickly accept that the operation has failed, and gracefully handle this failure. You can use the Circuit Breaker pattern to reject requests for specific operations for defined periods. The [Modèle de disjoncteur](https://msdn.microsoft.com/library/dn589784.aspx)page sur le site Web Microsoft fournit plus de détails.
- **Composer ou se replier à plusieurs composants** pour atténuer l'impact d'un service spécifique étant indisponible ou hors connexion. Conception d'applications pour tirer parti de plusieurs instances sans opération affecte et les connexions existantes lorsque c'est possible. Utiliser plusieurs instances et distribuer les requêtes entre eux et détecter et éviter l'envoi de demandes aux instances ayant échouées, afin de maximiser la disponibilité.
- **Basculer vers un autre service ou un workflow** Lorsque c'est possible. Par exemple, si l'écriture dans la base de données SQL échoue, stocker temporairement des données en stockage blob et fournir une installation de rejouer les écritures en stockage blob SQL base de données lorsque le service sera disponible. Dans certains cas, une opération qui a échoué peut avoir une action qui permet à l'application de continuer à travailler même quand un composant ou un service échoue. Si possible, pouvoir détecter les défaillances et rediriger les demandes vers d'autres services que peuvent offrir une fonctionnalité alternative convenable, ou pour sauvegarder ou de réduire les cas de fonctionnalité qui peuvent maintenir les opérations essentielles alors que le service primaire est actuellement hors ligne.

## Surveillance et reprise après sinistre

- **Prévoir des riche instrumentation susceptibles de défaillances et les événements d'échec** pour signaler la situation au personnel des opérations. Pour les défaillances qui sont susceptibles mais n'ont pas encore eu lieu, fournir des données suffisantes pour permettre au personnel d'opérations déterminer la cause, d'atténuer la situation et faire en sorte que le système reste disponible. Pour les défaillances qui ont déjà eu lieu, l'application doit renvoyer un message d'erreur à l'utilisateur mais tenter de continuer à fonctionner, mais avec des fonctionnalités réduites. Dans tous les cas, le système de surveillance doit capturer des détails complets pour permettre au personnel d'exploitation effectuer une restauration rapide et si nécessaire pour les concepteurs et les développeurs de modifier le système empêcher la situation de se produire à nouveau.
- **Surveiller la santé système en mettant en œuvre des fonctions de vérification.** La santé et les performances d'une application peuvent se dégrader au fil du temps sans être perceptible jusqu'à ce qu'il échoue. Une façon d'éviter cela consiste à implémenter des sondes ou vérifier les fonctions qui sont exécutées régulièrement de l'extérieur de l'application. Ces contrôles peuvent être aussi simples que de mesurer les temps de réponse pour l'application dans son ensemble, pour les différentes parties de la demande, pour des services individuels que l'application utilise ou pièces détachées. Fonctions de vérification peuvent exécuter des processus pour s'assurer qu'ils produisent des résultats valables, mesurent la latence et voir les disponibilités et extraire les informations du système.
- **Tester régulièrement tous les basculement et les systèmes de secours** pour s'assurer qu'ils sont disponibles et fonctionnent comme prévu. Changements aux opérations et systèmes peuvent affecter basculement et fonctions de secours, mais l'impact ne peut pas être détectée jusqu'à ce que le système principal tombe en panne ou devient surchargé. Il est conseillé de le tester avant, il est nécessaire pour compenser un problème direct lors de l'exécution.
- **Tester les systèmes de surveillance.** Basculement automatique et systèmes de secours et visualisation manuelle du système de santé et de la performance à l'aide de tableaux de bord que dépendent tous de la surveillance et l'instrumentation fonctionne correctement. Si ces éléments ne conduisent pas, manquer des informations critiques ou signaler des données inexactes, puis un opérateur ne réalise pas que le système est malsain ou défaillants.
- **Suivre la progression des flux de travail longue durée** et nouvelle tentative en cas d'échec. Flux de travail longue durée est souvent composées de plusieurs étapes. Lors de la conception de ces types de flux de veiller à ce que chaque étape est indépendante et peut être rejugé pour minimiser le risque que l'ensemble du workflow devront être annulées ou que plusieurs transactions de compensation doivent être exécutés. Surveiller et gérer l'état d'avancement du flux de travail de longue durée en mettant en place un modèle comme planificateur Agent superviseur. Pour plus d'informations, consultez le [Scheduler Agent superviseur Pattern](https://msdn.microsoft.com/library/dn589780.aspx) page sur le site Web de Microsoft.
- **Plan de reprise après sinistre.** Vérifiez un plan documenté, concertée et entièrement testé pour la récupération de n'importe quel type de défaillance susceptible de rendre partie ou la totalité du réseau principal est indisponible. Les procédures d'essai régulièrement et veiller à ce que tout le personnel des opérations est familier avec le processus.