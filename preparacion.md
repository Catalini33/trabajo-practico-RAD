---
title: "Trabajo 1: Quarto reporte descriptivo; Trabajo 2: Correlaciones y construcción de escala"
author: "Catalina Rojas y Matias villanueva"
output: html_document
---


Trabajo 1: Reporte descriptivo de Quarto

Durante los últimos años se ha puesto en debate la legitimidad del estado de Chile y ha sido una relevante importancia en la población, notada en las demandas sociales existentes, las cuales giran en torno a una mayor equidad frente a derechos básicos como la salud, la educación, seguridad y pensiones. Bajo esta premisa, nace la duda de cómo la ciudadanía capta el estado; cómo actúa frente a estas materias sociales (y económicas), si actúa acorde a las necesidades de la ciudadanía o no según la percepción popular presentado en la base de datos del SOC. “La formación de los miembros de una comunidad humana en una conciencia viva de pertenencia a la misma, en todo un conjunto de habilidades y actitudes para participar receptiva y activamente en su dinámica, así como en un compromiso profundo por mejorarla, desde una sana visión crítica hasta una auténtica implicación personal” (Jordán, J., 1995). Jordán, J. A. (1995). Concepto y objeto de la educación cívica. Pedagogía social: revista interuniversitaria. https://digitum.um.es/digitum/bitstream/10201/39623/1/Concepto%20y%20objeto%20de%20la%20educaci%C3%B3n%20c%C3%ADvica.pdf Para hablar de esto es necesario detallar en torno a que limites observamos el rol del estado, las cuales estan basadas en la idea de la educacion civica, entendiendola como un amplio conjunto de conocimientos, actitudes y valores que nos permiten comprender y participar en torno a una vida democratica, se manifiesta en creencias cotidianas como la confianza institucional, justicia social y el trato entre clases. Analizar este fenomeno como lo es la legitimacion del rol del estado en Chile debemos poder definir brevemente cual es el rol de este y que definimos como “El Estado se presenta como el garante del interés general, pero su legitimidad se construye en parte por la aceptación simbólica de un orden social que no siempre es justo” (Bourdieu, 2014 P. 45) Visto desde esta manera podemos avanzar en materia de como tratamos de abordar nuestra investigacion, en donde nos aproximamos a como las personas interiorizan principios civicos en su vida cotidiana.

El siguiente trabajo propone identificar a traves de una encuesta realizada sobre el año 2022, la existencia de diferencias entre las clases sociales y de que manera esta se refleja en la clase media en base al trato, esto relacionando hacia la percepcion de justicia por parte institucional en torno a la distribucion de recursos, algunos de los factores mas relevantes que se plasman frente a este problema son visibles en las clases sociales, educacion y salud. Pilares fundamentales para la creencia de que el rol del estado se cumple de manera protectora, por el contrario si estas preocupaciones se tornan de manera negativas, el vinculo entre cuidadania y estado se ve fuertemenet afectado tanto en la participacion como en la desconfianza institucional, sin ir mas lejos “La legitimidad es el reconocimiento por parte de la población de que los gobernantes de su Estado son los verdaderos titulares del poder y los que tienen derecho a ejercerlo: a crear y aplicar normas jurídicas, disponiendo del monopolio de la fuerza, de acuerdo con esas normas, sobre la población.” (Peña,C. , 2013) entendiendo a su vez que para que exista una real percepccion de cohesion social debe tener una legitimidad sobre la poblacion

Lo que proponemos estudiar este trabajo surge especificamente en torno a la justicia distribuitiva y de como esta se justifica en base a las distintas clases sociales y de que manera se ve influenciado el trato hacia la clase media, y de que manera entendemos si la educacion y salud se distribuye de manera equitativa, por lo que planteamos un analisis previo hacia: Mientras mayor sea la percepcion de justicia distributiva y trato equitativo en las clases sociales, mayor sera la creencia que el estado actua en torno al bienestar colectivo

