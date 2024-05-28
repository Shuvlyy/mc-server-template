<div align="center">
  <h1>🛠️ Workshop Plugin Minecraft 🛠️</h1>
</div>

## Présentation

Un plugin Minecraft est une extension du jeu, server-side, écrite en Java, permettant de manipuler les règles de Minecraft :
modifier l’action liée à un évènement, l’annuler, ou ajouter d’autres actions personnalisées à cet évènement.
Il est aussi possible de créer des tâches qui seront exécutées à un certain intervalle de temps, ou de créer des commandes permettant d’effectuer une action personnalisée.
Cela nous permettrait par exemple d’empêcher un joueur de poser un bloc dans une certaine zone du monde, ou d’envoyer un message à chaque joueur connecté
lorsque l’on exécutera une commande donnée (par exemple `/broadcast <message>`).

En bref, tout est possible avec les plugins. C'est différent des mods, qui modifient directement le jeu
(donc là on peut ajouter directement des nouveaux items, avec des textures personnalisées), mais on peut faire globalement ce qu'on veut avec des plugins,
avec de l'imagination ! 😊

C’est d’ailleurs avec les plugins que fonctionnent la majorité des gros serveurs aujourd’hui,
comme Hypixel, Minemen, Hashtek (😏), anciennement Epicube, Funcraft, Mineplex…

Pour créer notre plugin, nous utiliserons l’API Spigot, qui est dérivée de Bukkit
(et qui a donc la même utilité), car celle-ci est plus documentée, et bien plus à jour.
Elle contient l’ensemble des fonctions et des classes dont nous nous servirons pour interagir avec notre serveur.

À la fin de ce workshop, vous saurez faire une commande qui vous donne un item personnalisé, un chat personnalisé ainsi qu'une blacklist de blocs (certains blocs qu'on ne peut pas poser).

## Disclaimer

Tout ça est faisable sur Linux, macOS et sur Windows, mais les captures d’écrans seront faites avec Windows.

> [!TIP]
> La version utilisée sera la 1.8.8. Si vous voulez utiliser une version plus récente, récupérez le fichier JAR sur https://getbukkit.org/download/spigot
> et remplacez le `spigot.jar` présent dans votre serveur par le `.jar` que vous venez de télécharger. Faites attention à la version de Java à utiliser !

> [!TIP]
> Le nom des classes doit s'écrire en `PascalCase`, les packages en `lowercase` et le reste en `camelCase` (nom des variables...)

> [!TIP]
> N'hésitez pas à venir me voir si vous avez besoin d'aide, ne restez pas bloqué ! 😉\
> Utilisez aussi Google, la meilleure prompt est "spigot " + votre problème !

## Prérequis

-	Minecraft **1.8.8 / 1.8.9** (je vous conseille le launcher [Prism](https://prismlauncher.org/download/linux/))
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

2. Pour lancer le serveur, exécutez la commande `java -jar spigot.jar`.\
   Lancez le une première fois.

3. Vous verrez que le serveur refuse de démarrer. Pour continuer, il faut accepter les conditions d'utilisation de Mojang, les EULA.\
   Pour ce faire, allez dans le fichier `eula.txt` et remplacez le `false` par `true`.

4. Relancez le serveur. Tous les fichiers vont se créer, dont le monde, qui peut prendre un peu de temps (tout dépend de la puissance de votre PC).

5. Une fois terminé, il y aura une ligne avec écrit "Done".\
   Pour accéder à votre tout nouveau serveur, entrez l'IP `localhost` (ou `127.0.0.1` pour les puristes) sur votre jeu.\
   Laissez bien ce terminal tourner en fond, c'est lui qui fait tourner le serveur !

> [!TIP]
> N'oubliez pas de vous donner les permissions d'administrateur sur votre serveur !\
> Pour ce faire, exécutez la commande `op <pseudo>` dans la console. Pour retirer les permissions, c'est la commande `deop <pseudo>`.\
> Pour arrêter le serveur, exécutez la commande `stop`.

## Initialisation du projet (sur Eclipse)

1. Créez un nouveau projet (File > New > Java Project)\
   ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/e44ec2c1-3e05-4060-903c-35f955e78a53)\
   ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/f8be7a60-c070-4d4a-84c5-4786bce998c0)

