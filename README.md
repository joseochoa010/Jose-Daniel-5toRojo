# Jose-Daniel-5toRojo
Primera modificación 
README
1. Descomposición del Problema (Subproblemas)
   Llegada de estudiantes: Recepción inicial en la entrada del centro, donde el estudiante se presenta y se le informa sobre los canales o requisitos básicos.
Registro y clasificación: Captura de la información del estudiante, identificación del tipo de trámite (entrega de documentos, solicitud de constancia o consulta general) y evaluación de su estatus de prioridad.

Gestión de prioridades (Cola de espera): Ordenamiento dinámico de los turnos en una estructura de datos (cola con prioridad) que equilibra el orden de llegada (FIFO) con los criterios de urgencia o condición especial.

Atención en ventanilla: Llamado del siguiente estudiante alfabética o numéricamente, procesamiento de su solicitud en la única ventanilla disponible y registro de tiempos (espera y atención).
2. Datos Necesarios para Representar a Cada Estudiante
AtributoTipo de DatoDescripciónid_estudianteCadena / NúmeroIdentificador único (Cédula, Carné o DNI).nombreCadenaNombre completo del estudiante.tipo_tramiteEntero / Categoría1: Entrega de documentos, 2: Solicitud de constancia, 3: Consulta general.nivel_prioridadEntero0: Prioridad alta (Casos especiales), 1: Prioridad media (Urgencias académicas), 2: Prioridad normal.hora_llegadaMarca de tiempo (Timestamp)Hora exacta en la que el estudiante tomó su turno.hora_atencionMarca de tiempoHora en la que la ventanilla comienza a atenderlo (para medir tiempos).estadoCadenaEstado actual: "En espera", "Siendo atendido", "Finalizado".
3. Criterio de Prioridad y Justificación
Nivel 0 (Prioridad Alta): Personas con necesidades especiales (adultos mayores, mujeres embarazadas, personas con discapacidad). Su atención es mandatoria por ley y ética.

Nivel 1 (Prioridad Media): Estudiantes con trámites que tienen un plazo de vencimiento inminente (ej. entrega de documentos cuyo cierre de convocatoria es el mismo día).

Nivel 2 (Prioridad Normal): El resto de los estudiantes que realizan trámites cotidianos.
Equidad y Orden: Dentro de cada nivel de prioridad, se aplica estrictamente el principio de orden de llegada (FIFO - First In, First Out), lo que evita sensaciones de injusticia o favoritismo.
4. Algoritmo en Pseudocódigo
INICIO
    Crear ColaPrioridad cola_espera
    Definir bandera_centro_abierto = VERDADERO

    MIENTRAS bandera_centro_abierto == VERDADERO HACER
        // 1. Subproblema: Llegada y Registro
        SI Hay_Estudiante_Llegando() ENTONCES
            Leer datos: id, nombre, tipo_tramite, es_prioridad_especial
            
            // Determinar nivel de prioridad
            SI es_prioridad_especial == VERDADERO ENTONCES
                nivel = 0
            SINO SI tipo_tramite == "Urgencia con plazo límite" ENTONCES
                nivel = 1
            SINO
                nivel = 2
            FIN_SI

            Crear objeto Estudiante con datos, hora_llegada actual y nivel
            Agregar Estudiante a cola_espera (ordenado por nivel y hora_llegada)
        FIN_SI

        // 2. Subproblema: Atención en Ventanilla única
        SI Ventanilla_Libre() Y NO_ESTA_VACIA(cola_espera) ENTONCES
            Estudiante actual = cola_espera.ExtraerSiguiente()
            actual.hora_atencion = ObtenerHoraActual()
            actual.estado = "Siendo atendido"

            // Procesar trámite según tipo
            SEGUN(actual.tipo_tramite)
                CASO 1: ProcesarEntregaDocumentos(); Romper;
                CASO 2: ProcesarSolicitudConstancia(); Romper;
                CASO 3: ProcesarConsultaGeneral(); Romper;
            FIN_SEGUN

            actual.estado = "Finalizado"
            RegistrarTiemposEstadisticos(actual)
        FIN_SI

        // 3. Subproblema: Cierre
        SI Hora_De_Cierre() O Fin_De_Jornada() ENTONCES
            SI cola_espera.EstaVacia() ENTONCES
                bandera_centro_abierto = FALSO
            // Nota: Si quedan estudiantes, se les atiende hasta vaciar la cola o se les reprograma.
        FIN_SI
    FIN_MIENTRAS

    GenerarReporteFinalTiemposPromedio()
