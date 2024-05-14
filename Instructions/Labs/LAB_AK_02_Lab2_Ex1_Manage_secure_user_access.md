# Parcours d’apprentissage 2 – Labo 2 – Exercice 1 – Gérer l’accès utilisateur sécurisé 

Les organisations doivent s’assurer que l’accès à leurs données d’entreprise sur Microsoft 365 est toujours sécurisé. Microsoft 365, tout comme Copilot pour Microsoft 365, affiche souvent des données sensibles et confidentielles, notamment des e-mails, des documents, des informations client et de la propriété intellectuelle. L’accès non autorisé à Microsoft 365 peut engendrer des violations de données, une usurpation d’identité et d’autres activités malveillantes. En sécurisant l’accès des utilisateurs, les organisations peuvent empêcher les personnes non autorisées d’accéder aux données de l’entreprise, et potentiellement d’en faire un usage incorrect ou de provoquer une fuite de données, lors de l’utilisation de Microsoft 365 et Copilot pour Microsoft 365.

Dans l’exercice de labo suivant, vous allez continuer à jouer le rôle d’Holly Dickson, la nouvelle administratrice Microsoft 365 d’Adatum. Vous allez effectuer plusieurs fonctions de gestion des utilisateurs pour préparer Adatum en vue du prochain déploiement de Copilot pour Microsoft 365. Vous commencerez par créer un compte d’utilisateur Microsoft 365 pour Holly, à qui sera attribué le rôle Administrateur général Microsoft 365. 

**Remarque :** L’environnement de machine virtuelle fourni par votre fournisseur d’hébergement de labo comporte plus de 20 comptes d’utilisateur Microsoft 365, ainsi qu’un grand nombre de comptes d’utilisateur locaux. Plusieurs des comptes d’utilisateur Microsoft 365 existants seront utilisés dans les labos de ce cours. Bien que le compte Administrateur MOD ait été créé par votre fournisseur d’hébergement de labo, vous allez tout de même créer le compte d’utilisateur d’Holly Dickson, car l’attribution du rôle Administrateur général Microsoft 365 à plusieurs utilisateurs est une pratique recommandée. Cela vous offrira également l’opportunité de créer un compte d’utilisateur Microsoft 365, si vous n’êtes pas familiarisé avec le processus.

Holly a été chargée par le Directeur technique d’Adatum de déployer l’authentification multifacteur (MFA) Microsoft Entra et le verrouillage intelligent de Microsoft Entra. Ces fonctionnalités aideront à renforcer la gestion des mots de passe au sein de l’organisation en vue de l’adoption de Copilot pour Microsoft 365. Quant au verrouillage intelligent, vous le déploierez à l’aide de Gestion des stratégies de groupe. 

**NOTIFICATION MFA IMPORTANTE :** En raison d’une modification récente apportée par Microsoft aux locataires d’évaluation Microsoft 365 utilisés dans ce cours, nous avons dû apporter une modification correspondante à ce labo. <br/>

**Conception d’origine du labo :** Cet exercice de labo a été initialement conçu pour vous faire créer, en tant que Holly Dickson, une stratégie d’accès conditionnel qui active la MFA sur Adatum dans la tâche 3. Il s’agit de la méthode recommandée pour activer la MFA, comme vous l’avez appris dans la partie cours de cette formation. Dans la tâche 3, vous avez créé une stratégie d’accès conditionnel qui a activé la MFA pour tous les utilisateurs, à l’exception des membres du groupe Projet pilote Microsoft 365 que vous avez créé dans un exercice antérieur. Pour tester votre stratégie dans la tâche 4, vous vous connectiez ensuite en tant qu’Adele Vance. Étant donné qu’Adele n’était pas membre du groupe Projet pilote, vous deviez terminer le processus MFA pour vous connecter à Microsoft 365. Vous vous déconnectiez ensuite en tant qu’Adele et vous vous reconnectiez en tant que Holly Dickson. Étant donné que Holly est membre du groupe Projet pilote Microsoft 365, elle n’avait qu’à entrer son nom d’utilisateur et son mot de passe. Elle ne devait pas effectuer une deuxième forme d’authentification à l’aide de la MFA. Ces deux étapes de la tâche 4 vérifiaient votre stratégie d’accès conditionnel, c’est-à-dire que seuls les utilisateurs qui ne faisaient pas partie du groupe Projet pilote devaient utiliser la MFA. 

