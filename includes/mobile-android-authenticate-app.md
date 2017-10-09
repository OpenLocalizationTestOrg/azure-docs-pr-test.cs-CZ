
1. Hello otevřete projekt v Android Studio.

2. V **Project Exploreru** v nástroji Android Studio otevřete soubor ToDoActivity.java hello a přidejte následující příkazy pro import hello:

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. Přidejte následující metodu toohello hello **ToDoActivity** třídy:

        // You can choose any unique number here toodifferentiate auth providers from each other. Note this is hello same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

        private void authenticate() {
            // Login using hello Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check hello request code matches hello one we send in hello login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check hello error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    Tento kód vytvoří metoda toohandle hello procesu ověřování Google. Zobrazí se dialogové okno zobrazí ID hello hello ověřeného uživatele. Pouze můžete přejít na úspěšné ověření.

    > [!NOTE]
    > Pokud používáte zprostředkovatele identity než Google, změňte hodnotu hello předán toohello **přihlášení** tooone metoda Dobrý den následující hodnoty: _MicrosoftAccount_, _Facebook_, _Twitter_, nebo _windowsazureactivedirectory_.

4. V hello **onCreate** metoda, přidejte následující řádek kódu po hello kód, který vytvoří instanci hello hello `MobileServiceClient` objektu.

        authenticate();

    Toto volání zahájí proces ověřování hello.

5. Přesunout hello zbývající kód po `authenticate();` v hello **onCreate** metoda tooa nové **createTable** metoda:

        private void createTable() {

            // Get hello table instance toouse.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter toobind hello items with hello view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load hello items from Azure.
            refreshItemsFromTable();
        }

6. tooensure přesměrování funguje podle očekávání, přidejte následující fragment hello _RedirectUrlActivity_ too_AndroidManifest.xml_:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. Přidejte redirectUriScheme too_build.gradle_ vaší aplikace Android.

        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

8. Přidejte do vaší build.gradle com.android.support:customtabs:23.0.1 toohello závislosti:

      závislosti {/ / … zkompilovat 'com.android.support:customtabs:23.0.1'}

9. Z hello **spustit** nabídky, klikněte na tlačítko **spuštění aplikace** toostart hello a přihlaste se pomocí zprostředkovatele identity vybrané aplikace.

> [!WARNING]
> Hello schéma adresy URL uvedené rozlišuje velká a malá písmena.  Ujistěte se, že všechny výskyty `{url_scheme_of_you_app}` použití hello stejné případu.

Pokud jste úspěšně přihlášeni, hello aplikace se budou spouštět bez chyby a by měl být schopný tooquery hello back endové služby a zkontrolujte toodata aktualizace.
