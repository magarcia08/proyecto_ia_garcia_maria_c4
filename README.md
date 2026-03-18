# HelpDeskBot Examen

Bot de Telegram para gestionar tickets de soporte interno. Hecho con n8n + Google Sheets. Sin base de datos real, sin servidor, sin IA.

---

## Que hace

El usuario escribe por Telegram, el bot le muestra un menu con opciones numeradas, lo guia paso a paso para crear tickets y los guarda en Google Sheets.

---

## Stack

- n8n Community Edition (automatizacion)
- Google Sheets (base de datos)
- Telegram Bot API (interfaz)
- JavaScript (logica en nodos Code)

---

## Estructura Google Sheets

**USUARIOS**
```
telegram_user | nombre | rol | activo | email | sesion | sesion_data
```

**SOLICITUDES**
```
id_ticket | tipo | prioridad | descripcion | estado | creado_por | fecha_creacion
```

**LOGS**
```
timestamp | telegram_user | pantalla | opcion | resultado
```

> Los encabezados deben estar en la fila 1. Si estan en otra fila n8n no los detecta.

---

## Como configurar

1. Crear bot en Telegram con @BotFather y guardar el token
2. Crear Google Sheet con las 3 hojas y encabezados en fila 1
3. Copiar el ID del Sheet desde la URL (entre /d/ y /edit)
4. Importar HelpDeskBot_v3_workflow.json en n8n
5. Reemplazar SPREADSHEET_ID_AQUI en todos los nodos de Sheets
6. Configurar credenciales de Telegram y Google Sheets
7. En Sheets Buscar Usuario ir a Settings y activar Always Output Data
8. Activar el workflow

---

## Nodos importantes

| Nodo | Que hace |
|------|----------|
| Extraer Datos del Mensaje | Saca chatId, username, text del JSON de Telegram |
| Sheets: Buscar Usuario | Busca si el usuario existe en USUARIOS |
| Gestor de Sesion | Lee la columna sesion y decide en que paso esta el usuario |
| Switch: Router Principal | Manda el flujo al nodo correcto segun la pantalla |
| Sheets: Guardar Sesion | Actualiza la columna sesion para recordar el paso del wizard |
| Sheets: Limpiar Sesion | Borra la sesion cuando termina el wizard |

---

## Errores comunes

| Error | Solucion |
|-------|----------|
| Flujo se corta si usuario no existe | Activar Always Output Data en el nodo Sheets |
| username sale como sin_usuario | Normal, muchos no tienen. Se usa first_name como fallback |
| Mensaje se repite varias veces | Usar Switch central con sesion, no multiples IFs en paralelo |
| No columns found | Los encabezados no estan en la fila 1 del Sheet |
| Expresion sale como texto literal | Activar modo expresion con el icono {} en el campo |
| At least one value has to be added | Agregar campos en Values to Send con Add Field |
| .item.json da error en Code | Usar .first().json dentro de nodos Code |

---

## Lo que falta

- Cambio de estado automatico (hoy es manual en Sheets)
- Notificaciones cuando cambia el estado
- Adjuntos en los tickets
- Panel de administrador

---

*Marzo 2026 - n8n Community + Google Sheets + Telegram*
