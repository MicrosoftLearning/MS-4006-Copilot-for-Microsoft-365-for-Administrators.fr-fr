# Parcours d’apprentissage 2 – Labo 2 – Exercice 1 – Gérer l’accès utilisateur sécurisé 

Les organisations doivent s’assurer que l’accès à leurs données d’entreprise sur Microsoft 365 est toujours sécurisé. Microsoft 365, tout comme Copilot pour Microsoft 365, affiche souvent des données sensibles et confidentielles, notamment des e-mails, des documents, des informations client et de la propriété intellectuelle. L’accès non autorisé à Microsoft 365 peut engendrer des violations de données, une usurpation d’identité et d’autres activités malveillantes. En sécurisant l’accès des utilisateurs, les organisations peuvent empêcher les personnes non autorisées d’accéder aux données de l’entreprise, et potentiellement d’en faire un usage incorrect ou de provoquer une fuite de données, lors de l’utilisation de Microsoft 365 et Copilot pour Microsoft 365.

Dans l’exercice de labo suivant, vous allez continuer à jouer le rôle d’Holly Dickson, la nouvelle administratrice Microsoft 365 d’Adatum. Vous allez effectuer plusieurs fonctions de gestion des utilisateurs pour préparer Adatum en vue du prochain déploiement de Copilot pour Microsoft 365. Vous commencerez par créer un compte d’utilisateur Microsoft 365 pour Holly, à qui sera attribué le rôle Administrateur général Microsoft 365. 

**Remarque :** L’environnement de machine virtuelle fourni par votre fournisseur d’hébergement de labo comporte plus de 20 comptes d’utilisateur Microsoft 365, ainsi qu’un grand nombre de comptes d’utilisateur locaux. Plusieurs des comptes d’utilisateur Microsoft 365 existants seront utilisés dans les labos de ce cours. Bien que le compte Administrateur MOD ait été créé par votre fournisseur d’hébergement de labo, vous allez tout de même créer le compte d’utilisateur d’Holly Dickson, car l’attribution du rôle Administrateur général Microsoft 365 à plusieurs utilisateurs est une pratique recommandée. Cela vous offrira également l’opportunité de créer un compte d’utilisateur Microsoft 365, si vous n’êtes pas familiarisé avec le processus.

Holly a été chargée par le Directeur technique d’Adatum de déployer l’authentification multifacteur Microsoft Entra (MFA), l’authentification directe (PTA) et le verrouillage intelligent Microsoft Entra. Ces trois fonctionnalités aideront à renforcer la gestion des mots de passe au sein de l’organisation en vue de l’adoption de Copilot pour Microsoft 365. Vous déploierez l’authentification directe à l’aide de Synchronisation cloud Microsoft Entra. Quant au verrouillage intelligent, vous le déploierez à l’aide de Gestion des stratégies de groupe. 

Pour ce qui est de l’authentification multifacteur, vous allez créer une stratégie d’accès conditionnel afin de la déployer pour tous les utilisateurs d’Adatum. Ensuite, vous la modifierez de manière à exclure Holly et les membres sélectionnés de son équipe de projet pilote Microsoft 365. Cela vous évitera de devoir utiliser l’authentification multifacteur lorsque vous vous connecterez sous ces comptes d’utilisateur, et vous offrira une opportunité de pratiquer l’exclusion d’utilisateurs dans une stratégie d’accès conditionnel. 

**Remarque :** L’exclusion d’utilisateurs spécifiques de MFA n’est pas quelque chose que vous feriez normalement dans un scénario réel. Toutefois, pour gagner du temps dans ce labo de formation, nous désactiverons MFA pour les utilisateurs de test. 

### Tâche 1 – Créer un compte d’utilisateur pour l’Administrateur Microsoft 365 d’Adatum

Holly Dickson est la nouvelle administratrice Microsoft 365 d’Adatum. Aucun compte d’utilisateur Microsoft 365 n’ayant été configuré pour elle, elle s’est initialement connectée à Microsoft 365 sous le compte de l’Administrateur MOD (l’Administrateur général par défaut) dans le labo précédent. Durant cette tâche, vous continuerez à être connecté en tant qu’Administrateur MOD, et créerez un compte d’utilisateur Microsoft 365 pour Holly. Vous attribuerez également le rôle Administrateur général Microsoft 365 au compte d’Holly. Ce rôle fournira à Holly les autorisations nécessaires pour effectuer toutes les fonctions administratives dans Microsoft 365. Après cette tâche, vous vous connecterez à l’aide du nouveau compte de Holly et effectuerez tous les labos restants sous ce compte. 

**Note relative aux licences :** Avant de créer le compte de Holly, vous devez d’abord vérifier le nombre de licences disponibles. Ce faisant, vous remarquerez que bien que votre locataire de labo fournisse 20 licences Microsoft 365 E5 et 20 licences Enterprise Mobility + Security E5, toutes ces licences ont déjà été affectées aux comptes d’utilisateur existants créés par votre fournisseur d’hébergement de labo. Par conséquent, vous devez d’abord annuler l’affectation d’une licence de chaque type d’un utilisateur existant afin de pouvoir les affecter à Holly.

**Important :** En guise de meilleure pratique dans votre déploiement réel, vous devez toujours noter les informations d’identification du premier compte d’Administrateur général (dans ce labo, il s’agit du compte Administrateur MOD, dont le nom d’utilisateur est admin@xxxxxZZZZZZ.onmicrosoft.com, où xxxxxZZZZZZ est le préfixe de locataire attribué par votre fournisseur d’hébergement de labo). Vous devez conserver ces informations de compte en lieu sûr pour des raisons de sécurité. **Ce compte doit être une identité NON personnalisée** qui détient les privilèges les plus élevés possibles dans un locataire. Il ne doit **pas** être activé pour MFA, car il n’est pas personnalisé. Le nom d’utilisateur et le mot de passe de ce premier compte d’Administrateur général étant couramment partagés entre plusieurs utilisateurs, ce compte est une cible idéale pour les attaques. Par conséquent, il est toujours recommandé aux organisations de créer des comptes d’administrateur de service personnalisés (par exemple, un administrateur Exchange, un administrateur SharePoint, et ainsi de suite) et de limiter au plus le nombre d’Administrateurs généraux personnels. Les Administrateurs généraux personnels que vous créez dans votre déploiement réel doivent chacun être mappés à un utilisateur unique (par exemple, Holly Dickson), et l’authentification multifacteur (MFA) Microsoft Entra doit être appliquée pour chacun d’eux. 

Ceci étant dit, vous n’activerez pas MFA pour le compte d’Holly, car le temps est limité dans ce cours de formation, et nous ne souhaitons pas gaspiller de temps de labo en vous obligeant à vous connecter à l’aide d’une deuxième méthode d’authentification chaque fois que Holly se connecte.

1. Sur la machine virtuelle **LON-CL1**, le **Centre d’administration Microsoft 365** doit toujours être ouvert dans votre navigateur Microsoft Edge suite à l’exercice de labo précédent. Vous devez être connecté à Microsoft 365 en tant qu’**Administrateur MOD**. 

