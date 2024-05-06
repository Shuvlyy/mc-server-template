<div align="center">
  <h1>Workshop Plugin Minecraft</h1>
</div>

## Présentation

Un plugin Minecraft est une extension du jeu, server-side, écrite en Java, permettant de manipuler les règles de Minecraft :
modifier l’action liée à un évènement, l’annuler, ou ajouter d’autres actions personnalisées à cet évènement.
Il est aussi possible de créer des tâches qui seront exécutées à un certain intervalle de temps, ou de créer des commandes permettant d’effectuer une action personnalisée.
Cela nous permettrait par exemple d’empêcher un joueur de poser un bloc dans une certaine zone du monde, ou d’envoyer un message à chaque joueur connecté
lorsque l’on exécutera une commande donnée (par exemple `/broadcast <message>`). Entre autres, c’est cet outil qui nous permettra d’établir les règles des différents mini-jeux.

En bref, tout est possible avec les plugins. C'est différent des mods, qui modifient directement le jeu
(donc là on peut ajouter directement des nouveaux items, avec des textures personnalisées), mais on peut faire globalement ce qu'on veut avec des plugins,
avec de l'imagination 😊

C’est d’ailleurs avec les plugins que fonctionnent la majorité des gros serveurs aujourd’hui,
comme Hypixel, Hashtek (😏), Minemen, anciennement Epicube, Funcraft, Mineplex…

Pour créer notre plugin, nous utiliserons l’API Spigot, qui est dérivée de Bukkit
(et qui a donc la même utilité), car celle-ci est plus documentée.
Elle contient l’ensemble des fonctions et des classes dont nous nous servirons pour interagir avec notre serveur.

À la fin de ce workshop, vous aurez fait une commande qui vous donne un item personnalisé, un chat personnalisé ainsi qu'une blacklist (certains blocs qu'on ne peut pas poser).

## Disclaimer

Si vous ne connaissez pas le Java, pas de panique, nous sommes là pour apprendre ! 😊

Tout ça est faisable sur Linux, macOS et sur Windows, mais les captures d’écrans seront faites avec Windows.

> [!TIP]
> La version utilisée sera la 1.8.8. Si vous voulez utiliser une version plus récente, récupérez le fichier JAR sur https://getbukkit.org/download/spigot
> et remplacez le `spigot.jar` présent dans votre serveur (gardez le nom `spigot.jar`‼️). Faites attention à la version de Java à utiliser !

> [!TIP]
> Le nom des classes doit s'écrire en `PascalCase`, et le reste en `camelCase` (nom des variables...)

## Prérequis

-	Minecraft (si vous avez un compte Premium, je vous conseille le launcher [Prism](https://prismlauncher.org/download/linux/), autrement un bon vieux [TLauncher](https://tlauncher.org/jar/))
-	Java 8 (https://java.com/)
-	L'IDE Eclipse (https://www.eclipse.org/downloads/packages/) (ou IntelliJ IDEA 😉)

> [!CAUTION]
> Prenez bien cette version d'Eclipse !
> ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/83d13e57-abd5-4cd9-9672-10acb2a512ba)

-	Template de serveur (ce repository)

## Création du serveur

1. Clonez ce repository
```sh
git clone git@github.com:Shuvlyy/workshop-plugin-mc.git
```

2. Le script pour lancer le serveur est `run.sh` (ou `run.cmd` si vous êtes sur Windows).\
   Lancez le une première fois.

3. Vous verrez que le serveur ne démarre pas tout de suite. Pour continuer, il faut accepter les conditions d'utilisation de Mojang, les EULA.\
   Pour ce faire, allez dans le fichier `eula.txt` et remplacez le `false` par `true`.

4. Relancez le serveur. Tous les fichiers vont se créer, dont le monde, qui peut prendre un peu de temps (tout dépend de la puissance votre PC).

5. Une fois terminé, il y aura une ligne avec écrit "Done".\
   Pour accéder à votre tout nouveau serveur, entrez l'IP `localhost` (ou `127.0.0.1` pour les puristes) sur votre jeu.\
   Laissez bien ce terminal tourner en fond, c'est lui qui fait tourner le serveur !

> [!TIP]
> N'oubliez pas de vous donner les permissions d'administrateur sur votre serveur !\
> Pour ce faire, exécutez la commande `op <pseudo>` dans la console. Pour retirer les permissions, c'est la commande `deop <pseudo>`.

## Initialisation du projet (sur Eclipse)

1. Créez un nouveau projet (File > New > Java Project)\
   ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/e44ec2c1-3e05-4060-903c-35f955e78a53)\
   ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/f8be7a60-c070-4d4a-84c5-4786bce998c0)

