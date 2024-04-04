## Locataires WWL - Conditions d’utilisation

Si un locataire vous est fourni dans le cadre d’une formation dispensée par un instructeur, notez qu’il est mis à votre disposition dans le seul but de prendre en charge les labos pratiques de la formation. 

Vous ne devez ni partager ni utiliser les locataires en dehors des labos pratiques. Le locataire utilisé dans ce cours est un locataire d’essai. Au terme de la classe, le locataire ne pourra pas faire l’objet d’une prolongation et vous ne pourrez plus l’utiliser ni y accéder. 

Vous n’êtes pas autorisé à convertir un locataire en abonnement payant. Les locataires obtenus dans le cadre de ce cours sont la propriété de Microsoft Corporation. Nous nous réservons le droit d’y accéder et d’en reprendre possession à tout moment. 

# Parcours d’apprentissage 1 : Labo 1 – Exercice 1 – Initialiser votre locataire Microsoft 365 

Adatum Corporation est une filiale de Contoso Electronics. Adatum exécute ses applications héritées (comme Microsoft Exchange Server 2019) dans un déploiement local. En votre qualité de nouvel administrateur Microsoft 365, nommé Holly Dickson, vous avez été chargé de préparer le déploiement Microsoft 365 d’Adatum pour Copilot pour Microsoft 365. 

Dans cet exercice, vous allez configurer le locataire d’évaluation gratuite Microsoft 365 d’Adatum et votre instructeur vous expliquera comment obtenir vos informations d’identification Microsoft 365 dans votre environnement hébergé dans un labo. Vous allez utiliser ces informations d’identification dans les labos restants de ce cours. 

Dans votre environnement de labo, le fournisseur d’hébergement de votre labo a déjà obtenu un tenant d’évaluation gratuite Microsoft 365 pour vous. Le fournisseur de votre labo a déjà créé deux comptes administrateur que vous allez utiliser dans l’environnement de labo de votre machine virtuelle : 

- Un compte administrateur local pour l’environnement local d’Adatum (Adatum\Administrateur).
- Un compte administrateur de locataire par défaut dans Microsoft 365 (le nom d’affichage pour ce compte d’utilisateur est Administrateur MOD). 

Vous allez vous connecter au Client 1 PC (LON-CL1) en utilisant le compte Adatum\Administrateur local. Lorsque vous accédez à Microsoft 365 pour la première fois, vous vous connectez initialement en utilisant le compte administrateur de locataire Microsoft 365 (Administrateur MOD). Vous allez ensuite préparer le locataire Microsoft 365 d’Adatum pour Microsoft Entra ID et pour les labos ultérieurs en tirant parti des alertes d’audit et de Microsoft Graph PowerShell.


### Tâche 1 : obtenir vos informations d’identification Microsoft 365

Une fois votre labo lancé, vous allez être en mesure d’accéder au locataire d’évaluation gratuite Microsoft 365 fourni par le fournisseur d’hébergement de votre labo dans l’environnement Labo de machine virtuelle Microsoft. Au sein de ce locataire, le fournisseur d’hébergement de votre labo a créé un compte d’utilisateur Microsoft 365 pour un administrateur de locataire par défaut nommé Administrateur MOD. Le fournisseur d’hébergement de votre labo a attribué à ce compte d’utilisateur un nom d’utilisateur et un mot de passe uniques et le rôle Administrateur général Microsoft 365 lui a été affecté. Vous devez récupérer ce nom d’utilisateur et ce mot de passe afin de pouvoir vous connecter à Microsoft 365 dans l’environnement Labo virtuel Microsoft. Vous allez également vous voir attribuer un nom et un préfixe de locataire. Vous utilisez également ces informations dans diverses tâches des labos de ce cours.

Du fait que des partenaires de formation peuvent proposer ce cours en utilisant l’un des fournisseurs autorisés d’hébergement de labo, les étapes réelles impliquées pour récupérer le nom d’utilisateur principal (UPN) et l’ID de locataire associés à votre locataire peuvent varier selon le fournisseur d’hébergement de labo. Par conséquent, votre instructeur vous fournira les instructions nécessaires sur la façon de récupérer ces informations pour votre cours. <br/>