2. Étant donné que vous ajoutez un nouvel utilisateur, vous devez commencer par vérifier la disponibilité des licences avant d’ajouter le compte d’utilisateur. Dans le volet de navigation du **Centre d’administration Microsoft 365**, sélectionnez **Facturation** pour développer le groupe Facturation, puis sélectionnez **Licences**. 

3. Dans la page **Licences**, l’onglet **Abonnements** est affiché par défaut. Dans la liste des abonnements, notez que les abonnements **Enterprise Mobility + Security E5** et **Microsoft 365 E5** n’ont pas de licences disponibles. Votre locataire de labo fournit 20 licences pour chaque abonnement, mais les 40 licences ont été attribuées. Étant donné que vous devez affecter à Holly à la fois une licence **Enterprise Mobility + Security E5** et une licence **Microsoft 365 E5**, vous devez d’abord annuler l’affectation des licences d’un compte d’utilisateur existant afin de les mettre à disposition d’Holly. 

4. Dans le volet de navigation du **Centre d’administration Microsoft 365**, sélectionnez **Utilisateurs**, puis **Utilisateurs actifs**. Dans la liste **Utilisateurs actifs**, vous verrez la liste des comptes d’utilisateur existants qui ont été créés par votre fournisseur d’hébergement de labo. Étant donné que Christie Cline assumera un nouveau rôle au sein de l’entreprise et ne fera plus partie du projet pilote Microsoft 365, vous allez annuler l’attribution des licences **Enterprise Mobility + Security E5** et **Microsoft 365 E5** à partir de son compte, afin de pouvoir les réaffecter au nouveau compte d’Holly Dickson.

5. Dans la page **Utilisateurs actifs**, dans la liste des utilisateurs, sélectionnez **Christie Cline** (sélectionnez le nom de Christie en lien hypertexte, et non la case à cocher en regard de son nom).

6. Dans le volet **Christie Cline** qui s’affiche, l’onglet **Compte** s’affiche par défaut. Sélectionnez l’onglet **Licences et applications**. Sous **Licences (2)**, décochez les cases en regard d’**Enterprise Mobility + Security E5** et de **Microsoft 365 E5**, puis sélectionnez **Enregistrer les modifications**. Une fois les modifications enregistrées, fermez le volet **Christie Cline**. 

7. Vous êtes maintenant prêt à créer un compte d’utilisateur pour Holly Dickson, la nouvelle administratrice Microsoft 365 d’Adatum. Vous attribuerez à Holly le rôle Administrateur général Microsoft 365, qui lui donnera un accès global à la plupart des fonctionnalités de gestion et des données dans les services en ligne Microsoft. Vous affecterez également à Holly les deux licences que vous venez de retirer de Christie Cline. <br/>

    Dans la fenêtre **Utilisateurs actifs**, sélectionnez l’option **Ajouter un utilisateur** qui apparaît dans la barre de menus au-dessus de la liste des utilisateurs actifs. Cela démarre l’assistant **Ajouter un utilisateur**.

8. Dans la page **Configurer les éléments de base** de l’Assistant **Ajouter un utilisateur**, entrez les informations suivantes :

    - Prénom : **Holly**

    - Nom : **Dickson** 

    - Nom complet : Lorsque vous accédez à cet onglet dans ce champ, **Holly Dickson** s’affiche.

    - Nom d’utilisateur : **Holly** <br/>
    
        ‎**IMPORTANT :** À droite du champ **Nom d’utilisateur** figure le champ de domaine. Il est prérenseigné avec le domaine **xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire fourni par votre fournisseur d’hébergement de labo).<br/>
    
    - Décochez la case **Créer automatiquement un mot de passe**, ce qui affiche un nouveau champ pour entrer un mot de passe défini par l’administrateur.

    - Dans le nouveau champ **Mot de passe** qui s’affiche, entrez le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD).

    - Décochez la case **Demander à cet utilisateur de modifier son mot de passe lors de sa première connexion**. 

9. Cliquez sur **Suivant**. Si une boîte de dialogue **Enregistrer le mot de passe** s’affiche en haut de l’écran, sélectionnez **Jamais**.

10. Dans la page **Affecter des licences de produits**, entrez les informations suivantes : <br/>

    - Sélectionnez l’emplacement : **États-Unis**

    - Licences : Sous l’option **Affecter une licence de produit à l’utilisateur**, cochez les cases **Enterprise Mobility + Security E5** et **Microsoft 365 E5**.

11. Cliquez sur **Suivant**.

12. Dans la page **Paramètres facultatifs** , sélectionnez la flèche déroulante à droite de **Rôles**. 

13. Dans la section **Rôles**, sélectionnez l’option **Accès au Centre d’administration** . Lorsque vous sélectionnez cette option, les rôles d’administrateur Microsoft 365 aux niveaux inférieurs les plus couramment utilisés sont activés.  <br/>

    **Remarque :** Tous les rôles d’administrateur s’affichent si vous sélectionnez **Tout afficher par catégorie**, qui apparaît après le dernier rôle courant. Pour Holly, vous n’avez pas besoin d’afficher tous les rôles d’administrateur par catégorie, car elle recevra le rôle Administrateur général qui figure dans la liste des rôles couramment utilisés.

14. Cochez la case **Administrateur général**. <br/>

    **Remarque :** Un message d’avertissement s’affiche, indiquant qu’Adatum a déjà sept Administrateurs généraux. Dans un environnement normal, cela serait excessif et non recommandé. Pour les besoins de ce labo, le fournisseur d’hébergement de labo a attribué le rôle Administrateur général à l’Administrateur MOD et six autres comptes d’utilisateur, ce qui n’est pas quelque chose que vous verriez normalement dans un déploiement réel. Toutefois, dans le cadre de ce labo dans votre environnement de labo Adatum fictif, vous pouvez ignorer ce message. **Ceci étant dit, la meilleure pratique que vous devez suivre consiste à avoir de deux à quatre Administrateurs généraux dans vos déploiements Microsoft 365 réels.** 

15. Cliquez sur **Suivant**.

16. Dans la fenêtre **Vérifier, puis terminer**, passez en revue vos sélections. Si quelque chose doit être modifié, sélectionnez le lien **Modifier**approprié et apportez les modifications nécessaires. Autrement, si tout est correct, sélectionnez **Terminer l’ajout**. 

17. Dans la page **Holly Dickson ajoutée aux utilisateurs actifs**, dans la section **Détails de l’utilisateur**, sélectionnez l’option **Afficher** pour vérifier que le mot de passe d’Holly est identique au **Mot de passe d’administration** fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD).  <br/>

    **Remarque :** Si vous avez entré accidentellement un autre mot de passe, une fois revenu à la page **Utilisateurs actifs**, vous devrez sélectionner l’icône **Réinitialiser un mot de passe** (l’icône de clé qui apparaît lorsque vous pointez sur le compte d’Holly) afin de corriger son mot de passe.

