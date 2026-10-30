# Electrónica Medina - Proyecto integrado de Unidad 2

Alumnos: 
Medina González Cristian Roberto - 23170234  
Docente: Ing. José Luis Medina Jiménez  
Horario: 10:00 - 11:00

## Inicio rápido

1. Extraer todo el ZIP a una carpeta local con permiso de escritura.
2. Abrir Proyecto_Electronica.sln en Visual Studio 2022.
3. Seleccionar Debug y x86; el proyecto usa Win32 internamente.
4. Compilar con Ctrl+Mayús+B. Abrir main.asm y colocar un punto con F9.
5. Ejecutar con F5; avanzar con F10 sin entrar en las rutinas de Irvine.

También se incluye el ejecutable ensamblado y enlazado en Debug. Ejecutar.cmd lo inicia desde la carpeta que contiene inventario.dat. No requiere instalar Irvine para ejecutar el binario, pues su biblioteca se enlaza estáticamente. Para recompilar sí se necesitan los archivos de Irvine y las herramientas indicadas abajo.

El programa carga automáticamente inventario.dat al iniciar. Los ingresos comienzan en cero en cada proceso. Guardar conserva el catálogo, no los ingresos de sesiones anteriores.

## Requisitos para compilar

- Windows y Visual Studio 2022 con Desarrollo para el escritorio con C++.
- MSVC v143 para x86/x64 y un SDK de Windows 10 u 11.
- Irvine32 en C:\Irvine: Irvine32.inc, SmallWin.inc, VirtualKeys.inc e Irvine32.lib.

Si Irvine está en otra ubicación, modificar IrvineDir en Proyecto_Electronica.vcxproj. El proyecto importa las reglas MASM y genera símbolos de depuración. No seleccionar x64.

Alternativa: abrir x86 Native Tools Command Prompt for VS 2022, entrar en esta carpeta y ejecutar Compilar.cmd. Si hace falta, definir IrvineDir con la ubicación correcta antes de ejecutarlo. El script ensambla con ML y enlaza con LINK; no instala componentes ni modifica la seguridad de Windows.

## Menú

| Opción | Función |
| --- | --- |
| 1 | Registrar un producto; máximo diez y códigos únicos |
| 2 | Mostrar inventario y alertas |
| 3 | Buscar por código |
| 4 | Recibir mercancía sin exceder cien unidades |
| 5 | Vender un producto y acumular el ingreso |
| 6 | Mostrar unidades, valor total, ingresos y alertas |
| 7 | Guardar catálogo |
| 8 | Cargar catálogo, confirmando si hay cambios pendientes |
| 0 | Salir; ofrece guardar, descartar o volver si hay cambios |

Los precios se capturan como 185.50, 12.5 o 12; no usar signo de pesos, comas ni espacios. Todos los importes mostrados tienen dos decimales y MXN. Los nombres admiten de uno a veinticinco caracteres ASCII imprimibles y no pueden contener solo espacios. Las cantidades son enteros sin signo. Una captura de 250 caracteres o más se rechaza para no aceptar una línea truncada.

La regla del negocio es alertar cuando un producto tiene cinco unidades o menos. Se aplica al mostrar productos, consultar el inventario, recibir, vender y mostrar el resumen. No cambia los precios ni descuenta dinero.

## Archivos

- Proyecto_Electronica.sln y Proyecto_Electronica/: solución, proyecto, filtros y código comentado.
- inventario.dat: catálogo inicial con tres productos.
- datos_prueba/: copia del ejemplo, archivo inválido y listado legible de los datos iniciales.
- Informe_Proyecto.pdf e Informe_Proyecto.md: negocio, decisiones, memoria, archivo, alcance y fuentes.
- Tabla_Pruebas.md: entradas, resultados esperados y resultados obtenidos.
- Guia_Demostracion.md: recorrido del menú, depurador y respuestas a las doce preguntas.
- evidencias/: resultados de pruebas automáticas y transcripciones de consola, sin capturas inventadas.
- Debug/: ejecutable y símbolos generados con MASM/LINK de Visual Studio 2022.
- Compilar.cmd y Ejecutar.cmd: accesos auxiliares de compilación y ejecución.

En Visual Studio, la carpeta de trabajo del depurador está configurada como la raíz de esta entrega. Allí se leen inventario.dat y se escriben inventario.tmp e inventario.dat. Ejecutar.cmd usa la misma ubicación. Si se abre el ejecutable directamente desde otra carpeta, las rutas relativas corresponden a su carpeta de trabajo.

Para repetir la demostración desde cero, cerrar el programa y copiar datos_prueba\inventario_ejemplo.dat sobre inventario.dat. Esa acción sustituye el catálogo guardado por los tres productos iniciales.

## Verificación realizada

El código se ensambló y enlazó con ML/LINK de Visual Studio 2022, herramientas 14.44.35207, para x86 con información de depuración. Pasaron 113 pruebas del núcleo MASM y de archivos, además de una demostración real del menú en una consola de Windows y una segunda ejecución de recuperación.

La compilación mediante MSBuild de la solución encontró un error de acceso al detectar el SDK en este entorno. Por ello se verificó el ensamblador y el ejecutable directamente con las herramientas de Visual Studio y el SDK instalado. No se afirma haber completado una sesión del depurador de la interfaz de Visual Studio; la guía indica cómo realizarla y explicarla.

No se alteraron Microsoft Defender ni sus exclusiones. Las bibliotecas de terceros no se distribuyen como archivos separados en esta entrega.
