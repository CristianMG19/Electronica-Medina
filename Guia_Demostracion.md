# Guía de demostración y revisión oral

Alumnos:<br>
Medina González Cristian Roberto - 23170234<br>
Villalba Rodríguez Kevin Eliazar - 23170369<br>
Leon Fernandez Alexis Javier - 23171137<br>
Lenguajes de Interfaz | Ing. José Luis Medina Jiménez | 10:00 - 11:00

## Preparación

Extraer el ZIP. Con el programa cerrado, copiar datos_prueba/inventario_ejemplo.dat como inventario.dat para empezar con ESP-032 (10 unidades), RES-220 (4) y LED-005 (30). Abrir la solución con Debug/x86. La carpeta de trabajo debe ser la raíz de la entrega. El resumen inicial es 44 unidades y 1935.00 MXN; ingresos de sesión 0.00 MXN. RES-220 ya presenta alerta.

## Secuencia completa de la demostración

1. Elegir 1. Capturar ESP-032 para demostrar el rechazo del duplicado. Cuando vuelva a pedir el código, usar USB-001.
2. Nombre: Cable USB. Precio: 12.345; se rechaza por tener tres decimales. Recapturar 12.05. Existencia inicial: 6. Se acepta el alta.
3. Elegir 3 y buscar USB-001. Debe mostrar 12.05 MXN y seis unidades, sin alerta para ese producto.
4. Elegir 4, USB-001 y 95 unidades. Se rechaza porque 6 + 95 excede cien. Regresa al menú y conserva las seis unidades.
5. Repetir opción 4 con USB-001 y 1. Ahora queda en siete unidades.
6. Elegir 5, USB-001 y 8. Se rechaza por falta de existencias.
7. Repetir opción 5 con dos unidades. El importe es 24.10 MXN; quedan cinco y aparece la alerta. Esta es la comparación entre un caso sin regla aplicada (seis o siete) y otro con regla aplicada (cinco).
8. Elegir 6. El resumen debe mostrar 49 unidades, 1995.25 MXN de inventario y 24.10 MXN de ingresos de la sesión.
9. Elegir 0 antes de guardar. Se muestra el aviso; responder 0 para volver. Elegir 7 para guardar y después 0 para salir.
10. Ejecutar de nuevo. Buscar USB-001: siguen cinco unidades. Los ingresos ahora son 0.00 MXN porque pertenecen al proceso actual.
11. Con el programa cerrado, renombrar temporalmente inventario.dat como inventario.respaldo. Iniciar: debe avisar que no pudo cargar y permanecer con catálogo vacío. Cerrar y restaurar el archivo.
12. Para probar un archivo inválido sin perder el catálogo bueno, hacer una copia de respaldo y sustituir inventario.dat por datos_prueba/inventario_invalido.dat. Si el programa ya tiene un catálogo cargado y se usa la opción 8, el error debe conservar ese catálogo. Restaurar el archivo bueno al terminar.

## Recorrido con ESI en el depurador

Colocar F9 en fpLoop, dentro de FindProduct. Iniciar F5, elegir buscar y capturar LED-005. EDX señala el código solicitado; ESI comienza en catalog. productCount vale tres en el ejemplo inicial. Avanzar con F10: cada producto no coincidente suma SIZEOF Product, es decir 44 bytes, a ESI. El tercer registro está en catalog + 88. Abrir Registros y Memoria; mostrar los ocho bytes del código y los campos price y stock. ECX disminuye hasta encontrar el registro o agotar el catálogo.

## Conversión decimal y pila

Colocar F9 en utsDivide, dentro de UIntToString. Mostrar el resumen. EAX contiene el entero por convertir; EBX vale diez. Antes de DIV, EDX se limpia. Tras DIV, EAX conserva el cociente y EDX el residuo. Cada residuo se apila. En utsWrite, los POP recuperan los dígitos desde el más significativo, se suma '0' y se guardan en el buffer EDI. Mostrar que al volver de la función ESP recupera su valor de entrada. USES agrega la conservación de los registros declarados; no confundir estos PUSH/POP con los de los dígitos.

## Preguntas

1. **ReadString.** En ReadInput, EDX apunta a inputBuffer y ECX se establece en 255 para un buffer de 256 bytes. EAX devuelve la longitud. Los caracteres quedan en inputBuffer con terminador cero. Se rechazan longitudes de 250 o más; este margen también cubre variantes antiguas de Irvine que reservan el terminador dentro de ECX. ReadInput conserva el EDX del mensaje mediante USES.

