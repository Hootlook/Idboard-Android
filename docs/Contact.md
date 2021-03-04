<h1>Résumé</h1>
La page de contact permet de contacter directement l'école par mail ou par téléphone.

<h1>Architecture</h1>
<h3><a name="contactActivity">ContactActivity.kt</a></h3>

ContactActivity contient simplement deux variables 


```
val email = findViewById(R.id.email) as TextView
val call =  findViewById(R.id.call) as TextView
```

- email permet d'avoir un bouton cliquable
- call permet d'avoir un bouton cliquable

L'activity contient un setOnClickListener qui permet d'ecouter si un clique est effectué sur le texte de l'appel

```
call.setOnClickListener(
            {
                val tel = "0483066660"
                val intent = Intent(Intent.ACTION_DIAL, Uri.parse("tel:" + Uri.encode(tel)))
                startActivity(intent)
            }
        )
```

Si un clique est effectué on fait plusieurs opérations :

- on crée une variable tel qui contient le numéro de téléphone de l'école
- on crée une variable intent qui va ouvrir notre application téléphone et parser le numéro pour l'afficher directement
- on ouvre notre application téléphone avec startActivity
PRIVATE_MODE permet de protéger l’accès à nos données , il faut le déclarer de nouveau dans une autre activity pour pouvoir accéder aux données.

L'activity contient un setOnClickListener qui permet d'ecouter si un clique est effectué sur le  texte du mail

```
email.setOnClickListener(
            {
             sendEmail()
        )
```

Si un clique est effectué on fait une operation :

- on appel notre fonction sendEmail()

```
private fun sendEmail(){

        val mailto = "scolarite@campusid.eu"
        val mIntent = Intent(Intent.ACTION_SEND)

        mIntent.data = Uri.parse("mailto:")
        mIntent.type = "message/rfc822"
        mIntent.putExtra(Intent.EXTRA_EMAIL, arrayOf(mailto))

        if( mIntent.resolveActivity(getPackageManager()) != null )
            startActivity(mIntent);

    }
```

sendEmail() effectue plusieurs opérations :

- mailto qui est le mail de l'école
- mIntent qui dit à Android qu'on va utiliser l'action d'envoyer
- mIntent.data qui va parser notre adresse mail
- mIntent.type qui utilise un format exprés pour supporter le texte du mail
- mIntent.putExtra qui va passer en paramètre notre mail
- mIntent.resolveActivity qui va essayer de trouver des applications compatibles pour envoyer le mail.

Une fonctionnalité qui se fait automatiquement permet de se souvenir de notre choix , et donc de choisir notre application mail par défaut.
