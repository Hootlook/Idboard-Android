<h1>Résumé</h1>
Ce module affiche les messages reçus par les élèves. 

Les messages globaux sont un module analogue affichant les messages envoyés aux classes voire à toute l'école.

Les messages se basent sur ce modèle : [mockup de messages](https://zpl.io/VO7pnNX).
<h1>Architecture</h1>
<h3>MessagesActivity.kt & MessagesGlobauxActivity.kt</h3>

Activités principales pour les messages.
Elle récupère la liste des messages via [le service](#messageService).

```kotlin
MessageService().getAll(onMessageResponse, this)
```

La fonction onMessageResponse s'occupe ensuite d'initialiser un [adapter](#messageAdapter) qui remplira une recyclerView.

La fonction onErrorResponse affiche un [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts) si une erreur survient.
<h3><a name="messageAdapter">MessageAdapter.kt</a></h3>

Classe récupérant la liste des messages à afficher et l'adaptant pour la view. Elle commence par créer un [ViewHolder](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView.ViewHolder) possédant deux champs de texte, un pour la date et un pour le contenu du message.

Elle bind ensuite le ViewHolder ainsi créé et la liste pour afficher tous les messages.

Une fonction getItemCount est disponible. 
<h3><a name="messageService">MessageService.kt</a></h3>

Classe servant d'intermédiaire entre l'application et l'API. Elle possède deux fonctions : getAll et getAllByUser. Ces deux fonctions prennent pour paramètre une fonction onMessageResponse et une fonction onErrorResponse.

getAllByUser prend également, en troisième paramètre, l'id de l'utilisateur.

<h3>Message.kt</h3>

Modèle des messages. Elle contient les membres :
- id (int)
- value (string)
- idBusinnessEntity (string)
- dateStart (string)
- dateEnd (string)

La classe possède une fonction fromJSONObject pour créer un message à partir d'un objet JSON.