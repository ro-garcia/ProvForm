# Flujo Power Automate para formularios CSC S.A.

## 1. Lista de SharePoint sugerida

Crear una lista, por ejemplo `Solicitudes Datos Maestros`, con estas columnas:

- `Title` (texto): identificador corto de la solicitud.
- `Formulario` (texto): proveedor o cliente.
- `Empresa solicitante` (texto).
- `Tipo de solicitud` (texto).
- `Solicitado por` (texto).
- `Correo del solicitante` (texto).
- `Departamento solicitante` (texto).
- `Fecha de solicitud` (fecha).
- `Payload JSON` (varias líneas de texto).
- `Destinatarios` (texto).
- `Estado` (texto).

## 2. Trigger HTTP

Crear un flujo instantáneo o automatizado con el trigger:

`When an HTTP request is received`

En `Request Body JSON Schema`, usar `Use sample payload to generate schema` y pegar el contenido de:

`power-automate-sample-payload.json`

Si el flujo ya existia antes de agregar `empresa_codigo`, actualizar el esquema del trigger HTTP para que aparezca este campo en contenido dinamico.

Guardar el flujo para que Power Automate genere la URL HTTP POST.

## 3. Variables del flujo

Agregar estas variables tipo `String`:

- `aprobador`
- `destinatarios`

`aprobador` es quien aprueba o rechaza primero.
`destinatarios` es quien recibe el correo final despues de la aprobacion.

## 4. Enrutamiento del aprobador

El formulario envia `empresa_codigo` junto con el nombre completo de la empresa.

Agregar un `Modificador` / `Switch` usando:

```text
triggerBody()?['empresa_codigo']
```

Casos de unidades de negocio:

- `RX` -> `aprobador`: correo de [responsable configurado en el flujo]
- `EMA` -> `aprobador`: correo de [responsable configurado en el flujo]
- `CI` -> `aprobador`: correo de [responsable configurado en el flujo]
- `IMC` -> `aprobador`: correo de [responsable configurado en el flujo]
- `GM` -> `aprobador`: correo de [responsable configurado en el flujo]
- `TP` -> `aprobador`: correo de [responsable configurado en el flujo]
- `PROMARES` -> `aprobador`: correo de [responsable configurado en el flujo]
- `OTRO` -> `aprobador`: correo de [responsable configurado en el flujo]

En cada caso usar `Establecer variable` y asignar la variable `aprobador`.

### Caso CSC

En el caso `CSC`, agregar otro `Modificador` / `Switch` usando:

```text
triggerBody()?['datos']?['departamento_solicitante']
```

Si el departamento esta a nivel principal del JSON, usar:

```text
triggerBody()?['departamento_solicitante']
```

Casos CSC:

- `ABASTECIMIENTO` -> `aprobador`: correo de [responsable configurado en el flujo]
- `IT` -> `aprobador`: correo de [responsable configurado en el flujo]
- `FINANZAS` -> `aprobador`: correo de [responsable configurado en el flujo]
- `AUDITORIA INTERNA` -> `aprobador`: correo de [responsable configurado en el flujo]
- `TALENTO HUMANO` -> `aprobador`: correo de [responsable configurado en el flujo]
- `LEGAL` -> `aprobador`: correo de [responsable configurado en el flujo]
- `CASS` -> `aprobador`: correo de [responsable configurado en el flujo]

En cada caso usar `Establecer variable` y asignar la variable `aprobador`.

> Para pruebas, se puede asignar temporalmente `usuario@example.com` en todos los casos y tambien en `OTRO`.

<!--
Metodo anterior descartado:
Se puede enrutar comparando `empresa_solicitante` completo, pero es mas sensible a espacios y caracteres ocultos.
-->

<!--
Agregar una `Condicion` antes de crear la aprobacion:

Campo izquierdo:

```text
triggerBody()?['empresa_solicitante']
```

Operador:

```text
es igual a
```

Campo derecho:

```text
CENTRO DE SERVICIOS COMPARTIDOS S.A.
```

### Si la condicion es True: empresa CSC

Agregar un `Modificador` / `Switch` usando:

```text
triggerBody()?['departamento_solicitante']
```

Casos CSC:

- `ABASTECIMIENTO` -> `aprobador`: correo de [responsable configurado en el flujo]
- `IT` -> `aprobador`: correo de [responsable configurado en el flujo]
- `FINANZAS` -> `aprobador`: correo de [responsable configurado en el flujo]
- `AUDITORIA INTERNA` -> `aprobador`: correo de [responsable configurado en el flujo]
- `TALENTO HUMANO` -> `aprobador`: correo de [responsable configurado en el flujo]
- `LEGAL` -> `aprobador`: correo de [responsable configurado en el flujo]
- `CASS` -> `aprobador`: correo de [responsable configurado en el flujo]

