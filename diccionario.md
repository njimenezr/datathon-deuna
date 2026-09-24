# Diccionario de datos y glosario — Equipo **DEUNA**

Referencia de **tus** datos: qué columnas tiene `comercios_deuna` y qué significa cada término
de negocio. Para cruzar, la llave con las otras empresas es **`id_cliente_hash`**.

> **Todo es sintético.** Cédulas, RUCs, montos y clientes son ficticios (cero PII). Las
> estructuras imitan las reales de Deuna.

---

## 1. Glosario cross-company (aplica a todos)

- **`id_cliente_hash`** — huella SHA-256 de la cédula. Igual en las 3 empresas para la misma
  persona → es lo que permite cruzar sin exponer la cédula real.
- **Clean Room** — sala limpia de datos: cada empresa comparte tablas para analizarlas en
  conjunto **sin ver la fila cruda de la otra**. Aquí es donde se hace el join gobernado.
- **Overlap / cruce** — personas que aparecen en más de una empresa (misma `id_cliente_hash`).
- **Gap de cross-sell** — clientes/comercios de una empresa que **no** están en otra. Es la
  oportunidad comercial (p. ej. dueños de comercio Deuna que aún no tienen póliza).
- **Cross-sell** — venderle a un cliente actual un producto de otra línea/empresa del grupo.
- **Churn / retención** — abandono del cliente (churn) vs. acciones para conservarlo (retención).
- **Next-Best-Action (NBA)** — la siguiente mejor oferta o acción recomendada por cliente.
- **PII** — datos personales identificables. Aquí no hay: todo es sintético y hasheado.

---

## 2. Tus datos — `deuna.clean_room.comercios_deuna` (~100 000 filas · 36 columnas)

Ecosistema de pagos/adquirencia. Cada fila es un **comercio**; el `id_cliente_hash` corresponde
al **dueño-persona** (así se cruza con Banco y Seguros).

| Columna | Tipo | Dominio / rango | Descripción |
|---|---|---|---|
| `ruc` | string | 13 dígitos | RUC del negocio (prefijo de 10 = cédula del dueño). |
| `id_cliente_hash` | string | SHA-256 | **Llave de cruce** (hash de la cédula del dueño). |
| `segmento` | string | Micro · Pyme · Empresa · Corp | Tamaño del comercio (~98.8% Micro). |
| `ciudad` | string | — | Ciudad del comercio. |
| `giro_comercio` | string | TIENDA_BARRIO, RESTAURANTE, SERVICIOS, MODA, TECNOLOGIA, SALUD, TRANSPORTE, BELLEZA | Rubro del negocio. |
| `meses_activo` | int | ≥ 0 | Meses operando en Deuna. |
| `num_clientes` | long | ≥ 0 | Pagadores únicos (últimos 3 meses). |
| `num_transacciones` | long | ≥ 0 | Transacciones (últimos 3 meses); ≥ `num_clientes` si ambos > 0. |
| `monto` | double | ≥ 0 (USD) | Total cobrado (últimos 3 meses). Sesgo fuerte a la derecha. |
| `ticket_promedio` | double | ≥ 0 (USD) | **Derivado**: `monto / num_transacciones` (0 si no hay transacciones). |
| `numero_productos` | int | 1–4 | **Derivado**: suma de los 4 flags de adopción. |
| `tiene_deuna_negocios` | int | 0/1 | Usa la App Deuna Negocios. |
| `tiene_merchant` | int | 0/1 | Integrado a la solución Merchants (poco frecuente). |
| `tiene_boton_qr_terceros` | int | 0/1 | Cobra con Botón QR. |
| `tiene_deuna_personas` | int | 0/1 | Tiene App Deuna Personas (casi siempre 1). |
| `prob_desercion` | double | [0, 1] | Probabilidad de **churn** del comercio. Sube con el riesgo del dueño. |
| `dias_desde_ultima_tx` | int | ≥ 0 | Recencia. Alto si el comercio está inactivo; sube con el riesgo/churn. |
| `frecuencia_mensual` | double | ≥ 0 | Transacciones por mes (≈ trimestre/3). 0 si no hay transacciones. |
| `monto_ult_mes` | double | ≥ 0 (USD) | Facturado el último mes. 0 si no hay transacciones. |
| `crecimiento_mom` | double | −0.6 … 0.5 | Crecimiento mes a mes; negativo cuando hay churn. |
| `comision_generada` | double | ≥ 0 (USD) | Fee para Deuna (0.5%–2% del monto). 0 si no hay transacciones. |
| `saldo_wallet_personas` | double | ≥ 0 (USD) | Saldo en la app Deuna Personas del dueño. Sube con la afluencia. |
| `metodo_pago_principal` | string | QR · TRANSFERENCIA · TARJETA · LINK_PAGO | Método de cobro principal. |
| `num_reversos` | int | ≥ 0 | Reversos/contracargos. Bajo; sube con el riesgo. 0 si no hay transacciones. |
| `usa_credito_deuna` | int | 0/1 | Usa crédito Deuna. Sube con afluencia + antigüedad. |
| `nps_estimado` | int | −100 … 100 | NPS estimado. Cae con el churn/riesgo. |
| `dias_activos_mes` | int | 0–30 | Días del mes con al menos una venta. 0 si no hay transacciones. |
| `monto_promedio_diario` | double | ≥ 0 (USD) | Monto por día activo. 0 si no hay transacciones. |
| `ticket_maximo` | double | ≥ 0 (USD) | Mayor venta del comercio. Escala con la afluencia. |
| `pct_tx_qr` | double | [0, 1] | Proporción de transacciones cobradas por QR. |
| `pct_tx_tarjeta` | double | [0, 1] | Proporción de transacciones con tarjeta (`pct_tx_qr + pct_tx_tarjeta ≤ 1`). |
| `tasa_aprobacion` | double | [0.5, 1] | % de transacciones aprobadas. Baja con el riesgo del dueño. |
| `num_dispositivos` | int | ≥ 1 | Dispositivos/puntos de cobro. Crece con el tamaño del comercio. |
| `antiguedad_meses_app` | int | ≥ 1 | Meses usando la app (≤ `meses_activo`). |
| `propension_credito` | double | [0, 1] | Propensión a tomar crédito. Sube con la afluencia. |
| `ingreso_estimado_mensual` | double | ≥ 0 (USD) | Ingreso mensual estimado del negocio. Escala con la afluencia. |