Para este analisis abordaremos variables desde grados de acuerdo en torno a: El estado trabaja por el bienestar de las personas, justicia distribuitiva (tanto en educacion como en salud) ademas de utilizar la variable "justificacion de diferencias entre clases" entendiendo que existen distintas clases sociales y estigmas para asi enfocarnos mas a fondo en el trato justo de personas de clase media, cada variable sera fundamental para asi aportar de manera complementaria todos los datos puestos en escena y desarrollar nuestra hipotesis

Running Code

Descarga de paquetes a utilizar para nuestro trabajo

{r}
pacman::p_load(sjlabelled,
               dplyr,                                                           
              stargazer, 
              sjmisc, 
              summarytools, 
              kableExtra, 
              sjPlot,
              corrplot, 
              sessioninfo, 
              ggplot2,
              knitr) 

Descarga de base de datos a utilizar

La base de datos que utilizaremos corresponde a la "Encuesta Longuitunidal Social de Chile" que fue realizada por el COES

{r}
load(url("https://dataverse.harvard.edu/api/access/datafile/7245118"))

{r}
proc_data_2022 <- elsoc_long_2016_2022.2 %>% filter(ola=="6") %>%           select(Grad_acu_est_bien=c40_01,                                                   Grad_acu_just_edu=d02_02,                                                    Grad_acu_just_sal=d02_03,                                                   Just_dif_clas_soc=d19,                                                       Trat_just_clase_med=d25_02)                                    

Se eliminan datos perdidos correspondientes a NS/NR (No sabe/No responde) y estan numeradas como -999 -888 y -777

{r}
proc_data_2022 <- proc_data_2022 %>% sjlabelled::set_na(., na = c(-999, -888, -777, -666))

Procedemos a guardar nuestra sub-base de datos obtenida en la carpeta input

{r}
save(proc_data_2022, file = "C:\ Users\ enigm\ Documents\ UAH\ trabajo-practico-RAD\ input\ proc_data_2022.RData")

Se le asignan etiquetas a las variables

{r}
proc_data_2022$Grad_acu_est_bien <- set_label(
  x = proc_data_2022$Grad_acu_est_bien,
  label = "Grado de acuerdo: Estado trabaja por bienestar de personas"
)

proc_data_2022$Grad_acu_just_edu <- set_label(
  x = proc_data_2022$Grad_acu_just_edu,
  label = "Grado de acuerdo: Justicia distributiva en educación"
)

proc_data_2022$Grad_acu_just_sal <- set_label(
  x = proc_data_2022$Grad_acu_just_sal,
  label = "Grado de acuerdo: Justicia distributiva en salud"
)

proc_data_2022$Just_dif_clas_soc <- set_label(
  x = proc_data_2022$Just_dif_clas_soc,
  label = "Justificación de diferencias entre clases"
)

proc_data_2022$Trat_just_clase_med <- set_label(
  x = proc_data_2022$Trat_just_clase_med,
  label = "Trato justo: personas de clase media"
)

get_label(proc_data_2022$Grad_acu_est_bien)
get_label(proc_data_2022$Grad_acu_just_edu)
get_label(proc_data_2022$Grad_acu_just_sal)
get_label(proc_data_2022$Just_dif_clas_soc)
get_label(proc_data_2022$Trat_just_clase_med)


Analisis de la tabla descriptivo de las medidas de tendencia central de las variables

Esta tabla cuenta con los siguientes estadisticos:

Descriptivos generales: La pregunta realizada (label), Numero de N’s (n) y la proporción de NA’s (NA.proc)

Medidas de tendencia central tales como la: Media (mean) y el Rango (range) el cual este ultimo aportaria con los valores minimos y maximos

Medidas de disperción como: la desviación estandar (sd)

{r}
sjmisc::descr(proc_data_2022,                                                             
              show = c("label", "range","mean","sd","NA.prc", "n")) %>%
  kable(.,"markdown") %>%
  kable_styling(full_width = T) %>%
  scroll_box(width = "125%")


Graficos de relación entre variable dependiente y algunas variables independientes

{r,fig.cap:"Plots"}
plot_grpfrq(proc_data_2022$Grad_acu_est_bien , proc_data_2022$Grad_acu_just_edu)

