# Prácticas recomendadas para las pruebas de TensorFlow

Estas son las prácticas recomendadas para probar el código en el [repositorio de TensorFlow](https://github.com/tensorflow/tensorflow) .

## Antes de empezar

Antes de contribuir con el código fuente a un proyecto de TensorFlow, revise el `CONTRIBUTING.md` en el repositorio de GitHub del proyecto. (Por ejemplo, consulte el [archivo CONTRIBUTING.md para el repositorio principal de TensorFlow](https://github.com/tensorflow/tensorflow/blob/master/CONTRIBUTING.md) ). Todos los contribuyentes de código deben firmar un [Acuerdo de licencia de colaborador](https://cla.developers.google.com/clas) (CLA).

## Principios generales

### Solo depende de lo que uses en tus reglas de CONSTRUCCIÓN

TensorFlow es una biblioteca grande y, según el paquete completo, al escribir una prueba unitaria para sus submódulos ha sido una práctica común. Sin embargo, esto deshabilita el análisis basado en dependencia de `bazel` Esto significa que los sistemas de integración continua no pueden eliminar de manera inteligente pruebas no relacionadas para ejecuciones de preenvío / posenvío. Si solo depende de los submódulos que está probando en su `BUILD` , ahorrará tiempo para todos los desarrolladores de TensorFlow y una gran cantidad de valiosa potencia de cálculo.

Sin embargo, modificar su dependencia de compilación para omitir los objetivos TF completos trae algunas limitaciones para lo que puede importar en su código Python. Ya no podrá usar el `import tensorflow as tf` declaración tf en sus pruebas unitarias. Pero esta es una compensación que vale la pena, ya que evita que todos los desarrolladores ejecuten miles de pruebas innecesarias.

### Todo el código debe tener pruebas unitarias

Para cualquier código que escriba, también debe escribir sus pruebas unitarias. Si escribe un nuevo archivo `foo.py` , debe colocar sus pruebas unitarias en `foo_test.py` y enviarlo dentro del mismo cambio. Apunta a una cobertura de prueba incremental&gt; 90% para todo tu código.

### Evite el uso de reglas de prueba nativas de bazel en TF

TF tiene muchas sutilezas al realizar pruebas. Hemos trabajado para ocultar todas esas complejidades en nuestras macros de bazel. Para evitar tener que lidiar con ellos, use lo siguiente en lugar de las reglas de prueba nativas. Tenga en cuenta que todos estos están definidos en `tensorflow/tensorflow.bzl` Para las pruebas CC, use `tf_cc_test` , `tf_gpu_cc_test` , `tf_gpu_only_cc_test` . Para las pruebas de Python, use `tf_py_test` o `gpu_py_test` . Si necesita algo realmente parecido a la `py_test` , utilice la definida en tensorflow.bzl en su lugar. Solo necesita agregar la siguiente línea en la parte superior del archivo BUILD: `load(“tensorflow/tensorflow.bzl”, “py_test”)`

### Tenga en cuenta dónde se ejecuta la prueba

Cuando escribe una prueba, nuestra infraestructura de prueba puede encargarse de ejecutar sus pruebas en CPU, GPU y aceleradores si las escribe en consecuencia. Tenemos pruebas automatizadas que se ejecutan en Linux, macos, windows, que tienen sistemas con o sin GPU. Simplemente debe elegir una de las macros enumeradas anteriormente y luego usar etiquetas para limitar dónde se ejecutan.

- `manual` etiqueta manual evitará que su prueba se ejecute en cualquier lugar. Esto incluye ejecuciones de prueba manuales que utilizan patrones como `bazel test tensorflow/…`

- `no_oss` excluirá que su prueba se ejecute en la infraestructura de prueba oficial de TF OSS.

- `no_mac` etiquetas no_mac o `no_windows` se pueden utilizar para excluir su prueba de los conjuntos de pruebas del sistema operativo relevantes.

- `no_gpu` etiqueta no_gpu se puede utilizar para excluir que su prueba se ejecute en conjuntos de pruebas de GPU.

### Verifique que las pruebas se ejecuten en los conjuntos de pruebas esperados

TF tiene bastantes conjuntos de pruebas. A veces, puede resultar confuso configurarlos. Puede haber diferentes problemas que hagan que sus pruebas se omitan de las compilaciones continuas. Por lo tanto, debe verificar que sus pruebas se estén ejecutando como se esperaba. Para hacer esto:

- Espere a que se completen los preenvíos en su Pull Request (PR).
- Desplácese hasta la parte inferior de su PR para ver las verificaciones de estado.
- Haga clic en el enlace "Detalles" en el lado derecho de cualquier cheque de Kokoro.
- Consulte la lista "Objetivos" para encontrar los objetivos recién agregados.

### Cada clase / unidad debe tener su propio archivo de prueba de unidad

Las clases de prueba separadas nos ayudan a aislar mejor las fallas y los recursos. Conducen a archivos de prueba mucho más cortos y fáciles de leer. Por lo tanto, todos sus archivos de Python deben tener al menos un archivo de prueba correspondiente (para cada `foo.py` , debe tener `foo_test.py` ). Para pruebas más elaboradas, como pruebas de integración que requieren diferentes configuraciones, está bien agregar más archivos de prueba.

## Velocidad y tiempos de ejecución

### La fragmentación debe usarse lo menos posible

En lugar de fragmentar, considere:

- Hacer sus pruebas más pequeñas
- Si lo anterior no es posible, divida las pruebas

La fragmentación ayuda a reducir la latencia general de una prueba, pero se puede lograr lo mismo dividiendo las pruebas en objetivos más pequeños. Dividir las pruebas nos brinda un nivel más fino de control en cada prueba, minimizando las ejecuciones de preenvío innecesarias y reduciendo la pérdida de cobertura de un buildcop que deshabilita un objetivo completo debido a un caso de prueba que se comporta mal. Además, la fragmentación incurre en costos ocultos que no son tan obvios, como ejecutar todo el código de inicialización de prueba para todos los fragmentos. Los equipos de infraestructura nos han remitido este problema como una fuente que crea una carga adicional.

### Las pruebas más pequeñas son mejores

Cuanto más rápido se ejecuten sus pruebas, más probabilidades habrá de que las personas realicen sus pruebas. Un segundo adicional para su prueba puede acumularse en horas de tiempo adicional que los desarrolladores y nuestra infraestructura dedican a ejecutar su prueba. Intente hacer que sus pruebas se ejecuten en menos de 30 segundos (¡en modo no opcional!) Y hágalos pequeños. Solo marque sus pruebas como medio como último recurso. ¡La infra no ejecuta pruebas grandes como preenvíos o posenvíos! Por lo tanto, solo escriba una prueba grande si va a organizar dónde se ejecutará. Algunos consejos para que las pruebas se ejecuten más rápido:

- Ejecute menos iteraciones de entrenamiento en su prueba
- Considere usar la inyección de dependencia para reemplazar las dependencias pesadas del sistema bajo prueba con falsificaciones simples.
- Considere usar datos de entrada más pequeños en pruebas unitarias
- Si nada más funciona, intente dividir su archivo de prueba.

### Los tiempos de prueba deben apuntar a la mitad del tiempo de espera del tamaño de prueba para evitar escamas

Con `bazel` , las pruebas pequeñas tienen tiempos de espera de 1 minuto. Los tiempos de espera medianos de la prueba son de 5 minutos. Las pruebas grandes simplemente no son ejecutadas por la prueba de TensorFlow infra. Sin embargo, muchas pruebas no son deterministas en cuanto a la cantidad de tiempo que toman. Por varias razones, sus pruebas pueden llevar más tiempo de vez en cuando. Y, si marca una prueba que se ejecuta durante 50 segundos en promedio como pequeña, su prueba fallará si se programa en una máquina con una CPU antigua. Por lo tanto, apunte a un tiempo de ejecución promedio de 30 segundos para pruebas pequeñas. Apunte a 2 minutos y 30 segundos de tiempo de ejecución promedio para pruebas medianas.

### Reducir el número de muestras y aumentar las tolerancias para la formación.

Las pruebas de ejecución lenta disuaden a los contribuyentes. Ejecutar el entrenamiento en las pruebas puede ser muy lento. Prefiera tolerancias más altas para poder usar menos muestras en sus pruebas para mantener sus pruebas lo suficientemente rápidas (2,5 minutos como máximo).

## Elimina el no determinismo y las escamas

### Escribe pruebas deterministas

Las pruebas unitarias siempre deben ser deterministas. Todas las pruebas que se ejecutan en TAP y guitarra deben ejecutarse de la misma manera cada vez, si no hay ningún cambio de código que las afecte. Para garantizar esto, a continuación se presentan algunos puntos a considerar.

### Siempre siembre cualquier fuente de estocasticidad

Cualquier generador de números aleatorios o cualquier otra fuente de estocasticidad puede causar escamas. Por lo tanto, cada uno de estos debe ser sembrado. Además de hacer que las pruebas sean menos escamosas, esto hace que todas las pruebas sean reproducibles. Las diferentes formas de establecer algunas semillas que puede necesitar establecer en las pruebas de TF son:

```python
# Python RNG

import random
random.seed(42)

# Numpy RNG

import numpy as np
np.random.seed(42)

# TF RNG

from tensorflow.python.framework import random_seed
random_seed.set_seed(42)
```

### Evite usar el `sleep` en pruebas multiproceso

El uso de `sleep` función del sueño en las pruebas puede ser una causa importante de descamación. Especialmente cuando se utilizan varios subprocesos, el uso de la suspensión para esperar a otro subproceso nunca será determinante. Esto se debe a que el sistema no puede garantizar ningún orden de ejecución de diferentes hilos o procesos. Por lo tanto, prefiera construcciones de sincronización deterministas como mutex.

### Compruebe si la prueba es escamosa

Las escamas hacen que los constructores y los desarrolladores pierdan muchas horas. Son difíciles de detectar y de depurar. A pesar de que existen sistemas automatizados para detectar escamas, necesitan acumular cientos de ejecuciones de prueba antes de poder denegar las pruebas con precisión. Incluso cuando detectan, deniegan la lista de sus pruebas y se pierde la cobertura de las mismas. Por lo tanto, los autores de las pruebas deben verificar si sus pruebas son inestables al escribir pruebas. Esto se puede hacer fácilmente ejecutando su prueba con la bandera: `--runs_per_test=1000`

### Utilice TensorFlowTestCase

TensorFlowTestCase toma las precauciones necesarias, como la siembra de todos los generadores de números aleatorios utilizados para reducir la descamación tanto como sea posible. A medida que descubramos y arreglemos más fuentes de escamas, todas se agregarán a TensorFlowTestCase. Por lo tanto, debes usar TensorFlowTestCase al escribir pruebas para tensorflow. TensorFlowTestCase se define aquí: `tensorflow/python/framework/test_util.py`

### Escribe pruebas herméticas

Las pruebas herméticas no necesitan recursos externos. Están llenos de todo lo que necesitan y simplemente inician cualquier servicio falso que puedan necesitar. Cualquier servicio que no sean sus pruebas es fuente de no determinismo. Incluso con una disponibilidad del 99% de otros servicios, la red puede fallar, la respuesta de rpc puede retrasarse y es posible que termine con un mensaje de error inexplicable. Los servicios externos pueden ser, entre otros, GCS, S3 o cualquier sitio web.
