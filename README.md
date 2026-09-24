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

1. **Sube este repo como Git folder** en tu workspace de Databricks (Workspace → Create → Git
   folder → pega la URL de este repo).
2. Abre y corre **[`cargar_datos.py`](cargar_datos.py)** — crea tu catálogo y carga tus tablas
   desde los Parquet de **[`datos/`](datos/)** (una sola vez).
3. Lee **[`reto.md`](reto.md)** — tu desafío de negocio y los entregables por nivel.
4. Ten a mano **[`diccionario.md`](diccionario.md)** — qué significa cada columna y término.
5. Revisa **[`rubrica.md`](rubrica.md)** — cómo te evalúa el jurado.
6. Prepara tu presentación con **[`pitch/plantilla_pitch.html`](pitch/plantilla_pitch.html)**.
7. **Tu equipo arma el Clean Room** para cruzar con Banco y Seguros. **No hay guía paso a paso:
   descúbranlo** — es parte del reto (documentación de Databricks + experimentación).

> Tus datos vienen en **[`datos/`](datos/)** (Parquet) y los cargas tú con `cargar_datos.py`.
> La llave para cruzar entre empresas es siempre **`id_cliente_hash`**.

*Datos 100% sintéticos. Contenido educativo para el Datathon Grupo Pichincha.*