18. Sélectionnez **Fermer**.

19. Restez connecté à la machine virtuelle Client 1 (LON-CL1) avec le Centre d’administration Microsoft 365 ouvert dans votre navigateur pour la tâche suivante.

### Tâche 2 : Configurer des comptes d’utilisateur Microsoft 365

Une fois la tâche précédente terminée, vous devriez toujours être connecté au **Centre d’administration Microsoft 365** sous le compte **Administrateur MOD**. Durant cette tâche, vous allez commencer à implémenter le projet pilote Microsoft 365 d’Adatum en tant qu’Holly Dickson, la nouvelle administratrice Microsoft 365 d’Adatum. Par conséquent, vous commencerez cette tâche en vous déconnectant de Microsoft 365 en tant qu’Administrateur MOD et en vous reconnectant à l’aide du nouveau compte Microsoft 365 d’Holly. 

Lors de la tâche précédente, vous avez remarqué que votre locataire d’évaluation Microsoft 365 disposait d’une liste d’utilisateurs actifs. En tant qu’Holly Dickson, administratrice Microsoft 365 d’Adatum, vous avez sélectionné les membres suivants de l’équipe pilote Microsoft 365 afin de faciliter la phase initiale du déploiement : Alex Wilber, Joni Sherman, Lynne Robbins et Patti Fernandez. 

Chaque utilisateur est un membre clé de votre équipe de projet pilote. Bien que leurs comptes d’utilisateur soient déjà présents dans Microsoft 365, vous souhaitez configurer leurs mots de passe afin qu’ils puissent se connecter plus facilement à Microsoft 365 si nécessaire dans les exercices de labo à venir. Vous allez également modifier le mot de passe d’Adele Vance. Adele n’est pas membre de l’équipe de projet pilote Microsoft 365, mais elle prend part aux tests de l’implémentation de l’authentification multifacteur lors d’un labo ultérieur. Vous attribuerez le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD) comme mot de passe d’utilisateur, comme vous l’avez fait lors de la création du compte d’Holly.  

**IMPORTANT :** Dans le monde réel, il est évident que le même mot de passe ne doit jamais être utilisé pour plusieurs utilisateurs. Toutefois, nous le faisons ici dans votre environnement de formation simplement afin de faciliter les choses pour les étudiants durant leur progression parmi les labos. Ceci étant dit, cette tâche vous offrira également l’opportunité de pratiquer la modification d’un mot de passe utilisateur.

1. Sur la machine virtuelle LON-CL1, le **Centre d’administration Microsoft 365** doit toujours être ouvert dans votre navigateur Microsoft Edge suite à la tâche précédente. Vous devez être connecté à Microsoft 365 en tant qu’**Administrateur MOD**. <br/>

    Sous l’onglet **Centre d’administration Microsoft 365**, dans le coin supérieur droit de l’écran, notez la présence du nom de l’Administrateur MOD et d’une icône de mégaphone. La présence du nom est due au thème personnalisé que vous avez créé dans l’exercice de labo précédent, qui a été associé à un groupe d’utilisateurs de projet pilote Microsoft 365 incluant l’Administrateur MOD. Gardez cela à l’esprit lorsque vous vous reconnecterez en tant que Holly Dickson. <br/>

    Sélectionnez l’icône d’utilisateur pour l’**Administrateur MOD** dans le coin supérieur droit de votre navigateur. Dans la fenêtre **Administrateur MOD** qui s’affiche, sélectionnez **Se déconnecter**. <br/>
    
    **Important :** Lors du basculement d’un compte d’utilisateur vers un autre, vous devez fermer tous vos onglets de navigateur à l’exception de l’onglet **Se déconnecter**. Il s’agit d’une meilleure pratique qui permet d’éviter toute confusion grâce à la fermeture des fenêtres associées à l’utilisateur précédent. Une fois que vous êtes déconnecté du compte Administrateur MOD, fermez tous les autres onglets du navigateur, à l’exception de l’onglet **Se déconnecter**. 
    
2. Dans votre navigateur Microsoft Edge, sous l’onglet **Se déconnecter**, entrez l’URL suivante dans la barre d’adresse pour vous reconnecter à Microsoft 365 : **https://portal.office.com**. 

3. Dans la fenêtre **Choisir un compte**, seul le compte d’administrateur du locataire d’Administrateur MOD (le compte admin@xxxxxZZZZZZ.onmicrosoft.com) que vous venez de déconnecter est visible. Sélectionnez **Utiliser un autre compte**. 

4. Dans la fenêtre **Se connecter**, entrez **Holly@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire fourni par votre fournisseur d’hébergement de labo). Cliquez sur **Suivant**.

5. Dans la fenêtre **Entrer le mot de passe**, entrez le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD), puis sélectionnez **Se connecter**.

6. Si une boîte de dialogue **Bienvenue dans Microsoft 365** s’affiche au milieu de l’écran, il n’existe aucune option pour la fermer. Au lieu de cela, à droite de la fenêtre, sélectionnez deux fois l’icône de flèche vers l’avant (**>**), puis sélectionnez l’icône de coche pour parcourir les diapositives de cette fenêtre de message. 

7. Si une fenêtre **Créer avec Microsoft 365** s’affiche, sélectionnez le **X** dans le coin supérieur droit de la fenêtre pour la fermer. 

8. La page **Bienvenue dans Microsoft 365** s’affiche dans votre navigateur Edge sous l’onglet **Accueil | Microsoft 365**. Il s’agit de la page d’accueil Microsoft 365 d’Holly. Notez que les initiales d’Holly apparaissent dans le coin supérieur droit de l’écran ; en revanche, son nom n’est pas affiché. Cela est dû au fait que le compte d’Holly n’existait pas au moment où vous avez ajouté les utilisateurs du projet pilote Microsoft 365 au groupe associé au thème personnalisé dans l’exercice de labo précédent. Étant donné qu’Holly souhaite voir son nom en haut de chaque fenêtre Microsoft 365 lorsqu’elle est connectée au système, elle souhaite d’abord ajouter son compte au groupe d’utilisateurs du projet pilote Microsoft 365. <br>

    Dans la colonne des icônes d’application sur le côté gauche de l’écran, sélectionnez **Administrateur**. Cela ouvre le **Centre d’administration Microsoft 365** dans un nouvel onglet de navigateur. 

9. Dans le **Centre d’administration Microsoft 365**, sélectionnez **Équipes + groupes** dans le volet de navigation et, en dessous, sélectionnez **Équipes et groupes actifs**. 

10. Dans la page **Équipes et groupes actifs** figure un onglet permettant d’afficher chacun des types de groupes. L’onglet **Groupes Teams et Microsoft 365** est affiché par défaut. Sous cet onglet, sélectionnez **Projet pilote M365**.

11. Dans le volet **Projet pilote M365** qui s’affiche, l’onglet **Général** est affiché par défaut. Sélectionnez l’onglet **Appartenance**.

