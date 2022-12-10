#include <16f877.h> //Modelo de PIC

#use delay(clock=4000000) //frecuencia del cristal
#fuses XT //tipo de cristal
#fuses Nowdt //sin watchdog
#fuses noprotect //sin proteccion de cf
#fuses nobrownout //sin brownout reset
#fuses put //power up timer

int8 m1; //variable entera de 8 bits
int8 m2;
int8 timer; //variable entero 8 bits
int8 Poscaler;
int8 state;

//Programa principal
void main(){
  //Configuramos el puerto D como Salida
  set_tris_B(0B00000000);

  //Motor 2
  //Giro del Motor
  output_low(pin_B2); //Pone el pin B0 en bajo
  output_high(pin_B3); //Pone el pin B1 en alto

  //Configuramos PWM
  set_tris_C(0B00000000); //Configuramos el puerto C como salida (Salida 1 Entrada 0)

  Timer=255;
  Poscaler=1;
  //Configuracion del Timer 2
  setup_timer_2(t2_div_by_16, timer, Poscaler);

  //Configurar el modulo CCP1
  setup_ccp1(ccp_pwm);
  setup_ccp2(ccp_pwm);
  m1 = 0;
  m2 = 0;

  //Estado incial maquina de estados
  state = 'a';

  while (1) {

    switch(state){
      case 'a':
        output_low(pin_B0);
        output_high(pin_B1);
        m1 = 255;
        set_pwm1_duty(m1);
        set_pwm2_duty(m2);
        delay_ms(10000); //Duracion del PWM en ms
        state = 'b';
      break;

      case 'b':
        set_pwm1_duty(m1);
        set_pwm2_duty(m2);
        delay_ms(100); //Duracion del PWM en ms
        m1 = m1 - 1;  //Disminuimos el ancho de pulso del ccp_pwm

          if(m1 <= 0){
            output_low(pin_B1);
            set_pwm1_duty(m1);
            delay_ms(1000);
            state = 'c';
          }
      break;

      case 'c':
        set_pwm1_duty(m1);
        set_pwm2_duty(m2);
        delay_ms(100); //Duracion del PWM en ms
        m2 = m2 + 1;  //Incrementamos el ancho de pulso del ccp_pwm

          if(m2 >= 255){
            delay_ms(10000);
            m2 = 0;
            set_pwm2_duty(m2);
            state = 'a';
          }
      break;
    }//Fin de switch
  }//fin de ciclo
} //Fin del programa principal
