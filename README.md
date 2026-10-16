# 🧠 Cómo montar tu agente de IA personal

> Guía práctica para pasar de *"un chatbot en una pestaña"* a tener un **asistente con
> personalidad, memoria y voz** que te habla por Discord (o Telegram, WhatsApp, Slack…).
>
> Está basada en mi setup real, con **OpenClaw**. Al final tienes **plantillas listas para copiar**.

---

## Índice

1. [Qué es (y qué no es) un agente personal](#1-qué-es-y-qué-no-es-un-agente-personal)
2. [El motor: OpenClaw](#2-el-motor-openclaw)
3. [Instalación en 5 minutos](#3-instalación-en-5-minutos)
4. [Conéctalo a tus chats](#4-conéctalo-a-tus-chats)
5. [La personalidad: `SOUL.md`](#5-la-personalidad-soulmd)
6. [La memoria: que no se olvide de ti](#6-la-memoria-que-no-se-olvide-de-ti)
7. [Las skills: dale superpoderes](#7-las-skills-dale-superpoderes)
8. [La voz: que hable y te escuche](#8-la-voz-que-hable-y-te-escuche)
9. [Proactividad: que tome la iniciativa](#9-proactividad-que-tome-la-iniciativa)
10. [Seguridad y secretos](#10-seguridad-y-secretos)
11. [Coste y tokens](#11-coste-y-tokens)
12. [Checklist final](#12-checklist-final)

---

## 1. Qué es (y qué no es) un agente personal

Un **agente** no es un chat al que le preguntas cosas. Es un programa que:

- **vive en tus canales** (le escribes por Discord/Telegram como a un amigo),
- **tiene herramientas** (leer/escribir archivos, ejecutar comandos, buscar en la web, mandar mensajes),
- **recuerda** entre conversaciones,
- y tiene **personalidad** propia.

La diferencia clave: un chatbot *responde*; un agente **actúa y recuerda**.

## 2. El motor: OpenClaw

[OpenClaw](https://docs.openclaw.ai) es un **gateway self-hosted** que conecta tus apps de
mensajería con un agente de IA. Tú corres **un proceso** en tu máquina (o un servidor) y se
convierte en el puente entre tus chats y tu asistente.

- **Self-hosted:** tus datos, tus reglas.
- **Multi-canal:** un solo gateway sirve Discord, Telegram, WhatsApp, Signal, Slack…
- **Agent-native:** sesiones, memoria, herramientas y enrutado multi-agente.

Es open source y no dependes de ningún servicio alojado.

## 3. Instalación en 5 minutos

```bash
# Requiere Node 22.22.3+ (o 24+)
npm i -g openclaw
openclaw onboard      # asistente de configuración guiado
openclaw gateway      # levanta el Gateway
```

Necesitas una **API key** de tu proveedor de modelos (OpenAI, Anthropic, Google…) o un
proveedor local. Durante el onboarding eliges modelo y canales.

El agente trabaja sobre un **workspace** (por defecto `~/.openclaw/workspace`) lleno de
archivos Markdown. **Esos archivos son su cerebro.** Los vemos ahora.

## 4. Conéctalo a tus chats

En el onboarding (o en la config) conectas los canales. Ejemplo con Discord:

1. Crea una app/bot en el Discord Developer Portal y copia el **token**.
2. Mete el token en la config de OpenClaw.
3. Invita el bot a tu servidor.
4. Escríbele por DM → ya te responde.

A partir de ahí tienes a tu agente **en el bolsillo**: le hablas desde el móvil y trabaja en
tu servidor.

## 5. La personalidad: `SOUL.md`

Aquí es donde gana o pierde el encanto. `SOUL.md` es la **voz** de tu agente y OpenClaw lo
inyecta en cada sesión. Si tu agente suena soso, corporativo o te da largas, este es el archivo.

**Qué va aquí:** tono, opiniones, brevedad, humor, límites, nivel de franqueza.
**Qué NO va aquí:** tu biografía, changelogs o muros de texto sin efecto real.

Reglas que funcionan:

- **Que tenga opinión.** Nada de *"depende"* para todo. Que se moje.
- **Cero relleno corporativo.** Si una frase podría estar en un manual de empresa, fuera.
- **Brevedad obligatoria.** Si cabe en una línea, una línea.
- **Humor cuando toca**, natural, no forzado.
- **Que te pare los pies.** Si vas a hacer algo mal, que te lo diga.

> Plantilla lista: [`plantillas/SOUL.md`](plantillas/SOUL.md)

## 6. La memoria: que no se olvide de ti

La memoria de OpenClaw son **archivos Markdown**, no magia oculta. El modelo solo recuerda lo
que se escribe a disco. Tienes tres niveles:

| Archivo | Para qué |
|---|---|
| `MEMORY.md` | **Largo plazo.** Datos duraderos, preferencias, decisiones. Se carga al empezar. |
| `memory/AAAA-MM-DD.md` | **Notas diarias.** Contexto y observaciones del día. |
| `DREAMS.md` *(opcional)* | Resúmenes para revisión humana. |

Regla de oro: **si quieres que lo recuerde, que lo escriba.** Solo con decirle
*"acuérdate de que prefiero TypeScript"* lo guarda donde toca.

Con el tiempo, el agente **destila** las notas diarias en `MEMORY.md` y limpia lo que ya no sirve.

> Plantillas: [`plantillas/MEMORY.md`](plantillas/MEMORY.md) · [`plantillas/USER.md`](plantillas/USER.md)

## 7. Las skills: dale superpoderes

Una **skill** es un procedimiento reutilizable que tu agente aprende a seguir (cómo desplegar
un servicio, cómo transcribir un audio, cómo publicar un repo…). Se escribe como un `SKILL.md`
con instrucciones claras.

```
workspace/
└─ skills/
   └─ mi-skill/
      └─ SKILL.md
```

Cuanto más específica, mejor. El `SKILL.md` debe decir **qué hace**, **cuándo usarla** y **los pasos exactos**.

> Plantilla: [`plantillas/skill/SKILL.md`](plantillas/skill/SKILL.md)

## 8. La voz: que hable y te escuche

- **Escuchar (STT):** transcribe notas de voz en local con **faster-whisper** (privado, gratis).
- **Hablar (TTS):** conecta un TTS (p. ej. ElevenLabs) para que te conteste con voz, ideal para
  resúmenes y "storytime".
- **Voz a voz:** envíale un audio y respóndete con otro.

Todo esto van como *skills* que tu agente ejecuta cuando le mandas un audio.

## 9. Proactividad: que tome la iniciativa

Un buen agente no espera a que le escribas. Tres mecanismos:

- **Heartbeat:** una llamada periódica (p. ej. cada 30 min) donde revisa email, calendario,
  notificaciones… y solo te habla si hay algo importante. Un checklist en `HEARTBEAT.md`.
- **Cron:** tareas con horario exacto ("recuérdame esto a las 9:00", "resumen diario a las 20:00").
  Puede avisarte solo cuando se cumple la hora.
- **Reglas de aviso:** "avísame si el disco pasa del 90%" o "si hay un email de X".

La clave: **proactivo, no pesado.** Que avise cuando aporta, y calle cuando no.

> Plantilla: [`plantillas/HEARTBEAT.md`](plantillas/HEARTBEAT.md)

## 10. Seguridad y secretos

Reglas básicas (y no negociables):

- 🔑 **Los tokens y contraseñas nunca se comparten.** Ni en canales, ni en grupos, ni en repos.
- 📁 Si publicas un proyecto, **fuera `.env` y secretos** antes de subirlo (revisa el `.gitignore`).
- 🚦 **Externo = pídele permiso.** Mandar emails, publicar, tocar APIs de terceros → confirmar antes.
- 🏠 Interno = puede moverse con libertad: leer, organizar, aprender.
- 👀 **Ojo en grupos.** El agente no es tu voz; no hables por tu humano.

## 11. Coste y tokens

Los agentes queman tokens. Trucos que funcionan:

- **Caché = tu amigo.** Las sesiones largas aprovechan el *prompt cache* y salen mucho más baratas
  que reiniciar cada rato.
- **Contexto comprimido.** Archivos de contexto cortos = menos tokens por turno.
- **Modelo según la tarea.** Uno potente para pensar, uno barato para lo mecánico.
- **Sin narración innecesaria.** Respuestas cortas = menos tokens de salida (que son los caros).

## 12. Checklist final

- [ ] Gateway levantado y escuchando.
- [ ] Canal conectado (Discord/Telegram/…) y probado.
- [ ] `SOUL.md` escrito con voz propia (que tenga carácter).
- [ ] `MEMORY.md` + notas diarias funcionando.
- [ ] 1–2 skills que de verdad uses.
- [ ] STT/TTS si quieres voz.
- [ ] Un cron o heartbeat útil.
- [ ] Secretos fuera del código.
- [ ] Y lo más importante: **itera**. Tu agente mejora cada vez que lo corriges.

---

## Plantillas incluidas

- [`plantillas/SOUL.md`](plantillas/SOUL.md) — la voz del agente
- [`plantillas/IDENTITY.md`](plantillas/IDENTITY.md) — quién es
- [`plantillas/USER.md`](plantillas/USER.md) — sobre ti (su humano)
- [`plantillas/MEMORY.md`](plantillas/MEMORY.md) — memoria a largo plazo
- [`plantillas/HEARTBEAT.md`](plantillas/HEARTBEAT.md) — checks periódicos
- [`plantillas/skill/SKILL.md`](plantillas/skill/SKILL.md) — una skill de ejemplo

---

## Licencia

MIT © 2026 Francesc Leon Chamizo. Úsala, remézclala y móntate el tuyo. 🦞
