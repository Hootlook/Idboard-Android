<h1>Résumé</h1>
Le module permet de voir les notes recues.

<h1>Architecture</h1>
<h3>MarksActivity.kt</h3>

MarksActivity est l'activité principale pour les notes. Elle va récupérer la liste des notes via le service <strong>MarkService.kt</strong>

On commence par une liste de variable

    `var loader: ProgressBar? = null
	 val apiService = MarkService()`

Explication des paramètres :
<ul>
    <li>loader sert pour la barre de chargement</li>
    <li>apiService permet de faire appel à notre <strong>MarkService.kt</strong></li>
</ul>

<p>On récupère nos notes grâce à notre service qui est le suivant</p>

    `apiService.getContext(this@MarksActivity)
     apiService.getAllMarksForAStudent {
        if(it!=null){
            loader?.visibility = View.GONE
            var domains = it
            if (domains != null) {
                domainList = domains as List<Domain>
                fragmentData.putParcelableArrayList("domains", domains as ArrayList<Domain>)
                fragmentDomains?.arguments = fragmentData
                if (!fragmentDomains!!.isAdded) {
                    fm.beginTransaction().add(R.id.frame_container, fragmentDomains as DomainsFragment)
                                .commit()
                    currentFragment = "domains"
                }
            }
        }
    }`


<p>Notre fonction s'occupe ensuite d'initialiser un adapter (<Strong>MessageAdapter</strong>) qui remplira une recyclerView.</p>

<h3>MessageAdapter.kt</h3> 
<p>MessageAdapter.kt est appelé dans matterAdapter.kt <strong>A compléter</strong></p>

<h3>MarkService.kt</h3>
<p>MarkService.kt est la classe qui sert d'intermédiaire entre l'application et l'API. Elle possède une fonction getAllMarksForAStudent qui prend en paramètre un context.</p>

    `fun getAllMarksForAStudent(onResult: (List<Domain>?) -> Unit){
     val retrofit = ServiceBuilder.buildService(RestApi::class.java)
     retrofit.getAllMarksForAStudent(context).enqueue(
        object : Callback<List<Domain>>{
            override fun onResponse(call: Call<List<Domain>>, response: retrofit2.Response<List<Domain>>) {
                val domain = response.body()

                    .
                    .
                    .
                    
                notificationManager = context.getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                
                    // on paramètre notre notification
                    
                    notificationChannel = NotificationChannel(channelId, description, NotificationManager.IMPORTANCE_HIGH)
                    notificationChannel.enableLights(true)
                    notificationChannel.lightColor = Color.GREEN
                    notificationChannel.enableVibration(false)
                    notificationManager.createNotificationChannel(notificationChannel)

                    // On lui donne la forme souhaité (icone, texte...)
                    
                    builder = Notification.Builder(context, channelId)
                                          .setSmallIcon(R.drawable.campusid)
                                          .setContentText("Une nouvelle note est disponible")
                                        //.setContentIntent(pendingIntent)
                } else {
                
                    // On lui donne la forme souhaité (icone, texte...)
                    
                    builder = Notification.Builder(context)
                                          .setContentText("Une nouvelle note est disponible")
                                          .setSmallIcon(R.drawable.campusid)
                                        // .setContentIntent(pendingIntent)
                }
                notificationManager.notify(12345, builder.build())
                    
                    .
                    .
                    .
                    
                onResult(domain)
            }
            override fun onFailure(call: Call<List<Domain>>, t: Throwable) {
                onResult(null)
            }
        }
    )
    }`

<h3>Mark.kt</h3> 
<p>Mark.kt est le modèle pour les notes. Elle contient :</p>

<ul>
    <li>label</li>
    <li>matters</li>
    <ul>
        <li>reference</li>
        <li>label</li>
        <li>marksNumber</li>
        <li>marks</li>
        <ul>
            <li>value</li>
            <li>id</li>
            <li>typeEvaluation</li>
            <li>coefficient</li>
            <li>comments</li>
            <li>isJustifiedAbsence</li>
        </ul>
    </ul>
</ul>