# Resumen del proceso

1. GR Coordinación carga un fichero JSON con todos los centros.
2. La aplicación separa los centros por zona y genera un HTML para cada responsable de zona.
3. Cada responsable de zona recibe su HTML por correo o WhatsApp, lo revisa y puede responder centros de su zona.
4. El responsable de zona pulsa **Distribuir divisiones**. Se genera un HTML por división con sus centros.
5. Cada responsable de división recibe su HTML, puede responder centros y pulsa **Distribuir centros**.
6. Se genera un HTML para cada coordinador con los centros que le corresponden.
7. El coordinador cumplimenta los datos y pulsa **Enviar resultado del centro**.
8. Cada respuesta llega como un fichero JSON a la carpeta de resultados de Google Drive.
9. GR Coordinación podrá revisar posteriormente todos los ficheros recibidos.

La distribución es progresiva: primero trabaja GR Coordinación, después las zonas, luego las divisiones y finalmente los coordinadores de centro.