{r, fig.cap:"Plots"}
plot_grpfrq(proc_data_2022$Grad_acu_est_bien , proc_data_2022$Just_dif_clas_soc)

{r, fig.cap: "Plots"}
plot_grpfrq(proc_data_2022$Grad_acu_est_bien , proc_data_2022$Grad_acu_just_sal)

{r, fig.cap: "Plots"}
plot_grpfrq(proc_data_2022$Grad_acu_est_bien,proc_data_2022$Trat_just_clase_med)

A traves de los datos expuestos podemos interpretar lo siguiente:

Podemos entender desde una vision un tanto critica en torno al rol del estado de bienestar por parte de las personas, esto lo podemos ver en que la media no sube desde la puntuacion 3 (valor medido en escala likert). Otro factor relevante lo vemos en el alto porcentaje de datos faltantes, lo cual puede indicar que esta pregunta en si se puede interpretar como una pregunta sensible o que genera desconfianza, lo cual el sociologo nos da a entender su importancia en "sociologia del estado(Bordieu 2014)" ya que nos muestra una gran distancia simbolica frente al aparato estatal

Grad_acu_just_edu y Grad_acu_just_sal (Justicia distributiva en educación y salud)

Medias: 2.10 (educación) y 2.05 (salud)

Desviaciones estándar: 0.91 y 0.90

NA%: muy bajo (<0.3%)

Ambas variables pueden ser interpretadas de manera sustancial y critica frente a la distrivucion de recursos en los sistemas de educacion y salud, esto lo podemos observar en la baja media la cual nos refleja un desacuerdo frente al hecho de que estos sistemas institucionales operen bajo los principios de justicia social. Podemos entender sobre esto mismo que la idea previa frente a las expectativas cuidadanas y el cumplimiento estatal nos presenta una idea previa sobre la brecha sustancial que se debate sobre la legitimidad y justicia distribuitiva, ya que estos derechos son fundamentales para cada individuo y no debiera existir tal discrepancia sobre esta percepcion.

Just_dif_clas_soc (Justificación de diferencias entre clases)

Media: 1.88

Desviación estándar: 0.87

NA%: 63.5%

Podemos observar que se presenta un rechazo generalizada hacia la justificacion de desigualdades sociales, donde una media cercana al minimo posible nos refleja una vision critica y coherente frente a los valores igualitarios

Trat_just_clase_med (Trato justo hacia personas de clase media)

Media: 5.05 (en escala de 1 a 10)

Desviación estándar: 1.86

NA%: 63.4%

La media es neutral o levemente positiva lo que sugiere que la clase media es vista como tratada de manera “más o menos justa”.

hay quienes consideran que reciben un trato justo y quienes lo ven injusto.El alto porcentaje de NA es llamativo: puede deberse a que las personas no se sienten identificadas con la clase media, o no saben cómo posicionarla frente a otras.

El análisis descriptivo muestra un clima de desconfianza cuidadana hacia la equidad estatal, especialmente en áreas clave como educación y salud. Al mismo tiempo, se observa rechazo hacia las desigualdades de clases, lo que refuerza la idea de que las demandas de justicia social están presentes, aunque no necesariamente canalizadas politicamente

Esta lectura puede vincularse con teorías de legitimidad (Weber) y habitus político (Bourdieu), mostrando cómo las representaciones subjetivas del Estado están mediadas por percepciones de justicia y trato justo, que forman parte esencial de la educacion civica critica.



Graficos descriptivos de las variables

{r, fig.cap:"Plots"}
plot_frq(proc_data_2022$Grad_acu_est_bien,
         title = "Acuerdo: El Estado se preocupa del bienestar",
         show.na = TRUE)

plot_frq(proc_data_2022$Grad_acu_just_edu,
         title = "Acuerdo: La educación en Chile es justa",
         show.na = TRUE)

plot_frq(proc_data_2022$Grad_acu_just_sal,
         title = "Acuerdo: La salud en Chile es justa",
         show.na = TRUE)

plot_frq(proc_data_2022$Just_dif_clas_soc,
         title = "Justificación de las diferencias entre clases sociales",
         show.na = TRUE)

