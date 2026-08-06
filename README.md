# Índice de Vulnerabilidad Financiera para Población No Bancarizada del Perú

Construcción de un índice probabilístico de vulnerabilidad financiera para personas no bancarizadas del Perú, utilizando datos de la ENAHO 2024 (INEI) y técnicas de aprendizaje no supervisado.

## Descripción

Una proporción significativa de la población peruana permanece fuera del sistema financiero formal, y este grupo no es homogéneo: coexisten personas con distintas combinaciones de informalidad laboral, bajos ingresos, menor educación, carencias materiales y limitado acceso digital. Este proyecto construye un **Índice de Vulnerabilidad Financiera (IVF)** continuo y comparable entre individuos, capaz de representar esa heterogeneidad en lugar de aplicar una clasificación binaria rígida.

## Lo que hice

- Diseñé un proceso de análisis de datos en Python para integrar y depurar información de la ENAHO 2024 (módulos 100, 200, 300 y 500), obteniendo una base analítica de cerca de 30,000 registros de personas no bancarizadas entre 18 y 65 años.
- Preparé y transformé la información mediante imputación de valores faltantes (MICE y mediana/moda) y Análisis de Correspondencias Múltiples (MCA) para convertir variables categóricas en coordenadas continuas.
- Identifiqué perfiles latentes de vulnerabilidad financiera mediante Modelos de Mezclas Gaussianas (GMM), seleccionando el número óptimo de perfiles con criterios BIC, ICL y entropía de clasificación.
- Construí un índice probabilístico de vulnerabilidad financiera, combinando las probabilidades posteriores de pertenencia a cada perfil con el nivel promedio de vulnerabilidad de cada grupo, siguiendo la lógica de índices compuestos multidimensionales.
- Evalué la coherencia y estabilidad del índice mediante caracterización socioeconómica y territorial, modelos auxiliares de XGBoost, valores SHAP y bootstrap (200 iteraciones).

## Metodología

| Etapa | Técnica |
|---|---|
| Reducción de variables categóricas | Análisis de Correspondencias Múltiples (MCA) |
| Segmentación de perfiles latentes | Modelos de Mezclas Gaussianas (GMM) |
| Selección del número de perfiles | BIC, ICL, entropía de clasificación |
| Construcción del índice | Combinación probabilística vía teorema de probabilidad total |
| Interpretabilidad auxiliar | XGBoost + SHAP |
| Validación de robustez | Bootstrap simple (B = 200) |

**Fórmula del índice:**
IVF_i = Σ P(k | i) · R_k

Donde `P(k|i)` es la probabilidad posterior de que la persona *i* pertenezca al perfil *k*, y `R_k` es el nivel promedio de vulnerabilidad ponderado de ese perfil.

## Resultados principales

- Base analítica: **29,954 personas no bancarizadas** (18–65 años), proyectadas a **~9.24 millones** a nivel nacional.
- Se identificaron **5 perfiles latentes** de vulnerabilidad, con niveles Rk entre 0.31 y 0.60.
- El modelo XGBoost auxiliar reconstruye el índice con **R² ≈ 0.80**.
- El IVF medio resultó **altamente estable** frente a variaciones muestrales (bootstrap).

## Datos

Los datos provienen de la **Encuesta Nacional de Hogares (ENAHO) 2024**, elaborada por el INEI (módulos 100, 200, 300 y 500). Los microdatos no se incluyen en este repositorio; pueden descargarse desde el [portal de microdatos del INEI](https://proyectos.inei.gob.pe/microdatos/).

## Contenido del repositorio

- `Construccion_IVF.ipynb` — Notebook con el pipeline completo (limpieza, imputación, MCA, GMM, construcción del IVF, XGBoost/SHAP, bootstrap).
- `Construccion_Indice_Vulnerabilidad_Financiera.pdf` — Paper académico con la formulación teórica, metodología y resultados.
- `Informe_IVF_complementario.pdf` — Informe técnico completo, con detalle de cada bloque del notebook, diccionario de variables, recodificaciones y salidas de ejecución (documento de respaldo/trazabilidad).

## Herramientas

Python · pandas · numpy · scikit-learn · prince (MCA) · xgboost · shap · Google Colab

## Autor

Michael Sebastian Flores Quispe — Ingeniería Estadística, Universidad Nacional de Ingeniería (UNI)
Trabajo de investigación para el curso de Investigación Estadística, asesorado por Demetrio Antonio Ruiz Olorte.