Nous vous conseillons de noter les informations suivantes (fournies par votre instructeur) pour une utilisation ultérieure :

- **Préfixe de locataire.** Ce préfixe de locataire est destiné aux comptes d’utilisateur Microsoft 365 que vous allez utiliser pour vous connecter à Microsoft 365 dans les labos de ce cours. Le domaine de chaque compte d’utilisateur Microsoft 365 est au format {user alias}@xxxxxZZZZZZ.onmicrosoft.com, où xxxxxZZZZZZ correspond au préfixe de locataire. Il se compose de deux parties : le préfixe (xxxxx, certains hébergeurs utilisent un préfixe générique comme M365x, tandis que d’autres utilisent les initiales de leur entreprise ou une autre désignation) et l’ID de locataire (ZZZZZZ, un numéro à 6 chiffres) de l’hébergeur de votre labo. Enregistrez la valeur de ce préfixe de locataire xxxxxZZZZZZ pour une utilisation ultérieure. Lorsque l’une des étapes de labo vous demande de vous connecter à Microsoft 365 en tant que l’un des comptes d’utilisateur (tels que l’Administrateur MOD), vous devez entrer la valeur xxxxxZZZZZZ obtenue ici comme partie du préfixe de locataire de votre domaine .onmicrosoft.com.

- **Mot de passe du locataire.** Il s’agit du mot de passe fourni par le fournisseur d’hébergement de votre labo pour le compte administrateur de locataire. **Remarque :** Vous utilisez non seulement ce mot de passe pour le compte d’utilisateur de locataire, mais aussi pour chacun des comptes d’utilisateur prédéfinis utilisés dans les labos.

- **Nom de domaine personnalisé.** Le fournisseur d’hébergement de votre labo a créé un nom de domaine personnalisé pour Adatum. Vous allez utiliser ce domaine lors de l’ajout d’un domaine personnalisé dans Microsoft 365 dans un exercice ultérieur du labo. Le nom de domaine est au format **xxxUPNxxx.xxxCustomDomainxxx.xxx.** Vous devez remplacer **xxxUPNxxx** par le numéro du nom d’utilisateur principal fourni par le fournisseur d’hébergement de votre labo. Vous devez également remplacer **xxxCustomDomainxxx.xxx** par le nom de domaine du fournisseur d’hébergement du labo. Par exemple, supposons que le fournisseur d’hébergement de votre labo soit Fabrikam Inc. Si le nom d’utilisateur principal qu’il attribue à votre locataire est AMPVU3A et que son nom de domaine personnalisé est fabrikam.us, le nom de domaine de votre nouveau domaine personnalisé est alors AMPVU3a.fabrikam.us. Votre instructeur va vous fournir le nom de domaine personnalisé et le numéro du nom d’utilisateur principal du fournisseur d’hébergement de votre labo.  


### Tâche 2 : configurer un profil d’organisation pour Adatum

Tout au long des labos de ce cours, vous allez jouer le rôle du personnage nommé Holly Dickson, l’administrateur Microsoft 365 d’Adatum. Dans le cadre de votre rôle en tant qu’Holly, vous vous chargez de la configuration du profil de l’entreprise pour son locataire d’évaluation gratuite Microsoft 365. Dans cette tâche, vous allez configurer les options nécessaires pour un locataire d’Adatum. Étant donné qu’Holly n’a pas encore créé de compte d’utilisateur personnel Microsoft 365 pour elle-même (vous l’effectuerez dans l’exercice de labo suivant), Holly se connecte initialement à Microsoft 365 en utilisant le compte administrateur de locataire et le mot de passe Microsoft 365 par défaut créés par le fournisseur d’hébergement de votre labo. Ce compte constitue le compte **Administrateur MOD** dont l’alias est « admin ». Le nom d’utilisateur pour ce compte est **admin@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire attribué par le fournisseur d’hébergement de votre labo). Le nom d’affichage pour ce compte est Administrateur MOD.

