# Presentación del equipo
Buenos días, mi nombre es _______ y junto con _______ somos el grupo 2 de la mentoría sobre clasificación de tumoresferas dirigida por Lucía Benítez, de la diplomatura en ciencia de datos de la FaMAF, UNC.

# Introducción

## Cáncer
El cáncer es un conjunto de enfermedades en las que se encuentran células anormales que se multiplican sin control e invaden tejidos cercanos. Muchas de estas, involucran la formación de tumores sólidos o neoplasias, masas anormales de tejido que aparecen debido a dicha multiplicación.

El crecimiento de estos tumores depende de las interacciones dinámicas entre las propias células cancerosas, además de las interacciones con el microambiente. Estas células actúan en conjunto para dar lugar y controlar procesos como la proliferación, muerte y migración. 

## CMC
Un tipo particularmente problemático de células son las Células Madre Cancerosas (CMC). Estas se definen por su capacidad para renovar el crecimiento del tumor del cual son originarias y diferenciarse en distintos tipos de células especializadas (diferenciadas). Esto implica que aun suspendidas in vitro, aisladas de otras células, logran prosperar y reproducirse (a diferencia de una célula diferenciada, que moriría en dicha situación).

Se cree que estas células son las responsables del crecimiento del tumor a largo plazo, de las recaídas y de la metástasis. Por todo esto, el estudio de las CMC resulta de gran importancia.

## ¿Qué es una tumoresfera?
Debido a la complejidad de los tumores in vivo, ya que uno debe tener en cuenta los efectos del sistema inmune, el tejido sano, etc., como una primera aproximación para evaluar el rol de las CMC en el crecimiento se estudian modelos más simples donde uno considera pequeños tumores suspendidos in vitro, llamados tumoresferas. Estas son utilizadas en investigación como modelos intermedios entre cultivos in vitro de líneas celulares y tumores in vivo. Se forman por proliferación clonal a partir de una suspensión unicelular, derivadas de líneas celulares permanentes o tejido tumoral, y se cultivan en medios de baja adhesión con distintos factores de crecimiento. De los modelos esféricos de tumor, es específico para estudiar las propiedades de las CMC.

Explicar foto: el experimento biológico consiste en disociar el tejido tumoral, sembrar las células, las no madre mueren, y las madre forman esferas. (Fotos: blob & well)

Sin embargo, los ensayos de tumoresferas son experimentos complicados y suele suceder que algunas células se pegan entre sí, logrando formar agregados multicelulares que no son tumoresferas (no son monoclonales).

De los experimentos se obtienen fotos de los agregados generados (sean o no tumoresferas), de las cuales luego se extrae información estructurada mediante el programa Fiji. Este es un paquete de procesamiento de imágenes, y una distribución muy completa del programa ImageJ, que es ampliamente utilizado para procesar imágenes médicas. Ese es el origen de nuestro dataset, que fue obtenido en el Grupo de Materia Condensada (FaMAF-UNC, IFEG-CONICET) en colaboración con el Laboratorio de Células Madre del IBYME-CONICET a partir de imágenes tomadas usando un microscopio óptico invertido común. En el mismo, los agregados fueron etiquetados según fueran o no tumoresferas por un experto. Nuestro objetivo a la fecha era realizar una exploración de los datos estructurados, con vistas a desarrollar un modelo que reconozca las tumoresferas a partir de estos, sin la necesidad de un experto. Esto permitiría automatizar el proceso y obtener una mayor cantidad de datos que den lugar a futuros trabajos.

# Análisis Exploratorio de los Datos
El data set contenía una amplia variedad de variables que caracterizan la forma y el tamaño del agregado, o que ayudan a identificar la correspondencia entre el registro y la imagen experimental. De estas, destacamos:
- el área ocupada por el esferoide en la foto, que es una medida de su tamaño;
- los días que transcurrieron desde el inicio del cultivo hasta que se tomó la foto;
- `redondez` y `circularidad`, dos medidas de cuán esférico es el agregado, i.e., de su forma.

(Idea: un pair plot y matriz de correlación por día, discriminando según sean tumoresferas.)

(Gráfico distribuciones del área por día.) De los análisis realizados, lo primero a destacar fue el análisis de las distribuciones de área para  los distintos días, discriminando según sean o no tumoresferas. Si nos enfocamos en la media, la desviación estándar, el sesgo y la curtosis de estas distribuciones en función del tiempo, podemos notar que:
- Los datos del día 6 son erróneos, ya que son dos pares de datos idénticos, donde uno se clasificó como tumoresfera y el otro no. Esto puede verse notando que las distribuciones del día 6 son gaussianas idénticas. Claramente se trata de un error en la carga de los datos, se han mantenido por completitud (ya que son los únicos datos del día 6), pero no deberían ser tenidos en cuenta (drop).

(Gráfico media, la desviación estándar, sesgo y curtosis vs día.)
- Por otro lado, el área para los esferoides comienza siendo menor, pero eventualmente supera a la del resto.
- Si bien ambos grupos tienen sesgo positivo (las distribuciones son asimétricas, con más área a la derecha de la media), los esferoides tienen un sesgo menor al resto, que parece disminuir con el tiempo.
- En general, la curtosis de las distribuciones de área para los esferoides es menor a la del resto y cae con el tiempo, indicando que las distribuciones de áreas son más “achatadas” para los esferoides que para el resto.

(Idea: sacar el día 6 de los gráficos (e.g., del área media).)

(Gráfico distribuciones de la circularidad por día.) Lo mismo se puede hacer con la circularidad, obteniéndose que:
- la circularidad media es significativamente mayor para las tumoresferas sólo en los primeros días, luego ...;
- la redondez media por su parte, se mantiene consistentemente mayor para las tumoresferas que para el resto de los agregados.

(Idea: hacer el gráfico de distribuciones de pc1 por día, en lugar de circularidad y redondez.)

En conclusión, el análisis del dataset nos sugiere que las variables más prometedoras para la clasificación de los agregados según sean tumoresferas, son el área y la redondez, además de permitirnos identificar datos erróneos en el día 6. Esto junto al resto del análisis realizado nos da una base firme para afrontar la futura tarea de clasificación.