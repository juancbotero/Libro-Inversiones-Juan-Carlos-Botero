1. Ingrese a la página web de Investor Relations Colombia (www.irc.gov.co)

2. De click en Deuda Pública - Deuda Interna GNC
<img width="1210" height="399" alt="image" src="https://github.com/user-attachments/assets/1b8c60ad-0a91-4bbe-b5ce-f134abfea5f5" />

3. Den click en Mercado Secundario
<img width="1585" height="357" alt="image" src="https://github.com/user-attachments/assets/a164daa5-2ae1-4ed1-85f5-2fd156e8dd7c" />

4. Luego den click en informe diario de TES
<img width="936" height="377" alt="image" src="https://github.com/user-attachments/assets/389da670-4b37-4db7-8810-96d9981e87d7" />

5. Se van a desplegar los años para los que está disponible la información. Dan click en el que deseen, que seguramente será el último disponible.

6. Luego se les desplegarán todos los informes diarios disponibles; dan click en el que desean revisar.

7. Descargan el informe buscado, el cual luce como el que aquí se muestra a continuación:
<img width="1302" height="736" alt="image" src="https://github.com/user-attachments/assets/eb96bba1-2c6a-408e-8361-b2245396c214" />

8. Pasan dicha tabla, que está en PDF, a Excel, tal como aparece en la hoja "CalculoCurva" en este archivo.

9. Tenga en cuenta que la información hasta la columna Duración viene de la tabla. La última columna "Precio Estimado" es resultante de un cálculo que se muestra más adelante.
 
10. En las celdas E43 de esa hoja hasta la H43 aparecen los parámetros Beta y tau. Ingrese unos valores inciales, por ejemplo, los disponibles el día anterior.
     Tenga en cuenta que son esos parámetros los que hay que obtener, pero es necesario que el optimizador (Solver) parta de unos valores iniciales.
    
 11. Posteriormente verá varios bloques de color aguamarina. Cada bloque de esos corresponde a cada uno de los papeles cuyo vencimiento aparece en la primera columna de la tabla.
     Dentro de cada bloque aguamarina verá los flujos que le faltan a cada papel. Por ejemplo, como la fecha del informe es marzo 17 de 2026 y el primer papel que aparece en la tabla
     tiene vencimiento 2 de junio de 2026, pues solo resta un flujo, el del vencimiento, que para todos los títulos se toma como igual a 100.
     
12. Un papel como el que vence el 30 de junio de 2032 tendrá 7 flujos restantes, así: cupones del 7% ($7 por cada $100 de valor facial) pagaderos cada año los 30 de junio, desde 2026 hasta 
      su vencimiento en 2032.
    
13. En cada bloque aguamarina, note que la columna B contiene el flujo que paga el papel, esto es, el cupón cada año y al final, el cupón más el nominal de $100. La columna C contiene
      los días al vencimiento para cada flujo. Estos días se calculan automáticamente; sin embargo, tenga cuidado porque es muy probable que le aparezcan días negativos. Esto se debe a que
      cuando convirtió la tabla del PDF al Excel, el sistema toma el año de los 1900s; es decir, si la fecha es 2 de junio de 2032, es muy probable que el sistema haya tomado 2 de junio de 1932.
    
14. La columna D convierte el tiempo en días de cada flujo entre la fecha actual y la fecha de pago, a años. Para esto simplemente se divide por la convención de 365.
 
15. La columna E muestra la tasa spot para cada uno de esos plazos. Note que en cada celda de la columna D está la fórmula de Nelson - Siegel, calculada usando los betas iniciales
      ingresados por usted. Evidentemente, esa tasa spot hasta este punto es apenas un valor de referencia, pero aún no corresponde al valor que forma la curva del día.
    
16. Luego la columna F muestra el factor de descuento, usando cada una de esas tasas spot o cero cupón.
 
17. Finalmente, la columna G muestra el valor presente de cada flujo, obtenido multiplicando el valor del flujo, por el factor de descuento.

18. Note que al final de cada bloque aguamarina hay una celda azul oscura. Esta celda es la suma de todos los valores presentes de los flujos que componen un determinado bono y por lo tanto
      es el estimativo del precio de ese papel. De nuevo, recurde que esto es hecho con unos betas que aún no han pasado por el optimizador.
    
19. Ahora sí podemos entender la columna I (PrecioEstimado) de la tabla de arriba. Esta trae todos los precios estimados de cada bloque aguamarina.

20. Al final, a partir de la fila 568, está una tabla resumen con dos datos importantes: el precio sucio observado para cada papel y el valor estimado. La diferencia entre los dos corresponde a lo
     que llamamos error. Posteriormente se calcula el error al cuadrado para cada papel, siguiendo la metodología MSE (Mínimo del Cuadrado de los Errores).
     
21. La celda A580 corresponde a la suma de todos los cuadrados de los errores. Es esa celda la que corresponde a la función objetivo en el Solver. Dicha celda se quiere minimizar, variando las
     celdas que contienen los betas. Asegúrese que el cajón "Haga las variables no-restringidas no-negativas" no esté marcado. Esto porque vamos a permitir que los betas puedan tomar valores negativos.
<img width="903" height="845" alt="image" src="https://github.com/user-attachments/assets/46a0ad45-ca06-4113-9c11-fc615bb03df7" />

22. Note que dentro de las restricciones se ha inclido que la celda E43, es decir, la que contine el coeficiente tau, sea igual a un determinado valor, en este caso igual a 3,7.

23. Una vez el Solver converge, entrega como resultado los nuevos betas que deberán aplicar para ese día.

24. En la hoja "DatosCurvaSpot" aparecen los datos con la tasa cero cupón, en este caso en
     términos efectivo anual, para cada uno de los plazos, en este caso desde 1 día hasta 
    10.950 días, es decir, 30 años.

25. En la hoja "Gráfica Curva Spot" se grafica la curva de tasas cero cupón vigente para 
     ese día.





