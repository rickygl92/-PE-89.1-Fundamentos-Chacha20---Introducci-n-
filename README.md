# Preguntas de ejercicio CHACHA20.



### 1. ¿Cuál es el texto cifrado resultante?

Al cifrar el mensaje `Este es un mensaje secreto que quiero cifrar` mediante ChaCha20, obtenemos el siguiente resultado en hexadecimal:

```text
4568713083cb8f6fabedd05306ccbbec77e9ec281fc545db88186057c379516ead33ec2e08929f92b8778a0d
```

El mensaje cifrado tiene una longitud de **44 bytes**, igual que el texto original.

### 2. ¿Qué tamaño tiene el nonce que has utilizado para cifrar el texto plano?

El nonce utilizado en el programa tiene un tamaño de **8 bytes (64 bits)**.

```python
nonce = b'\xd6\x7f6\xc7\xe8i*\xa4'
```

### 3. ¿Puedes utilizar un nonce de mayor tamaño (ej. 12 bytes o 16 bytes)?

Sí, pero depende del tamaño. La librería PyCryptodome permite utilizar nonces de **8 o 12 bytes** con ChaCha20 y de **24 bytes** con XChaCha20.

Sin embargo, no admite directamente nonce de 16 bytes en esta implementación.

### 4. ¿Qué implicaciones de seguridad tiene utilizar un nonce de mayor tamaño?

Utilizar un nonce de mayor tamaño permite disponer de más combinaciones posibles, reduciendo el riesgo de repetir accidentalmente un nonce cuando se genera de forma aleatoria.

Por ejemplo, un nonce de 12 bytes ofrece más combinaciones que uno de 8 bytes.

Sin embargo, utilizar un nonce más largo no significa necesariamente que el cifrado sea más fuerte. Lo importante es **no repetir nunca el mismo nonce con la misma clave**.

### 5. ¿Qué riesgos de seguridad existen si se reutiliza la misma clave y el mismo nonce para cifrar dos mensajes diferentes?

Si reutilizamos la misma clave y el mismo nonce, ChaCha20 genera exactamente el mismo *keystream*.

Esto supone un problema grave de seguridad, porque un atacante podría relacionar ambos mensajes cifrados y obtener información sobre su contenido.

Si conoce uno de los mensajes originales, podría recuperar información del otro mensaje.

Por eso, **nunca debemos reutilizar la misma combinación de clave y nonce**.

### 6. ¿Qué diferencias existen entre ChaCha20 y ChaCha20-Poly1305 en cuanto a la confidencialidad e integridad de la información?

La principal diferencia es que **ChaCha20 proporciona confidencialidad**, mientras que **ChaCha20-Poly1305 proporciona confidencialidad, integridad y autenticación del mensaje**.

| Característica | ChaCha20 | ChaCha20-Poly1305 |
|---|---|---|
| Confidencialidad | Sí | Sí |
| Integridad | No | Sí |
| Autenticación del mensaje | No | Sí |
| Detecta modificaciones | No | Sí |

ChaCha20-Poly1305 incorpora un código de autenticación de 16 bytes que permite comprobar si el mensaje ha sido modificado o falsificado.

### 7. ¿Cuál es el texto plano resultante?

Al descifrar `texto_cifrado_2` utilizando la clave y el nonce proporcionados en el programa, obtenemos el siguiente mensaje:

```text
Este es un mensaje secreto que quiero que descifres
```

Este es el texto original que estaba oculto en el mensaje cifrado del segundo apartado.

### 8. Busca el método de la librería ChaCha20 que estés utilizando para sincronizar el keystream con el del receptor. ¿Cuál es el texto plano resultante?

El método utilizado es **`seek()`**, que permite desplazar el keystream hasta una posición determinada.

En nuestro programa utilizamos:

```python
descifrador_extra.seek(8)
```

Esto permite saltar los primeros 8 bytes del keystream para sincronizarnos con el mensaje recibido, ya que los primeros 8 bytes del texto cifrado se han perdido durante la transmisión.

El texto plano recuperado es:

```text
saje ha sufrido un error en la transmision
```

El mensaje aparece incompleto al principio porque se han perdido los primeros 8 bytes.

El método `seek(8)` permite descifrar correctamente la parte restante del mensaje, pero no recuperar los bytes perdidos.
