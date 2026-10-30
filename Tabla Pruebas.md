# Tabla de pruebas

Proyecto: Electrónica Medina | Medina González Cristian Roberto | 23170234

Pruebas realizadas el 28 de octubre de 2026. Las primeras 113 ejecutan los procedimientos MASM reales en un arnés de comprobación. Los casos de archivo se ejecutaron también en procesos separados. El éxito indica que todas las comparaciones del caso se cumplieron; no es una predicción de resultados.

| ID | Entrada / caso | Resultado esperado | Resultado obtenido |
| --- | --- | --- | --- |
| 1 | ParsePrice: '185.50' | 18550 | 18550 (verificado en MASM) |
| 2 | ParsePrice: '12.05' | 1205 | 1205 (verificado en MASM) |
| 3 | ParsePrice: '12.5' | 1250 | 1250 (verificado en MASM) |
| 4 | ParsePrice: '12' | 1200 | 1200 (verificado en MASM) |
| 5 | ParsePrice: '0.01' | 1 | 1 (verificado en MASM) |
| 6 | ParsePrice: '9999.99' | 999999 | 999999 (verificado en MASM) |
| 7 | ParsePrice: '0001.20' | 120 | 120 (verificado en MASM) |
| 8 | ParsePrice: '' | Rechazado | Rechazado (verificado en MASM) |
| 9 | ParsePrice: '0' | Rechazado | Rechazado (verificado en MASM) |
| 10 | ParsePrice: '0.00' | Rechazado | Rechazado (verificado en MASM) |
| 11 | ParsePrice: '10000' | Rechazado | Rechazado (verificado en MASM) |
| 12 | ParsePrice: '9999.999' | Rechazado | Rechazado (verificado en MASM) |
| 13 | ParsePrice: '1.' | Rechazado | Rechazado (verificado en MASM) |
| 14 | ParsePrice: '.50' | Rechazado | Rechazado (verificado en MASM) |
| 15 | ParsePrice: '1.2.3' | Rechazado | Rechazado (verificado en MASM) |
| 16 | ParsePrice: '$1' | Rechazado | Rechazado (verificado en MASM) |
| 17 | ParsePrice: '1,000' | Rechazado | Rechazado (verificado en MASM) |
| 18 | ParsePrice: '-1' | Rechazado | Rechazado (verificado en MASM) |
| 19 | ParsePrice: '+1' | Rechazado | Rechazado (verificado en MASM) |
| 20 | ParsePrice: ' 1' | Rechazado | Rechazado (verificado en MASM) |
| 21 | ParsePrice: '1 ' | Rechazado | Rechazado (verificado en MASM) |
| 22 | ParsePrice: '1e2' | Rechazado | Rechazado (verificado en MASM) |
| 23 | ParsePrice: '42949672960' | Rechazado | Rechazado (verificado en MASM) |
| 24 | ParseUnsigned: '0' | 0 | 0 (verificado en MASM) |
| 25 | ParseUnsigned: '100' | 100 | 100 (verificado en MASM) |
| 26 | ParseUnsigned: '4294967295' | 4294967295 | 4294967295 (verificado en MASM) |
| 27 | ParseUnsigned: '4294967296' | Rechazado | Rechazado (verificado en MASM) |
| 28 | ParseUnsigned: '9999999999999999999' | Rechazado | Rechazado (verificado en MASM) |
| 29 | ParseUnsigned: '' | Rechazado | Rechazado (verificado en MASM) |
| 30 | ParseUnsigned: '-1' | Rechazado | Rechazado (verificado en MASM) |
| 31 | ParseUnsigned: '1.0' | Rechazado | Rechazado (verificado en MASM) |
| 32 | ParseUnsigned: '1a' | Rechazado | Rechazado (verificado en MASM) |
| 33 | ParseUnsigned: ' 2' | Rechazado | Rechazado (verificado en MASM) |
| 34 | ParseUnsigned: '+1' | Rechazado | Rechazado (verificado en MASM) |
| 35 | ValidateCode: 'ABC-123' | Aceptado | Aceptado (verificado en MASM) |
| 36 | ValidateCode: 'XYZ-000' | Aceptado | Aceptado (verificado en MASM) |
| 37 | ValidateCode: 'abc-123' | Rechazado | Rechazado (verificado en MASM) |
| 38 | ValidateCode: 'AB-123' | Rechazado | Rechazado (verificado en MASM) |
| 39 | ValidateCode: 'ABCD-123' | Rechazado | Rechazado (verificado en MASM) |
| 40 | ValidateCode: 'ABC_123' | Rechazado | Rechazado (verificado en MASM) |
| 41 | ValidateCode: 'ABC-12X' | Rechazado | Rechazado (verificado en MASM) |
| 42 | ValidateCode: 'ABC-1234' | Rechazado | Rechazado (verificado en MASM) |
| 43 | ValidateCode: '' | Rechazado | Rechazado (verificado en MASM) |
| 44 | ValidateName: 'ESP32 DevKit' | Aceptado | Aceptado (verificado en MASM) |
| 45 | ValidateName: 'A' | Aceptado | Aceptado (verificado en MASM) |
| 46 | ValidateName: 'AAAAAAAAAAAAAAAAAAAAAAAAA' | Aceptado | Aceptado (verificado en MASM) |
| 47 | ValidateName: 'AAAAAAAAAAAAAAAAAAAAAAAAAA' | Rechazado | Rechazado (verificado en MASM) |
| 48 | ValidateName: '' | Rechazado | Rechazado (verificado en MASM) |
| 49 | ValidateName: '   ' | Rechazado | Rechazado (verificado en MASM) |
| 50 | ValidateName: 'X\tY' | Rechazado | Rechazado (verificado en MASM) |
| 51 | UIntToString: 0 | 0 | 0 (verificado en MASM) |
| 52 | UIntToString: 1 | 1 | 1 (verificado en MASM) |
| 53 | UIntToString: 10 | 10 | 10 (verificado en MASM) |
| 54 | UIntToString: 100 | 100 | 100 (verificado en MASM) |
| 55 | UIntToString: 4294967295 | 4294967295 | 4294967295 (verificado en MASM) |
| 56 | MoneyToString: 0 | 0.00 MXN | 0.00 MXN (verificado en MASM) |
| 57 | MoneyToString: 1 | 0.01 MXN | 0.01 MXN (verificado en MASM) |
| 58 | MoneyToString: 5 | 0.05 MXN | 0.05 MXN (verificado en MASM) |
| 59 | MoneyToString: 100 | 1.00 MXN | 1.00 MXN (verificado en MASM) |
| 60 | MoneyToString: 1205 | 12.05 MXN | 12.05 MXN (verificado en MASM) |
| 61 | MoneyToString: 18550 | 185.50 MXN | 185.50 MXN (verificado en MASM) |
| 62 | MoneyToString: 999999 | 9999.99 MXN | 9999.99 MXN (verificado en MASM) |
| 63 | MoneyToString: 4294967295 | 42949672.95 MXN | 42949672.95 MXN (verificado en MASM) |
| 64 | Resumen de tres productos | 44 unidades; 1935.00 MXN | 44 unidades; 1935.00 MXN (verificado en MASM) |
| 65 | Busqueda ESP-032 | Registro 1 | Registro 1 (verificado en MASM) |
| 66 | Busqueda RES-220 | Registro 2 | Registro 2 (verificado en MASM) |
| 67 | Busqueda LED-005 | Registro 3 | Registro 3 (verificado en MASM) |
| 68 | Busqueda NUL-000 | No encontrado | No encontrado (verificado en MASM) |
| 69 | Entrada sobre stock 10: 5 | Aceptada; stock 15 | Aceptada; stock 15 (verificado en MASM) |
| 70 | Entrada sobre stock 10: 90 | Aceptada; stock 100 | Aceptada; stock 100 (verificado en MASM) |
| 71 | Entrada sobre stock 10: 91 | Rechazada; stock 10 | Rechazada; stock 10 (verificado en MASM) |
| 72 | Entrada sobre stock 10: 0 | Rechazada; stock 10 | Rechazada; stock 10 (verificado en MASM) |
| 73 | Entrada sobre stock 10: 101 | Rechazada; stock 10 | Rechazada; stock 10 (verificado en MASM) |
| 74 | Entrada sobre stock 10: 4294967295 | Rechazada; stock 10 | Rechazada; stock 10 (verificado en MASM) |
| 75 | Venta sobre stock 10: 2 | Aceptada; stock 8; ingresos 371.00 MXN | Aceptada; stock 8; ingresos 371.00 MXN (verificado en MASM) |
| 76 | Venta sobre stock 10: 10 | Aceptada; stock 0; ingresos 1855.00 MXN | Aceptada; stock 0; ingresos 1855.00 MXN (verificado en MASM) |
| 77 | Venta sobre stock 10: 11 | Rechazada; stock 10; ingresos 0.00 MXN | Rechazada; stock 10; ingresos 0.00 MXN (verificado en MASM) |
| 78 | Venta sobre stock 10: 0 | Rechazada; stock 10; ingresos 0.00 MXN | Rechazada; stock 10; ingresos 0.00 MXN (verificado en MASM) |
| 79 | Venta sobre stock 10: 101 | Rechazada; stock 10; ingresos 0.00 MXN | Rechazada; stock 10; ingresos 0.00 MXN (verificado en MASM) |
| 80 | Venta con ingresos cercanos a 2^32 | Rechazo por desbordamiento; stock e ingresos sin cambios | Rechazo por desbordamiento; stock e ingresos sin cambios (verificado en MASM) |
| 81 | Alerta con stock 0 | Alerta | Alerta (verificado en MASM) |
| 82 | Alerta con stock 4 | Alerta | Alerta (verificado en MASM) |
| 83 | Alerta con stock 5 | Alerta | Alerta (verificado en MASM) |
| 84 | Alerta con stock 6 | Sin alerta | Sin alerta (verificado en MASM) |
| 85 | Alerta con stock 100 | Sin alerta | Sin alerta (verificado en MASM) |
| 86 | Alta duplicada ESP-032 | Rechazada; tres productos sin cambios | Rechazada; tres productos sin cambios (verificado en MASM) |
| 87 | Alta USB-001 valida | Aceptada; cuatro productos | Aceptada; cuatro productos (verificado en MASM) |
| 88 | Alta con diez productos | Rechazada por capacidad | Rechazada por capacidad (verificado en MASM) |
| 89 | Alta con price=0 | Rechazada sin alta parcial | Rechazada sin alta parcial (verificado en MASM) |
| 90 | Alta con price=1000000 | Rechazada sin alta parcial | Rechazada sin alta parcial (verificado en MASM) |
| 91 | Alta con stock=101 | Rechazada sin alta parcial | Rechazada sin alta parcial (verificado en MASM) |
| 92 | Archivo valido de tres productos | Archivo aceptado | Archivo aceptado (verificado en MASM) |
| 93 | Archivo con firma incorrecta | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 94 | Archivo con 11 productos | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 95 | Archivo truncado | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 96 | Archivo con bytes extra | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 97 | Archivo vacio | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 98 | Archivo con precio cero | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 99 | Archivo con precio excesivo | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 100 | Archivo con stock 101 | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 101 | Archivo con codigo minusculo | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 102 | Archivo con codigo duplicado | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 103 | Archivo con nombre sin terminador | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 104 | Archivo con relleno alterado | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 105 | Archivo con reservado distinto de cero | Archivo rechazado; catalogo conservado | Archivo rechazado; catalogo conservado (verificado en MASM) |
| 106 | Archivo valido sin productos | Archivo aceptado | Archivo aceptado (verificado en MASM) |
| 107 | Pila y conservacion en UIntToString | ESP y registros conservados | ESP y registros conservados (verificado en MASM) |
| 108 | guardar_catalogo | Archivo guardado; cambios pendientes desactivados | Archivo guardado; cambios pendientes desactivados (verificado en proceso separado) |
| 109 | cargar_nueva_ejecucion | Tres productos recuperados; ingresos de sesion conservados | Tres productos recuperados; ingresos de sesion conservados (verificado en proceso separado) |
| 110 | archivo_ausente | Error; catalogo y cambios pendientes conservados | Error; catalogo y cambios pendientes conservados (verificado en proceso separado) |
| 111 | archivo_invalido | Error; catalogo y cambios pendientes conservados | Error; catalogo y cambios pendientes conservados (verificado en proceso separado) |
| 112 | fallo_guardado | Error de escritura; cambios pendientes conservados | Error de escritura; cambios pendientes conservados (verificado en proceso separado) |
| 113 | resumen_limite | 1000 unidades; 9999990.00 MXN | 1000 unidades; 9999990.00 MXN (verificado en proceso separado) |

