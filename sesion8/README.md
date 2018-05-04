Asignatura Cloud Computing del Máster en Ingeniería Informática. 

Departamento de Ciencias de la Computación e Inteligencia Artificial.

Universidad de Granada.

<HR>

Profesor: **Manuel J. Parra-Royón**

Email: **manuelparra@decsai.ugr.es**

Tutorías: **Viernes, de 17:30 a 18:30, despacho D31 (4ª planta) Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT).**

Material de prácticas de la asignatura: **https://github.com/manuparra/PracticasCC**

<HR>

# Sesión: Apache Mahout - Hadoop

Apache Mahout es una biblioteca de algoritmos para el aprendizaje máquina escalable en Hadoop.

Apache Mahout es una librería de algoritmos escalables de aprendizaje automático, implementados sobre Apache Hadoop y usando el paradigma MapReduce. El aprendizaje  máquina es una disciplina de la inteligencia artificial centrada en permitir que las máquinas aprendan sin estar programadas explícitamente, y se utiliza comúnmente para mejorar el rendimiento futuro basado en resultados anteriores.

Una vez que los datos grandes se almacenan en el Sistema de Archivo Distribuido de Hadoop (Hadoop Distributed File System, HDFS), Mahout proporciona las herramientas de ciencia de datos para encontrar automáticamente patrones significativos en esos grandes conjuntos de datos. El proyecto Apache Mahout tiene como objetivo hacer más rápido y fácil convertir datos grandes en información grande.

## Qué hace Mahout

- Mahout soporta cuatro casos principales de uso de la ciencia de datos:

- Filtrado colaborativo: extrae el comportamiento de los usuarios y hace recomendaciones de productos (por ejemplo, recomendaciones de Amazon).

- Agrupación - toma elementos de una clase en particular (como páginas web o artículos de periódicos) y los organiza en grupos naturales, de manera que los elementos que pertenecen al mismo grupo o son similares entre sí.

- Clasificación: aprende de las categorizaciones existentes y, a continuación, asigna los elementos no clasificados a la mejor categoría.

- Minería frecuente de conjuntos de artículos: analiza los artículos de un grupo (por ejemplo, los artículos de un carro de la compra o los términos de una sesión de consulta) y, a continuación, identifica los artículos que suelen aparecer junto

Mahout proporciona una implementación de varios algoritmos de aprendizaje de máquina, algunos en modo local y otros en modo distribuido (para uso con Hadoop). Cada algoritmo en la biblioteca de Mahout puede ser invocado usando la línea de comandos de Mahout o bien desde JAVA.

La siguiente es una lista de algoritmos para uso en modo distribuido (compatibles con Hadoop), clasificados por las cuatro categorías: filtrado colaborativo, agrupación, clasificación o minería frecuente de conjuntos de artículos. Mahout también incluye algunos algoritmos de aprendizaje de máquina que pueden utilizarse localmente. Para obtener una lista de algoritmos de quejas, visite http://mahout.apache.org/users/basics/algorithms.html.

- Distributed Item-based Collaborative Filtering	Collaborative Filtering	Estimates a user’s preference for one item by looking at his/her preferences for similar items
- Collaborative Filtering Using a Parallel Matrix Factorization	Collaborative Filtering	Among a matrix of items that a user has not yet seen, predict which items the user might prefer
- Canopy Clustering	Clustering	For preprocessing data before using a K-means or Hierarchical clustering algorithm
- Dirichlet Process Clustering	Clustering	Performs Bayesian mixture modeling
- Fuzzy K-Means	Clustering	Discovers soft clusters where a particular point can belong to more than one cluster
- Hierarchical Clustering	Clustering	Builds a hierarchy of clusters using either an agglomerative “bottom up” or divisive “top down” approach
- K-Means Clustering	Clustering	Aims to partition n observations into k clusters in which each observation belongs to the cluster with the nearest mean
- Latent Dirichlet Allocation	Clustering	Automatically and jointly cluster words into “topics” and documents into mixtures of topics
- Mean Shift Clustering	Clustering	For finding modes or clusters in 2-dimensional space, where the number of clusters is unknown
- Minhash Clustering	Clustering	For quickly estimating similarity between two data sets
- Spectral Clustering	Clustering	Cluster points using eigenvectors of matrices derived from the data
- Bayesian	Classification	Used to classify objects into binary categories
- Random Forests	Classification	An ensemble learning method for classification (and regression) that operate by constructing a multitude of decision trees
- Parallel FP Growth Algorithm	Frequent Itemset Mining	Analyzes items in a group and then identifies which items typically appear together