L’autre conséquence de cette stratégie est que vous n’aviez pas besoin d’utiliser la MFA lors de la connexion au reste des labos, car les autres utilisateurs de test étaient également membres du groupe Projet pilote Microsoft 365. Bien que les exclusions de la MFA puissent être définies pour des raisons pratiques en fonction des besoins métier d’une entreprise, les entreprises doivent évaluer soigneusement les risques associés et appliquer des exclusions avec modération, toujours visant à s’aligner sur les meilleures pratiques de sécurité. Ce labo vous faisait créer l’exclusion pour le groupe Projet pilote pour deux raisons : pour vous permettre d’apprendre à créer une exclusion dans une stratégie d’accès conditionnel, et comme moyen de gagner du temps dans les labos d’entraînement lors de la connexion en tant qu’utilisateur de test. <br/>

**Modification du locataire Microsoft :** Cela dit, Microsoft a depuis créé une stratégie d’accès conditionnel dans le locataire d’évaluation Microsoft 365 utilisé dans ce labo qui nécessite la MFA pour tous les comptes d’utilisateur. Malheureusement, la stratégie d’accès conditionnel de Microsoft est prioritaire sur la stratégie d’accès conditionnel que vous allez créer dans ce labo pour exclure un groupe spécifique. Les stratégies d’accès conditionnel dans Microsoft Entra sont évaluées ensemble et la stratégie de priorité la plus élevée qui s’applique à un utilisateur est appliquée. S’il existe plusieurs stratégies avec la même priorité, ce qui est le cas ici, la stratégie la plus restrictive s’applique. Dans ce cas, la stratégie de Microsoft est plus restrictive que la stratégie que vous créez, car la vôtre inclut des exceptions utilisateur. <br/>

**Impact sur cet exercice de labo :** Même si Microsoft applique la MFA dans ce locataire d’évaluation par le biais d’une stratégie d’accès conditionnel, vous allez toujours créer votre propre stratégie d’accès conditionnel comme conçu à l’origine dans la tâche 3. La tâche a été légèrement modifiée à partir de sa version d’origine afin que vous puissiez voir la stratégie que Microsoft a intégrée au locataire d’évaluation. Mais à part cela, la stratégie que vous allez créer n’a pas changé à partir de sa conception d’origine : elle requiert la MFA pour tous les utilisateurs, à l’exception des membres du groupe Projet pilote Microsoft 365. Toutefois, étant donné que la stratégie intégrée par Microsoft dans le locataire d’évaluation du labo est la plus restrictive des deux stratégies, cette stratégie est prioritaire lors de l’application. Il y a deux conséquences à ce scénario. Tout d’abord, la stratégie de Microsoft vous oblige à utiliser la MFA lors de la connexion en tant qu’utilisateur de test dans le reste de ces labos. Deuxièmement, étant donné que Microsoft Entra ignore votre stratégie, il ne sert à rien de la tester. Par conséquent, nous avons supprimé la tâche 4 d’origine qui testait la stratégie d’accès conditionnel que vous avez créée. Maintenant, même si vous ne pouvez pas tester votre stratégie, nous voulons toujours la créer dans la tâche 3 afin que vous obteniez cette expérience pour vos déploiements réels. 

### Tâche 1 – Créer un compte d’utilisateur pour l’Administrateur Microsoft 365 d’Adatum

