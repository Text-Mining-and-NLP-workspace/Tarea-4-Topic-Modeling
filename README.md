<a href="https://www.uvg.edu.gt/"><img align="left" src="https://www.uvg.edu.gt/wp-content/uploads/socialshare-logo.jpg" width="90" height="90"></a>
**_Corporate Master in Applied Data Science_**<br/>
**_Text Mining & Natural Language Processing_**<br/>
**_Catedratico: Pedro Alberto Aguilar Nuñez_**<br/>
<br/>

Integrantes:
- Aldo Montenegro Margnoni
- Gabriel Fernando Montenero Ortiz
- Axel Adolfo Muralles Carranza
- German Antonio Oliva Muralles

# Tarea 4 Topic Modeling

- [Descripcion](#descripcion)
- [Requerimientos](#requerimientos)
- [Instrucciones](#instrucciones)
- [Metodologia y Resultados](#metodologia-y-resultados)
- [Contribuciones](#contribuciones)


## Descripcion:
La presente tarea trata del desarrollo, preprocesamiento, entranamiento, evaluacion y seleccion de hiperparametros para modelado de Topicos utilizando LDA. 

## Requerimientos:
- Requiere instalacion de [Anaconda Distribution](https://www.anaconda.com/products/distribution) o [miniconda](https://docs.conda.io/en/latest/miniconda.html)

## Instrucciones:

clonamos este repositorio en el directorio deseado desde una terminal, para windows podemos utilizar [Git Bash](https://gitforwindows.org/) 
```
git clone https://github.com/Text-Mining-and-NLP-workspace/Tarea-4-Topic-Modeling.git
```
Desde la terminal o anaconda prompt (windows) creamos un ambiente virtual con conda (reemplazar my_enviroment con el nombre deseado)
```
conda create -n my_enviroment
```
cuando conda pregunte por la confirmacion responder con _y_
```
proceed ([y]/n)?
```
una vez creado el ambiente virtual, proceder a activarlo
```
conda activate my_enviroment
```
instalamos los siguientes paquetes en nuestro ambiente:
```
pip install jupyter
pip install notebook
pip install "spacy[lookups]"
python -m spacy download es_core_news_sm
pip install pyLDAvis
pip install -U gensim
pip install matplotlib 
pip install seaborn 
pip install spacy
pip install clean-text
```
nos cambiamos hacia el path en donde se clono el repositorio
```
cd Tarea-4-Topic-Modeling
```
ejecutamos nuestro notebook
```
jupyter notebook "TopicModelling2-NLP-4.ipynb"
```

## Metodologia y Resultados:

Para nuestro modelo utilizamos el dump de wikipedia en español sugerido (https://dumps.wikimedia.org/eswiki/20220620/), del cual realizamos varias pruebas de carga y preprocesamiento del texto, tratamos de utilizar el dump completo (mas de 3.5 Millones de articulos) pero nos fue imposible poder procesar esa cantidad de texto (limpiar texto,tokenizar, remover stopwords, lemmatizar, etc.) por lo cual realizamos varias pruebas quedandonos con unicamente 1500 articulos para el entrenamiento de nuestro modelo.

primero realizamos la carga de nuestros documentos a un dataframe, del cual extraemos el texto a una lista, y utilizamos las siguientes expresiones regulares para comenzar el preprocesamiento del texto:

```
#Eliminar newlines y otros caracteres de espacios
data = [re.sub(r'\s+', ' ', sent) for sent in data]

#Eliminar tags HTML
CLEANR = re.compile('<.*?>')
data = [re.sub(CLEANR, '', sent) for sent in data]

#Quitar tildes
data = [re.sub(
        r"([^n\u0300-\u036f]|n(?!\u0303(?![\u0300-\u036f])))[\u0300-\u036f]+", r"\1", 
        normalize( "NFD", sent), 0, re.I
    ) for sent in data]

# Quitar Propiedades de configuracion
aligns = re.compile('(align|text-align)[=](center|left|right)')
data = [re.sub(aligns, '', sent) for sent in data]

aligns = re.compile('(align|text-align)[=]["](center|left|right)["]')
data = [re.sub(aligns, '', sent) for sent in data]

# Quitarmos palabras comunes propias de Wiki
wikiwords = re.compile('(left|right|width|class|align|style|background|Archivo|font|weight|bold|size|colspan|urlarchivo|fechaarchivo|deadurl|fechaacceso|bgcolor|listaref|wikitable|thumb)')
data = [re.sub(wikiwords, '', sent) for sent in data]
```
Y continuamos utilizando el pipeline de spacy, para lemmatizar, quitar puntuaciones y stopwords, removiendo los siguientes tags de Part of speech:
```
removal= ['ADV','PRON','CCONJ','PUNCT','PART','DET','ADP','SPACE', 'NUM', 'SYM']
```
uilizamos nlp.pipe para poder realizar el procesamiento de una manera mas rapida y eficiente. El pipeline de Spacy implementa el siguiente flujo de funciones:<br>
![nlp pipe](https://user-images.githubusercontent.com/70925688/178872351-056253ee-9b19-4670-a4a8-7c2fadd2a97d.JPG)

Para la seleccion de hyperparametros utilizaremos dos mediciones, la coherencia u_mass y la coherencia por c_v, y evaluamos cual satisface mejor ambos casos, podemos observar las siguientes graficas obtenidas de nuestras pruebas

- u_mass<br>
![umass](https://user-images.githubusercontent.com/70925688/178891165-2988fac4-b42c-4d31-a35b-00c0a761cc8b.png)

- c_v<br>
![c_v](https://user-images.githubusercontent.com/70925688/178891188-459f7bca-9144-4955-af04-194f79376468.png)

De las graficas anteriores podemos determinar que nuestro mejor modelo es en el que utilizamos 9 topicos para ambos puntajes, cumple con el mejor puntaje para ambas mediciones.

Procedimos a entrenar con estos hyerparametros y obtuvimos los siguientes topicos, de los cuales podemos inferir el tema de las palabras de los topicos:

**Geografia**
```
(0,
  '0.007*"ciudad" + 0.007*"web" + 0.006*"españa" + 0.006*"provincia" + 0.006*"nacional" + 0.006*"año" + 0.005*"pais" + 0.005*"san" + 0.004*"poblacion" + 0.004*"gobierno"')
```
**Cine / Literatura** 
```
(1,
  '0.008*"año" + 0.008*"obra" + 0.005*"libro" + 0.005*"ingl" + 0.005*"pelicula" + 0.004*"web" + 0.004*"novela" + 0.003*"literatura" + 0.003*"historia" + 0.003*"cine"')
```
**Historia**
```
(2,
  '0.012*"lengua" + 0.011*"idioma" + 0.007*"español" + 0.006*"derecho" + 0.005*"guerra" + 0.005*"país" + 0.004*"juego" + 0.004*"mundial" + 0.004*"mundo" + 0.004*"francia"')
```
**Botanica**
```
(3,
  '0.016*"familia" + 0.016*"genero" + 0.015*"especie" + 0.013*"world" + 0.009*"planta" + 0.008*"image" + 0.008*"grass" + 0.007*"flor" + 0.007*"poaceae" + 0.006*"flora"')
```
**Cristianismo**
```
(4,
  '0.011*"libro" + 0.009*"jesus" + 0.009*"dios" + 0.006*"iglesia" + 0.005*"cristiano" + 0.005*"citar" + 0.004*"evangelio" + 0.004*"año" + 0.004*"university" + 0.004*"cita"')
```
**Tecnologias de la informacion**
```
(5,
  '0.009*"sistema" + 0.008*"numero" + 0.006*"web" + 0.004*"lenguaje" + 0.004*"ejemplo" + 0.004*"codigo" + 0.004*"dato" + 0.004*"utilizar" + 0.004*"tipo" + 0.004*"forma"')
```
**Teoria de la Evolucion**
```
(6,
  '0.006*"celula" + 0.006*"publicacion" + 0.006*"especie" + 0.005*"planta" + 0.005*"grupo" + 0.004*"enfermedad" + 0.004*"forma" + 0.004*"tipo" + 0.004*"formar" + 0.004*"sistema"')
```
**Premios Cinematograficos**
```
(7,
  '0.040*"premio" + 0.037*"anexo" + 0.030*"edicion" + 0.024*"goya" + 0.023*"premios" + 0.016*"pelicula" + 0.009*"jose" + 0.009*"candidato" + 0.007*"luis" + 0.006*"cccccc"')
```
**Ciencia y Arte**
```
(7,
  '0.040*"premio" + 0.037*"anexo" + 0.030*"edicion" + 0.024*"goya" + 0.023*"premios" + 0.016*"pelicula" + 0.009*"jose" + 0.009*"candidato" + 0.007*"luis" + 0.006*"cccccc"')
```

La coherencia c_v para este modelo es de **0.512896**<br>

Con nuestro modelo entrenado, procedemos a evaluar el siguiente articulo, que tiene como titulo **_JPG o PNG: diferencias e idoneidad de los formatos de imagen_** a continuacion se adjunta un fragmento del texto sin procesar:

> '{{Referencias|t=20180621142211}}\n{{Ficha de formato de archivo\n|nombre          = JPEG \n|icono           = \n|captura         = [[Archivo:Phalaenopsis JPEG.png|240px]]\n|pie             = Foto de una flor comprimida gradualmente con el formato JPEG.\n|extensión       = .jpeg</tt>, <tt>.jpg</tt>, <tt>.jpe</tt><br /><tt>.jfif</tt>, <tt>.jfi</tt>, <tt>.jif</tt> (contenedores)\n|MIME            = image/jpeg\n|type_code       = JPEG\n|uniform_type    = public.jpeg\n|número_mágico   = ff d8\n|desarrollador   = \'\'Joint Photographic Experts Group\'\'\n|género          = [[Formatos gráficos|Gráficos]] con [[Algoritmo de compresión con pérdida|compresión con pérdida]]\n|contenedor_para = \n|contenido_por   = \n|extendido_de    = \n|extendido_a     = \n|estándar        = \n}}\n\n\'\'\'Joint Photographic Experts Group\'\'\' (\'\'\'JPEG\'\'\') es el nombre de un comité de expertos que creó un [[Norma (tecnología)|estándar]] de [[Compresión de datos|compresión]] y [[Codificación digital|codificación]] de [[Archivo (informática)|archivos]] e [[Imagen|imágenes]] fijas, que es actualmente uno de los formatos más utilizados para fotografías<ref>{{Cita web|url=https://www.ionos.mx/digitalguide/paginas-web/diseno-web/jpg-o-png/|título=JPG o PNG: diferencias e idoneidad de los formatos de imagen|fechaacceso=2022-05-16|sitioweb=IONOS Digitalguide|idioma=es}}</ref>. Este comité fue integrado desde sus inicios por la fusión de varias agrupaciones en un intento de compartir y desarrollar su experiencia en la digitalización de imágenes. La [[ISO]], tres años antes (abril de [[1983]]), había iniciado sus investigaciones en el área.\n\nAdemás de ser un método de compresión, es a menudo considerado como un [[formato de archivo]]. \'\'\'JPEG/Exif\'\'\' es el formato de imagen más común, utilizado por las [[Cámara digital|cámaras fotográficas digitales]] y otros dispositivos de captura de imagen, junto con \'\'\'JPG/JFIF\'\'\', que también es otro formato para el almacenamiento y la transmisión de imágenes fotográficas en la [[World Wide Web]]. Estas variaciones de formatos a menudo no se distinguen, y se llaman “JPEG”. Los archivos de este tipo se suelen nombrar con la [[extensión de archivo|extensión]] <code>.jpg</code>.\n\n== Compresión del JPEG ==\n\n[[Archivo:Webp - Jpeg - Lossless comparative.png|miniatura|280px|Comparativa de calidad entre la imagen original, comprimida en JPG (con pérdida) y comprimida en [[WebP]] (con pérdida).]]\nEl formato JPEG utiliza habitualmente un [[algoritmo de compresión con pérdida]] para reducir el tamaño de los [[Archivo (informática)|archivos]] de imágenes. Esto significa que al descomprimir o visualizar la imagen no se obtiene exactamente la misma imagen de la que se partía antes de la [[Compresión de datos|compresión]]. Existen también tres variantes del estándar JPEG que comprimen la imagen sin pérdida de datos: [[JPEG 2000]], [[JPEG-LS]] y [[Lossless JPEG]]. \n\nEl algoritmo de compresión JPEG se basa en dos fenómenos visuales del [[ojo humano]]: uno es el hecho de que es mucho más sensible al cambio en la [[luminancia]] que en la [[crominancia]]; es decir, capta más claramente los cambios de [[Luminosidad|brillo]] que de [[color]]. El otro es que nota con más facilidad pequeños cambios de brillo en zonas homogéneas que en zonas donde la variación es grande, por ejemplo en los bordes de los cuerpos de los objetos.\n\nUna de las características del JPEG es la flexibilidad a la hora de ajustar el grado de compresión. Un grado de compresión muy alto generará un archivo de pequeño tamaño, a costa de una pérdida significativa de calidad. Con una tasa de compresión baja se obtiene una calidad de imagen muy parecida a la del original, pero con un [[tamaño de archivo]] mayor.\n\nLa pérdida de calidad cuando se realizan sucesivas compresiones es acumulativa. Esto significa que si se comprime una imagen y se descomprime, se perderá calidad de imagen, pero si se vuelve a comprimir una imagen ya comprimida se obtendrá una pérdida todavía mayor. Cada sucesiva compresión causará pérdidas adicionales de calidad. La compresión con pérdida no es conveniente en imágenes o gráficos que tengan [[texto]]s, líneas o bordes muy definidos, pero sí para archivos que contengan grandes áreas de colores sólidos.\n\nEl formato JPEG no incluye en su codificación la gestión del \'\'\'canal alfa,\'\'\' que es lo que define la opacidad de un [[píxel]] en una imagen. Por tanto a diferencia de formatos como el [[PNG]], o variantes del estándar como el [[JPEG 2000]], el formato \'\'\'JPEG estándar no es capaz de gestionar transparencias\'\'\'.\n\n== Codificación ==\n\nMuchas de las opciones del estándar JPEG se usan poco. Esto es una descripción breve de uno de los muchos métodos usados comúnmente para comprimir imágenes cuando se aplican a una imagen de entrada con 24 [[bits por pixel]] (ocho por cada [[Modelo de color RGB|rojo, verde, y azul]], o también dicho "8 bits por canal"). Esta opción particular es un método de [[compresión con pérdida]].\n\n=== Transformación del espacio de color ===\n[[Archivo:Cubo RGB con las capas de color.png|miniatura|200px|Esquema del modelo RGB.]]\n[[Archivo:Cubo YUV con las capas de color.png|miniatura|200px|Esquema del modelo YUV.]]\nComienza convirtiendo la imagen desde su [[modelo de color RGB]] a otro llamado [[Modelo de color YUV|YUV]] o YCbCr. Este espacio de color es similar al que usan los sistemas de color para [[televisión]] [[PAL]] y [[NTSC]], pero es mucho más parecido al sistema de televisión [[:en:Multiplexed Analogue Components| MAC]] (Componentes Analógicas Multiplexadas).\n\nEste espacio de color (YUV) tiene tres componentes:\n* La componente \'\'\'Y\'\'\', o [[luminancia]] (información de brillo); es decir, la imagen en [[escala de grises]].\n* Las componentes \'\'\'U\'\'\' o Cb y \'\'\'V\'\'\' o Cr, respectivamente diferencia del azul (relativiza la imagen entre [[azul]] y [[rojo]]) y diferencia del rojo (relativiza la imagen entre [[verde]] y [[rojo]]); ambas señales son conocidas como [[crominancia]] (información de color).\n\nLas [[Ecuación|ecuaciones]] que realizan este cambio de base de RGB a YUV son las siguientes:\n\n Y  =      0,257 * R + 0,504 * G + 0,098 * B + 16\n Cb = U = -0,148 * R - 0,291 * G + 0,439 * B + 128\n Cr = V =  0,439 * R - 0,368 * G - 0,071 * B + 128\n\nLas ecuaciones para el cambio inverso se pued


Al evaluarlo en nuestro modelo, obtenenmos el siguiente resultado:
```
[(5, 0.9990085)]
```
Del cual vemos que el modelo clasifica con un 99.90% de probabilidad que sea del topico 5, el cual nosotros etiquetamos como **_Tecnologias de la informacion_** lo cual hace mucho sentido con el topico del contenido del articulo.


Para finaliza utilizamos la libreria **pyLDAvis.gensim_models** para visualizar nuestros topicos de forma grafica e interactiva, y exportamos a un archivo html, al cual podemos acceder en el siguiente enlace, o con el archivo index.html:<br>

[Visualizacion](https://htmlpreview.github.io/?https://github.com/Text-Mining-and-NLP-workspace/Tarea-4-Topic-Modeling/blob/main/index.html)

en la grafica de la derecha podemos ver un mapa de distancia inter topico, que se realiza apartir de un analisis de componentes principales (PCA) y del lado derecho podemos visualizar los 30 terminos mas relevantes para cada topico.
![intertopic](https://user-images.githubusercontent.com/70925688/178893312-28cd4c2c-6dbb-4857-88cb-b4cc7c156a45.JPG)



## Contribuciones

- Aldo Montenegro Margnoni
	- Seleccion de mejor modelo
	- Metodo de prueba de hyperparametros 
<br/>

- Gabriel Fernando Montenero Ortiz
	- clasificacion de topicos
	- Preprocesamiento de texto
    
<br/>

- Axel Adolfo Muralles Carranza
	- Pruebas de datasets
	- Seleccion de documentos, para mejor desempeño con nuestro poder de computo actual

<br/>

- German Antonio Oliva Muralles
	- Graficas de Topicos interactivas
	- Sanity Check

<br/>