# Random Forest

Random Forest (RF) es un ensemble de árboles dedecisión. La clase predicha se calcula mediante la agregación delas predicciones del conjunto a través de la votación por mayoría.


## Random Forest usando MapReduce - Mahout

La implementación Partial de la biblioteca Mahout(RF-BigData) es un algoritmo que construye múltiples árboles para diferentes porciones de los datos
Este algoritmo consta de dos fases diferentes:

- 1 Fase de Construcción del Modelo
- 2 Fase de Clasificación

De forma adicional, cada una de las fases constan de tres etapas: 1 Inicial, 2 Map, 3 Final

## Marco Experimental

- 3 casos de estudio derivados del conjunto de datos KDDCup 1999
- Conjunto de datos disponible en UCI Machine Learning Repository
- Esquema de validación cruzada en 5 particiones (usaremos la primera partición)
- Caso de estudio: kddcup_10_normal_vs_DOS


### Descriptor del fichero

Generar el fichero que describe al conjunto de datos:

```
hadoop jar /home/<user>/mahout-distribution-0.9.jar org.apache.mahout.classifier.df.tools.Describe -p /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.arff -f /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.info -d N 3 C 37 N L
```

de donde:

- p es la ruta a conunto de datos
- f el la ruta al fichero de salida del descriptor
- d Indica la disposición de las columnas y sus tipos (por ejemplo -d N 3 C 37 N L): 1 N(umerico),3 C(ategoricos), 37 N(uméricos), 1 L(abel)


### Ejecutar Random Forest

Ejecutar la aplicación (5 maps y 100 árboles):

```
hadoop jar /home/<user>/mahout-distribution-0.9.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapreduce.input.fileinputformat.split.minsize=11886574 -Dmapreduce.input.fileinputformat.split.maxsize=11886574 -o output_kddcup_10_normal_versus_DOS
-d /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.arff
-ds /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.info
-sl 6 -p -t 100
```

de donde:

- minsize es el tamaño mínimo de la particion
- d el dataset de entrada
- ds el descriptor del dataset
- sl es el numero de atributos aleatorios que se seleccionan
- t numero de árboles


Usar el modelo generado en el paso anterior para clasificar nuevos datos:

```
hadoop jar /home/<user>/mahout-distribution-0.9.jar org.apache.mahout.classifier.df.mapreduce.TestForest
-i /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tst.arff
-ds /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.info
-m output_kddcup_10_normal_versus_DOS
-a -mr
-o predictions_kddcup_10_normal_versus_DOS
```

de donde:

- i indica el dataset con los datos de test
- a informa de la matriz de confusion
- mr infroma de distribución de clasificacion
- o salida de las predicciones

Comprobar la salida: 

```
hadoop fs -cat predictions_kddcup_10_normal_versus_DOS | head -n 10
```

## Random Over Sampling

Es un método no heurístico que intenta ajustar la distribución de clases a través de la replicación aleatoria de ejemplos en la clase minoritaria.

Permite obtener un conjunto de datos con una distribución balanceada de clases mediante la replicación aleatoria de ejemplos de la clase minoritaria.

Este algoritmo consta de dos fases diferentes:

- 1 Fase Map: cada proceso map se encarga de balacear la distribución de clases de su partición mediante la replicación de instancias de la clase minoritaria
- 2 Fase Reduce: se combinan cada una de las salidas de los maps para formar el conjunto de datos final ya balanceado

### Descriptor del fichero

```
hadoop jar mahout-distribution-sige.jar org.apache.mahout.classifier.df.tools.Describe
-p /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.arff
-f /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.info
-d N 3 C 37 N L
```

### Calculo del split

Calcular el tamaño del split (por ejemplo, 5 maps) [shell script]