> [!WARNING]
> Faites bien attention à prendre la bonne version de Java, la `JavaSE-1.8` !

2. Créez un nouveau package dans le dossier `src` : `fr.<votre_pseudo>.spigot.<le_nom_de_votre_plugin>`\
   Ce package sera la racine de notre plugin (voyez les packages comme des dossiers).

4. Maintenant, créez une nouvelle classe Java dans le package créé qui porte le nom de votre plugin (dans notre cas, `Workshop`) et qui étend de la classe `JavaPlugin`.

5. Notre structure principale est finie. Maintenant, paramétrons notre projet pour accéder à l'API de Spigot.

   1. Ajoutez le fichier `spigot.jar` présent dans votre serveur dans les archives externes (clic droit sur votre projet > Build Path > Add External Archives...)
   2. Et voilà ! Spigot est maintenant utilisable.

6. Écrivons maintenant les instructions basiques du plugin :
   - Une fonction de type `void` appelée `onEnable` (avec le décorateur `Override`) qui sera appelée au lancement du serveur
   - La même fonction mais appelée `onDisable`, qui sera appelée à l'arrêt du serveur
```java
@Override
public void onEnable()
{
    System.out.println("Plugin loaded!");  // Équivalent de "console.log" en JS.
}

@Override
public void onDisable()
{
    System.out.println("Goodbye, server... :(");
}
```

7. Dernière étape avant de pouvoir tester, il faut créer "l'identité" du plugin (son nom, sa version, son auteur...).\
   Pour ce faire, créez un fichier nommé `plugin.yml` dans le dossier `src` de votre plugin et collez-y dedans ceci :
   ```yml
   name: Workshop
   version: 1.0
   author: <votre_pseudo>
   main: fr.shuvly.spigot.workshop.Workshop # Adaptez cette ligne à votre nom de package.
   ```
   Cette "identité" est consultable grâce à la commande `/version` (ou `/ver`) suivi du nom de votre plugin.

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

> [!TIP]
> Cliquez là dessus, ça rend l'architecture beaucoup plus claire 😉\
> ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/85ae56f6-71e6-4c1e-9822-6dd73898559b)

## Création de commandes

Sur Minecraft, il existe des commandes, comme `/give` pour se donner des items, `/tp` pour se téléporter à certaines coordonnées ou à un joueur, etc...

Mais, avec les plugins, il est possible de créer ses propres commandes ! Créons-en une pour envoyer un message à tous les joueurs : `/broadcast`

1. Créez un sous-package qu'on va nommer `command` (qui contiendra toutes nos commandes), puis dans ce sous-package une nouvelle classe nommée `CommandBroadcast` qui implémente la classe `CommandExecutor`.\
   Ajoutez-y les fonctions demandées (normalement, juste `onCommand`).\
   *(le nom pré-généré des arguments n'étant pas très explicite, je vous conseille de les remplacer par `sender`, `command`, `label`, `args` respectivement)*

3. Puis, dans cette fonction `onCommand`, collez-y ce bout de code :
   ```java
   Bukkit.broadcastMessage("Ceci est un broadcast");
   ```