Holly Dickson est la nouvelle administratrice Microsoft 365 d’Adatum. Aucun compte d’utilisateur Microsoft 365 n’ayant été configuré pour elle, elle s’est initialement connectée à Microsoft 365 sous le compte de l’Administrateur MOD (l’Administrateur général par défaut) dans le labo précédent. Durant cette tâche, vous continuerez à être connecté en tant qu’Administrateur MOD, et créerez un compte d’utilisateur Microsoft 365 pour Holly. Vous attribuerez également le rôle Administrateur général Microsoft 365 au compte d’Holly. Ce rôle fournira à Holly les autorisations nécessaires pour effectuer toutes les fonctions administratives dans Microsoft 365. Après cette tâche, vous vous connecterez à l’aide du nouveau compte de Holly et effectuerez tous les labos restants sous ce compte. 

**Note relative aux licences :** Avant de créer le compte de Holly, vous devez d’abord vérifier le nombre de licences disponibles. Ce faisant, vous remarquerez que bien que votre locataire de labo fournisse 20 licences Microsoft 365 E5 et 20 licences Enterprise Mobility + Security E5, toutes ces licences ont déjà été affectées aux comptes d’utilisateur existants créés par votre fournisseur d’hébergement de labo. Par conséquent, vous devez d’abord annuler l’affectation d’une licence de chaque type d’un utilisateur existant afin de pouvoir les affecter à Holly.

**Important :** En guise de meilleure pratique dans votre déploiement réel, vous devez toujours noter les informations d’identification du premier compte d’Administrateur général (dans ce labo, il s’agit du compte Administrateur MOD, dont le nom d’utilisateur est admin@xxxxxZZZZZZ.onmicrosoft.com, où xxxxxZZZZZZ est le préfixe de locataire attribué par votre fournisseur d’hébergement de labo). Vous devez conserver ces informations de compte en lieu sûr pour des raisons de sécurité. **Ce compte doit être une identité NON personnalisée** qui détient les privilèges les plus élevés possibles dans un locataire. Il ne doit **pas** être activé pour MFA, car il n’est pas personnalisé. Le nom d’utilisateur et le mot de passe de ce premier compte d’Administrateur général étant couramment partagés entre plusieurs utilisateurs, ce compte est une cible idéale pour les attaques. Par conséquent, il est toujours recommandé aux organisations de créer des comptes d’administrateur de service personnalisés (par exemple, un administrateur Exchange, un administrateur SharePoint, et ainsi de suite) et de limiter au plus le nombre d’Administrateurs généraux personnels. Les Administrateurs généraux personnels que vous créez dans votre déploiement réel doivent chacun être mappés à un utilisateur unique (par exemple, Holly Dickson), et l’authentification multifacteur (MFA) Microsoft Entra doit être appliquée pour chacun d’eux. Cela étant dit, Microsoft a déjà activé la MFA par défaut pour tous les utilisateurs de votre locataire d’évaluation Microsoft 365.

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

### Tâche 2 – Ajouter Holly au groupe Projet pilote Microsoft 365

Une fois la tâche précédente terminée, vous devriez toujours être connecté au **Centre d’administration Microsoft 365** sous le compte **Administrateur MOD**. Durant cette tâche, vous allez commencer à implémenter le projet pilote Microsoft 365 d’Adatum en tant qu’Holly Dickson, la nouvelle administratrice Microsoft 365 d’Adatum. Par conséquent, vous commencerez cette tâche en vous déconnectant de Microsoft 365 en tant qu’Administrateur MOD et en vous reconnectant à l’aide du compte d’Holly. 

Dans une tâche précédente, vous avez créé un groupe Microsoft 365 pour les membres de l’équipe Projet pilote d’Adatum afin de pouvoir créer un thème personnalisé pour ce groupe. Dans cette tâche, vous allez ajouter Holly à ce groupe. 

