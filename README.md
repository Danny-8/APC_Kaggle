# Practica Kaggle APC UAB 2021-22
### Nombre: Daniel Suárez
### DATASET: Brewer's Friend Beer Recipes
### URL: [kaggle](https://www.kaggle.com/jtrofe/beer-recipes)

## Resumen
El dataset que estudiaremos es sobre cervezas caseras de una web llamada Brewer's Friend: https://www.brewersfriend.com/
Podemos observar que las columnas del primer dataset recipeData.csv llamadas Style y StyleID coinciden con las de nuestro segundo dataset styleData.csv. Es por ello que este segundo dataset practicamente no lo utilizaremos.  

### Objetivo del dataset
Como objetivo principal queremos poder clasificar entre los 175 estilos de cerveza en la que cada cerveza casera está dividida

## Experimentos y Preprocesamiento
Viendo los resultados que obtenemos de los null he considerado directamente no utilizar esas variables que tienen una gran cantidad de falta de información, ja que tratarlos con la media, la moda u otros metodos solo generarían muchos datos no reales, que no es lo que estamos buscando a la hora de querer clasificar las diferentes cervezas.
Las únicas que vale la pena tratar son las de BoilGravity y Name, ya que tienen una cantidad reducida de nulls y nos podrían servir más adelante. Las trataremos con la media y la moda respectivamente.
Para el caso de Style, aun teniendo StyleID que nos facilita mucho a la hora de trabajar como si fuese el tarjet, creamos un nuevo estilo llamado 'Unknown' para evitar estos nulls en esta clase de tipo string por si quisieramos utilizarla más adelante.

Nos fijaremos ahora en la correlación que tiene todas las variables a estudiar. Recordar que nuestro objetivo es el StyleID para poder clasificar las diferentes bebidas.
Hay varias variables que no son de tipo float o int que no aparecen, así que vamos a tratarlas para poderlas utilizar y ver su correlación. Estas son; Name, URL, SugarScale y BrewMethod.
Podemos ver por los colores que en general todas las correlaciones han mejorado considerablemente entre ellas, aunque siguen siendo algo bajas sobretodo para nuestro atributo principal.
Esto me hace pensar que posiblemente el gran problema que tenemos es la cantidad de cervezas que hay.
Vemos que la cantidad no está muy bien distribuida. Para arreglar esto se me ocurren agrupar las clases en unas que sean más generales y probar ese target a ver su funciona mejor o peor que el que tenemos por defecto cuando estemos trabajando con el modelo.

Estudiando las cervezas vemos que podemos agrupar varios tipos que se repiten bastante, y los demás meterlos todos en 'otros'.
Primero creamos este nuevo atributo con variables de tipo char, i posteriormente las pasamos a int como hemos hecho anteriormente con los atributos que tenían las mismas características y venían por defecto.
Hemos reducido considerablemente la cantidad de clases, lo que las proporciones siguen siendo muy diferentes. Posiblemente tengamos que fijarnos en el f1 score, ya que los datos no están bien distribuidos, o utilizar SMOTE para generar más datos de cada tipo de clase hasta llegar al màximo del que tenemos, en este caso 'Other' o '7' (American IPA)
Podemos ver como sí que tiene más correlación este nuevo atributo, aun que sea gracias a que se reduce la classificación

### Modelo
| Modelo | Target | Hiperparametros | Métrica | Tiempo |
| -- | -- | -- | -- |
| Logistic regression | Style | C=10 | 5% | 100ms |
| Logistic regression | GenericStyle | C=1 | 27% | 100ms |
| -- | -- | -- | -- |
| Random Forest | Style | n_estimators=200, min_samples_leaf=1 | 15% | 100ms |
| Random Forest | GenericStyle | n_estimators=100, min_samples_leaf=1 | 45% | 100ms |
| -- | -- | -- | -- |
| Decision Tree | Style | criterion='gini' | 10% | 100ms |
| Decision Tree | GenericStyle | criterion='entropy' | 38% | 100ms |
| -- | -- | -- | -- |
| Ada Boost | Style | n_estimators=100 | 0.8% | 100ms |
| Ada Boost | GenericStyle | n_estimators=100 | 36% | 100ms |


## Conclusiones
El mejor modelo para nuestros dos targets ha estado el Random Forest.

Como nuestros modelos tienen que entrenar y clasificar de base una cantidad de 176 tipos diferentes, encima sin tener las variables bien distribuidas ya que hay de algunas más cantidad que de otras, obtenemos unos resultados muy bajos. 
Por ello, los modelos entrenados con el target GenericStyle que reduce las clases, aumenta casi el doble los resultados obtenidos, porque al clasificar menos por norma fallará menos veces.  
Viendo las matrices de confusión vemos que no destaca ninguna clasificación en concreto, ya que en la mayoría tienen un número medio elevado de fallos aunque acierte gran parte, entonces los resultados obtenidos los atribuiremos a la correlación de las variables, que como hemos visto, con nuestros target es practicamente mínima.

## Ideas para trabajar en el futuro
Crec que seria interesant indagar més en 