## Demostración real de consola

Se inició el ejecutable en una consola de Windows sin ventana visible y se enviaron entradas de teclado. Se consultó el texto del buffer real de consola y se verificó el archivo final. No se utilizaron implementaciones alternativas de las operaciones.

| ID | Entrada / caso | Resultado esperado | Resultado obtenido |
| --- | --- | --- | --- |
| 114 | Menú: alta de ESP-032 existente | Rechazar duplicado y volver a pedir código | Código duplicado; nueva captura |
| 115 | Alta USB-001, Cable USB, precio 12.345 | Rechazar tercer decimal | Dato inválido; precio recapturado |
| 116 | Alta USB-001, Cable USB, 12.05, stock 6 | Cuarto producto, stock 6 | Alta aceptada |
| 117 | Buscar USB-001 | Mostrar precio 12.05 MXN | Precio 12.05 MXN observado |
| 118 | Recibir 95 con stock 6 | Rechazar 101 unidades totales | Operación rechazada |
| 119 | Recibir 1 con stock 6 | Stock 7 | Existencia 7 observada |
| 120 | Vender 8 con stock 7 | Rechazar sin cambios | Operación rechazada |
| 121 | Vender 2 a 12.05 MXN | 24.10 MXN; stock 5; alerta | Importe 24.10 MXN y alerta observados |
| 122 | Resumen después de esa venta | 49 unidades; valor 1995.25 MXN; ingresos 24.10 MXN | Importes observados; catálogo guardado con stock 5 |
| 123 | Salir con cambios pendientes y elegir 0 | Avisar y regresar al menú | Aviso y regreso al menú observados |
| 124 | Guardar y salir | Guardar cuatro productos | Archivo binario comprobado: cuatro registros, USB-001 a 1205 centavos y stock 5 |
| 125 | Segunda ejecución, buscar USB-001 y mostrar resumen | Recuperar stock 5; ingresos 0.00 MXN | Stock 5 e ingresos 0.00 MXN observados |

## Verificación pendiente de presentación

| Caso | Esperado | Obtenido |
| --- | --- | --- |
| Abrir y compilar con la interfaz de Visual Studio | Debug/x86 con símbolos | Pendiente; MSBuild encontró una restricción de acceso al detectar el SDK. ML y LINK directos sí compilaron. |
| Explicación oral de ESI, DIV y pila | El alumno señala y explica las instrucciones | Pendiente de la demostración personal; se incluye una guía. |

No se simularon fallos de energía, almacenamiento lleno ni todos los errores posibles de Windows. Sí se provocó un fallo real al crear el archivo temporal y se comprobó que el catálogo previo y los cambios pendientes se conservaron.