plot_frq(proc_data_2022$Trat_just_clase_med,
         title = "Percepción de trato justo hacia la clase media",
         show.na = TRUE)




{r}


A partir de los siguientes datos arrojados, hemos podido observar que se percibe una gran representacion frente a servicios publicos de los cuales no se sienten satisfechos o que no cumple con su grado de "justicia" hacia la persona, es por esto que podemos inferir de manera explicita a traves de los graficos que la gran parte de la poblacion o encuentra insuficiente o poco efectivo los servicios publicos basicos como lo son la salud y educacion, servicios de los cuales para un pais debiesen ser prioridad ya que son exclusivamente para la poblacion, en torno a la otra muestra sobre las clases sociales podemos observar que a pesar que hay una gran cantidad (la mayoria en este caso) de las cuales no responde, mientras que las personas que si respondieron observamos una posicion mucho mas neutra que nos indica que las respuestas obtenidas no solo tratan de representar hacia alguna clase social en especifico sino mas bien un problema estructural frente a la posicion de que los servicios basicos se presentan como injusta o de poca efectividad hacia la percepcion de la poblacion chilena.

Trabajo 2:

Asociación de variables en base a la construcción de una tabla de correlaciones y construcción de escala de "Percepcion de justicia social".

1. Introducción

Esta parte del trabajo (Trabajo 2)que corresponde a la continuación de nuestra primer entrega ( Trabajo 1) , tenemos como objetivo la obtencion de la tabla de correlaciones entre las variables ya mencionadas anteriormente (Trabajo 1 ), con el proposito de analizar el grado de asociación entre estas con el fin de de evaluar si responder a algun patron en común. Gracias a dicho analisis, procederemos a construir una escala que permitira medir el concepto de "percepcion de justicia social" que como lo define Castillo, S. & Torres, M. (2011)"La percepción de justicia social implica la evaluación subjetiva que hacen las personas sobre la equidad en la distribucion de bienes, servicios y oportunidades en una sociedad, asi como la legitimidad de los mecanismos que la generan."A partir de la seleccion de este concepto central que se estara trabajando en esta sección, nuestra hipotesis es: Se espera que a medida que disminuya la percepcion de justicia social, aumente la disposicion de las personas a cuestionar o no estar de acuerdo con el orden social establecido.

2. Preparación del trabajo

2.1. Instalación de paquetes:

A diferencia del Trabajo 1, añadiremos los paquetes GGally y corrplot, estos paquetes nos sirven para elaborar correlaciones y para graficarlas. Los demás paquetes son los utilizados anteriormente en el Trabajo 1

{r}
pacman::p_load(sjlabelled,
              dplyr,                                                              
              stargazer, 
              sjmisc, 
              summarytools, 
              kableExtra, 
              sjPlot,
              corrplot, 
              sessioninfo, 
              ggplot2,
              knitr,
              GGally,                                                           
              corrplot,                                                                          ggcorrplot)

2.2. Descarga de la base de datos

La base de datos a utilizar es "Encuesta Longitudinal Social de Chile" la cual fue realizada por COES (Centro de Estudios de Conflicto y Cohesión social).

El link de la base de datos proveniente de la web:

{r}
load(url("https://dataverse.harvard.edu/api/access/datafile/7245118"))

2.3. Selección de variables

Añadiremos una sub-base a partir de la base utilizada en el Trabajo 2: (nombre) y selecccionaremos las variables a utilizar en este trabajo (2), las cuales son las mismas que el trabajo 1, puesto que estas variables engloban nuestro concepto central a medir.

{r}
proc_data_2022tr_dos <- elsoc_long_2016_2022.2 %>% filter(ola=="6") %>%           select(Grad_acu_est_bien=c40_01,                                                   Grad_acu_just_edu=d02_02,                                                    Grad_acu_just_sal=d02_03,                                                   Just_dif_clas_soc=d19,                                                       Trat_just_clase_med=d25_02)


3.-Casos perdidos

Eliminamos casos perdidos como (NS/NR) que corresponden a los valores -88 y -999 marcados como NA de las variables seleccionadas