1. Lorsque vous ouvrez l’environnement de machine virtuelle du fournisseur d’hébergement de votre labo, vous devez commencer par la Machine virtuelle Client 1 (LON-CL1). Si votre environnement de machine virtuelle s’ouvre avec l’une des autres machines (telle que LON-DC1), basculez maintenant vers **LON-CL1**.

2. Connectez-vous à **LON-CL1** en tant que le compte **Administrateur** local créé par le fournisseur d’hébergement de votre labo avec le mot de passe **Pa55w.rd**. 

3. Dans la barre des tâches en bas de votre écran, sélectionnez l’icône **Microsoft Edge**. Si nécessaire, agrandissez la fenêtre de votre navigateur quand elle s’ouvre.

4. Dans votre navigateur Edge, accédez à la page **Accueil Microsoft 365** en entrant l’URL suivante dans la barre d’adresse : **https://portal.office.com** 

5. Dans la boîte de dialogue **Se connecter** qui s’affiche, entrez le **Nom d’utilisateur de locataire Microsoft 365** fourni par le fournisseur d’hébergement de votre labo (il s’agit du compte Administrateur MOD). Le nom d’utilisateur doit se présenter sous la forme **admin@xxxxxZZZZZZ.onmicrosoft.com**, où xxxxxZZZZZZ est le préfixe de locataire attribué par le fournisseur d’hébergement de votre labo. Cliquez sur **Suivant**. <br/>

    **Remarque :** Il est possible que l’hébergement de votre labo offre la possibilité de sélectionner un bouton **Saisir le texte** (ou équivalent) près des données de ressource, tel que les noms d’utilisateur, les mots de passe, les commandes PowerShell et d’autres données devant être entrés au cours de ces labos. D’autres fournisseurs d’hébergement de labo peuvent offrir une autre méthode, comme la possibilité de copier et de coller ces informations. Tirez parti de cette fonctionnalité pour vous éviter d’avoir à saisir manuellement ces informations. 

6. Dans la boîte de dialogue **Entrer le mot de passe**, entrez le **Mot de passe de locataire Microsoft 365** unique fourni par le fournisseur d’hébergement de votre labo, puis sélectionnez **Se connecter**.

7. Dans la boîte de dialogue **Rester connecté ?**, cochez la case **Ne plus afficher**, puis sélectionnez **Oui.** Dans la boîte de dialogue **Enregistrer le mot de passe** qui s’affiche, sélectionnez **Jamais**.

8. Si une boîte de dialogue **Bienvenue dans Microsoft 365** s’affiche au milieu de l’écran, il n’existe aucune option pour la fermer. Au lieu de cela, à droite de la fenêtre, sélectionnez deux fois l’icône de flèche vers l’avant (**>**), puis sélectionnez l’icône de coche pour consulter les diapositives de cette fenêtre de message. 

9. Si une fenêtre **Rechercher d’autres applications** ou **Créer avec Microsoft 365** s’affiche, sélectionnez le **X** dans le coin supérieur droit de la fenêtre pour la fermer. 

10. La page **Bienvenue dans Microsoft 365** s’affiche dans votre navigateur Edge sous l’onglet **Accueil | Microsoft 365**. Il s’agit de la page d’accueil Microsoft 365 de l’Administrateur MOD. <br/>

    Notez que l’icône s’affiche dans le coin supérieur droit de l’écran. Cette icône représente le compte **Administrateur MOD**, qui est le compte administrateur de locataire créé par le fournisseur d’hébergement de votre labo, au nom duquel vous venez de vous connecter. Les autres comptes d’utilisateur Microsoft 365 existants créés par le fournisseur d’hébergement de votre labo ont une image associée à chacun de leur compte. Par conséquent, lorsque vous vous connectez comme l’un de ces utilisateurs dans des labos ultérieurs, l’image de l’utilisateur s’affiche au lieu des initiales de l’utilisateur. Toutefois, quand un utilisateur comme l’Administrateur MOD n’y a aucune image associée, les initiales de l’utilisateur s’affichent au lieu de l’image ou, comme dans ce cas, une icône ayant été attribuée au compte. <br/>

    Dans la page **Bienvenue dans Microsoft 365**, dans la liste des icônes d’application qui s’affiche dans le volet gauche, sélectionnez **Administrateur**. Cela ouvre le **Centre d’administration Microsoft 365** dans un nouvel onglet de navigateur. 