1. Sur la machine virtuelle LON-CL1, le **Centre d’administration Microsoft 365** doit toujours être ouvert dans votre navigateur Microsoft Edge suite à la tâche précédente. Vous devez être connecté à Microsoft 365 en tant qu’**Administrateur MOD**. <br/>

    Sous l’onglet **Centre d’administration Microsoft 365**, dans le coin supérieur droit de l’écran, notez la présence du nom de l’Administrateur MOD et d’une icône de mégaphone. La présence du nom est due au thème personnalisé que vous avez créé dans l’exercice de labo précédent, qui a été associé à un groupe d’utilisateurs de projet pilote Microsoft 365 incluant l’Administrateur MOD. Gardez cela à l’esprit lorsque vous vous reconnecterez en tant que Holly Dickson. <br/>

    Sélectionnez l’icône d’utilisateur pour l’**Administrateur MOD** dans le coin supérieur droit de votre navigateur. Dans la fenêtre **Administrateur MOD** qui s’affiche, sélectionnez **Se déconnecter**. <br/>
    
    **Important :** Lors du basculement d’un compte d’utilisateur vers un autre, vous devez fermer tous vos onglets de navigateur à l’exception de l’onglet **Se déconnecter**. Il s’agit d’une meilleure pratique qui permet d’éviter toute confusion grâce à la fermeture des fenêtres associées à l’utilisateur précédent. Une fois que vous êtes déconnecté du compte Administrateur MOD, fermez tous les autres onglets du navigateur, à l’exception de l’onglet **Se déconnecter**. 
    
2. Dans votre navigateur Microsoft Edge, sous l’onglet **Se déconnecter**, entrez l’URL suivante dans la barre d’adresse pour vous reconnecter à Microsoft 365 : **https://portal.office.com**. 

3. Dans la fenêtre **Choisir un compte**, seul le compte d’administrateur du locataire d’Administrateur MOD (le compte admin@xxxxxZZZZZZ.onmicrosoft.com) que vous venez de déconnecter est visible. Sélectionnez **Utiliser un autre compte**. 

4. Dans la fenêtre **Se connecter**, entrez **Holly@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire fourni par votre fournisseur d’hébergement de labo). Cliquez sur **Suivant**.

5. Dans la fenêtre **Entrer le mot de passe**, entrez le même **Mot de passe administratif** que celui fourni par le fournisseur d’hébergement de votre labo pour le compte d’administrateur du locataire (c’est-à-dire le compte Administrateur MOD), que vous avez également affecté au compte de Holly, puis sélectionnez **Se connecter**. Si nécessaire, complétez le processus de connexion MFA.  <br/>

    **Remarque :** À partir de ce stade, Holly et Administrateur MOD sont les seuls utilisateurs à qui le **mot de passe administratif** fourni par le fournisseur d’hébergement de votre labo a été affecté. Tous les autres utilisateurs ont reçu le **mot de passe utilisateur** fourni par le fournisseur d’hébergement de votre labo.

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

Comme indiqué dans votre formation, il existe trois manières d’implémenter MFA : avec des stratégies d’accès conditionnel, avec des paramètres de sécurité par défaut, et avec l’authentification multifacteur héritée par utilisateur (non recommandée pour les grandes organisations). Dans cet exercice, vous allez activer MFA par le biais d’une stratégie d’accès conditionnel, ce qui est la méthode recommandée par Microsoft. Adatum a chargé Holly d’activer la MFA pour tous ses utilisateurs Microsoft 365, à la fois internes et externes. Toutefois, pour tester l’implémentation du projet pilote Microsoft 365 d’Adatum, Holly souhaite faire en sorte que les membres du groupe de projet pilote M365 ne soient pas obligés d’utiliser MFA pour se connecter. Une fois le projet pilote terminé, Holly mettra à jour la stratégie en supprimant l’exclusion de ce groupe de l’exigence MFA. La stratégie inclura également deux autres exigences. Elle exigera MFA pour toutes les applications cloud, même si un utilisateur se connecte à partir d’une localisation approuvée. 

1. Sur la machine virtuelle LON-CL1, le **Centre d’administration Microsoft 365** doit toujours être ouvert dans votre navigateur Microsoft Edge suite à la tâche précédente. Vous devez être connecté à Microsoft 365 en tant que **Holly Dickson**.
   
2. Dans le **Centre d’administration Microsoft 365**, dans la section **Centres d’administration** du volet de navigation, sélectionnez **Identité**. Cela ouvre le centre d’administration Microsoft Entra dans un nouvel onglet de navigateur. Dans la fenêtre **Choisir un compte** qui s’affiche, sélectionnez le **compte d’Holly Dickson**.

