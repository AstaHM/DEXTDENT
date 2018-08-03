

Windows bitmap
Desarrollador
Microsoft en 1986
Información general
Extensión de archivo	.bmp o .dib
Tipo de MIME	image/x-ms-bmp (no oficial)
Uniform Type Identifier	com.microsoft.bmp
Número mágico	{{{Número_mágico}}}
Tipo de formato	Gráfico rasterizado
Formato abierto	?
[editar datos en Wikidata]
Windows bitmap (.BMP) es un formato de imagen de mapa de bits, propio del sistema operativo Microsoft Windows. Puede guardar imágenes de 24 bits (16,7 millones de colores), 8 bits (256 colores) y menos. Puede darse a estos archivos una compresión sin pérdida de calidad: la compresión RLE (Run-length encoding).

Los archivos de mapas de bits se componen de direcciones asociadas a códigos de color, uno para cada cuadro en una matriz de píxeles tal como se esquematizaría un dibujo de "colorea los cuadros" para niños pequeños. Normalmente, se caracterizan por ser muy poco eficientes en su uso de espacio en disco, pero pueden mostrar un buen nivel de calidad. A diferencia de los gráficos vectoriales al ser reescalados a un tamaño mayor, pierden calidad. Los archivos BMP no son utilizados en páginas web debido a su gran tamaño en relación a su resolucion.

Dependiendo de la profundidad de color que tenga la imagen cada píxel puede ocupar 1 o varios bytes. Generalmente se suelen transformar en otros formatos, como JPEG (fotografías), GIF o PNG (dibujos y esquemas), los cuales utilizan otros algoritmos para conseguir una mayor compresión (menor tamaño del archivo).

Los archivos comienzan (cabecera o header) con las letras 'BM' (0x42 0x4D), que lo identifica con el programa de visualización o edición. En la cabecera también se indica el tamaño de la imagen y con cuántos bytes se representa el color de cada píxel.

A continuación se detalla la estructura de la cabecera de un fichero .BMP

Bytes	Información
0, 1	Tipo de fichero "BM"
2, 3, 4, 5	Tamaño del archivo
6, 7	Reservado
8, 9	Reservado
10, 11, 12, 13	Inicio de los datos de la imagen
14, 15, 16, 17	Tamaño de la cabecera del bitmap
18, 19, 20, 21	Anchura (píxels)
22, 23, 24, 25	Altura (píxels)
26, 27	Número de planos
28, 29	Tamaño de cada punto
30, 31, 32, 33	Compresión (0=no comprimido)
34, 35, 36, 37	Tamaño de la imagen
38, 39, 40, 41	Resolución horizontal
42, 43, 44, 45	Resolución vertical
46, 47, 48, 49	Tamaño de la tabla de color
50, 51, 52, 53	Contador de colores importantes
El Bitmap de una imagen .BMP comienza a leerse desde abajo a arriba, es decir: en una imagen en 24 bits los primeros 3 bytes corresponden al primer píxel inferior izquierdo.
