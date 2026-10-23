# Cifrado y descifrado con ChaCha20

## Parte 1. Cifrar un mensaje

```python
from Crypto.Cipher import ChaCha20

# Mensaje que queremos cifrar
texto = "Este es un mensaje secreto que quiero cifrar"

# Clave de cifrado (32 bytes)
clave = b'z\xe8~"\xcayW\x14g\x18+\x1c+\xf9\x80\x06P\x9ej\x888\xb4G\xdf\xe4\xc50,\x8dY\x80\x19'

# Nonce (8 bytes)
nonce = b'\xd6\x7f6\xc7\xe8i*\xa4'

# Creamos el cifrador ChaCha20
cifrador = ChaCha20.new(key=clave, nonce=nonce)

# Ciframos el mensaje
texto_cifrado = cifrador.encrypt(texto.encode("utf-8"))

# Mostramos los resultados
print("Mensaje original:", texto)
print("Mensaje cifrado:", texto_cifrado.hex())
print("Longitud del mensaje cifrado:", len(texto_cifrado), "bytes")
```

## Parte 2. Descifrar un mensaje

```python
# PARTE 2 - DESCIFRAR UN MENSAJE

# Texto cifrado que nos da el ejercicio
texto_cifrado_2 = b'Ehq0\x83\xcb\x8fo\xab\xed\xd0S\x06\xcc\xbb\xecw\xe9\xec(\x1f\xc5E\xdb\x88\x18`W\xc3yQn\xad3\xec.\x08\x92\x8d\x8e\xbb%\x8f\x1a\xa6\xc9=\x15\x0f5\xaa'
print("Longitud del texto cifrado 2:", len(texto_cifrado_2))

# Creamos un nuevo descifrador con la misma clave y nonce
descifrador = ChaCha20.new(key=clave, nonce=nonce)

# Desciframos el mensaje
texto_descifrado = descifrador.decrypt(texto_cifrado_2)

# Prueba para comprobar que ChaCha20 funciona
prueba = ChaCha20.new(key=clave, nonce=nonce)

resultado_prueba = prueba.decrypt(texto_cifrado)

print("Prueba de descifrado:", resultado_prueba.decode("utf-8"))

# Mostramos el resultado
print("Mensaje descifrado:", texto_descifrado.decode("utf-8"))
```

## Parte extra. Sincronización del keystream

```python
# PARTE EXTRA - SINCRONIZACION DEL KEYSTREAM

# Mensaje cifrado recibido (faltan los primeros 8 bytes)
texto_cifrado_extra = b"\xad\xe2\x9a[C\xca\xa9\xadn\xf9\xaa)\x13\xc2X\x9e\x89\x19`C\xc4n\x1em\xf8?\xe7|\x0b\xd3\xdc\x8f\xacd\x85\x0c\xb8\xc3'\x1a\x12>"

# Creamos un nuevo descifrador
descifrador_extra = ChaCha20.new(key=clave, nonce=nonce)

# Saltamos los primeros 8 bytes perdidos
descifrador_extra.seek(8)

# Desciframos el mensaje recibido
mensaje_extra = descifrador_extra.decrypt(texto_cifrado_extra)

# Mostramos el resultado
print("Mensaje EXTRA descifrado:", mensaje_extra.decode("utf-8"))
```