11. Dans le **Centre d’administration Microsoft 365**, sélectionnez **Tout afficher** dans le volet de navigation, puis **Sécurité**. Dans le groupe **Paramètres**, sélectionnez **Paramètres de l’organisation**. 

12. Sur la page des **paramètres de l'organisation**, l'onglet **Services** s'affiche par défaut. Sélectionnez l’onglet **Profil d’organisation**.

13. Sous l’onglet **Profil d’organisation**, sélectionnez **Informations de l’organisation** dans la liste des données de profil.

14. Dans le volet **Informations de l’organisation** qui s’affiche, entrez les informations suivantes : <br/>

    - Nom : **Adatum Corporation** (remarque : Adatum Corporation est une filiale de Contoso Inc. Il est possible que le locataire d’évaluation gratuite Microsoft obtenu par le fournisseur d’hébergement de votre labo pour ce labo ait été initialement attribué à Contoso. Si **Contoso** (ou toute autre valeur) s’affiche comme le nom d’organisation, remplacez-le par **Adatum Corporation**).

    - Nom de rue : **555 Main Street**

    - City: **Redmond**

    - État ou province : **Washington**

    - Code postal : **98052**

    - Téléphone : ne pas modifier

    - Contact technique : ne pas modifier

    - Langue préférée : **Anglais**

15. Sélectionnez **Enregistrer**.

16. En haut du volet **Informations de l’organisation**, notez le message indiquant que les modifications ont été enregistrées. Sélectionnez le **X** dans le coin supérieur droit pour fermer le volet.

17. Restez connecté à **LON-CL1** avec Microsoft Edge ouvert dans le **Centre d’administration Microsoft 365** pour la tâche suivante.

### Tâche 3 : créer un thème personnalisé pour une équipe de projet pilote d’Adatum

Dans la tâche précédente, vous avez appris que lorsqu’un utilisateur est connecté à Microsoft 365, le système affiche sa photo (s’il en a fourni une) ou ses initiales dans le cas contraire. Holly Dickson, l’Administrateur Microsoft 365 d’Adatum, ne se satisfait pas de voir simplement la photo ou les initiales de l’utilisateur connecté. Elle souhaite créer un thème personnalisé pour les membres de l’équipe de son projet pilote afin qu’il affiche également le nom de l’utilisateur connecté. Si les membres de l’équipe préfèrent cette conception à la fin du projet pilote, elle configurera cette même option dans le thème par défaut afin qu’elle s’applique à tous les utilisateurs. 

Les thèmes personnalisés doivent être associés à un ou plusieurs groupes Microsoft 365. Par conséquent, Holly doit d’abord créer un groupe Microsoft 365 pour tous les membres de l’équipe du projet pilote afin d’implémenter cette modification. Elle peut également créer un thème personnalisé associé à ce groupe qui permet au paramètre d’afficher le nom d’utilisateur connecté. Dans cette tâche, vous allez créer un groupe Microsoft 365 pour les membres de l’équipe du projet pilote Microsoft 365. Vous allez ensuite créer un thème personnalisé qui affiche le nom d’utilisateur connecté et attribuer ce thème à l’équipe du projet pilote. Vous allez également passer en revue d’autres options que vous pouvez configurer avec des thèmes personnalisés. Vous pouvez également apporter tous les changements de couleur de votre choix.

**Remarque :** Vous allez également utiliser l’enregistrement d’équipe, que vous créez pour l’équipe de projet pilote Microsoft 365, dans des exercices de labo ultérieurs dans lesquels vous créez une stratégie d’accès conditionnel pour activer l’authentification multifacteur. Vous allez activer l’authentification multifacteur (MFA) pour tous les utilisateurs, à l’exception des membres de l’équipe du projet pilote.

