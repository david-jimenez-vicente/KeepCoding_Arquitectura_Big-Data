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
**A.- Definición la estrategia del DAaaS**  

El objetivo de esta arquitectura es desplegar una infraestructura suficientemente eficiente para analizar las imágenes tomadas por los últimos satélites de reconocimiento óptico de la Tierra en varias frecuencias y los de reconocimiento por radar SAR en varias bandas. El fin del proyecto es de Investigación. Se usará una comparación de las imágenes en varios espectros ópticos y de radar de las mismas coordenadas terrestres a lo largo de varios puntos de un periodo de tiempo, para intentar detectar cambios en los niveles del mar, geografía de las costas y cambios en la tectónica terrestre como los generados por  ejemplo por terremotos, movimientos de placas o fuerzas naturales sobre tierra firme. Servirá para predecir futuros cambios en la geología del planeta.  
Dichas imágenes son públicas, y publicadas por los organismos oficiales y científicos responsables de las sucesivas misiones de reconocimiento satelital.  
La frecuencia de actualización será puntual y realizada manualmente, bien por periodicidad cada 6 meses, o tras desastres naturales como erupciones, corrimientos de tierras, maremotos, etc….  

**B.- Arquitectura DAaaS**  
**Fuentes de datos:**
- Obtención directa de los datos del bucket de Amazon de los resultados de los satélites de Umbra: arn:aws:s3:::umbra-open-data-catalog/
- Obtención directa de los datos del bucket de Amazon de los resultados de los satélites de Capella: arn:aws:s3:::capella-open-data/data/
- Obtención de los datos del programa de misiones Copernicus con la API propia de la ESA: https://apihub.copernicus.eu/apihub/
- Componentes:
- Google Cloud Storage para almacenamiento de las imágenes y metadatos extraídos.
- Google BigQuery para crear la base de datos maestra del datawarehouse.
- Cluster de Dataproc de Google Cloud para uso con Jupyter Notebooks.


DAaaS Operating Model Design and Rollout
Crear y configurar un Proyecto de Google Cloud.
Crear un bucket de almacenamiento en Google Storage.
Crear un Cluster en Dataproc con autoinstalación de Jupyter Notebooks.
Configurar Hadoop, y Yarn para distribuir el servicio de Jupyter Notebooks.
Usar un Notebook de Python usando la API de Amazon para extraer los datos de los dos buckets de S3, y conectar con la API REST  de OpenSearch del repositorio del proyecto Copernicus. Dicho Notebook extraerá las imágenes del periodo que nos interesa y su metadata y las volcará al bucket de Google Storage.
Otro Notebook se usará para empezar el procesamiento ELT conectándose al bucket de Google Storage, leer las imágenes y transformarlas a su matriz de valores en escalas de grises (las imágenes de radar SAR son en grises)
