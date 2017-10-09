
1. <span data-ttu-id="1b2f9-101">V zobrazení řešení hello (nebo **Průzkumníku řešení** v sadě Visual Studio), klikněte pravým tlačítkem na hello **součásti** složky, klikněte na tlačítko **získat další komponenty...** , vyhledejte hello **klient Google Cloud Messaging** součástí a přidejte ji toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="1b2f9-101">In hello Solution view (or **Solution Explorer** in Visual Studio), right-click hello **Components** folder, click  **Get More Components...**, search for hello **Google Cloud Messaging Client** component and add it toohello project.</span></span>
2. <span data-ttu-id="1b2f9-102">Otevřete soubor projektu ToDoActivity.cs hello a přidejte následující hello pomocí třídy toohello příkaz:</span><span class="sxs-lookup"><span data-stu-id="1b2f9-102">Open hello ToDoActivity.cs project file and add hello following using statement toohello class:</span></span>
   
        using Gcm.Client;
3. <span data-ttu-id="1b2f9-103">V hello **ToDoActivity** třídy, přidejte následující kód nové hello:</span><span class="sxs-lookup"><span data-stu-id="1b2f9-103">In hello **ToDoActivity** class, add hello following new code:</span></span> 
   
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
   
    <span data-ttu-id="1b2f9-104">To vám umožní tooaccess hello mobilního klienta instance z procesu služby obslužná rutina nabízené hello.</span><span class="sxs-lookup"><span data-stu-id="1b2f9-104">This enables you tooaccess hello mobile client instance from hello push handler service process.</span></span>
4. <span data-ttu-id="1b2f9-105">Přidejte následující kód toohello hello **OnCreate** metoda po hello **MobileServiceClient** je vytvořena:</span><span class="sxs-lookup"><span data-stu-id="1b2f9-105">Add hello following code toohello **OnCreate** method, after hello **MobileServiceClient** is created:</span></span>
   
       // Set hello current instance of TodoActivity.
       instance = this;
   
       // Make sure hello GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register hello app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

<span data-ttu-id="1b2f9-106">Vaše **ToDoActivity** je nyní připraven pro přidání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="1b2f9-106">Your **ToDoActivity** is now prepared for adding push notifications.</span></span>

