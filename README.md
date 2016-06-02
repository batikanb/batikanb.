# batikanb.
 #include <LCD5110_Basic.h> // LCD kütüphanesini taslağımıza dahil ediyoruz.
     
    LCD5110 myGLCD(8,9,10,11,12); //LCD pinlerinin hangi Arduino pinlerine bağlı olduğunu belirtiyoruz.
    extern uint8_t SmallFont[]; // Küçük harflerle yazı yazmamızı sağlayan SmallFont'u tanıtıyoruz.
    extern uint8_t MediumNumbers[]; // Ortanca boy sayıları yazmamızı sağlayan MediumNumbers'ı tanıtıyoruz.
    extern uint8_t BigNumbers[]; // Büyük sayıları yazmamızı sağlayan BigNumbers'ı tanıtıyoruz.
     const int sespini = 0; //  mikrofonun gönderdiği sayısal değeri saklayacağımız değişken
     const int sespini1 = 0;
     const int sespini2 = 0;
     const int sespini3 = 0;
     int kuzey ;
     int guney ;
     int dogu ;
     int bati ;
     int yon;
     int yon1;
     int yon2;
     int KGYon;
     int GKYon;
     int DBYon;
     int BDYon;
    void setup()
    {
    Serial.begin(9600);
      
     pinMode(A0, INPUT); //Mikrofonun gönderdiği sinyalleri aldığımız pini giriş olarak ayarlıyoruz.
    pinMode(A1, INPUT);
    pinMode(A2, INPUT);
    pinMode(A3, INPUT);
    pinMode(7, OUTPUT); //LCD ekranın arkaplan aydınlatmasına giden pini çıkış olarak ayarlıyoruz.
    digitalWrite(7,HIGH); //Ekrana ışık veriyoruz.
    myGLCD.InitLCD(); //Ekranı başlatıyoruz.
    myGLCD.setContrast(90); //Kontrast'ı ayarlıyoruz
   
    }
     
    void loop()
    {
     readMics();
     }
     void readMics() {
    myGLCD.clrScr(); //Ekranı temizliyoruz.
    myGLCD.setFont(SmallFont); //Harfleri kullanacağımızı bildiriyoruz.
     myGLCD.print("sesinyonu",1,1); //Ekrana yazacağımız şeyi ve koordinatlarını giriyoruz.
   
    kuzey = analogRead(sespini); //gelen analog degerler
    guney = analogRead(sespini1);
    dogu = analogRead(sespini2);
    bati = analogRead(sespini3);
    
    kuzey = map(kuzey,0,1023,5,0); //map komutu analog verinin skalasını 0 5 arasında bir degere çevirir.
   guney = map(guney,0,1023,5,0);
   dogu = map(dogu,0,1023,5,0);
   bati = map(bati,0,1023,5,0);
   KGYon = kuzey-guney;
   DBYon = dogu-bati ;
   GKYon = guney-kuzey;
   BDYon = bati-dogu;
   ///////////////////////////////1. BOLGE//////////////////////////////////////////
   if (kuzey>guney & dogu>bati){
   KGYon = kuzey - guney;
   DBYon = dogu - bati;
   }
   else if (KGYon > DBYon ) {
     yon = 90*(KGYon /(KGYon + DBYon));
     myGLCD.setFont(SmallFont);
    myGLCD.print("KD",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); //   Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
   else if (DBYon > KGYon ) {
     yon = (90*(DBYon/(KGYon+DBYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("KD",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); //  Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    else if (DBYon=KGYon) {
     yon=45;
      myGLCD.setFont(SmallFont);
    myGLCD.print("KD",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    
   //////////////////////////////////////2.BOLGE//////////////////////////////////////////////
    if (kuzey > guney & bati > dogu){
   KGYon = kuzey-guney;
   BDYon = bati-dogu;
   }
   else if (KGYon > BDYon ) {
     yon = 90+(90*(KGYon/(KGYon+BDYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("KB",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
   else if (BDYon > KGYon ) {
     yon = 90+(90*(DBYon/(KGYon+DBYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("KB",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    else if (BDYon=KGYon) {
     yon=135;
      myGLCD.setFont(SmallFont);
    myGLCD.print("KB",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    
    /////////////////////////////////////////3. BOLGE///////////////////////////////////////////////
    if (guney>kuzey & bati>dogu){
   GKYon = guney-kuzey;
   BDYon = bati-dogu;
   }
   else if (GKYon > BDYon ) {
     yon = 180+(90*(GKYon/(GKYon+BDYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("GB",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
   else if (BDYon > GKYon ) {
     yon = 180+(90*(BDYon/(GKYon+BDYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("GB",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // EEkranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    else if (BDYon=GKYon) {
     yon=225;
      myGLCD.setFont(SmallFont);
    myGLCD.print("GB",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); //  Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    
    /////////////////////////////////////4. BOLGE ///////////////////////////////////////
     if (guney>kuzey & dogu>bati){
   GKYon = guney-kuzey;
   DBYon= bati-dogu;
   }
   else if (GKYon > DBYon ) {
     yon = 270+(90*(GKYon/(GKYon+DBYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("GD",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // EKRANA BAS Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
   else if (DBYon > GKYon ) {
     yon = 270+(90*(DBYon/(GKYon+DBYon)));
     myGLCD.setFont(SmallFont);
    myGLCD.print("GD",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    else if (DBYon=GKYon) {
     yon=315;
      myGLCD.setFont(SmallFont);
    myGLCD.print("GD",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20); // Ekranda yazacağımız şeyi ve koordinatlarını giriyoruz.
    }
    
   /////////////////////////////////////////////// ESITLIK ////////////////////////////////////////////
  if (kuzey = guney & bati> dogu) {
    yon = 180;
      myGLCD.setFont(SmallFont);
    myGLCD.print("bati",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20);
    }
    else if ( kuzey = guney & dogu> bati ) {
      yon = 0;
        myGLCD.setFont(SmallFont);
    myGLCD.print("dogu",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20);
    }
    else if ( bati = dogu & guney> kuzey){
      yon= 270;
        myGLCD.setFont(SmallFont);
    myGLCD.print("guney",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20);
    }
    else if ( bati = dogu & kuzey> guney){
      yon = 90;
        myGLCD.setFont(SmallFont);
    myGLCD.print("kuzey",50,1);
     myGLCD.setFont(BigNumbers); //Büyük sayıları kullanacağımızı bildiriyoruz.
    
    myGLCD.printNumI(yon,10,20);
    }
  
    delay(400); // gecikme komutu 400 ms.
    }
    
