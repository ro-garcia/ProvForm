# Guía técnica de referencia para crear la guía de usuario

## Objetivo

Usa este documento como fuente para redactar una guía de uso en español, dirigida a las personas que llenan los formularios. Explica qué formulario elegir, qué tipo de solicitud seleccionar, qué datos completar y cómo enviar la solicitud.

Limita la guía a la experiencia visible del usuario: selección del formulario, tipos de solicitud, captura de datos, campos condicionales, validaciones y envío. No incluyas ni menciones Power Automate, flujos, variables, reglas de enrutamiento, aprobaciones ni procesamiento posterior.

## Formularios que deben documentarse

El selector presenta dos formularios activos:

- **Formulario alta de Proveedores**
- **Formulario alta de Clientes**

El formulario **Proveedor de caja chica** está temporalmente comentado y no aparece como opción activa. No lo incluyas en la guía.

## Datos de solicitud comunes

Ambos formularios comienzan con los mismos datos:

- **Empresa solicitante**: selección obligatoria. Las opciones son REPIMEX S.A., GUATEMAR S.A., IMC S.A., CENTRO DE SERVICIOS COMPARTIDOS S.A., CENTRANS INTERNACIONAL S.A., EMAPAC S.A., TALPORT S.A. y PROMARES S.A.
- **Departamento solicitante**: solo se muestra y se exige cuando la empresa elegida es CENTRO DE SERVICIOS COMPARTIDOS S.A. Sus opciones son Abastecimiento, IT, Finanzas, Auditoría Interna, Talento Humano, Legal y CASS.
- **Solicitado por**: nombre de la persona solicitante; obligatorio.
- **Correo del solicitante**: obligatorio.
- **Fecha de la solicitud**: se completa automáticamente y no se puede editar.

Si se cambia la empresa desde CSC a otra, el departamento deja de mostrarse y se limpia.

## Tipos de solicitud

En los dos formularios activos se elige una de estas opciones:

| Opción mostrada | Cuándo se utiliza |
|---|---|
| **Creación de proveedor / cliente** | Para solicitar el alta de una entidad nueva. |
| **Modificación de información existente** | Para solicitar cambios en la información de una entidad ya registrada. |
| **Inactivación de proveedor / cliente** | Para solicitar que una entidad existente se inactive. |

Después de elegir el tipo de solicitud aparecen los datos correspondientes. Las secciones se mantienen ocultas hasta seleccionar una opción.

## Creación y modificación

Para cada formulario, **Creación** y **Modificación** presentan los mismos campos, secciones y requisitos. La única diferencia de presentación es que **Modificación** muestra, debajo del título “Datos generales”, esta nota completa en negrita:

> **Importante: Debido a que todos los campos son obligatorios, en todos los campos a los que no aplique una modificación, escriba textualmente N/A.**

Al redactar la guía, indica que se debe escribir `N/A` en los campos que no apliquen a la modificación. No presentes la nota como un campo que se deba llenar.

En ambas solicitudes, los adjuntos mantienen los mismos requisitos que en Creación. Los campos identificados con asterisco son obligatorios. Las referencias comerciales #1 y #2 son obligatorias; la referencia #3 es opcional.

Los datos específicos varían según se elija el formulario de proveedores o de clientes. Para la documentación requerida, se muestran los archivos correspondientes al tipo de persona elegido (**Individual** o **Jurídica**).

## Inactivación

La solicitud conserva los datos comunes del solicitante y la selección del tipo de solicitud. En los datos del proveedor o cliente, solo se muestran estos cuatro campos y todos son obligatorios:

1. **Nombre comercial**
2. **Razón social**
3. **NIT / Identificador fiscal**
4. **Justificación** — caja de texto para explicar el motivo de la inactivación.

Las demás secciones de datos del proveedor o cliente y sus adjuntos no se muestran ni se solicitan para esta opción.

## Diferencias entre proveedores y clientes

La guía debe explicar ambos formularios por separado y conservar sus nombres y datos propios. Aplican las mismas reglas para empresa, departamento, tipos de solicitud, nota de modificación e inactivación. No supongas que las secciones particulares de un formulario también existen en el otro.

## Pasos de envío que debe explicar la guía

1. Elegir **Formulario alta de Proveedores** o **Formulario alta de Clientes** en el selector superior.
2. Completar empresa solicitante, departamento cuando aparezca, nombre y correo del solicitante. La fecha se llena automáticamente.
3. Seleccionar el tipo de solicitud.
4. Completar los campos visibles. En Creación y Modificación, escoger el tipo de persona y adjuntar los documentos requeridos que correspondan. En Modificación, seguir la nota de `N/A`.
5. Presionar **Enviar formulario**.
6. Si el formulario marca campos obligatorios incompletos, completarlos y volver a enviarlo.
7. Al aparecer el mensaje **Solicitud ingresada correctamente**, presionar **OK**. El formulario se restablece para una nueva solicitud.

## Criterios editoriales para el agente

- Escribe en español claro, con pasos breves y nombres de campos iguales a los de la pantalla.
- Distingue los datos del solicitante de los datos del proveedor o cliente.
- Explica que el departamento solo se solicita para CSC.
- Señala que Creación y Modificación usan los mismos campos; la nota de `N/A` solo aparece en Modificación.
- Explica por separado los cuatro campos de Inactivación.
- Incluye una tabla corta de tipos de solicitud y una lista numerada para enviar el formulario.
- No incluyas nombres de variables, códigos internos, reglas de enrutamiento ni detalles técnicos que el usuario final no necesita.
- No menciones Power Automate ni describas cómo se procesa la solicitud después de enviarla.
- No inventes campos ni requisitos que no estén descritos aquí o visibles en el formulario.
