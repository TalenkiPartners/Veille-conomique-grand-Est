# Veille économique — Grand Est & Luxembourg

Outil maison, gratuit, qui surveille chaque jour et récapitule chaque semaine
l'actualité économique du Grand Est et du Luxembourg dans l'industrie,
l'ingénierie et l'énergie : investissements, levées de fonds, fermetures,
licenciements, recrutement, rachats.

Deux sorties : une page web (mise à jour automatiquement, hébergée
gratuitement sur GitHub Pages) et un email envoyé automatiquement.

## Mise en route (15-20 minutes, une seule fois)

### 1. Créer le dépôt GitHub

- Va sur github.com, crée un compte si tu n'en as pas (gratuit).
- Crée un nouveau dépôt, par exemple `veille-grand-est`. Mets-le en **privé**
  si tu ne veux pas que le contenu soit visible publiquement (la page web
  restera accessible via son lien même en dépôt privé, à condition d'activer
  GitHub Pages — voir étape 4).
- Dépose tous les fichiers de ce projet dans le dépôt (glisser-déposer sur
  l'interface web GitHub fonctionne très bien pour démarrer, pas besoin de
  ligne de commande).

### 2. Créer une adresse d'envoi (gratuite)

Pour envoyer les emails, le plus simple est un compte **Brevo**
(anciennement Sendinblue) : gratuit jusqu'à 300 emails/jour, largement
suffisant pour un envoi quotidien + hebdo.

- Crée un compte sur brevo.com.
- Dans les paramètres SMTP de Brevo, récupère : l'hôte SMTP, le port, ton
  identifiant SMTP et ta clé SMTP (ce n'est pas ton mot de passe de compte,
  c'est une clé dédiée).

Alternative si tu préfères : un compte Gmail avec un "mot de passe
d'application" dédié (à générer dans les paramètres de sécurité Google).

### 3. Configurer les secrets dans GitHub

Dans le dépôt : **Settings → Secrets and variables → Actions → New
repository secret**. Ajoute ces cinq secrets :

| Nom              | Valeur                                    |
|------------------|--------------------------------------------|
| `SMTP_HOST`      | ex. `smtp-relay.brevo.com`                 |
| `SMTP_PORT`      | ex. `587`                                  |
| `SMTP_USER`      | ton identifiant SMTP Brevo                 |
| `SMTP_PASSWORD`  | ta clé SMTP Brevo                          |
| `SENDER_EMAIL`   | l'adresse qui apparaîtra comme expéditeur  |
| `RECIPIENT_EMAIL`| ton adresse mail de réception              |

### 4. Activer GitHub Pages (pour la page web)

**Settings → Pages → Source** : choisis la branche `main` et le dossier
`/docs`. GitHub te donnera une URL du type
`https://tonpseudo.github.io/veille-grand-est/` — c'est ta page,
mise à jour automatiquement à chaque exécution.

### 5. Vérifier que ça tourne

Les deux automatisations sont déjà programmées (quotidien du lundi au
vendredi à 6h30, récap le lundi à 7h). Pour ne pas attendre le lendemain,
tu peux les lancer manuellement dès maintenant :

**Actions → Veille quotidienne → Run workflow**. Regarde les logs, vérifie
que tu reçois bien l'email et que la page s'est mise à jour.

## Réglages

Tout se pilote depuis le haut du fichier `veille.py` :

- `ZONES` — les départements/zones surveillées
- `THEMES` — les thématiques et leurs mots-clés
- `SECTEURS` — le filtre sectoriel (industrie/ingénierie/énergie)
- `DEPARTEMENTS_GRAND_EST` — les codes départements pour la partie BODACC

Rien à recompiler : tu modifies, tu commit, le prochain run applique les
changements.

## Limites à avoir en tête

- Le filtrage se fait par mots-clés — il laissera passer quelques faux
  positifs et en ratera sûrement quelques-uns. C'est le compromis d'un outil
  gratuit sans étape de relecture humaine ou d'IA.
- BODACC couvre les procédures officielles (créations, cessions,
  liquidations, redressements) mais pas les levées de fonds ou recrutements,
  qui viennent uniquement de Google News.
- La couverture Luxembourg est plus faible que celle du Grand Est : Google
  News référence assez peu la presse spécialisée luxembourgeoise
  (Paperjam, Delano). Si ça manque de contenu après quelques semaines
  d'usage, on pourra ajouter leurs flux RSS directement — plus fiable que
  de passer par Google News pour cette partie.

## Évolution possible

Si le filtrage par mots-clés s'avère trop bruyant, l'étape suivante
naturelle est d'ajouter un tri par l'API Claude (quelques centimes par
exécution) pour ne garder que les articles vraiment pertinents et générer
un résumé en une ligne par article — sans changer le reste de
l'architecture.