3. Dans le **centre d’administration Microsoft Entra**, sélectionnez **Protection** dans le volet de navigation, puis **Accès conditionnel**.

4. Sur la page **Accès conditionnel | Vue d’ensemble**, sélectionnez **Stratégies** dans le volet de navigation central.

5. Dans la page **Accès conditionnel | Stratégies**, passez en revue les stratégies par défaut disponibles avec votre abonnement Microsoft 365. Notez la stratégie intitulée **Authentification multifacteur pour les partenaires et les fournisseurs Microsoft**. Il s’agit de la stratégie d’accès conditionnel créée par Microsoft qui requiert la MFA pour tous les utilisateurs sur toutes les applications cloud. Sélectionnez cette stratégie pour voir comment Microsoft applique la MFA à tous les utilisateurs de ce locataire d’évaluation.

6. Sur la page **Authentification multifacteur pour les partenaires et les fournisseurs Microsoft**, sous le groupe **Utilisateurs**, sélectionnez **Tous les utilisateurs inclus et utilisateurs spécifiques exclus**. Cela affiche deux onglets : **Inclure** et **Exclure**.

7. Sous l’onglet **Inclure**, notez que **Tous les utilisateurs** est sélectionné. Juste pour satisfaire votre curiosité, essayons de changer cette politique en désactivant la MFA. Sélectionnez **Aucun**, puis sélectionnez le bouton **Enregistrer**. <br/>

    **Notez ce qui s’est passé :** Une boîte de message s’affiche brièvement en haut de la page indiquant **Échec de mise à jour de l’authentification multifacteur pour les partenaires et les fournisseurs Microsoft**. Le système vous a ensuite renvoyé à la page **Accès conditionnel | Stratégies**. Si vous n’avez pas vu ce message, répétez de nouveau cette étape.  <br/>

    La même chose se produit si, sous l’onglet **Inclure**, vous sélectionnez **Utilisateurs et groupes** et vous sélectionnez des utilisateurs ou des groupes spécifiques plutôt que tous les utilisateurs. En fait, Microsoft a intégré un pare-feu de sécurité afin que si vous essayez d’apporter des modifications à cette stratégie, un échec se produit lorsque vous tentez d’enregistrer la stratégie. 

8. Microsoft a créé une exclusion dans cette stratégie. Examinons à présent cette exclusion. Sur la page **Stratégies**. sélectionnez de nouveau la stratégie **Authentification multifacteur pour les partenaires et les fournisseurs Microsoft**, puis sélectionnez **Tous les utilisateurs inclus et des utilisateurs spécifiques exclus**. Cette fois, sélectionnez l’onglet **Exclure**. 

9. Notez que Microsoft a coché la case **Rôles d’annuaire**. Sélectionnez le champ sous cette option. Dans le menu des rôles que vous pouvez choisir d’exclure de la MFA, le seul rôle sélectionné par Microsoft est **Comptes de synchronisation d’annuaires**. Ce rôle est automatiquement attribué au compte de service Microsoft Entra Connect Sync et n’est pas destiné à être utilisé à d’autres fins. Il s’agit d’un rôle spécial qui dispose d’autorisations uniquement pour effectuer des tâches de synchronisation d’annuaires. Ce rôle est essentiel pour la synchronisation d’un AD DS (Active Directory Domain Services) local avec Microsoft Entra ID, ce qui fournit la coexistence d’identités. Microsoft ne requiert pas la MFA pour ce rôle, car le service de synchronisation doit s’exécuter en continu sans interruptions pouvant être provoquées par des invites MFA. En outre, ce compte de service est configuré avec des autorisations très limitées et n’est pas utilisé pour les connexions interactives, ce qui réduit le risque de sécurité.   <br/>

    Comme à l’étape précédente où vous avez essayé de modifier le paramètre MFA pour n’inclure aucun utilisateur ou des utilisateurs sélectionnés, si vous essayez de sélectionner d’autres rôles pour contourner la configuration MFA, vous rencontrerez la même erreur indiquant **Échec de la mise à jour de l’authentification multifacteur pour les partenaires et les fournisseurs Microsoft** lorsque vous essayez d’enregistrer la stratégie. Le pare-feu de sécurité de Microsoft n’autorise aucune modification apportée à cette stratégie d’accès conditionnel qui affecte les comptes qui doivent utiliser la MFA.

