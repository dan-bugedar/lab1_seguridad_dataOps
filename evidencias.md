# Evidencias del laboratorio 1

Participante: Daniela Bugeda Reyes  Fecha: 23 octubre 2026

Registra lo que observaste realmente. No pegues claves, tokens ni datos recuperados.

| Prueba | Resultado esperado | Resultado observado | Explicación o corrección |
| --- | --- | --- | --- |
| Recuperar correo en 1A | True | True | Se cifro el correo y se pudo recuperar correctamente ya que el texto obtenido coincide con el original.|
| Comparar original | True | True | El hash SHA-256 del archivo original coincide con el hash de referencia previamente calculado. Esto indica que el archivo conserva su integridad y no presenta modificaciones respecto a la referencia. |
| Comparar copia alterada | False | False | El hash de la copia alterada no coincide con el hash de referencia, ya que se modificó el contenido del archivo. Esto demuestra que el control de integridad permite detectar modificaciones |
| Ejecutar como operador | 12 filas | 12 filas | La operación se ejecutó correctamente porque el rol operador tiene permiso para ejecutar el pipeline. Se procesaron los 12 registros y se generaron los archivos de salida.|
| Consultar como analista | 3 ciudades y total 10750.00 | 3 ciudades y total 10750.00 | El resumen agrupó las ventas por ciudad y calculó los totales correctamente. Se excluyeron los pedidos cancelados y el total de control fue 10750.00.|
| Recuperar como operador | Muestra ficticia coincidente | Muestra ficticia coincidente | La operación fue autorizada para el rol operador y permitió recuperar la muestra ficticia esperada|
| Ejecutar como analista | Bloqueado sin destino nuevo | BLOQUEADO: Rol u operación no autorizados |La operación fue denegada porque el rol analista solo tiene permiso para consultar el resumen. No se creó el destino indicado, evitando una ejecución no autorizada. |
| Ejecutar con rol desconocido | Bloqueado sin destino nuevo | BLOQUEADO: Rol u operación no autorizados | El sistema rechazó la operación porque el rol no está definido en la lista de permisos. La denegación por defecto impide que perfiles desconocidos ejecuten el pipeline.|
| Recuperar como auditor | Bloqueado | BLOQUEADO: Rol u operación no autorizados | La recuperación fue denegada porque el rol auditor no tiene permiso para recuperar información. Esto limita el acceso a datos protegidos según el principio de mínimo privilegio.|
| Procesar copia alterada | Bloqueado sin salidas/alterada | BLOQUEADO: Rol u operación no autorizados |El sistema detectó que el archivo de entrada no coincide con la referencia de integridad y bloqueó el procesamiento. No se debe recalcular la referencia para aceptar el archivo modificado. |

## Archivos de salida

Destino válido utilizado: salidas/lab1

Filas de detalle: 12  Filas de resumen: 3

Totales observados por ciudad: Mérida: 4000.00; Puebla: 2500.00; Villahermosa: 4250.00

## Reflexión

1. ¿Qué protege cada control y qué prueba lo demuestra?

**Verificación de integridad:** compara el archivo original con una referencia previamente calculada para detectar modificaciones. Las pruebas Comparar original y Comparar copia alterada permiten comprobar si se detectan los cambios.



**Control de acceso por rol:** limita las operaciones que puede realizar cada perfil. Las pruebas de ejecución como operador, analista y auditor permiten comprobar si se respetan los permisos definidos.



**Validación del destino:** evita generar archivos en ubicaciones no autorizadas. Las pruebas con un rol no permitido o sin un destino válido comprueban si la ejecución se bloquea.



**Protección de la información:** limita la exposición de datos sensibles y evita que los resultados incluyan información que el usuario no está autorizado a consultar.

2. ¿Por qué no debes recalcular la referencia ante una alteración?

La referencia debe mantenerse independiente del archivo que se está verificando. Si el archivo se modifica y se recalcula su referencia utilizando la versión alterada, se estaría aceptando potencialmente la modificación como válida y se perdería la capacidad de detectar la alteración.

3. ¿Por qué --rol no equivale a autenticar una identidad?

El parámetro --rol únicamente indica qué rol solicita utilizar el programa; no demuestra quién está ejecutando el proceso.

4. ¿Qué riesgos quedan en el original, los identificadores y la clave?

**Archivo original:** puede contener datos sensibles y quedar expuesto por accesos indebidos, copias no controladas o almacenamiento inseguro.



**Identificadores:** si son predecibles, se comparten indebidamente o no se validan, podrían facilitar la asociación de registros con personas o el acceso a información.



**Clave:** si se almacena en texto plano, se incluye en el repositorio, aparece en los registros o se comparte accidentalmente, un tercero podría utilizarla para acceder a información o generar referencias fraudulentas.

5. ¿Qué añadirías para utilizar este enfoque en producción?

Para llevar este enfoque a producción, incorporaría los siguientes controles:
- Autenticación robusta, con usuarios identificables y, cuando corresponda, autenticación multifactor.
- Autorización centralizada, con permisos asociados a identidades verificadas y aplicando el principio de mínimo privilegio.
- Gestión segura de secretos, utilizando un gestor de claves y evitando almacenarlas en el código o en el repositorio.
- Trazabilidad y monitoreo, con registros de las operaciones, los accesos y los intentos fallidos, sin incluir secretos ni datos sensibles.