12. Sous l’onglet **Appartenance**, le sous-onglet **Propriétaires** s’affiche par défaut dans le volet de navigation sur le côté gauche du volet. Sélectionnez le sous-onglet **Membres** qui apparaît en dessous.

13. Dans le sous-onglet **Membres**, sélectionnez **+Ajouter des membres**.

14. Dans le volet **Ajouter des membres de groupe à Projet pilote M365** qui s’affiche, sélectionnez à l’intérieur du champ **Rechercher par nom ou adresse de messagerie**. Dans la liste des utilisateurs qui s’affiche, faites défiler vers le bas et sélectionnez **Holly Dickson**. Sélectionnez le bouton **Ajouter (1)**, puis fermez le volet **Ajouter des membres de groupe à Projet pilote M365** une fois qu’Holly a été ajoutée au groupe.

15. Dans la page **Équipes et groupes actifs**, sélectionnez l’icône **Actualiser** qui apparaît en haut de l’écran, à gauche de la barre d’adresse. Notez que le nom d’Holly Dickson apparaît en regard de ses initiales dans le coin supérieur droit de l’écran (Remarque : Vous devrez peut-être actualiser deux fois pour que le nom d’Holly soit visible).

16. Dans le **Centre d’administration Microsoft 365**, sélectionnez **Utilisateurs** dans le volet de navigation et, en dessous, sélectionnez **Utilisateurs actifs**.

17. Dans la fenêtre **Utilisateurs actifs**, lorsque vous pointez la souris sur le **Nom d’affichage** d’un utilisateur, une **icône de clé** apparaît à droite du nom de l’utilisateur. En sélectionnant l’icône de clé, vous pouvez réinitialiser le mot de passe d’un utilisateur. Vous devez réinitialiser les mots de passe d’Adele Vance, Alex Wilber, Joni Sherman, Lynne Robbins et Patti Fernandez, et leur affecter le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD), que vous avez précédemment affecté à Holly Dickson.<br/>

    Placez le pointeur de votre souris sur **Adele Vance** et sélectionnez l’icône de clé qui s’affiche.

18. Dans le volet **Réinitialiser le mot de passe** qui s’affiche pour Adele, décochez la case **Créer automatiquement un mot de passe**. 

19. Dans le champ **Mot de passe** qui s’affiche, entrez le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD). Sélectionnez l’icône d’œil (**Afficher le mot de passe**) à la fin du champ **Mot de passe** pour afficher la valeur que vous avez entrée. Vérifiez que vous avez correctement entré le mot de passe du locataire.

20. Décochez la case **Demander à cet utilisateur de modifier son mot de passe lors de sa première connexion**.

21. Sélectionnez **Réinitialiser le mot de passe**. Si une boîte de dialogue **Enregistrer le mot de passe** s’affiche en haut de l’écran, sélectionnez **Jamais**. Ensuite, sélectionnez **Fermer** dans le volet **Le mot de passe a été réinitialisé**.

22. Répétez les étapes 17 à 21 pour **Alex Wilber**, **Joni Sherman**, **Lynne Robbins** et **Patti Fernandez**. Réinitialisez le mot de passe de chacun d’eux en leur affectant le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD). À l’étape 19, n’oubliez pas d’afficher le mot de passe que vous avez entré pour vérifier qu’il est correct.

23. Restez connecté à LON-CL1 avec le **Centre d’administration Microsoft 365** ouvert dans votre navigateur pour la tâche suivante.


### Tâche 3 : Déployer MFA en utilisant une stratégie d’accès conditionnel

Comme indiqué dans votre formation, il existe trois manières d’implémenter MFA : avec des stratégies d’accès conditionnel, avec des paramètres de sécurité par défaut, et avec l’authentification multifacteur héritée par utilisateur (non recommandée pour les grandes organisations). Dans cet exercice, vous allez activer MFA par le biais d’une stratégie d’accès conditionnel, ce qui est la méthode recommandée par Microsoft. Adatum a chargé Holly d’activer MFA pour tous ses utilisateurs Microsoft 365, à la fois internes et externes. Toutefois, pour tester l’implémentation du projet pilote Microsoft 365 d’Adatum, Holly souhaite faire en sorte que les membres du groupe de projet pilote M365 ne soient pas obligés d’utiliser MFA pour se connecter. Une fois le projet pilote terminé, Holly mettra à jour la stratégie en supprimant l’exclusion de ce groupe de l’exigence MFA. La stratégie inclura également deux autres exigences. Elle exigera MFA pour toutes les applications cloud, même si un utilisateur se connecte à partir d’une localisation approuvée. 

1. Sur la machine virtuelle LON-CL1, le **Centre d’administration Microsoft 365** doit toujours être ouvert dans votre navigateur Microsoft Edge suite à la tâche précédente. Vous devez être connecté à Microsoft 365 en tant que **Holly Dickson**.
   
2. Dans le **Centre d’administration Microsoft 365**, dans la section **Centres d’administration** du volet de navigation, sélectionnez **Identité**. Cela ouvre le centre d’administration Microsoft Entra dans un nouvel onglet de navigateur. 

3. Dans le **centre d’administration Microsoft Entra**, sélectionnez **Protection** dans le volet de navigation, puis **Accès conditionnel**.

4. Dans la page **Accès conditionnel | Vue d’ensemble**, sélectionnez **Stratégies**.

5. Dans la page **Accès conditionnel | Stratégies**, passez en revue les stratégies par défaut disponibles avec votre abonnement Microsoft 365. Dans la barre de menus en haut de la page, sélectionnez **+Nouvelle stratégie**.

6. Dans la fenêtre **Nouvelle stratégie d’accès conditionnel**, entrez **MFA pour tous les utilisateurs Microsoft 365** dans le champ **Nom**.

7. Vous commencerez par définir l’exigence MFA pour les utilisateurs. Sous le groupe **Utilisateurs**, sélectionnez **0 utilisateurs et groupes sélectionnés**. Cela affiche deux onglets : **Inclure** et **Exclure**.

8. Sous l’onglet **Inclure**, sélectionnez **Tous les utilisateurs**. Notez le message d’avertissement qui s’affiche. Vous allez réagir à cet avertissement lors des deux étapes suivantes.

9. Sélectionnez l’onglet **Exclure**. Pour éviter le verrouillage système, comme indiqué dans le message d’avertissement précédent, vous souhaitez exclure vos Administrateurs généraux (en l’occurrence, Holly). Holly souhaite également exclure les autres membres du groupe de projet pilote Microsoft 365 pour des raisons de rapidité lors des tests. Une fois Microsoft 365 en ligne, Holly supprimera le groupe de projet pilote de la liste Exclure dans cette stratégie d’accès conditionnel, et s’exclura simplement elle-même ainsi que plusieurs autres Administrateurs généraux. Mais pour l’instant, Holly souhaite exclure l’ensemble du groupe. <br/>

    Pour cela, sélectionnez **Utilisateurs et groupes**. 

