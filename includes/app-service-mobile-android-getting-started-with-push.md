1. Ve vašem **aplikace** projektu, otevřete soubor `AndroidManifest.xml`. Přidejte následující kód po `application` počáteční značce:

    ```xml
    <service android:name=".ToDoMessagingService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
    <service android:name=".ToDoInstanceIdService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Otevřete soubor `ToDoActivity.java`a proveďte následující změny:

    - Přidejte příkaz importu:

        ```java
        import com.google.firebase.iid.FirebaseInstanceId;
        ```

    - Změnit definici `MobileServiceClient` z **privátní** k **privátní statickou**, takže ho teď vypadá takto:

        ```java
        private static MobileServiceClient mClient;
        ```

    - Přidat `registerPush` metoda:

        ```java
        public static void registerPush() {
            final String token = FirebaseInstanceId.getInstance().getToken();
            if (token != null) {
                new AsyncTask<Void, Void, Void>() {
                    protected Void doInBackground(Void... params) {
                        mClient.getPush().register(token);
                        return null;
                    }
                }.execute();
            }
        }
        ```

    - Aktualizace **onCreate** metodu `ToDoActivity` třídy. Nezapomeňte přidat tento kód po `MobileServiceClient` je vytvořena instance.

        ```java
        registerPush();
        ```

3. Přidání nové třídy pro zpracování oznámení. Otevřete v prohlížeči projektu klikněte **aplikace** > **java** > **vašeho projektu názvů** uzly a klikněte pravým tlačítkem na název uzlu balíčku. Klikněte na tlačítko **nový**a potom klikněte na **třída jazyka Java**. Zadejte název, `ToDoMessagingService`a pak klikněte na tlačítko OK. Potom nahraďte deklarace třídy se:

    ```java
    import android.app.Notification;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;

    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;

    public class ToDoMessagingService extends FirebaseMessagingService {

        private static final int NOTIFICATION_ID = 1;

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            String message = remoteMessage.getData().get("message");
            if (message != null) {
                sendNotification("Notification Hub Demo", message);
            }
        }

        private void sendNotification(String title, String messageBody) {
            PendingIntent contentIntent = PendingIntent.getActivity(this, 0, new Intent(this, ToDoActivity.class), 0);
            Notification.Builder notificationBuilder = new Notification.Builder(this)
                    .setSmallIcon(R.drawable.ic_launcher)
                    .setContentTitle(title)
                    .setContentText(messageBody)
                    .setContentIntent(contentIntent);
            NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
            if (notificationManager != null) {
                notificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
            }
        }
    }
    ```

4. Přidejte jinou třídu pro zpracování tokenu aktualizací. Vytvoření `ToDoInstanceIdService` java třídy a nahraďte deklarace třídy se:

    ```java
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class ToDoInstanceIdService extends FirebaseInstanceIdService {

        @Override
        public void onTokenRefresh() {
            ToDoActivity.registerPush();
        }
    }
    ```

Aplikace je nyní aktualizovat o podporu nabízených oznámení.
