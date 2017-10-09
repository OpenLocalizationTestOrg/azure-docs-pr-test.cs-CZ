1. Ve vaší **aplikace** projekt, otevřete hello soubor `AndroidManifest.xml`. V kódu hello v hello následující dva kroky, nahraďte  *`**my_app_package**`*  s názvem hello hello aplikace balíčku pro váš projekt. Toto je hodnota hello hello `package` atribut hello `manifest` značky.
2. Přidejte následující nové oprávnění po hello existující hello `uses-permission` element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Přidejte následující kód po hello hello `application` počáteční značce:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Soubor otevřete hello *ToDoActivity.java*a přidejte následující příkaz importu hello:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. Přidejte následující soukromé proměnné toohello třída hello. Nahraďte  *`<PROJECT_NUMBER>`*  s číslem projektu hello přiřazené službou Google tooyour aplikace v předchozím postupu hello.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Změnit definici hello *MobileServiceClient* z **privátní** příliš**veřejné statické**, takže ho teď vypadá takto:

        public static MobileServiceClient mClient;
7. Přidáte nové oznámení toohandle třídy. V prohlížeči projektu, otevřete hello **src** > **hlavní** > **java** uzly a klikněte pravým tlačítkem na hello balíčku název uzlu. Klikněte na tlačítko **nový**a potom klikněte na **třída jazyka Java**.
8. V **název**, typ `MyHandler`a potom klikněte na **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. V souboru MyHandler hello nahraďte deklarace třídy hello se:

        public class MyHandler extends NotificationsHandler {
10. Přidejte následující příkazy import pro hello hello `MyHandler` třídy:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Dále přidejte tento člen toohello `MyHandler` třídy:

        public static final int NOTIFICATION_ID = 1;
12. V hello `MyHandler` třídy, přidejte následující kód toooverride hello hello **onRegistered** metodu, která registruje zařízení s centrem oznámení hello mobilní služby.

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
       }
13. V hello `MyHandler` třídy, přidejte následující kód toooverride hello hello **události onReceive** metodu, která způsobuje toodisplay hello oznámení, když je obdržena.

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
       }
14. Zpět v souboru TodoActivity.java hello aktualizovat hello **onCreate** metoda hello *ToDoActivity* třídy třídu obslužné rutiny pro tooregister hello oznámení. Zkontrolujte, zda tooadd tento kód po hello *MobileServiceClient* je vytvořena instance.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Vaše aplikace je teď aktualizovaný toosupport nabízená oznámení.
