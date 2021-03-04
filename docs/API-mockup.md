IDCity mockup

Les params `:id` ne seront certainement pas présent dans l'api finale,
mais mettre en place la session prendrait trop de temps.

La racine de l'API est `http://dyndns.delchris.fr/mockup/`.
Les ids des utilisateurs ont le même format que ceux pour campusid, donc les URL ont cette tête là:
`http://dyndns.delchris.fr/mockup/annonces/notMine/2015168`

### GET Methods
```
/users
    /one/:id        - Retourne l'utilisateur [:id]
    /classe/:id     - Retourne les utilisateurs présents dans la classe [:id]
    /mark/:id       - Retourne les notes de l'utilisateur dont l'id est [:id]
    /randomId       - Retourne un idBusinessEntity aléatoire parmis ceux qui existent
    /personalInformation -Retourne les infos complète de l'utilisateur 2010999 selon les specs fournies par le back'.

/messages
    /globaux        - Retourne les messages globaux
    /:id            - [Deprecated]Retourne les messages de l'utilisateur dont l'id est [:id]

/annonces
    /mine/:id        - Retourne les annonces postés par l'utilisateur dont l'id est [:id]
    /notMine/:id     - Retourne toutes les annonces dont l'utilisateur [:id] n'est pas le créateur.
    /all             - Retourne toutes les annonces

/convention
    /:id             - Retourne la liste des demandes de convention de stages pour l'utilisateur [:id]

/trombi
    /:id             - Retourne la liste des utilisateurs la classe [:id], format spécifiquement pour le trombinoscope

/courses             
    /                - Retourne la liste de tous les cours, pas de filtre sur un identifiant car peu de données ont été fournis, et ça diluerais bien trop la requête.

/offers 
    /                - Retourne toutes les offres de stages
    /one/:id         - Retourne l'offre de stage d'id [:id]

/event
    /                - Retourne les messages à destination d'un seul utilisateur.
```

### PUT Methods

Pour utiliser ces points, il suffit de `PUT` un json sur l'url spécifiée.
Le json doit toujours contenir la valeur permettant d'identifier le tuple devant être modifié.
Les modifications ne se font que pour un tuple à la fois.
```
/users               - update un utilisateurs identifié par idBusinessEntity
/messages            - update un message identifié par id
/marks               - update une note identifié par idMark
/annonces            - update une annonce identifiée par id
/matters             - update une matière identifiée par idMatter
/classes             - update une Classe identifiée par idClass
```

### POST Methods
Pour utiliser ces points, il suffit de `POST` un json sur l'url spécifiée.
Les créations ne se font que pour un tuple à la fois.

```
/users               - La dateOfBirth doit être spécifié au format 'YYYY-mm-dd';
                       Le idBusinessEntity doit être spécifié, une erreur sera renvoyé s'il est déjà pris
/messages            - RAS
/marks               - RAS
/anonces             - RAS
/matters             - RAS
/classes             - RAS
```