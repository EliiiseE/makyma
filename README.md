# Documentation - **[makyma.org](http://www.makyma.org)** 🌊

Les instructions suivantes vous aideront à la mise en route et à l'utilisation de l'application.

## Prérequis 🔧

#### NodeJS

[Installation](https://nodejs.org/en/download/) pour MacOS, Linux et Windows.

#### Version de Ruby : `2.7.2`

* [Installation](https://github.com/lewagon/setup/blob/master/macOS.md) pour MacOS
* [Installation](https://github.com/lewagon/setup/blob/master/UBUNTU.md) pour Linux
* [Installation](https://github.com/lewagon/setup/blob/master/WINDOWS.md) pour Windows

#### Version de Ruby on Rails : `5.2.4.1`

* [Installation](https://www.synbioz.com/blog/tech/installer-ruby-on-rails-sur-mac) pour MacOS
* [Installation](https://doc.ubuntu-fr.org/rubyonrails) pour Linux
* [Installation](https://gorails.com/setup/windows/10) pour Windows

#### PostgreSQL

* [Installation](https://postgresapp.com/downloads.html) pour MacOS
* [Installation](https://doc.ubuntu-fr.org/postgresql) pour Linux
* [Installation](https://www.postgresql.org/download/windows/) pour Windows

## Configuration ⚙️

### Dans le dossier du projet, installer les gems

```bash
bundle install
```

## Création de la base de données 📚

### Dans le dossier du projet, **créer** la database

```bash
rails db:create
```

⚠️ Si cette commande ne fonctionne pas, exécuter la commande suivante

```bash
bundle exec rake db:create
```

### Effectuer **les migrations** de la database

```bash
rails db:migrate
```

⚠️ Si cette commande ne fonctionne pas, exécuter la commande suivante

```bash
bundle exec rake db:migrate
```

## Initialisation de la base de données 🔍

Pour récupérer les données de la database, il y a **2 façons** de faire :

### Récupérer les données de la database du site en ligne

> ⚠️ Pour cette solution, il est impératif d'être invité en tant que collaborateur sur **Scalingo**, ensuite connectez vos avec `scalingo login`

Ensuite execute cette site de commandes pour `dump` la base de donnée prod, un fichier `dump.sql` sera créé dans le dossier du projet.
```bash
DATABASE_URL="$(scalingo --app makyma env | grep POSTGRESQL | cut -d '=' -f2- | sed -n 2p)"
pg_dump --clean --if-exists --format c --dbname $DATABASE_URL --no-owner --no-privileges --no-comments --exclude-schema 'information_schema' --exclude-schema '^pg_*' --file dump.pgsql
```

Il va maintenant falloir `restore` sur la database locale.
```bash
pg_restore --clean --if-exists --no-owner --no-privileges --no-comments --dbname <URL_DE_VOTRE_DB_LOCAL> dump.pgsql
```

Vous pouvez ensuite supprimer le fichier `dump.pgsql`

### Récupérer les données de la database du site local

#### Importer les données

```bash
rails makyma:import
```

#### Créer les comptes admin

```bash
ADMIN_EMAIL="<your_email>" ADMIN_PASSWORD="<password>" rails makyma:admin
```

## Référencement du site ⭐

### Créer le sitemap

```bash
rake sitemap:create
```

### Modifier le sitemap

```bash
rake sitemap:refresh
```

### Effacer le sitemap

```bash
rake sitemap:clean
```

Pour plus d'information, voir la [documentation](https://github.com/kjvarga/sitemap_generator) de la gem.

## Exécution du site 💻

Pour **lancer** le serveur rails

```bash
rails s
```

> ⚠️ Les changements sur le site sont chargé automatiquement, pas besoin de relancer le serveur

## Instructions pour la mise en ligne ✔️

### Installer le CLI de Heroku

[Installation](https://devcenter.heroku.com/articles/heroku-cli) pour MacOS, Linux et Windows.

Puis, **se connecter** à Heroku

```bash
heroku login
```

### Commandes de la mise en ligne

#### GitHub

**Ajouter** les fichiers

```bash
git add .
```

**Effectuer** le commit

```bash
git commit -m "message"
```

**Mettre** en ligne les fichiers

```bash
git push
```

#### Heroku

**Mettre** en ligne les fichiers

> ⚠️ Si vous avez fait un `git clone` pour créer le répertoire, il faut **exécuter la commande suivante** avant de continuer

```bash
heroku git:remote -a makyma
```

Vous pouvez ensuite **push** les fichiers en ligne

```bash
git push heroku master
```

**Migrer** la base de donnée

```bash
heroku run rails db:migrate
```

---

© [makyma.org](http://www.makyma.org) 2021 🧡