1. Vous devez toujours être connecté à LON-CL1 sous le compte local **adatum\administrateur**, et dans votre navigateur Edge, vous devez toujours être connecté à Microsoft 365 en tant qu’**Administrateur MOD**. 

2. Dans le **Centre d’administration Microsoft 365**, sélectionnez **Équipes + groupes** dans le volet de navigation et, en dessous, sélectionnez **Équipes et groupes actifs**. 

3. Dans la page **Équipes et groupes actifs** figure un onglet permettant d’afficher chacun des types de groupe. L’onglet **Groupes Microsoft 365 et Teams** est affiché par défaut. Cet onglet affiche les groupes Microsoft 365 existants.  <br/>

    Sélectionnez l’option **+ Ajouter un groupe Microsoft 365** qui s’affiche dans la barre de menus au-dessus de la liste des Groupes Microsoft 365 et Teams. Cette option lance l’Assistant **Ajouter un groupe Microsoft 365**. 

4. Dans l’Assistant **Ajouter un groupe Microsoft 365** de la page **Configurer les éléments de base**, entrez **Projet pilot M365** dans le champ **Nom**, puis **Membres de l’équipe du projet pilote Microsoft 365** dans le champ **Description** (remarque : même si vous n’entrez aucune description, vous devez toujours sélectionner ce champ pour activer le bouton **Suivant**). Cliquez sur **Suivant**.

5. Vous allez maintenant attribuer l’Administrateur MOD en tant que propriétaire du groupe **projet pilote M365**. Dans la fenêtre **Attribuer des propriétaires**, sélectionnez **+ Attribuer des propriétaires**.
    
6. Dans le volet **Attribuer des propriétaires** qui s’affiche, sélectionnez la case à cocher près de **Administrateur MOD**, puis le bouton **Ajouter (1)** en bas du volet.

7. Dans la page **Attribuer des propriétaires**, l’Administrateur MOD doit s’afficher comme propriétaire du groupe. Cliquez sur **Suivant**.

8. Vous allez maintenant attribuer des membres au groupe de projet pilote M365. Dans la page **Ajouter des membres**, sélectionnez **+ Ajouter des membres**.

9. Dans le volet **Ajouter des membres** qui s’affiche, sélectionnez les cases à cocher près des utilisateurs suivants (dans l’ordre alphabétique) : **Alex Wilber**, **Allan Deyoung**, **Diego Siciliani**, **Isaiah Langer**, **Joni Sherman**, **Lynne Robbins**, **Megan Bowen**, **Administrateur MOD**, **Nestor Wilke** et **Patti Fernandez**. Ensuite, sélectionnez le bouton **Ajouter (10)** en bas du volet.

10. Dans la page **Ajouter des membres**, vérifiez que ces 10 utilisateurs sont répertoriés comme membres du groupe. Si un utilisateur est manquant, sélectionnez **+ Ajouter des membres**, puis ajoutez l’un des 10 utilisateurs manquants. Quand tous les 10 utilisateurs s’affichent dans la page, sélectionnez **Suivant**.

11. Dans la page **Modifier les paramètres**, entrez les informations suivantes : <br/>

    - Entrez **m365pilotproject** dans le champ **Adresse e-mail du groupe**.
    - Dans le champ **Confidentialité**, sélectionnez **Privé**.
    - Sous la section **Ajouter Microsoft Teams à votre groupe**, vérifiez que la case à cocher **Créer une équipe pour ce groupe** est sélectionnée (sélectionnez-la si elle est vide), puis sélectionnez **Suivant**.

12. Dans la page **Vérifier et terminer l’ajout d’un groupe**, passez en revue le contenu saisi. Si vous devez corriger un élément, sélectionnez **Modifier** sous la zone spécifique nécessitant un ajustement, apportez les corrections nécessaires, puis sélectionnez **Suivant** afin de poursuivre sur cette page. Une fois que tout est correct, sélectionnez **Créer un groupe**.

