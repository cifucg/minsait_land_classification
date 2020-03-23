# Reto Minsait Land Classification

## UniversityHack2020 - Equipo: CMBC

![Logo UniversityHack2020](data/img/logo-light.png)

## Introducción

Este proyecto fundamenta la propuesta ideada por el equipo **CMBC** para el reto de [UniversityHack 2020 DATATHON](https://www.cajamardatalab.com/datathon-cajamar-universityhack-2020/)

El equipo está compuesto por:

* [Cristian Cifuentes García](https://www.linkedin.com/in/cifucg)
* [Manuel Bermúdez Martínez](https://www.linkedin.com/in/manuelbermudezmartinez)


El objetivo del reto **Minsait Land Classification** consiste en maximizar la _exactitud_, que se define como el _número de registros correctamente clasificados / número total de registros proporcionados por la Organización_, tal y como se indica en la propia página oficial.

Para ello se cuenta con dos ficheros, los cuales contienen un listado de superficies sobre las que se han reocrtado la imagen del satélite Sentinel II del servicio Copernicus de la Agencia Espacial Europea y se han extraído una serie de características de sus geometrías, posición, colores, etc. Y finalmente, se ha etiquetado el conjunto de los datos según la clasificación de suelo.

![Mapa introducctorio de ejemplo](data/img/mapa_introduccion.jpg)

Se cuenta con un fichero para realizar el análisis y la generación de los algoritmos de clasificación empleados, denominado _Estimar.txt_ y también con un segundo fichero, el cual se utilizará para realizar la entrega a la organzación. Este segundo fichero contiene una serie de registros con las mismas variables pero sin la etiqueta que clasifique el suelo.

## Tecnología necesaria para la realización de este proyecto:
* [Python](https://www.python.org)
* [Jupyter notebook](https://jupyter.org)
* [scikit-learn](https://scikit-learn.org/stable/index.html)
* [Pandas](https://pandas.pydata.org)
* [XGBoost](https://xgboost.readthedocs.io/en/latest/)

## Breve resumen del trabajo desarrollado


Con el fin de cumplir el principal objetivo del reto, se ha realizado un extenso análisis previo para comprenter y aprender lo máximo posible acerda de las imágenes extraida por el satélite Sentinel II del servicio Copernicus de la Agencia Espacial Europea, ya que la mayoría de las variables del conjunto de datos pertenecen a la misma. Además, se ha comprobado la importancia de cada una de estas variables en función de nuestro propósito.

Después de revisar toda la información posible y de un análisis exploratorio, el cual se comenta en el siguiente apartado, se ha construido un conjunto de datos en función de los problemas que han ido surgiendo (valores atípicos, outliers, valores perdidos, etc). Una vez estudiado el conjunto de datos, se ha continuado con una estrategia de apilamiento de modelos, es decir, se ha desarrollado un apartado comparando diversos modelos binarios para finalmente, seleccionar aquel que ofrezca un resultado más preciso y robusto. Posteriormente se ha desarrollado un modelo multietiqueta mediante el resultado obtenido en el primer modelo.

En todos y cada uno de estos modelos se ha realizado un intenso estudio de los hiperparámetros, regresores, parámetros y distintos conjuntos de entrenamiento y validación mediante técnicas como la validación cruzada. Finalmente se han comprobado los resultados con un conjunto de validación previamente definido, el cual no se ha utilizado en ninguna otra fase del proyecto.


## Análisis exploratorio y manipulación de variables

El principal problema de este reto es el desbalanceo que existe en el conjunto de datos proporcionado por la Organización, por lo tanto, para intentar reducir o comprender los registros y las variables, se ha realizado un profundo análisis del mismo y previamente de los aspectos que pudiesen favorecer la tarea del mismo.

Primeramente y tras comprobar el total de registros que presenta cada una de las diferentes etiquetas de la variable objetivo y ha llevado a cabo una división en función del tipo de cada una de las variables, dando como resultado una lista de variables numéricas y otra de variables categóricas. El análisis de las mismas se ha realizado de forma independiente ya que cada tipo requiere una tratamiento distinto.

Para las variables numéricas se ejecutado otra separación en función de la información que aporta cada una de las variables, produciendo así cuatro grupos distintos: geoposición, color, geometría y otras.

	* Geoposición:
	* Color:
	* Geometría:
	*Otras

## Construcción y justificación selección de los modelos

Como se ha comentado al inicio, se han desarrollado diversos modelos con el fin de buscar una diversidad entre los mismos y seleccionar aquel que ofrezca unos resultados más precisos y robustos. Además, cabe destacar que se ha realizado una técnica, la cual consiste en el **apilamiento de modelos**, es decir, primeramente se ha seleccionado y entrenado un modelo binario y posteriormente, con los resultados obtenidos se ha desarrollado un modelos multietiqueta. 

Para poder trabajar correctamente, en este apartado se han implementado una serie de _Pipelines_ y de _ColumTransform_ con el fin de poder obtener un conjunto de datos óptimo para el entrenamiento de los modelos necesarios. En estos _Pipelines_ se realiza una normalización de los valores de cada una de las variables, se realiza un **over-sampling** mediante la técnica de _SMOTE_ con el fin de balancear el conjunto de datos y por último, se realizan una serie de funciones previamente implementadas.

En el primero de ellos, el modelo binario, se ha buscado maximizar los hiperparámetros usando siempre el sentido común. Para conseguir esto se ha hecho uso de técnicas de validación cruzada y _GridSearchCV_. El resultado de este procedimiento fue la obtención de dos modelos robustos en función de la métrica establecida, _f1-score_, un **Random Forest** y un **XGBoost**.

En el segundo de ellos, el modelo multietiqueta, de igual forma se han buscado maximizar los hiperparámetros pero debido a la complejidad del problema y al elevado número de registros se ha optado por un **Random Forest** el cual ha mostrado siempre resultados más que decentes y robustos en todas las pruebas realizadas. Este modelo cuenta con todas las etiquetas de la variable a predecir y con los datos resultantes del primer modelo y el resto de ellos.

Un esquema muy sencillo de la estrategia utilizada es el mostrado a continuación:

![Esquema estrategia apilamiento de modelos](data/img/diagrama_modelos.png)