2. **Precios a centavos.** ParsePrice acumula los pesos con valor = valor * 10 + dígito. Al encontrar el punto, exige uno o dos dígitos. Un decimal se multiplica por diez. 185.50 produce 185 * 100 + 50 = 18550; 12.05 produce 1205. Un tercer decimal, punto final o símbolo provoca CF=1 sin escribir en el producto.

3. **Entero a texto.** UIntToString divide repetidamente entre diez. El cociente permite continuar; cada residuo es un dígito de 0 a 9. Al apilar los residuos se invierte su orden de extracción. Sumar el ASCII de '0' los convierte en caracteres. El cero también realiza una iteración, por eso se muestra como 0 y no como cadena vacía.

4. **Pesos y centavos.** MoneyToString pone EDX en cero antes de DIV 100. EAX recibe pesos y EDX centavos. Después divide los centavos entre diez para obtener decenas y unidades: el residuo cinco se escribe como 05. Agrega punto, espacio, MXN y el terminador.

5. **Memoria.** Product ocupa 44 bytes: código 8, nombre 26, reservado 2, precio 4 y existencia 4. catalog reserva diez registros, 440 bytes. ESI señala un registro y la expresión (Product PTR [esi]).price accede al desplazamiento 36. stock está en 40.

6. **Comparaciones y saltos.** ValidateCode usa JB/JA para rangos ASCII y JNE para guion y longitud exacta. FindProduct compara los dos DWORD del código completo. RegisterCandidate rechaza si el código ya existe o productCount es diez. ReceiveStock y SellProduct usan límites sin signo; SellProduct salta al rechazo con JA cuando la cantidad supera el stock.

7. **Cambios completos.** SellProduct valida la cantidad, existencia, producto EDX:EAX e ingresos mediante ADD/JC. Solo entonces descuenta stock y publica el ingreso. Si hay error, ninguna de estas dos escrituras ocurre. RegisterCandidate prepara un registro temporal; ValidateFile prepara y valida un catálogo separado antes de reemplazar el vigente.

8. **CALL, RET y registros.** CALL apila la dirección de regreso y RET la recupera. USES indica a MASM qué registros guardar y restaurar. UIntToString conserva EAX, EBX, ECX, EDX y EDI; además tiene un POP por cada residuo apilado. El arnés comprobó ESP y esos registros con valores centinela, incluyendo ESI que la función no modifica.

9. **Handle y escritura.** El handle es el identificador del archivo abierto. EAX contiene ese handle antes de WriteToFile; EDX apunta a los bytes y ECX indica su cantidad. Al regresar, CF=0 señala éxito y EAX la cantidad escrita. SaveCatalog exige que coincida con fileLength, verifica CloseFile y después reemplaza el archivo destino.

10. **Bytes leídos y capacidad.** ECX es la capacidad solicitada; EAX es la cantidad realmente leída, que puede ser menor. Aquí el buffer tiene 449 bytes y el formato válido llega a 448. No se pasa el archivo binario completo a WriteString. Los campos de texto incluyen y validan su propio 00h. Si se quisiera mostrar un archivo de texto arbitrario, habría que reservar capacidad + 1 y colocar cero en buffer[bytesLeidos], después de verificar límites y éxito.

11. **Formato y recuperación.** El formato binario INV1 contiene firma, contador DWORD y registros de 44 bytes en little-endian. ValidateFile comprueba longitud exacta, máximo diez, códigos únicos, nombres terminados y válidos, precio, stock y relleno cero. LoadCatalog comprueba lectura y cierre antes de copiar. Un archivo ausente o inválido conserva el catálogo actual; al iniciar, este es vacío. No se guarda la dirección de memoria ni el ingreso de una sesión anterior.

12. **Regla e investigación.** IsLowStock activa la alerta para stock <= 5, incluido cero. Con seis unidades no se aplica y con cinco sí. El umbral es una decisión del negocio, no una cifra tomada de una fuente. Las fuentes consultadas documentan la captura de cadenas, contratos de archivo, PROC/USES, símbolos de depuración y reemplazo de archivos; están en el informe. Practicar esta explicación en el código antes de la revisión individual.

## Alcance de la evidencia

Los casos de consola y del núcleo incluidos en la tabla se ejecutaron. La navegación personal por la interfaz de Visual Studio y la exposición oral deben realizarse por el alumno; no se presentan como efectuadas.