13. Une fois que la fenêtre **Groupe de projet pilot M365 créé** s’affiche, notez le commentaire en haut de la page indiquant que l’affichage du nouveau groupe dans la liste des groupes actifs peut prendre 5 minutes.  </br>

    Sélectionnez **Fermer**. Cette option vous fait revenir à la page **Équipes et groupes actifs** dans laquelle l’onglet **Groupes Microsoft 365 et Teams** doit s’afficher. Étant donné que le groupe du projet pilote M365 était un groupe Microsoft 365, il doit finir par s’afficher sous cet onglet. Le cas échéant, sélectionnez l’option **Actualiser** sur la barre de menu jusqu’à ce que vous voyiez le groupe du projet pilote M365 dans la liste des Groupes Microsoft 365 et Teams.

14. Dans le **Centre d’administration Microsoft 365**, sous le groupe **Paramètres** dans le volet de navigation, sélectionnez **Paramètres de l’organisation**. 

15. Dans la page **Paramètres de l’organisation**, sélectionnez l’onglet **Profil de l’organisation**.

16. Dans les données du profil de l’organisation, sélectionnez **Thèmes personnalisés**.

17. Dans le volet **Personnaliser Microsoft 365 pour votre organisation** qui s’affiche, vous pouvez personnaliser le thème par défaut visible par les utilisateurs connectés à Microsoft 365 ou ajouter d’autres thèmes personnalisés. Sélectionnez l’option **+ Ajouter un thème**.

18. Dans le volet **Nouveau thème de groupe** qui s’affiche, l’onglet **Général** s’affiche par défaut. Entrez **Thème de projet pilote M365** dans le champ **Nom**.

19. Cliquez à l’intérieur du champ **Groupes**. Dans la liste de groupes qui s’affiche, sélectionnez **Projet pilote M365** s’il s’affiche dans celle-ci. <br/>

    **Remarque :** Si **Projet pilote M365** ne s’affiche pas dans la liste de groupes, entez **M365** dans le champ **Groupes**. Une zone des résultats de la recherche doit s’afficher et montrer le groupe **Projet pilote M365**. Sélectionnez **Projet pilote M365**. 

20. Sélectionnez la case à cocher **Afficher le nom d’affichage de l’utilisateur**. Il s’agit du paramètre qu’Holly souhaite personnaliser pour les membres de l’équipe du projet pilote M365. Cette option affiche le nom des utilisateurs à côté de leurs initiales dans chaque titre de fenêtre.
 
21. Sélectionnez **Enregistrer**. Fermez le volet **Thème de projet pilote M365** une fois vos modifications enregistrées. 

22. Sélectionnez l’icône **Actualiser** en haut de l’écran, à gauche de la barre d’adresse. Une fois l’écran actualisé, notez la façon dont le nom **Administrateur MOD** s’affiche à gauche du cercle avec l’icône de mégaphone. Lorsque des membres de l’équipe du projet pilote Microsoft 365 se connectent à Microsoft 365, leur nom d’utilisateur s’affiche désormais à gauche de leur image de profil (ou, dans ce cas, une icône de mégaphone) ou de leurs initiales en raison du thème personnalisé créé.

23. Dans les données du profil de l’organisation, sélectionnez **Thèmes personnalisés**.

24. Dans le volet **Personnaliser Microsoft 365 pour votre organisation** qui s’affiche, notez l’affichage du **Thème par défaut** et du **Thème du projet pilote M365**. Sélectionnez le **Thème par défaut**. 

25. Dans le volet **Thème par défaut**, notez que l’option **Afficher le nom d’affichage de l’utilisateur** n’est pas sélectionnée pour le thème par défaut. Si Holly décide plus tard de transformer l’option **Afficher le nom d’affichage de l’utilisateur** en fonctionnalité permanente, elle sélectionnera cette option dans le volet **Thème par défaut** pour qu’elle s’applique à tous les utilisateurs Adatum et supprimera le **Thème du projet pilote M365**. <br/>

    Fermez le volet **Thème personnalisé**.

26. Restez connecté à **LON-CL1** avec Microsoft Edge ouvert dans le **Centre d’administration Microsoft 365** pour la tâche suivante.

### Tâche 4 : installer Microsoft Graph PowerShell 

