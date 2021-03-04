Ce module permet de faire une demande de Certificat de scolarité auprès de l'administration par mail.


Les messages se basent sur ces modèles :

* [mockup de la page de certificat de scolarité](https://zpl.io/bLEGnjB).

# Architecture

<h3><a name="CertificatScolariteActivity">CertificatScolariteActivity.kt</a></h3>

Activité principale pour la demande de certificat. Elle récupère la liste des classes étudiées via [le service](#CertificatService).

```kotlin
loginPreferences.getString("username", "")?.let { it: String ->
            CertificatService().getAllClasses(onCertificateService,this, it)
        }
```

La fonction onScolariteResponse s'occupe ensuite d'initialiser un Array Adapter qui sera initialisé dans le spinner afin d'afficher les classes dans le Spinner

```kotlin
var onCertificateService = fun(classes: List<String?>?){
        val spinner = findViewById<Spinner>(R.id.certificate_year_spinner)
        if (classes != null) {

            if( spinner != null ){
                val adapter = ArrayAdapter(
                    this,
                    R.layout.component_spinner_element,
                    classes)
                adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
                spinner.adapter = adapter
            }
        }
        loader?.visibility = View.GONE

    }
```
Permet de récupérer l'évenement de click sur le bouton envoyer, afin d'envoyer un mail

```kotlin
val btn_send = findViewById<Button>(R.id.certificate_send_btn)

        btn_send.setOnClickListener {
            val selected = spinner.selectedItem.toString()
            sendEmail(nomentier , email.toString() , selected)
            // TODO: call the service
        }
    }
```

La fonction sendEmail, utilise un Intent pour envoyer des informations dans une autre application, de mail en l'occurence.

```kotlin 
private fun sendEmail(nomEntier :String , email : String , selected : String ){
```

La fonction onErrorResponse affiche un [toast](https://developer.android.com/guide/topics/ui/notifiers/toasts) si une erreur survient.


<h3><a name="CertificatService">CertificatService.kt</a></h3>


Classe servant d'intermédiaire entre l'application et l'API. Elle possède une fonction : getAllClasses. Cette fonction prend pour paramètre une fonction onResponse, une fonction onErrorResponse et l'id de l'utilisateur connecté.

