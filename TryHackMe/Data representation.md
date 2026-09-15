## Colores
Los colores se representan con RGB. Cada letra representa 1-bit y cada uno de estos bits tiene dos estados: encendido y apagado (0 y 1) y en total tendríamos:
`2 x 2 x 2 = 8` que es 1 byte. Por lo tanto $$2^8=1byte=2^4*2^4=16*16=1*16^2+0*16^1+0*16^0=256$$
Por tanto, dos dígitos hex equivalen a 1 byte $$AB=1010 1110$$
Ya que $$A=1010$$
y $$B=1110$$
8 colores es muy limitado...
### Conversión
**Hexadecimal** es la forma más conveniente de usar colores en computación.
Para hacer una conversión se toma el número que está representado, por ejemplo A en hex es 10 en base-10. Se multiplica por la base
$$
D=10 * 16
$$
La base, en este caso 16, se eleva a la posición donde se encuentra el dígito. En este ejemplo solo se tiene uno, entonces se eleva a 0
$$
D=10*16^0
$$
Dando como resultado $$ D=10*1; D = 10$$

[[Encodeo de datos]]