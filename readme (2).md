![ifp_logo](
https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTZaS7gErfP2ddsie5E0NJuLGLQcNKkEVef2w&s
)


# Desafío Empresarial: Análisis de Insights y Cohortes Avanzados para los Pagos de Business Payments

## Introducción

Business Payments, una empresa de servicios financieros de vanguardia, ha estado ofreciendo soluciones innovadoras de adelanto de efectivo desde su creación en 2020. Con un compromiso de proporcionar adelantos de dinero gratuitos y precios transparentes, Business Payments ha logrado construir una base de usuarios sólida. Como parte de su esfuerzo continuo por mejorar sus servicios y entender el comportamiento de los usuarios, Business Payments ha encargado un proyecto para realizar un análisis avanzado de insights y cohortes.

## Visión General del Proyecto

En este proyecto, realizarás una extracción de insights accionables, un análisis avanzado de cohortes y una segmentación exhaustiva de los datos proporcionados por Business Payments. El objetivo principal es analizar cohortes de usuarios relevantes, como, por ejemplo, aquellas definidas según el período en el que realizaron su primer adelanto de efectivo, aunque se deben explorar más opciones de cohortes para su análisis. Además, se seguirá la evolución temporal de métricas clave para estas cohortes y se propondrán insights relevantes, así como modelos de regresión y clasificación que permitirán a Business Payments obtener valiosas perspectivas sobre el comportamiento de los usuarios y el rendimiento de sus servicios financieros.


### Análisis Exploratorio de Datos (EDA)

Contexto: Estructura de los Datos

"extract - fees - data analyst.csv"
Contiene información sobre pagos, incluyendo montos, estados y fechas.
Tiene valores nulos significativos en:
category (18,865 valores nulos).
paid_at (5,530 valores nulos).
from_date y to_date (13,295 valores nulos).
No hay valores duplicados.
total_amount tiene un valor mínimo de 5 y máximo de 10, lo que indica poca variabilidad.
type y status pueden ser relevantes para segmentaciones.

"extract - cash request - data analyst.csv"
Registra solicitudes de efectivo con estados y fechas de reembolso.
Presenta valores nulos en varias columnas clave, como:
user_id (2,103 valores nulos).
deleted_account_id (21,866 valores nulos).
recovery_status, reco_creation y reco_last_update tienen más de 20,000 valores nulos.
amount varía entre 1 y 200, con una media de 82.72.
No hay valores duplicados.
transfer_type y status podrían ser útiles para segmentaciones.

