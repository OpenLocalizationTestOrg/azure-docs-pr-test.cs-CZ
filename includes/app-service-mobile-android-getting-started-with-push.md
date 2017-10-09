1. <span data-ttu-id="855e8-101">Ve vaší **aplikace** projekt, otevřete hello soubor `AndroidManifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="855e8-101">In your **app** project, open hello file `AndroidManifest.xml`.</span></span> <span data-ttu-id="855e8-102">V kódu hello v hello následující dva kroky, nahraďte  *`**my_app_package**`*  s názvem hello hello aplikace balíčku pro váš projekt.</span><span class="sxs-lookup"><span data-stu-id="855e8-102">In hello code in hello next two steps, replace *`**my_app_package**`* with hello name of hello app package for your project.</span></span> <span data-ttu-id="855e8-103">Toto je hodnota hello hello `package` atribut hello `manifest` značky.</span><span class="sxs-lookup"><span data-stu-id="855e8-103">This is hello value of hello `package` attribute of hello `manifest` tag.</span></span>
2. <span data-ttu-id="855e8-104">Přidejte následující nové oprávnění po hello existující hello `uses-permission` element:</span><span class="sxs-lookup"><span data-stu-id="855e8-104">Add hello following new permissions after hello existing `uses-permission` element:</span></span>

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. <span data-ttu-id="855e8-105">Přidejte následující kód po hello hello `application` počáteční značce:</span><span class="sxs-lookup"><span data-stu-id="855e8-105">Add hello following code after hello `application` opening tag:</span></span>

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. <span data-ttu-id="855e8-106">Soubor otevřete hello *ToDoActivity.java*a přidejte následující příkaz importu hello:</span><span class="sxs-lookup"><span data-stu-id="855e8-106">Open hello file *ToDoActivity.java*, and add hello following import statement:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. <span data-ttu-id="855e8-107">Přidejte následující soukromé proměnné toohello třída hello.</span><span class="sxs-lookup"><span data-stu-id="855e8-107">Add hello following private variable toohello class.</span></span> <span data-ttu-id="855e8-108">Nahraďte  *`<PROJECT_NUMBER>`*  s číslem projektu hello přiřazené službou Google tooyour aplikace v předchozím postupu hello.</span><span class="sxs-lookup"><span data-stu-id="855e8-108">Replace *`<PROJECT_NUMBER>`* with hello project number assigned by Google tooyour app in hello preceding procedure.</span></span>

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. <span data-ttu-id="855e8-109">Změnit definici hello *MobileServiceClient* z **privátní** příliš**veřejné statické**, takže ho teď vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="855e8-109">Change hello definition of *MobileServiceClient* from **private** too**public static**, so it now looks like this:</span></span>

        public static MobileServiceClient mClient;
7. <span data-ttu-id="855e8-110">Přidáte nové oznámení toohandle třídy.</span><span class="sxs-lookup"><span data-stu-id="855e8-110">Add a new class toohandle notifications.</span></span> <span data-ttu-id="855e8-111">V prohlížeči projektu, otevřete hello **src** > **hlavní** > **java** uzly a klikněte pravým tlačítkem na hello balíčku název uzlu.</span><span class="sxs-lookup"><span data-stu-id="855e8-111">In Project Explorer, open hello **src** > **main** > **java** nodes, and right-click hello package name node.</span></span> <span data-ttu-id="855e8-112">Klikněte na tlačítko **nový**a potom klikněte na **třída jazyka Java**.</span><span class="sxs-lookup"><span data-stu-id="855e8-112">Click **New**, and then click **Java Class**.</span></span>
8. <span data-ttu-id="855e8-113">V **název**, typ `MyHandler`a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="855e8-113">In **Name**, type `MyHandler`, and then click **OK**.</span></span>

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. <span data-ttu-id="855e8-114">V souboru MyHandler hello nahraďte deklarace třídy hello se:</span><span class="sxs-lookup"><span data-stu-id="855e8-114">In hello MyHandler file, replace hello class declaration with:</span></span>

        public class MyHandler extends NotificationsHandler {
10. <span data-ttu-id="855e8-115">Přidejte následující příkazy import pro hello hello `MyHandler` třídy:</span><span class="sxs-lookup"><span data-stu-id="855e8-115">Add hello following import statements for hello `MyHandler` class:</span></span>

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. <span data-ttu-id="855e8-116">Dále přidejte tento člen toohello `MyHandler` třídy:</span><span class="sxs-lookup"><span data-stu-id="855e8-116">Next add this member toohello `MyHandler` class:</span></span>

        public static final int NOTIFICATION_ID = 1;
12. <span data-ttu-id="855e8-117">V hello `MyHandler` třídy, přidejte následující kód toooverride hello hello **onRegistered** metodu, která registruje zařízení s centrem oznámení hello mobilní služby.</span><span class="sxs-lookup"><span data-stu-id="855e8-117">In hello `MyHandler` class, add hello following code toooverride hello **onRegistered** method, which registers your device with hello mobile service notification hub.</span></span>

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
       <span data-ttu-id="855e8-118">}</span><span class="sxs-lookup"><span data-stu-id="855e8-118">}</span></span>
13. <span data-ttu-id="855e8-119">V hello `MyHandler` třídy, přidejte následující kód toooverride hello hello **události onReceive** metodu, která způsobuje toodisplay hello oznámení, když je obdržena.</span><span class="sxs-lookup"><span data-stu-id="855e8-119">In hello `MyHandler` class, add hello following code toooverride hello **onReceive** method, which causes hello notification toodisplay when it is received.</span></span>

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
       <span data-ttu-id="855e8-120">}</span><span class="sxs-lookup"><span data-stu-id="855e8-120">}</span></span>
14. <span data-ttu-id="855e8-121">Zpět v souboru TodoActivity.java hello aktualizovat hello **onCreate** metoda hello *ToDoActivity* třídy třídu obslužné rutiny pro tooregister hello oznámení.</span><span class="sxs-lookup"><span data-stu-id="855e8-121">Back in hello TodoActivity.java file, update hello **onCreate** method of hello *ToDoActivity* class tooregister hello notification handler class.</span></span> <span data-ttu-id="855e8-122">Zkontrolujte, zda tooadd tento kód po hello *MobileServiceClient* je vytvořena instance.</span><span class="sxs-lookup"><span data-stu-id="855e8-122">Make sure tooadd this code after hello *MobileServiceClient* is instantiated.</span></span>

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    <span data-ttu-id="855e8-123">Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="855e8-123">Your app is now updated toosupport push notifications.</span></span>