> [!WARNING]
> Faites bien attention à prendre la bonne version de Java, la `JavaSE-1.8` !

2. Créez un nouveau package dans le dossier `src` (clic droit sur `src` > New > Package) :\
   `fr.<votre_pseudo>.spigot.<le_nom_de_votre_plugin>`\
   Ce package sera la racine de notre plugin (voyez les packages comme des dossiers).

4. Maintenant, créez une nouvelle classe Java dans le package créé qui porte le nom de votre plugin (dans notre cas, `Workshop`).

5. Notre structure principale est finie. Maintenant, paramétrons notre projet pour accéder à l'API de Spigot.

   1. Ajoutez le fichier `spigot.jar` présent dans votre serveur dans les archives externes (clic droit sur votre projet > Build Path > Add External Archives...)
   2. Et voilà ! Spigot est maintenant utilisable.

6. Écrivons maintenant les instruction du plugin.

   1. Ajoutez `extends JavaPlugin` après le nom de votre classe, puis importez la classe correspondante (`Ctrl + Shift + O` par défaut).
   2. Maintenant, collez ce bout de code à l'intérieur de votre classe :
      ```java
      /**
       * Code appelé lors du lancement du serveur.
       */
      @Override
      public void onEnable()
      {
          System.out.println("Plugin loaded!");  // Équivalent de "printf" en C.
      }

      /**
       * Code appelé lors de l'arrêt du serveur.
       */
      @Override
      public void onDisable()
      {
          System.out.println("Goodbye, server... :(");
      }
      ```

7. Dernière étape avant de pouvoir tester, il faut créer "l'identité" du plugin (son nom, sa version, son auteur...).\
   Pour ce faire, créez un fichier nommé `plugin.yml` dans le dossier `src` de votre plugin (clic droit sur `src` > New > File) et collez-y dedans ceci :
   ```yml
   name: Workshop # Nom de votre plugin
   version: 1.0 # Version de votre plugin
   author: <votre_pseudo> # Qui l'a créé
   main: fr.shuvly.spigot.workshop.Workshop # Mettez ici le chemin vers la classe principale qu'on a créé précédement.
   #  /* ^ Nom du package */    /* ^ Nom de la classe (du fichier) */
   ```

9. Notre premier jet est désormais fini ! Maintenant, vérifions qu'il fonctionne bien. Exportez le plugin dans le dossier `plugins` de votre serveur.\
   Clic droit sur votre projet > Export

   1. En type de fichier, sélectionnez `Java/JAR file`.\
      ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/f7aead34-f60d-4c0c-a6b9-57f1be57f8d3)
   2. Cliquez sur "Next".
   3. Cliquez sur le bouton "Browse..." et sélectionnez le chemin vers le dossier `plugins` de votre serveur, et donnez un nom à votre fichier.
   4. Cochez la case "Overwrite existing files without warning" (sinon c'est relou il y aura toujours une popup quand vous exporterez).
   5. Puis cliquez sur "Finish" !\
      ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/f44eab89-ce96-491d-869e-c595c6d151ae)
   6. Maintenant, dans la console, tapez la commande `reload`.\
      Si vous voyez le petit message qu'on a écrit au dessus, c'est gagné ! Modifiez-le et regardez 😎\
      ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/8c246407-0c0c-4158-8e0f-d7b7fbf34954)

> [!TIP]
> À chaque changement que vous faites sur le plugin, il faudra exporter le plugin et reload le serveur.

`⁉️` Votre plugin ne fonctionne pas ? Vérifiez bien que votre plugin a bien été exporté dans le dossier `plugins`.\
Regardez également la console, l'erreur est souvent explicite.

## Gestion d'événements

Sur Minecraft, il se passe ce qu'on appelle des events (des événements). À chaque action faite sur le serveur, un événement est appelé.
- Un joueur rejoint / quitte le serveur ? `PlayerJoinEvent` / `PlayerQuitEvent` est appelé.
- Un joueur pose un bloc ? `BlockPlaceEvent` est appelé.
- Un message est envoyé ? `AsyncPlayerChatEvent` est appelé.

Il existe beaucoup d'events, dont la liste se trouve [ici](https://helpch.at/docs/1.8/org/bukkit/event/class-use/Event.html).
