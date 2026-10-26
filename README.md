# ProvForm

Formularios web en español para solicitudes de creación, modificación e inactivación de proveedores y clientes de CSC.

## Ejecutar localmente

1. Copia `config.example.js` como `config.local.js`.
2. En `config.local.js`, reemplaza `PEGAR_AQUI_LA_URL_DEL_TRIGGER_HTTP` con la URL del trigger HTTP de Power Automate.
3. Desde esta carpeta, inicia un servidor local:

   ```powershell
   python -m http.server 4173
   ```

4. Abre `http://localhost:4173` en el navegador.

La URL del trigger contiene una firma privada. `config.local.js` está excluido de Git; no compartas ni subas ese archivo. Sin esa configuración, el formulario no podrá enviar solicitudes al flujo.

## Contenido

- `index.html`: interfaz y lógica de los formularios.
- `assets/csc-logo.png`: identidad visual usada por la página.
- `plantillas_power_automate/`: plantillas Word de proveedor y cliente.
- `power-automate-flow.md`: notas de configuración del flujo.
- `power-automate-sample-payload.json`: ejemplo de estructura de solicitud.
- `guia_tecnica_para_guia_usuario.md`: referencia funcional para documentar los formularios.
- `tools/build_word_templates.py`: generador de las plantillas Word.

## Integración

El sitio envía la solicitud al endpoint configurado en `config.local.js`. La recepción, el guardado y las aprobaciones dependen de Power Automate y de la configuración de Microsoft 365.

Este repositorio contiene el código fuente. No se configuró GitHub Pages porque el formulario requiere una URL privada de Power Automate para enviar solicitudes.
