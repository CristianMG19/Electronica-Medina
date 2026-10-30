# Electrónica Medina

Ventas e inventario en MASM x86 / Irvine32

Medina González Cristian Roberto · Matrícula 23170234  
Lenguajes de Interfaz, Unidad 2  
Docente: Ing. José Luis Medina Jiménez · Horario: 10:00 - 11:00  
28 de octubre de 2026

## 1. Negocio y regla adicional

El sistema administra una tienda pequeña de componentes electrónicos. Integra en un único programa el registro, consulta, búsqueda, recepción, venta, resumen y persistencia de hasta diez productos. No usa las funciones numéricas de Irvine prohibidas por la consigna.

La regla adicional es una alerta de existencia baja cuando un producto tiene cinco unidades o menos. Se muestra en las fichas y en el resumen; no modifica el precio. El umbral es una decisión del negocio: permite identificar productos que requieren reposición. Con seis unidades no se activa y con cinco sí.

| Código | Nombre | Precio MXN | Stock |
| --- | --- | --- | --- |
| ESP-032 | ESP32 DevKit | 185.50 | 10 |
| RES-220 | Resistencia 220 ohm | 1.25 | 4 |
| LED-005 | LED rojo 5 mm | 2.50 | 30 |

El archivo de prueba contiene estos tres productos. Su valor inicial es 1935.00 MXN, con 44 unidades. RES-220 activa la alerta. Cada nueva ejecución inicia sus ingresos en cero.

## 2. Validaciones y decisiones

Los códigos siguen exactamente ABC-123 y no se repiten. Los nombres tienen de 1 a 25 caracteres ASCII imprimibles y al menos uno distinto de espacio. El precio acepta pesos enteros o uno o dos decimales, entre 0.01 y 9999.99 MXN. El stock admite 0 a 100; recepción y venta exigen enteros positivos y respetan existencias y capacidad.

Se usan centavos DWORD para evitar redondeos de coma flotante. ParseUnsigned comprueba el producto por diez y la suma mediante EDX y CF. ParsePrice separa la parte decimal; UIntToString y MoneyToString generan las cadenas que muestra WriteString. El buffer de captura es mayor que cualquier campo válido; se rechazan longitudes de 250 o más para no aceptar una entrada truncada. [1]

Una entrada inválida no publica cambios. Los errores de formato repiten la captura del campo; los rechazos de recepción o venta regresan al menú para permitir otra operación.

# Memoria y persistencia

## 3. Organización de los datos

| Desplazamiento | Campo | Tamaño |
| --- | --- | --- |
| 0 | Código y terminador 00h | 8 bytes |
| 8 | Nombre y relleno cero | 26 bytes |
| 34 | Reservado, siempre cero | 2 bytes |
| 36 | Precio en centavos | 4 bytes |
| 40 | Existencia | 4 bytes |

Cada Product ocupa 44 bytes. catalog reserva 440 bytes; productCount determina cuántos registros están activos. ESI recorre las fichas sumando SIZEOF Product. candidate permite preparar un alta sin alterar el catálogo. sessionIncome y dirty pertenecen a la sesión. Los procedimientos documentan entradas, salidas y registros conservados; USES genera las salvaguardas correspondientes. [2]

## 4. Formato del archivo

inventario.dat es binario, little-endian. Comienza con los cuatro bytes ASCII INV1, sigue un contador DWORD y después exactamente N registros Product. Su longitud debe ser 8 + 44 × N bytes: 140 en el ejemplo y 448 con diez productos. Se distribuye además un listado de texto legible del ejemplo.

LoadCatalog lee hasta 449 bytes en fileBuffer para detectar un archivo con exceso de datos. Valida firma, contador, longitud exacta, terminadores, caracteres, relleno cero, precios, existencias y códigos duplicados. Solo después de una lectura, cierre y validación correctos reemplaza el catálogo; los ingresos actuales se conservan. El archivo no es una cadena para WriteString. [3, 4]

## 5. Guardado y operaciones completas

SaveCatalog serializa y valida el catálogo. Escribe inventario.tmp y exige que la cantidad informada por WriteToFile coincida con la solicitada. Comprueba CloseFile y reemplaza inventario.dat mediante MoveFileExA con REPLACE_EXISTING y WRITE_THROUGH. dirty se limpia únicamente al terminar con éxito. Esta estrategia evita truncar el archivo anterior al abrir el temporal; no se presenta como garantía frente a cualquier fallo de energía. [4, 5, 6]

SellProduct calcula precio × cantidad, verifica la parte alta EDX y prueba el ingreso acumulado con ADD/JC antes de descontar stock. Un rechazo deja ambos valores intactos. El máximo legal del inventario es 999999000 centavos; los ingresos pueden crecer entre ventas, por lo que también se comprueba su desbordamiento de 32 bits.

