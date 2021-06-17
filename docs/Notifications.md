# Résumé
Le module permet de voir les notes recues.

# Architecture
### MarksActivity.kt

MarksActivity est l'activité principale pour les notes. Elle va récupérer la liste des notes via le service **MarkService.kt**

On commence par une liste de variable

    `var loader: ProgressBar? = null
	 val apiService = MarkService()`

Explication des paramètres :

- loader sert pour la barre de chargement
- apiService permet de faire appel à notre **MarkService.kt**


On récupère nos notes grâce à notre service qui est le suivant

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


Notre fonction s'occupe ensuite d'initialiser un adapter (**MessageAdapter**) qui remplira une recyclerView.

### MarkService.kt
MarkService.kt est la classe qui sert d'intermédiaire entre l'application et l'API. Elle possède une fonction getAllMarksForAStudent qui prend en paramètre un context.

    `fun getAllMarksForAStudent(onResult: (List<Domain>?) -> Unit){
     val retrofit = ServiceBuilder.buildService(RestApi::class.java)
     retrofit.getAllMarksForAStudent(context).enqueue(
        object : Callback<List<Domain>>{
            override fun onResponse(call: Call<List<Domain>>, response: retrofit2.Response<List<Domain>>) {
                val domain = response.body()
                    
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
                    
                onResult(domain)
            }
            override fun onFailure(call: Call<List<Domain>>, t: Throwable) {
                onResult(null)
            }
        }
    )
    }`

### Mark.kt 
Mark.kt est le modèle pour les notes. Elle contient :


- label
- matters
    
    - reference
    - label
    - marksNumber
    - marks
... (8 lignes restantes)
