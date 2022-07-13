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

## Contribuciones

- Aldo Montenegro Margnoni
	- Tokenizacion y preparacion de data
	- Metodo de prueba de hyperparametros 
<br/>

- Gabriel Fernando Montenero Ortiz
	- Optimizacion en metodo para remover stopwords
	- Organizacion de funciones en pipeline
    
<br/>

- Axel Adolfo Muralles Carranza
	- Elaboracion de clase para modelo, metodos fit, predict y val_test
	- seleccion de Hyperparametros

<br/>

- German Antonio Oliva Muralles
	- Pruebas para obtener mejor modelo
	- Sanity Check

<br/>

