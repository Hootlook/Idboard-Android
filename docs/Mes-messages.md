<h1>Résumé</h1>
Ce module affiche les messages personnelles de l'eleve 

Les messages est un module analogue affichant les messages envoyés à l’élève

Les messages se basent sur ce modèle : [mockup de messages](https://zpl.io/V15vBmX).
<h1>Architecture</h1>
<h3>MessagesActivity.kt</h3>

Activités principales pour les messages.
Elle récupère la liste des messages via [le service](#eventService).

```kotlin
EventService().getAll(onEventResponse, this)
```

La fonction onEventResponse s'occupe ensuite d'initialiser un [adapter](#eventAdapter) qui remplira une recyclerView.

La fonction onErrorResponse affiche un [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts) si une erreur survient.

<h3><a name="messageAdapter">EventAdapter.kt</a></h3>

Classe récupérant la liste des messages à afficher et l'adaptant pour la view. Elle commence par créer un [ViewHolder](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView.ViewHolder) possédant deux champs de texte, un pour la date et un pour le contenu du message.

Elle bind ensuite le ViewHolder ainsi créé et la liste pour afficher tous les messages.

Une fonction getItemCount est disponible. 
<h3><a name="eventService">EventService.kt</a></h3>

Classe servant d'intermédiaire entre l'application et l'API. Elle possède une fonction : getAll. Cette fonction prends pour paramètre une fonction onMessageResponse et une fonction onErrorResponse.

<h3>Event.kt</h3>

Modèle des messages. Elle contient les membres :
- idEvent (int)
- idBusinessEntity (int)
- idTypeEvent (int)
- idSystemLevel (int)
- bbcode (string)
- date (string)
- idBusinessEntityNavigationStart(int)
- idSystemLevelNavigation(int)
- idTypeEventNavigation (int)

La classe possède une fonction fromJSONObject pour créer un message à partir d'un objet JSON.
