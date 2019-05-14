//# Sensores-de-presion-arduino-processing-aplicacion-en-ingenieria-civil
//Utilización de sensores de presión para el correcto funcionamiento de apuntalamientos durante la construcción de vigas de hormigón. Lectura de datos con arduino y procesamiento de datos e interfaz gráfica con processing
//CODIGO PARA LECTURA DE DATOS DE ARDUINO DE TRES SENSORES DE PRESIÓN
int v;
int w;
int x;
void setup() {
  Serial.begin(9600);
  delay(1000);
}

void loop() {
  v=analogRead(A0);
  w=analogRead(A1);
  x=analogRead(A2);
  Serial.print(v);
  Serial.print(',');
  Serial.print(w);
  Serial.print(',');
  Serial.print(x);
  Serial.println('.');
  
  delay(40);

}
//CODIGO PARA PROCESAMIENTO DE DATOS E INTERFAZ GRÁFICA TRES SENSORES DE PRESIÓN EN PROCESSING
import processing.serial.*;
Serial puerto;
float v;
float w;
float x;
float voltajev;
float voltajew;
float voltajex;
float VCC=4.98;
float R_DIV=3230.0;
int n;
void setup() {
  size(1300,800);
  printArray(Serial.list());
  puerto=new Serial(this, Serial.list()[0],9600);

}
void draw() { 
  background(#45ACAF);
  PImage img;
  img = loadImage("2.jpg");
  image(img,500,200,100,350);
  image(img,750,200,100,350);
  image(img,1000,200,100,350);
  textSize(50);
  text("Ingresar cargas en kg",0.1*width,0.1*height);

  if(n >= 0 && n <= 9){
   
    text(n,0.6*width,0.1*height);
    textSize(30);
    text("Reacciones calculadas",0.05*width,0.75*height);
    text("Reacciones leídas",0.05*width,0.8*height);
    text(n*250+"g",500,0.75*height);
    text(n*590+"g",750,0.75*height);
    text(n*250+"g",1000,0.75*height);
    fill(#1FFF1C);
    text("ATENCIÓN, se está trabajando con el "+int((n*100)/31.1)+"% de la capacidad del elemento",0.1*width,0.2*height);
    fill(#050505);
  }
  else {
    text("Solo numero",0.6*width,0.1*height);
    
  } 
  String dato=puerto.readStringUntil('.');
if (dato!=null){
float[] valores=float(split(dato, ','));
if (valores !=null && valores.length==3) {
v=valores[0];
w=valores[1];
x=valores[2];

 if (v!=0)
 {
   float voltajev=(v)*VCC/1023.0;
   float fsrR1=R_DIV*((VCC/voltajev)-1.0);
   float fuerza1;
   float fsrG1=1.0/fsrR1;
   if(fsrR1<=600)
   fuerza1=((fsrG1-0.00075)/0.000000326399);
   else
   fuerza1=(fsrG1/0.000000642857);
     if (fuerza1<20)
     {
       text(0+"g",500,0.8*height);
     }
     else
     {
     text(nfs(fuerza1*10,4,1)+"g",500,0.8*height);
    
     }
   if ((n*250)>1.10*fuerza1*10){
     fill(#FC0026);
     text("SUBIR",500,0.85*height);
     fill(#050505);
   }
   else
   {
     if ((n*250)<0.9*fuerza1*10){
       fill(#FC0026);
       text("BAJAR",500,0.85*height);
       fill(#050505);
     }
     else
     {
       fill(#1FFF1C);
       text("CORRECTO",500,0.85*height);
       fill(#050505);
       PImage img2;
       
      img2 = loadImage("descarga.png"); // Se debe tener los archivos de las imagenes en la misma carpeta del archivo del codigo
      image(img2,500,700,100,100);
     
     }
   }
  }
  else
  {
  
  }
if (w!=0)
 {
   float voltajew=(w)*VCC/1023.0;
   float fsrR2=R_DIV*((VCC/voltajew)-1.0);
   float fuerza2;
   float fsrG2=1.0/fsrR2;
   if(fsrR2<=600)
   fuerza2=(fsrG2-0.00075)/0.000000326399;
   else
   fuerza2=fsrG2/0.000000642857;
     if (fuerza2<15)
     {
       text(0+"g",750,0.8*height);
     }
     else
     {
     text(nfs(fuerza2*10,4,1)+"g",750,0.8*height);
     
     }
         if ((n*590)>1.10*fuerza2*10){
           fill(#FC0026);
         text("SUBIR",750,0.85*height);
         fill(#050505);
         }
         else
         {
         if ((n*590)<0.9*fuerza2*10){
           fill(#FC0026);
         text("BAJAR",750,0.85*height);
         fill(#050505);
         }
         else
         {
           fill(#1FFF1C);
         text("CORRECTO",750,0.85*height);
         fill(#050505);
         PImage img2;
         img2 = loadImage("descarga.png");
         image(img2,750,700,100,100);
      
          }
         }
 

  }
  else
  {

  }
if (x!=0)
 {
   float voltajex=(x)*VCC/1023.0;
   float fsrR3=R_DIV*((VCC/voltajex)-1.0);
   float fuerza3;
   float fsrG3=1.0/fsrR3;
   if(fsrR3<=600)
   fuerza3=(fsrG3-0.00075)/0.000000326399;
   else
   fuerza3=fsrG3/0.000000642857;
     if (fuerza3<20)
       {
          text(0+"g",1000,0.8*height);
       }
       else
       {
       text(nfs(fuerza3*10,4,1)+"g",1000,0.8*height);
     
       }
       if ((n*250)>1.10*fuerza3*10){
         fill(#FC0026);
         text("SUBIR",1000,0.85*height);
         fill(#050505);
         }
         else
         {
         if ((n*250)<0.9*fuerza3*10){
           fill(#FC0026);
         text("BAJAR",1000,0.85*height);
         fill(#050505);
         }
         else
         {
           fill(#1FFF1C);
         text("CORRECTO",1000,0.85*height);
         fill(#050505);
         PImage img2;
         img2 = loadImage("descarga.png");
         image(img2,1000,700,100,100);
      
          }
         }

  }
  else
  {

  }

}
}
}

void keyPressed() {
  // get the value of the key pressed
  n = int(key);  // int('0') = 48
  n = n - 48;
  if(n >= 0 && n <= 9){
    println("Valid digit: " + n);
  }
  else {
    println("Invalid you pressed " + key);
  }

}