![image](https://github.com/user-attachments/assets/4e880b7d-42fa-4e99-af6a-25701b243358)

![image](https://github.com/user-attachments/assets/5f9ec99c-cccb-4d63-b998-6b9be6ec73c1)

![image](https://github.com/user-attachments/assets/3cdd6737-6798-42e1-9b0c-d545b2131069)

![image](https://github.com/user-attachments/assets/5bec81b4-24fb-4a58-b049-39cc0c8e154a)

1. Distribución de montos en ambas tablas:

*En fees, los montos están altamente concentrados en 5 y 10 unidades.

*En cash requests, hay una mayor variabilidad en los montos, con valores entre 1 y 200 unidades, aunque la mayoría de los valores están por debajo de 100.

2. Distribución de estados (status) en ambas tablas:
   
*En fees, ciertos estados predominan claramente, lo que puede indicar que la mayoría de las transacciones siguen un patrón definido.

*En cash requests, la distribución de estados también muestra tendencias claras, lo que puede ayudar a segmentar a los usuarios.

# Inconsistencias en los Datos

*Se encontraron fechas de solicitud de adelanto de efectivo posteriores a la fecha de devolución, lo que no es lógico.

*Algunos registros de fees no tienen una solicitud de adelanto asociada, lo que podría significar errores en la integración de datos.

*Estados de solicitud con nombres similares pero escritos de manera diferente (ejemplo: "Approved" y "approved").

*Duplicados
Se identificaron registros duplicados en el conjunto de solicitudes de adelanto, especialmente en solicitudes con el mismo usuario y monto dentro del mismo minuto.


## Mejoras en la Tabla "extract - fees - data analyst.csv"

Problemas detectados:

*Valores nulos en category (18,865 registros vacíos)

*paid_at tiene 5,530 valores nulos

*Fechas from_date y to_date con más de 13,000 valores nulos

*No hay información del usuario asociado al fee

Columnas a Agregar para Completar y Mejorar

✅ user_id → Para vincular cada fee con el usuario correspondiente.

✅ days_until_payment → Diferencia en días entre paid_at y created_at (si paid_at no está vacío).

✅ payment_delay_flag → Indicador (1 si el pago se retrasó más de X días, 0 si no).

✅ category_filled → Para reemplazar valores nulos de category, se podría:

*Rellenar con la categoría más común del usuario si user_id está disponible.
*Imputar valores según el tipo de fee (type). ✅ fee_status_final → Nueva variable que combine status y charge_moment para entender mejor cuándo se cobra cada fee.


##Mejoras en la Tabla "extract - cash request - data analyst.csv"

Problemas detectados:

*user_id tiene 2,103 valores nulos

*Muchas fechas (cash_request_received_date, money_back_date, send_at) tienen valores faltantes

*La relación con los fees no es clara

*No hay información sobre la cantidad total de solicitudes por usuario

Columnas a Agregar para Completar y Mejorar

✅ total_requests_by_user → Cantidad de solicitudes de adelanto hechas por cada usuario.

✅ average_request_amount → Promedio del monto solicitado por cada usuario.

✅ days_to_reimbursement → Diferencia entre created_at y reimbursement_date (tiempo que tarda el usuario en devolver el dinero).

✅ approval_rate_by_user → % de solicitudes aprobadas para cada usuario.

✅ first_request_flag → 1 si es la primera solicitud del usuario, 0 si no.

✅ is_recurrent_user → 1 si el usuario ha solicitado más de X adelantos en un periodo, 0 si es esporádico.

✅ cash_request_status_final → Nueva variable combinando status y transfer_type para identificar qué tipo de solicitud es más efectiva.

*Conexión entre ambas tablas

Para mejorar el análisis y las predicciones, se podría agregar:

✅ total_fees_paid_by_user → Relacionando user_id de "fees" con "cash requests".

✅ total_amount_fees_by_user → Total pagado en fees por cada usuario.

✅ days_between_requests → Tiempo promedio entre solicitudes de adelanto para cada usuario.

# Frecuencia de uso del servicio

He calculado la frecuencia de uso del servicio por cohorte, mostrando el promedio de solicitudes de adelanto de efectivo por usuario en cada mes de cohortización.

![image](https://github.com/user-attachments/assets/9be4f7b7-2679-4ddd-8a1b-66faa7cdf0d4)

# Tasa de incidentes por cohorte

He calculado la tasa de incidentes por cohorte, mostrando el porcentaje de solicitudes de adelanto de efectivo que tuvieron problemas (rechazadas, fallidas o canceladas) en cada mes de cohortización.

![image](https://github.com/user-attachments/assets/2a2a9134-a82f-43af-ad34-daa13ad9266f)

# Ingresos generados por cohorte

He calculado los ingresos generados por cohorte, mostrando el total de ingresos obtenidos en cada mes de cohortización.

![image](https://github.com/user-attachments/assets/617be294-0189-437d-a65c-f42e3fea52c1)


# Visualizaciones clave

*Distribucion de estados de las solicitudes

![image](https://github.com/user-attachments/assets/33166b02-02b6-44dc-a8a6-0b3664f188fe)

1. Distribución de Montos de Solicitudes de Adelanto de Efectivo
Se observa una mayor concentración de solicitudes en montos menores a 100 unidades monetarias, con una distribución sesgada hacia la derecha.

2. Distribución de Ingresos por Fees
Los ingresos generados por fees presentan una variabilidad significativa, con algunos valores atípicos que sugieren usuarios de alto valor que contribuyen más significativamente al total de ingresos.

3. Frecuencia de Solicitudes de Adelanto de Efectivo a lo Largo del Tiempo
Se aprecia una fluctuación en la frecuencia de solicitudes, con un incremento en determinados períodos que podría correlacionarse con necesidades estacionales o eventos externos.

4. Distribución de Estados de Solicitudes de Adelanto
Aprobadas: Mayoría de las transacciones
Rechazadas: Representan una fracción importante del total
Pendientes y otras: En menor proporción

## Conclusiones Clave

*Patrón de Montos Solicitados: La mayor parte de las solicitudes son por montos relativamente bajos, lo que sugiere que los usuarios buscan adelantos para necesidades inmediatas y pequeñas.

*Variabilidad en los Fees Generados: Algunos usuarios contribuyen de manera desproporcionada a los ingresos totales, lo que indica que un pequeño grupo de clientes podría ser clave en la rentabilidad.

*Fluctuación de la Frecuencia de Uso: La cantidad de solicitudes cambia con el tiempo, lo que podría indicar que ciertos períodos (ej. fin de mes, temporada festiva) generan más demanda.

*Tasa de Rechazo No Despreciable: Un porcentaje significativo de solicitudes es rechazado, lo que podría analizarse en profundidad para entender los factores que contribuyen a ello.

# Graficos importantes

## Distribucion de Cash Request
![image](https://github.com/user-attachments/assets/04fad72c-3e5f-402e-9b07-9b769be03af1)

## Distribucion de Fees
![image](https://github.com/user-attachments/assets/a9ce0299-609e-4a3a-a530-bf27390ec71d)

## Registros de Cash request por dia

![image](https://github.com/user-attachments/assets/220b1de6-8f8c-468f-9085-2796061d975d)


## Promedios

![image](https://github.com/user-attachments/assets/3914c7a3-943c-4a18-8317-42f1e1373eb7)

## Retrasos al devolver el prestamo

![image](https://github.com/user-attachments/assets/aa2b9b88-f288-481d-806a-3e3a741ef766)


## Evolucion de amount y fee

![image](https://github.com/user-attachments/assets/77303fa2-034b-4165-b340-7f98d5a3cd1f)

## Grafico de dispersion del monto de transferencia 

![image](https://github.com/user-attachments/assets/0ff9c2f7-a572-41f2-ae1c-3a72b577ac5f)

## Matriz de correlacion de las variables

![image](https://github.com/user-attachments/assets/2f3ef717-ae8c-417a-bbd3-90eda58cb518)

Variables altamente correlacionadas con otras:
['id_x', 'id_y', 'cash_request_id']

## Matriz de correlacion de las variables categoricas

![image](https://github.com/user-attachments/assets/e8f07ac4-8f74-4e62-b443-513d4fea0747)

Variables altamente correlacionadas con otras:
['id_x', 'id_y', 'cash_request_id', 'transfer_type_instant', 'transfer_type_regular']

## Evolucion, tendencia y estacionalidad del monto solicitado

![image](https://github.com/user-attachments/assets/8cf7e19c-a952-4d9e-9123-71e85d8529ab)

## Evolucion de las solicitudes moderadas 

![image](https://github.com/user-attachments/assets/148be8ad-8be5-4196-a1ae-d1fcc166cf03)

## Evolucion de solicitudes creadas  vs moderadas 

![image](https://github.com/user-attachments/assets/e30912f1-c091-4339-b686-ab403c76d400)

## Numero de adelantos por mes y semana

![image](https://github.com/user-attachments/assets/d292b117-db5e-4028-909b-ede62abfafe3)

## Numero de adelantos devueltos correctamente por semana

![image](https://github.com/user-attachments/assets/e8359417-8909-43ad-bfc9-e6f1d2a85afd)

## Cantidad de adelantos por dia de la semana

![image](https://github.com/user-attachments/assets/a192d4b0-21c6-4103-b8cf-8573f772458f)

## Cantidad de adelantos por mes

![image](https://github.com/user-attachments/assets/53657ed5-5854-4bcc-8442-9d59e57a4839)

## Estacionalidad de adelantos: dia de la semana vs mes

![image](https://github.com/user-attachments/assets/58de7d62-46a8-4f12-914e-4b41910eb005)

## Patrones semanales de adelantos

![image](https://github.com/user-attachments/assets/25805a4f-c4fc-4442-9131-2bd69dcf004d)

## Patrones de adelantos a lo largo de los dias del mes

![image](https://github.com/user-attachments/assets/31754308-d9b5-41ad-b7df-52fb5359e838)


## Descomposicion de la Serie temporal semanal de adelantos 

![image](https://github.com/user-attachments/assets/e5625f74-3c27-4dd2-8a41-b4aa815ab63f)


## Evolucion de la cantidad de clientes 

![image](https://github.com/user-attachments/assets/02a26f0d-87ea-45f0-8483-65de825d649a)

## Evolucionde usuarios unicos creados por semana

![image](https://github.com/user-attachments/assets/0fcf7c27-6ead-4c3d-852b-e907ef53ea79)

## Tendencia de las horas a las que se solicita el dinero

![image](https://github.com/user-attachments/assets/1b52df91-18a9-4cd1-b746-59c02a2c8e35)


## Evolucion mensual de fees ratios

![image](https://github.com/user-attachments/assets/ecb332eb-176d-4877-9d50-12b3e3b5d74a)

*El fees ratio (ratio de fees) es una métrica que indica la proporción de ingresos por fees (tarifas cobradas) en relación con el monto total de adelantos de efectivo otorgados.

*¿Qué mide?

Qué porcentaje de los adelantos de efectivo proviene de las tarifas cobradas a los clientes.
Si el fees ratio es alto, significa que el banco está obteniendo una parte significativa de sus ingresos de las tarifas.
Si es bajo, podría indicar que:
Se están cobrando menos fees en relación al dinero adelantado.
O que la mayoría de los adelantos no generan fees (por descuentos, promociones, etc.).

¿Por qué es importante?

Evaluar la rentabilidad de los adelantos de efectivo.
Analizar si la estrategia de cobro de fees está siendo efectiva.
Comparar diferentes tipos de transferencias instant vs regular para ver cuál genera más ingresos.
Identificar posibles ajustes en las políticas de fees si el ratio es muy bajo o inestable.

## Evolucion menusal de adelantos vs fees

![image](https://github.com/user-attachments/assets/62efffe3-4fa1-4933-babf-21da0022bcbd)

## Evolucion menusal de fees por tipo

![image](https://github.com/user-attachments/assets/a83681d4-d803-4215-90c7-8be6a887cb62)

## Distribucion de fees de pagos instantaneos por charge moment 

![image](https://github.com/user-attachments/assets/c2a50138-e3a6-4300-9587-368ef9ebab65)

El gráfico representa la distribución de fees generados en pagos instantáneos según el momento en el que se cobra la tarifa.

¿Qué conclusiones sacamos?

Identificar el momento en el que más fees se generan:

Si una porción es mucho más grande que las otras, significa que la mayoría de los fees de pagos instantáneos se cobran en ese momento. Esto puede ayudar a entender cuándo los clientes están dispuestos a pagar más fees. Evaluar la estrategia de cobro:

Si un charge_moment genera muy pocos fees, podrías preguntarte si tiene sentido mantener esa estructura de cobro o si deberías incentivar otros momentos con mayores ingresos.

## Proporcion de fees pagadas
![image](https://github.com/user-attachments/assets/4448a63b-a0a4-43eb-9b12-aeafcec65c95)

Conclusiones que sacamos:

1. Comparar la efectividad de los momentos de cobro (charge_moment)

Si en un charge_moment la mayoría de los fees se pagan, significa que ese es un momento efectivo para cobrar.
Si en otro charge_moment hay muchas fees no pagadas, puede indicar que los clientes no suelen pagar en ese momento.

2. Identificar problemas en el cobro de fees

Si el porcentaje de fees no pagadas es alto en todos los charge_moment, puede haber un problema con la estrategia de cobro (quizás los clientes no pueden pagar en esos momentos o hay fallos en el sistema de cobro).
Si solo un charge_moment tiene muchas fees impagas, podría ser un horario o método de cobro poco conveniente para los clientes.

3. Analizar patrones de pago de los clientes

Si las fees pagadas son mayores en ciertos momentos, podrías priorizar esos horarios para futuras estrategias de cobro.
Si las fees impagas son muy altas en ciertos momentos, podrías evaluar alternativas (recordatorios, cambios en la estrategia de cobro, etc.).

4. Evaluar la rentabilidad de los pagos instantáneos

Si la mayoría de los fees se pagan, significa que los pagos instantáneos generan ingresos de manera estable.
Si hay muchas fees impagas, puede indicar que este método de pago no es tan rentable como parece y necesita ajustes.



# ANALISIS DE COHORTES

## Tasa de retención de clientes por cohorte

![image](https://github.com/user-attachments/assets/218b7af8-fa15-45f5-abcf-30e570e29249)

## Tasa de abandono de clientes por cohorte

![image](https://github.com/user-attachments/assets/dfda7d51-45a1-4bce-bf06-f269aaf8b771)

## Tasa de retención de solicitudes por cohorte

![image](https://github.com/user-attachments/assets/e706fc15-6040-4d7a-be03-4d921a68423d)

## Tasa de abandono de solicitudes por cohorte

![image](https://github.com/user-attachments/assets/0dc035f2-0af8-441f-aa99-fb65c34d0c8a)

## Tasa de retención de tarifas por cohorte

![image](https://github.com/user-attachments/assets/cc27b05a-782e-4a36-93e0-440e48b337e6)

## Tasa de abandono de tarifas por cohorte

![image](https://github.com/user-attachments/assets/04c5c03b-ccb7-423b-9483-a193a70079ce)

## Cantidad de adelanto de efectivo por cohorte y mes

![image](https://github.com/user-attachments/assets/39f76a03-9bed-4030-96e2-b909b19231ad)

## Interpolacion

![image](https://github.com/user-attachments/assets/0eeac38e-f486-491f-967d-2225648bb66b)

![image](https://github.com/user-attachments/assets/78e7ed53-ab96-472d-84f7-ce6562858742)

![image](https://github.com/user-attachments/assets/24da9297-0069-400b-8b11-171f7e6074cf)

![image](https://github.com/user-attachments/assets/63a619ac-788e-4019-b15e-43ed2c61ba41)

![image](https://github.com/user-attachments/assets/2b49fe36-496e-42f9-94f2-21c7936979ff)


## Evolucion del numero de solicitudes por tipo de cohorte 

![image](https://github.com/user-attachments/assets/f46a7196-fa67-4cba-81e3-b07caad92d07)

## Tasa de retencion de cohortes a lo largo del tiempo

![image](https://github.com/user-attachments/assets/7e7d00bb-52bd-448d-8578-0e615c4845ad)

## Valor de vida de cliente por cohortes a lo largo del tiempo

![image](https://github.com/user-attachments/assets/12f7d6eb-f9f4-4388-8c0c-18868b385044)

1. Cohortes con LTV más alto:
Filas con colores más intensos. Estas cohortes son las más valiosas; podrías analizar: ¿Qué promociones o campañas atrajeron a esos clientes? ¿Qué diferencias hay con cohortes menos valiosas?

2. Tendencias a lo largo del tiempo:
Si los colores se vuelven más claros mes a mes, significa que: Los clientes tienden a generar menos ingresos con el tiempo. Si algunas cohortes mantienen o aumentan su LTV con el tiempo: Son clientes leales que siguen generando ingresos.

3. Impacto de cambios en el negocio:
Si se aplicó un nuevo método de fees o estrategia de marketing en un mes específico, puedes ver si las cohortes siguientes tienen un LTV más alto o bajo.


# CLASIFICACION

## Distribucion de cash request por estado 

![image](https://github.com/user-attachments/assets/630b7af9-f30a-4c36-92d4-c41ff9371964)

## Distribucion de fees por estado

![image](https://github.com/user-attachments/assets/90fab636-4634-4a26-8eb9-ed9d20c003d1)

## Rango de morosidad de los clientes

![image](https://github.com/user-attachments/assets/a14110e8-e5a1-4c73-b623-350328e0efd1)

## Distribucion de probabilidad de morosidad matriz de confusion de regresion logistica y arbol de decision

![image](https://github.com/user-attachments/assets/ec82ed9a-6541-4452-8316-f6a8d5d87057)

![image](https://github.com/user-attachments/assets/bf8d35a3-206a-4a64-a544-ef04f8904a9d)

![image](https://github.com/user-attachments/assets/371fbdbe-3bc0-4318-a2df-37f24995e83f)

![image](https://github.com/user-attachments/assets/b6433ad8-37dd-4383-a0df-0e8f298e3365)





<!-- ## Información Adicional

Business Payments está entusiasmado de obtener perspectivas clave de este análisis para tomar decisiones basadas en datos que mejoren sus servicios financieros y la experiencia del usuario. Tu trabajo desempeñará un papel crucial en la configuración de futuras estrategias.

Gracias por asumir este desafío. ¡Esperamos tus valiosas contribuciones!

Atentamente,  
Ejecutivo de Business Payments -->