En cada caso usar `Establecer variable` y asignar la variable `aprobador`.

### Si la condicion es False: unidad de negocio

Agregar un `Modificador` / `Switch` usando:

```text
triggerBody()?['empresa_solicitante']
```

Casos de unidades de negocio:

- `REPIMEX S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]
- `EMAPAC S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]
- `CENTRANS INTERNACIONAL S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]
- `IMC S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]
- `GUATEMAR S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]
- `TALPORT S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]
- `PROMARES S.A.` -> `aprobador`: correo de [responsable configurado en el flujo]

En cada caso usar `Establecer variable` y asignar la variable `aprobador`.

> Importante: los casos deben coincidir exactamente con lo que envia el formulario. Por eso `CENTRANS` debe configurarse como `CENTRANS INTERNACIONAL S.A.`.
-->

## 5. Destinatario posterior a la aprobacion

Despues de definir `aprobador`, asignar tambien la variable `destinatarios`.

Para pruebas usa un correo de prueba. Al terminar, asigna el correo del destinatario final de la solicitud.

Ejemplo:

```text
destinatarios = usuario@example.com
```

Si el destinatario final tambien cambia por empresa, puedes asignarlo dentro de cada caso del mismo `Modificador`.

## 6. Crear item en SharePoint

Agregar acción `Create item` de SharePoint.

Mapeo sugerido:

- `Title`: `tipo_solicitud - empresa_solicitante`
- `Formulario`: `formulario`
- `Empresa solicitante`: `empresa_solicitante`
- `Departamento solicitante`: `departamento_solicitante`
- `Tipo de solicitud`: `tipo_solicitud`
- `Solicitado por`: `solicitado_por`
- `Correo del solicitante`: `correo_solicitante`
- `Payload JSON`: usar el cuerpo completo recibido por HTTP.
- `Destinatarios`: variable `destinatarios`
- `Estado`: `Pendiente de aprobacion`

## 7. Correo de aprobación con opciones

En lugar de usar `Iniciar y esperar una aprobacion`, agregar accion de Office 365 Outlook:

`Enviar correo electronico con opciones`

Configurar:

- `Para`: `variables('aprobador')`
- `Asunto`: `Solicitud pendiente de aprobacion - empresa_solicitante`
- `Opciones de usuario`: `Aprobar,Rechazar`
- `Cuerpo`: plantilla HTML o texto con nombre del proveedor/cliente, NIT y nombre del solicitante.

Ejemplo de cuerpo simple:

```text
concat(
'Solicitud pendiente de aprobacion', decodeUriComponent('%0A%0A'),
'Nombre del proveedor: ', triggerBody()?['datos']?['nombre_comercial'], decodeUriComponent('%0A'),
'NIT / Identificador fiscal: ', triggerBody()?['datos']?['nit_identificador_fiscal'], decodeUriComponent('%0A'),
'Nombre del solicitante: ', triggerBody()?['solicitado_por'], decodeUriComponent('%0A%0A'),
'Favor revisar la informacion antes de aprobar o rechazar la solicitud.'
)
```

Despues de esta accion, agregar una `Condicion`:

```text
SelectedOption es igual a Aprobar
```

En `True`, evaluar el tipo de solicitud. Si es creacion, generar el documento Word y enviarlo. Si es modificacion o inactivacion, enviar solo correo.
En `False`, enviar correo al solicitante indicando que fue rechazado o finalizar el flujo.

## 8. Decidir si genera Word o solo correo

Para llenar un documento automaticamente, la plantilla Word debe estar guardada en SharePoint o OneDrive y debe tener `Controles de contenido` de Word. Un documento visual normal no basta para la accion de Word Online.

Preparar una plantilla por formulario:

- `Plantilla_Proveedor_PowerAutomate.docx`
- `Plantilla_Cliente_PowerAutomate.docx`
- `Plantilla_Caja_Chica.docx` cuando se defina ese proceso.

En Word, activar la pestana `Programador` / `Developer`, insertar `Plain Text Content Control` donde deba ir cada dato y en `Propiedades` colocar el `Titulo` o `Tag` con nombres como:

