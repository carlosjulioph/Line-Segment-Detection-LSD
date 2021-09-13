# Line-Segment-Detection-LSD

LSD Line Segment Detection LSD es un detector de segmento de línea de tiempo lineal que proporciona resultados precisos de subpíxeles. Está diseñado para trabajar en cualquier imagen digital sin ajuste de parámetros. Controla su propio número de detecciones falsas: en promedio, se permite una falsa alarma por imagen. El método se basa en el método de Burns, Hanson y Riseman, y utiliza un enfoque de validación a-contrario de acuerdo con la teoría de Desolneux, Moisan y Morel. La versión descrita aquí incluye algunas mejoras adicionales sobre la descrita en el artículo original.

### INSTALACIÓN

pylsd es el envoltorio o wrapper para Python de [LSD - Detector de segmento de línea.](http://www.ipol.im/pub/art/2012/gjmr-lsd/)

Para instalar este módulo se hace uso de gestor de paquetes `pip`:

```
pip install pylsd-nova
```

### CÓMO DETERMINAR LOS SEGMENTOS DE LÍNEA

Inicialmente realizamos la importación del módulo `from pylsd import lsd`

Utilizamos la función `lines = lsd(img)`. `img` es una imagen a escala de grises, el retorno `lines`, es un arreglo Numpy `(N x 5)`, en donde cada fila corresponde a los segmentos de línea detectados. El formato de cada fila es el siguiente:

```
[point1.x, point1.y, point2.x, point2.y, width]
```

Es posible hacer uso de OpenCV o PIL para leer las imágenes.

```python
import cv2
import numpy as np
import os
from pylsd import lsd
fullName = 'test.jpg'
folder, imgName = os.path.split(fullName)
src = cv2.imread(fullName, cv2.IMREAD_COLOR)
gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)
lines = lsd(gray)
for i in range(lines.shape[0]):
    pt1 = (int(lines[i, 0]), int(lines[i, 1]))
    pt2 = (int(lines[i, 2]), int(lines[i, 3]))
    width = lines[i, 4]
    cv2.line(src, pt1, pt2, (0, 0, 255), int(np.ceil(width / 2)))
cv2.imwrite(os.path.join(folder, 'cv2_' + imgName.split('.')[0] + '.jpg'), src)

cv2.imshow('Segmentos de línea', src)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

El resultado `src` es el siguiente para nuestra imagen de prueba `test.jpg`:
<img src="https://github.com/carlosjulioph/Line-Segment-Detection-LSD/blob/main/Imágenes_Readme/Resultado.png" width="400">
