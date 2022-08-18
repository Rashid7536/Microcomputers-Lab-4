/*
 * File:   lab4.c
 * Author: Rashid
 *
 * Created on July 1, 2022, 11:44 AM
 */


// PIC16F877A Configuration Bit Settings

// 'C' source line config statements

// CONFIG

    #pragma config FOSC = HS        // Oscillator Selection bits (HS oscillator)
    #pragma config WDTE = OFF       // Watchdog Timer Enable bit (WDT disabled)
    #pragma config PWRTE = OFF      // Power-up Timer Enable bit (PWRT disabled)
    #pragma config BOREN = OFF      // Brown-out Reset Enable bit (BOR disabled)
    #pragma config LVP = OFF        // Low-Voltage (Single-Supply) In-Circuit Serial Programming Enable bit (RB3 is digital I/O, HV on MCLR must be used for programming)
    #pragma config CPD = OFF        // Data EEPROM Memory Code Protection bit (Data EEPROM code protection off)
    #pragma config WRT = OFF        // Flash Program Memory Write Enable bits (Write protection off; all program memory may be written to by EECON control)
    #pragma config CP = OFF         // Flash Program Memory Code Protection bit (Code protection off)

// #pragma config statements should precede project file includes.
// Use project enums instead of #define for ON and OFF.

    #define _XTAL_FREQ 20000000
    #include <xc.h>

    void __interrupt() isr(void){

        INTF = 0; //Interrupt Flag disable to keep interrupt looping
      
        RC3 = 1; //Output Motor enable
        __delay_ms(500);
        RC3 = 0; //Output Motor disable
        __delay_ms(500);

    }

    void main(void) {
        TRISB = 0xFF; //PortB enable as input
        TRISC = 0x00; //PortC enable as output

        GIE = 1; //Global Interrupt Enable
        INTE = 1; //External Interrupt Enable
        INTF = 0; //External Interrupt Flag disable
        INTEDG = 0; // Interrupt works in Falling Edge

        RC2 = 0; // Input Motor
        RC3 = 0; // Output Motor
       
        RB2 = 1; // Sensor 001
        RB3 = 1; // Sensor 002
        RB0 = 1; // Sensor 003

           while(1){
           
              if( (RB2 == 0) && (RB3 == 1) && (RB0 == 1 ) ){ //When Sensor 001 is logic Low

                  RC2 = 1; //Input Motor
                  RC3 = 0; //Output Motor

              }

              if ( (RB2 == 0) && (RB3 == 0) && (RB0 == 1) ){ //When Sensor 001 and 002 are giving logic low

                  RC2 = 1; //Input Motor
                  RC3 = 0; //Output Motor

              }

              else { 
              
                  //If any of the above conditions aren't met,
                  //the following code will run which disables all the devices
                  RC2 = 0; //Input Motor
                  RC3 = 0; //Output Motor

              }
          }  
    }
