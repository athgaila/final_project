DESCRIPCIÓN
Este proyecto desarrolla un pipeline completo de ciencia de datos para analizar y modelizar los precios de alojamientos Airbnb en Sicilia.
Combina:
Análisis exploratorio de datos (EDA)
Segmentación de mercado
Feature engineering
Modelos de machine learning
Interpretabilidad (Permutation Importance + SHAP)
Análisis de residuos


OBJETIVOS
Negocio: entender qué factores influyen en el precio
Modelización: construir un modelo predictivo robusto


DATASET
Fuente: dataset de Airbnb (Sicilia.xlsx)
Variables principales:
Características del listing (room_type, minimum_nights)
Información del host (calculated_host_listings_count)
Ubicación (latitude, longitude, neighbourhood)
Demanda (number_of_reviews, reviews_per_month)
Disponibilidad (availability_365)


LIMPIEZA Y PROCESAMIENTO
Principales decisiones:
Tratamiento de missing values:
reviews_per_month: imputación con mediana + indicador de missing
license: rellenado con "unknown"
Filtrado:
Eliminación de precios nulos o inválidos
Eliminación de coordenadas faltantes
Transformación del target:
log_price = log(price) para estabilizar la varianza


EDA
Se analizaron:

Distribución del precio (log-transformado)
Precio por tipo de habitación
Engagement (number_of_reviews)
Rentabilidad proxy:
profit_score = median_price × reviews_per_month × (1 - availability/365)
Estructura de mercado:
Relación entre tamaño del host y precio (OLS)
Análisis por vecindario
Visualización geográfica con mapas interactivos


FEATURE ENGINEERING
Variables creadas:
Transformaciones seguras:
number_of_reviews_safe
minimum_nights_safe
Densidad:
neighbourhood_density
Variables geográficas:
lat_sq, lon_sq
Ratios:
reviews_per_listing
availability_ratio

Tratamiento de outliers:
Se eliminan valores por encima del percentil 95 del precio


MODELADO
Modelo utilizado
HistGradientBoostingRegressor
Pipeline
Variables numéricas:
Imputación por mediana
Variables categóricas:
Imputación por moda
One-Hot Encoding
Integración con ColumnTransformer
Entrenamiento
Split: 80% train / 20% test
Validación cruzada (CV=5)


EVALUACIÓN
Toda la evaluación se realiza de forma consistente sobre:
Conjunto de test (X_test, y_test)

Métricas utilizadas:
RMSE (escala log y original)
MAE
R²
MAPE (en análisis de residuos)


INTERPRETABILIDAD DEL MODELO
1. Permutation Importance
Aplicado sobre el pipeline completo
Mide el impacto de cada variable original
Interpretación directa a nivel negocio

2. SHAP
Aplicado sobre variables transformadas
Explica predicciones individuales y globales
Uso de shap.Explainer para compatibilidad con modelos de tipo árbol


ANÁLISIS DE RESIDUOS
Realizado exclusivamente sobre el conjunto de test:
Distribución de errores (real − predicho)
Gráfico real vs predicho
Error vs precio
Error relativo
Identificación de los mayores errores

Consistencia
Ausencia de data leakage


SEGMENTACIÓN DE MERCADO
Algoritmo: KMeans (k=4)
Variables:
precio
reviews
disponibilidad
minimum nights

Resultado:
Identificación de segmentos diferenciados del mercado Airbnb


ANÁLISIS GEOGRÁFICO
Visualización con Folium
Muestreo por vecindario
Color por rango de precios


PRINCIPALES INSIGHTS
El tipo de habitación es uno de los principales drivers del precio
Hosts con múltiples listings influyen en la estructura del mercado
La disponibilidad impacta directamente en la rentabilidad
La ubicación tiene un efecto no lineal (capturado con features geográficas)


TECNOLOGÍAS USADAS
Python
pandas, numpy
matplotlib, seaborn
scikit-learn
statsmodels
shap
folium


MEJORAS FUTURAS
Optimización de hiperparámetros (Optuna / GridSearch)
Modelos espaciales más avanzados
Incorporación de estacionalidad
Datos externos (turismo, eventos, clima)