10. Si vous avez toujours la page **Authentification multifacteur pour les partenaires et les fournisseurs Microsoft** ouverte, sélectionnez le **X** dans le coin supérieur pour la fermer et revenir à la page **Accès conditionnel | Stratégies**. Ou si vous avez essayé d’enregistrer une modification de la stratégie, votre tentative a échoué et vous devez déjà être revenu sur la page **Accès conditionnel | Stratégies**.

11. Sur la page **Accès conditionnel | Stratégies**, dans la barre de menus en haut de la page, sélectionnez **+Nouvelle stratégie**.

12. Dans la fenêtre **Nouvelle stratégie d’accès conditionnel**, entrez **MFA pour tous les utilisateurs Microsoft 365** dans le champ **Nom**.

13. Vous commencerez par définir l’exigence MFA pour les utilisateurs. Sous le groupe **Utilisateurs**, sélectionnez **0 utilisateurs et groupes sélectionnés**. Cela affiche deux onglets : **Inclure** et **Exclure**.

14. Sous l’onglet **Inclure**, sélectionnez **Tous les utilisateurs**. Notez le message d’avertissement qui s’affiche. Vous allez réagir à cet avertissement lors des deux étapes suivantes.

15. Sélectionnez l’onglet **Exclure**. Pour éviter le verrouillage système, comme indiqué dans le message d’avertissement précédent, vous souhaitez exclure vos Administrateurs généraux (en l’occurrence, Holly). Holly souhaite également exclure les autres membres du groupe de projet pilote Microsoft 365 pour des raisons de rapidité lors des tests. Une fois Microsoft 365 en ligne, Holly supprimera le groupe de projet pilote de la liste Exclure dans cette stratégie d’accès conditionnel, et s’exclura simplement elle-même ainsi que plusieurs autres Administrateurs généraux. Mais pour l’instant, Holly souhaite exclure l’ensemble du groupe. <br/>

    Pour cela, sélectionnez **Utilisateurs et groupes**. 

16. Dans la fenêtre **Sélectionner des utilisateurs et des groupes exclus** qui s’affiche, vous souhaitez sélectionner le groupe de projet pilote Microsoft 365. L’onglet **Tous** s’affiche par défaut. Pour trouver rapidement le groupe de projet pilote, sélectionnez l’onglet **Groupes**. Dans la liste des groupes actifs, cochez la case en regard du groupe **Projet pilote M365**, puis sélectionnez le bouton **Sélectionner** en bas de la fenêtre. De retour dans la fenêtre **Nouvelle stratégie d’accès conditionnel**, notez le message qui s’affiche dans la section **Utilisateurs**. 

17. Vous allez maintenant définir l’exigence MFA pour toutes les applications cloud. Dans la section **Ressources cibles**, sélectionnez **Aucune ressource cible sélectionnée**. Cela affiche deux onglets : **Inclure** et **Exclure**.

18. Sélectionnez le champ déroulant **Sélectionner à quoi cette stratégie s’applique** pour afficher les différentes options du menu déroulant. Sélectionnez **Applications cloud**. 

