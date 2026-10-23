# Mettre l'écran d'accueil en ligne

Le dossier est prêt à être publié : il contient déjà un dépôt Git avec un
premier enregistrement, un fichier `robots.txt` et une balise `noindex` qui
tiennent la page hors des moteurs de recherche.

Il reste trois étapes, que tu dois faire toi-même : je n'ai pas accès à tes
comptes et je ne peux pas saisir de mot de passe à ta place.

---

## Étape 1 — Publier le dossier sur GitHub

Le plus simple est de passer par **GitHub Desktop**, qui gère la connexion
sans ligne de commande ni jeton d'accès.

1. Télécharger GitHub Desktop sur <https://desktop.github.com> et se
   connecter (créer un compte GitHub si tu n'en as pas).
2. Menu **File → Add Local Repository**, puis choisir le dossier :
   `Documents/Organisation Charles/ajr-display`
   Il est déjà reconnu comme dépôt Git, avec son premier enregistrement.
3. Cliquer sur **Publish repository**.
   - Nom suggéré : `ajr-ecran-accueil`
   - **Décocher « Keep this code private »** — GitHub Pages ne publie pas
     depuis un dépôt privé sur l'offre gratuite.

Si tu préfères la ligne de commande, le dépôt est prêt :

```bash
git remote add origin https://github.com/TON-COMPTE/ajr-ecran-accueil.git
git push -u origin main
```

Attention : depuis 2021, GitHub refuse le mot de passe en ligne de commande.
Il faut un jeton d'accès personnel, ce qui rend GitHub Desktop nettement
plus confortable.

## Étape 2 — Activer GitHub Pages

Sur la page du dépôt, aller dans **Settings → Pages** :

- **Source** : `Deploy from a branch`
- **Branch** : `main`, dossier `/ (root)`
- **Save**

Au bout d'une à deux minutes, l'adresse apparaît en haut de la page :

```
https://TON-COMPTE.github.io/ajr-ecran-accueil/
```

## Étape 3 — Afficher sur le téléviseur

Ouvrir cette adresse dans le navigateur du téléviseur, puis le passer en
plein écran. Selon le matériel :

- **Smart TV / Chromecast / Fire TV** : ouvrir l'adresse dans le navigateur
  intégré. Beaucoup de Smart TV n'ont pas de plein écran : prévois une mise
  en favori de la page d'accueil pour qu'elle se rouvre après une coupure.
- **Mini-PC ou Raspberry Pi** : lancer Chrome en mode kiosque, c'est de loin
  le plus fiable, et la page se relance seule après une coupure de courant :

  ```bash
  chromium-browser --kiosk --incognito --noerrdialogs \
    --disable-session-crashed-bubble --check-for-update-interval=31536000 \
    https://TON-COMPTE.github.io/ajr-ecran-accueil/
  ```

---

## Mettre à jour le contenu, plus tard

Modifier `index.html`, puis dans GitHub Desktop : saisir un court résumé,
**Commit to main**, puis **Push origin**. La mise en ligne prend une minute.

Le téléviseur se recharge tout seul chaque nuit à 3 h : une modification
faite dans la journée est donc visible le lendemain matin, sans toucher à
l'écran. Pour la voir tout de suite, il faut recharger la page sur le
téléviseur.

---

## Deux points à régler

### Les polices Campton et Soleil

Une fois le dépôt public, les fichiers `assets/fonts/*.otf` sont
téléchargeables par n'importe qui. Les licences de police distinguent
l'usage bureautique de l'usage web, et une licence bureautique ne couvre
généralement **pas** la diffusion des fichiers sur un site.

Vérifie la licence achetée par l'AJR auprès de la fonderie. Si elle ne
couvre pas le web, supprime simplement le dossier `assets/fonts` : j'ai
prévu des polices de repli proches, installées d'office sur la plupart des
machines (Avenir Next, Century Gothic, Futura). Le rendu reste correct,
simplement un peu moins fidèle à la charte.

### Une page publique

La page n'apparaîtra pas dans Google, mais elle reste accessible à qui
connaît l'adresse. Y figurent les consignes de sécurité internes, le prénom
d'Anne-Sophie et le compteur d'accidents du travail.

Si cela pose un problème, deux solutions :

- **Un boîtier dédié derrière l'écran** (mini-PC, Raspberry Pi), avec la
  page en local. Rien ne sort du réseau de l'atelier, hormis l'appel météo.
  C'est la solution la plus autonome et la plus discrète.
- **Un hébergement avec contrôle d'accès**, par exemple Cloudflare Pages
  avec Cloudflare Access, ou Netlify avec protection par mot de passe. Dis-le
  moi et je prépare la configuration.
