======================================================================
                  GUÍA RÁPIDA: MANEJO DE JSON EN PYTHON Y C#
======================================================================

----------------------------------------------------------------------
1. PYTHON: RECIBIR JSON POR SOCKET (A través de bytes)
----------------------------------------------------------------------
import socket
import json

# Crear y conectar socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("127.0.0.1", 5000))

# 1. Recibir los datos en bytes (búfer de 1024 bytes)
bytes_recibidos = s.recv(1024)

# 2. Decodificar de bytes a texto (string UTF-8)
texto_json = bytes_recibidos.decode('utf-8')

# 3. Convertir el texto JSON a un Diccionario de Python
datos = json.loads(texto_json)

# 4. Acceder a las claves
status = datos["status"]
mensaje = datos.get("mensaje")  # Uso seguro con .get()

s.close()


----------------------------------------------------------------------
2. PYTHON: LEER JSON DESDE ARCHIVO LOCAL O CADENA (Sin Socket)
----------------------------------------------------------------------
import json

# OPCIÓN A: Si el JSON ya viene como una variable string en memoria
json_string = '{"hora": "14:30", "mensaje": "Conexión exitosa", "status": 200}'
datos_memoria = json.loads(json_string)  # 'loads' viene de String
status = datos_memoria["status"]

# OPCIÓN B: Si el JSON está guardado en un archivo físico (.json)
with open("respuesta.json", "r", encoding="utf-8") as archivo:
    datos_archivo = json.load(archivo)   # 'load' lee directamente del file
    status = datos_archivo["status"]


----------------------------------------------------------------------
3. C#: RECIBIR JSON POR SOCKET (Con System.Net.Sockets)
----------------------------------------------------------------------
using System;
using System.Net.Sockets;
using System.Text;
using System.Text.Json;
using System.Text.Json.Serialization;


class EjemploSocket
{
    static void RecibirPorSocket()
    {
        TcpClient cliente = new TcpClient("127.0.0.1", 5000);
        NetworkStream stream = cliente.GetStream();

# 1. Crear búfer de bytes
        byte[] buffer = new byte[1024];
        int bytesLeidos = stream.Read(buffer, 0, buffer.Length);

# 2. Convertir bytes recibidos a un string UTF-8
        string textoJson = Encoding.UTF8.GetString(buffer, 0, bytesLeidos);

# 3. Deserializar el string JSON al objeto C#
        var opciones = new JsonSerializerOptions { PropertyNameCaseInsensitive = true };
        RespuestaSocket datos = JsonSerializer.Deserialize<RespuestaSocket>(textoJson, opciones);

# 4. Acceder a los datos
        Console.WriteLine($"Status: {datos.Status}");
        Console.WriteLine($"Mensaje: {datos.Mensaje}");

        cliente.Close();
    }
}


----------------------------------------------------------------------
4. C#: LEER JSON LOCAL (Sin Socket: Las 2 Formas)
----------------------------------------------------------------------
using System;
using System.IO;
using System.Text.Json;
using System.Text.Json.Serialization;


