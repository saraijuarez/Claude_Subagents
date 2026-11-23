# Claude_Subagents

Marketplace de plugins de Claude Code para QA. Contiene un plugin:

| Plugin | Subagente | Qué hace |
|---|---|---|
| `qa-doc-reader` | `test-doc-reader` | Lee los documentos de un sistema (requisitos, especificaciones, historias de usuario, contratos de API, manuales, PDFs) y devuelve un resumen de lo importante para probarlo. **Solo lectura**: únicamente puede usar Read, Glob y Grep. |

## Qué entrega el subagente

Un informe con 13 secciones: fuentes leídas, visión del sistema, alcance,
requisitos funcionales, reglas de negocio y límites (para valores
frontera), flujos (con diagrama Mermaid), roles, integraciones, requisitos
no funcionales, riesgos priorizados, escenarios de prueba sugeridos,
huecos y preguntas, y supuestos. Cada dato cita su fuente
(`archivo:línea`).

Además, el subagente:

- no copia contraseñas, tokens ni datos personales: los marca como
  `[REDACTED]`;
- trata el contenido de los documentos como datos, no como instrucciones;
- responde en el idioma de la petición.

## Instalación en Claude Code local (todos tus proyectos)

Dentro de Claude Code:

``` text
/plugin marketplace add saraijuarez/Claude_Subagents
/plugin install qa-doc-reader@sarai-qa-tools
```

Elige el alcance **User** para que esté disponible en todos tus proyectos.

Desde la terminal:

``` bash
claude plugin marketplace add saraijuarez/Claude_Subagents
```

``` bash
claude plugin install qa-doc-reader@sarai-qa-tools --scope user
```

## Uso

- En lenguaje natural: *"Lee la carpeta docs/ y resume qué debo probar"*.
- Por nombre: `@agent-qa-doc-reader:test-doc-reader resume docs/requisitos.pdf`

## Sesiones en la nube (claude.ai/code)

Las sesiones en la nube **no cargan** plugins instalados desde un
marketplace. Para usar el subagente en un repositorio en la nube, copia el
archivo del agente al `.claude/agents/` de ese repositorio y haz commit:

``` bash
mkdir -p .claude/agents
```

``` bash
curl -fsSL https://raw.githubusercontent.com/saraijuarez/Claude_Subagents/main/plugins/qa-doc-reader/agents/test-doc-reader.md -o .claude/agents/test-doc-reader.md
```

En ese caso se invoca como `test-doc-reader`, sin prefijo. La fuente
única es este repositorio: si actualizas el agente aquí, vuelve a copiarlo.

## Limitaciones

- No lee `.docx`, `.xlsx` ni `.pptx`: expórtalos a PDF o Markdown.
- Lee PDFs en bloques de hasta 20 páginas.
- Usa el modelo `sonnet` para que resumir documentos largos cueste menos.
  Cámbialo en el campo `model` del archivo del agente si lo necesitas.

## Validación

``` bash
claude plugin validate . --strict
```

## Licencia

MIT