{r}
proc_data_2022 <- proc_data_2022 %>% sjlabelled::set_na(., na = c(-999, -888, -777, -666))

4. Recodificación de valores a las variables a utilizar

Recodificaremos los valores de las categorias de respuestas de nuestras variables, puesto que las categorias de respuestas van de 1 a 5, lo recodificaremos de 0 a 4 para mayor facilidad de interpretacion en la escala que realizaremos.

{r}
proc_data_2022tr_dos$Grad_acu_est_bien <- car::recode(proc_data_2022$Grad_acu_est_bien,"1=0; 2=1; 3=2; 4=3; 5=4; else=NA")               proc_data_2022tr_dos$Grad_acu_just_edu<- car::recode(proc_data_2022tr_dos$Grad_acu_just_edu,"1=0; 2=1; 3=2; 4=3; 5=4; else=NA")         proc_data_2022tr_dos$Grad_acu_just_sal <- car::recode(proc_data_2022tr_dos$Grad_acu_just_sal,"1=0; 2=1; 3=2; 4=3; 5=4; else=NA")       proc_data_2022tr_dos$Just_dif_clas_soc<- car::recode(proc_data_2022tr_dos$Just_dif_clas_soc, "1=0; 2=1; 3=2; 4=3; 5=4; else=NA")        proc_data_2022tr_dos$Trat_just_clase_med<- car::recode(proc_data_2022tr_dos$Trat_just_clase_med, "1=0; 2=1; 3=2; 4=3; 5=4; else=NA")

5.-Guardar en la base de datos

{r}
save(proc_data_2022tr_dos, file = "C:\ Users\ enigm\ Desktop\ UAH\ trabajo-practico-RAD\ input\ proc_data_2022.RData")

6.-Tabla de correlaciones

{r}
Correlacion_2022 <- cor(proc_data_2022tr_dos, use = "complete.obs")
            

{r}
sjPlot::tab_corr(proc_data_2022tr_dos, triangle = "lower")

{r}
corrplot.mixed(Correlacion_2022)

                                                                    



7.-Elaboración de la escala de "percepción de justicia social" y su interpretación

#Primeramente mediremos si las variables tienen consistencia entre si, esto lo realizaremos midiendo la consistncia interna de cada variable

#Utilizaremos este codigo

{r}
psych::alpha(Correlacion_2022)





Como resultado de esta tabla podemos observar que tenemos un valor de 0.54 de Alfa de Cronbach (raw_alpha), lo cual indica una consistencia interna baja, puesto que lo minimo es 0.60 para considerar que las variables estan estrechamente relacionadas para la construccion de una buena escala, tambien se puede observar que las correlaciones promedio entre los items se mantuvieron bajas, como se puede observar entre 0.09 y 0.22 y por ultimo ninguna de las variable si es eliminada contribuye siginificativamente a aumentar el valor del alfa cronbach.Queremos hacer enfasis en que elegimos estas variables para este analisis, ya que podrian estar relacionadas con la percepcion de justicia social, puesto que este concepto aborda aspectos como bienestar estatal, justicia distributiva en educacion y salud, justificacion de diferencia entre clases y trato justo. Sin embargo, los resultados del analisis de consistencia interna indica que no presentan una correlacion suficiente para una escala solida y homogenea. Lo cual puede deberse a que a pesar de estar conectadas a un mismo constructo, em la practica las respuestan reflejan distintas dimensiones del concepto de "justicia social" alterando la percepcion de esta impidiendo una coherencia estadisticaA continuacion examinaremos mejor estas dimensiones. 

8.-Confeccion de las dimensiones del indice

A pesar que la consistencia interna de nuestras variables no es suficiente para realizar una escala homogenea y solida ,como mencionamos anteriormente, decidimos avanzar con la construccion de la misma debido al interes de explorar una medida compuesta que refleje la percepcion de justicia social en sus distintas dimensiones, para asi identificar patrones de asociacion si es que los hay, reconociendo asi tambien limitaciones estadisticas del indice.

