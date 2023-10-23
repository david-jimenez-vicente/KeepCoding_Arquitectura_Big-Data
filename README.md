# Repositorio para la asignatura de "Arquitectura Big-Data"
# Repository for "Database Model Design and Intro to SQL" subject
---
## *Contenido del repositorio*  
· Este README.md, con la descripción del enunciado y la resolución de la práctica.

## *Repository content*  
· This README.md, that contains the exercise statement and its resolution.

---  

## *Enunciado del ejercicio*
· Este es el proyecto final de la asignatura de "Arquitectura Big-Data". Se pedía diseñar y especificar un data lake para procesar los datos de distintas fuentes, extraídos de sitios de dominio público, dimensionando adecuadamente los requerimientos e infraestructura necesaria, así como los pasos operativos de implantación.
· Para el proyecto me comportaré como un arquitecto de DataOps de una agencia del gobierno nacional que se encarga de monitorizar cambios en la tierra firme y los mares y océanos para detectar consecuencias de desastres naturales y el cambio climático, y poder predecir situaciones de peligro para los asentamientos humanos a lo largo del planeta.
· Nos encargaremos de tomar las imágenes de radar SAR de los satélites de los proyectos recientes y existentes, y para varias localizaciones compararemos las imágenes de las mismas coordenadas a lo largo de varios años en sucesión, detectando cambios en la superficie planetaria usando técnicas de ML. Dichas imágenes se tratarán usando la matriz de valores en escala de grises para cada píxel.


## *Exercise statement*  
· This is the final project for the "Big-Data Architecture" subject. The request was to design and specify a data lake to process data from different sources, extracted from public domain sites, appropriately sizing the requirements and necessary infrastructure, as well as the operational implementation steps.
· For the project I will perform the role of a DataOps architect working for a national government agency that is responsible for monitoring changes in land and seas and oceans to detect consequences of natural disasters and climate change, and be able to predict dangerous situations for human settlements throughout the planet.
· We will take the SAR radar images from satellites of recent and existing projects, and for various locations we will compare images of the same coordinates over several years in succession, detecting changes in the planetary surface using ML techniques. These images will be treated using the matrix of grayscale values ​​for each pixel.  

---  

## *Resolución del ejercicio*  
### **Diseño del DAaaS**  

**A. Definición la estrategia del DAaaS**  

El objetivo de esta arquitectura es desplegar una infraestructura suficientemente eficiente para analizar las imágenes tomadas por los últimos satélites de reconocimiento óptico de la Tierra en varias frecuencias y los de reconocimiento por radar SAR en varias bandas. El fin del proyecto es de Investigación. Se usará una comparación de las imágenes en varios espectros ópticos y de radar de las mismas coordenadas terrestres a lo largo de varios puntos de un periodo de tiempo, para intentar detectar cambios en los niveles del mar, geografía de las costas y cambios en la tectónica como los generados por  ejemplo por terremotos, movimientos de placas o fuerzas naturales sobre tierra firme. Servirá para predecir cambios catastróficos en la geología del planeta.  
Dichas imágenes son públicas, y publicadas por los organismos oficiales y científicos responsables de las sucesivas misiones de reconocimiento satelital.  
La frecuencia de actualización será puntual y realizada manualmente, bien por periodicidad mensual, o tras desastres naturales conocidos como erupciones, corrimientos de tierras, maremotos, tsunamis, etc….  
El resultado de cada estudio será personalizado y presentado al responsable de la Agencia.

**B. Arquitectura DAaaS**  

**Fuentes de datos:**  

- Obtención directa de los datos del bucket de Amazon de los resultados de los satélites de Umbra:  
    - arn:aws:s3:::umbra-open-data-catalog/
- Obtención directa de los datos del bucket de Amazon de los resultados de los satélites de Capella:  
    - arn:aws:s3:::capella-open-data/data/
- Obtención de los datos del programa de misiones Copernicus con la API propia de la ESA: https://apihub.copernicus.eu/apihub/.  
.

**Componentes:**  

+ Google Cloud Storage para almacenamiento de las imágenes y metadatos extraídos.
+ Google BigQuery para crear la base de datos maestra del datawarehouse.
+ Cluster de Dataproc de Google Cloud para uso con Jupyter Notebooks, dimensionado con al menos 5 workers además del master.  
.

**C. DAaaS Operating Model Design y Lanzamiento**  

