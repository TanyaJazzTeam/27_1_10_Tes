# Contribuir al código de TensorFlow

Ya sea que esté agregando una función de pérdida, mejorando la cobertura de la prueba o escribiendo un RFC para un cambio de diseño importante, esta parte de la guía del colaborador lo ayudará a comenzar. Gracias por el trabajo y el interés en mejorar TensorFlow.

## Antes de empezar

Antes de contribuir con el código fuente a un proyecto de TensorFlow, revise el `CONTRIBUTING.md` en el repositorio de GitHub del proyecto. (Por ejemplo, consulte el [archivo CONTRIBUTING.md para el repositorio principal de TensorFlow](https://github.com/tensorflow/tensorflow/blob/master/CONTRIBUTING.md) ). Todos los contribuyentes de código deben firmar un [Acuerdo de licencia de colaborador](https://cla.developers.google.com/clas) (CLA).

Para evitar duplicar el trabajo, revise [las RFC actuales](https://github.com/tensorflow/community/tree/master/rfcs) y comuníquese con los desarrolladores en los foros de TensorFlow antes de comenzar a trabajar en una función no trivial. Somos algo selectivos cuando decidimos agregar una nueva funcionalidad, y la mejor manera de contribuir y ayudar al proyecto es trabajar en problemas conocidos.

## Problemas para nuevos contribuyentes

Los nuevos colaboradores deben buscar las siguientes etiquetas cuando busquen una primera contribución al código base de TensorFlow. Recomendamos encarecidamente que los nuevos colaboradores aborden primero los proyectos "fáciles" y "buenos primeros números"; esto ayuda al colaborador a familiarizarse con el flujo de trabajo de contribución y a que los desarrolladores principales se familiaricen con el colaborador.

- `good first issue`
- `easy`
- `contributions welcome`

Si está interesado en contratar un equipo para ayudar a abordar un problema a gran escala o una nueva función, envíe un correo electrónico al [grupo de desarrolladores @](https://groups.google.com/a/tensorflow.org/forum/#!forum/developers) y revise nuestra lista actual de RFC.

## Revisión de código

Las nuevas funciones, las correcciones de errores y cualquier otro cambio en la base del código están sujetos a revisión del código.

Revisar el código contribuido al proyecto como solicitudes de extracción es un componente crucial del desarrollo de TensorFlow. Alentamos a todos a que comiencen a revisar el código enviado por otros desarrolladores, especialmente si la función es algo que probablemente usará.

A continuación, se incluyen algunas preguntas para tener en cuenta durante el proceso de revisión del código:

- *¿Queremos esto en TensorFlow?* ¿Es probable que se utilice? ¿A usted, como usuario de TensorFlow, le gusta el cambio y tiene la intención de usarlo? ¿Este cambio se encuentra en el alcance de TensorFlow? ¿Valdrá la pena el costo de mantener una nueva característica por sus beneficios?

- *¿El código es coherente con la API de TensorFlow?* ¿Están las funciones, clases y parámetros públicos bien nombrados y diseñados de forma intuitiva?

- *¿Incluye documentación?* ¿Todas las funciones públicas, clases, parámetros, tipos de devolución y atributos almacenados se nombran de acuerdo con las convenciones de TensorFlow y están claramente documentados? ¿Se describe la nueva funcionalidad en la documentación de TensorFlow y se ilustra con ejemplos, siempre que sea posible? ¿La documentación se muestra correctamente?

- *¿Es el código legible por humanos?* ¿Tiene poca redundancia? ¿Deberían mejorarse los nombres de las variables para mayor claridad o coherencia? ¿Deben agregarse comentarios? ¿Deberían eliminarse los comentarios por ser inútiles o superfluos?

- *¿Es eficiente el código?* ¿Podría reescribirse fácilmente para que funcione de manera más eficiente?

- ¿El código es *compatible* con versiones anteriores de TensorFlow?

- ¿El nuevo código agregará *nuevas dependencias* en otras bibliotecas?

## Pruebe y mejore la cobertura de la prueba

Las pruebas unitarias de alta calidad son una piedra angular del proceso de desarrollo de TensorFlow. Para ello, utilizamos imágenes de Docker. Las funciones de prueba tienen el nombre apropiado y son responsables de verificar la validez de los algoritmos, así como las diferentes opciones del código.

Todas las funciones nuevas y las correcciones de errores *deben* incluir una cobertura de prueba adecuada. También agradecemos las contribuciones de nuevos casos de prueba o mejoras a las pruebas existentes. Si descubre que nuestras pruebas existentes no están completas, incluso si eso no está causando un error actualmente, presente un problema y, si es posible, una solicitud de extracción.

Para obtener detalles específicos de los procedimientos de prueba en cada proyecto de TensorFlow, consulte los `README.md` y `CONTRIBUTING.md` en el repositorio del proyecto en GitHub.

De particular preocupación en *las pruebas adecuadas* :

- ¿Se prueban *todas las funciones y clases públicas?*
- ¿Se *prueba un conjunto razonable de parámetros* , sus valores, tipos de valor y combinaciones?
- ¿Las pruebas validan que el *código es correcto* y que está *haciendo lo que la documentación dice* que debe hacer el código?
- Si el cambio es una corrección de errores, ¿se incluye una *prueba de no regresión?*
- ¿Las pruebas *pasan la* construcción de integración continua?
- ¿Las pruebas *cubren todas las líneas de código?* De no ser así, ¿son las excepciones razonables y explícitas?

Si encuentra algún problema, considere ayudar al colaborador a comprender esos problemas y resolverlos.

## Mejorar los mensajes de error o los registros

Agradecemos las contribuciones que mejoran los mensajes de error y el registro.

## Flujo de trabajo de contribución

Las contribuciones al código (corrección de errores, nuevo desarrollo, mejora de la prueba) siguen un flujo de trabajo centrado en GitHub. Para participar en el desarrollo de TensorFlow, configure una cuenta de GitHub. Luego:

1. Bifurque el repositorio en el que planea trabajar. Vaya a la página del repositorio del proyecto y use el botón *Bifurcación.* Esto creará una copia del repositorio, bajo su nombre de usuario. (Para obtener más detalles sobre cómo bifurcar un repositorio, consulte [esta guía](https://help.github.com/articles/fork-a-repo/) ).

2. Clone el repositorio en su sistema local.

    `$ git clone git@github.com:your-user-name/project-name.git`

3. Crea una nueva rama para albergar tu trabajo.

    `$ git checkout -b new-branch-name`

4. Trabaja en tu nuevo código. Escribe y ejecuta pruebas.

5. Confirme sus cambios.

    `$ git add -A`

    `$ git commit -m "commit message here"`

6. Envíe sus cambios a su repositorio de GitHub.

    `$ git push origin branch-name`

7. Abra una *solicitud de extracción* (PR). Vaya al repositorio del proyecto original en GitHub. Habrá un mensaje sobre su rama recientemente enviada, preguntándole si desea abrir una solicitud de extracción. Siga las indicaciones, *compare entre repositorios* y envíe el PR. Esto enviará un correo electrónico a los confirmadores. Es posible que desee considerar enviar un correo electrónico a la lista de correo para obtener más visibilidad. (Para obtener más detalles, consulte la [guía de GitHub sobre relaciones públicas](https://help.github.com/articles/creating-a-pull-request-from-a-fork) .

8. Los mantenedores y otros colaboradores *revisarán su PR* . Participe en la conversación e intente *realizar los cambios solicitados* . Una vez que se aprueba el RP, el código se fusionará.

*Antes de trabajar en su próxima contribución* , asegúrese de que su repositorio local esté actualizado.

1. Configure el control remoto ascendente. (Solo tiene que hacer esto una vez por proyecto, no siempre).

    `$ git remote add upstream git@github.com:tensorflow/project-repo-name`

2. Cambie a la rama principal local.

    `$ git checkout master`

3. Baje los cambios desde aguas arriba.

    `$ git pull upstream master`

4. Empuje los cambios a su cuenta de GitHub. (Opcional, pero una buena práctica).

    `$ git push origin master`

5. Cree una nueva rama si está comenzando un nuevo trabajo.

    `$ git checkout -b branch-name`

Recursos adicionales de `git` y GitHub:

- [Documentación de Git](https://git-scm.com/documentation)
- [Flujo de trabajo de desarrollo de Git](https://docs.scipy.org/doc/numpy/dev/gitwash/development_workflow.html)
- [Resolución de conflictos de fusión](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/) .

## Lista de verificación de colaboradores

- Lea las pautas de contribución.
- Lea el Código de conducta.
- Asegúrese de haber firmado el Acuerdo de licencia de colaborador (CLA).
- Verifique si sus cambios son consistentes con las pautas.
- Verifique si sus cambios son consistentes con el estilo de codificación de TensorFlow.
- Ejecute pruebas unitarias.
