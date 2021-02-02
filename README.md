# Les jeux vidéos
---

[1. Introduction](Introduction)

[2. Jeu de données](Jeu de données)

[3. Analyse et visualisation](Analyse et visualisation)

---

## 1. Introduction
Pour cet examen, j'ai choisi le thème des jeux vidéos sans véritable restriction dès le départ. J'ai choisi de partir de rien pour avoir un jeu de données sur ce thème. En effet ayant trouvé peu de jeu de données abordant ce dernier, je me suis mis a réfléchir sur la façon d'en construire. Le fait d'avoir traiter wikidata et openrefine en cours m'a permis de le créer selon les critères que je souhaitais. 
* Voici la requête sparql utilisée pour récupérer les instances wikidata :

```sparql
SELECT ?item ?itemLabel
WHERE {
   ?plateform wdt:P279 ?type;
               wdt:P176|wd:P178 ?entreprise;
               wdt:P577|wdt:P571 ?dateSortiePlateform FILTER ("1993-01-01"^^xsd:dateTime < ?dateSortiePlateform).
    ?item wdt:P31 wd:Q7889;
          wdt:P400 ?plateform;
          wdt:P136 ?_genre;
          wdt:P123 ?_editeur;
          
    OPTIONAL {?item  wdt:P495 ?_pays;
                     wdt:P18 ?image
             }
    VALUES ?type {wd:Q17589470 wd:Q27496624}
    VALUES ?entreprise {wd:Q18594 wd:Q8093 wd:Q2283 wd:Q463094}
    SERVICE wikibase:label { bd:serviceParam wikibase:language "en".}
}
```
Cette requête me renvoie un résultat de 22 679 ce qui est normal puisque certains jeux apparaissent plusieurs fois du fait des différentes plateformes, genres et pays . J'ai donc du revoir la reguête en les regroupant par titre, comme on le montre la requête suivante :

```sparql
SELECT ?titre
WHERE {
   ?plateform wdt:P279 ?type.
   ?plateform wdt:P176|wd:P178 ?entreprise.
   ?plateform wdt:P577|wdt:P571 ?dateSortiePlateform FILTER ("1993-01-01"^^xsd:dateTime < ?dateSortiePlateform).
   ?titre wdt:P31 wd:Q7889.
   ?titre wdt:P400 ?plateform.
   ?titre wdt:P136 ?_genre.
   ?titre wdt:P123 ?_editeur.
          
    OPTIONAL {?titre wdt:P495 ?_pays.
              ?titre wdt:P18 ?image.
             }
    VALUES ?type {wd:Q17589470 wd:Q27496624}
    VALUES ?entreprise {wd:Q18594 wd:Q8093 wd:Q2283 wd:Q463094}
    SERVICE wikibase:label { bd:serviceParam wikibase:language "en".}
}
GROUP BY ?titre
```
On obtient maintenant un résultat de 6843 jeux vidéos. Je ne pense pas que ce résultat soit exhaustive, une requête plus poussée pourrait surement le permettre.

---

## 2. Jeu de données


Le jeu de données se présente ainsi :
<iframe style="width: 80vw; height: 50vh; border: none;" src="https://query.wikidata.org/embed.html#SELECT%20%3Ftitre%0AWHERE%20%7B%0A%20%20%20%3Fplateform%20wdt%3AP279%20%3Ftype.%0A%20%20%20%3Fplateform%20wdt%3AP176%7Cwd%3AP178%20%3Fentreprise.%0A%20%20%20%3Fplateform%20wdt%3AP577%7Cwdt%3AP571%20%3FdateSortiePlateform%20FILTER%20%28%221993-01-01%22%5E%5Exsd%3AdateTime%20%3C%20%3FdateSortiePlateform%29.%0A%20%20%20%3Ftitre%20wdt%3AP31%20wd%3AQ7889.%0A%20%20%20%3Ftitre%20wdt%3AP400%20%3Fplateform.%0A%20%20%20%3Ftitre%20wdt%3AP136%20%3F_genre.%0A%20%20%20%3Ftitre%20wdt%3AP123%20%3F_editeur.%0A%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20OPTIONAL%20%7B%3Ftitre%20wdt%3AP495%20%3F_pays.%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3Ftitre%20wdt%3AP18%20%3Fimage.%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20VALUES%20%3Ftype%20%7Bwd%3AQ17589470%20wd%3AQ27496624%7D%0A%20%20%20%20VALUES%20%3Fentreprise%20%7Bwd%3AQ18594%20wd%3AQ8093%20wd%3AQ2283%20wd%3AQ463094%7D%0A%20%20%20%20SERVICE%20wikibase%3Alabel%20%7B%20bd%3AserviceParam%20wikibase%3Alanguage%20%22en%22.%7D%0A%7D%0AGROUP%20BY%20%3Ftitre" referrerpolicy="origin" sandbox="allow-scripts allow-same-origin allow-popups" ></iframe>

Le but maintenant est alors de le traiter avec OpenRefine pour réconciler le jeu de données et pouvoir l'augmenter avec la réconcialiation. Or lors de la réconciliation à partir de l'instance wikidata, la restriction mise en place dans la requête n'est plus valide et toutes les informations qui sont en lien avec une propriété sont récupérées. Ce la peut ou non poser problème mais ici, j'aurais tendance à dire que cela permettra d'avoir plus de possibilité de visualisation je pense.

Les modifications apporter et les augmentations faites avec OpenRefine sont dans le dossier compressé Examen_dataviz_M2DEFI_2021_Julien_Mattei qui contient les fichiers Jeux_Videos.csv et Modifications_OpenRefine_Jeux_Videos.json.

Après l'augmentation et le nettoyage du jeu de données, il ressort finalement 166 résultats qui sont divisés en  catégories distinctes :
* titre
* pays d'origine (country of origin)
* genre
* la date de sortie (publication date)
* la plateforme ou console (platform)
* le mode de jeu (game mode)
* l'âge auquel on peut jouer (PEGI rating)

---      

## 3. Analyse et visualisation
Pour cette partie, j'ai décidé de créer une story avec [Flourish](https://app.flourish.studio/projects)

C'est partie pour une courte story :

<iframe src='https://flo.uri.sh/story/748703/embed' title='Interactive or visual content' frameborder='0' scrolling='no' style='width:100%;height:600px;'></iframe><div style='width:100%!;margin-top:4px!important;text-align:right!important;'><a class='flourish-credit' href='https://public.flourish.studio/story/748703/?utm_source=embed&utm_campaign=story/748703' target='_top' style='text-decoration:none!important'><img alt='Made with Flourish' src='https://public.flourish.studio/resources/made_with_flourish.svg' style='width:105px!important;height:16px!important;border:none!important;margin:0!important;'> </a></div>
