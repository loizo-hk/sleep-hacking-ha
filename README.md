# sleep-hacking-ha
Sleep Hacking pour Home Assistant

## Disclamer - Attention !
Tout d'abort ce dépot GIT et les documents qui s'y trouvent ne représentent pas de conseils médicaux mais mon avis personnel et le fruit de mes recherches réalisés principalement à l'aide de chatGPT o3 avec la recherche approfondie et ou internet activé pour qu'il me résume les études scientifiques et me donne les formules, algo à mettre en place. 

## L'objectif
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

  Pour calculer le SD il faut simplement l'heure de couché donc un simple capteur de présence suffit.
  Dans Home Assistant avec Emfit j'ai donc ceci dans un fichier template.yaml

\# un premier trigger pour récupérer l'heure de coucher la nuit (la plage horaire sert à traiter distinctement les sieste des nuits)
  - trigger:
    - platform: state
      entity_id: binary_sensor.in_bed_smooth
      to: 'on'
      for: '00:05:00'
  condition:
    - condition: time           # fenêtre nuit
      after: "18:00:00"
      before: "05:00:00"
  sensor:
    - name: "Heure de coucher"
      state: '{{ now().strftime("%H:%M:%S") }}'
      unique_id: "heure_coucher_emfit"
      icon: mdi:bed

\# un second trigger pour récupérer l'heure des siestes si il y a (la plage horaire sert à traiter distinctement les sieste des nuits)

  - trigger:
    - platform: state
      entity_id: binary_sensor.in_bed_smooth
      from: "off"
      to:   "on"
      for:  "00:10:00"          # sieste courte
  condition:
    - condition: time
      after: "11:00:00"
      before: "17:00:00"
  sensor:
    - name: "Dernière sieste"
      device_class: timestamp
      state: '{{ now().strftime("%H:%M:%S") }}'
      icon: mdi:bed
      
\# Un sensor binaire pour lisser les présence dans le liet et éviter qu'un réveil de 10mn (pause toilette) soit compté, les articles scientifiques considère qu'en dessous de 10mn de réveil ça ne compte pas.
  - binary_sensor:
    - name: "In‑bed lissé"
      unique_id: in_bed_smooth
      device_class: occupancy
      state: "{{ is_state('binary_sensor.emfitqs_002105_bed_presence', 'on') }}"
      delay_off: "00:10:00"       # fusionne les sorties <10 min
      delay_on:  "00:00:30"       # évite les faux on/off rapides
      

  à continuer ... 
  # Todo 
  Avancer sur les autres calculs
  Entrainer un petit model de machine learning pour améliorer la précision et les détection de phases Wake / Light / Deep / REM
  Avoir un score du sommeil le plus fiable possible
  Faire un dashboard
  Ajouter les sources des informations cités
  Mettre tout ça dans une intégration
  
