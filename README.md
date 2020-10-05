PROCESSOR 16F887
    #include <xc.inc>
    ;configuración de los fuses
    CONFIG FOSC=INTRC_NOCLKOUT
    CONFIG WDTE=OFF
    CONFIG PWRTE=ON
    CONFIG MCLRE=OFF
    CONFIG CP=OFF
    CONFIG CPD=OFF
    CONFIG BOREN=OFF
    CONFIG IESO=OFF
    CONFIG FCMEN=OFF
    CONFIG LVP=OFF
    CONFIG DEBUG=ON
    CONFIG BOR4V=BOR40V
    CONFIG WRT=OFF
    
    PSECT udata
 mul:
    DS 1
    
PSECT resetVec,class=CODE,delta=2
    resetVect:
	PAGESEL main
	goto main
	
PSECT code 
pausa:
    MOVLW   1			
    MOVWF   0x20
    
suma:
    incfsz  0x20,f		
    GOTO    suma
suma2:
    INCfsz  0x20,f		
    GOTO    suma2
  
suma3:
    INCfsz  0x20,f		
    GOTO    suma3
     RETURN
    PSECT code
 main:
    BANKSEL PORTB
    BANKSEL TRISB
    clrf TRISB
    loop:
    movlw 0x1
    movwf PORTB
    REP1:
    RLF PORTB,0
    CALL pausa
    BTFSS PORTB,7
    GOTO REP2
    REP2:
    RRF PORTB,1
    CALL pausa
    BTFSS PORTB,0
    GOTO REP2
    GOTO REP1
    END
