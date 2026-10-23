# Écran d'accueil — Atelier Jean Regniers

Page d'affichage pour le téléviseur de l'accueil. Une seule page web, conçue
pour tourner en continu sans intervention.

Pour la mise en ligne sur GitHub Pages, voir `HEBERGEMENT.md`.

## Lancer l'écran (en local, pour travailler dessus)

Dans le dossier `ajr-display`, démarrer un petit serveur local :

```
python3 -m http.server 8765
```

Puis ouvrir `http://localhost:8765` dans le navigateur et appuyer sur `F`
pour le plein écran.

Ouvrir `index.html` par un double-clic **ne fonctionne pas correctement** :
le navigateur bloque alors les polices et la météo. Il faut passer par le
serveur, ou héberger la page en ligne.

## Commandes au clavier

| Touche | Effet |
|---|---|
| `→` / `←` | Vue suivante / précédente |
| `espace` | Met la rotation en pause |
| `F` | Plein écran |

## Ce qu'il faut modifier au quotidien

Tout se règle dans le bloc `CONFIG`, au début du `<script>` en bas de
`index.html`.

| Réglage | À quoi ça sert |
|---|---|
| `dernierAccident` | Date du dernier accident de travail, au format `AAAA-MM-JJ`. Le compteur s'incrémente ensuite tout seul, chaque jour. |
| `annonce.titre` | Message exceptionnel (livraison, coupure, visite…). Tant que ce champ est vide, la vue Annonce reste masquée. |
| `annonce.detail` | Précisions affichées sous le titre de l'annonce. |
| `ville`, `latitude`, `longitude` | Lieu utilisé pour la météo. |
| `seuilChaleur` | Température maximale à partir de laquelle la vue « Pensez à vous hydrater » apparaît (28 °C par défaut). |
| `rafraichirMeteo` | Minutes entre deux relevés météo. |
| `heureRechargement` | Heure à laquelle la page se recharge la nuit, pour prendre en compte les modifications. |

Les textes des vues se modifient directement dans le HTML : chaque vue est
une section `<section class="vue" id="v-...">`, dans l'ordre d'affichage.

## Les onze vues

1. **Accueil** — logo, « Bienvenue », mosaïque des photos d'atelier
2. **Annonce** — message exceptionnel *(masquée si `annonce.titre` est vide)*
3. **Vision** — politique interne, citation, certifications
4. **Engagements** — les cinq caps
5. **Sécurité** — les consignes du jour
6. **Document C32A** — rappel administratif
7. **Compteur** — jours sans accident de travail
8. **Alcool & travail**
9. **Météo** — aujourd'hui et les trois jours suivants *(masquée si la météo est injoignable)*
10. **Nos équipes** — galerie photo
11. **Hydratation** — *(uniquement au-delà du `seuilChaleur`)*

Un tour complet dure environ deux minutes.

## Paramètres d'adresse, pour les tests

| Paramètre | Effet |
|---|---|
| `?tout=1` | Affiche aussi les vues conditionnelles |
| `?vue=7` | Démarre sur la vue n° 7 |
| `?chaleur=1` | Force la vue Hydratation |

Exemple : `http://localhost:8765/?tout=1&vue=5`

## Fiabilité

La page est prévue pour rester allumée des semaines sans surveillance.

- **Aucune dépendance Internet pour l'affichage.** Les polices Campton et
  Soleil et toutes les images sont servies depuis le dossier `assets`.
  Seule la météo interroge le réseau.
- **Météo tolérante aux pannes.** Appel limité à 9 secondes, dernier relevé
  conservé d'une extinction à l'autre, mention « dernier relevé il y a X h »
  au-delà de 3 heures. Si la météo reste injoignable, sa vue sort simplement
  de la rotation.
- **Chien de garde.** Si la rotation se fige, la page se recharge d'elle-même.
- **Filet de sécurité d'affichage.** Si une animation d'entrée ne démarre pas
  ou se fige, l'état final est forcé après 1,4 seconde : l'écran ne peut pas
  rester vide.
- **Image manquante sans conséquence.** Une photo absente est masquée, la
  mise en page tient.
- **Charge processeur minime.** Pas de boucle d'animation permanente, pas de
  flou ni de transparence coûteuse : l'avancement repose sur un minuteur et
  une animation CSS, ce qui convient à un boîtier d'affichage modeste.
- **Rechargement nocturne** à l'heure fixée par `heureRechargement`.

## Charte graphique

Conforme au brandbook AJR :

- **Bleu foncé** `#2c2c7a` · **Orange** `#F17700` · **Vert** `#ABBE00` · **Rose** `#e6007d`
- **Campton** pour les titres et sous-titres, **Soleil** pour le corps de texte
- Logo vectoriel officiel, décliné en couleurs et en blanc

Chaque vue reçoit une couleur d'accent selon son sujet : orange pour la
sécurité et les alertes, vert pour la qualité et la vision, rose pour
l'humain et l'administratif.

## Contenu du dossier

```
index.html          la page complète
NOTICE.md           ce document
assets/
  logo-full.png            logo couleurs avec la signature
  logo-full-white.png      idem, en blanc
  logo-lockup.png          logo couleurs sans la signature
  logo-lockup-white.png    idem, en blanc
  symbol.png               le symbole seul
  symbol-white.png         idem, en blanc
  photo-1..6.jpg           photos d'atelier
  c32a-doc.jpg             formulaire C32A
  c32a-money.jpg           billets
  alcool.jpg               pictogramme « zéro alcool »
  fonts/                   Campton et Soleil
```

## À remplacer dès que possible

- **Photos d'atelier et pictogrammes** : ils sont découpés dans des captures
  d'écran, donc de qualité moyenne. Les fichiers d'origine donneraient un
  bien meilleur rendu sur un grand écran.
- **Polices** : Campton et Soleil sont sous licence. Tant que la page reste
  sur le réseau interne, il n'y a rien à faire ; avant toute mise en ligne
  publique, il faut vérifier que la licence couvre la diffusion web.
