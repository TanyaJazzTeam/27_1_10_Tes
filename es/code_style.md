# Guía de estilo del código de TensorFlow

## Estilo Python

Siga la [guía de estilo de Python de PEP 8](https://www.python.org/dev/peps/pep-0008/) , excepto que TensorFlow usa 2 espacios en lugar de 4. [Cumpla](https://www.pylint.org/) [con la Guía de estilo de Python de Google](https://github.com/google/styleguide/blob/gh-pages/pyguide.md) y use pylint para verificar los cambios de Python.

### pilón

Para instalar `pylint` y recuperar la definición de estilo personalizado de TensorFlow:

```bash

$ pip install pylint
$ wget -O /tmp/pylintrc https://raw.githubusercontent.com/tensorflow/tensorflow/master/tensorflow/tools/ci_build/pylintrc

```

Para comprobar un archivo con `pylint` :

```bash
$ pylint --rcfile=/tmp/pylintrc myfile.py
```

### Versiones de Python compatibles

TensorFlow admite Python&gt; = 3.5. Consulte la [guía de instalación](https://www.tensorflow.org/install) para obtener más detalles.

Consulta el [estado de compilación continua de](https://github.com/tensorflow/tensorflow/blob/master/README.md#continuous-build-status) TensorFlow para compilaciones oficiales y respaldadas por la comunidad.

## Estilo de codificación C ++

Los cambios en el código de TensorFlow C ++ deben ajustarse a la [Guía de estilo de Google C ++](https://google.github.io/styleguide/cppguide.html) . Use `clang-format` para verificar sus cambios de C / C ++.

Para instalar en Ubuntu 16+, haga lo siguiente:

```bash
$ apt-get install -y clang-format
```

Puede verificar el formato de un archivo C / C ++ con lo siguiente:

```bash
$ clang-format <my_cc_file> --style=google > /tmp/my_cc_file.cc
$ diff <my_cc_file> /tmp/my_cc_file.cc
```

## Otros idiomas

- [Guía de estilo de Google Java](https://google.github.io/styleguide/javaguide.html)
- [Guía de estilo de JavaScript de Google](https://google.github.io/styleguide/jsguide.html)
- [Guía de estilo de Google Shell](https://google.github.io/styleguide/shell.xml)
- [Guía de estilo de Google Objective-C](https://google.github.io/styleguide/objcguide.html)

## Convenciones y usos especiales de TensorFlow

### Operaciones de Python

Una *operación de* TensorFlow es una función que, dados los tensores de entrada, devuelve tensores de salida (o agrega una operación a un gráfico cuando se crean gráficos).

- El primer argumento debe ser tensores, seguido de parámetros básicos de Python. El último argumento es el `name` con un valor predeterminado de `None` .
- Los argumentos del tensor deben ser un solo tensor o un iterable de tensores. Es decir, un "Tensor o lista de tensores" es demasiado amplio. Consulte `assert_proper_iterable` .
- Las operaciones que toman tensores como argumentos deben llamar a `convert_to_tensor` para convertir entradas no tensoriales en tensores si están usando operaciones C ++. Tenga en cuenta que los argumentos todavía se describen como un `Tensor` de un tipo d específico en la documentación.
- Cada operación de Python debe tener un `name_scope` . Como se ve a continuación, pase el nombre de la operación como una cadena.
- Las operaciones deben contener un comentario extenso de Python con declaraciones Args y Returns que expliquen tanto el tipo como el significado de cada valor. Las posibles formas, tipos o rangos deben especificarse en la descripción. Consulte los detalles de la documentación.
- Para una mayor usabilidad, incluya un ejemplo de uso con entradas / salidas de la operación en la sección Ejemplo.
- Evite hacer un uso explícito de `tf.Tensor.eval` o `tf.Session.run` . Por ejemplo, para escribir lógica que depende del valor de Tensor, usa el flujo de control de TensorFlow. Alternativamente, restrinja la operación para que solo se ejecute cuando la ejecución ansiosa esté habilitada ( `tf.executing_eagerly()` ).

Ejemplo:

```python
def my_op(tensor_in, other_tensor_in, my_param, other_param=0.5,
          output_collections=(), name=None):
  """My operation that adds two tensors with given coefficients.

  Args:
    tensor_in: `Tensor`, input tensor.
    other_tensor_in: `Tensor`, same shape as `tensor_in`, other input tensor.
    my_param: `float`, coefficient for `tensor_in`.
    other_param: `float`, coefficient for `other_tensor_in`.
    output_collections: `tuple` of `string`s, name of the collection to
                        collect result of this op.
    name: `string`, name of the operation.

  Returns:
    `Tensor` of same shape as `tensor_in`, sum of input values with coefficients.

  Example:
    >>> my_op([1., 2.], [3., 4.], my_param=0.5, other_param=0.6,
              output_collections=['MY_OPS'], name='add_t1t2')
    [2.3, 3.4]
  """
  with tf.name_scope(name or "my_op"):
    tensor_in = tf.convert_to_tensor(tensor_in)
    other_tensor_in = tf.convert_to_tensor(other_tensor_in)
    result = my_param * tensor_in + other_param * other_tensor_in
    tf.add_to_collection(output_collections, result)
    return result
```

Uso:

```python
output = my_op(t1, t2, my_param=0.5, other_param=0.6,
               output_collections=['MY_OPS'], name='add_t1t2')
```
