1. <span data-ttu-id="a8138-101">Ve vašem **aplikace** projektu, otevřete soubor `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="a8138-101">In your **app** project, open the file `AndroidManifest.xml`.</span></span> <span data-ttu-id="a8138-102">V kódu v následujících dvou krocích, nahraďte  *`**my_app_package**`*  s názvem balíčku aplikace pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="a8138-102">In the code in the next two steps, replace *`**my_app_package**`* with the name of the app package for your project.</span></span> <span data-ttu-id="a8138-103">Toto je hodnota `package` atribut `manifest` značky.</span><span class="sxs-lookup"><span data-stu-id="a8138-103">This is the value of the `package` attribute of the `manifest` tag.</span></span>
2. <span data-ttu-id="a8138-104">Přidejte následující nová oprávnění po existující `uses-permission` element:</span><span class="sxs-lookup"><span data-stu-id="a8138-104">Add the following new permissions after the existing `uses-permission` element:</span></span>

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. <span data-ttu-id="a8138-105">Přidejte následující kód po `application` počáteční značce:</span><span class="sxs-lookup"><span data-stu-id="a8138-105">Add the following code after the `application` opening tag:</span></span>

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="a8138-106">Otevřete soubor *ToDoActivity.java*a přidejte následující příkaz importu:</span><span class="sxs-lookup"><span data-stu-id="a8138-106">Open the file *ToDoActivity.java*, and add the following import statement:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. <span data-ttu-id="a8138-107">Přidejte následující soukromé proměnné do třídy.</span><span class="sxs-lookup"><span data-stu-id="a8138-107">Add the following private variable to the class.</span></span> <span data-ttu-id="a8138-108">Nahraďte  *`<PROJECT_NUMBER>`*  číslem projektu Google přiřazené vaší aplikaci v předchozím postupu.</span><span class="sxs-lookup"><span data-stu-id="a8138-108">Replace *`<PROJECT_NUMBER>`* with the project number assigned by Google to your app in the preceding procedure.</span></span>

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. <span data-ttu-id="a8138-109">Změnit definici *MobileServiceClient* z **privátní** k **veřejné statické**, takže ho teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a8138-109">Change the definition of *MobileServiceClient* from **private** to **public static**, so it now looks like this:</span></span>

        public static MobileServiceClient mClient;
7. <span data-ttu-id="a8138-110">Přidání nové třídy pro zpracování oznámení.</span><span class="sxs-lookup"><span data-stu-id="a8138-110">Add a new class to handle notifications.</span></span> <span data-ttu-id="a8138-111">Otevřete v prohlížeči projektu klikněte **src** > **hlavní** > **java** uzly a klikněte pravým tlačítkem na název uzlu balíčku.</span><span class="sxs-lookup"><span data-stu-id="a8138-111">In Project Explorer, open the **src** > **main** > **java** nodes, and right-click the package name node.</span></span> <span data-ttu-id="a8138-112">Klikněte na tlačítko **nový**a potom klikněte na **třída jazyka Java**.</span><span class="sxs-lookup"><span data-stu-id="a8138-112">Click **New**, and then click **Java Class**.</span></span>
8. <span data-ttu-id="a8138-113">V **název**, typ `MyHandler`a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="a8138-113">In **Name**, type `MyHandler`, and then click **OK**.</span></span>

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. <span data-ttu-id="a8138-114">V souboru MyHandler nahraďte deklarace třídy se:</span><span class="sxs-lookup"><span data-stu-id="a8138-114">In the MyHandler file, replace the class declaration with:</span></span>

        public class MyHandler extends NotificationsHandler {
10. <span data-ttu-id="a8138-115">Přidejte následující příkazy pro import `MyHandler` třídy:</span><span class="sxs-lookup"><span data-stu-id="a8138-115">Add the following import statements for the `MyHandler` class:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. <span data-ttu-id="a8138-116">Dále přidejte tohoto člena `MyHandler` třídy:</span><span class="sxs-lookup"><span data-stu-id="a8138-116">Next add this member to the `MyHandler` class:</span></span>

        public static final int NOTIFICATION_ID = 1;
12. <span data-ttu-id="a8138-117">V `MyHandler` třídy, přidejte následující kód k přepsání **onRegistered** metodu, která registruje zařízení s centrem oznámení mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="a8138-117">In the `MyHandler` class, add the following code to override the **onRegistered** method, which registers your device with the mobile service notification hub.</span></span>

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       <span data-ttu-id="a8138-118">}</span><span class="sxs-lookup"><span data-stu-id="a8138-118">}</span></span>
13. <span data-ttu-id="a8138-119">V `MyHandler` třídy, přidejte následující kód k přepsání **události onReceive** metoda, což způsobí, že zobrazíte při obdržení oznámení.</span><span class="sxs-lookup"><span data-stu-id="a8138-119">In the `MyHandler` class, add the following code to override the **onReceive** method, which causes the notification to display when it is received.</span></span>

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       <span data-ttu-id="a8138-120">}</span><span class="sxs-lookup"><span data-stu-id="a8138-120">}</span></span>
14. <span data-ttu-id="a8138-121">Zpět v souboru TodoActivity.java aktualizace **onCreate** metodu *ToDoActivity* třída registrovat třídu obslužné rutiny oznámení.</span><span class="sxs-lookup"><span data-stu-id="a8138-121">Back in the TodoActivity.java file, update the **onCreate** method of the *ToDoActivity* class to register the notification handler class.</span></span> <span data-ttu-id="a8138-122">Nezapomeňte přidat tento kód po *MobileServiceClient* je vytvořena instance.</span><span class="sxs-lookup"><span data-stu-id="a8138-122">Make sure to add this code after the *MobileServiceClient* is instantiated.</span></span>

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    <span data-ttu-id="a8138-123">Aplikace je nyní aktualizovat o podporu nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="a8138-123">Your app is now updated to support push notifications.</span></span>