1. Crear y configurar un Proyecto de Google Cloud.
2. Crear un bucket de almacenamiento en Google Storage.
3. Crear un Cluster en Dataproc con autoinstalación de Jupyter Notebooks.
4. Configurar Hadoop, y Yarn para distribuir el servicio de Jupyter Notebooks.
5. Usar un Notebook de Python usando la API de Amazon para extraer los datos de los dos buckets de S3, y conectar con la API REST  de OpenSearch del repositorio del proyecto Copernicus. Dicho Notebook extraerá las imágenes del periodo que nos interesa y su metadata y las volcará al bucket de Google Storage.
6. Otro Notebook se usará para empezar el procesamiento ELT conectándose al bucket de Google Storage, leer las imágenes y transformarlas a su matriz de valores en escalas de grises (las imágenes de radar SAR son en grises). Luego insertará las matrices de cada imagen en una tabla creada para alojar la tabla principal del data warehouse en BigQuery, en el campo de "pixel_matrix". En dicha tabla habrá otros tres campos, uno de ellos con la coordenada central de la imagen, otro campo con con la fecha de escaneo, y otro más con el link a la imagen original del bucket, los cuales se completarán con sucesivos párrafos del notebook.
7. Un tercer Notebook se usará para el modelo de ML. Tomará las matrices de las localizaciones deseadas en las dos fechas a comparar y realizará los cálculos de diferencias entre las matrices de imágenes de cada localización. El resultado lo comparará con las diferencias entre imágenes resultantes de otros sucesos ya etiquetados como catástrofe o no, y decidirá si el cambio detectado es suficientemente importante como para etiquetarlo como un suceso crítico, o por el contrario no lo es. Devolverá un resultado con los valores en escala de grises para formar una imagen de dónde están los cambios, generará su imagen, y su etiqueta resultado de la evaluación de evento crítico, para que el operador pueda generar el informe.  

## *Exercise solution*  
### **DAaaS Design**  

**A. DAaaS Strategy definition**  

The objective of this architecture is to deploy an infrastructure sufficiently efficient to analyze the images taken by the latest optical reconnaissance satellites over the Earth using various frequencies, and SAR radar reconnaissance satellites in various bands too. The purpose of the project is Research. A comparison of images in various optical and radar spectra of the same Earth coordinates over various points over a period of time will be used to attempt to detect changes in sea levels, coastal geography and changes in tectonics such as those generated, for example, by earthquakes, plate movements or natural forces on land. It will serve to predict catastrophic changes in the planet's geology.
These images are public, and published by the official and scientific organizations responsible for successive satellite reconnaissance missions.
The update frequency will be punctual and carried out manually, either on a monthly basis, or after known natural disasters such as eruptions, landslides, tidal waves, tsunamis, etc...
The result of each study will be personalized and presented to the Agency Manager.  

**B. DAaaS Architecture**  

**Data sources:**  

- Direct obtaining of data from the Amazon bucket of the results of the Umbra satellites:
     - arn:aws:s3:::umbra-open-data-catalog/
- Direct obtaining of data from the Amazon bucket of Capella satellite results:
     - arn:aws:s3:::capella-open-data/data/
- Obtaining data from the Copernicus mission program with ESA's own API: https://apihub.copernicus.eu/apihub/.  
.  
**Components:**

+ Google Cloud Storage for extracted images and metadata storing.
+ Google BigQuery to create the master data warehouse database.
+ Google Cloud Dataproc cluster for use with Jupyter Notebooks, sized with at least 5 workers in addition to the master.  
.  
**C. DAaaS Operating Model Design and Rollout**

1. Create and configure a Google Cloud Project.
2. Create a storage bucket in Google Storage.
3. Create a Cluster in Dataproc with auto-installation of Jupyter Notebooks.
4. Hadoop and Yarn configuration to distribute the Jupyter Notebooks service.
5. Usage of a first Python Notebook using the Amazon API to extract the data from the two S3 buckets, and connect to the OpenSearch REST API of the Copernicus project repository. This Notebook will extract the images of the time period of interest and their metadata too, and dump them into the Google Storage bucket.
6. A second Notebook will be used to start the ELT processing by connecting to the Google Storage bucket, reading the images and transforming them to its matrix of grayscale values (SAR radar images are B&W). It will then insert the matrices from each image into a table created to host the main table of the data warehouse in BigQuery, in the "pixel_matrix" field. In this table there will be three other fields, one of them with the central coordinate of the image, another field with the scanning date, and another with the link to the original image on the bucket, which will be completed with successive paragraphs of the notebook.
7. A third Notebook will be used for the ML model. It will take the matrices for the desired locations on both dates to be compared and will perform the calculations of the differences between the image matrices of each location. The result will be compared with the differences between images resulting from other events already labeled as a catastrophe or not, and will decide if the change detected is important enough to label it as a critical event, or on the contrary, it is not. It will return a result with gray scale values to form an image of where the changes are, it will generate its image, and its label resulting from the critical event evaluation, so that the operator can generate the report.


### **Diagrama / Diagram**
