
1. <span data-ttu-id="28448-101">Hello otevřete projekt v Android Studio.</span><span class="sxs-lookup"><span data-stu-id="28448-101">Open hello project in Android Studio.</span></span>

2. <span data-ttu-id="28448-102">V **Project Exploreru** v nástroji Android Studio otevřete soubor ToDoActivity.java hello a přidejte následující příkazy pro import hello:</span><span class="sxs-lookup"><span data-stu-id="28448-102">In **Project Explorer** in Android Studio, open hello ToDoActivity.java file and add hello following import statements:</span></span>

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. <span data-ttu-id="28448-103">Přidejte následující metodu toohello hello **ToDoActivity** třídy:</span><span class="sxs-lookup"><span data-stu-id="28448-103">Add hello following method toohello **ToDoActivity** class:</span></span>

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

    <span data-ttu-id="28448-104">Tento kód vytvoří metoda toohandle hello procesu ověřování Google.</span><span class="sxs-lookup"><span data-stu-id="28448-104">This code creates a method toohandle hello Google authentication process.</span></span> <span data-ttu-id="28448-105">Zobrazí se dialogové okno zobrazí ID hello hello ověřeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="28448-105">A dialog displays hello ID of hello authenticated user.</span></span> <span data-ttu-id="28448-106">Pouze můžete přejít na úspěšné ověření.</span><span class="sxs-lookup"><span data-stu-id="28448-106">You can only proceed on a successful authentication.</span></span>

    > [!NOTE]
    > <span data-ttu-id="28448-107">Pokud používáte zprostředkovatele identity než Google, změňte hodnotu hello předán toohello **přihlášení** tooone metoda Dobrý den následující hodnoty: _MicrosoftAccount_, _Facebook_, _Twitter_, nebo _windowsazureactivedirectory_.</span><span class="sxs-lookup"><span data-stu-id="28448-107">If you are using an identity provider other than Google, change hello value passed toohello **login** method tooone of hello following values: _MicrosoftAccount_, _Facebook_, _Twitter_, or _windowsazureactivedirectory_.</span></span>

4. <span data-ttu-id="28448-108">V hello **onCreate** metoda, přidejte následující řádek kódu po hello kód, který vytvoří instanci hello hello `MobileServiceClient` objektu.</span><span class="sxs-lookup"><span data-stu-id="28448-108">In hello **onCreate** method, add hello following line of code after hello code that instantiates hello `MobileServiceClient` object.</span></span>

        authenticate();

    <span data-ttu-id="28448-109">Toto volání zahájí proces ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="28448-109">This call starts hello authentication process.</span></span>

5. <span data-ttu-id="28448-110">Přesunout hello zbývající kód po `authenticate();` v hello **onCreate** metoda tooa nové **createTable** metoda:</span><span class="sxs-lookup"><span data-stu-id="28448-110">Move hello remaining code after `authenticate();` in hello **onCreate** method tooa new **createTable** method:</span></span>

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

6. <span data-ttu-id="28448-111">tooensure přesměrování funguje podle očekávání, přidejte následující fragment hello _RedirectUrlActivity_ too_AndroidManifest.xml_:</span><span class="sxs-lookup"><span data-stu-id="28448-111">tooensure redirection works as expected, add hello following snippet of _RedirectUrlActivity_ too_AndroidManifest.xml_:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. <span data-ttu-id="28448-112">Přidejte redirectUriScheme too_build.gradle_ vaší aplikace Android.</span><span class="sxs-lookup"><span data-stu-id="28448-112">Add redirectUriScheme too_build.gradle_ of your Android application.</span></span>

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

8. <span data-ttu-id="28448-113">Přidejte do vaší build.gradle com.android.support:customtabs:23.0.1 toohello závislosti:</span><span class="sxs-lookup"><span data-stu-id="28448-113">Add com.android.support:customtabs:23.0.1 toohello dependencies in your build.gradle:</span></span>

      <span data-ttu-id="28448-114">závislosti {/ / … zkompilovat 'com.android.support:customtabs:23.0.1'}</span><span class="sxs-lookup"><span data-stu-id="28448-114">dependencies {        // ...        compile 'com.android.support:customtabs:23.0.1'    }</span></span>

9. <span data-ttu-id="28448-115">Z hello **spustit** nabídky, klikněte na tlačítko **spuštění aplikace** toostart hello a přihlaste se pomocí zprostředkovatele identity vybrané aplikace.</span><span class="sxs-lookup"><span data-stu-id="28448-115">From hello **Run** menu, click **Run app** toostart hello app and sign in with your chosen identity provider.</span></span>

> [!WARNING]
> <span data-ttu-id="28448-116">Hello schéma adresy URL uvedené rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="28448-116">hello URL Scheme mentioned is case-sensitive.</span></span>  <span data-ttu-id="28448-117">Ujistěte se, že všechny výskyty `{url_scheme_of_you_app}` použití hello stejné případu.</span><span class="sxs-lookup"><span data-stu-id="28448-117">Ensure that all occurrences of `{url_scheme_of_you_app}` use hello same case.</span></span>

<span data-ttu-id="28448-118">Pokud jste úspěšně přihlášeni, hello aplikace se budou spouštět bez chyby a by měl být schopný tooquery hello back endové služby a zkontrolujte toodata aktualizace.</span><span class="sxs-lookup"><span data-stu-id="28448-118">When you are successfully signed in, hello app should run without errors, and you should be able tooquery hello back-end service and make updates toodata.</span></span>
