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






Podemos observar nuestros topicos de forma grafica e interactiva en el siguiente enlace:
[Visualizacion](https://htmlpreview.github.io/?https://github.com/Text-Mining-and-NLP-workspace/Tarea-4-Topic-Modeling/blob/main/index.html)
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