19. Sous l’onglet **Inclure**, notez que le paramètre par défaut est **Aucune**. Si vous n’aviez pas modifié ce paramètre, aucune application cloud (y compris Microsoft 365) n’exigerait la MFA. Ainsi, même si vous aviez créé cette stratégie et sélectionné l’option pour exiger la MFA de tous les utilisateurs, mais que vous aviez conservé la valeur de **Ressources cibles** sur **Aucune**, aucun utilisateur se connectant à Microsoft 365 ne serait obligé d’utiliser la MFA. <br/>

    Sous l’onglet **Inclure**, sélectionnez l’option **Sélectionner les applications**. Cette opération affiche deux sections : **Modifier le filtre** et **Sélectionner**. Dans la section **Sélectionner**, sélectionnez **Aucun**. Dans le volet **Sélectionner des applications cloud** qui s’affiche, faites défiler la liste des applications afin d’afficher toutes les différentes applications pour lesquelles vous pourriez avoir besoin de MFA. **Ne sélectionnez AUCUNE des applications.** Nous vous demandons de faire défiler cette liste simplement pour que vous puissiez constater le degré de précision que vous pouvez appliquer lorsque vous avez besoin d’exiger MFA, dans le cas où vous décideriez de limiter MFA à certaines applications.  <br/>

    Pour Adatum, Holly souhaite exiger MFA pour toutes les applications cloud, ce qui est généralement un scénario métier plus courant que la sélection d’applications spécifiques. Sous l’onglet **Inclure**, sélectionnez l’option **Toutes les applications cloud**. Adatum n’exclura aucune application cloud de l’authentification MFA. Vous pouvez sélectionner l’onglet **Exclure** si vous souhaitez afficher les options proposées. Il fonctionne essentiellement de la même façon que l’onglet **Inclure**. Vous pouvez afficher cet onglet, mais ne sélectionnez aucune application cloud pour l’exclusion. 

20. Pour finir, vous allez définir l’exigence MFA pour toutes les localisations de connexion utilisateur. Dans certains scénarios, il se peut que les organisations exigent MFA uniquement si un utilisateur se connecte à partir d’une localisation non approuvée. Toutefois, Adatum souhaite exiger la MFA pour tous les utilisateurs inclus, quel que soit l’endroit où ils se connectent. <br/>

    Sous **Conditions**, sélectionnez **0 condition sélectionnée**. Cette opération affiche une liste des conditions potentielles que la stratégie vérifiera. Pour cet exercice de labo, sous la condition **Localisations**, sélectionnez **Non configuré**. Cette opération affiche un bouton bascule **Configurer** et deux onglets, **Inclure** et **Exclure**. Les deux onglets sont actuellement désactivés.

21. Définissez le bouton bascule **Configurer** sur **Oui** afin d’activer les deux onglets. 

22. Sous l’onglet **Inclure**, vérifiez que l’option **Tous les emplacements** est sélectionnée (sélectionnez-la si nécessaire). Sélectionnez l’onglet **Exclure**. Si votre organisation reconnaît des adresses IP ou des plages d’adresses spécifiques comme étant « approuvées », vous pouvez exclure l’exigence MFA si un utilisateur se connecte à partir de l’une de ces localisations. Toutefois, Adatum souhaite exiger la MFA pour toutes les tentatives de connexion utilisateur, quelle que soit leur localisation. Cela comprend les connexions des utilisateurs internes et externes. Vérifiez que l’option **Emplacements sélectionnés** est sélectionnée, et que la section **Sélectionner** indique **Aucun**. En ne spécifiant aucun emplacement sélectionné, ce paramètre garantit qu’aucun emplacement n’est exclu de MFA. 

23. Dans la section **Contrôles d’accès**, dans le groupe **Octroyer**, sélectionnez **0 contrôles sélectionnés**. Un volet **Octroyer** s’affiche.

24. Dans le volet **Octroyer** qui s’affiche, vérifiez que l’option **Autoriser l’accès** est sélectionnée (sélectionnez-la si nécessaire). Ensuite, cochez la case **Exiger une authentification multifacteur**. Notez tous les autres contrôles d’accès disponibles qui peuvent être activés avec cette stratégie. Pour cette stratégie, vous n’aurez besoin que de MFA. Sélectionnez le bouton **Sélectionner** en bas du volet **Octroyer** afin de fermer le volet. 

25. En bas de la fenêtre **Nouveau**, dans le champ **Activer la stratégie**, sélectionnez **Activée**.

26. Notez l’option qui apparaît en bas de la page pour vous avertir de ne pas vous bloquer vous-même. Sélectionnez l’option **Je comprends que mon compte sera affecté par cette stratégie. Continuer malgré tout.** En fait, Holly ne sera pas affectée, car elle est membre du groupe de projet pilote M365, qui est exclu de cette stratégie.

