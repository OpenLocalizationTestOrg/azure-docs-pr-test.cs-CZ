
1. <span data-ttu-id="2fca1-101">V zobrazení řešení (nebo **Průzkumníku řešení** v sadě Visual Studio), klikněte pravým tlačítkem myši **součásti** složku, klikněte na tlačítko **získat další komponenty...** , vyhledejte **klient Google Cloud Messaging** součástí a přidejte ji do projektu.</span><span class="sxs-lookup"><span data-stu-id="2fca1-101">In the Solution view (or **Solution Explorer** in Visual Studio), right-click the **Components** folder, click  **Get More Components...**, search for the **Google Cloud Messaging Client** component and add it to the project.</span></span>
2. <span data-ttu-id="2fca1-102">Otevřete soubor projektu ToDoActivity.cs a přidejte následující pomocí příkazu pro třídu:</span><span class="sxs-lookup"><span data-stu-id="2fca1-102">Open the ToDoActivity.cs project file and add the following using statement to the class:</span></span>
   
        using Gcm.Client;
3. <span data-ttu-id="2fca1-103">V **ToDoActivity** třídy, přidejte následující kód nové:</span><span class="sxs-lookup"><span data-stu-id="2fca1-103">In the **ToDoActivity** class, add the following new code:</span></span> 
   
        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();
   
        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }
   
    <span data-ttu-id="2fca1-104">Můžete se připojit k instanci mobilního klienta z procesu nabízené obslužné rutiny služby.</span><span class="sxs-lookup"><span data-stu-id="2fca1-104">This enables you to access the mobile client instance from the push handler service process.</span></span>
4. <span data-ttu-id="2fca1-105">Přidejte následující kód, který **OnCreate** metoda, po **MobileServiceClient** je vytvořena:</span><span class="sxs-lookup"><span data-stu-id="2fca1-105">Add the following code to the **OnCreate** method, after the **MobileServiceClient** is created:</span></span>
   
       // Set the current instance of TodoActivity.
       instance = this;
   
       // Make sure the GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register the app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

<span data-ttu-id="2fca1-106">Vaše **ToDoActivity** je nyní připraven pro přidání nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="2fca1-106">Your **ToDoActivity** is now prepared for adding push notifications.</span></span>

