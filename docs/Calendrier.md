# Résumé
Le module affiche le calendrier ainsi qu'une liste de cours et/ou partiels.

En cliquant sur un jour en particulier, on obtient une liste détaillé sur notre journée de cours.

Pour notre appel http, on se base [sur un appel vers l'api](http://idboard.net:9000/student-api/Courses/courses-in-dates/66/2019-10-01/2020-07-01).

On utilise cette [librairie](https://github.com/SundeepK/CompactCalendarView).


# Architecture

<h3><a name="mainActivity">MainActivity.kt</a></h3>

MainActivity contient toutes les informations nécessaires pour pouvoir utiliser le calendrier, ainsi que pour afficher les cours.

On commence par une liste de variable
```kotlin
val showPreviousMonth = findViewById<View>(R.id.prev_button)
val selectedMonth = findViewById(R.id.monthOfCalendar) as TextView
val showNextMonth = findViewById<View>(R.id.next_button)
val listView = findViewById<RecyclerView>(R.id.listViewForCalendar)
val compactCalendarView =findViewById<View>(R.id.compactcalendar_view) as CompactCalendarView
```
Explication des paramètres :
- showPreviousMonth permet d'utiliser un bouton pour aller au mois précédent

- selectedMonth permet d'afficher le mois actuel

- showNextMonth permet d'utiliser un bouton pour aller au moins suivant

- listView qui est la liste ou s'afficher les cours

- compactCalendarView correspond à notre calendrier


On récupère la liste des cours grâce à notre service qui est le suivant
```kotlin
 CoursesService().getAll(onCoursResponse, this)
```
La fonction onMessageResponse s'occupe ensuite d'initialiser un [adapter](#coursListadapter) qui remplira une recyclerView qui est ici listView.

Lorsque on reçoit la reponse de notre back , on va utiliser notre variable onCoursResponse et on va boucler sur notre réponse pour pouvoir créer des événements.

Un événement peut-être un cours ou un partiel par exemple.

Un événement contient :
- Horaire du début du cours
- Horaire de fin du cours
- Description du cours
- Prénom du professeur
- Nom du professeur

```kotlin       
        for (cours in listCours) {
            val pattern = "HH:mm"
            val simpleDateFormat = SimpleDateFormat(pattern, Locale.FRANCE)
            val horaireDebut = simpleDateFormat.format(cours.dateStart)
            val horaireFin = simpleDateFormat.format(cours.dateEnd)
            // on crée un evenement avec la couleur , la date , et les informations necessaires
            val ev = Event(
                        createColor(currentCour.backgroundColor),
                        zdtS.toInstant().toEpochMilli(),
                        horaireDebut+ " - " + horaireFin + "\n" + currentCour.label + "\n" + currentCour.teacherDetails.firstName + " " + currentCour.teacherDetails.lastName
                )
            // on ajoute l'evenement
            compactCalendarView.addEvent(ev)
        }
```
On utilise un pattern pour afficher uniquement l'heure et les minutes sur un cours.

Une fonction permet de créer la couleur par rapport au String envoyé par l'api.

```kotlin

    private fun createColor(colorString : String ) : Int{
        var color = "#" + colorString
        // on convertit en Int
        color = Color.parseColor(color).toString()
        return color.toInt()
    }

```

Pour placer le cours à une date , on utilise le .time pour pouvoir avoir la date en millisecondes (obligatoire sur ce calendrier).

La fonction onErrorResponse affiche un [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts) si une erreur survient.

Notre calendrier utilise des paramètres provenant directement de la librairie comme 

```kotlin
compactCalendarView.setFirstDayOfWeek(Calendar.MONDAY)
compactCalendarView.setLocale(TimeZone.getDefault(), Locale.FRANCE)
compactCalendarView.setUseThreeLetterAbbreviation(true)
```
- setFirstDayOfWeek permet de choisir le premier jour de la semaine

- setLocale permet de définir la date française

- setUseThreeLetterAbbreviation permet d'afficher les mois sous format 3 lettres

Lors d'un appel à l'api , nous recevons toute une liste de cours
.
Pour pouvoir afficher uniquement les cours du jour sélectionné ( lors d'un clique sur une date) , on a du créer un évenement lors d'un clique sur le calendrier.

```kotlin
compactCalendarView.setListener(object : CompactCalendarViewListener {
            override fun onDayClick(dateClicked: Date) {
                adapter.coursList = listCours.filter {
                    calendar.time = it.dateStart
                    val dayOne = calendar.get(Calendar.DAY_OF_YEAR)
                    calendar.time = dateClicked
                    val dayTwo = calendar.get(Calendar.DAY_OF_YEAR)
                    dayOne == dayTwo
                }
                listView.adapter = adapter
                listView.visibility = View.VISIBLE
                adapter.notifyDataSetChanged()
                selectedMonth.setText(upperFirstLetter(dateFormatForMonth.format(dateClicked)))
            }
```
Pour expliquer facilement le fonctionnement il est important de se rappeler "à quoi sert un adapter ?".

Un adapter à pour mission de recevoir une liste (ici notre liste de cours) et de l'adapter à notre liste  (ici listView).

On utilise la fonction .filter pour filtrer les cours selon la date cliqué.

 - val dayOne correspond à notre date reçu dans la liste

 - val dayTwo correspond à notre date cliqué dans le calendrier

On fait ensuite une comparaison pour voir si les 2 jours sont égaux.

Si ils sont égaux , alors ils sont ajoutés dans notre adapter.

<h3><a name="coursListadapter">CoursListAdapter.kt</a></h3>

Classe récupérant la liste des cours à afficher et l'adaptant pour la view. Elle commence par créer un [ViewHolder](https://developer.android.com/reference/kotlin/androidx/recyclerview/widget/RecyclerView.ViewHolder) possédant trois champs :
 - Une pastille de couleur (viewPastille) pour afficher le type de données (cours/ partiel)
 - Un champ pour afficher les informations du cours (textViewCours)
 - Un champ pour afficher les informations sur les horaires (textViewHoraire)

Elle bind ensuite le ViewHolder ainsi créé et la liste pour afficher tous les messages.

Une fonction getItemCount est disponible.

<h3><a name="coursService">CoursService.kt</a></h3>

Classe servant d'intermédiaire entre l'application et l'API. Elle possède une fonction : getAll. Cette fonction prends pour paramètre une liste de type Cours et une fonction onErrorResponse.

<h3><a name="coursClass">Cours.kt</a></h3>

Modèle des cours. Elle contient les membres :

* idCourse (int)
* idClass (int)
* dateStart (Date)
* dateEnd (Date)
* teacherName (string)
* teacherFirstName (string)
* teacherSurname (string)
* descriptionDefaultValue (string)
* typeDefaultValue (string)
* backgroundColor (string)
* fontColor (string)

Petite particularité de cette classe :

- Elle contient une fonction createColor pour ajouter un # à notre backgroundColor afin d'avoir une couleur sous format hexadecimal.
- Elle contient
```kotlin       
val pattern = "yyyy-MM-dd'T'HH:mm:ss"
val simpleDateFormat = SimpleDateFormat(pattern, Locale.FRANCE)
```
Ces variables sont utiles au moment de la creation d'un cours, pour permettre de convertir les dates de type string en type Date.

La classe possède une fonction fromJSONObject pour créer un cours à partir d'un objet JSON.

