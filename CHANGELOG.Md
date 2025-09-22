# Historial de Cambios (Changelog)

Todos los cambios notables en este proyecto serán documentados en este archivo.

El formato se basa en [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
y este proyecto se adhiere al [Versionamiento Semántico](https://semver.org/spec/v2.0.0.html).



[2.0.4] - 2025-09-21
Corregido (Fixed)
Crash Crítico en Desconexión: Se solucionó un error fundamental (NetworkOnMainThreadException) que causaba que la aplicación se cerrara inesperadamente en algunas versiones de Android. Este problema ocurría al detener la conexión VPN o al perder la señal de red. La lógica de desconexión ahora se ejecuta de forma segura en un hilo secundario, garantizando la estabilidad de la aplicación.

---

## [2.0.3] - 2025-09-21

### Seguridad (Security)

-   **Sistema de Autenticación HWID Renovado:** Se ha reemplazado por completo el sistema de autenticación estático para el modo HWID. La aplicación ahora genera una **contraseña dinámica de un solo uso** para cada intento de conexión. Esta contraseña está firmada criptográficamente con una clave secreta y tiene una validez de tiempo limitada, lo que previene ataques de repetición y el uso compartido no autorizado de credenciales.
-   **Cierre de Vulnerabilidad Crítica:** Se solucionó un agujero de seguridad que permitía a los usuarios conectarse en modo "Usuario/Contraseña" introduciendo el HWID de otro usuario. La aplicación ahora valida activamente y bloquea cualquier intento de usar un nombre de usuario que se parezca a un HWID en este modo.
-   **Protección de Claves y Endpoints:** La clave secreta para la firma HMAC y la URL del servidor de autenticación han sido movidas al código nativo (C++) y ofuscadas, protegiéndolas contra la ingeniería inversa y mejorando significativamente la seguridad general de la aplicación.

### Corregido (Fixed)

-   **Falso Estado de Conexión en V2Ray:** Se corrigió un error fundamental en el flujo de conexión V2Ray donde la interfaz de usuario podía mostrar el estado "Conectado" (verde) aunque la conexión real al servidor hubiera fallado. El estado final ahora solo se muestra después de una verificación exitosa de latencia y de la autorización del usuario.

### Mejorado (Improved)

-   **Flujo de Conexión de la UI:** La interfaz de usuario ahora es más reactiva. Al presionar el botón de conectar, el estado cambia inmediatamente a "Conectando" (naranja), proporcionando una respuesta instantánea al usuario mientras se realizan las verificaciones en segundo plano.
-   **Claridad de los Logs:** Se han limpiado y mejorado los mensajes en el registro de conexión. Se eliminaron los errores técnicos alarmantes (como "read/write on closed pipe") y se reemplazaron por mensajes claros y amigables.
-   **Banner de Bienvenida para V2Ray:** Al establecer una conexión V2Ray exitosa, ahora se muestra un banner de bienvenida personalizado en el log, creando una experiencia consistente con el modo de conexión SSH.

---

## [2.0.2] - 2025-09-20

### Corregido (Fixed)

-   **Persistencia del Ping Real:** Se solucionó un problema crucial donde la latencia real (ping) no se actualizaba en la tarjeta del servidor si la aplicación se abría después de haber conectado o si se cambiaba de pestaña. Ahora el valor se guarda de forma persistente y se restaura al volver a la pantalla principal, mostrando siempre el ping más reciente mientras la VPN esté activa.
-   **Actualización de UI en Ajustes:** Se corrigió un error visual en la pantalla de "Ajustes" donde la preferencia del "Pinger de Conexión" no actualizaba su valor en el resumen después de ser modificado. Ahora el cambio se refleja inmediatamente, mejorando la coherencia de la interfaz.

### Mejorado (Improved)

-   **Privacidad del Servidor:** Se ha mejorado la privacidad y seguridad al eliminar del registro de conexión la línea que mostraba el dominio del servidor (`CN=...`) durante el handshake SSL. Esto protege la infraestructura del proveedor de la VPN de ser expuesta en los logs del cliente.

### Mejoras Internas (Developer)

-   **Refactorización de Código:** Se ha refactorizado significativamente el código del gestor de túneles (`TunnelManagerThread.java`). La lógica para verificar la conexión a internet y para medir la latencia real ha sido encapsulada en funciones dedicadas (`verifyTunnelAndFinalizeConnection`, `triggerRealLatencyUpdate`), mejorando enormemente la legibilidad y el mantenimiento del código.
-   **Limpieza de Logs:** Se eliminó el código de depuración y los logs verbosos del sistema de activación del pinger, optimizando el rendimiento y limpiando la salida en Logcat para las versiones de producción.

---

## [2.0.1] - 2025-09-19

### Añadido (Added)

-   **Modo Persistente:** Se ha introducido una nueva opción en "Ajustes" para mantener la conexión activa. Si el sistema operativo cierra el servicio para ahorrar energía, este modo intentará reiniciarlo automáticamente para mantener la estabilidad de la conexión.
-   **Asistente de Optimización de Batería:** Al activar el "Modo Persistente", la aplicación ahora comprueba si la optimización de batería está desactivada. Si no lo está, se le sugiere al usuario ir a los ajustes del sistema para añadir la app a la lista de excepciones, garantizando el correcto funcionamiento del modo persistente. El mensaje solo aparece si es necesario.

### Cambiado (Changed)

-   **Guía de Ayuda Renovada:** Se ha reemplazado el simple diálogo de ayuda por una interfaz completamente nueva, moderna e interactiva. La guía ahora está organizada en tres pestañas con iconos:
    -   **Primeros Pasos:** Instrucciones básicas de conexión.
    -   **Modo Demo:** Explicación sobre cómo funciona el tiempo de prueba.
    -   **Obtener VIP:** Guía para copiar el HWID y activar el acceso premium.
-   **Diseño de Ajustes Mejorado:** La pantalla de "Ajustes" ha sido reorganizada en categorías lógicas ("Conexión", "Red y DNS", "Avanzado") para una navegación más clara e intuitiva. Se ha mejorado el espaciado y el estilo para una apariencia más profesional.

### Corregido (Fixed)

-   Se solucionó un error en Logcat causado por una clave de preferencia duplicada (`keep_vpn_service`), mejorando la estabilidad del sistema de ajustes.
-   Se ajustó el ancho del nuevo diálogo de ayuda para que ocupe el 90% de la pantalla, evitando que el texto se corte en dispositivos de diferentes tamaños y mejorando la legibilidad.

---

## [2.0.0] - 2025-09-18

### Corregido (Fixed)

-   **Reconexión Crítica:** Se solucionó un error fundamental que causaba que la aplicación se quedara "congelada" o no respondiera al intentar reconectar después de una pérdida de señal. El proceso de reconexión ahora es mucho más robusto y fiable.
-   **Mensaje de Conexión:** Se corrigió el comportamiento del mensaje "¡Conectado con éxito!", que ahora solo se muestra una única vez al establecer la conexión, en lugar de aparecer cada vez que se abría la app.

### Cambiado (Changed)

-   **Lógica de Actualizaciones:** Se renovó por completo el sistema de verificación de versiones para ser compatible con la nueva estructura del JSON del servidor. La app ahora diferencia correctamente entre actualizaciones opcionales (versión más reciente) y actualizaciones forzosas (versión mínima requerida).

### Mejoras Internas (Developer)

-   Se optimizaron los logs de depuración en el código nativo (JNI) para la verificación de versiones. Ahora se muestra la versión actual, la mínima requerida y la última disponible, facilitando el seguimiento del flujo de actualización.
