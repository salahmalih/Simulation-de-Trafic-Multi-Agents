## Simulation de Trafic Multi-Agents (NetLogo)

Ce projet simule la circulation urbaine autour d'une intersection en utilisant une approche multi-agents dans NetLogo.

Chaque voiture, feu de signalisation et evenement de collision est gere comme une entite autonome. Le modele permet d'etudier la fluidite du trafic, les temps d'attente, les collisions et l'impact des parametres de circulation.

---

## Objectif du projet

Le but est d'analyser le comportement du trafic a une intersection et de mesurer l'equilibre entre :

- la performance (vitesse moyenne, flux),
- la securite (collisions, quasi-collisions),
- la qualite des trajets (temps d'attente, temps de trajet).

---

## Equipe

- Abdo: Introduction, architecture et environnement
- Marwan: Systeme de controle et logique des feux
- Salah: Dynamique des voitures et securite
- Ziko: Analyse des resultats, metriques et conclusion

---

## Structure du modele

Le code repose sur quatre types d'agents NetLogo :

```netlogo
breed [ cars car ]
breed [ lights light ]
breed [ fires fire ]
breed [ timerlabels timerlabel ]
```

### Roles des agents

- `cars`: vehicules autonomes avec vitesse, distance de securite, logique de freinage et etat de collision.
- `lights`: feux de signalisation avec etat (`green`, `yellow`, `red`) et minuterie.
- `fires`: effet visuel apres collision.
- `timerlabels`: affichage du compte a rebours des feux.

---

## Variables principales

### Variables des voitures (`cars-own`)

- `speed`, `max-speed`: vitesse actuelle et limite individuelle.
- `safety-distance`: distance minimale avec la voiture devant.
- `car-ahead`: reference de la voiture detectee devant.
- `wait-time`: temps passe a l'arret.
- `crashed`, `crash-timer`: gestion des collisions et reinitialisation.
- `turning`, `stuck-timer`, `committed-to-stop`, `braking`: logique de comportement.
- `reaction-delay`, `caution-level`: prudence et temps de reaction.

### Variables des feux (`lights-own`)

- `state`: etat du feu.
- `timer-count`: compteur avant changement d'etat.
- `axis`: axe controle (`H` horizontal, `V` vertical).

### Variables globales (`globals`)

- couleurs de l'environnement,
- durees des feux (`green-duration`, `yellow-duration`, `red-duration`).

---

## Procedures cles du code

### 1. Initialisation

- `setup`: initialise le monde, les routes, les feux, les voitures, les graphiques et le fichier CSV.
- `setup-roads`: construit la carte (routes, intersections, marquages).
- `setup-lights`: place et configure les feux.
- `setup-cars`: cree les voitures sur des patches valides.

### 2. Boucle principale

La procedure `go` pilote chaque tick de simulation :

1. mise a jour des feux,
2. gestion des voitures crashées,
3. decision et deplacement des voitures actives,
4. correction anti-blocage,
5. mise a jour des effets (`fires`),
6. export CSV et graphiques,
7. `tick`.

### 3. Intelligence de conduite

- `make-intelligent-decision`: moteur de decision principal.
- `should-stop-for-light?`: decide l'arret selon feu + position.
- `can-go-on-green?`: autorise la reprise.
- `cross-traffic-danger?`: detecte le risque venant des axes croises.
- `apply-defensive-braking`: freinage defensif en cas de risque.
- `adjust-speed-smart`: ajuste finement la vitesse selon la distance et le contexte.

### 4. Mouvement et maintien sur la voie

- `detect-car-ahead`: detection de vehicule devant.
- `move-forward`: avance selon la vitesse.
- `align-with-lane`: maintien de l'orientation sur la voie.
- `stay-on-road`: repositionnement si la voiture sort de la route.

### 5. Securite et collisions

- `check-near-miss`: detecte les quasi-collisions.
- `check-collision-safe`: detecte et traite les collisions.
- `anti-deadlock-resolve`: debloque les voitures en impasse.

### 6. Evaluation des performances

- `initialize-plots`, `update-all-plots`: metriques en temps reel.
- reporters: `mean-speed`, `num-stopped-cars`, `num-crashes`, `collision-rate`, `mean-wait-time`, `mean-trip-time`, `cars-stuck-in-intersection`.
- `export-stats-to-csv`: export de donnees dans `trafic_stats.csv`.

---

## Fichiers du projet

- `SM.nlogox`: modele NetLogo principal.
- `New Text Document.txt`: version texte du code NetLogo.
- `trafic_stats.csv`: donnees exportees pendant la simulation.

---

## Plan de presentation (4 intervenants)

### 1) Abdo - Introduction et architecture

Slides:
- Contexte et objectifs
- Architecture multi-agents
- Environnement et routes

Code a presenter:
- definition des agents (`breed`)
- variables (`cars-own`, `lights-own`, `fires-own`, `globals`)
- `setup`, `setup-roads`, `setup-cars`

Message cle:
- modelisation decentralisee ou chaque voiture est autonome.

### 2) Marwan - Systeme de controle

Slides:
- Feux de signalisation
- Pipeline de decision

Code a presenter:
- `setup-lights`, `update-lights`
- `make-intelligent-decision`
- `should-stop-for-light?`, `can-go-on-green?`, `cross-traffic-danger?`

Message cle:
- la decision combine etat du feu, distance de securite et contexte intersection.

### 3) Salah - Dynamique et securite

Slides:
- Controle de vitesse et suivi de voie
- Collisions et securite

Code a presenter:
- `detect-car-ahead`, `move-forward`, `align-with-lane`, `stay-on-road`
- `adjust-speed-smart`, `apply-defensive-braking`
- `check-near-miss`, `check-collision-safe`, `anti-deadlock-resolve`

Message cle:
- priorite a la securite, avec adaptation dynamique de la vitesse.

### 4) Ziko - Analyse et conclusion

Slides:
- Metriques et export CSV
- Parametres et scenarios
- Resultats et conclusion

Code a presenter:
- `initialize-plots`, `update-all-plots`
- `export-stats-to-csv`
- reporters de performance et securite

Message cle:
- interpretation des resultats et compromis entre fluidite et risque.

---

## Questions probables et reponses courtes

- Pourquoi une approche multi-agents?
   Parce que chaque voiture a son propre etat et ses propres decisions, ce qui rend la simulation plus realiste.

- Pourquoi exporter en CSV?
   Pour comparer les scenarios et analyser les resultats hors de NetLogo (Excel, Python, etc.).

- Quelle metrique est la plus importante?
   Selon l'objectif: securite (collision-rate) ou performance (mean-speed, mean-wait-time).

- Pourquoi `committed-to-stop`?
   Pour eviter les oscillations entre arret et reprise pres des feux.

---

## Conclusion

Cette simulation montre comment une architecture multi-agents peut reproduire des comportements de trafic realistes.
Le modele combine:

- conception de l'environnement,
- controle intelligent des feux,
- prise de decision locale des vehicules,
- mecanismes de securite,
- outils d'analyse de performance.

Les resultats permettent d'etudier l'effet des parametres (nombre de voitures, vitesse maximale, distance de securite) sur la fluidite et la securite.
# Simulation de Trafic Multi-Agents (NetLogo)

## 1. Objectif du modele
Ce projet simule un trafic urbain autour d'une intersection centrale, avec feux de circulation, gestion des collisions, comportement intelligent des voitures, et export de statistiques.

Le modele permet d'observer l'impact des parametres (nombre de voitures, vitesse maximale, distance de securite) sur:
- la fluidite du trafic,
- les temps d'attente,
- les collisions,
- les blocages en intersection.

---

## 2. Structure du code
Le code est organise en blocs logiques:

- **Definitions des agents** (`breed`, `...-own`, `globals`)
- **Initialisation** (`setup`, `setup-roads`, `setup-lights`, `setup-cars`)
- **Boucle principale** (`go`)
- **Prise de decision des voitures**
- **Gestion des feux**
- **Securite / collisions / anti-deadlock**
- **Reporters (indicateurs)**
- **Export CSV**
- **Mise a jour des graphiques**

---

## 3. Agents et variables

### 3.1 Voitures (`cars`)
Chaque voiture possede notamment:
- `speed`: vitesse courante
- `max-speed`: vitesse maximale autorisee pour cette voiture
- `safety-distance`: distance de securite
- `car-ahead`: voiture detectee devant
- `wait-time`: compteur de temps d'arret
- `crashed`: etat collision (true/false)
- `trip-start-tick`, `trip-distance`: mesures de trajet
- `turning`: indique si la voiture tourne
- `stuck-timer`: detecte un blocage prolonge
- `committed-to-stop`: vrai si la voiture est engagee a l'arret au feu
- `braking`: indicateur de freinage
- `near-intersection`: voiture proche d'une intersection
- `reaction-delay`: delai de reaction
- `caution-level`: niveau de prudence

### 3.2 Feux (`lights`)
- `state`: `green`, `yellow`, `red`
- `timer-count`: compteur du feu
- `axis`: axe concerne (`H` horizontal, `V` vertical)
- `intersection-x`, `intersection-y`: intersection associee

### 3.3 Feux d'accident (`fires`)
- `fire-age`: age visuel de l'effet d'accident

---

## 4. Fonctionnement principal

### `setup`
Initialise toute la simulation:
1. Reinitialise le monde.
2. Defini les couleurs et durees de feux.
3. Reinitialise le fichier CSV (`trafic_stats.csv`).
4. Construit routes, feux, voitures.
5. Initialise les plots.

### `go`
Boucle executee en continu:
1. Met a jour les feux.
2. Gere les voitures crashees (respawn apres delai).
3. Fait agir les voitures actives:
   - detection vehicule devant,
   - decision intelligente,
   - deplacement,
   - alignement voie,
   - controle collisions.
4. Applique anti-deadlock sur voitures bloquees.
5. Met a jour incendies et vehicules prioritaires.
6. Exporte les stats CSV.
7. Met a jour les plots.
8. Avance le temps (`tick`).

---

## 5. Intelligence des voitures
Le coeur de conduite est dans `make-intelligent-decision`:

- Respect des feux (`should-stop-for-light?`, `can-go-on-green?`)
- Freinage defensif (`apply-defensive-braking`)
- Gestion de la voiture devant
- Ralentissement aux intersections
- Reduction des risques de conflit transversal (`cross-traffic-danger?`)
- Adaptation dynamique de la vitesse (`adjust-speed-smart`)

Des mecanismes supplementaires limitent les blocages:
- relance douce des voitures presque immobiles si la voie est libre,
- anti-deadlock local (`anti-deadlock-resolve`),
- limitation des distances effectives pour eviter les embouteillages permanents.

---

## 6. Interface NetLogo

## 6.1 Boutons
Creer 2 boutons:
- `setup`
- `go` (cocher **Forever**)

## 6.2 Sliders
Creer ces sliders (noms exacts):
1. `num-cars`
2. `max-speed-limit`
3. `min-safety-distance`

Conseil de plages:
- `num-cars`: 10 a 120
- `max-speed-limit`: 0.5 a 12
- `min-safety-distance`: 0.5 a 5

## 6.3 Monitors
Creer ces monitors (reporters exacts):
1. `mean-speed`
2. `collision-rate`
3. `mean-wait-time`
4. `num-stopped-cars`
5. `num-crashes`
6. `mean-trip-time`
7. `cars-stuck-in-intersection`

## 6.4 Plots
Le code met a jour automatiquement 3 plots. Les noms doivent correspondre exactement:

### Plot 1: `Traffic Flow`
Pense a creer ces pens:
- `Mean Speed` -> trace `mean-speed`
- `Stopped Cars` -> trace `num-stopped-cars`

### Plot 2: `Safety`
Pense a creer ces pens:
- `Crashes` -> trace `num-crashes`
- `Collision Rate (%)` -> trace `collision-rate`

### Plot 3: `Travel Quality`
Pense a creer ces pens:
- `Mean Wait` -> trace `mean-wait-time`
- `Mean Trip Time` -> trace `mean-trip-time`
- `Stuck In Intersection` -> trace `cars-stuck-in-intersection`

Remarque: les procedures `initialize-plots` et `update-all-plots` utilisent `carefully`, donc le modele continue meme si un plot/pen manque, mais pour des graphes complets il faut les creer correctement.

---

## 7. Export des donnees
Chaque tick, `export-stats-to-csv` ajoute une ligne dans `trafic_stats.csv`:
- tick
- num_cars
- max_speed_limit
- min_safety_distance
- mean_speed
- num_stopped
- num_crashes

Cela permet d'analyser les resultats hors NetLogo (Excel, Python, etc.).

---

## 8. Procedure de test recommandee
1. Cliquer `setup`.
2. Lancer `go` (Forever).
3. Tester plusieurs valeurs de `min-safety-distance` (ex: 1.5, 2.5, 3.5).
4. Observer simultanement:
   - `num-crashes`,
   - `collision-rate`,
   - `cars-stuck-in-intersection`.
5. Comparer aussi les courbes `Traffic Flow` et `Travel Quality`.

---

## 9. Notes pratiques
- Si les voitures semblent trop prudentes, reduire `min-safety-distance` ou augmenter legerement `max-speed-limit`.
- Si les collisions montent vite, augmenter `min-safety-distance` progressivement.
- Un compromis realiste est generalement obtenu avec:
  - `num-cars` moyen,
  - `max-speed-limit` modere,
  - `min-safety-distance` entre 2 et 3.