> [!TIP]
> Jouez avec les [codes couleurs](https://helpch.at/docs/1.8/index.html?org/bukkit/ChatColor.html) de Minecraft !

3. Maintenant, il faut dire au serveur que cette commande existe.\
   Pour ce faire, au lancement du serveur, appelez ce bout de code :
   ```java
   getCommand("broadcast").setExecutor(new CommandBroadcast());
   //          ^ Nom de la commande        ^ Classe responsable de la commande
   ```

4. Finalement, il faut rajouter notre commande dans "l'identité" du plugin (le fichier `plugin.yml`).
   Créez simplement une section `commands` à la fin du fichier, puis ajoutez-y le nom de votre commande. Vous pouvez même ajouter une description, une permission et plus !
   ```yml
   ...
   commands:
     broadcast:
       description: "Envoie un message à tout le serveur."
   ```
   La description est accessible grâce à la commande `/help` suivi du nom de la commande (donc `/help broadcast`).

5. Et voilà ! Maintenant, quand un joueur exécute la commande `/broadcast`, un message est envoyé à tous les joueurs.\
   ![image](https://github.com/Shuvlyy/workshop-plugin-mc/assets/123988037/a88d469b-e075-4673-9459-c932ea3c6739)


## Gestion d'événements

Sur Minecraft, il se passe ce qu'on appelle des events (des événements). À chaque action faite sur le serveur, un événement est déclenché.
- Un joueur rejoint / quitte le serveur ? `PlayerJoinEvent` / `PlayerQuitEvent` est déclenché.
- Un joueur pose un bloc ? `BlockPlaceEvent` est déclenché.
- Un message est envoyé ? `AsyncPlayerChatEvent` est déclenché.

Il existe beaucoup d'events, dont la liste se trouve [ici](https://helpch.at/docs/1.8/org/bukkit/event/class-use/Event.html).

Par exemple, implémentons un petit `Listener` (une classe qui écoute des events) qui se chargera d'exécuter quelques trucs quand un joueur rejoint notre serveur.

1. Créez un sous-package qu'on va nommer `listener` (qui contiendra tous nos listeners), puis dans ce sous-package une nouvelle classe nommée `ListenerJoin` qui implémente la classe `Listener`.

2. Créez une fonction de type `void` qui prend en paramètre l'event `PlayerJoinEvent` avec le décorateur `EventHandler`.\
   C'est cette fonction qui sera appelée quand l'événement sera déclenché.\
   Écrivez-y les instructions que vous voulez, profitez-en pour découvrir un peu la documentation et ce que vous pouvez faire !

4. Maintenant, tout comme pour les commandes, il faut dire au serveur que ce `Listener` existe.\
   Pour ce faire, au lancement du serveur, il faut `register` le `Listener` dans le `PluginManager`.
```java
PluginManager pluginManager = getServer().getPluginManager();

pluginManager.registerEvents(new ListenerJoin(), this);
```

5. Et voilà ! Maintenant, quand un joueur se connectera sur votre serveur, le contenu de la fonction que vous avez créé sera exécuté.

> [!TIP]
> Regardez bien toutes les fonctions disponibles dans l'event (`PlayerJoinEvent`) !

## 📚 Vos devoirs

- Quand un joueur rejoint / quitte le serveur, modifiez le message envoyé par défaut par celui de votre choix (il doit inclure le pseudo du joueur !).
- Quand un joueur envoie un message dans le chat, modifiez le formattage par défaut (doit inclure le pseudo du joueur et le message qu'il a envoyé !).
- Quand un joueur pose un bloc "interdit" (ceux que vous voulez, de la pierre par exemple), annulez la pose et lui envoyez lui un message pour lui dire que ce bloc est "interdit".
- Faites une commande "/customgive" qui donne une épée en fer avec les caractéristiques suivantes :
  - Nom : `Excalibur` (en rouge)
  - Enchantments : Sharpness 2, Unbreaking 3
  - Incassable
 
### Bonus
Si vous êtes arrivés ici, je vous félicite 👏\
Je vous encourage à aller plus loin : vous pouvez mixer tout ce que vous avez appris pour faire un mini-jeu ! Inspirez vous d'autres mini-jeux, essayez de les reproduire !\
Les plus simples à faire sont les mini-jeux de types FFA, car aucun système de partie n'est requis.

## Outro

Si vous êtes curieux de voir à quoi ressemble un code de production d'un "vrai" serveur Minecraft,
je vous invite à regarder les repos d'[Hashtek](https://github.com/hashtek-mc) et de [SamaGames](https://github.com/SamaGames).

Merci d'avoir été présent pendant ce Workshop ! 🌟💜

## Fait avec 💜 par [Lysandre B.](https://github.com/shuvlyy)
