# Reto DEUNA — Retener comercios y hacer crecer al dueño

## Contexto de negocio

Eres el equipo de datos de **Deuna**. Tienes cientos de miles de comercios en tu
ecosistema de pagos, pero muchos dejan de transaccionar (churn) y no sabes qué otros
productos del grupo podrían necesitar sus dueños. El **Banco** conoce el perfil financiero
de esas personas y **Seguros** sabe quién ya tiene póliza — pero por privacidad no puedes
ver sus datos directamente. Un **Clean Room** te deja cruzarlos sin exponer información
sensible de nadie.

## Pregunta central

> **¿Qué comercios están a punto de irse, y qué producto del grupo (crédito, inversión o
> seguro) deberíamos ofrecerle a cada dueño para retenerlo y crecerlo?**

## Tus datos — `deuna.clean_room.comercios_deuna`

| Columna | Descripción |
|---|---|
| `ruc`, `id_cliente_hash` | Identificadores (hash = llave de cruce) |
| `segmento` | Micro / Pyme / Empresa / Corp |
| `ciudad`, `giro_comercio`, `meses_activo` | Perfil del comercio |
| `num_clientes`, `num_transacciones`, `monto`, `ticket_promedio` | Actividad transaccional (3 meses) |
| `numero_productos` + 4 flags (`tiene_deuna_negocios`, `tiene_merchant`, `tiene_boton_qr_terceros`, `tiene_deuna_personas`) | Adopción de soluciones Deuna |
| `dias_desde_ultima_tx`, `frecuencia_mensual`, `monto_ult_mes`, `crecimiento_mom` | Recencia y momentum del comercio |
| `comision_generada`, `saldo_wallet_personas`, `metodo_pago_principal`, `num_reversos`, `usa_credito_deuna`, `nps_estimado` | Valor, wallet, comportamiento y satisfacción |
| `prob_desercion` | Probabilidad de churn ya estimada (tu variable objetivo) |

> **26 columnas** en total: al perfil base se sumaron indicadores de recencia, momentum
> (`crecimiento_mom`), valor (`comision_generada`, `saldo_wallet_personas`) y satisfacción
> (`nps_estimado`) — úsalos como features de churn y de propensión.

## Datos que obtienes vía Clean Room

- **Banco** (`clientes_banco`): `score_buro`, `flag_mora_30d`, `saldo_cuentas`, `sub_segmento`, `maximo_cupo_tc`… → capacidad y riesgo del dueño.
- **Seguros** (`clientes_seguros`): `prima_total_anual`, `segmento_riesgo`, `num_cancelaciones_12m`, `estado_cliente`… → si el dueño ya está asegurado o es un **gap de cross-sell** (~70k dueños Deuna aún NO tienen póliza).

**Llave de cruce:** `id_cliente_hash` (el RUC del comercio comparte el dueño-persona con Banco y Seguros).

## Pistas (señal real en los datos)

- El **riesgo del banco** (mora/score bajo) tiende a acompañar **mayor churn** del comercio → úsalo como feature.
- La **afluencia** del dueño (saldos altos) se asocia a comercios de mayor monto → segmenta ofertas.
- Los dueños con alta actividad y **sin seguro** son el mejor blanco de cross-sell.

## Entregables por nivel (sube tan alto como puedas)

1. **Insight** *(obligatorio)*: cruce en Clean Room + dashboard AI/BI del churn por segmento/ciudad y del gap de cross-sell.
2. **Predicción** *(obligatorio)*: modelo de churn de comercios usando features propias + del banco (score, mora). MLflow.
3. **Enriquecimiento**: `ai_classify` para segmentar dueños, `ai_gen` para redactar la oferta personalizada.
4. **Conversacional**: Genie Space que responda "¿qué comercios en riesgo tienen buen score de banco y no tienen seguro?".
5. **Agente / App** *(bonus)*: agente Next-Best-Action que recomiende retención + producto por dueño, o app para el equipo comercial.

## Ideas de insight

- Top comercios de alto valor con churn inminente → plan de retención.
- Dueños con buen `score_buro` y sin seguro → cross-sell de póliza priorizado.
- Micro-comercios de alta actividad candidatos a crédito de capital de trabajo.

## Criterios de éxito

- El cruce se hace **dentro del Clean Room**, sin exponer PII.
- El modelo/insight es **accionable** y **cuantifica** la oportunidad ($, # comercios, % retención).
- El pitch muestra el valor en vivo (idealmente con el Genie/agente respondiendo).
