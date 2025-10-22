# Proyecto-11-Bootcamp

•	Objetivo: Análisis de Pruebas A/B para una aplicación de compras

•	Librerías usadas: pandas, numpy, matplotlib.pyplot, seaborn, math, scipy.stats, statsmodels.stats.proportion.proportions_ztest.

•	Carga y preprocesado:

o	logs = pd.read_csv('/datasets/logs_exp_us.csv', sep='\t')

o	Renombrado de columnas: EventName → event_name, DeviceIDHash → device_id_hash, EventTimestamp → event_time_stamp, ExpId → exp_id.

o	Casting device_id_hash a str y conversión event_time_stamp a datetime (unit='s').

o	Creación de columnas event_date (fecha), event_time y event_hour.

o	Filtrado por fecha de corte: fecha_corte = '2019-08-01' → logs_corte = logs[logs['event_date'] >= pd.to_datetime(fecha_corte)]

•	Análisis Exploratorio de Datos y métricas básicas:

o	total_eventos = len(logs)

o	usuarios_únicos = logs['device_id_hash'].nunique()

o	promedio_eventos_usuario = total_eventos / usuarios_únicos

o	Conteos por fecha/hora y visualizaciones con seaborn.barplot.

o	Cálculo de cuántos eventos/usuarios se pierden después del corte.

•	Funnel (embudo):

o	Cálculo de usuarios únicos por evento: usuarios_por_evento = logs_corte.groupby('event_name')['device_id_hash'].nunique().reset_index()

o	Selección de secuencia: sec_embudo = ['MainScreenAppear','OffersScreenAppear','CartScreenAppear','PaymentScreenSuccessful']


o	Cálculo de tasa de conversión entre etapas con shift(1).

•	Agrupación experimental:

o	Identificación de grupos: exp_id (ej.: 246, 247, 248).

o	Conteo de usuarios por grupo.

•	Pruebas estadísticas:

o	Implementación manual de test z para proporciones (usando scipy.norm and fórmula de error estándar) para comparar 246 vs 247.

o	Función check_aa_test(event_name, group_1_data, group_2_data, n_1, n_2) que usa proportions_ztest de statsmodels.

o	Bucle que aplica check_aa_test a todos los eventos.

o	Creación de grupo_248 (test) y control_combinado = concat([grupo_246,grupo_247]).

o	Función check_ab_test para comparar grupo 248 contra cada control; bucle que crea tablas con p-valores por evento y consolida resultados en resultados_finales.

o	Nivel de significancia usado: 0.05; conclusión final: no diferencias significativas (p-valores > 0.05).