10. Dans la fenêtre **Sélectionner des utilisateurs et des groupes exclus** qui s’affiche, vous souhaitez sélectionner le groupe de projet pilote Microsoft 365. L’onglet **Tous** s’affiche par défaut. Pour trouver rapidement le groupe de projet pilote, sélectionnez l’onglet **Groupes**. Dans la liste des groupes actifs, cochez la case en regard du groupe **Projet pilote M365**, puis sélectionnez le bouton **Sélectionner** en bas de la fenêtre. De retour dans la fenêtre **Nouvelle stratégie d’accès conditionnel**, notez le message qui s’affiche dans la section **Utilisateurs**. 

11. Vous allez maintenant définir l’exigence MFA pour toutes les applications cloud. Dans la section **Ressources cibles**, sélectionnez **Aucune ressource cible sélectionnée**. Cela affiche deux onglets : **Inclure** et **Exclure**.

12. Sous l’onglet **Inclure**, notez que le paramètre par défaut est **Aucune**. Si vous ne mettez pas à jour ce paramètre, aucune application cloud (y compris Microsoft 365) n’exigera MFA. Ainsi, même si vous créez cette stratégie et sélectionnez l’option pour exiger MFA à l’étape 17, mais que vous conservez la valeur **Aucune** pour ce paramètre **Ressources cibles**, aucun utilisateur se connectant à Microsoft 365 ne sera obligé d’utiliser MFA. <br/>

    Sélectionnez l’option **Sélectionner des applications**. Dans le groupe **Sélectionner**, sélectionnez **Aucune**. Dans le volet **Sélectionner des applications cloud** qui s’affiche, faites défiler la liste des applications afin d’afficher toutes les différentes applications pour lesquelles vous pourriez avoir besoin de MFA. **Ne sélectionnez AUCUNE des applications.** Nous vous demandons de faire défiler cette liste simplement pour que vous puissiez constater le degré de précision que vous pouvez appliquer lorsque vous avez besoin d’exiger MFA, dans le cas où vous décideriez de limiter MFA à certaines applications.  <br/>

    Pour Adatum, Holly souhaite exiger MFA pour toutes les applications cloud, ce qui est généralement un scénario métier plus courant que la sélection d’applications spécifiques. Sous l’onglet **Inclure**, sélectionnez l’option **Toutes les applications cloud**. Adatum n’exclura aucune application cloud de l’authentification MFA. Vous pouvez sélectionner l’onglet **Exclure** si vous souhaitez afficher les options qu’il fournit, mais ne sélectionnez pas d’applications cloud pour l’exclusion. 

13. Pour finir, vous allez définir l’exigence MFA pour toutes les localisations de connexion utilisateur. Dans certains scénarios, il se peut que les organisations exigent MFA uniquement si un utilisateur se connecte à partir d’une localisation non approuvée. Toutefois, Adatum exigera MFA pour tous les utilisateurs inclus, quel que soit l’endroit d’où ils se connectent. <br/>

    Sous **Conditions**, sélectionnez **0 condition sélectionnée**. Cette opération affiche une liste des conditions potentielles que la stratégie vérifiera. Pour cet exercice de labo, sous la condition **Localisations**, sélectionnez **Non configuré**. Cette opération affiche un bouton bascule **Configurer** et deux onglets, **Inclure** et **Exclure**.

14. Définissez le bouton bascule **Configurer** sur **Oui** afin d’activer les deux onglets. 

15. Sous l’onglet **Inclure**, vérifiez que l’option **Tous les emplacements** est sélectionnée (sélectionnez-la si nécessaire). Sélectionnez l’onglet **Exclure**. Si votre organisation reconnaît des adresses IP ou des plages d’adresses spécifiques comme étant « approuvées », vous pouvez exclure l’exigence MFA si un utilisateur se connecte à partir de l’une de ces localisations. Toutefois, Adatum souhaite exiger MFA pour tous les utilisateurs, quelle que soit leur localisation. Cela comprend les connexions des utilisateurs internes et externes. Vérifiez que l’option **Emplacements sélectionnés** est sélectionnée, et que la section **Sélectionner** indique **Aucun**. En ne spécifiant aucun emplacement sélectionné, ce paramètre garantit qu’aucun emplacement n’est exclu de MFA. 

16. Dans la section **Contrôles d’accès**, dans le groupe **Octroyer**, sélectionnez **0 contrôles sélectionnés**. Un volet **Octroyer** s’affiche.

17. Dans le volet **Octroyer** qui s’affiche, vérifiez que l’option **Autoriser l’accès** est sélectionnée (sélectionnez-la si nécessaire). Ensuite, cochez la case **Exiger une authentification multifacteur**. Notez tous les autres contrôles d’accès disponibles qui peuvent être activés avec cette stratégie. Pour cette stratégie, vous n’aurez besoin que de MFA. Sélectionnez le bouton **Sélectionner** en bas du volet **Octroyer** afin de fermer le volet. 

18. En bas de la fenêtre **Nouveau**, dans le champ **Activer la stratégie**, sélectionnez **Activée**.

19. Notez l’option qui apparaît en bas de la page pour vous avertir de ne pas vous bloquer vous-même. Sélectionnez l’option **Je comprends que mon compte sera affecté par cette stratégie. Continuer malgré tout.** En fait, Holly ne sera pas affectée, car elle est membre du groupe de projet pilote M365, qui est exclu de cette stratégie.

20. Sélectionnez le bouton **Créer** pour créer la stratégie.

21. Dans la fenêtre **Accès conditionnel | Stratégies** qui s’affiche, vérifiez que la stratégie **MFA pour tous les utilisateurs Microsoft 365** s’affiche et que son **État** est **Activée**.

22. Restez connecté à LON-CL1 avec tous vos onglets de navigateur Microsoft Edge ouverts pour la tâche suivante.

### Tâche 4 : Tester MFA pour un utilisateur inclus et un utilisateur exclu

Pour tester la stratégie d’accès conditionnel que vous venez de créer, vous allez vous déconnecter de Microsoft 365 en tant que Holly, puis vous reconnecter en tant qu’Adele Vance. Adele n’est pas membre du groupe de projet pilote M365. Microsoft Entra devrait donc exiger qu’elle utilise MFA lors de la connexion. Après vous être connecté en tant qu’Adele et avoir vérifié le bon fonctionnement de MFA, vous vous déconnecterez en tant qu’Adele, puis vous vous reconnecterez en tant qu’Holly. Holly étant membre du groupe de projet pilote M365 qui a été exclu de l’utilisation de MFA dans la stratégie d’accès conditionnel, vous ne devriez pas avoir à utiliser MFA lors de la connexion en tant qu’Holly. 

**Important :** Pour implémenter MFA, vous devez utiliser votre téléphone mobile afin de recevoir un code de vérification, que vous entrerez dans votre locataire en guise de deuxième forme d’authentification. Si vous n’avez pas de téléphone, vous pouvez quand même tester votre stratégie d’accès conditionnel. Lors de la connexion en tant qu’Adele Vance, le système vous oblige à vous connecter avec une deuxième forme d’authentification. À ce stade, vous pouvez simplement annuler votre connexion, puis vous reconnecter en tant qu’Holly, pour qui MFA ne sera pas nécessaire. Vous ne terminerez pas la connexion MFA, mais vous vérifierez tout de même que le système vous oblige à l’utiliser lors de la tentative de connexion en tant qu’Adele.

