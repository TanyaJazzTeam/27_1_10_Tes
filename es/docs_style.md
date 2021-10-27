# Guía de estilo de la documentación de TensorFlow

## Mejores prácticas

- Céntrese en la intención y la audiencia del usuario.
- Use palabras cotidianas y mantenga las oraciones cortas.
- Utilice una construcción, redacción y uso de mayúsculas coherentes en las oraciones.
- Utilice encabezados y listas para que sus documentos sean más fáciles de escanear.
- La [Guía de estilo de Google Developer Docs](https://developers.google.com/style/highlights) es útil.

## Reducción

Con algunas excepciones, TensorFlow usa una sintaxis de Markdown similar a [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/) (GFM). En esta sección, se explican las diferencias entre la sintaxis de GFM Markdown y el Markdown que se usa para la documentación de TensorFlow.

### Escribir sobre el código

#### Menciones de código en línea

Coloca <code>`backticks`</code> alrededor de los siguientes símbolos cuando se usan en el texto:

- Nombres de argumentos: <code>`input`</code> , <code>`x`</code> , <code>`tensor`</code>
- Nombres de tensor devueltos: <code>`output`</code> , <code>`idx`</code> , <code>`out`</code>
- Tipos de datos: <code>`int32`</code> , <code>`float`</code> , <code>`uint8`</code>
- Otros nombres de operaciones hacen referencia en el texto: <code>`list_diff()`</code> , <code>`shuffle()`</code>
- Nombres de clase: <code>`tf.Tensor`</code> , <code>`Strategy`</code>
- Nombre de archivo: <code>`image_ops.py`</code> , <code>`/path_to_dir/file_name`</code>
- Expresiones o condiciones matemáticas: <code>`-1-input.dims() &lt;= dim &lt;= input.dims()`</code>

#### Bloques de código

Utilice tres comillas invertidas para abrir y cerrar un bloque de código. Opcionalmente, especifique el lenguaje de programación después del primer grupo de comillas invertidas, por ejemplo:

<pre><code>
```python
# some python code here
```
</code></pre>

### Enlaces en Markdown

#### Vínculos entre archivos en este repositorio

Utilice enlaces relativos entre archivos en un repositorio. Esto funciona en [tensorflow.org](https://www.tensorflow.org) y [GitHub](https://github.com/tensorflow/docs/tree/master/site/en) :<br> <code>\[Custom layers\]\(../tutorials/eager/custom_layers.ipynb\)</code> produce [capas personalizadas](https://www.tensorflow.org/tutorials/eager/custom_layers) en el sitio.

#### Enlaces a la documentación de la API

Los enlaces API se convierten cuando se publica el sitio. Para vincular a la página de referencia de la API de un símbolo, incluya la ruta completa del símbolo entre comillas invertidas:

- <code>`tf.data.Dataset`</code> produce [`tf.data.Dataset`](https://www.tensorflow.org/api_docs/python/tf/data/Dataset)

Para la API de C ++, use la ruta del espacio de nombres:

- `tensorflow::Tensor` produce [tensorflow :: Tensor](https://www.tensorflow.org/api_docs/cc/class/tensorflow/tensor)

#### enlaces externos

Para los enlaces externos, incluidos los archivos en <var>https://www.tensorflow.org</var> que no están en el `tensorflow/docs` , utilice enlaces estándar de Markdown con el URI completo.

Para vincular al código fuente, use un vínculo que comience con <var>https://www.github.com/tensorflow/tensorflow/blob/master/</var> , seguido del nombre del archivo que comienza en la raíz de GitHub.

Este esquema de nomenclatura de URI garantiza que <var>https://www.tensorflow.org</var> pueda reenviar el enlace a la rama del código correspondiente a la versión de la documentación que está viendo.

No incluya parámetros de consulta de URI en el enlace.

Las rutas de archivo usan guiones bajos para los espacios, por ejemplo, `custom_layers.ipynb` .

Incluya la extensión del archivo en los enlaces para usar en el sitio *y* GitHub, por ejemplo,<br> <code>\[Custom layers\]\(../tutorials/eager/custom_layers.ipynb\)</code> .

### Matemáticas en Markdown

Puede usar MathJax dentro de TensorFlow al editar archivos de Markdown, pero tenga en cuenta lo siguiente:

- MathJax se procesa correctamente en [tensorflow.org](https://www.tensorflow.org) .
- MathJax no se procesa correctamente en GitHub.
- Esta notación puede resultar desagradable para desarrolladores desconocidos.
- Por coherencia, [tensorflow.org](https://www.tensorflow.org) sigue las mismas reglas que Jupyter / Colab.

Use <code>$$</code> alrededor de un bloque de MathJax:

<pre><code>$$
E=\frac{1}{2n}\sum_x\lVert (y(x)-y'(x)) \rVert^2
$$</code></pre>

$$ E = \ frac {1} {2n} \ sum_x \ lVert (y (x) -y '(x)) \ rVert ^ 2 $$

Envuelva expresiones MathJax en línea con <code>$ ... $</code> :

<pre><code>
This is an example of an inline MathJax expression: $ 2 \times 2 = 4 $
</code></pre>

Este es un ejemplo de una expresión MathJax en línea: $ 2 \ times 2 = 4 $

<code>\( ... \)</code> también funcionan para matemáticas en línea, pero el formulario $ a veces es más legible.

Nota: Si necesita usar un signo de dólar en el texto o en expresiones MathJax, aplique una barra diagonal al principio: `\$` . Los signos de dólar dentro de los bloques de código (como los nombres de las variables Bash) no necesitan escaparse.

## Estilo prosa

Si va a escribir o editar partes sustanciales de la documentación narrativa, lea la [Guía de estilo de documentación para desarrolladores de Google](https://developers.google.com/style/highlights) .

### Principios del buen estilo

- *Revise la ortografía y la gramática en sus contribuciones.* La mayoría de los editores incluyen un corrector ortográfico o tienen un complemento de corrección ortográfica disponible. También puede pegar su texto en un documento de Google u otro software de documentos para una revisión ortográfica y gramatical más sólida.
- *Use una voz informal y amigable.* Escribe la documentación de TensorFlow como una conversación, como si estuvieras hablando con otra persona uno a uno. Use un tono de apoyo en el artículo.

Nota: Ser menos formal no significa ser menos técnico. Simplifique su prosa, no el contenido técnico.

- *Evite renuncias, opiniones y juicios de valor.* Palabras como "fácilmente", "simplemente" y "simple" están cargadas de suposiciones. Algo puede parecerle fácil, pero difícil para otra persona. Trate de evitarlos siempre que sea posible.
- *Utilice oraciones sencillas y directas sin una jerga complicada.* Las oraciones compuestas, las cadenas de cláusulas y los modismos específicos de la ubicación pueden hacer que el texto sea difícil de entender y traducir. Si una oración se puede dividir en dos, probablemente debería hacerlo. Evite el punto y coma. Use listas de viñetas cuando sea apropiado.
- *Proporcione contexto.* No use abreviaturas sin explicarlas. No menciones proyectos que no sean de TensorFlow sin vincularlos. Explique por qué el código está escrito como está.

## Guía de uso

### Ops

Utilice `# ⇒` lugar de un solo signo igual cuando desee mostrar lo que devuelve una operación.

```python
# 'input' is a tensor of shape [2, 3, 5]

(tf.expand_dims(input, 0))  # ⇒ [1, 2, 3, 5]
```

### Tensores

Cuando se habla de un tensor en general, no escriba en mayúscula la palabra *tensor* . Cuando se habla del objeto específico que se proporciona o devuelve de una operación, entonces debe escribir con mayúscula la palabra *Tensor* y agregar comillas inversas alrededor de ella porque está hablando de un objeto `Tensor`

No uses la palabra *Tensores* (plural) para describir múltiples `Tensor` menos que realmente estés hablando de un objeto `Tensors` En su lugar, diga "una lista (o colección) de `Tensor` ".

Use la palabra *forma* para detallar las dimensiones de un tensor y muestre la forma entre corchetes con comillas invertidas. Por ejemplo:

<pre><code>
If `input` is a three-dimensional tensor with shape `[3, 4, 3]`, this operation
returns a three-dimensional tensor with shape `[6, 8, 6]`.
</code></pre>
