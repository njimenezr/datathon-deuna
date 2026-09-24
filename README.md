# Datathon Grupo Pichincha — Equipo **DEUNA**

Bienvenidos al datathon del **Grupo Pichincha**. El reto de fondo es uno solo: **¿cuánto valor
se desbloquea cuando Deuna, Banco Pichincha y Seguros del Pichincha combinan sus datos — sin
que ninguno vea la información sensible del otro?** La colaboración es segura y gobernada vía
**Databricks Clean Rooms**, y todos los datos son **100% sintéticos** (cero PII).

## Tu rol

Eres el equipo de datos de **Deuna** (pagos/adquirencia). Tu materia prima es
`deuna.clean_room.comercios_deuna`: un comercio por fila, con su actividad transaccional,
adopción de productos y probabilidad de churn. Tu oportunidad está en **cruzar** ese perfil con
el mundo financiero (Banco) y de protección (Seguros) para retener comercios y recomendar el
próximo mejor producto a sus dueños.

## Quick start

1. Tus datos están en **[`datos/`](datos/)** (Parquet). **Súbelos a tu workspace de Databricks
   como veas conveniente** — descubran cómo hacerlo, es parte del reto.
2. Lee **[`reto.md`](reto.md)** — tu desafío de negocio.
3. Ten a mano **[`diccionario.md`](diccionario.md)** — qué significa cada columna y término.
4. Revisa **[`rubrica.md`](rubrica.md)** — cómo te evalúa el jurado.
5. Prepara tu presentación con **[`pitch/plantilla_pitch.html`](pitch/plantilla_pitch.html)**.
6. **Tu equipo arma el Clean Room** para cruzar con Banco y Seguros. **No hay guía paso a paso:
   descúbranlo** — es parte del reto.

> La llave para cruzar personas entre empresas es siempre **`id_cliente_hash`**. Antes de
> compartir datos, revisa **[`guia_participante.md`](guia_participante.md)**.

*Datos 100% sintéticos. Contenido educativo para el Datathon Grupo Pichincha.*