1. Sur la machine virtuelle LON-CL1, le **Centre d’administration Microsoft 365** doit toujours être ouvert dans votre navigateur Microsoft Edge suite à la tâche précédente. Vous devez être connecté à Microsoft 365 en tant que **Holly Dickson**. Vous allez commencer par vous déconnecter de Microsoft 365. Sous l’onglet **Centre d’administration Microsoft 365**, sélectionnez le nom d’Holly dans le coin supérieur droit de votre navigateur. Dans la fenêtre **Holly Dickson** qui s’affiche, sélectionnez **Se déconnecter**. <br/>
    
    **Important :** Une fois déconnecté, fermez votre session de navigateur pour effacer votre cache, puis ouvrez une nouvelle session de navigateur Microsoft Edge. 
    
2. Sélectionnez l’icône **Edge** dans votre barre des tâches pour ouvrir une nouvelle session de navigateur. Dans votre navigateur, accédez à la page **Accueil Microsoft Office** en entrant l’URL suivante dans la barre d’adresse : **https://portal.office.com/** 

3. Dans la fenêtre **Choisir un compte**, sélectionnez **Utiliser un autre compte**. 

4. Dans la fenêtre **Se connecter**, entrez **AdeleV@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire fourni par votre fournisseur d’hébergement de labo), puis sélectionnez **Suivant**. Dans la fenêtre **Entrer le mot de passe**, entrez le mot de passe d’administration fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD), puis sélectionnez **Se connecter**.

5. MFA étant activé pour tous les utilisateurs sauf les membres du groupe de projet pilote M365 (dont Adele n’est pas membre), une fenêtre **Plus d’informations requises** s’affiche. Cliquez sur **Suivant**. Cela provoque l’affichage de la page **Microsoft Authenticator**, qui constitue le point de départ de la connexion avec MFA. <br/>

    **Important :** Si vous n’avez pas de téléphone, vous ne pourrez pas aller plus loin dans le processus de connexion en tant qu’Adele. Même si vous ne pouvez pas terminer la connexion, vous avez confirmé que la première partie de votre stratégie d’accès conditionnel fonctionnait, car elle exige qu’Adele se connecte à l’aide de MFA. À ce stade, fermez votre session de navigateur Edge, puis passez à l’étape 15, durant laquelle vous vous reconnecterez en tant qu’Holly.

6. Dans la page **Microsoft Authenticator** qui s’affiche, vous pouvez télécharger cette application mobile ou utiliser une autre méthode pour la vérification MFA. Pour les besoins de ce labo, nous vous recommandons d’utiliser votre téléphone mobile afin de ne pas avoir à installer l’application Microsoft Authenticator, que vous n’utiliserez peut-être plus après ce cours. Sélectionnez l’option **Je veux configurer une autre méthode** en bas de la page (**Important :** Ne confondez PAS ce lien avec le lien **Je souhaite utiliser une autre application d’authentification** qui se trouve au-dessus). 

7. Dans la boîte de dialogue **Choisir une autre méthode** qui s’affiche, sélectionnez la flèche déroulante dans le champ **Quelle méthode voulez-vous utiliser ?**, sélectionnez **Téléphone**, puis **Confirmer**. 

8. Dans la fenêtre **Téléphone** qui s’affiche, dans le champ **Quel numéro de téléphone voulez-vous utiliser ?**, sélectionnez votre pays ou région puis, dans le champ en regard de celui-ci, entrez votre numéro de téléphone (au format **nnn-nnn-nnnn**). Vérifiez que l’option **Recevoir un code** est sélectionnée, puis sélectionnez **Suivant**.

9. Récupérez le code de vérification à partir du SMS envoyé à votre téléphone.

10. Dans la fenêtre **Téléphone**, entrez le code de vérification à six chiffres dans le champ de code, puis sélectionnez **Suivant**. Lorsque la fenêtre **Téléphone** affiche un message indiquant que votre téléphone a été enregistré avec succès, sélectionnez **Suivant**.

11. Dans la page **Opération réussie**, sélectionnez **Terminé**.

12. Si une boîte de dialogue **Rester connecté ?** s’affiche, cochez la case **Ne plus afficher**, puis sélectionnez **Oui**. 

13. Sous l’onglet **Accueil | Microsoft 365**, sélectionnez l’icône **Word** affichée dans la colonne des icônes d’application sur le côté gauche de l’écran. **Microsoft Word** s’ouvre dans un nouvel onglet de navigateur. Cela confirme que vous pouvez accéder à une application Microsoft 365 après vous être connecté à l’aide de MFA.  <br/>

    **Important :** Vous avez maintenant vérifié que la première partie de la stratégie d’accès conditionnel que vous avez créée fonctionne. La stratégie impose qu’un utilisateur qui n’est pas membre de l’équipe de projet pilote Microsoft 365 se connecte à l’aide de MFA. Vous avez vérifié que cela fonctionnait lorsque vous vous êtes connecté en tant qu’Adele. Vous allez maintenant vous déconnecter en tant qu’Adele et vous reconnecter en tant qu’Holly, afin de vérifier que la deuxième partie de la stratégie d’accès conditionnel fonctionne également. Vous ne devriez PAS avoir à utiliser MFA lors de la connexion en tant qu’Holly, puisqu’elle est membre du groupe de projet pilote M365, qui est exclu de l’exigence MFA dans la stratégie d’accès conditionnel.

14. Sous l’onglet **Centre d’administration Microsoft 365**, sélectionnez l’icône du compte d’Holly dans le coin supérieur droit de votre navigateur. Dans la fenêtre **Adele Vance** qui s’affiche, sélectionnez **Se déconnecter**. <br/>
    
    **Important :** Une fois déconnecté, fermez votre session de navigateur pour effacer votre cache, puis ouvrez une nouvelle session de navigateur Microsoft Edge. 
    
15. Sélectionnez l’icône **Edge** dans votre barre des tâches pour ouvrir une nouvelle session de navigateur. Dans votre navigateur, accédez à la page **Accueil Microsoft Office** en entrant l’URL suivante dans la barre d’adresse : **https://portal.office.com/** 

16. Dans la fenêtre **Choisir un compte**, sélectionnez **Holly@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire fourni par votre fournisseur d’hébergement de labo), puis **Suivant**. Dans la fenêtre **Entrer le mot de passe**, entrez le mot de passe d’administration fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD), puis sélectionnez **Se connecter**.

