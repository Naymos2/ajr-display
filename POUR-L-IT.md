# Écran d'accueil — note pour l'IT

## À dire en deux phrases

> On a refait l'écran d'affichage de l'accueil. C'est une simple page web
> statique, sans serveur ni base de données, qui tourne en boucle sur le
> téléviseur. J'ai besoin de savoir où on l'héberge et ce qui pilote l'écran.

---

## Ce que c'est

- **Une seule page web statique.** Un fichier `index.html` et un dossier
  d'images et de polices. 2,3 Mo au total.
- **Aucun code côté serveur**, aucune base de données, aucun identifiant,
  aucun cookie, aucune collecte de données.
- **Format 16:9**, conçu pour du 1920 × 1080. La page s'adapte seule à la
  résolution réelle de l'écran.
- Onze vues qui défilent en boucle, environ deux minutes par tour.

## Ce dont elle a besoin sur le réseau

Un seul appel sortant, en HTTPS :

```
https://api.open-meteo.com/v1/forecast        (toutes les 30 minutes)
```

C'est le service de météo, gratuit et sans clé d'accès. Quelques kilo-octets
par appel. **S'il est bloqué par le pare-feu ou le proxy, rien ne casse** :
la vue météo disparaît simplement de la boucle, le reste continue.

Aucun port entrant, aucun flux à ouvrir vers l'écran.

## Navigateur

Chrome, Edge ou Safari récents. Techniquement : Chrome 90 ou plus, Safari 15
ou plus. Une Smart TV avec un navigateur ancien risque de mal afficher la
mise en page.

---

## Décision 1 — Où héberger la page

| Solution | Avantage | Inconvénient |
|---|---|---|
| **Sur le boîtier de l'écran**, en local | Rien ne sort du réseau. Fonctionne même sans Internet, hormis la météo. Le plus autonome. | Toute modification demande d'accéder au boîtier. |
| **Serveur web interne** (si l'AJR en a un) | Contenu qui reste dans l'entreprise. Mise à jour centralisée. | Dépend d'un serveur à maintenir. |
| **GitHub Pages** (gratuit, externe) | Aucune maintenance, HTTPS, très fiable. Mise à jour en deux clics. | **La page devient publique.** Elle est exclue des moteurs de recherche, mais reste accessible à qui connaît l'adresse. |

**Le point d'attention sur GitHub Pages** : la page contient les consignes de
sécurité internes, le rappel sur le document C32A signé d'un prénom, le
compteur d'accidents du travail et les photos de six collaborateurs. Tout
cela se retrouverait sur le web ouvert. À l'IT de dire si c'est acceptable,
ou s'il faut un hébergement avec contrôle d'accès.

## Décision 2 — Ce qui pilote le téléviseur

La solution la plus fiable est un **petit boîtier dédié** derrière l'écran :
mini-PC, Intel NUC ou Raspberry Pi 4. Chrome en mode kiosque, lancé au
démarrage. L'écran se remet en route tout seul après une coupure de courant.

À configurer sur le boîtier :

- lancement automatique du navigateur en mode kiosque à l'ouverture de session ;
- **veille et économiseur d'écran désactivés** ;
- redémarrage automatique après coupure de courant, dans le BIOS ;
- mises à jour système en dehors des heures d'ouverture.

Une Smart TV qui ouvre une adresse web fonctionne aussi, mais beaucoup n'ont
ni plein écran ni réouverture automatique après extinction.

## Décision 3 — Réglages du téléviseur

**Le type de dalle est réglé** : c'est du LCD à rétroéclairage LED, et non de
l'OLED. Le bandeau du bas est affiché en permanence, mais sur cette
technologie il n'y a pas de marquage définitif — tout au plus une rémanence
passagère qui s'efface seule. Aucune précaution particulière à prendre, et
rien à ajouter dans la page.

Restent les réglages d'image :

- **surbalayage (overscan) désactivé**, sinon les bords sont rognés ;
- **extinction automatique désactivée** ;
- mode d'image « PC » ou « Jeu » si disponible, pour un rendu net du texte ;
- **rétroéclairage autour de 60 à 70 %** plutôt qu'au maximum : c'est plus
  confortable dans un hall et cela ménage le rétroéclairage, qui s'use à
  l'usage.

Un point à vérifier si le téléviseur est un modèle grand public : beaucoup
sont prévus pour six à huit heures d'utilisation par jour, là où les écrans
professionnels tiennent seize à vingt-quatre heures. Certains fabricants
excluent même l'usage commercial de la garantie. Si l'écran doit tourner
toute la journée, la question mérite d'être posée.

---

## Questions à poser, dans l'ordre

1. A-t-on un serveur web interne où déposer une page statique, ou part-on
   sur un hébergement externe ?
2. Un hébergement externe public est-il acceptable pour des consignes
   internes et des photos de collaborateurs ? Sinon, peut-on mettre un
   contrôle d'accès ?
3. Qu'est-ce qui pilote le téléviseur de l'accueil aujourd'hui, et peut-on
   y brancher un petit boîtier dédié ?
4. Peut-on autoriser `api.open-meteo.com` en sortie depuis ce poste ?
5. Le téléviseur est-il prévu pour un fonctionnement continu, ou s'agit-il
   d'un modèle grand public limité à quelques heures par jour ?
6. Qui gère le boîtier au quotidien, pour la veille, les mises à jour et le
   redémarrage automatique ?

La question du type de dalle est réglée : LCD à rétroéclairage LED, donc
aucun risque de marquage.

## Une question pour le service achats, pas pour l'IT

Les polices Campton et Soleil de la charte sont sous licence. Si la page est
hébergée publiquement, leurs fichiers deviennent téléchargeables, ce que les
licences bureautiques ne couvrent généralement pas. Il faut retrouver la
licence achetée par l'AJR. À défaut, on retire les fichiers de police et la
page bascule sur des polices de repli proches, déjà prévues.

---

## Ce qui est déjà prêt

- La page est terminée et vérifiée vue par vue.
- Le dossier est un dépôt Git, prêt à être publié sur GitHub Pages si c'est
  la solution retenue : voir `HEBERGEMENT.md`.
- Elle tient en continu sans surveillance : rechargement automatique si la
  boucle se fige, rechargement nocturne à 3 h, dernier relevé météo conservé
  en cas de coupure réseau, et aucune animation permanente qui chargerait le
  processeur.
