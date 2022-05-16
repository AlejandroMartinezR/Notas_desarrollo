# Novedades en via

### Por hacer

> No cerrar evento de manera inmediata  
    * Pomodoros:
        * 



### Algoritmo Inicio Vista

Incia vista  
Coloca el nombre a el panel  
_vista_llama funcion get_datos_seguimiento()
    llama al _controlador_ funcion get_datos_full() [no envia datos]  
        consulta todas la novedad cuyo estado sea diferente de 1 y 5  
        FOR Recorre todos los eventos (array)
            revisa la tabla de relacion para validar el tipo de servicio  
            Si el tipo de servicio es 2 o 3
                Entonces consulta el detalle en la tabla VARADOS EN VIA  
            SINO  
                Entonces consulta el detalle en la tabla ACCIDENTES  
            FIN SI  
            Consulta seguimiento realizados asociados al evento
        FIN FOR
        return Objeto $novedades_via
_vista_ llam funcion calculos_ico($novedades_via)  
    Consulta tabla gesvia_tiempos_respuesta $tiempos_respuesta
    FOR recorre todos los eventos $novedades_via  
        SI El campo tiempo_ICO_id = 1  (sin revisar distancia)  
            Entonces  
                toma la dirrecion del evento
                toma la dirrecion del patio
                calcula la distancia
                encuentra el rango para asignar el tiempo de respuesta  
                llama al _controlador_ save_tiempo_ico() [envia el id de la novedad y el tiempo]
                    Actualiza tiempo para respues ICO
        SI NO  
            calcula la fecha fin del evento
                SI la fecha_actual > fecha_vencimiento
                    Entonces
                        llama al _controlador_ update_tiempo_ico_vencimiento() [envia el id de la novedad y el tiempo] 
                            Actualiza campo aplica_ICO = 1 (aplica ICO)
                            Actualiza estado de la novedad a 1
                            <del>Actualiza fecha fin del evento</del> 
                            Inserta seguimiento  
                FIN SI           
                                     
        FIN SI  
    FIN FOR  
    
$datos = _vista_llama funcion get_datos_seguimiento()  
_vista_llama funcion render_vista_activos($datos)  
    FOR recorre eventos novedades en via  
        SI $datos == '' Entonces  
            muestra mensaje sin eventos activos a la hora  
        SI NO Entonces  
            Limpia paneles
            captura tipo de novedad  
                2 = varado en via
                1 = Accidente  
                3 = Sirci  
            calcula tiempo transcurrido
            Captura estado de la novedad
            Segun el tipo de evento pinta vista en su panel correspondiente     
    FIN FOR  
    
    Inicia contador Jquery  TimeCircles
    
             
### FIN Algoritmo Inicio Vista

### Algoritmo Finalizar evento
_vista_ presiona btn Finalizar evento  
LLama _vista_ funcion finalizar_Evento(id_novedad)  
    Solicita confirmacion de finalizacion del evento  
        SI confirma accion ENTONCES
            llama funcion _controlador_ finalizar_evento(id_novedad)  
                consulta novedad_via
                actualiza campo_aplica_ico = 0
                actualiza estado = 5
                SI la novedad en via tiene un servicio ENTONCES  
                    consulta el id del servicio  
                        SI servicio = 1 (grua) ENTONCES  
                            consulta placa grua
                            libera grua tabla de moviles
                        SI NO servicio = 2 ENTONCES  
                            libera placa tabla moviles
                        FIN SI
                FIN SI
    Actualiza logs
    
### FIN Algoritmo Finalizar evento
        
        
    