17. L’authentification MFA étant requise pour tous les utilisateurs sauf les membres de l’équipe de projet pilote M365 (dont Holly est membre), elle ne sera pas obligatoire ici. MFA n’étant pas requis, le système affiche la page **Accueil Microsoft 365**. Sélectionnez l’icône **Administrateur** pour accéder au **Centre d’administration Microsoft 365**. <br/>

    **Important :** Vous avez maintenant vérifié que la deuxième partie de la stratégie d’accès conditionnel que vous avez créée fonctionne. La stratégie exclut les membres du groupe de projet pilote Microsoft 365 de l’obligation de se connecter à l’aide de MFA. Holly est membre de ce groupe, et elle n’a pas été contrainte de se connecter à l’aide de MFA.

18. Restez connecté à LON-CL1 avec le **Centre d’administration Microsoft 365** ouvert dans votre navigateur.

  

### Tâche 5 : Déployer le verrouillage intelligent Microsoft Entra

Le Directeur technique d’Adatum vous a demandé de déployer le verrouillage intelligent Microsoft Entra, qui aide à bloquer les acteurs malveillants qui essaient de deviner les mots de passe de vos utilisateurs ou d’utiliser des méthodes de force brute pour être admis dans votre réseau. Le verrouillage intelligent peut reconnaître les connexions provenant d’utilisateurs validés, et les traiter différemment de celles des attaquants et autres sources inconnues. 

Le Directeur technique tient beaucoup à l’implémentation du verrouillage intelligent, car il empêchera les attaquants de pénétrer dans le système, tout en permettant aux utilisateurs d’Adatum de continuer à accéder à leurs comptes et de travailler. Le Directeur technique a demandé à Holly de configurer le verrouillage intelligent afin que les utilisateurs ne puissent pas utiliser le même mot de passe plusieurs fois et qu’ils ne puissent pas utiliser de mots de passe considérés comme trop simplistes ou courants. 

**Remarque :** Vous pouvez tenir à jour les paramètres de stratégie de verrouillage de compte dans l’Éditeur de gestion des stratégies de groupe et dans Microsoft Entra par le biais de sa fonctionnalité de verrouillage intelligent. Cette tâche de labo montre comment accéder à chaque méthode, mais vous mettrez à jour les paramètres de verrouillage intelligent uniquement dans Microsoft Entra. Vous devez utiliser le contrôleur de domaine de l’entreprise (LON-DC1) pour accéder à l’Éditeur de gestion des stratégies de groupe. 

1. Lors des tâches précédentes, vous avez travaillé dans LON-CL1. Durant cette tâche, vous allez travailler à partir du contrôleur de domaine d’Adatum, LON-DC1. <br/>

    Basculez vers **LON-DC1**.

2. Sur **LON-DC1**, vous devez sélectionner **Ctrl+Alt+Supprimer** pour vous connecter (votre formateur vous expliquera comment trouver cette option dans votre environnement de machine virtuelle). Connectez-vous à LON-DC1 sous le compte d’administrateur Adatum local créé par votre fournisseur d’hébergement de labo (**Administrateur**) avec le mot de passe **Pa55w.rd**.

3. Sur LON-DC1, le **Gestionnaire de serveur** est lancé automatiquement au démarrage. Dans le **Gestionnaire de serveur**, sélectionnez **Outils** dans la barre de menus en haut à droite puis, dans le menu déroulant, sélectionnez **Gestion des stratégies de groupe**. Agrandissez la fenêtre **Gestion des stratégies de groupe** qui s’affiche.

4. Vous souhaitez modifier la stratégie de groupe qui inclut la stratégie de verrouillage de compte de votre organisation. Si nécessaire, dans l’arborescence de la console racine dans le volet gauche, développez **Forest:Adatum.com**, **Domaines**, puis **Adatum.com**.  <br/>

    Sous **Adatum.com**, cliquez avec le bouton droit sur **Stratégie de domaine** par défaut, puis sélectionnez **Modifier** dans le menu.

5. Agrandissez la fenêtre **Éditeur de gestion des stratégies de groupe** qui s’affiche.

6. Dans l’arborescence **Stratégie de domaine par défaut** dans le volet gauche, sous **Configuration ordinateur**, développez **Stratégies**, **Paramètres Windows**, **Paramètres de sécurité**, puis **Stratégies de compte**.

7. Dans le dossier **Stratégies de compte**, sélectionnez **Stratégie de verrouillage de compte**.

8. Comme vous pouvez le voir dans le volet droit, aucun des paramètres de verrouillage intelligent n’a été défini. Au lieu de tenir à jour ces paramètres de verrouillage dans l’Éditeur de gestion des stratégies de groupe, vous allez utiliser le Centre d’administration Microsoft Entra. Bien que vous puissiez utiliser l’Éditeur de gestion des stratégies de groupe, cette méthode est généralement utilisée dans les environnements Active Directory locaux. Nous vous avons montré cet éditeur afin que vous sachiez que cette alternative existe. Toutefois, pour les organisations qui utilisent strictement des services cloud tels que Microsoft 365 ou qui trouvent l’utilisation du centre d’administration Microsoft Entra beaucoup plus conviviale que l’accès à l’Éditeur de gestion des stratégies de groupe, l’utilisation du **centre d’administration Microsoft Entra** pour attribuer des valeurs correspondantes dans le contexte Entra ID est préférable. <br/>  

    Sachez également que le comportement de verrouillage et les options de personnalisation diffèrent entre les deux méthodes. Avec l’Éditeur de gestion des stratégies de groupe, vous avez un contrôle plus précis sur les paramètres de stratégie, notamment Seuil de verrouillage, Durée du verrouillage et Réinitialiser le compteur de verrouillage du compte au bout de. Toutefois, l’application de cette méthode nécessite une connaissance de la stratégie de groupe et de l’administration Active Directory. À l’inverse, la stratégie de verrouillage de compte dans Microsoft Entra ne peut pas être personnalisée de manière aussi étendue. En revanche, elle est plus facile à utiliser, même si elle n’offre pas certaines des options de réglage fin disponibles dans Stratégie de groupe. <br/>

    Pour Adatum, Holly a choisi d’utiliser le centre d’administration Microsoft Entra pour configurer la stratégie de verrouillage de compte de l’entreprise. Dans la barre des tâches en bas de votre écran, sélectionnez l’icône **Microsoft Edge**. Si nécessaire, agrandissez votre fenêtre de navigateur quand elle s’ouvre.

9. Dans votre navigateur Edge, accédez à la page **Accueil Microsoft 365** en entrant l’URL suivante dans la barre d’adresse : **https://portal.office.com** 

10. Dans la boîte de dialogue **Se connecter**, vous devez vous connecter en tant qu’Holly Dickson. Entrez **Holly@xxxxxZZZZZZ.onmicrosoft.com**, où xxxxxZZZZZZ est le préfixe de locataire attribué par votre fournisseur d’hébergement de labo. Cliquez sur **Suivant**. <br/>

11. Dans la boîte de dialogue **Entrer le mot de passe**, entrez le **Mot de passe d’administration** unique fourni par votre fournisseur d’hébergement de labo, puis sélectionnez **Se connecter**.

12. Dans la boîte de dialogue **Rester connecté ?**, cochez la case **Ne plus afficher**, puis sélectionnez **Oui**. Dans la boîte de dialogue **Enregistrer le mot de passe** qui s’affiche, sélectionnez **Jamais**.

