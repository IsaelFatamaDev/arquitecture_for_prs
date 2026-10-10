# Estándares de arquitectura PRS

[FATAMA.md](./FATAMA.md) es el estándar base del proyecto. Las guías PRS resumen la arquitectura por producto y deben respetar sus convenciones. Aplica únicamente paquetes y componentes que el microservicio realmente necesite; no agregues estructuras opcionales como si fueran obligatorias.

## Guías

| Documento | Producto | Arquitectura de referencia | Raíz Java |
|---|---|---|---|
| [PRS1 · Cañete Seguro](./PRS1-ARQUITECTURA.md) | Cañete Seguro | Hexagonal | `pe.edu.vallegrande.caneteseguro.<contexto>` |
| [PRS2 · Vía Academy](./PRS2-ARQUITECTURA.md) | Vía Academy | Hexagonal | `pe.edu.vallegrande.viaacademy.<contexto>` |
| [PRS3 · FideiNexus](./PRS3-ARQUITECTURA.md) | FideiNexus | Hexagonal | `pe.edu.vallegrande.fideinexus.<contexto>` |
| [PRS4 · SIGRC](./PRS4-ARQUITECTURA.md) | SIGRC | Hexagonal, con aislamiento por organización cuando corresponda | `pe.edu.vallegrande.sigrc.<contexto>` |
| [PRS5 · EduNova](./PRS5-ARQUITECTURA.md) | EduNova | Hexagonal | `pe.edu.vallegrande.edunova.<contexto>` |

## Convenciones compartidas derivadas de FATAMA.md

- Mantén cada servicio como una unidad desplegable y configura su propia aplicación, perfiles y dependencias.
- Usa nombres Java en `PascalCase` para tipos y paquetes en minúsculas. `<contexto>`, `<Aggregate>` y `<Remote>` son marcadores que se reemplazan por nombres reales.
- `record` es una opción para datos inmutables, no una obligación ni un paquete. Úsalo para DTOs y value objects sencillos si la versión de Java del proyecto lo permite; usa una clase cuando necesites mutabilidad, herencia o comportamiento apropiado.
- No crees una carpeta `records`. Los modelos de persistencia pertenecen a `infrastructure/persistence`; usa sufijo `Entity` para SQL o `Document` para MongoDB.
- Elige el adaptador de persistencia según la base que use el servicio. No mantengas los dos árboles por defecto.
- Sigue el árbol de FATAMA.md: `infrastructure/exception/custom/` para excepciones propias solo cuando hagan falta, y `infrastructure/exception/` para `GlobalExceptionHandler` y handlers REST/validación necesarios. No se requiere una excepción por capa ni un handler por excepción.
- Agrega DTOs, mappers y excepciones cuando el servicio tenga una frontera o conversión que los requiera; evita paquetes vacíos y capas artificiales.
- Los mappers convierten entre modelos cuando las representaciones difieren. No agregues un mapper vacío si las clases son compatibles y no hay transformación.
- La configuración y la seguridad dependen del runtime, por eso pertenecen a infraestructura. Nunca pongas credenciales reales en el repositorio; documenta variables en `.env.example`.
- Tenant/multitenancy es opcional y solo se incorpora cuando hay aislamiento real por organización. No agregues `TenantContext` por defecto.
- El gateway, cuando el producto lo necesite, es un componente independiente: enruta y aplica políticas transversales; no contiene dominio, casos de uso ni persistencia de los microservicios. Aplica la sección Gateway común de cada PRS.

## Cómo aplicar una guía

1. Identifica producto, contexto de negocio, runtime, base de datos y servicios externos.
2. Usa la arquitectura hexagonal en todos los productos PRS y adapta el árbol del documento a las dependencias reales del servicio.
3. Sustituye los marcadores y elimina componentes que el servicio no necesite.
4. Verifica que las dependencias sigan el sentido descrito en la guía y que el README del microservicio documente cómo compilar, configurar y ejecutar el servicio.

Las guías describen organización de código y no sustituyen decisiones de despliegue, seguridad o contratos de API específicos del servicio.
