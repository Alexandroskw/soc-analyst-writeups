## ASCII
**American Standard Code of Information Interchange** se creó en 1963 y son los caracteres del alfabeto inglés, signos de puntuación y otros elementos; usa los números de 0 a 127 para representar el alfabeto.
Las letras y números van en orden, es decir, `a=61`, `b=62` y así sucesivamente
`A=41`, `B=42`, etc., `1=31`, `2=32`...
ASCII extendido intenta cubrir las variantes regionales, sobre todo europeas
- **ISO-8859-1**: para Europa del Oeste
- **ISO-8859-2**: para Europa del Este
## Unicode
Es un estándar de codificación de caracteres universal. Asigna puntos de código únicos a caracteres de todos los sistemas de escritura modernos e históricos en todo el mundo. Unicode actualmente define cerca de 157 mil caracteres y cerca de 4000 de ellos son secuencias de emojis
### UTF-8, UTF-16, UTF-32
- **UTF-8**: es el estándar más usado en el internet. Los caracteres de `U+0000` a `U+007F` son del ASCII original y utilizan la misma cantidad de bytes (1 byte) pero un emoji (🔥) puede utilizar hasta 4 bytes `U+1F525`
- **UTF-16**: utiliza entre 2 a 4 caracteres por caracter. Por ejemplo, `A` utilizaría `U+0041`, mientras que 🔥 utilizaría `U+D83D U+DD25`
- **UTF-32**: es el más inútil o derrochador, ya que cada caracter utiliza exactamente 4 bytes, es decir, `A` es codificado como `U+00000041` y el emoji 🔥 sería `U+0001F525`