Microsoft Graph PowerShell est nécessaire pour effectuer plusieurs tâches de configuration lors de l’installation de Microsoft 365. Étant donné que de futurs exercices de labo vont effectuer plusieurs de ces tâches en tirant parti de Windows PowerShell, vous devez commencer par installer le module Microsoft Graph PowerShell. Ce module vous permet d’effectuer plusieurs des tâches d’administration d’organisation et d’utilisateur Microsoft 365 via PowerShell. Il est idéal pour des tâches en bloc comme les réinitialisations de mots de passe, les stratégies de mots de passe, la gestion des licences et la création de rapports, etc.  

1. Sur LON-CL1, vous devez encore être connecté en tant que compte **adatum\administrateur** local. Pour installer Microsoft Graph PowerShell, vous devez ouvrir une instance élevée de **Windows PowerShell**. Tapez **power** dans la Zone de recherche qui s’affiche dans le coin inférieur gauche de la barre des tâches. Dans la liste des résultats de la recherche, cliquez avec le bouton droit sur **Windows PowerShell** (ne sélectionnez pas Windows PowerShell ISE), puis sélectionnez **Exécuter en tant qu’administrateur** dans le menu déroulant qui s’affiche. 

2. Agrandissez votre fenêtre PowerShell. Dans **Windows PowerShell**, tapez la commande suivante lors de l’invite de commandes pour installer le module Microsoft Graph PowerShell à partir de PowerShell Gallery, puis appuyez sur Entrée : <br/>

        Install-Module Microsoft.Graph -Scope CurrentUser

3. Vous allez être invité à confirmer que vous souhaitez installer le module à partir d’un référentiel non approuvé (PSGallery). Entrez **Oui** pour sélectionner **[A] Oui pour tout**, puis appuyez sur Entrée.  <br/>

    **Remarque :** Votre réponse va lancer l’installation de tous les sous-modules de Microsoft Graph. Une fois l’affichage terminé des messages d’installation pour chaque sous-module, l’installation complète de Microsoft Graph PowerShell prend entre 5 et 10 minutes de plus. Pendant ce temps, le curseur continuer de clignoter sous le message de référentiel non approuvé. <br/>

    **Cela peut être le bon moment de faire une courte pause.**

4. Une invite de commandes s’affiche après l’installation de Microsoft Graph PowerShell. La liste des sous-modules installés s’affiche désormais. Pour cela, exécutez la commande suivante :

        Get-InstalledModule Microsoft.Graph.* | select *name*

5. Vérifiez que la liste des sous-modules installés comprend les trois sous-modules suivants utilisés dans des exercices ultérieurs du labo : 

    - Microsoft.Graph.Groups
    - Microsoft.Graph.Identity.DirectoryManagement
    - Microsoft.Graph.Users
    
    Si les trois sous-modules s’affichent dans la liste des sous-modules installés, passez à l’étape suivante. Toutefois, si l’un de ces trois sous-modules ne s’affiche pas dans la liste, exécutez la commande PowerShell suivante pour installer manuellement le sous-module manquant :

        Install-Module -Name <module name> -Scope CurrentUser

    Par exemple, si le module Microsoft.Graph.Identity.DirectoryManagement n’a pas été installé, vous exécutez la commande suivante :

        Install-Module -Name Microsoft.Graph.Identity.DirectoryManagement

6. Les paramètres de stratégie d’exécution de PowerShell imposent les scripts PowerShell qui doivent être exécutés sur un système Windows. Définir cette stratégie sur **Non approuvé** permet à Holly de charger tous les fichiers config et d’exécuter tous les scripts. À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :   <br/>

        Set-ExecutionPolicy unrestricted

    ‎Si vous êtes invité à confirmer que vous souhaitez modifier la stratégie d’exécution, entrez **A** pour sélectionner **[A] Oui pour tout.** 

7. Conservez la fenêtre PowerShell ouverte, mais réduisez-la. Vous l’utiliserez dans un exercice ultérieur du labo.


**Félicitations ! Vous avez effectué toutes les étapes d’initialisation du locataire de votre labo. Vous êtes maintenant prêt à effectuer les exercices restants du labo.**

# Fin du labo 1