# sleep-hacking-ha
Sleep Hacking pour Home Assistant

L'objectif de ce dépot est de capitaliser des connaissance sur le sleep hacking et des métriques ou des aides via home assistant pour aider les gens à mieux dormir. 
Beaucoup de données médicales ou de capteurs opensources deviennent payant et obligent les gens à fournir leurs données médicales dans le cloud. 
Ici l'objectif principale est de tout garder en local. Il y aura probablement aussi des projets parallèles open hardware pour faire des capteurs libres. 

Ce projet ne sera jamais vendu, il doit rester libre et gratuit. 

Je le réalise sur mon temps libre, si vous voulez vous joindre à l'aventure faites moi signe !

## Les débuts : 
Le sommeil est l'une des pierres angulaire pour être en bonne santé, avoir des fonctions cognitives opérationnelles et vivre longtemps. 

Je me suis basé sur un capteur emfitQS v1 qui expose localement une api avec les informations sur : 
- la présence
- le temps au lit
- la fréquence cardiaque (FC) / Heart Rate (HR)
- la fréquence respiratoire (FR) / Respiratory Rate (RR)
- les mouvements (activity)

  Il est possible pour les metriques les plus basiques de se baser uniquement sur un capteur de présence on / off cela permet déjà d'améliorer pas mal de choses. Un radare micrométrique ou un capteur piezzo font l'affaire pour relativement peu chère si vous ne trouvez pas de emfitQS. Il serait intéressant de refaire un emfitQS libre maison mais pour l'étalonner il faut des données de polysomnographe... que je n'ai pas !
  Si vous n'avez vraiment pas de budget vous pouvez aussi faire un bouton sur l'appli home assistant pour dire couché ou pas couché.

  En effet les premières étapes se font simplement en mesurant la régularité des heures de sommeil. Sans capteur dédié ce sera moins précis mais la littérature scientifique publie déjà des choses à ce sujet.

  à continuer ... 