class EjemploLocal
{
    static void LeerJsonForma1()
    {
# Leer el archivo plano como string
        string textoJson = File.ReadAllText("respuesta.json");

# 2. Mapear directamente a la clase
        RespuestaLocal datos = JsonSerializer.Deserialize<RespuestaLocal>(textoJson);

# 3. Acceder con autocompletado y tipado fuerte
        Console.WriteLine($"Status: {datos.Status}");
        Console.WriteLine($"Mensaje: {datos.Mensaje}");
    }




# FORMA 2: Con JsonDocument / RootElement (Sin crear clases) 
    static void LeerJsonForma2()
    {
# 1. Leer el archivo plano como string
        string textoJson = File.ReadAllText("respuesta.json");

# 2. Parsear el árbol JSON en memoria
        using (JsonDocument doc = JsonDocument.Parse(textoJson))
        {
# 3. Obtener el elemento raíz
            JsonElement root = doc.RootElement;

# 4. Navegar las propiedades y extraer los tipos requeridos
            int status = root.GetProperty("status").GetInt32();
            string mensaje = root.GetProperty("mensaje").GetString();








PUEDO HACER DATETIME.PARSE(VARIABLE_TIPO_DATETIME.GETSTRING()) ESTO LO QUE HACE ES QUE INTERPRETA LA FECHA COMO FECHA POR QUE SI NO LO VE COMO UN STRING
ENTONCES LO QUE HACE ES QUE PRIMERO LA PASA A STRING Y DESPUES LA INTERPRETA 












# COMO HACER UN PARA QUE UN TEXBOX PUEDA LEER FECHA Y HORA Y MINUTO MANUAL O SEA UN DATETIMEPACKER

# Guía del método `CrearFechaHora`

El método `CrearFechaHora` se utiliza para crear un control de tipo `DateTimePicker`, configurarlo visualmente, agregarlo al panel del formulario y devolverlo para guardarlo en una variable.

```csharp
private DateTimePicker CrearFechaHora(
    Control contenedor,
    string nombre,
    int posicionY,
    int posicionX)
{
    DateTimePicker fecha = new DateTimePicker
    {
        CustomFormat = "dd/MM/yyyy HH:mm",
        Format = DateTimePickerFormat.Custom,
        Font = new Font("Segoe UI", 10F),
        Location = new Point(posicionX, posicionY - 4),
        Name = nombre,
        ShowUpDown = true,
        Size = new Size(310, 26)
    };

    contenedor.Controls.Add(fecha);

    return fecha;
}
```

## Declaración del método

```csharp
private DateTimePicker CrearFechaHora(...)
```

- `private`: el método solo puede utilizarse dentro de la clase `cita_from`.
- `DateTimePicker`: indica que el método devuelve un control de fecha y hora.
- `CrearFechaHora`: es el nombre del método.

El método necesita recibir cuatro datos:

```csharp
Control contenedor,
string nombre,
int posicionY,
int posicionX
```

---

## Parámetro `Control contenedor`

```csharp
Control contenedor
```

Indica dónde se colocará el control creado.

En el formulario se llama de esta manera:

```csharp
fechaHoraInicio = CrearFechaHora(
    formulario,
    "fechaHoraInicio",
    174,
    190
);
```

El objeto `formulario` se envía al parámetro `contenedor`.

`Control` es un tipo general de Windows Forms. Un `Panel`, como el utilizado en el formulario, pertenece a este tipo de controles.

Por eso el método puede trabajar con el panel recibido sin necesitar conocer detalles adicionales del panel.

La siguiente instrucción utiliza ese parámetro:

```csharp
contenedor.Controls.Add(fecha);
```

`Controls` contiene los elementos visuales que están dentro del contenedor.

`Add(fecha)` agrega el `DateTimePicker` al panel para que aparezca visualmente en el formulario.

---

## Parámetro `string nombre`

```csharp
string nombre
```

Recibe el nombre interno que tendrá el control.

En la llamada se envía:

```csharp
"fechaHoraInicio"
```

Ese valor se asigna a la propiedad `Name`:

```csharp
Name = nombre,
```

Por lo tanto, el control queda identificado internamente como:

```csharp
fechaHoraInicio
```

Este nombre permite distinguirlo de los otros controles de fecha y hora del formulario.

---

## Parámetros `posicionY` y `posicionX`

```csharp
int posicionY,
int posicionX
```

Representan la posición del control dentro del panel.

La posición se aplica en esta línea:

```csharp
Location = new Point(posicionX, posicionY - 4),
```

`Point` recibe primero la coordenada horizontal `X` y después la coordenada vertical `Y`.

Por eso se utiliza:

```csharp
posicionX
```

como primera coordenada y:

```csharp
posicionY - 4
```

como segunda coordenada.

El valor `-4` ajusta verticalmente el control para que quede alineado con la etiqueta correspondiente.

---

## Creación del `DateTimePicker`

```csharp
DateTimePicker fecha = new DateTimePicker
{
```

Aquí se crea un nuevo objeto de tipo `DateTimePicker`.

La variable que guarda ese objeto se llama:

```csharp
fecha
```

El `DateTimePicker` es el control que permite seleccionar una fecha y una hora para la cita.

---

## Propiedad `CustomFormat`

```csharp
CustomFormat = "dd/MM/yyyy HH:mm",
```

Define el formato en que se mostrará la fecha y la hora.

El formato utilizado es:

```text
dd/MM/yyyy HH:mm
```

Cada símbolo representa una parte:

- `dd`: día con dos dígitos.
- `MM`: mes con dos dígitos.
- `yyyy`: año con cuatro dígitos.
- `HH`: hora en formato de 24 horas.
- `mm`: minutos.

El resultado se muestra, por ejemplo, así:

```text
15/10/2026 20:30
```

---

## Propiedad `Format`

```csharp
Format = DateTimePickerFormat.Custom,
```

Indica que el control utilizará un formato personalizado.

El formato personalizado que debe utilizar está definido en:

```csharp
CustomFormat = "dd/MM/yyyy HH:mm",
```

Estas dos propiedades trabajan juntas para mostrar la fecha y la hora con el formato establecido.

---

## Propiedad `Font`

```csharp
Font = new Font("Segoe UI", 10F),
```

Define la fuente utilizada dentro del control.

En este caso:

- Tipo de letra: `Segoe UI`.
- Tamaño: `10`.

La fuente coincide con el estilo visual utilizado en los demás formularios.

---

## Propiedad `Location`

```csharp
Location = new Point(posicionX, posicionY - 4),
```

Determina dónde aparecerá el calendario dentro del panel.

La posición se calcula utilizando las coordenadas recibidas como parámetros.

- `posicionX`: distancia horizontal desde el lado izquierdo.
- `posicionY - 4`: distancia vertical desde la parte superior, con un ajuste de alineación.

---

## Propiedad `Name`

```csharp
Name = nombre,
```

Asigna al control el nombre recibido mediante el parámetro `nombre`.

Cuando se ejecuta:

```csharp
fechaHoraInicio = CrearFechaHora(
    formulario,
    "fechaHoraInicio",
    174,
    190
);
```

el control recibe:

```csharp
Name = "fechaHoraInicio";
```

---

## Propiedad `ShowUpDown`

```csharp
ShowUpDown = true,
```

Activa los botones de flecha del control.

Estos botones permiten aumentar o disminuir los valores de:

- Día.
- Mes.
- Año.
- Hora.
- Minutos.

Esto facilita seleccionar la fecha y hora de la cita.

---

## Propiedad `Size`

```csharp
Size = new Size(310, 26)
```

Define el tamaño del control.

- Ancho: `310`.
- Alto: `26`.

El ancho permite mostrar completa la fecha y la hora.

---

## Agregar el control al panel

```csharp
contenedor.Controls.Add(fecha);
```

Esta instrucción agrega el calendario al panel recibido.

En este formulario, el parámetro `contenedor` representa el panel:

```csharp
formulario
```

Por lo tanto, el calendario queda dentro de ese panel y puede mostrarse en la ventana.

---

## Devolver el control

```csharp
return fecha;
```

Devuelve el `DateTimePicker` que se acaba de crear.

Por eso puede guardarse en una variable como:

```csharp
fechaHoraInicio = CrearFechaHora(
    formulario,
    "fechaHoraInicio",
    174,
    190
);
```

La variable `fechaHoraInicio` conserva una referencia al calendario creado y permite utilizarlo posteriormente dentro de la clase.

---

## Funcionamiento completo

Cuando se ejecuta:

```csharp
fechaHoraInicio = CrearFechaHora(
    formulario,
    "fechaHoraInicio",
    174,
    190
);
```

ocurre lo siguiente:

1. El panel `formulario` se recibe como `contenedor`.
2. El texto `"fechaHoraInicio"` se recibe como `nombre`.
3. Los valores `174` y `190` se reciben como posiciones.
4. Se crea un `DateTimePicker`.
5. Se configura el formato `dd/MM/yyyy HH:mm`.
6. Se establece la fuente, la posición, el nombre, los botones de flecha y el tamaño.
7. El control se agrega al panel mediante `Controls.Add`.
8. El control se devuelve y se guarda en `fechaHoraInicio`.

En resumen, el método centraliza la creación y configuración de los calendarios utilizados en el formulario de citas.

            Console.WriteLine($"Status: {status}");
            Console.WriteLine($"Mensaje: {mensaje}");
        }
    }
}
======================================================================
