# Les jeux vidéos
## Introduction
Pour cet examen, j'ai choisi le thème des jeux vidéos sans véritable restriction dès le départ. J'ai choisi de partir de rien pour avoir un jeu de données sur ce thème. En effet ayant trouvé peu de jeu de données abordant ce dernier, je me suis mis a réfléchir sur la façon d'en construire. Le fait d'avoir traiter wikidata et openrefine en cours m'a permis de le créer selon les critères que je souhaitais. 
* Voici la requête sparql utilisée pour récupérer les instances wikidata :

'''sparql
SELECT ?item
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
'''sparql

## Jeu de données
## Visualisation