Las dimensiones a utilizar son las siguientes (dimensiones vinculadas a cada variable)1.-Justicia y bienestar 2.-Conflicto de clase 3.- Percepcion de trato justo y 4.-Cuidadania

{r}                                                                               
proc_data_2022tr_dos=proc_data_2022tr_dos %>%                                       rowwise() %>%                                                                      mutate(JusticiayBienestar = mean(c(Grad_acu_just_edu,Grad_acu_just_sal)),           Conflictoclase = mean(c(Just_dif_clas_soc)),                                       Perctrjust = mean(c(Trat_just_clase_med)),                                        Cuidadania = mean(c(Grad_acu_est_bien))) %>%                                       ungroup()

Asignacion de peso a las dimensiones 

{r}
proc_data_2022tr_dos = proc_data_2022tr_dos %>%                                     rowwise() %>%                                                                      mutate(Percepcion_just_soc = (JusticiayBienestar*25)+(Conflictoclase*25)+(Perctrjust*25)+(Cuidadania*25)) %>%                                              ungroup()


Ahora procedemos a hacer los descriptivos de las dimensiones y del indice de "Percepcion de justicia social"

Descriptivos de las dimensiones y indice de "Percepción de justicia social"

{r}
summary(proc_data_2022tr_dos$JusticiayBienestar)

Descrpitvo de la dimension de Conflicto de clase

{r}
summary(proc_data_2022tr_dos$Conflictoclase)

Descriptivo de la dimension de la Percepcion de trato justo

{r}
summary(proc_data_2022tr_dos$Perctrjust)

Y por ultimo la dimension de Cuidadania

{r}
summary(proc_data_2022tr_dos$Cuidadania)

Interpretaciones finales de las dimensiones y del indice de "percepcion de justicia social"

Existe una tendencia de valoración medianamente critica o desaprobatoria sobre el funcionamiento del roden social en terminos de justicia y equidad, pues la dimension que representa la justicia y bienestar al presentar una media baja (1.07) sugieriendo una baja percepcion de equidad en la distribucion de serivicios escenciales como lo es la educacion y salud (trabajados en este informe)Percpecion complementada por la dimension de Cuidadania, reflejando una media de 1.88, donde existe un nivel moderado de confianza en el beneficio de bienestar general por parte del Estado.La dimension de conflicto al arrojar un 0.87 refleja una baja justificacion de las desigualdades sociales, indicando que se rechaza la idea de justificar la diferencia entre clases.La media mas alta (3.17) se encuentra en la dimension de Percepcion de trato jusyo indicando que si bien esta la presencia de una desconfianza hacia las instituciones, se percibe que grupos, como la clase media percibe un trato mas justo.Como conclusion podriamos decir que la percepcion de justicia social se encuentra fragmentada, ya que por un lado se reconoce grados de desacuerdo o desaprobacion respecto al rol del estado, ya sea en: la legitimidad de las desigualdades estructurales y distribucion de recursos escenciales (educación y salud) y por otro lado, se reconoce cierto trato justo o justicia a sectores como la clase mediaTal contraste puede ser debido a una tension existente entre ideales de igualdad y la experencia cotidiana de esta, lo cual tiene implicancias a la hora de comprender como la percepción de una justiica social eficiente o satisfactoria o la existencia de una deficiencia o insatisfacción hacia esta.



Bibliografia

-Peña, C. (2013). La legitimidad en el ejercicio del poder político en el Estado constitucional. Revista de Derecho (Valdivia), 26(2), 27–42. https://www.scielo.cl/scielo.php?pid=S0718-00122013000200004&script=sci_arttext

-(Jordán, J., 1995). Jordán, J. A. (1995). Concepto y objeto de la educación cívica. Pedagogía social: revista interuniversitaria. https://digitum.um.es/digitum/bitstream/10201/39623/1/Concepto%20y%20objeto%20de%20la%20educaci%C3%B3n%20c%C3%ADvica.pdf

-Castillo Valenzuela, J. C. (2012). La legitimidad de las desigualdades salariales. Una aproximación multidimensional. Revista Internacional De Sociología, 70(3), 533–560. https://doi.org/10.3989/ris.2010.11.22 