- `empresa_solicitante`
- `departamento_solicitante`
- `solicitado_por`
- `correo_solicitante`
- `fecha_solicitud`
- `nombre_comercial`
- `razon_social`
- `nit_identificador_fiscal`
- `grupo`
- `tipo_solicitud`

En Power Automate, dentro de la rama `True` de aprobacion:

1. Agregar una `Condicion` para revisar si la solicitud es de creacion.

Campo izquierdo:

```text
triggerBody()?['tipo_solicitud']
```

Operador:

```text
es igual a
```

Campo derecho:

```text
creacion
```

2. En la rama `True` de esa condicion se genera el Word.

3. En la rama `False` se envia solo correo final, sin crear documento.

Para `proveedor`, los casos quedan asi:

- `creacion` -> llena `Plantilla_Proveedor_PowerAutomate.docx`, crea archivo y lo adjunta.
- `modificacion` -> solo correo.
- `inactivacion` -> solo correo.

Para `cliente`, los casos quedan asi:

- `creacion` -> llena `Plantilla_Cliente_PowerAutomate.docx`, crea archivo y lo adjunta.
- `modificacion` -> solo correo.
- `inactivacion` -> solo correo.

## 9. Generar documento Word cuando es creacion

Dentro de la rama `True` de `tipo_solicitud = creacion`, agregar un `Modificador` usando:

```text
triggerBody()?['formulario']
```

Crear casos:

- `proveedor`
- `cliente`
- `caja_chica`

Dentro de `proveedor`, agregar la accion de Word Online:

`Rellenar una plantilla de Microsoft Word`

Seleccionar:

```text
Plantilla_Proveedor_PowerAutomate.docx
```

Dentro de `cliente`, agregar la misma accion de Word Online y seleccionar:

```text
Plantilla_Cliente_PowerAutomate.docx
```

Cuando se seleccione cada plantilla, Power Automate mostrara sus campos.

Mapear campos principales con valores como:

```text
empresa_solicitante = triggerBody()?['empresa_solicitante']
empresa_codigo = triggerBody()?['empresa_codigo']
departamento_solicitante = triggerBody()?['datos']?['departamento_solicitante']
solicitado_por = triggerBody()?['solicitado_por']
correo_solicitante = triggerBody()?['correo_solicitante']
fecha_solicitud = triggerBody()?['datos']?['fecha_solicitud']
tipo_solicitud = triggerBody()?['tipo_solicitud']
nombre_comercial = triggerBody()?['datos']?['nombre_comercial']
razon_social = triggerBody()?['datos']?['razon_social']
nit_identificador_fiscal = triggerBody()?['datos']?['nit_identificador_fiscal']
grupo = triggerBody()?['datos']?['grupo']
```

Para campos que puedan venir vacios, usar `coalesce`, por ejemplo:

```text
coalesce(triggerBody()?['datos']?['departamento_solicitante'], '')
```

Despues agregar `Crear archivo` en SharePoint o OneDrive.

Nombre sugerido:

```text
Solicitud_@{triggerBody()?['empresa_codigo']}_@{triggerBody()?['datos']?['nit_identificador_fiscal']}.docx
```

Contenido del archivo:

```text
Documento generado por la accion Rellenar una plantilla de Microsoft Word
```

Agregar `Obtener contenido del archivo` usando el `body/Id` del archivo creado.

## 10. Correo final despues de aprobar

Agregar acción `Send an email (V2)` de Office 365 Outlook.

Para solicitudes de `creacion`, enviarlo despues de crear el documento y adjuntar el Word.

Para solicitudes de `modificacion` o `inactivacion`, enviar el correo sin adjunto.

- `To`: variable `destinatarios`
- `Subject`: `Solicitud aprobada - empresa_solicitante`
- `Body`: usar la plantilla HTML del correo final.
- `Attachments Name`: nombre del archivo Word creado.
- `Attachments Content`: contenido obtenido con `Obtener contenido del archivo`.

Opcionalmente, despues del envio final, actualizar el item de SharePoint:

- `Estado`: `Aprobado y enviado`
- `Link documento`: enlace al archivo creado

## 11. Respuesta HTTP

Agregar acción `Response`:

- `Status code`: `200`
- `Body`:

```json
{
  "ok": true,
  "message": "Solicitud recibida"
}
```

## 12. Conectar el HTML

Cuando tengas la URL del trigger HTTP, pegarla en `index.html`:

```js
const POWER_AUTOMATE_ENDPOINT = "PEGAR_AQUI_LA_URL_DEL_TRIGGER_HTTP";
```
