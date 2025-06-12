## Contexte
La qualité de l’air est un enjeu de santé publique majeur, affectant directement les populations, en particulier en milieu urbain. Ce tableau de bord Power BI vise à analyser les niveaux de pollution atmosphérique dans différents pays du monde, à partir de données environnementales.

## Objectif
Ce rapport permet d'avoir :
- Une visualisation claire et interactive de la pollution de l'air
- Une évaluation de la conformité aux seuils OMS
- Une base solide pour des actions ciblées en matière de prévention sanitaire et de politique environnementale.

#### [Télécharger les données](https://github.com/Hines98/Suivi_des_polluants_air/blob/main/openaq.csv)

## Quelques notions
Le tableau de bord se concentre sur les 6 polluants principaux reconnus par l’OMS et la directive européenne 2008/50/CE :
- NO₂ (Dioxyde d’azote)
- O₃ (Ozone)
- CO (Monoxyde de carbone)
- PM10 (Particules de diamètre ≤ 10 µm)
- PM2.5 (Particules fines ≤ 2.5 µm)
- SO₂ (Dioxyde de soufre)
  
| **Polluant** | **Seuil OMS (moyenne annuelle)** | **Pourquoi le suivre ?**                                       |
| ------------ | -------------------------------- | -------------------------------------------------------------- |
| **NO₂**      | 10 µg/m³                         | Irritations respiratoires, effet sur les asthmatiques          |
| **PM10**     | 15 µg/m³                         | Affections respiratoires et cardiovasculaires                  |
| **PM2.5**    | 5 µg/m³                          | Particules fines pouvant pénétrer les alvéoles pulmonaires     |
| **O₃**       | 60 µg/m³ (moyenne 8h)            | Polluant secondaire, effet oxydant sur les voies respiratoires |
| **CO**       | 4 mg/m³ (moyenne 24h)            | Risques cardiovasculaires, effets sur le système nerveux       |
| **SO₂**      | 40 µg/m³ (moyenne 24h)           | Irritations, problèmes respiratoires                           |


Les polluants comme le PM1 et l’humidité relative (Relative Humidity) ont été retirées, car elles ne font pas partie des polluants de référence suivis par l’Organisation Mondiale de la Santé (OMS) pour l’évaluation de la qualité de l’air.

## Démarche méthodologique

### 1. Importation des données
Les ont été téléchargées sur le site [https://public.opendatasoft.com/explore/dataset/openaq/table/?flg=fr-fr&disjunctive.measurements_parameter&disjunctive.location&disjunctive.city&sort=measurements_lastupdated]

### 2. Traitement
Dans power query :
- les valeurs négatives (ex. : -1, -1000) ont été identifiées comme des erreurs de capteurs ou des valeurs manquantes et ont été exclues du calcul des moyennes et des visualisations.
- certaines valeurs positives ont également été considérées comme aberrantes selon le polluant
  
| **Polluant**   | **À considérer comme aberrant si…** | **Justification du seuil aberrant**                                             |
| -------------- | ----------------------------------- | ------------------------------------------------------------------------------- |
| **NO₂**        | > 200 µg/m³                         | Les pics urbains atteignent rarement 150-180 µg/m³ même dans les grandes villes |
| **PM₁₀**       | > 150 µg/m³                         | La directive européenne fixe un plafond journalier à 50 µg/m³                   |               
| **PM₂.₅**      | > 100 µg/m³                         | Le pic OMS est 25 µg/m³ sur 24h                                                 |
| **O₃**         | > 300 µg/m³                         | Le seuil d’alerte européen est 240 µg/m³ sur 1h                                 |
| **CO**         | > 10000 µg/m³ (soit 10 mg/m³)       | L’OMS fixe 4 mg/m³ sur 8h                                                       |
| **SO₂**        | > 500 µg/m³                         | Seuil journalier OMS : 40 µg/m³                                                 |

Dans la réalité, les valeurs de concentration des polluants atmosphériques ne peuvent pas être négatives. Les concentrations sont des mesures physiques, souvent exprimées en microgrammes par mètre cube (µg/m³) ou milligrammes par mètre cube (mg/m³), et elles représentent une quantité présente dans l’air. Or, on ne peut pas avoir une "quantité négative" d’un gaz ou de particules.

## Principaux indicateurs du tableau de bord
- Moyenne et valeur maximale : permettent d’évaluer la concentration globale et les pics de pollution par ville ou pays
- Nombre de jours dépassant le seuil de l'OMS : indique le nombre de jour avec des concentrations dépassant le seuil de l'OMS
- Nombre de jours mesurés : reflète la densité temporelle de la couverture des capteurs.
- Carte géographique : localisation des villes suivies avec leurs niveaux de pollution, facilitant l'identification des zones critiques.
- Série temporelle (année-mois) : permet d'observer l’évolution mensuelle des concentrations.
- Tableau ville/polluant : synthèse des valeurs par polluant pour chaque ville.

## Vue spécifique du Dioxyde d'azote (NO2) en 2024 en France 
![image](https://github.com/user-attachments/assets/c3c55285-4d3e-4b1c-9e07-7bdc95556a85)

| Indicateur                      | Valeur observée     | Interprétation                                                                                                     |
| ------------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **Moyenne générale (NO₂)**      | **24,30 µg/m³**     | Juste en dessous du seuil OMS de 25 µg/m³, donc préoccupant à l’échelle nationale                                  |
| **Valeur maximale enregistrée** | **94,00 µg/m³**     | Bien au-dessus du seuil OMS journalier                                                                             |
| **Nbre de jours > seuil OMS**   | **10 jours sur 23** | Soit environ **43 % des jours mesurés** dépassent le seuil recommandé par l’OMS, ce qui est inquiétant             |
| **Nbre total de jours mesurés** | **23 jours**        | Assez court pour une période d’analyse mensuelle ou ponctuelle                                                     |

La carte montre une forte concentration de capteurs dans le sud et l’est de la France (notamment Rhône-Alpes, PACA, Occitanie, Grand Est).

Cela peut indiquer une surveillance renforcée dans les zones urbaines ou industrielles.

Certaines zones (centre et ouest de la France) semblent moins couvertes, ce qui peut biaiser la moyenne nationale. En effet, les stations sont souvent situées dans les villes ou zones industrielles, où la pollution est naturellement plus élevée (trafic, chauffage, activités industrielles). Et si ces zones sont surreprésentées, la moyenne nationale sera surestimée.
