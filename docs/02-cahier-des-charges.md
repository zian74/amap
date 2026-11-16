# Cahier des charges (v1.2 – 16/11/2026)

*Version mise en forme du cahier des charges initial (fichier org de l'AMAP). Elle intègre les réponses à l'audit et les règles tirées des documents existants : contrat type 2026 du Corti des Aravis, code de bonne conduite 2026, bulletin d'adhésion et planning 2026.*

**Faire évoluer ce document.** Ce fichier est la seule version du cahier des charges. On le modifie sur GitHub (crayon ✏️, puis « Create a new branch and start a pull request »), et la modification est relue avant d'être fusionnée. Chaque exigence porte un numéro (`ADH-07`, `REF-13`…) :

- une nouvelle exigence prend le numéro suivant de sa partie, même si elle est insérée au milieu ;
- un numéro n'est jamais réutilisé : une exigence abandonnée reste, barrée, avec la mention « (abandonnée le …) » ;
- l'état de réalisation de chaque exigence est suivi dans [04-suivi-conformite.md](04-suivi-conformite.md).

| Préfixe | Partie                       | Préfixe | Partie                     |
|---------|------------------------------|---------|----------------------------|
| `ROL`   | §3 Rôles et droits           | `DIS`   | §9 Distributions           |
| `PUB`   | §4 Partie publique           | `DOC`   | §10 Documents PDF          |
| `CON`   | §4.1 Formulaire de contact   | `MEL`   | §11 E-mails automatiques   |
| `ADH`   | §5 Espace adhérent           | `NF`    | §12 Non fonctionnel        |
| `REF`   | §6 Espace référent           | `HEB`   | §13 Hébergement et budget  |
| `PRO`   | §7 Espace producteur         | `GIT`   | §14 Organisation GitHub    |
| `ADM`   | §8 Administration            | `EVO`   | §15 Évolutions futures     |
| `Q`     | §16 Points à préciser        |         |                            |

## 1. Présentation du projet

### 1.1 Contexte

-   AMAP « Le Panier des Aravis », Thônes (74). Site actuel sur Wix : <https://www.lepanierdesaravis.fr>
-   Environ **100 adhérents**, **9 producteurs sous contrat**, environ **20 producteurs en commandes groupées**, environ **10 référents**.
-   Une distribution **tous les 15 jours** (semaines calendaires impaires), le samedi de 9h30 à 10h30, à l'EHPAD « Le Chant du Fier » à Thônes. Les permanents arrivent à 8h30.
-   Les contrats couvrent l'année civile (de janvier à décembre). Les adhésions ont lieu début décembre.

### 1.2 Objectifs

1.  Remplacer le site Wix par une **application web sur mesure, open source**, hébergée chez Infomaniak.
2.  Donner à chaque personne un **compte personnel** avec un rôle : adhérent, producteur, référent ou administrateur.
3.  **Gérer en ligne** les contrats (« commandes standard »), les commandes groupées, les distributions, les permanences et le pointage des paiements.
4.  Permettre aux référents de **créer une page producteur ou une commande groupée en remplissant un formulaire**, sans connaissances techniques.
5.  **Générer automatiquement** les documents de distribution (récapitulatifs producteurs, feuilles d'émargement) et les e-mails (confirmations, rappels).
6.  Rendre l'application **réutilisable par d'autres AMAP** (nom, logo, couleurs, lieux, règles paramétrables).

### 1.3 Décisions prises

| Sujet                   | Décision                                                                                          |
|-------------------------|---------------------------------------------------------------------------------------------------|
| Type de solution        | Application 100 % sur mesure                                                                      |
| Technologies            | Laravel (PHP) + Filament (back-office) + MySQL/MariaDB                                            |
| Hébergement             | Hébergement Web Infomaniak (offre Web + Mail)                                                     |
| Code source             | Open source, dans une *organisation* GitHub appartenant à l'association                           |
| Paiements des contrats  | **L'AMAP n'encaisse rien** : chèques ou virements à l'ordre du producteur, avec pointage en ligne |
| Paiement de l'adhésion  | En ligne possible (HelloAsso recommandé), sinon chèque ou virement                                |
| Reprise des données Wix | Aucune. Chaque adhérent crée son compte. Seuls les contenus et les images sont repris.            |
| Identité visuelle       | Logo et couleurs conservés, présentation modernisée                                               |
| Blog                    | Mis de côté (hors V1)                                                                             |

### 1.4 Hors périmètre de la V1 (évolutions futures, §15)

Scénarios d'e-mail marketing avancés, tutoriels interactifs, vidéos pédagogiques, blog, paiement en ligne des contrats.

## 2. Glossaire

| Terme                       | Définition                                                                                                   |
|-----------------------------|--------------------------------------------------------------------------------------------------------------|
| Administrateur              | Paramètre et gère le site. Attribut les rôles aux utilisateurs.
| Adhérent (amapien·ne)       | Personne à jour de sa cotisation annuelle                                                                    |
| Producteur                  | Paysan ou paysanne partenaire. Il a un espace et des produits.                                               |
| Référent                    | Adhérent bénévole qui suit un ou plusieurs producteurs ou commandes groupées (le « super-utilisateur »)      |
| Contrat (commande standard) | Engagement d'un adhérent auprès d'un producteur pour une saison : produits, quantités, fréquence, échéancier |
| Offre de contrat            | Ce que propose un producteur pour une saison (période, produits, prix, fréquences et échéanciers possibles)  |
| Commande groupée            | Commande ponctuelle, avec une date d'ouverture et une date de clôture, livrée lors d'une distribution donnée |
| Distribution                | Une date de remise des paniers (n°, date, lieu, horaires, producteurs présents)                              |
| Permanence                  | Participation d'un adhérent à l'organisation d'une distribution (installation, distribution, nettoyage)      |
| Responsable de distribution | Personne qui coordonne une distribution et envoie le rappel aux permanents                                   |
| Échéance                    | Un règlement prévu (montant, mois d'encaissement, mode chèque ou virement, bénéficiaire)                     |
| Pointage                    | Le fait de marquer une échéance comme reçue ou encaissée                                                     |
| Feuille d'émargement        | Liste imprimable « adhérent / quantité / signature », une par produit et par distribution                    |

## 3. Rôles et droits

Les rôles se cumulent : un référent est aussi adhérent, et un administrateur a tous les droits.

| N°     | Fonction                                               | Visiteur | Adhérent     | Producteur             | Référent             | Admin      |
|--------|--------------------------------------------------------|----------|--------------|------------------------|----------------------|------------|
| ROL-01 | Voir la vitrine, les producteurs, le calendrier public | ✔        | ✔           | ✔                      | ✔                   | ✔          |
| ROL-02 | Formulaire de contact, demande d'adhésion              | ✔        | ✔           | ✔                      | ✔                   | ✔          |
| ROL-03 | Gérer son compte                                       |          | ✔           | ✔                      | ✔                   | ✔          |
| ROL-04 | Souscrire des contrats, commander en commande groupée  |          | ✔           |                        | ✔                   | ✔          |
| ROL-05 | Voir ses commandes, son calendrier, ses échéances      |          | ✔           |                        | ✔                   | ✔          |
| ROL-06 | S'inscrire aux permanences                             |          | ✔           |                        | ✔                   | ✔          |
| ROL-07 | Voir le pointage de ses propres paiements              |          | ✔ (lecture) |                        | ✔                   | ✔          |
| ROL-08 | Voir les récapitulatifs de commandes qui le concernent |          |             | ✔ (les siennes)        | ✔ (ses producteurs) | ✔          |
| ROL-09 | Pointer les paiements                                  |          |             | ✔ (les siens)          | ✔ (ses producteurs) | ✔          |
| ROL-10 | Créer ou modifier une page producteur et ses produits  |          |             | (à décider : sa fiche) | ✔ (ses producteurs) | ✔ (tous)   |
| ROL-11 | Créer ou gérer une commande groupée                    |          |             |                        | ✔ (les siennes)     | ✔ (toutes) |
| ROL-12 | Inscrire un adhérent à une permanence                  |          |             |                        | ✔                   | ✔          |
| ROL-13 | Gérer les distributions et valider leur clôture        |          |             |                        |                     | ✔          |
| ROL-14 | Modifier les pages d'information et la page d'accueil  |          |             |                        |                     | ✔          |
| ROL-15 | Gérer les comptes et les rôles, modérer, paramètres    |          |             |                        |                     | ✔          |
| ROL-16 | Envoyer des e-mails groupés (lettre d'information)     |          |             |                        | (à décider)         | ✔          |

**ROL-17** Règle par défaut : un référent modifie **les producteurs et commandes groupées qu'il a créés ou qui lui sont attribués**. L'administrateur peut attribuer plusieurs référents à un même producteur.

## 4. Partie publique (vitrine)

| N°     | Page                      | Contenu                                                                                                                  | Modifiable par |
|--------|---------------------------|--------------------------------------------------------------------------------------------------------------------------|----------------|
| PUB-01 | Accueil                   | Accroche, prochaine distribution, actualités et période d'adhésion, commande groupée ouverte (titre), bouton « Adhérer » | Admin          |
| PUB-02 | L'AMAP                    | Fusion de « Qu'est-ce qu'une AMAP » et « Notre fonctionnement » + charte et code de bonne conduite en PDF                | Admin          |
| PUB-03 | Nos producteurs           | Liste en vignettes, filtrable par type de produit → fiche producteur générée automatiquement                             | Référents      |
| PUB-04 | Fiche producteur          | Présentation, photos, produits, offres de contrat (sans les prix si on le souhaite), liens, labels                       | Référents      |
| PUB-05 | Calendrier                | Dates des distributions et des prochaines permanences d'adhésion (pour venir rencontrer l'AMAP)                          | Admin          |
| PUB-06 | Adhérer                   | Étapes, cotisation, dates des permanences d'adhésion, paiement en ligne de la cotisation, création de compte             | Admin          |
| PUB-07 | Contact / Nous rencontrer | Formulaire de contact, adresse postale, lieu de distribution (carte), prochaines permanences                             | Admin          |
| PUB-08 | Pages légales             | Mentions légales, politique de confidentialité, gestion des cookies (si nécessaire)                                      | Admin          |

### 4.1 Formulaire de contact

-   **CON-01** Champs : Nom, Prénom, E-mail, « Je suis » (adhérent·e / producteur·rice / personne intéressée par l'AMAP — formulation à affiner), Sujet (liste déroulante), Message.
    **[Remarques : Il est peut-être préférable que les adhérents +/- les producteurs passent par la fonction "Echanges" de l'espace adhérents pour communiquer]**
-   **CON-02** Les sujets proposés sont : Adhésion, Commandes et contrats, Permanences, Devenir producteur partenaire, Site internet, Autre. Chaque sujet est **envoyé à l'adresse ou à l'alias adapté** (§13).
-   **CON-03** Anti-spam : champ caché piège et limitation du nombre d'envois, sans captcha intrusif. La vérification de l'adresse e-mail est inutile pour un simple message. Un accusé de réception est envoyé à l'expéditeur.
-   **CON-04** Quand l'expéditeur choisit « Je suis adhérent·e », le formulaire propose un lien vers « Échanges entre adhérents » (ADH-23), après connexion.

## 5. Espace adhérent

### 5.1 Tableau de bord

**ADH-01** Il affiche : la prochaine distribution (ce que je récupère, si je suis de permanence), les commandes groupées ouvertes, mes échéances à venir et une alerte si mon adhésion est à renouveler.

### 5.2 Compte et adhésion

-   **ADH-02** Création du compte (e-mail et mot de passe), validation de l'adresse e-mail, mot de passe oublié.
-   **ADH-03** Profil : nom, prénom, adresse, téléphones, **consentement au partage du téléphone** avec les autres adhérents (pour les remplacements de permanence).
-   **ADH-04** Adhésion annuelle : montant paramétrable (20 € en 2026). Paiement en ligne (HelloAsso) ou déclaration « chèque » ou « virement », avec pointage par l'admin.
-   **ADH-05** Les fonctions de commande ne sont disponibles **qu'une fois l'adhésion validée**.

### 5.3 Parcours « commande standard » (souscrire un contrat)

1.  **ADH-06** Tableau de bord → « Mes contrats » → liste des **offres de contrat ouvertes**, par producteur.
2.  **ADH-07** Pour chaque producteur : liste des produits et de leurs prix. Pour chaque produit, l'adhérent choisit :
    -   la **quantité** ;
    -   la **fréquence**, parmi les choix autorisés par le producteur : toutes les distributions, 1 sur 2 (série A ou série B), [ou dates choisies à la main ;]
    -   le **calendrier personnel** et le **panier** se mettent à jour en direct, avec une icône par produit sur chaque date.
3.  **ADH-08** « Voir mon panier » : récapitulatif **par producteur**, avec le nombre de livraisons, le total, et le **choix de l'échéancier** (par exemple 1, 3 ou 6 chèques, selon ce que le producteur autorise) et du mode (chèque ou virement). L'écran affiche l'ordre du chèque ou le RIB, ainsi que les mois d'encaissement.
4.  **ADH-09** Validation → **e-mail de confirmation** avec le récapitulatif, l'échéancier et le contrat du producteur en PDF.
5.  **ADH-10** Après validation, l'adhérent peut modifier son contrat jusqu'à la date de clôture des souscriptions. Après cette date, seul le référent peut le modifier.

### 5.4 Parcours « commande groupée »

1.  **ADH-11** Tableau de bord → « Commandes groupées » → commandes ouvertes (souvent une seule), avec la date de clôture et la date de livraison.
2.  **ADH-12** L'adhérent sélectionne des produits et des quantités. Le panier et le calendrier se mettent à jour.
3.  **ADH-13** « Voir mon panier » : récapitulatif par producteur, total, modalités de paiement indiquées par le référent.
4.  **ADH-14** Validation → e-mail de confirmation. L'adhérent peut modifier sa commande jusqu'à la clôture.

### 5.5 Calendrier et permanences

-   **ADH-15** Calendrier personnel : mes livraisons par date (contrats et commandes groupées) et mes permanences.
-   **ADH-16** Calendrier général des distributions, avec les producteurs présents à chaque date.
-   **Inscription aux permanences** (règles du code de bonne conduite) :
    -   **ADH-17** les adhérents avec contrats choisissent **3 vœux de dates** et l'AMAP en retient 1 ou 2 ; les adhérents qui ne font que des commandes groupées choisissent 1 date ;
    -   **ADH-18** chaque distribution a une équipe de 5 à 6 personnes, plus un **responsable de distribution** ;
    -   **ADH-19** en cas d'empêchement : échange avec un autre adhérent ou remplaçant extérieur (nom, téléphone, e-mail), saisi sur le site ;
    -   **ADH-20** une absence non remplacée est consignée (liste des « adhérents sollicitables ») ;
    -   **ADH-21** le référent ou l'admin peut inscrire ou déplacer un adhérent.
-   Rappels automatiques (§11, MEL-07).

### 5.6 Mes paiements

**ADH-22** Il affiche la liste de mes échéances par producteur et leur statut de pointage (en lecture seule) : attendu, reçu ou encaissé.

### 5.7 Échanges entre adhérents (retours du 16/11/2026)

-   **ADH-23** Page « Échanges entre adhérents » dans l'espace adhérent. L'adhérent écrit :
    -   à **tous les adhérents** (réservé aux adhérents à jour de cotisation, nombre de messages par jour limité) ;
    -   au **référent d'un producteur**, choisi dans une liste (par exemple pour une question sur ses produits) ;
    -   au **bureau** (administrateurs).

    Les adresses ne sont pas affichées : le message part par e-mail, et « Répondre » écrit directement à l'expéditeur. Les derniers messages à tous restent consultables sur la page. Chaque adhérent peut refuser de recevoir les messages des autres adhérents (case dans « Mon compte »). Fonctionnalité de base, à enrichir.

### 5.8 Calendrier enrichi et commandes liées à un engagement (retours du 16/11/2026)

-   **ADH-24** Pendant la souscription, le calendrier montre aussi ce que l'adhérent reçoit déjà chez d'autres producteurs : icônes du contrat en cours en grand, autres commandes en petit. Au survol (ou au toucher) d'une date, une infobulle détaille tout ce qui est commandé.
-   **ADH-25** « Mon calendrier » commence par la même vue, en petit : un mois à la fois, avec des flèches pour passer au mois suivant, et un bouton pour l'agrandir en fenêtre avec tous les mois. Elle garde toutes les icônes et les infobulles. La liste date par date reste en dessous.
-   **ADH-26** **Commande liée à un engagement** (exemple : boulangerie). L'adhérent s'engage sur un montant pour l'année (REF-22), puis commande ses produits tous les 15 jours (REF-24). S'il est engagé, il n'a rien à régler : le producteur déduit la commande du solde, et le site affiche une **estimation du solde restant**. Un adhérent non engagé peut aussi commander, pour du ponctuel : le site calcule le montant à régler **sur place** au producteur, à la distribution.

## 6. Espace référent (back-office)

### 6.1 Ajouter ou modifier un producteur

**REF-01** Le bouton « Ajouter un producteur » ouvre un formulaire :

-   nom de la ferme, nom des producteurs, commune et adresse, téléphone, e-mail, site web et réseaux sociaux, SIRET (pour le contrat) ;
-   types de produits (cases à cocher), labels (Bio, Demeter, Nature & Progrès…) ;
-   présentation (éditeur de texte simple), photos (glisser-déposer, recadrage automatique), documents PDF (gamme, tarifs, conseils) ;
-   mode de participation : contrat, commandes groupées ou les deux ;
-   publication : brouillon ou publié.

**REF-02** À l'enregistrement, **la fiche publique est générée automatiquement** et le producteur apparaît dans la liste et dans le « magasin ». **REF-03** Si le producteur a une adresse e-mail, le référent peut l'inviter à créer son compte producteur.

### 6.2 Produits et offres de contrat (le « magasin » standard)

-   **REF-04** **Produits** : nom, description, photo, unité (panier, pièce, kg, litre…), prix unitaire, actif ou inactif. On peut **dupliquer** un produit.
-   **REF-20** **Icône de chaque produit**, choisie dans une banque d'icônes (légumes, fruits, animaux, pain…), avec une **couleur** et un type de produit pour la légende (par exemple une chèvre noire pour « Fromage de chèvre »). Une même icône dans une même couleur ne peut pas servir à deux producteurs : deux chevriers prennent la chèvre en noir et en blanc. Dans le sélecteur, les icônes prennent la couleur choisie.
-   **REF-21** Une **photo** peut remplacer l'icône sur la fiche producteur et l'écran de souscription. Le calendrier garde toujours l'icône.
-   **REF-22** **Montants d'engagement** : au lieu de produits, le référent peut proposer plusieurs montants qu'il définit (par exemple 50 €, 100 €, 200 €), pour les producteurs dont les produits varient dans l'année. Le montant vaut pour toute la saison, quel que soit le nombre de livraisons.
-   **Offre de contrat** (par producteur et par saison) :
    -   **REF-05** période (par exemple du 6 juin au 27 novembre) et distributions concernées, cochées automatiquement selon la présence du producteur ;
    -   **REF-06** produits proposés et prix ;
    -   **REF-07** fréquences autorisées ;
    -   **REF-08** échéanciers autorisés (nombre de chèques et mois d'encaissement, par exemple 3 chèques en mars, juin et septembre, ou 6 chèques de juin à novembre) ;
    -   **REF-09** ordre des chèques et RIB pour les virements ;
    -   **REF-10** texte du contrat (articles), à partir d'un modèle ;
    -   **REF-11** **date et heure d'ouverture et de clôture des souscriptions**.
-   **REF-12** Le bouton « Dupliquer l'offre de l'an dernier » permet de préparer la saison suivante.
-   **REF-23** Case à cocher « le producteur peut télécharger la liste des adhérents » : prénom, nom, e-mail et engagement des souscripteurs, en PDF ou en tableur (PRO-05). Le référent peut toujours la télécharger.
-   **REF-25** Les contrats sont en général **annuels**, mais certains sont **trimestriels** (ou semestriels) : le choix de la durée remplit les dates, et la duplication décale l'offre d'un an, de six mois ou de trois mois.
-   **REF-26** **Nombre de contrats limité** (facultatif) : premier arrivé, premier servi. L'écran indique les places restantes, puis « Complet ».
-   **REF-27** **Annonce de l'ouverture** des souscriptions par e-mail (MEL-11), au choix : à tous les adhérents, ou seulement à ceux qui ont un contrat en cours avec ce producteur.

### 6.3 Créer une commande groupée

-   **REF-13** Formulaire : titre, producteur ou fournisseur, description, produits (nom, conditionnement, prix, quantité maximale par adhérent ou au total), **date et heure d'ouverture et de clôture**, distribution de livraison, modalités de paiement.
-   **REF-14** Ouverture et fermeture automatiques.
-   **REF-15** Option : **e-mail d'annonce aux adhérents** à l'ouverture.
-   **REF-16** Le bouton « Dupliquer » permet de relancer une commande d'une année sur l'autre.
-   **REF-24** Une commande groupée peut être **liée à un engagement** (ADH-26) et **répétée à chaque distribution** du producteur, avec les mêmes horaires (par exemple ouverture le mercredi à 12 h, clôture le jeudi à 18 h). Les adhérents engagés reçoivent à chaque ouverture un e-mail qui les invite à commander (MEL-12).

### 6.4 Suivi et pointage

-   **REF-17** Liste des contrats et des commandes de ses producteurs, avec filtres et export Excel.
-   **REF-18** **Outil de pointage** : pour chaque adhérent et chaque échéance, cases « reçu » et « encaissé » (avec date et n° de chèque facultatif). L'adhérent voit le statut.
-   **REF-19** Documents de distribution (§10) consultables et téléchargeables.

## 7. Espace producteur

-   **PRO-01** Calendrier des distributions où il est attendu, avec les quantités à préparer.
-   **PRO-02** **Récapitulatif des commandes** passées chez lui : par distribution, par produit et par adhérent. Il peut le consulter en ligne, le télécharger en PDF ou en Excel, et le reçoit par e-mail.
-   **PRO-03** **Outil de pointage** de ses paiements (mêmes fonctions que pour le référent, limitées à ses adhérents).
-   **PRO-04** (À décider) Modification de sa propre fiche et de ses produits, avec validation par le référent.
-   **PRO-05** Téléchargement de la liste de ses adhérents engagés (PDF ou tableur), si le référent l'a autorisé (REF-23).

## 8. Administration

-   **ADM-01** Accès complet à tous les contenus et données.
-   **ADM-02** Pages d'information et page d'accueil : modification par blocs simples (texte, image, bouton, encadré « actualité »).
-   **ADM-03** Comptes : création, rôles, attribution des référents aux producteurs, désactivation.
-   **ADM-04** Modération (désactiver un compte, masquer un contenu, journal des modifications).
-   **ADM-05** **Distributions** : création du calendrier annuel (génération automatique un samedi sur deux en semaine impaire, puis ajustements), lieux, horaires, producteurs présents par date.
-   **ADM-06** **Clôture d'une distribution** : l'admin valide les quantités, puis déclenche l'envoi des récapitulatifs aux producteurs.
-   **ADM-07** Gestion des « magasins » : vue d'ensemble des offres et des commandes groupées, avec leurs dates d'ouverture et de clôture.
-   **ADM-08** Adhésions : liste, pointage des cotisations, export.
-   **ADM-09** Paramètres : nom de l'AMAP, logo, couleurs, adresses, montant de la cotisation, adresses e-mail de destination (pour la réutilisation par d'autres AMAP).
-   **ADM-10** E-mails groupés : lettre d'information et annonces, envoyées à tous les adhérents ou à un groupe (par exemple « adhérents ayant un contrat avec X »), avec désinscription possible.

## 9. Règles de gestion des distributions

-   **DIS-01** Une distribution a un n°, une date, un lieu, des horaires, la liste des producteurs présents et des besoins en permanents (5 à 6 personnes).
-   **DIS-02** Les distributions de l'AMAP ont lieu les **samedis des semaines calendaires impaires**. Un producteur peut avoir des **livraisons hors distribution AMAP**, dans un autre lieu et à un autre horaire (exemple : le Corti des Aravis livre aussi le vendredi des semaines paires aux Villards). Il faut donc pouvoir gérer **plusieurs lieux et créneaux**.
-   **DIS-03** Une livraison « 1 sur 2 » s'appuie sur une série A ou une série B de distributions.
-   **DIS-04** En cas d'absence de l'adhérent : pas de remboursement ni de rattrapage (règle du contrat). Le site peut proposer « Je serai absent, prévenir le producteur avant vendredi 12h ».

## 10. Documents générés (PDF)

| N°     | Document                          | Contenu                                                                   | Pour qui                    | Comment                                     |
|--------|-----------------------------------|---------------------------------------------------------------------------|-----------------------------|---------------------------------------------|
| DOC-01 | Récapitulatif producteur          | Par distribution : total par produit, puis détail par adhérent            | Producteur, référent        | Espace en ligne + PDF + e-mail à la clôture |
| DOC-02 | Feuille d'émargement              | Une page par produit : Nom de l'adhérent / Quantité / Signature           | Responsable de distribution | PDF à imprimer                              |
| DOC-03 | Récapitulatif de commande groupée | Total par produit, détail par adhérent, montant                           | Producteur, référent        | Espace + PDF + e-mail                       |
| DOC-04 | Émargement de commande groupée    | Comme ci-dessus                                                           | Référent                    | Espace + e-mail                             |
| DOC-05 | Contrat d'engagement              | Contrat du producteur pré-rempli avec les données de l'adhérent           | Adhérent, producteur        | PDF joint à la confirmation                 |
| DOC-06 | Planning annuel                   | Tableau des distributions × producteurs (équivalent du « Planning 2026 ») | Tous                        | Page web + PDF                              |
| DOC-07 | Planning des permanences          | Par distribution : équipe, responsable, téléphones (si consentement)      | Adhérents                   | Page web + PDF                              |

→ *Les modèles PDF existants (émargement, récapitulatifs) seront fournis pour être reproduits.*

## 11. E-mails automatiques (V1)

| N°     | Déclencheur                                     | Destinataire                            | Contenu                                   |
|--------|-------------------------------------------------|-----------------------------------------|-------------------------------------------|
| MEL-01 | Création de compte                              | Adhérent                                | Validation de l'adresse e-mail            |
| MEL-02 | Message via le formulaire de contact            | Alias concerné + expéditeur             | Message, accusé de réception              |
| MEL-03 | Validation d'un contrat ou d'une commande       | Adhérent (copie au référent ?)          | Récapitulatif, échéancier, contrat PDF    |
| MEL-04 | Ouverture d'une commande groupée                | Adhérents (option)                      | Annonce et lien                           |
| MEL-05 | J-2 avant la clôture d'une commande groupée     | Adhérents n'ayant pas commandé (option) | Rappel                                    |
| MEL-06 | Clôture d'une distribution, validée par l'admin | Producteurs concernés                   | Récapitulatif PDF                         |
| MEL-07 | J-5 avant une permanence                        | Permanents + responsable                | Rappel (équipe, horaires 8h30, consignes) |
| MEL-08 | J-3 avant une distribution (option)             | Adhérents concernés                     | « Samedi vous récupérez : … »             |
| MEL-09 | Échéance de paiement à venir (option)           | Adhérent                                | Rappel                                    |
| MEL-10 | Renouvellement de l'adhésion (novembre)         | Adhérents                               | Invitation à réadhérer                    |
| MEL-11 | Ouverture des souscriptions d'un contrat        | Tous les adhérents ou ceux en contrat avec le producteur (au choix) | Annonce et lien |
| MEL-12 | Ouverture d'une commande liée à un engagement   | Adhérents engagés                       | Invitation à commander, avec le lien      |
| MEL-13 | Message « Échanges entre adhérents »            | Adhérents, référent ou bureau           | Message, réponse directe à l'expéditeur   |

## 12. Exigences non fonctionnelles

-   **NF-01** ~**Mobile d'abord** : la plupart des adhérents utiliseront un téléphone.~ **PC d'abord** : la plupart des adhérents utiliseront un pc
-   **NF-02** **Simplicité** : chaque rôle ne voit que ses menus. Libellés en français courant, sans jargon technique.
-   **NF-03** **Accessibilité** : contrastes, textes alternatifs, navigation au clavier (objectif : bonnes pratiques RGAA).
-   **NF-04** **RGPD** : l'AMAP est responsable du traitement et Infomaniak est sous-traitant. Les données sont hébergées en Suisse. Il faut une politique de confidentialité, un consentement pour le partage du téléphone, la possibilité de supprimer son compte, et ne collecter que les données nécessaires. Pas de traceur publicitaire.
-   **NF-05** **Sécurité** : mots de passe chiffrés, HTTPS, droits vérifiés côté serveur, mises à jour régulières, journal des actions sensibles.
-   **NF-06** **Sauvegardes** : sauvegarde automatique quotidienne de la base de données et des fichiers, avec un test de restauration.
-   **NF-07** **Qualité** : tests automatiques, environnement de test séparé, déploiement automatisé depuis GitHub.
-   **NF-08** **Portabilité** : aucun élément propre au Panier des Aravis dans le code. Tout passe par les paramètres et une installation documentée.
-   **NF-09** **Performance** : pages légères (pas de Wix), images optimisées.

## 13. Hébergement, e-mail et budget

-   **HEB-01** Hébergement Web + Mail Infomaniak (environ 7,25 €/mois, 5 adresses e-mail incluses) : il contient le site, la base MySQL/MariaDB et les e-mails.
-   **HEB-02** Nom de domaine `lepanierdesaravis.fr` (non inclus, environ 10 à 15 €/an), à transférer chez Infomaniak **avant son expiration le 13/02/2027**.
-   **HEB-03** Adresses e-mail : quelques vraies boîtes et des **alias**. Proposition : `contact@` (boîte du bureau), `ne-pas-repondre@` (envois automatiques du site), plus les alias `adhesion@`, `permanences@`, `producteurs@`, `tresorerie@` et `webmaster@`, redirigés vers les bonnes personnes.
-   **HEB-04** Budget visé : environ 100 €/an, contre environ 168 €/an aujourd'hui (337 € pour 2 ans).

## 14. Organisation GitHub

-   **GIT-01** Une **organisation GitHub** « lepanierdesaravis » (gratuite), dont **au moins deux membres du bureau sont propriétaires**. Le dépôt appartient à l'association et non à une personne.
-   **GIT-02** Le dépôt est public, sous licence libre (AGPL-3.0 proposée), avec les tickets, un tableau de projet, des modèles de tickets (bug, demande d'amélioration) et une documentation d'installation pour d'autres AMAP.
-   **GIT-03** Les secrets (mots de passe, clés) ne sont **jamais** dans le dépôt.

## 15. Évolutions futures (après la V1)

-   **EVO-01** Scénarios d'e-mail marketing (relances, lettre d'information automatisée).
-   **EVO-02** Rappel au producteur ou au référent après une interruption (plus de 2 distributions sautées). Rappels pour les commandes groupées.
-   **EVO-03** Aide à la prise en main : infobulles et tutoriel à la première connexion, selon le rôle. Vidéos pédagogiques.
-   **EVO-04** Blog et recettes.
-   **EVO-05** Paiement en ligne des contrats (si un jour souhaité).
-   **EVO-06** Liste d'attente pour les contrats complets.

## 16. Points à préciser

Le responsable fonctionnel les tranche avec l'association. La réponse est alors reportée dans l'exigence concernée, et le point est marqué « tranché le … ».

-   **Q-01** Le producteur modifie-t-il lui-même sa fiche et ses produits, ou passe-t-il toujours par son référent ? (ROL-10, PRO-04)
-   **Q-02** Plusieurs référents peuvent-ils gérer un même producteur ? (ROL-17)
-   **Q-03** ~~Que recouvrent les produits « Engagement de 50 € / 100 € / 1000 € pour l'année » de la boutique actuelle : un crédit à utiliser, ou un contrat au montant ?~~ Tranché le 16/11/2026 : les deux. Un montant peut se proposer comme un produit (REF-22), et servir de crédit pour des commandes liées (ADH-26, REF-24).
-   **Q-11** L'annonce d'ouverture d'un contrat va-t-elle par défaut à tous les adhérents, ou seulement à ceux qui ont un contrat en cours ? Le site permet les deux (REF-27).
-   **Q-12** Pour les commandes liées à un engagement, les adhérents non engagés doivent-ils aussi recevoir l'e-mail d'ouverture ? Aujourd'hui, seuls les engagés le reçoivent ; les autres voient la commande dans leur espace. (MEL-12)
-   **Q-04** Les contrats ont-ils tous les mêmes fréquences (toutes / 1 sur 2), ou faut-il des choix de dates libres ? (ADH-07)
-   **Q-05** Après la clôture des souscriptions, l'adhérent peut-il encore modifier son contrat ? (ADH-10)
-   **Q-06** Faut-il gérer les livraisons hors distribution AMAP (autre lieu ou autre jour) dès la V1 ? (DIS-02)
-   **Q-07** Qui envoie les e-mails groupés : uniquement l'admin, ou aussi les référents ? (ROL-16, ADM-10)
-   **Q-08** Permanences : confirmez-vous le fonctionnement « 3 vœux, puis l'AMAP en retient 1 ou 2 » ? Qui fait cette sélection ? (ADH-17)
-   **Q-09** Les prix doivent-ils être visibles publiquement sur les fiches producteurs ? (PUB-04)
-   **Q-10** Formulation pour le « visiteur » dans le champ « Je suis ». (CON-01)
