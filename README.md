// Biblioteca LCD
#include <LiquidCrystal.h>

// Inicializa a biblioteca LCD
LiquidCrystal LCD(12,11,5,4,3,2);

// Define o pino analogico A0 como entrada do Sensor de Temperatura
int SensorTempPino=0;

// Define o pino 8 para o alerta de temperatura baixa
int AlertaTempBaixa=8;
// Define o pino 13 para o alerta de temperatura alta
int AlertaTempAlta=13;
// Define o pino 6 como comporta
int comporta=6;
// Define temperatura alta como acima de 38 graus Celsius
int TempAlta=38;

void setup() {
	// Define o pino de alerta de temperatura baixa como saida
  	pinMode(AlertaTempBaixa, OUTPUT);
	// Define o pino de alerta de temperatura alta como saida
	pinMode(AlertaTempAlta, OUTPUT);

  // Define a quantidade de colunas e linhas do LCD
	LCD.begin(16,2);
      //-----------------------APAGA O DISPLAY-----------------------------------
    LCD.clear();
    //-----------------------APAGA O DISPLAY-----------------------------------
// Imprime a mensagem no LCD
	LCD.print("Temperatura:");
	// Muda o cursor para a primeira coluna e segunda linha do LCD
	LCD.setCursor(0,1);
	// Imprime a mensagem no LCD
	LCD.print("      C        ");

  
	
  
}

void loop() {
	// Faz a leitura da tensao no Sensor de Temperatura
int SensorTempTensao=analogRead(SensorTempPino);

  	// Converte a tensao lida
float Tensao=SensorTempTensao*5;
	Tensao/=1024;

  	// Converte a tensao lida em Graus Celsius
float TemperaturaC=(Tensao-0.5)*100;
  
	// Muda o cursor para a primeira coluna e segunda linha do LCD
	LCD.setCursor (0,1);

  	// Imprime a temperatura em Graus Celsius
	LCD.print(TemperaturaC);

//-----------------------APAGA O DISPLAY-----------------------------------
  LCD.clear ();
// Muda o cursor para a decima coluna e segunda linha do LCD 
  LCD.setCursor(1,0);
if (TemperaturaC>=TempAlta) {
  LCD.setCursor(1,0);
      LCD.print("BLOQUEADO!");
    }
  
  //Indica se pode ou nao pode entrar de acordo com a temp
  LCD.setCursor(1,0);
if (TemperaturaC<=TempAlta) {
  LCD.setCursor(1,0);
      LCD.print("LIBERADO!");
   }
  
	// Acende ou apaga os alertas luminosos de temperatura baixa e alta
if (TemperaturaC>=TempAlta) {
      digitalWrite(AlertaTempBaixa, LOW);
  		digitalWrite(AlertaTempAlta, HIGH);
    
    }
else if (TemperaturaC<=TempAlta){
  		digitalWrite(AlertaTempBaixa, HIGH);
  		digitalWrite(AlertaTempAlta, LOW);
  	}
else {
  		digitalWrite(AlertaTempBaixa, LOW);
  		digitalWrite(AlertaTempAlta, LOW);
    }
  
  LCD.setCursor (0,1);
  LCD.print(TemperaturaC); 
LCD.print("C");  
  
	
if (TemperaturaC<=TempAlta) {
      digitalWrite(comporta, HIGH);
    
    }
else  {
      digitalWrite (comporta, LOW);
  	}
  
  	// Aguarda 1 segundo
  	delay(1000);

}