27. Sélectionnez le bouton **Créer** pour créer la stratégie.

28. Dans la fenêtre **Accès conditionnel | Stratégies** qui s’affiche, vérifiez que la stratégie **MFA pour tous les utilisateurs Microsoft 365** s’affiche et que son **État** est **Activée**.

29. Restez connecté à LON-CL1 avec tous vos onglets de navigateur Microsoft Edge ouverts pour la tâche suivante.

**Remarque :** Conformément à la discussion précédente, il n’existe aucun moyen de tester votre stratégie d’accès conditionnel dans le locataire d’évaluation Microsoft 365 actuel. La stratégie d’accès conditionnel de Microsoft requiert la MFA pour tous les utilisateurs. Lorsque vous avez plusieurs stratégies qui requièrent la MFA, la stratégie la plus restrictive s’applique. Dans ce cas, la stratégie de Microsoft est plus restrictive que celle que vous venez de créer, qui incluait des exceptions pour les membres du groupe Projet pilote. Même si vous ne pouvez pas tester votre stratégie à l’aide de ce locataire d’évaluation, nous vous encourageons à utiliser cette expérience de création d’une stratégie d’accès conditionnel pour exiger la MFA dans vos déploiements Microsoft 365 réels.


### Tâche 4 : Déployer le verrouillage intelligent Microsoft Entra

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

11. Dans la boîte de dialogue **Entrer le mot de passe**, entrez le **Mot de passe d’administration** unique fourni par votre fournisseur d’hébergement de labo, puis sélectionnez **Se connecter**. Si nécessaire, complétez le processus de connexion MFA.

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

28. Dans la fenêtre **Entrer un mot de passe**, entrez une combinaison aléatoire de lettres et de chiffres, puis sélectionnez **Se connecter**. Notez le message d’erreur de mot de passe non valide qui s’affiche. 

    Répétez cette étape deux autres fois. 
    
    Étant donné que vous avez défini le **Seuil de verrouillage** sur **3**, vous devez recevoir un message d’erreur indiquant que ce compte est verrouillé après la troisième tentative de connexion ayant échoué. <br/>

    **Remarque :** Si vous ne recevez pas ce message de verrouillage après la troisième tentative, cela signifie que le système n’a pas encore fini de propager ce seuil de verrouillage dans tout le service. Les modifications seront effectives après quelques minutes. Patientez quelques minutes, puis reconnectez-vous avec un mot de passe incorrect. Les tests de ce labo ont entraîné des résultats variables. La modification se propage parfois presque immédiatement, auquel cas vous êtes verrouillé après la troisième tentative de connexion. D’autres fois, il a fallu entre cinq et dix minutes avant l’affichage du message de verrouillage. Poursuivez ce processus jusqu’à ce que vous receviez le message de verrouillage, après quoi le compte de Patti sera temporairement verrouillé afin d’empêcher tout accès non autorisé.

29. Vous ne pourrez plus vous reconnecter en tant que Patti tant que la **durée de verrouillage de 90 secondes** que vous avez définie précédemment ne se sera pas écoulée. <br/>

    Une fois que vous avez été verrouillé, attendez 90 secondes, puis reconnectez-vous en tant que **pattif@xxxxxZZZZZZ.onmicrosoft.com** (où xxxxxZZZZZZ est le préfixe de locataire qui vous a été attribué par votre fournisseur d’hébergement de labo). Dans le champ **Mot de passe**, entrez le mot de passe de Patti, qui est le **mot de passe utilisateur** fourni par le fournisseur d’hébergement de votre labo. Si nécessaire, complétez le processus de connexion MFA. Vérifiez que vous êtes en mesure de vous connecter correctement en tant que Patti.

30. Une fois la connexion réussie, vous pouvez fermer toutes les applications ouvertes. Il s’agit là de votre dernier exercice de labo utilisant le contrôleur de domaine LON-DC1.
 
# Passer au labo 2 – Exercice 2