13. Si une boîte de dialogue **Bienvenue dans Microsoft 365** s’affiche au milieu de l’écran, il n’existe aucune option pour la fermer. Au lieu de cela, à droite de la fenêtre, sélectionnez deux fois l’icône de flèche vers l’avant (**>**), puis sélectionnez l’icône de coche pour parcourir les diapositives de cette fenêtre de message. 

14. Si une fenêtre **Rechercher d’autres applications** ou **Créer avec Microsoft 365** s’affiche, sélectionnez le **X** dans le coin supérieur droit de la fenêtre pour la fermer. 

15. Dans la page **Bienvenue dans Microsoft 365**, dans la liste des icônes d’application qui s’affiche dans le volet gauche, sélectionnez **Administrateur**. Cela ouvre le **Centre d’administration Microsoft 365** dans un nouvel onglet de navigateur. 

16. Dans le **centre d’administration Microsoft 365**, dans le volet de navigation, sélectionnez **Afficher tout**. Sous **Centres d’administration**, sélectionnez **Identité** afin d’afficher le **Centre d’administration Microsoft Entra** dans un nouvel onglet.

17. Dans le **centre d’administration Microsoft Entra**, sélectionnez **Protection** dans le volet de navigation, puis **Méthodes d’authentification**.

18. Dans la page **Méthodes d’authentification | Stratégies**, dans le volet central, dans la section **Gérer**, sélectionnez **Protection par mot de passe**.

19. Dans la fenêtre **Méthodes d’authentification | Protection par mot de passe**, dans le volet de détails à droite, entrez les informations suivantes :

    - Dans la section **Verrouillage intelligent personnalisé** :

        - **Seuil de verrouillage** : ce champ indique le nombre d’échecs de connexion autorisés avant qu’un compte ne soit verrouillé. La valeur par défaut est de 10. À des fins de test, Adatum vous a demandé d’affecter la valeur **3** à ce paramètre.

        - **Durée du verrouillage en secondes** : il s’agit de la durée en secondes de chaque verrouillage. La valeur par défaut est 60 secondes (une minute). Adatum vous a demandé de régler ce paramètre sur **90** secondes.

    - Dans la section **Mots de passe interdits personnalisés** :

        - **Appliquer la liste personnalisée** : sélectionnez **Oui**

        - **Liste de mots de passe interdits personnalisés :** entrez les valeurs suivantes (appuyez sur Entrée après avoir entré chaque valeur, afin qu’elles se trouvent sur des lignes distinctes) :

            - **Password01**

            - **F00tball01**

            - **Se@Hawks1**

            - **Never4get!!**

    - Dans la section **Mode**, sélectionnez **Appliqué**.

20. Sélectionnez **Enregistrer** dans la barre de menus en haut de la page.

21. Vous devez maintenant tester la fonctionnalité de mot de passe interdit. Sélectionnez l’icône utilisateur d’Holly Dickson dans le coin supérieur droit de l’écran puis, dans le menu qui s’affiche, sélectionnez **Afficher le compte**. 

22. Dans la fenêtre **Mon compte** qui s’affiche, dans la vignette **Mot de passe**, sélectionnez** MODIFIER LE MOT DE PASSE**.

23. Un nouvel onglet s’ouvre et affiche la fenêtre **Modifier le mot de passe**. Dans le champ **Ancien mot de passe**, entrez le mot de passe existant d’Holly, qui est le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD). <br/>

    Entrez **Never4get !!** dans les champs **Créer un mot de passe** et **Confirmer le nouveau de mot de passe**, puis sélectionnez **Envoyer**. Notez le message d’erreur que vous recevez.

24. Dans votre navigateur, fermez l’onglet **Modifier le mot de passe**. 

25. Vous devez maintenant tester la fonctionnalité de seuil de verrouillage. Sélectionnez l’icône utilisateur d’Holly Dickson dans le coin supérieur droit de l’écran puis, dans le menu qui s’affiche, sélectionnez **Se déconnecter**.  

26. Une fois que vous êtes déconnecté en tant qu’Holly, la fenêtre **Choisir un compte** s’affiche sous l’onglet **Se connecter à Microsoft Entra**. En guise de meilleure pratique lors de la déconnexion d’un service en ligne Microsoft et du basculement d’un utilisateur vers un autre, fermez tous vos onglets de navigateur sauf l’onglet **Se déconnecter** ou **Se connecter**. Dans le cas présent, fermez les autres onglets maintenant et laissez l’onglet **Se connecter** ouvert.  <br/>

    Dans la fenêtre **Choisir un compte**, sélectionnez **Utiliser un autre compte**. 

27. Dans la fenêtre **Se connecter**, vous allez vous connecter en tant que Patti Fernandez. Entrez **pattif@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire qui vous a été attribué par votre fournisseur d’hébergement de labo), puis sélectionnez **Suivant**. 

28. Dans la fenêtre **Entrer un mot de passe**, entrez une combinaison aléatoire de lettres, puis sélectionnez **Se connecter**. Notez le message d’erreur de mot de passe non valide qui s’affiche. 

    Répétez cette étape deux autres fois. 
    
    Étant donné que vous avez défini le **Seuil de verrouillage** sur **3**, vous devez recevoir un message d’erreur indiquant que ce compte est verrouillé après la troisième tentative de connexion ayant échoué. <br/>

    **Remarque :** Si vous ne recevez pas ce message de verrouillage après la troisième tentative, cela signifie que le système n’a pas encore fini de propager ce seuil de verrouillage dans tout le service. Les modifications seront effectives après quelques minutes. Patientez quelques minutes, puis reconnectez-vous avec un mot de passe incorrect. Les tests de ce labo ont entraîné des résultats variables. La modification se propage parfois presque immédiatement, auquel cas vous êtes verrouillé après la troisième tentative de connexion. D’autres fois, il a fallu entre cinq et dix minutes avant l’affichage du message de verrouillage. Poursuivez ce processus jusqu’à ce que vous receviez le message de verrouillage, après quoi le compte de Patti sera temporairement verrouillé afin d’empêcher tout accès non autorisé.

29. Vous ne pourrez plus vous reconnecter en tant que Patti tant que la **durée de verrouillage de 90 secondes** que vous avez définie précédemment ne se sera pas écoulée. <br/>

    Une fois que vous avez été verrouillé, attendez 90 secondes, puis reconnectez-vous en tant que **pattif@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire qui vous a été attribué par votre fournisseur d’hébergement de labo). Dans le champ **Mot de passe**, entrez le mot de passe de Patti, qui est le même **Mot de passe d’administration** que celui fourni par votre fournisseur d’hébergement de labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD). Vérifiez que vous êtes en mesure de vous connecter correctement en tant que Patti.

30. Une fois la connexion réussie, vous pouvez fermer toutes les applications ouvertes. Il s’agit là de votre dernier exercice de labo utilisant le contrôleur de domaine LON-DC1.
 
# Passer au labo 2 – Exercice 2
