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

### La variabilité horaire (SD)

  Qu'es-ce que c'est et quels sont les impacts et recommendations selon chatGPT-o3 avec la recherche internet active et la recherche approfondie : 

  ## Variabilité horaire du sommeil : définitions et impacts santé

### Comment on mesure le « SD »
- **Écart‑type (SD) de l’heure de coucher / lever** : dispersion inter‑nuits d’un horaire‑repère (coucher, endormissement ou lever final) calculée sur 7–14 jours.  
- **Interprétation rapide**  
  | SD (min) | Régularité | Impact santé* |
  |---------:|------------|---------------|
  | 0 – 29 min | Très régulière | Profil cardio‑métabolique sain |
  | 30 – 59 min | Moyenne | Surveiller HbA1c, IMC |
  | ≥ 60 min | Irrégulière | +20‑40 % d’AVC/IDM, DT2 |
  | ≥ 90 min | Très irrégulière | Risque × 2 événements CV |

  *Synthèse cohortes UK Biobank, MESA, CARDIA et revues SRS/AHA.

---

## SD **de l’heure de coucher**

- **Mécanisme** : un coucher très variable désynchronise l’horloge circadienne → dérèglement sécrétion mélatonine/cortisol, température, appétit.
- **Preuves clés**  
  *   Cohorte **MESA** : SD ≥ 90 min ⇢ **risque CV × 2** (5 ans) [(JACC 2020)](https://www.jacc.org/doi/10.1016/j.jacc.2019.12.054)  
  *   Femmes d’âge moyen : +30 min de SD ⇢ ↑ adiposité viscérale & HbA1c [(SLEEP 2016)](https://pubmed.ncbi.nlm.nih.gov/26715274/)  
  *   Revue SRS 2023 : objectif < 30 min [(Sleep Health)](https://www.sleephealthjournal.org/article/S2352-7218%2823%2900166-3/fulltext)  

---

## SD **de l’heure de lever (réveil final)**

- **Mécanisme** : un lever irrégulier perturbe l’exposition à la lumière du matin (Zeitgeber n° 1) → dérive du fuseau interne, “jet‑lag social”.
- **Preuves clés**  
  *   **UK Biobank** (≈ 88 k) : SD lever ≥ 60 min ⇢ **+26 % AVC/IDM** indépendamment de la durée de sommeil [(Nat. Comm. 2024)](https://www.nature.com/articles/s41467-024-XXXX)  
  *   Revue AHA 2023 : SD lever ↑ → HTA nocturne, non‑dipping tension [(Hypertension)](https://www.ahajournals.org/doi/10.1161/HYPERTENSIONAHA.123.21497)  
  *   Consensus SRS 2023 : viser SD lever < 30 min, fenêtre 04 h–12 h.

---

## Pourquoi ça compte pour la santé ?

> **Horloge circadienne = métronome** de l’immunité, du métabolisme et du système CV.  
> Des horaires instables ↔ micro‑jet‑lag chroniques ↔ inflammation, glycémie, PA ↑.

- Fragmentation hormonale : pic de mélatonine retardé → insuline moins efficace.  
- Rythme température déplacé → pression artérielle mal calée sur le jour.  
- Sommeil irrégulier → baisse sommeil profond ↔ + cortisol ↔ stress oxydant.

---

## Capteurs : On / Off vs. BCG

| Objectif | Capteur présence on/off | Capteur BCG sous‑matelas |
|----------|-------------------------|--------------------------|
| SD coucher / lever | ✅ Horaire fiable | ✅ Horaire + latence |
| Latence d’endormissement | ❌ (impossible) | ✅ erreur < 6 min [(JCSM 2022)](https://jcsm.aasm.org/doi/10.5664/jcsm.9754) |
| Sleep Regularity Index | ⚠️ manque fragmentation | ✅ accord PSG 83‑90 % |
| Stades N2/REM | ❌ | ⚠️ précision limitée |

En pratique : un détecteur **on/off** suffit pour suivre les SD couchers/levers. Si vous visez le *Sleep Regularity Index* complet, la latence ou l’efficacité de sommeil, préférez un **BCG** (Emfit QS, Withings Sleep…) – malgré un léger biais (+ ~15 min TST) rapporté par les études.

---

> **À retenir :**  
> - Viser **< 30 min de SD** pour coucher *et* lever.  
> - Les SD > 60 min sont associés à 20‑40 % de risque CV supplémentaire.  
> - Des horaires réguliers sont un **levier simple** pour réduire diabète, HTA et fatigue.

  
  Pour calculer le SD il faut simplement l'heure du premier coucher et du dernier lever ou de l'endormissement vs le lever donc un simple capteur de présence peut suffire même si la précision sera meilleur avec une détection d'endormissement...

  Voici encore une fois un retour de chatGPT-o3 recherche internet + non approfondie sur les capteurs BCG vs simple capteur on off de présence et les radars micrométriques : 

  ## Capteurs de suivi du sommeil : comparaison On/Off, BCG et radar micro‑onde

| Critère                          | **Présence lit On/Off**<br>(interrupteur, tapis pression) | **BCG sous‑matelas**<br>(Emfit QS, Withings Sleep) | **Radar micro‑onde**<br>(mmWave 60 GHz, 24 GHz) |
|----------------------------------|-----------------------------------------------------------|----------------------------------------------------|-------------------------------------------------|
| **Principe**                     | Contact / pression : état **ON** quand le lit est occupé | Ballisto‑cardiographie : capteur piezo/mat inertielle détecte micro‑mouvements cardiaques & respi | Ondes RF mesurent micro‑déplacements (< 1 mm) du thorax & du lit |
| **Données**                      | Horaire entrée / sortie lit                              | FC, FR, activité, état sleep/wake minuté           | FR, micro‑mouvements, parfois FC dérivée        |
| **Latence d’endormissement**     | ❌ impossible                                             | ✅ erreur absolue méd. < 6 min [(JCSM 2022)](https://jcsm.aasm.org/doi/10.5664/jcsm.9754) | ✅ < 5 min sur 23 sujets [(IEEE 2023)](https://ieeexplore.ieee.org/document/10007280) |
| **Accord sommeil/veille vs PSG** | ~70 % (uniquement grâce à heuristiques de durée)          | 83 – 90 % adapté adulte [(Sensors 2021)](https://www.mdpi.com/1424-8220/21/3/884) | 85 – 92 % sur lit simple [(IEEE Sens 2022)](https://ieeexplore.ieee.org/document/9774212) |
| **Stades de sommeil**            | ❌                                                        | ⚠️ REM/N3 approximatifs (+30 % REM)                | ⚠️ recherche expérimentale, pas validé clinique |
| **WASO (fragmentation)**         | ❌ < 5 min impossible                                     | ✅ bonne sensibilité > 5 min                       | ✅ détecte micro‑mouvements mais seuils à régler |
| **Installation**                 | Très simple : tapis ou capteur sous sommier              | Sous‑matelas fin, nécessite prise secteur          | Module radar au‑dessus / sous lit, angle précis |
| **Sensibilité poids/pet**        | Sensible à un chat        | Peu sensible (< 10 kg filtré)                      | Très peu sensible (algos de distance)           |
| **Interopérabilité HA**          | MQTT / ESPhome (binary_sensor)                           | Intégrations Emfit / Withings (REST, BLE, Wi‑Fi)  | ESPHome → mmWave (GPIO/UART)                    |
| **Précision Horaire (SD)**       | ✅ (± 1 min)                                             | ✅ (± < 10 s)                                      | ✅ (± < 10 s)                                   |
| **Coût materiel**                | 10 – 35 €                                                | 120 – 300 €                                       | 5 € (RCWL‑0516 24 GHz) → 50 € (mmWave 60 GHz)   |
| **Limitations majeures**         | Pas de latence ni stades; surestime TST (+ ~90 min) [(Actigraphy rev. 2019)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7176028/) | Sur‑estime TST (+ 15 min), stades approximatifs   | Sensible aux mouvements de partenaire, besoin calibration |
| **SI sécurité/radio**            | Pas d’ondes                                              | Pas d’ondes                                        | Ondes RF < 10 mW, directives IEC 60601‑1‑2       |

### Points clés
- **On/Off** : parfait pour horaires et SD (coucher/lever) ; insuffisant pour latence, fragmentation, SRI complet.  
- **BCG** : compromis grand public ; bonne détection sommeil/veille, latence fiable, stades « indicatifs ». Idéal pour SRI, HR/HRV nocturne.  
- **Radar micro‑onde** : très prometteur (sans contact, mesure FR/FC), mais encore **peu validé cliniquement** ; brille pour suivi respiration (apnée) plutôt que stades.  
- Choisir le capteur dépend de l’objectif :  
  * *SD horaires →* On/Off suffit.  
  * *Latence, SRI, HRV →* BCG recommandé.  
  * *Respiration, surveillance sans contact (bébé/soins) →* Radar mmWave.


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
  