---

## 3. Glosario de negocio — Deuna (pagos / adquirencia)

- **Comercio** — negocio que cobra a través de Deuna. La fila de `comercios_deuna`.
- **RUC** — Registro Único de Contribuyentes (id fiscal del negocio en Ecuador).
- **Giro** — rubro o actividad del comercio (tienda, restaurante, etc.).
- **Ticket promedio** — monto medio por transacción (`monto / num_transacciones`).
- **Botón QR** — cobro por código QR. **Merchant** — integración de cobro para comercios.
- **Deuna Negocios / Deuna Personas** — apps para el lado negocio vs. persona.
- **`num_clientes` vs `num_transacciones`** — pagadores únicos vs. total de pagos (un cliente
  puede pagar varias veces).
- **`prob_desercion`** — probabilidad de que el comercio deje de operar (churn).
- **Recencia (`dias_desde_ultima_tx`)** — días desde el último cobro; alto = comercio dormido.
- **Frecuencia (`frecuencia_mensual`)** — transacciones por mes; junto con recencia mide actividad.
- **Momentum (`crecimiento_mom`)** — crecimiento mes a mes; negativo anticipa churn.
- **Comisión (`comision_generada`)** — fee que Deuna cobra por procesar los pagos del comercio.
- **Wallet (`saldo_wallet_personas`)** — saldo del dueño en la app Deuna Personas.
- **Reversos (`num_reversos`)** — pagos revertidos/contracargos; señal de fricción o riesgo.
- **NPS (`nps_estimado`)** — índice de satisfacción/recomendación (−100 a 100).
- **Método de pago (`metodo_pago_principal`)** — canal de cobro dominante (QR, transferencia, tarjeta, link de pago).

---

> **Para cruzar:** unirás con **Banco** y **Seguros** por `id_cliente_hash`. **Descubrirás el
> esquema de las otras empresas dentro del Clean Room** — no está aquí. Explora las tablas que
> te compartan y pregúntale a Genie qué columnas traen.
