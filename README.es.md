[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | [日本語](README.ja.md) | Español

# golla-jev (골라제브)

<p align="center">
  <img src="assets/golla-jev-hero.png" alt="golla-jev" width="320">
</p>

> En los mercados coreanos los vendedores gritan "골라 골라~" ("¡elige, elige!"). En la oficina también se pasa el día eligiendo: a qué partida va este recibo, si este correo necesita respuesta, si esta cláusula es arriesgada. golla-jev encuentra esos **momentos de elegir** en tu trabajo y diseña un flujo que se los entrega a Jev.

Jev (TypeSafe System One) no escribe texto. Elige entre opciones fijas, responde con probabilidades y tarda unos 0,2 s por llamada. golla-jev toma la descripción de tu trabajo y devuelve un diseño que deja la elección a Jev, la aritmética, las fechas y los cruces de datos al código, las frases a plantillas y la lectura de fotos o audio a un modelo multimodal.

## Inicio rápido

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
/plugin install golla-jev
```

Claude Code 2.1.275+: `/plugin install golla-jev --marketplace fivetaku/gptaku_plugins`

Reinicia Claude Code y prueba:

- `/golla-jev Quiero automatizar los reembolsos de recibos de mi equipo`
- "golla-jev, ¿dónde podría ayudar Jev en mi empresa?" — si el alcance es amplio, primero lista 3–5 candidatos (modo idea)
- "Diseña un flujo con Jev para esta tarea" — diseña una tarea de principio a fin (modo diseño)

## Qué obtienes

- Un flujo de un vistazo: entrada → transcripción → N preguntas a Jev → script → resultado
- Una tabla por pasos que asigna cada parte a Jev, código, un LLM o una persona
- Un borrador de preguntas Jev en JSON con un ejemplo de state
- Una vista previa del documento final (formulario de aprobación, registro, libro), con cada campo marcado [J] Jev, [C] código, [원] texto original, [사람] persona
- Casos públicos parecidos con cifras, puntos donde suele fallar y cómo evitarlo
- Un plan de pruebas: tamaño de muestra, respuestas fijadas, procedimiento dev/holdout para el umbral

## Diseños de ejemplo

Incluimos tres diseños que golla-jev produjo de verdad. Lee uno primero para ver en qué se convierte un párrafo de descripción (el texto está en coreano).

- [Recibos → informe de gastos](examples/01-expense-report.md) — modo diseño
- [Buzón compartido → tablero kanban e informe semanal](examples/02-mail-board.md) — modo diseño
- [Preselección de influencers](examples/03-influencer-screening.md) — de modo idea a modo diseño

## Plantilla de benchmark incluida

`skills/golla-jev/bench/` contiene un benchmark de 108 ítems sobre 5 tareas de oficina en coreano (partida de gasto, tablero de correo, actas de reunión, encuesta, cláusulas de contrato). Los ítems están escritos a mano, no son datos de clientes. La ejecución del autor del 2026-11-13 con jev-1.14.0 acertó el 96,3 % (208/216) con preguntas en coreano y el 93,5 % en inglés. Un umbral de 0,70 elegido en el conjunto dev procesó automáticamente el 97 % (57/59) del holdout sin errores entre ellos. Un mes antes, jev-1.13.0 dio 0,85 con el mismo procedimiento, así que vuelve a elegir el umbral cada vez que cambie el modelo (`bench/REPORT.md`).

```
cp -r <ruta del plugin>/skills/golla-jev/bench ./my-bench && cd my-bench
python3 build_items.py          # tras adaptar el diccionario WF a tu tarea
python3 run.py                  # necesita TYPESAFE_API_KEY (no escribas el valor en la línea de comandos)
python3 analyze.py && python3 sweep.py
```

## Requisitos

- Claude Code
- Para ejecutar las preguntas del borrador necesitas una clave de API de TypeSafe (de pago, https://typesafe.ai). Para el diseño no hace falta clave.
- Recomendado: el plugin oficial `typesafe:typesafe-ai`. golla-jev comprueba allí y en docs.typesafe.ai los datos que cambian, como IDs de modelo, precios y límites, en lugar de fiarse de la memoria.
- Benchmark: Python 3.9+, `certifi` opcional

## Lo que no hace

- No se queda en "Jev encaja / no encaja". Lo que no encaja pasa a otro responsable para completar el flujo.
- No recomienda el umbral de otros. El umbral sale de tu propia muestra.
- No le pide a Jev que escriba texto, haga cuentas ni cuente fechas.

## Licencia

MIT. Proyecto independiente, sin relación con TypeSafe. Consulta [DISCLAIMER](DISCLAIMER.md).
