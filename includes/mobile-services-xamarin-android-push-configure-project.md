
1. V zobrazení řešení hello (nebo **Průzkumníku řešení** v sadě Visual Studio), klikněte pravým tlačítkem na hello **součásti** složky, klikněte na tlačítko **získat další komponenty...** , vyhledejte hello **klient Google Cloud Messaging** součástí a přidejte ji toohello projektu.
2. Otevřete soubor projektu ToDoActivity.cs hello a přidejte následující hello pomocí třídy toohello příkaz:
   
        using Gcm.Client;
3. V hello **ToDoActivity** třídy, přidejte následující kód nové hello: 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return hello current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return hello Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    To vám umožní tooaccess hello mobilního klienta instance z procesu služby obslužná rutina nabízené hello.
4. Přidejte následující kód toohello hello **OnCreate** metoda po hello **MobileServiceClient** je vytvořena:
   
       // Set hello current instance of TodoActivity.
       instance = this;
   
       // Make sure hello GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register hello app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Vaše **ToDoActivity** je nyní připraven pro přidání nabízených oznámení.