FIN
1. Representación de Paradas, Distancias y EstudiantesEstudiantes por paradaParada 1 ($P_1$): 5 estudiantesParada 2 ($P_2$): 8 estudiantesParada 3 ($P_3$): 4 estudiantesParada 4 ($P_4$): 10 estudiantesParada 5 ($P_5$): 6 estudiantesParada 6 ($P_6$): 7 estudiantesTotal de estudiantes: 40Matriz de Distancias (en kilómetros)Desde \ HaciaDepósitoParada 1Parada 2Parada 3Parada 4Parada 5Parada 6ColegioDepósito02.54.06.15.28.07.110.0Parada 12.502.04.53.86.55.98.5Parada 24.02.003.02.55.04.87.0Parada 36.14.53.002.13.23.55.5Parada 45.23.82.52.103.02.86.0Parada 58.06.55.03.23.002.24.0Parada 67.15.94.83.52.82.203.0Colegio10.08.57.05.56.04.03.00
2. Algoritmo Paso a Paso
   Paso 1 (Inicialización): El autobús parte del Depósito. Se define el conjunto de paradas pendientes por visitar: $S = \{P_1, P_2, P_3, P_4, P_5, P_6\}$. Se inicializa el contador de pasajeros en 0.Paso 2 (Búsqueda de proximidad): Desde la ubicación actual, revisar la tabla de distancias y seleccionar la parada $P \in S$ que se encuentre a la menor distancia.Paso 3 (Validación y Registro):Moverse a dicha parada.Sumar los estudiantes de esa parada al total acumulado (verificando que no supere la capacidad máxima del autobús).Registrar la parada en la ruta y retirarla del conjunto $S$.Paso 4 (Iteración): Repetir los pasos 2 y 3 hasta que el conjunto $S$ esté vacío (es decir, se hayan visitado todas las paradas).Paso 5 (Finalización): Desde la última parada visitada, trazar el trayecto directo hacia el Colegio.
   3. Aplicación del Algoritmo y Propuesta de Ruta
      Petición Inicial: Partimos del Depósito. Paradas pendientes: $\{P_1, P_2, P_3, P_4, P_5, P_6\}$.Paso A: La distancia más corta desde el Depósito es a la Parada 1 ($2.5\text{ km}$).Acumulado: 5 estudiantes.Paso B: Desde la Parada 1, la más cercana es la Parada 2 ($2.0\text{ km}$).Acumulado: $5 + 8 = 13$ estudiantes.Paso C: Desde la Parada 2, la más cercana es la Parada 4 ($2.5\text{ km}$).Acumulado: $13 + 10 = 23$ estudiantes.Paso D: Desde la Parada 4, la más cercana es la Parada 3 ($2.1\text{ km}$).Acumulado: $23 + 4 = 27$ estudiantes.Paso E: Desde la Parada 3, la más cercana es la Parada 5 ($3.2\text{ km}$).Acumulado: $27 + 6 = 33$ estudiantes.Paso F: Desde la Parada 5, la más cercana es la Parada 6 ($2.2\text{ km}$).Acumulado: $33 + 7 = 40$ estudiantes (todas las paradas visitadas).Paso G: Desde la Parada 6, nos dirigimos al Colegio ($3.0\text{ km}$).Ruta Propuesta (Ruta A)$$\text{Depósito} \rightarrow P_1 \rightarrow P_2 \rightarrow P_4 \rightarrow P_3 \rightarrow P_5 \rightarrow P_6 \rightarrow \text{Colegio}$$Distancia total recorrida: $2.5 + 2.0 + 2.5 + 2.1 + 3.2 + 2.2 + 3.0 = \mathbf{17.5\text{ km}}$
   Comparación con una Alternativa
Ruta B (Alternativa):

Dep
o
ˊ
sito
→
𝑃
5
→
𝑃
6
→
𝑃
3
→
𝑃
4
→
𝑃
2
→
𝑃
1
→
Colegio
Dep 
o
ˊ
 sito→P 
5
​
 →P 
6
​
 →P 
3
​
 →P 
4
​
 →P 
2
​
 →P 
1
​
 →Colegio
Cálculo de Distancia para la Ruta B:

Depósito 
→
𝑃
5
→P 
5
​
 : 
8.0
 km
8.0 km

𝑃
5
→
𝑃
6
P 
5
​
 →P 
6
​
 : 
2.2
 km
2.2 km

𝑃
6
→
𝑃
3
P 
6
​
 →P 
3
​
 : 