Las rutas son relativas a la raíz de la entrega, configurada como carpeta de trabajo del depurador. Al salir con cambios se puede guardar, descartar o volver; una carga manual también solicita confirmación antes de descartar cambios pendientes.

# Comprobación y trazabilidad

## 6. Evidencia obtenida

El programa fue ensamblado y enlazado para x86 con ML/LINK de Visual Studio 2022, versión de herramientas 14.44.35207, usando símbolos de depuración. Pasaron 113 casos ejecutados sobre los procedimientos MASM y operaciones de archivo. La tabla adjunta registra entrada, resultado esperado y obtenido; las comprobaciones no se sustituyeron por una simulación del negocio en otro lenguaje. [7]

| Área | Procedimientos / evidencia |
| --- | --- |
| Registro y formato | ValidateCode, ValidateName, RegisterCandidate, FindProduct |
| Captura y conversión | ReadInput, ParseUnsigned, ParsePrice, UIntToString |
| Entradas y ventas | ReceiveStock, SellProduct; rechazo sin cambios |
| Resumen y regla | ComputeSummary, MoneyToString, IsLowStock |
| Archivo | ValidateFile, SaveCatalog, LoadCatalog; procesos separados |
| Pila | UIntToString: ESP y registros verificados con centinelas |

También se ejecutó el menú real en una consola de Windows: alta de USB-001, rechazo del duplicado y de un tercer decimal, búsqueda, entrada aceptada y excesiva, venta aceptada y rechazada, alerta, resumen, aviso de salida y guardado. Una segunda ejecución recuperó el producto y reinició los ingresos de sesión.

Resultado de esa secuencia: Cable USB a 12.05 MXN, cinco unidades restantes, venta de 24.10 MXN, 49 unidades totales y valor del inventario de 1995.25 MXN. El archivo final se inspeccionó y contenía cuatro registros. La entrega conserva el ejemplo inicial limpio para repetir la demostración.

## 7. Alcance y pendientes

La detección del SDK mediante MSBuild encontró una restricción de acceso a la carpeta local Microsoft SDKs. La compilación comprobada se hizo directamente con ML/LINK y las bibliotecas del SDK instalado. Se incluye la solución .sln, el .vcxproj y Compilar.cmd como alternativa. No se alteró Microsoft Defender.

No se afirma haber completado la navegación por el depurador gráfico ni la explicación oral del alumno. Guia_Demostracion.md identifica los puntos de interrupción, el recorrido con ESI y las doce respuestas de revisión. Los errores de archivo ausente, inválido y creación del temporal se comprobaron; no se probaron cortes de energía ni todos los posibles fallos del sistema operativo.

El formato está validado estructuralmente, pero no incluye cifrado ni autenticación. No se diseñó para dos instancias que escriban simultáneamente. Los ingresos son de la sesión; el catálogo es el dato persistente.

# Fuentes y decisiones investigadas

Se consultaron las referencias siguientes para contrastar los contratos de la biblioteca y de las herramientas. El formato INV1, el umbral de cinco unidades y la organización en registros son decisiones propias de esta implementación. Consulta: 28 de octubre de 2026.

[1] Kip Irvine, documentación de ReadString, copia HTML alojada por California State University, Dominguez Hills. Parámetros EDX/ECX, longitud de salida y terminador.
https://csc.csudh.edu/mmccullough/asm/help/source/irvinelib/readstring.htm

[2] Microsoft Learn, PROC. Declaración de procedimientos y lista USES en MASM de 32 bits.
https://learn.microsoft.com/en-us/cpp/assembler/masm/proc?view=msvc-170

[3] Kip Irvine, ReadFromFile. Diferencia entre capacidad solicitada, bytes recibidos y CF de error.
https://csc.csudh.edu/mmccullough/asm/help/source/irvinelib/readfromfile.htm

[4] Kip Irvine, CloseFile. Resultado distinto de cero para un cierre satisfactorio.
https://csc.csudh.edu/mmccullough/asm/help/source/irvinelib/closefile.htm

[5] Kip Irvine, WriteToFile. Verificación de CF y de la cantidad realmente escrita.
https://csc.csudh.edu/mmccullough/asm/help/source/irvinelib/writetofile.htm

[6] Microsoft Learn, MoveFileExA. Reemplazo del destino y opción WRITE_THROUGH; retorno cero en caso de error.
https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-movefileexa

[7] Microsoft Learn, ML and ML64 command-line reference. Ensamblado COFF de 32 bits e información de depuración.
https://learn.microsoft.com/en-us/cpp/assembler/masm/ml-and-ml64-command-line-reference?view=msvc-170

## Material de partida

Consigna proporcionada: U2_PROYECTO_ENSAMBLADOR.pdf, tres páginas. También se revisó la implementación local de ReadString y WriteString en el código fuente distribuido con Irvine32 para considerar el comportamiento del buffer y la conservación de registros. No se copió una solución de inventario de estas fuentes.
