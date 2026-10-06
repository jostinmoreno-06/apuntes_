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

            Console.WriteLine($"Status: {status}");
            Console.WriteLine($"Mensaje: {mensaje}");
        }
    }
}
======================================================================