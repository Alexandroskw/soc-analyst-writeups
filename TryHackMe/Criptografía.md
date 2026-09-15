## Cifrado simétrico
la misma llave se utiliza para cifrar el mensaje y la misma llave se usa para descifrar el mensaje, es la misma llave para ambos casos.
- **Es rápido**: pueden procesar grandes cantidades de datos rápidamente
- **Es eficiente**: perfecto para cifrar archivos, discos duros y tráfico de red
Tiene un problema, se genera un **problema de distribución de clave**
## Cifrado asimétrico
El cifrado asimétrico utiliza dos claves ligadas matemáticamente
- **Clave pública**: Cualquiera la puede utilizar
- **Clave privada**: solo una persona la puede utilizar
Si se cifra algo con la clave pública de alguien, solo se puede descifrar con su clave privada.
Si cifro con mi clave privada, solo alguien con mi clave pública puede descifrarlo
### Certificados
- Contiene la clave pública de un sitio web
- Indica a quien corresponde
- Una autoridad de confianza lo firma digitalmente, llamada Autoridad de Certificación (CA).
- El navegador verifica que un CA de confianza lo ha firmado
- el navegador revisa que siga siendo válido
- Si todo es correcto, el navegador muestra un candado y confía en la clave pública
