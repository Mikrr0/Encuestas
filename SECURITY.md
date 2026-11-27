# Configuración de credenciales

El proyecto obtiene las credenciales desde las variables de entorno del proceso.
No se deben guardar contraseñas, tokens ni claves en archivos versionados.

Configura `DJANGO_SECRET_KEY`, `DB_NAME`, `DB_USER` y `DB_PASSWORD` con valores
propios del entorno. `DB_HOST` y `DB_PORT` permiten configurar la conexión a
PostgreSQL. Revisa los nombres utilizados en `encuestas/settings.py` de tu rama.

Las ramas que utilizan Gmail necesitan `EMAIL_HOST_USER` y
`EMAIL_HOST_PASSWORD`. Para Gmail utiliza una contraseña de aplicación cuando
corresponda. Configura también `EMAIL_BACKEND` según el entorno y comprueba el
envío de mensajes antes de usarlo en producción.

En `main`, la carga masiva de usuarios utiliza `INITIAL_USER_PASSWORD` para los
dos campos de confirmación del formulario. Define un valor nuevo y robusto en
el entorno del proceso antes de utilizar esa función. El flujo de carga masiva
se conserva; ya no incluye una contraseña fija en el código. Los usuarios
creados deben cambiar su contraseña temporal mediante el flujo existente.

Los archivos `.env` y el bytecode de Python están ignorados por Git. Django no
carga automáticamente un `.env`: el servicio de despliegue o una herramienta
local debe inyectar las variables en el proceso. Nunca publiques sus valores.

## Historial saneado

El historial de las seis ramas fue reescrito con `git-filter-repo` y
`--sensitive-data-removal`. Los SHA anteriores cambian; se preservan los commits,
la autoría y los merges. Se retira el bytecode histórico de configuración que
contenía credenciales.

Después de la limpieza:

- Rota la clave de Django, las contraseñas de PostgreSQL y de Gmail, y las
  contraseñas temporales de usuarios que procedan de los valores anteriores.
- Clona nuevamente el repositorio. Recupera trabajo local mediante parches
  revisados, sin fusionar ni volver a publicar el historial antiguo.
- Revisa otras copias, forks, artefactos y respaldos que puedan contener datos
  anteriores. La reescritura de ramas no elimina esas copias.
- Si GitHub conserva vistas de commits antiguos o referencias internas de PR,
  consulta a GitHub Support sobre su eliminación y la recolección de objetos.

La eliminación del historial no revoca las credenciales anteriores. La rotación
se realiza en cada proveedor o sistema y debe hacerse por separado.