3.5
 km
3.5 km

𝑃
3
→
𝑃
4
P 
3
​
 →P 
4
​
 : 
2.1
 km
2.1 km

𝑃
4
→
𝑃
2
P 
4
​
 →P 
2
​
 : 
2.5
 km
2.5 km

𝑃
2
→
𝑃
1
P 
2
​
 →P 
1
​
 : 
2.0
 km
2.0 km

𝑃
1
→
Colegio
P 
1
​
 →Colegio: 
8.5
 km
8.5 km

Distancia total Ruta B: 
8.0
+
2.2
+
3.5
+
2.1
+
2.5
+
2.0
+
8.5
=
28.8
 km
8.0+2.2+3.5+2.1+2.5+2.0+8.5=28.8 km

Criterio de Selección y Conclusión
Criterio elegido: Menor distancia total de recorrido (y por consiguiente, menor tiempo de viaje y menor consumo de combustible).

Comparativa:

Ruta Propuesta (Algoritmo): 
17.5
 km
17.5 km

Ruta Alternativa: 
28.8
 km
28.8 km

Conclusión: La ruta propuesta mediante el algoritmo del vecino más cercano es claramente superior, logrando un ahorro de 
11.3
 km
11.3 km por viaje. Esto reduce de forma significativa el tiempo en carretera para los estudiantes y optimiza los costos operativos del autobús escolar.
Errores
Error en el cálculo del promedio: El algoritmo suma las notas (nota1 + nota2 + nota3), pero nunca las divide entre la cantidad total de notas (3). Por lo tanto, está comparando la suma bruta (que podría llegar a 300) con el valor 60, lo cual da un resultado erróneo.

Falta de validación de datos: No existe ningún control para verificar que las notas ingresadas estén dentro de un rango lógico (por ejemplo, de 0 a 100). Un usuario podría ingresar números negativos o mayores a 100.
2. Pseudocódigo Corregido
Proceso CalcularPromedioEstudiante
    Definir nota1, nota2, nota3, promedio Como Real
    
    // Validación para nota 1 (Debe estar entre 0 y 100)
    Repetir
        Escribir "Ingrese la nota 1 (0-100): "
        Leer nota1
        Si nota1 < 0 O nota1 > 100 Entonces
            Escribir "Error: La nota debe estar entre 0 y 100. Intente de nuevo."
        FinSi
    Hasta Que nota1 >= 0 Y nota1 <= 100
    
    // Validación para nota 2 (Debe estar entre 0 y 100)
    Repetir
        Escribir "Ingrese la nota 2 (0-100): "
        Leer nota2
        Si nota2 < 0 O nota2 > 100 Entonces
            Escribir "Error: La nota debe estar entre 0 y 100. Intente de nuevo."
        FinSi
    Hasta Que nota2 >= 0 Y nota2 <= 100
    
    // Validación para nota 3 (Debe estar entre 0 y 100)
    Repetir
        Escribir "Ingrese la nota 3 (0-100): "
        Leer nota3
        Si nota3 < 0 O nota3 > 100 Entonces
            Escribir "Error: La nota debe estar entre 0 y 100. Intente de nuevo."
        FinSi
    Hasta Que nota3 >= 0 Y nota3 <= 100
    
    // Cálculo correcto del promedio (Suma dividida entre 3)
    promedio = (nota1 + nota2 + nota3) / 3
    
    // Mejora adicional: Mostrar el promedio calculado
    Escribir "----------------------------------------"
    Escribir "El promedio final es: ", promedio
    
    // Evaluación con condición de recuperación
    Si promedio >= 60 Entonces
        Escribir "Estado: ¡Aprueba!"
    SiNo
        Si promedio >= 50 Entonces
            Escribir "Estado: No aprueba directo. Debe ir a RECUPERACIÓN."
        SiNo
            Escribir "Estado: No aprueba."
        FinSi
    FinSi
    Escribir "----------------------------------------"
    
FinProceso
3. Resumen de Mejoras
Cálculo matemático exacto: Se añadió la división (nota1 + nota2 + nota3) / 3.

Validación de rango (0-100): Se utilizó la estructura repetitiva Repetir ... Hasta Que para bloquear la entrada de valores inválidos y obligar al usuario a ingresar notas correctas.

Transparencia en el resultado: Se agregó la instrucción para imprimir el promedio numérico exacto en pantalla.

Condición de recuperación: Se implementó una lógica intermedia donde un promedio entre 50 y 59 otorga una oportunidad de recuperación en lugar de un fallo definitivo.
