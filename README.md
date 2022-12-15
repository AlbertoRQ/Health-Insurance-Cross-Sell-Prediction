# Práctica Kaggle APC UAB 2022-23
### Nombre: Alberto Real Quereda 
### DATASET: Health Insurance Cross Sell Prediction
### URL: [Health Insurance Cross Sell Prediction](https://www.kaggle.com/datasets/anmolkumar/health-insurance-cross-sell-prediction)
## Resumen
Este caso Kaggle trata de una compañía de seguros que necesita unmodelo que les pueda predecir
si los titulares de pólizas del año pasado también estarán interesados en el seguro de vehículos
proporcionado por esta misma compañía.
Para poder predecir si el cliente estaría interesado en un seguro de vehículos, tenemos información
sobre diferentes datos de la persona, como: demográficos (género, edad, tipo de código de región),
vehículos (edad del vehículo, daños), póliza (prima, canal de abastecimiento), etc.

Tenemos 381109 muestras con 12 atributos. 6 de ellos son del tipo "int", 3 del tipo "str" y 3 tipo "float". 

### Objetivos del dataset
Los objetivos para este trabajo son:
- Aplicar diferentes clasificadores.
- Evaluar correctamente el error de los modelos.
- Visualizar los datos y el modelo resultante.
- Comparar los diferentes resultados.

## Experimentos
Para observar como se comporta nuestro dataset, primero hemos visto los histogramas de los atributos y la matriz de correlación para poder ver como las variables estaban relacionadas entre ellas y con nuestro atributo objetivo.

A lo largo del trabajo hemos visto un desbalanceo en el atributo objetvo, que hemos tratado de solucionar usando diferentes tecnicas de balanceo de los datos viendo cual de ellas daba un mejor resultado.

Todas estas pruebas relaizadas para el balanceo de los datos pueden verse en el notebook principal en el apartado "Estudio y selección de modelos"

### Preprocesamiento

Vemos que ninguno de los atributos siguen una distribución normal.
Cambiamos los atributos que son “string” a “int”. Con esto nos cambian los atributos:
- Gender: 1 si es mujer y 0 si es hombre.
- Vehicle_Age: 2 si “>2 Years”, 1 si “1-2 Year” y 0 si “<1 Year”.
- Vehicle_Damage (str): 1 si “Yes” y 0 si “No”.

Podemos eliminar algunas variables innecesarias como lo son:
- id: ID único para cada cliente y no aporta información.
- Driving_License: ya que un 99% de las veces tiene valor 1.

Y al hacer la matriz de correlación vemos que los atributos más relacionados con “Response”, son “Previously_Insured”, “Vehicle_
Age” y “Vehicle_Damage”.

Como el atributo "Response" no está balanceado, tenemos que usar algunos metodos de balanceo para balancearlo.


### Model
| Model | Hiperparametres | Accuracy | F1-score | 
| ----- | --------------- | -------- | -------- |
| Regresor Logístico (SMOTETomek)| C=2.0, fit_intercept=True, penalty='l2', tol=0.001 		     | 77% | 0.796 |
| Random Forest      (SMOTETomek)| max_depth=2, random_state=0                                       | 80% | 0.828 |
| Random Forest      (SMOTETomek)| n_estimators=100, max_depth=5, random_state=0,criterion="entropy" | 82% | 0.838 |

| Model | Hiperparametres | Accuracy | F1-score | 
| ----- | --------------- | -------- | -------- |
| Regresor Logístico (RandomUnderSampler)| C=2.0, fit_intercept=True, penalty='l2', tol=0.001 		     | 78% | 0.818 |
| Random Forest      (RandomUnderSampler)| max_depth=2, random_state=0                                       | 79% | 0.818 |
| Random Forest      (RandomUnderSampler)| n_estimators=100, max_depth=5, random_state=0,criterion="entropy" | 80% | 0.819 |

| Model | Hiperparametres | Accuracy | F1-score | 
| ----- | --------------- | -------- | -------- |
| Regresor Logístico (NearMiss)| C=2.0, fit_intercept=True, penalty='l2', tol=0.001 		   | 90% | 0.897 |
| Random Forest      (NearMiss)| max_depth=2, random_state=0                                       | 91% | 0.898 |
| Random Forest      (NearMiss)| n_estimators=100, max_depth=5, random_state=0,criterion="entropy" | 91% | 0.901 |

| Model | Hiperparametres | Accuracy | F1-score | 
| ----- | --------------- | -------- | -------- |
| Random Forest      (RandomUnderSampler)(Random Search)| max_depth= None, n_estimators=1090, bootstrap = True | 79% | 0.808 |
 

## Conclusiones
Hemos podido ver que el mejor modelo de clasificación para saber si un cliente va a querer el
seguro de coche o no es el Random Forest con el criterio “entropy”, aunque el Random Forest con
“gini” tampoco nos da resultados mucho peores y tarda considerablemente menos.
Como hemos visto en el apartado de hiperparámetros, para el Random Forest (gini), los mejores
hiparparametros son:

| n_estimators  | max_depth | Bootstrap |
| ------------- | --------- | --------- |
|      1090     |    None   |    True   |

Escogemos los hiperparámetros del “Random Search” debido a que estos clasifican bien un 79%
nuestros datos una vez habiendo aplicado el “RandomUnderSampler”, y también, estos hiperpará-
metros son los que mejor clasifican las muestras eliminadas después de aplicar undersampling siendo
la accuracy de estos un 64 % aproximadamente. Aunque los hiperparámetros del “Grid Search” dan
mejores resultados, estos tampoco son tan significantes para que compense el tiempo de ejecución
que conlleva buscarlos.

Resumiendo, para hacer una buena clasificación en nuestro dataset hemos utilizado, undersam-
pling con el algoritmo “RandomUnderSampler” con el fin de balancear el dataset y “Random Forest”
con el criterio “gini” y los hiperparámetros dados por el algoritmo “Random Search” con el fin de
clasificar de la mejor manera posible este dataset.


## Ideas para trabajar en un futuro
Se podría probar otros métodos que no requieran aplicar un undersampling para no perder muestras en el momento de entrenar nuestro clasificador. 
También se podrían hacer búsquedas más profundas en los hiperparametros y aplicar otros modelos para clasificar entre otras mejoras posibles.
