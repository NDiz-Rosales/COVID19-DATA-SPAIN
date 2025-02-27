COVID-19 databases by health area and day
in Spain[^1]
================
2024-04-21

**Authors:**
-  <p align="justify"> Name: Naomi Diz-Rosales<sup>1</sup>, María José Lombardía<sup>2</sup>, Domingo Morales<sup>3</sup> </p>
-  <p align="justify"> Affiliation: <sup>1</sup>naomi.diz.rosales@udc.es, CITIC, Universidade da Coruña, Spain. <sup>2</sup>maria.jose.lombardia@udc.es, CITIC, Universidade da Coruña, Spain. <sup>3</sup>d.morales@umh.es, IUICIO, Universidad Miguel Hernández de Elche, Spain. </p>

  ------------------------------------------------------------------------

# Structure

- [What to find in this repository?](#what-to-find-in-this-repository?)
- [Hardware and Software](#hardware-and-software)
- [Section 1. Spatial and temporal domains](#section-1-spatial-and-temporal-domains)
  - [1.1 Spatial domains](#11-spatial-domains)
  - [1.2 Temporal domains](#12-temporal-domains)
- [Section 2. Target variable: ICU](#section-2-target-variable-icu)
- [Section 3. Auxiliary Information](#section-3-auxiliary-information)
  - [3.1 Sociodemographic indicators](#31-sociodemographic-indicators)
    - [3.1.1 Population](#311-population)
    - [3.1.2 Population density](#312-population-density)
    - [3.1.3 Ratio of retirement home places](#313-ratio-of-retirement-home-places)
    - [3.1.4 Assessment of socio-demographic indicators](#314-assessment-of-sociodemographic-indicators)
  - [3.2 Epidemiological indicators](#32-epidemiological-indicators)
    - [3.2.1 Disease spread](#321-disease-spread)
    - [3.2.2 Disease severity](#322-disease-severity)
    - [3.2.3 Care pressure](#323-care-pressure)
- [Section 4. Definitive Dataset](#section-4-definitive-dataset)


## What to find in this repository?

> [!NOTE]
> This repository includes highly detailed datasets on transmission, severity and healthcare pressure variables caused by COVID-19. Specifically, data have been collected and processed for the health areas of Castilla y León with observations between 02/11/2020 and 06/03/2022. If any user has any doubts or curiosity, please contact the first author. </p>

## Hardware and Software

> [!TIP]
> <p align="justify"> It is recommended to download the repository and to indicate in path the directory where the folders are stored, to facilitate the execution. </p>

``` r
path<-"D:/COVID_DATA"
setwd(path)
```

<p align="justify">There are no special hardware requirements for data handling. For
reference, the construction of the dataset was carried out on a computer
with a 7-core processor, with a RAM of 16.0 GB and a maximum processor
speed of 1.50 GHz. As for the software, the operating system was Windows
10 and both the creation and reading of the datasets was done with the R
programming language (version 4.2.2 and higher), highlighting the
following packages for reading and handling the data.</p>


``` r
# Reading csv files in R, especially recommended for large data volumes.
if(!require(data.table)){
  install.packages("data.table",dependencies=TRUE)
  library(data.table)
}

# Dynamic reporting.
if(!require(knitr)){
  install.packages("knitr",dependencies=TRUE)
  library(knitr)
}
```


# Section 1. Spatial and temporal domains

<p align="justify">The main objective of this research is to <strong>estimate and predict the
count and proportions of intensive care unit (ICU) beds occupied by
COVID-19 patients</strong> by health area and day in Castilla y León.</p>

## 1.1 Spatial domains

> [!NOTE]
> <p align="justify">The data files and images referenced in this section are located in the <strong>HEALTH AREAS</strong> folder, and correspond to:</p>
> 
> - <p align="justify"><strong>municipality_health_areas.csv</strong>: file corresponding to the equivalences between municipalities and health areas or H.A.</p>
> - <p align="justify"><strong>mapSpainCL.png</strong>: image represented in <a href="#figure1">Figure 1</a>.</p>
> - <p align="justify"><strong>mapCastillaLeon.png</strong>: image represented in <a href="#figure2">Figure 2</a>.</p>

<p align="justify">The domains or spatial areas of interest correspond to the Health Areas (H.A.) of Castilla y León, an Autonomous Community in Spain, located, as can be seen in <a href="#figure1">Figure 1</a>, in the north-western fringe of the Peninsula.</p>

<p align="center">
  <a id="figure1"></a>
  <img alt="Geographical location of Castilla y León" src="./README_COVID_files/mapSpainCL.png">
  <br>
  <em>Figure 1: Geographical location of Castilla y León.</em>
</p>

<p align="justify"> The spatial domains of interest correspond to its 11 H.A., as shown in <a href="#figure2">Figure 2</a>.</p>

<p align="center">
  <a id="figure2"></a>
  <img alt="figure2" src="./README_COVID_files/mapCastillaLeon.png">
  <br>
    <em>Figure 2: H.A. of Castilla y León.</em>
</p>


<p align="justify">To define the correspondence between the H.A. and the municipalities they cover, we consulted:</p>

- <p align="justify"> <a href="https://www.sanidad.gob.es/estadEstudios/estadisticas/docs/siap/Ordenacion_sanitaria_2021.pdf">Territorial Health Planning in the Autonomous Communities
  2021</a>.</p>
- <p align="justify"> <a href="https://www.saludcastillayleon.es/institucion/es/organizacion/ordenacion-sistema-sanitario/guia-ordenacion-sanitaria-castilla-leon">Castilla y León Health Planning
  Guide</a>.</p>

<p align="justify"> As a result, we obtain the file <strong>municipality_health_areas.csv</strong> with
the H.A-municipalities correspondence, attached in the <strong>HEALTH AREAS</strong>
folder and whose header is illustrated below. Specifically, there are
2248 rows and 7 columns. This corresponds to the 2248 municipalities in
Castilla y León and the following variables:</p>

- <p align="justify"><strong>ID_HA</strong>: Official H.A. identifier code.</p>
- <p align="justify"><strong>HA</strong>: Name of the H.A.</p>
- <p align="justify"><strong>Municipality</strong>: Name of the municipality.</p>
- <p align="justify"><strong>ID_INE</strong>: Official code of the municipality in the INE (Instituto
  Nacional de Estadística).</p>
- <p align="justify"><strong>ID_Municipality</strong>: Municipality identifier in the file.</p>
- <p align="justify"><strong>Province</strong>: Province in which the municipality is located.</p>
- <p align="justify"><strong>ID_province</strong>: Official code of the province that appears in the
  INE.</p>

``` r
# Load the data.table library
library(data.table)

# Read csv file
data <- as.data.frame(fread("./HEALTH AREAS/municipality_health_areas.csv"))

# Display the file header
head(data)
```

    ##   ID_HA          HA                Municipality ID_INE ID_Municipality Province
    ## 1  1702 H.A. BURGOS        SALINILLAS DE BUREBA   9334             334   BURGOS
    ## 2  1702 H.A. BURGOS       SAN ADRIAN DE JUARROS   9335             335   BURGOS
    ## 3  1702 H.A. BURGOS          SAN JUAN DEL MONTE   9337             337   BURGOS
    ## 4  1702 H.A. BURGOS               SANTA CECILIA   9343             343   BURGOS
    ## 5  1702 H.A. BURGOS    SANTA CRUZ DE LA SALCEDA   9345             345   BURGOS
    ## 6  1702 H.A. BURGOS SANTA CRUZ DEL VALLE URBION   9346             346   BURGOS
    ##   ID_Province
    ## 1           9
    ## 2           9
    ## 3           9
    ## 4           9
    ## 5           9
    ## 6           9

## 1.2 Temporal domains

<p align="justify"> The temporal domains of interest correspond to <strong>488 consecutive days</strong>
for each H.A., collected from <strong>2 November 2020 to 6 March 2022</strong>. Of
this total, it should be specified that the 181 days between 2 November
2020 and 2 May 2021 are used to assess the adjustment capacity of the
model. On the other hand, the remaining 307 days, from 3 May 2021 to 6
March 2022, are used to assess the quality of the forward forecast with
data that have not been used in the model fit.</p>

<p align="justify"> According to the <a href="https://www.isciii.es/QueHacemos/Servicios/VigilanciaSaludPublicaRENAVE/EnfermedadesTransmisibles/Documents/INFORMES/Informes%20COVID-19/INFORMES%20COVID-19%202022/Informe%20n%C2%BA%20154%20Situaci%C3%B3n%20actual%20de%20COVID-19%20en%20Espa%C3%B1a%20a%2011%20de%20noviembre%20de%202022.pdf">Centro Nacional de Epidemiología (CNE)</a>, a total of five epidemic periods (EP) occurred during this time range.</p>

- <p align="justify"><strong>Second epidemic period (2nd EP)</strong>: Between 22 June 2020 and 6
  December 2020, at which point the 14-day CI (Cumulative Incidence)
  tipping point of COVID-19 cases occurs, leading to the third period.</p>
- <p align="justify"><strong>Third epidemic period (3rd EP)</strong>: Between 7 December 2020 and 14
  March 2021, when the 14-day CI tipping point of COVID-19 cases occurs,
  leading to the fourth period.</p>
- <p align="justify"><strong>Fourth epidemic period (4th EP)</strong>: Between 15 March 2021 and 19 June
  2021, when the 14-day CI turning point of COVID-19 cases occurs,
  leading to the fifth period.</p>
- <p align="justify"><strong>Fifth epidemic period (5th EP)</strong>: Between 20 June 2021 and 13
  October 2021, when the 14-day CI tipping point for COVID-19 cases
  occurs, leading to the sixth period.</p>
- <p align="justify"><strong>Sixth epidemic period (6th EP)</strong>: Between 14 October 2021 and 27
  March 2022, the last day before the implementation of a new
  epidemiological surveillance strategy.</p>

<p align="justify"> In the same vein, the 488 days under study allow to take into
consideration the impact of three main SARS-CoV-2 variants, which were
considered by the European Centre for Disease Prevention and Control as
Variants of Concern (VOC) for their significant epidemiological impact:
Alpha (B.1.1.7), Delta (B.1.617.2) and Omicron (B.1.1.529).</p>

# Section 2. Target variable: ICU

> [!NOTE]
> <p align="justify"> The target variable of the model is the count of people hospitalised with COVID-19 in ICU.</p>
>
> <p align="justify">The data file referenced in this section is located in the <strong>ICU</strong> folder, and corresponds to:</p>
 >  
 > - <p align="justify"><strong>ICU.csv</strong>: File containing the number of people with suspected COVID-19 admitted to ICU by H.A. and day.</p>



<p align="justify"> For its construction, we carry out the following steps:</p>

- <p align="justify">We consulted the Open Data portal of the Junta de Castilla y León. We
  downloaded the database <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/situacion-de-hospitalizados-por-coronavirus-en-castilla-y-leon/information/?flg=es-es&sort=fecha">“Situation of hospitalised patients with
  Coronavirus in Castilla y
  León”</a>. In this database, we have records at <strong>hospital level</strong>, indicating,
  for a total of 15 hospitals in the Autonomous Community, the number of
  patients in ICU due to COVID-19. These patients, in accordance with
  the guidelines of the Ministry of Health, are counted both for having
  a positive diagnostic test or for a high clinical suspicion of the
  disease. In addition, they specify that those hospitalised in ICU
  include: ICU, REA (resuscitation units), URPA (post-anaesthesia
  resuscitation units) and other units with adequate staffing for
  critical patients.</p>

- <p align="justify">We carried out a search for the H.A. to which each hospital belongs,
  using the <a href="https://idecyl.jcyl.es/vcig/?service=https://idecyl.jcyl.es/geoserver/hh/wms&layer=salud_cyl_zonas_basicas_v&type=wms&style=hh%3Asalud_cyl_zonas_basicas_oscuro&bbox=160000,4440000,605000,4790000&srs=EPSG:25830">IdeCyL
  viewer</a>,
  the <a href="https://buscadorcentros.saludcastillayleon.es/BUCE/">Health centre
  finder</a> and the <a href="https://www.saludcastillayleon.es/institucion/es/organizacion/ordenacion-sistema-sanitario/guia-ordenacion-sanitaria-castilla-leon">Castilla y León Health Planning
  Guide</a>.
  The correspondence is as follows:</p>

  - <p align="justify">H.A. El Bierzo: Hospital El Bierzo.</p>
  - <p align="justify">H.A. León: Complejo Asistencial Universitario de León.</p>
  - <p align="justify">H.A. Palencia: Complejo Asistencial Universitario de Palencia.</p>
  - <p align="justify">H.A. Burgos: Hospital Santos Reyes; Complejo Asistencial
    Universitario de Burgos; Hospital Santiago Apóstol.</p>
  - <p align="justify">H.A. Soria: Complejo Asistencial de Soria.</p>
  - <p align="justify">H.A. Segovia: Complejo Asistencial de Segovia.</p>
  - <p align="justify">H.A. Ávila: Complejo Asistencial de Ávila.</p>
  - <p align="justify">H.A. Salamanca: Complejo Asistencial Universitario de Salamanca.</p>
  - <p align="justify">H.A. Valladolid Este: Hospital Cínico Universitario de Valladolid;
    Edificio Rondilla.</p>
  - <p align="justify">H.A. Valladolid Oeste: Hospital Universitario Río Hortega; Hospital
    Medina del Campo.</p>
  - <p align="justify">H.A. Zamora: Complejo Asistencial de Zamora.</p>
  
  <p align="justify">In addition, it should be noted that hospitals Santiago Apóstol, Santos Reyes and Medina del Campo do not have ICUs.</p>

- <p align="justify"> We checked the available time range to see if there are observations
  for all the days between <strong>02/11/2020 and 06/03/2022</strong>. Thus, we find
  that data are missing for four dates, corresponding to the following
  days: Saturday 3 July 2021, Sunday 4 July 2021, Saturday 10 July 2021,
  and Sunday 11 July 2021. To perform the imputation of these four
  missing occupancy values, we rely on the data available for these
  dates in:
  
  - <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/indicadores-de-riesgo-covid-19-por-provincias/table/?disjunctive.indicador&sort=fecha">COVID-19 risk indicators by provinces up to
    24-03-22</a>.
  - <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/indicadores-de-riesgo-covid-19-por-municipios/table/?disjunctive.provincia&sort=fecha">COVID-19 risk indicators by municipalities until
    24-03-2022</a>.
    </p>

- <p align="justify"> Once the allocation of the H.A. of each hospital has been carried out
  and with the target time range complete, the aggregation of those
  hospitalised in ICU is carried out, by H.A. and day, saved in the
  <strong>ICU</strong> csv file, the header of which can be seen below.</p>


``` r
# Load the data.table library
library(data.table)

# Read csv file
data <- as.data.frame(fread("./ICU/ICU.csv"))

# Display the file header
head(data)
```

    ##         date         HA ID_HA province ICU
    ## 1 02/11/2020 H.A. ÁVILA  1701    Ávila   9
    ## 2 03/11/2020 H.A. ÁVILA  1701    Ávila   9
    ## 3 04/11/2020 H.A. ÁVILA  1701    Ávila   9
    ## 4 05/11/2020 H.A. ÁVILA  1701    Ávila   7
    ## 5 06/11/2020 H.A. ÁVILA  1701    Ávila   9
    ## 6 07/11/2020 H.A. ÁVILA  1701    Ávila   9

# Section 3. Auxiliary Information

<p align="justify"> Since the target variable is occupancy in ICU due to COVID-19, we looked
at factors that could influence it, classifying them into two large
blocks: <strong>sociodemographic factors</strong> and <strong>epidemiological factors</strong>.</p>

<p align="justify"><a href="#figure3">Figure 3</a> shows schematically.</p>

<p align="center">
  <a id="figure2"></a>
  <img alt="figure3" src="./README_COVID_files/auxiliar_information.png">
  <br>
    <em>Figure 3: Auxiliar information diagram.</em>
</p>


## 3.1 Sociodemographic indicators


<p align="justify"> We proceeded to characterise the H.A. on the basis of sociodemographic
factors such as population, population density and the ratio of nursing
home places per 100 inhabitants over 65 years of age.</p>

> [!NOTE]
> <p align="justify">The data files referenced in this section are located in the <strong>SOCIODEMOGRAPHIC INDICATORS</strong> folder and correspond to:</p>
> 
> <p align="justify"><strong>ORIGINAL SOURCES</strong>: Folder containing the original data files downloaded from each source web page referenced. Specifically:</p>
> 
> - <p align="justify"><strong>population_municipalities.csv</strong>: Data file of the Instituto Nacional de Estadística (INE) corresponding to 2021 Population and Housing Census, containing information on the <a href="https://ine.es/jaxiT3/Tabla.htm?t=33571&L=0">2021 Population by sex, municipalities, nationality (Spanish/foreign) and age (large groups)</a>.</p>
> - <p align="justify"><strong>surface_municipalities.csv</strong>: Data file of the Instituto Geográfico Nacional (IGN), specifically the <a href="https://www.ign.es/web/rcc-nomenclator-nacional">“Geographic Nomenclator of Municipalities and Population Entities”</a> showing the geographical surface of each territory.</p>
> - <p align="justify"><strong>retirement home_places.csv</strong>: Data files from the open data portal <a href="https://envejecimientoenred.csic.es/datos-abiertos/residencias/">“Envejecimiento en Red”</a>, detailing, for each province, the existing old people’s homes, as well as their number of places and municipal location.</p>
>   
>   - retirement home_places_avila.csv
>   - retirement home_places_burgos.csv
>   - retirement home_places_leon.csv
>   - retirement home_places_palencia.csv
>   - retirement home_places_salamanca.csv
>   - retirement home_places_segovia.csv
>   - retirement home_places_soria.csv
>   - retirement home_places_valladolid.csv
>   - retirement home_places_zamora.csv
>    
> <p align="justify"><strong>sociodemographic_indicators.csv</strong>: csv file constructed with all the sociodemographic indicators contemplated by H.A. (population, population density and rate of number of retirement home home places per 100 people over 65 years of age).</p>


### 3.1.1 Population

<p align="justify"> In order to deepen the analysis, we aimed to obtain the total population
for each H.A., but also the proportion of women, men, people under 16,
between 16 and 64 and over 65 years.</p>

- <p align="justify">Firstly, we extracted the information from the INE’s 2021 Population
  and Housing Census, and specifically, from the results of <a href="https://ine.es/jaxiT3/Tabla.htm?t=33571&L=0">“2021
  Population by sex, municipalities, nationality (Spanish/foreign) and
  age (large groups)”</a>. We chose 2021, since our range of interest is between 2020 and 2022, and
  the population figures are known in January of the year after the
  current one. It is therefore the most consistent year, of those then
  available, to take into consideration the population effect of
  COVID-19 in 2020.</p>
- <p align="justify">We then define the following variables:</p>

   - <strong>POP_2021</strong>: Total number of inhabitants per H.A.
   - <strong>sex0</strong>: Base 1 proportion of the number of men.
   - <strong>sex1</strong>: Base 1 proportion of the number of women.
   - <strong>cit0</strong>: Base 1 proportion of the number of persons with Spanish
    nationality or dual nationality (one of them Spanish).
   - <strong>cit1</strong>: Base 1 proportion of the number of persons without Spanish
    nationality.
   - <strong>age0</strong>: Base 1 proportion of persons under 16 years of age.
   - <strong>age1</strong>: Base 1 proportion of persons aged between 16 and 64 years
    of age.
   - <strong>age2</strong>: Base 1 proportion of persons aged over 64 years.
     
- <p align="justify">Finally, to obtain the values of the variables, we aggregate, for each
  H.A., the information collected by the INE at the municipality level.</p>

### 3.1.2 Population density

<p align="justify"> The population density for each H.A. is defined as the number of people
per km<sup>2</sup> of surface area. To obtain it, we carry out the
following steps.</p>

- <p align="justify">We consulted the Instituto Geográfico Nacional (IGN), specifically the
  <a href="https://www.ign.es/web/rcc-nomenclator-nacional">“Geographic Nomenclator of Municipalities and Population
  Entities”</a>, which,
  based on the Register of Local Entities, the INE and the IGE’s own
  cartographic databases, provides, among other characteristics, the
  surface area of each municipality in km<sup>2</sup>.</p>
- <p align="justify">Once this information was downloaded, the next step was to add the
  surface area by H.A., and finally to calculate the population/surface
  area ratio to obtain the population density.</p>

### 3.1.3 Ratio of retirement home places

<p align="justify"> One of the keys to the COVID-19 pandemic was the severity with which it
affected older people in general, and in particular retirement home care
homes. It is estimated that there should be a minimum of 5 retirement
home care places for every 100 inhabitants over the age of 65. To
calculate this ratio, we need to find the number of places.</p>

- <p align="justify">First, we took the data from the Open Data portal <a href="https://envejecimientoenred.csic.es/datos-abiertos/residencias/">“Envejecimiento en
  Red”</a>, where the information was located at municipality level.</p>
- <p align="justify">Based on the Municipality-H.A. correlation, we aggregated the
  information and calculated the ratio of retirement home places by H.A.</p>


### 3.1.4 Assessment of sociodemographic indicators

<p align="justify"> All variables are combined in the sociodemographic indicators file for
each H.A.</p>

``` r
# Load the data.table library and the knitr library
library(data.table)
library(knitr)

# Read csv file
data <- as.data.frame(fread("./AUXILIARY INFORMATION/SOCIODEMOGRAPHIC INDICATORS/sociodemographic_indicators.csv"))
                    
data$`H.A.` <- format(data$`H.A.`)
# Display the data file
kable(data)
```

| H.A.                  | POP_2021 |      sex0 |      sex1 |      cit0 |      cit1 |      age0 |      age1 |      age2 | population_density | ratio_retirement_places |
|:----------------------|---------:|----------:|----------:|----------:|----------:|----------:|----------:|----------:|-------------------:|------------------------:|
| H.A. ÁVILA            |   159684 | 0.5028181 | 0.4971819 | 0.9328925 | 0.0671075 | 0.1270134 | 0.6131798 | 0.2598069 |          19.441587 |                8.715983 |
| H.A. BURGOS           |   355031 | 0.4999028 | 0.5000972 | 0.9192916 | 0.0807084 | 0.1354896 | 0.6231709 | 0.2413395 |          25.641513 |                7.731989 |
| H.A. LEÓN             |   324663 | 0.4861256 | 0.5138744 | 0.9520056 | 0.0479944 | 0.1140348 | 0.6076005 | 0.2783647 |          26.122730 |                6.554910 |
| H.A. PALENCIA         |   159720 | 0.4959366 | 0.5040634 | 0.9542575 | 0.0457425 | 0.1188017 | 0.6198660 | 0.2613323 |          19.157662 |               10.263536 |
| H.A. EL BIERZO        |   128167 | 0.4870208 | 0.5129792 | 0.9618154 | 0.0381846 | 0.1047843 | 0.6260143 | 0.2692014 |          35.877389 |                5.747326 |
| H.A. SALAMANCA        |   328242 | 0.4858336 | 0.5141664 | 0.9531626 | 0.0468374 | 0.1200943 | 0.6118017 | 0.2681040 |          26.448208 |                8.347443 |
| H.A. SEGOVIA          |   151440 | 0.5018357 | 0.4981643 | 0.8818814 | 0.1181186 | 0.1388669 | 0.6375462 | 0.2235869 |          22.645145 |                8.603072 |
| H.A. SORIA            |    89458 | 0.5063829 | 0.4936171 | 0.9018209 | 0.0981791 | 0.1292115 | 0.6172170 | 0.2535715 |           8.587615 |               11.100335 |
| H.A. VALLADOLID ESTE  |   266181 | 0.4889718 | 0.5110282 | 0.9414461 | 0.0585539 | 0.1314927 | 0.6234531 | 0.2450541 |          56.317029 |                8.795168 |
| H.A. VALLADOLID OESTE |   253981 | 0.4852646 | 0.5147354 | 0.9490751 | 0.0509249 | 0.1439984 | 0.6303675 | 0.2256341 |          70.593441 |                9.063465 |
| H.A. ZAMORA           |   168658 | 0.4954108 | 0.5045892 | 0.9619644 | 0.0380356 | 0.0996514 | 0.5895540 | 0.3107946 |          16.065885 |                8.207104 |


<p align="justify"> The H.A. with the highest number of inhabitants in 2021 correspond to
H.A. Burgos and H.A. Salamanca, while the least populated are H.A. Soria
and H.A. El Bierzo. If we look at the distribution of the population,
the proportion of men and women is very similar, with H.A. Ávila, H.A.
Segovia and H.A. Soria being the only health care areas with more men
than women. In terms of age distribution, the H.A. Valladolid Oeste,
H.A. Segovia, H.A. Burgos and H.A. Valladolid Este stand out as those
with the highest proportion of people under 16 years of age, and H.A.
Zamora as having the lowest proportion of people under 16 years of age.
Thus, the opposite behaviour is observed in terms of the proportion of
the population over 65 years of age, clearly dominating over the rest in
H.A. Zamora, followed by H.A. León and H.A. El Bierzo. In terms of
population density, the H.A. Valladolid Oeste and H.A. Valladolid Este
stand out, with H.A. Soria being by far the health H.A. with the lowest
population density. In no case, however, is the population density of
95.26 inhabitants per km<sup>2</sup> higher than the Spanish average.
Finally, with regard to the ratio of retirement home care home places
per 100 people over 65 years of age, in all H.A. the minimum recommended
5% is reached, with H.A. Soria standing out with the highest rate, and
H.A. El Bierzo with the lowest, but always higher than the national
average.</p>


## 3.2 Epidemiological indicators

<p align="justify">We proceed to characterise the H.A. based on epidemiological factors,
following the guidelines indicated in the <a href="https://www.sanidad.gob.es/profesionales/saludPublica/ccayes/alertasActual/nCov/documentos/Nueva_estrategia_vigilancia_y_control.pdf">Epidemic Surveillance
Strategies of the Ministry of
Health</a>. In this way, we create indicator variables for disease transmissibility,
disease severity and care pressure.</p>

> [!NOTE]
> <p align="justify"> The data files referenced in this section are located in the <strong>EPIDEMIOLOGICAL INDICATORS</strong> folder and correspond to:</p>
> 
> - <p align="justify"><strong>ORIGINAL SOURCES</strong>: Folder containing the original data files downloaded from each source web page referenced. Specifically:</p>
>  
>   - <p align="justify"><strong>cumulative_sick_rate_by_health_area.csv</strong>: Data file from the Open Data Portal of the Junta de Castilla y León that collects the <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-enfermos-acumulados-por-areas-de-salud/information/?flg=es-es&disjunctive.zbs_geo&sort=fecha">Sickness rate by basic health area and day</a>.</p>
>   - <p align="justify"><strong>situation_of_hospitalised_coronavirus_patients_in_castilla_leon.csv</strong>: the Open Data Portal of the Junta de Castilla y León that collects the <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/situacion-de-hospitalizados-por-coronavirus-en-castilla-y-leon/information/?flg=es-es&sort=fecha">Situation of hospitalised coronavirus patients in Castilla y León by days and hospitals.</a>.</p>
>   - <p align="justify"><strong>hospital_bed_occupancy.csv</strong>: Data file from the Open Data Portal of the Junta de Castilla y León that collects the <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/ocupacion-de-camas-en-hospitales/table/?flg=es-es&sort=fecha">occupancy of hospital beds per day and hospital.</a></p>
>   - <p align="justify"><strong>covid_mortality_rate_by_basic_health_areas.csv</strong>: Data file from the Open Data Portal of the Junta de Castilla y León that collects the <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-mortalidad-covid-por-zonas-basicas-de-salud/information/?sort=fecha">Covid mortality rate by basic health area and day.</a></p>
>   - <p align="justify"><strong>mortality_rate_by_health_centre.csv</strong>: Data file from the Open Data Portal of the Junta de Castilla y León that collects the <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-mortalidad-covid-por-zonas-basicas-de-salud/information/?sort=fecha">Mortality rate by basic health areas and day.</a></p>
>   - <p align="justify"><strong>people_vaccinated_covid.csv</strong>: Data file from the Open Data Portal of the Junta de Castilla y León that collects the <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/personas-vacunadas-covid/table/?disjunctive.provincia&sort=fecha">Persons vaccinated COVID-19 until 23-09-2022 by province and day.</a></p>
> - <p align="justify"><strong>COVID19_SPREAD_INDICATORS_LAGS.csv</strong>: csv file constructed with all epidemiological indicators on disease spread, specified by H.A. and day.</p>
> - <p align="justify"><strong>COVID19_SEVERITY_INDICATORS_LAGS.csv</strong>: csv file constructed with all epidemiological indicators on the severity of the disease, specified by H.A. and day.</p>
> - <p align="justify"><strong>COVID19_CARE_PRESSURE_INDICATORS_LAGS.csv</strong>: csv file constructed with all epidemiological indicators on COVID-19 care pressure, specified by H.A. and day.</p>


### 3.2.1 Disease spread

<p align="justify">For its construction, we carry out the following steps:</p>

- <p align="justify">We consulted the Open Data Portal of the Junta de Castilla y León. We
  downloaded the database <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-enfermos-acumulados-por-areas-de-salud/information/?flg=es-es&disjunctive.zbs_geo&sort=fecha">Sickness rate by basic health
  areas.</a> In this database we have records at <strong>Basic Health Zone</strong> level. As indicated in its platform it provides information on:</p>
  
  - <p align="justify">Daily incidence of positives with confirmed or suspected COVID-19.</p>
  - <p align="justify">Diagnostic tests for active infection, including both PCR and
    antigen tests. It should be noted that as of <strong>13/01/2022</strong>, Ag
    Autotest tests (Pharmacy Test), which are reported by citizens
    through the application of the Junta de Castilla y León, will also
    be integrated into all the accounting systems of Castilla y León.</p>
    
- <p align="justify">We check that in the target time range between 02/11/2020 and
  06/03/2022 all information is available.</p>
- <p align="justify">We assign each Basic Health Zone with its H.A., with the assignment
  provided by the database itself, and aggregate the variables by H.A.</p>
- <p align="justify">We defined the indicators indicated in the <a href="https://www.sanidad.gob.es/profesionales/saludPublica/ccayes/alertasActual/nCov/documentos/Nueva_estrategia_vigilancia_y_control.pdf">“Epidemic Surveillance
  Strategies of the Ministry of Health”</a>, which include the following:</p>
  
  - <p align="justify"><strong>prop_cases</strong> or proportion of suspected COVID-19 positives.
    Calculated as COVID-19 positives (confirmed or suspected) among the
    total population, per 100,000 inhabitants.</p>
  - <p align="justify"><strong>CI7</strong> or 7-day cumulative incidence, calculated as the ratio of
    7-day COVID-19 positives to total population per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>CI14</strong> or 14-day cumulative incidence, calculated as the ratio of
    14-day COVID-19 positives to total population per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>positive_rate</strong>, calculated as positives in a diagnostic test
    divided by the number of diagnostic tests per 100.</p>
  - <p align="justify"><strong>positive_rate7</strong> or 7-day positive rate, calculated as 7-day
    positive diagnostic test results divided by the number of diagnostic
    tests in 7 days per 100.</p>
  - <p align="justify"><strong>positive_rate14</strong> or 14-day positive rate, calculated as the
    positives in a 14-day diagnostic test divided by the number of
    diagnostic tests in 14 days per 100.</p>
    
- <p align="justify">Finally, we performed the lags for each variable. In this way we take
  into account the delay caused by reporting delays and the inherent
  nature of the disease itself. Specifically, for the <strong>prop_cases</strong> and
  <strong>positive_rate</strong> variables, lags from 1 to 26 days are provided. For
  the variables <strong>CI7</strong> and <strong>positive_rate7</strong> lags from 1 to 20 days.
  For the variables <strong>CI14</strong> and <strong>positive_rate14</strong> lags from 1 to 14
  days.</p>

<p align="justify">All variables and their lags, together with the date, H.A., population
size and province identifier are stored in the csv file
<strong>COVID_SPREAD_INDICATORS</strong>, stored in the folder <strong>EPIDEMIOLOGICAL
INDICATORS</strong>, the header of which is shown below.</p>


``` r
# Load the data.table library
library(data.table)

# Read csv file
data <- as.data.frame(fread("./AUXILIARY INFORMATION/EPIDEMIOLOGICAL INDICATORS/COVID19_SPREAD_INDICATORS_LAGS.csv"))

# Display the file header and some variables
head(data[,1:12])
```

    ##         date         HA ID_HA province POP_2021 prop_cases      CI7     CI14
    ## 1 02/11/2020 H.A. ÁVILA  1701    Ávila   159684   15.02968 353.1976 645.0239
    ## 2 03/11/2020 H.A. ÁVILA  1701    Ávila   159684   45.71529 344.4302 655.0437
    ## 3 04/11/2020 H.A. ÁVILA  1701    Ávila   159684   58.24002 374.4896 675.7095
    ## 4 05/11/2020 H.A. ÁVILA  1701    Ávila   159684   63.87616 320.0070 691.9917
    ## 5 06/11/2020 H.A. ÁVILA  1701    Ávila   159684   67.00734 329.4006 704.5164
    ## 6 07/11/2020 H.A. ÁVILA  1701    Ávila   159684   34.44302 316.8758 694.4966
    ##   positive_rate positive_rate7 positive_rate14 prop_cases_LAG1
    ## 1      14.11765       29.22280        27.55484        32.56431
    ## 2      23.93443       28.54177        27.65001        15.02968
    ## 3      30.69307       29.72167        27.99689        45.71529
    ## 4      28.33333       27.71150        28.25364        58.24002
    ## 5      31.47059       28.21888        28.97991        63.87616
    ## 6      28.49741       27.94036        28.79024        67.00734

### 3.2.2 Disease severity

<p align="justify">We consulted the Open Data Portal of the Junta de Castilla y León. We
downloaded four databases. <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/situacion-de-hospitalizados-por-coronavirus-en-castilla-y-leon/information/?flg=es-es&sort=fecha">“Situation of hospitalised coronavirus
patients in Castilla y
León”</a>,
<a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/ocupacion-de-camas-en-hospitales/table/?flg=es-es&sort=fecha">“Hospital bed
occupancy”</a>,
<a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-mortalidad-covid-por-zonas-basicas-de-salud/information/?sort=fecha">“Covid mortality rate by basic health
areas”</a>
and <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-mortalidad-por-centros-de-salud/table/?flg=es-es&sort=fecha">“Mortality rate by basic health
areas”</a>.</p>

- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/situacion-de-hospitalizados-por-coronavirus-en-castilla-y-leon/information/?flg=es-es&sort=fecha">Situation of hospitalised coronavirus patients in Castilla y
  León</a>.
  This database provides information at hospital level about the number
  of patients with COVID-19 or suspected COVID-19 on the ward and in
  ICU; the number of admissions for COVID-19 on the ward and in ICU; and
  the number of new discharges.</p>
- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/ocupacion-de-camas-en-hospitales/table/?flg=es-es&sort=fecha">Hospital bed
  occupancy</a>.
  This database provides information at hospital level about the number
  of ward and ICU beds available on each date.</p>
- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-mortalidad-covid-por-zonas-basicas-de-salud/information/?sort=fecha">Covid mortality rate by basic health
  areas</a>.
  This database provides information at the Basic Health Zone level
  about the number of deceased persons diagnosed with COVID-19 (both
  confirmed and with symptoms compatible with the disease). As
  highlighted in the description of the data, as of 01/03/2021, all
  persons with a positive diagnostic test in their Primary Care
  Electronic Health Record in the three months prior to their death are
  counted. Furthermore, as specified, the diagnosis of COVID-19 prior to
  death does not imply that this disease was the direct cause of death.</p>
- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/tasa-mortalidad-por-centros-de-salud/table/?flg=es-es&sort=fecha">Mortality rate by basic health
  areas</a>.
  This database provides information at the Basic Health Zone level,
  about the number of deceased persons, not only by COVID-19.</p>

<p align="justify">To construct the epidemiological indicators of severity, we proceeded
with the following steps.</p>

- <p align="justify">For hospital-level databases, we performed the hospital-H.A.
  assignment specified in the <a href="#section-2-target-variable-icu">Section 2. Target variable: ICU</a>,
  and for mortality databases, we performed the Basic Health Area-H.A.
  assignment with the information provided by the database itself.</p>
- <p align="justify">We checked the availability of data in the target time range between
  02/11/2020 and 06/03/2022. In the hospital-level datasets, we found
  that data are missing for four dates, corresponding to the following
  days: Saturday 3 July 2021, Sunday 4 July 2021, Saturday 10 July 2021
  and Sunday 11 July 2021. In addition, in the available bed dataset,
  there is also missing information for Sunday 22/08/2021. To perform
  the imputation of these five missing values, we rely on the data
  available for these dates at:</p>
  
  - <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/indicadores-de-riesgo-covid-19-por-provincias/table/?disjunctive.indicador&sort=fecha">COVID-19 risk indicators by provinces up to
    24-03-22</a>.</p>
  - <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/indicadores-de-riesgo-covid-19-por-municipios/table/?disjunctive.provincia&sort=fecha">COVID-19 risk indicators by municipalities until
    24-03-2022</a>.</p>
    
- <p align="justify">We aggregate the information of the variables by H.A.</p>
- <p align="justify">We defined the indicators explained in the <a href="https://www.sanidad.gob.es/profesionales/saludPublica/ccayes/alertasActual/nCov/documentos/Nueva_estrategia_vigilancia_y_control.pdf">Epidemic Surveillance
  Strategies of the Ministry of
  Health</a>,
  which include the following:</p>
  
  - <p align="justify"><strong>ward_rate</strong> or population occupancy rate to the ward. Calculated
    as the percentage ratio of the proportion of people with COVID-19 in
    ward beds to the total population per 100,000 inhabitants.</p>
  - <p align="justify"><strong>acute_rate</strong> or ICU population occupancy rate. Calculated as the
    percentage ratio of the proportion of people with COVID-19 in ICU
    beds to the total population per 100,000 inhabitants.</p>
  - <p align="justify"><strong>acute_rate7</strong> or rate of ICU inpatients in relation to the overall
    number of COVID-19 inpatients at 7 days. Calculated as the
    percentage ratio of COVID-19 ICU inpatients to the total number of
    COVID-19 inpatients at 7 days.</p>
  - <p align="justify"><strong>acute_rate14</strong> or rate of hospitalised patients in ICU in relation
    to the overall number of patients admitted for COVID-19 at 14 days.
    Calculated as the percentage ratio between those hospitalised by
    COVID-19 in ICU and the total number of those hospitalised by
    COVID-19 at 14 days.</p>
  - <p align="justify"><strong>ward_rate7</strong> or rate of hospitalised patients on the ward in
    relation to the overall number of patients admitted for COVID-19 at
    7 days. Calculated as the percentage ratio between those
    hospitalised for COVID-19 on the ward and the total number of those
    hospitalised by COVID-19 at 7 days.</p>
  - <p align="justify"><strong>ward_rate14</strong> or rate of hospitalised patients on the ward in
    relation to the overall number of patients admitted for COVID-19 at
    14 days. Calculated as the percentage ratio between those
    hospitalised for COVID-19 on the ward and the total number of those
    hospitalised for COVID-19 at 14 days.</p>
  - <p align="justify"><strong>income_ward_rate</strong> or rate of population admission to the ward due
    to COVID-19. Calculated as the ratio of the proportion of new ward
    admissions due to COVID-19 to the total population per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>income_ICU_rate</strong> or rate of population admission to ICU due to
    COVID-19. Calculated as the ratio of the proportion of new ICU
    admissions due to COVID-19 to the total population per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>disch_rate</strong> or population rate of new discharges. Calculated as
    the ratio of the number of new COVID-19 discharges to the total
    population per 100,000 inhabitants.</p>
  - <p align="justify"><strong>disch_rate7</strong> or population rate of new 7-day discharges.
    Calculated as the ratio of the number of new 7-day COVID-19
    discharges to the total population per 100,000 inhabitants.</p>
  - <p align="justify"><strong>disch_rate14</strong> or population rate of new discharges at 14 days.
    Calculated as the ratio of the number of new 14-day COVID-19
    discharges to the total population per 100,000 inhabitants.</p>
  - <p align="justify"><strong>death_rate</strong> or population rate of deaths due to COVID-19.
    Calculated as the ratio of the number of COVID-19 deaths to the
    total population per 1,000,000 inhabitants.</p>
  - <p align="justify"><strong>death_rate7</strong> or population rate of deaths due to 7-day COVID-19.
    Calculated as the ratio of the number of 7-day COVID-19 deaths to
    the total population per 1,000,000 inhabitants.</p>
  - <p align="justify"><strong>death_rate14</strong> or population rate of deaths due to 14-day
    COVID-19. Calculated as the ratio of the number of deaths from
    14-day COVID-19 to the total population per 1,000,000 inhabitants.</p>
  - <p align="justify"><strong>death_rate_covid_total</strong> or rate of COVID-19 deaths in relation to
    the total number of deaths. Calculated as the ratio of the number of
    COVID-19 deaths to the overall number of deaths.</p>
  - <p align="justify"><strong>death_rate_covid_total7</strong> or rate of COVID-19 deaths in relation
    to the total number of deaths at 7 days. Calculated as the ratio of
    the number of COVID-19 deaths to the overall number of deaths at 7
    days.</p>
  - <p align="justify"><strong>death_rate_covid_total14</strong> or rate of deaths due to COVID-19 in
    relation to the total number of deaths at 14 days. Calculated as the
    ratio of the number of COVID-19 deaths to the overall number of
    deaths at 14 days.</p>
    
- <p align="justify">Finally, we lagged each variable. In this way we take into account the
  time lag caused by delays in reporting and the inherent nature of the
  disease itself. Specifically, for variables calculated at 7 days, lags
  of 1 to 20 days are included, for variables calculated at 14 days,
  lags of 1 to 14 days are included, and for the rest of the variables,
  lags of 1 to 26 days are included.</p>

<p align="justify">All variables and their lags, together with the date, H.A., population
size and province identifier are stored in the csv file
<strong>COVID_SEVERITY_INDICATORS_LAGS</strong>, stored in the folder
<strong>EPIDEMIOLOGICAL INDICATORS</strong>, the header of which is shown below.</p>

``` r
# Load the data.table library
library(data.table)

# Read csv file
data <- as.data.frame(fread("./AUXILIARY INFORMATION/EPIDEMIOLOGICAL INDICATORS/COVID19_SEVERITY_INDICATORS_LAGS.csv"))

# Display the file header and some variables
head(data[,1:20])
```

    ##         date         HA ID_HA province acute_rate ward_rate acute_rate7
    ## 1 02/11/2020 H.A. ÁVILA  1701    Ávila   5.636131  43.21034    13.36303
    ## 2 03/11/2020 H.A. ÁVILA  1701    Ávila   5.636131  43.21034    13.30472
    ## 3 04/11/2020 H.A. ÁVILA  1701    Ávila   5.636131  45.08905    12.88344
    ## 4 05/11/2020 H.A. ÁVILA  1701    Ávila   4.383658  42.58410    12.13307
    ## 5 06/11/2020 H.A. ÁVILA  1701    Ávila   5.636131  43.21034    11.74242
    ## 6 07/11/2020 H.A. ÁVILA  1701    Ávila   5.636131  51.97766    11.21157
    ##   ward_rate7 acute_rate14 ward_rate14 income_ward_rate income_ICU_rate
    ## 1   86.63697     13.45912    86.54088         3.131184        0.000000
    ## 2   86.69528     13.28502    86.71498         3.757421        0.000000
    ## 3   87.11656     13.06358    86.93642         5.636131        0.000000
    ## 4   87.86693     12.86682    87.13318         4.383658        0.000000
    ## 5   88.25758     12.71930    87.28070         5.636131        1.252474
    ## 6   88.78843     12.35602    87.64398        11.272263        0.000000
    ##   disch_rate disch_rate7 disch_rate14 death_rate death_rate_covid_total
    ## 1   0.000000    18.16087     35.06926   0.000000              0.0000000
    ## 2   1.252474    15.65592     32.56431  12.524736              0.4000000
    ## 3   3.757421    14.40345     32.56431   6.262368              0.1666667
    ## 4   8.141079    16.90839     40.70539  12.524736              0.5000000
    ## 5   3.131184    18.78710     40.07916  25.049473              0.3333333
    ## 6   5.009895    23.17076     39.45292  12.524736              0.2222222
    ##   death_rate_covid_total7 death_rate_covid_total14 death_rate7
    ## 1               0.3000000                0.2282609    75.14842
    ## 2               0.3170732                0.2307692    81.41079
    ## 3               0.2941176                0.2235294    62.62368
    ## 4               0.3055556                0.2470588    68.88605
    ## 5               0.3125000                0.2804878    93.93552
    ## 6               0.3061224                0.2808989    93.93552

### 3.2.3 Care pressure

<p align="justify">We consulted the Open Data Portal of the Junta de Castilla y León. We
downloaded three databases. <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/situacion-de-hospitalizados-por-coronavirus-en-castilla-y-leon/information/?flg=es-es&sort=fecha">Situation of hospitalised coronavirus
patients in Castilla y
León</a>,
<a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/ocupacion-de-camas-en-hospitales/table/?flg=es-es&sort=fecha">Hospital bed
occupancy</a>,
and <a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/personas-vacunadas-covid/table/?disjunctive.provincia&sort=fecha">Persons vaccinated COVID-19 until
23-09-2022</a>.</p>

  
- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/situacion-de-hospitalizados-por-coronavirus-en-castilla-y-leon/information/?flg=es-es&sort=fecha">Situation of hospitalised coronavirus patients in Castilla y
  León</a>.
  This database provides information at hospital level about the number
  of patients with COVID-19 or suspected COVID-19 on the ward and in
  ICU; the number of admissions for COVID-19 on the ward and in ICU; and
  the number of new discharges.</p>

- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/ocupacion-de-camas-en-hospitales/table/?flg=es-es&sort=fecha">Hospital bed
  occupancy</a>.
  This database provides information at hospital level about the number
  of ward and ICU beds available on each date.</p>

- <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/personas-vacunadas-covid/table/?disjunctive.provincia&sort=fecha">Persons vaccinated COVID-19 until
  23-09-2022</a>.
  This database provides province-level information on the number of
  persons vaccinated with the first and third dose and fully vaccinated,
  counted as the number of doses administered each day. The dataset
  specifies that from 01/04/2022 with third dose refers to all booster
  doses, not just the third dose. To construct the epidemiological
  indicators of care pressure, the following steps were taken.</p>

- <p align="justify">For hospital-level databases, we performed the hospital-H.A.
  assignment specified in the <a href="#section-2-target-variable-icu">Section 2. Target variable: ICU</a>,
  and for mortality databases, we performed the Basic Health Area-H.A.
  assignment with the information provided by the database itself.</p>

- <p align="justify">We checked the availability of data in the target time range between
  02/11/2020 and 06/03/2022. In the hospital-level datasets, we found
  that data are missing for four dates, corresponding to the following
  days: Saturday 3 July 2021, Sunday 4 July 2021, Saturday 10 July 2021
  and Sunday 11 July 2021. In addition, in the available bed dataset,
  there is also missing information for Sunday 22/08/2021. To perform
  the imputation of these five missing values, we rely on the data
  available for these dates at:</p>

  - <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/indicadores-de-riesgo-covid-19-por-provincias/table/?disjunctive.indicador&sort=fecha">COVID-19 risk indicators by provinces up to
    24-03-22</a>.</p>
  - <p align="justify"><a href="https://analisis.datosabiertos.jcyl.es/explore/dataset/indicadores-de-riesgo-covid-19-por-municipios/table/?disjunctive.provincia&sort=fecha">COVID-19 risk indicators by municipalities until
    24-03-2022</a>.</p>

- <p align="justify">We aggregate the information for the variables by H.A. and by province
  in the case of the vaccines data. In addition, in the case of the
  vaccination dataset, for each date we also calculate the cumulative
  number of vaccines administered, in order to take into account the
  coverage of the population.</p>

- <p align="justify">We defined the indicators explained in the <a href="https://www.sanidad.gob.es/profesionales/saludPublica/ccayes/alertasActual/nCov/documentos/Nueva_estrategia_vigilancia_y_control.pdf">Epidemic Surveillance
  Strategies of the Ministry of
  Health</a>,
  which include the following:</p>

  - <p align="justify"><strong>ward_beds_rate</strong> or hospital occupancy rate on the ward. It is
    calculated as the percentage ratio of persons with COVID-19 or
    suspected COVID-19 hospitalised on the ward to the total number of
    beds available on the ward at that time.</p>
  - <p align="justify"><strong>ICU_beds_rate</strong> or ICU hospital occupancy rate calculated as the
    percentage ratio of persons with COVID-19 or suspected COVID-19
    hospitalised in the ICU to the total number of ICU beds available at
    that time.</p>
  - <p align="justify"><strong>inc_ward_rate7</strong> or rate of new 7-day ward admissions per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>inc_ward_rate14</strong> or rate of new 14-day ward admissions per
    100,000 inhabitants.</p>
  - <p align="justify"><strong>inc_ICU_rate7</strong> or rate of new 7-day ICU admissions per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>inc_ICU_rate14</strong> or rate of new 14-day ICU admissions per 100,000
    inhabitants.</p>
  - <p align="justify"><strong>vacc_complete_ratio</strong> or daily rate of fully vaccinated people.
    Calculated as the ratio of the number of fully vaccinated
    inhabitants in a day to the total provincial population.</p>
  - <p align="justify"><strong>vacc_complete_ratio_cum</strong> or cumulative rate of fully vaccinated
    people. Calculated as the ratio of the number of fully vaccinated
    inhabitants to date to the total provincial population.</p>
  - <p align="justify"><strong>vacc_1st_dose_ratio</strong> or daily rate of persons with one dose of
    vaccine. Calculated as the ratio of the number of persons with a
    dose of vaccine on a given day divided by the total provincial
    population.</p>
  - <p align="justify"><strong>vacc_1st_dose_ratio_cum</strong> or cumulative rate of persons with a
    dose of vaccine. Calculated as the ratio of the number of persons
    with a dose of vaccine to date divided by the total provincial
    population.</p>
  - <p align="justify"><strong>vacc_3rd_dose_ratio</strong> or daily rate of persons with the third dose
    of vaccine. Calculated as the ratio of the number of persons with
    the third dose of vaccine on a given day to the total provincial
    population.</p>
  - <p align="justify"><strong>vacc_3rd_dose_ratio_cum</strong> or cumulative rate of persons with the
    third dose of vaccine. Calculated as the ratio of the number of
    persons with the third dose of vaccine to date divided by the total
    provincial population.</p>

- <p align="justify">Finally, we lagged each variable. In this way we take into account the
  time lag caused by delays in reporting and the inherent nature of the
  disease itself. Specifically, for variables calculated at 7 days, lags
  of 1 to 20 days are included, for variables calculated at 14 days,
  lags of 1 to 14 days are included, and for the rest of the variables,
  lags of 1 to 26 days are included.</p>

<p align="justify">All variables and their lags, together with the date, H.A., population
size and province identifier are stored in the csv file
<strong>COVID-CARE-PRESSURE-INDICATORS_LAGS</strong>, stored in the folder
<strong>EPIDEMIOLOGICAL INDICATORS</strong>, the header of which is shown below.</p>



``` r
# Load the data.table library
library(data.table)

# Read csv file
data <- as.data.frame(fread("./AUXILIARY INFORMATION/EPIDEMIOLOGICAL INDICATORS/COVID19_CARE_PRESSURE_INDICATORS_LAGS.csv"))

# Display the file header and some variables
head(data[,1:22])
```

    ##         date         HA ID_HA province POP_2021 ICU ward ICU_beds ward_beds
    ## 1 02/11/2020 H.A. ÁVILA  1701    Ávila   159684   9   69       16       390
    ## 2 03/11/2020 H.A. ÁVILA  1701    Ávila   159684   9   69       18       390
    ## 3 04/11/2020 H.A. ÁVILA  1701    Ávila   159684   9   72       18       389
    ## 4 05/11/2020 H.A. ÁVILA  1701    Ávila   159684   7   68       18       388
    ## 5 06/11/2020 H.A. ÁVILA  1701    Ávila   159684   9   69       18       386
    ## 6 07/11/2020 H.A. ÁVILA  1701    Ávila   159684   9   83       18       385
    ##   ICU_beds_rate ward_beds_rate inc_ward_rate7 inc_ward_rate14 inc_ICU_rate7
    ## 1      56.25000       17.69231       32.56431        52.60389      2.504947
    ## 2      50.00000       17.69231       26.92818        53.23013      2.504947
    ## 3      50.00000       18.50900       29.43313        56.36131      1.878710
    ## 4      38.88889       17.52577       31.31184        53.23013      1.878710
    ## 5      50.00000       17.87565       31.31184        56.36131      2.504947
    ## 6      50.00000       21.55844       37.57421        64.50239      2.504947
    ##   inc_ICU_rate14 vacc_complete_ratio vacc_complete_ratio_cum
    ## 1       4.383658                   0                       0
    ## 2       3.757421                   0                       0
    ## 3       3.757421                   0                       0
    ## 4       3.757421                   0                       0
    ## 5       4.383658                   0                       0
    ## 6       4.383658                   0                       0
    ##   vacc_1st_dose_ratio vacc_1st_dose_ratio_cum vacc_3rd_dose_ratio
    ## 1                   0                       0                   0
    ## 2                   0                       0                   0
    ## 3                   0                       0                   0
    ## 4                   0                       0                   0
    ## 5                   0                       0                   0
    ## 6                   0                       0                   0
    ##   vacc_3rd_dose_ratio_cum ICU_LAG1
    ## 1                       0       10
    ## 2                       0        9
    ## 3                       0        9
    ## 4                       0        9
    ## 5                       0        7
    ## 6                       0        9

# Section 4. Definitive Dataset


<p align="justify">This section contains the final dataset used in the article. As
specified in the main of the paper, after a process of evaluation of the
auxiliary information, its correlation with the target variable,
epidemiological interpretability, performance in terms of cAIC and
goodness of fit of the different models proposed, the variables were
selected:</p>

- <p align="justify"><strong>acute_rate7</strong>: Cases admitted to ICU among all COVID-19 inpatients
  within 7 days.</p>
- <p align="justify"><strong>disch_rate14_LAG3</strong>: COVID-19 discharges within 14 days with a
  three-day lag among the total population.</p>
- <p align="justify"><strong>ward_rate_LAG2</strong>: COVID-19 ward occupancy delayed by two days among
  the total population.</p>
- <p align="justify"><strong>POP_2021</strong>: Total population included as offset in the model.</p>

<p align="justify">In the dataset, the csv <strong>COVID_DATASET</strong>, in addition to these
variables are included:</p>

- <p align="justify"><strong>date</strong>: Date between 02/11/2020 and 06/03/2022.</p>
- <p align="justify"><strong>HA</strong>: Name of the health area.</p>
- <p align="justify"><strong>ID_HA</strong>: Health area identifier.</p>
- <p align="justify"><strong>province</strong>: Name of the province.</p>
- <p align="justify"><strong>POP_2021</strong>: Population total. Offset in the model.</p>
- <p align="justify"><strong>ICU</strong>: Number of persons with COVID-19 in ICU. Target variable.</p>
- <p align="justify"><strong>acute_rate7</strong>: Cases admitted to ICU among all COVID-19 inpatients
  within 7 days. Auxiliary variable.</p>
- <p align="justify"><strong>disch_rate14_LAG3</strong>: COVID-19 discharges within 14 days lagged three
  days among the total population. Auxiliary variable.</p>
- <p align="justify"><strong>ward_rate_LAG2</strong>: COVID-19 occupancy in ward lagged two days among
  the total population. Auxiliary variable.</p>

``` r
# Load the data.table library
library(data.table)

# Read Excel file
data <- as.data.frame(fread("./COVID_DATASET.csv"))

# Display the file header
head(data)
```

    ##         date         HA ID_HA province POP_2021 ICU acute_rate7
    ## 1 02/11/2020 H.A. ÁVILA  1701    Ávila   159684   9    13.36303
    ## 2 03/11/2020 H.A. ÁVILA  1701    Ávila   159684   9    13.30472
    ## 3 04/11/2020 H.A. ÁVILA  1701    Ávila   159684   9    12.88344
    ## 4 05/11/2020 H.A. ÁVILA  1701    Ávila   159684   7    12.13307
    ## 5 06/11/2020 H.A. ÁVILA  1701    Ávila   159684   9    11.74242
    ## 6 07/11/2020 H.A. ÁVILA  1701    Ávila   159684   9    11.21157
    ##   disch_rate14_LAG3 ward_rate_LAG2
    ## 1          37.57421       36.32174
    ## 2          34.44302       38.20045
    ## 3          35.69550       43.21034
    ## 4          35.06926       43.21034
    ## 5          32.56431       45.08905
    ## 6          32.56431       42.58410


[^1]: This research is part of the grant PID2020-113578RB-I00, funded by
    MCIN/AEI/10.13039/501100011033/. It has also been supported by the
    Spanish grant PID2022-136878NB-I00, the Valencian grant
    Prometeo/2021/063, by the Xunta de Galicia (Grupos de Referencia
    Competitiva ED431C-2020/14) and by CITIC that is supported by Xunta
    de Galicia, convenio de colaboración entre la Consellería de
    Cultura, Educación, Formación Profesional e Universidades y las
    universidades gallegas para el refuerzo de los centros de
    investigación del Sistema Universitario de Galicia (CIGUS). The
    first author was also sponsored by the Spanish Grant for Predoctoral
    Research Trainees RD 103/2019 being this work part of grant
    PRE2021-100857, funded by MCIN/AEI/10.13039/501100011033/ and ESF+.
    In addition, we thank the Centro de Supercomputación de Galicia
    (CESGA) for providing their services for part of the simulations in
    this work.


