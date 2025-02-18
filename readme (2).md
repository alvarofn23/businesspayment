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


## Visualizaciones clave

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

# Distribucion de Cash Request
![image](https://github.com/user-attachments/assets/04fad72c-3e5f-402e-9b07-9b769be03af1)

# Distribucion de Fees
![image](https://github.com/user-attachments/assets/a9ce0299-609e-4a3a-a530-bf27390ec71d)

# Registros de Cash request por dia

![image](https://github.com/user-attachments/assets/220b1de6-8f8c-468f-9085-2796061d975d)


# Promedios

![image](https://github.com/user-attachments/assets/3914c7a3-943c-4a18-8317-42f1e1373eb7)

# Retrasos al devolver el prestamo

![image](https://github.com/user-attachments/assets/aa2b9b88-f288-481d-806a-3e3a741ef766)


# Evolucion de amount y fee

![image](https://github.com/user-attachments/assets/77303fa2-034b-4165-b340-7f98d5a3cd1f)

# Grafico de dispersion del monto de transferencia 

![image](https://github.com/user-attachments/assets/0ff9c2f7-a572-41f2-ae1c-3a72b577ac5f)



<!-- ## Información Adicional

Business Payments está entusiasmado de obtener perspectivas clave de este análisis para tomar decisiones basadas en datos que mejoren sus servicios financieros y la experiencia del usuario. Tu trabajo desempeñará un papel crucial en la configuración de futuras estrategias.

Gracias por asumir este desafío. ¡Esperamos tus valiosas contribuciones!

Atentamente,  
Ejecutivo de Business Payments -->