```
FILE_SIZE=( ´hadoop fs -ls /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.arff | awk ’{print $5}’‘)
BYTES_BY_PARTITION=$((FILE_SIZE/5)) MAX_BYTES_BY_PARTITION=$((BYTES_BY_PARTITION+1))
```

### Calcular el número de instancias de la clase minoritaria (NPositiva)

```
NPOS=( ‘hadoop fs -cat /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.arff | grep ’,positive$’ | wc -l´)
```

### Calcular el número de instancias de la clase minoritaria (NNegativa)

```
NNEG=( ‘hadoop fs -cat /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.arff | grep ’,negative$’ | wc -l‘)
```

### Ejecutar Random Oversampling (por ejemplo, 5 maps):

```
hadoop jar mahout-distribution-sige.jar org.apache.mahout.classifier.df.mapreduce.Resampling -Dmapreduce.input.fileinputformat.split.minsize=$BYTES_BY_PARTITION -Dmapreduce.input.fileinputformat.split.maxsize= $MAX_BYTES_BY_PARTITION -dp /tmp/kddcup/kddcup_10_normal_versus_DOS-5- 1tra.arff -d output-ROS-kddcup_10_normal_versus_DOS -ds datasets/kddcup kddcup_10_normal_versus_DOS-5- 1tra.info -rs overs -p 5 -npos $NPOS -nneg $NNEG -negclass negative
```

### Verificar salida

Comprobar la salida. El fichero de salida tendrá el nombre "part-r-00000".

```
hadoop fs -ls output-ROS-kddcup_10_normal_versus_DOS
```


## Script (copiable/pegable) para ROS

```
Ejemplo de uso: Implementación MapReduce para Random Oversampling

#1. Generar el fichero que describe al conjunto de datos

hadoop jar mahout-distribution-sige.jar org.apache.mahout.classifier.df.tools.Describe -p /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.arff  -f /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.info -d N 3 C 37 N L

#2. Calcular el tamaño del split (por ejemplo, 5 maps)

FILE_SIZE=( `hadoop fs -ls /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.arff | awk '{print $5}'`)

BYTES_BY_PARTITION=$((FILE_SIZE/5))

MAX_BYTES_BY_PARTITION=$((BYTES_BY_PARTITION+1))

#3. Calcular el número de instancias de la clase minoritaria o menos representativa. En este ejemplo, "positive" es el nombre la clase asociada a la clase minoritaria

NPOS=( `hadoop fs -cat /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.arff | grep ',positive$' | wc -l`)

#3. Calcular el número de instancias de la clase mayoritaria. En este ejemplo, "negative" es el nombre la clase asociada a la clase mayoritaria

NNEG=( `hadoop fs -cat /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.arff | grep ',negative$' | wc -l`)

#4. Ejecutar Random Oversampling (por ejemplo, 5 maps)

hadoop jar mahout-distribution-sige.jar org.apache.mahout.classifier.df.mapreduce.Resampling -Dmapreduce.input.fileinputformat.split.minsize=$BYTES_BY_PARTITION -Dmapreduce.input.fileinputformat.split.maxsize=$MAX_BYTES_BY_PARTITION -dp /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.arff -d output-ROS-kddcup_10_normal_versus_DOS -ds /tmp/kddcup/kddcup_10_normal_versus_DOS-5-1tra.info -rs overs -p 5 -npos $NPOS -nneg $NNEG -negclass negative -tm ROS-kddcup_10_normal_versus_DOS-build_time

#5. Comprobar la salida. El fichero de salida tendrá el nombre "part-r-00000"

hadoop fs -ls output-ROS-kddcup_10_normal_versus_DOS



```


# Referencias:

- Apache Mahout: http://mahout.apache.org/
- Random Forest MapReduce implementation in Mahout: http://mahout.apache.org/users/classification/partial- implementation.html
- UCI Machine Learning Repository - KDD Cup dataset: https://archive.ics.uci.edu/ml/datasets/KDD+Cup+1999+Data
- S. Río, V. López, J.M. Benítez, F. Herrera. On the use of MapReduce for Imbalanced Big Data using Random Forest. Information Sciences 285 (2014) 112-137. doi: 10.1016/j.ins.2014.03.043 http://sci2s.ugr.es/sites/default/files/ficherosPublicaciones/1742_2014-delRio-INS.pdf



