# rappiplus-subscription-strategic-diagnosis

Análisis integral del desempeño, rentabilidad, conversión, retención y experimentación de producto para **RappiPlus**, combinando preparación y limpieza de datos en Python, consultas SQL, análisis estadístico y visualización ejecutiva en *dashboard*.

## 📂 Contenido del repositorio
* `README.md` — Documentación del proyecto, metodología, hallazgos y recomendaciones.

## 🧪 Metodología y etapas del análisis
* **1. Preparación y calidad de datos (Python):** Auditoría de nulos, eliminación de 100 registros duplicados, filtrado de cantidades negativas, unificación de categorías (`Electronica` a `Electrónica`) y tratamiento de valores extremos mediante winsorización (`np.clip` entre percentiles 1 y 99) para calcular `cantidad_winsor` y `monto_total_corregido`.
* **2. Rentabilidad del negocio (Python):** Consolidación de pedidos, catálogo y gasto de marketing para evaluar ingresos, costos unitarios por producto, inversión publicitaria y beneficio neto.
* **3. Funnel de conversión (SQL):** Construcción del embudo por evento en PostgreSQL (`first_visit` → `add_to_cart` → `select_item` → `begin_checkout` → `add_payment_info` → `purchase`) para detectar puntos de fuga y conversión final.
* **4. Retención por cohortes (SQL):** Evaluación de recurrencia y actividad posterior al registro (`activo = 1`) por cohorte y ventana temporal.
* **5. Experimentación A/B (Python):** Evaluación del impacto de cambios en el *checkout* (`experiment_checkout_ui.csv`) comparando variantes de control y tratamiento.
* **6. Dashboard y comunicación ejecutiva:** Vista ejecutiva (KPIs, categorías, evolución mensual, ventas del mes anterior y *YTD*) y vista detallada (rentabilidad por producto, gasto por canal y resultados por país).

## 📈 Métricas principales (Paso 2 — Rentabilidad)
* **Ingreso total (`monto_total_corregido`):** $9.627.993,65
* **Costo total:** $3.834.482,61
* **Inversión en marketing:** $2.871.843,53
* **Beneficio neto (*Profit*):** $2.921.667,51
* **Ticket promedio por orden:** $385,95
* **Promedio de productos por orden:** **1,51** unidades
* **Productos más vendidos (por unidades):** `Vacuum-Pro-Black` (**6.284**), `Blender-XL-Red` (**6.279**), `Jacket-Winter-M` (**6.256**) y `Sneakers-Urban-42` (**6.172**).
* **Gasto en marketing por canal:** `social` ($918.043,21), `organic` ($913.533,01), `paid_search` ($863.088,21) y `desconocido` ($177.179,10).

## 📌 Conclusiones y hallazgos clave
* **¿Podemos confiar en los datos?** La limpieza rigurosa de nulos, duplicados, cantidades negativas y valores extremos evita distorsiones severas en el ingreso y la rentabilidad (por ejemplo, acotando montos atípicos extremos del dato crudo). Antes de exportar `orders_clean.csv`, conviene asegurar que el *dashboard* consuma exclusivamente las columnas corregidas (`cantidad_winsor` y `monto_total_corregido`) para mantener plena coherencia con el *notebook*.
* **¿Estamos ganando dinero?** El negocio es rentable, con un **beneficio neto de $2.921.667,51** tras cubrir costos de catálogo (**$3.834.482,61**) y marketing (**$2.871.843,53**). Sin embargo, el análisis detallado por producto evidencia diferencias de margen y casos puntuales con pérdidas (como `Laptop-Gaming-16GB`), por lo que conviene revisar precios, costos y descuentos por SKU.
* **¿Dónde se pierden los usuarios (*Funnel*)?** De **7.796** usuarios únicos en `first_visit`, **6.240** completan `purchase`, logrando una **conversión final del 80,04%**. El mayor *drop-off* se concentra entre `begin_checkout` y `add_payment_info` (**13,29%**), mientras que el resto de transiciones presenta caídas bajas (**2,08%** a carrito, **0,68%** a selección de ítem, **4,93%** a inicio de *checkout* y **0,16%** de pago a compra).
* **¿Los usuarios regresan (*Cohortes*)?** El seguimiento por cohortes permite contrastar la actividad post-registro (`activo = 1`) tanto por días/semanas desde el registro como por fechas calendario, identificando qué tanto se sostiene el uso recurrente frente a la caída tras la etapa inicial. *(Completa aquí los porcentajes exactos de retención de tu tabla de cohortes).*
* **¿Los cambios en *checkout* generan impacto (Prueba A/B)?** La evaluación entre grupos de control y tratamiento en `experiment_checkout_ui.csv` exige validar si las diferencias en conversión y duración de sesión son estadísticamente significativas antes de escalar el cambio a todo el tráfico. *(Completa aquí los valores exactos de conversión, *p-value* y decisión final de tu prueba A/B).*

## 🎯 Recomendaciones estratégicas
* **Reducir la fricción en el pago:** Investigar y optimizar la transición entre `begin_checkout` y `add_payment_info` (**13,29%** de pérdida), revisando métodos de pago, tiempos de carga o validaciones en formulario.
* **Optimizar rentabilidad del catálogo y marketing:** Auditar productos con bajo margen o pérdidas (como `Laptop-Gaming-16GB`) y reasignar inversión hacia canales y categorías con mayor retorno neto, reduciendo además registros sin canal identificado (`desconocido`).
* **Asegurar consistencia entre *notebook* y *dashboard*:** Verificar que todas las métricas del *dashboard* utilicen las variables limpias (`cantidad_winsor` y `monto_total_corregido`) y actualizar el enlace del *dashboard* una vez concluida la revisión.
* **Tomar decisiones de producto con rigor estadístico:** Escalar cambios de UI en *checkout* únicamente cuando exista evidencia estadística concluyente y complementar la conversión con palancas de retención recurrente.

## 🔗 Dashboard y recursos
[Abrir archivo del proyecto (.pbix) en Google Drive](https://drive.google.com/file/d/1Vk3fHPj3hf9QJUAuS1m15jQstcMLnwdp/view?usp=sharing)
