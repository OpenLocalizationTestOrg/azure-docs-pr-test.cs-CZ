
<span data-ttu-id="0e6ec-101">Hello předchozí příklad ukázal u standardní přihlášení, které vyžaduje klient toocontact hello obou hello identity zprostředkovatele a hello back-end služby Azure při každém spuštění aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-101">hello previous example showed a standard sign-in, which requires hello client toocontact both hello identity provider and hello back-end Azure service every time hello app starts.</span></span> <span data-ttu-id="0e6ec-102">Tato metoda je neefektivní a může mít problémy související s využití, pokud mnoho zákazníků zkuste toostart aplikace současně.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-102">This method is inefficient, and you can have usage-related issues if many customers try toostart your app simultaneously.</span></span> <span data-ttu-id="0e6ec-103">Lepším řešením je, že toocache hello autorizační token vrácený hello služby Azure a zkuste to toouse to před použitím nejprve u založenou na poskytovateli přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-103">A better approach is toocache hello authorization token returned by hello Azure service, and try toouse this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="0e6ec-104">Můžete mezipaměti hello token vystavený hello back-end služby Azure bez ohledu na to, jestli používáte ověřování klienta spravovat nebo spravované služby.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-104">You can cache hello token issued by hello back-end Azure service regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="0e6ec-105">Tento kurz používá službu spravovat ověřování.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-105">This tutorial uses service-managed authentication.</span></span>
>
>

1. <span data-ttu-id="0e6ec-106">Otevřete soubor ToDoActivity.java hello a přidejte následující příkazy pro import hello:</span><span class="sxs-lookup"><span data-stu-id="0e6ec-106">Open hello ToDoActivity.java file and add hello following import statements:</span></span>

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. <span data-ttu-id="0e6ec-107">Přidejte následující členy toohello hello `ToDoActivity` třídy.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-107">Add hello following members toohello `ToDoActivity` class.</span></span>

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. <span data-ttu-id="0e6ec-108">V souboru ToDoActivity.java hello, přidejte následující definice hello hello `cacheUserToken` metoda.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-108">In hello ToDoActivity.java file, add hello following definition for hello `cacheUserToken` method.</span></span>

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    <span data-ttu-id="0e6ec-109">Tato metoda ukládá v souboru předvoleb, která je označena privátní hello ID uživatele a token.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-109">This method stores hello user ID and token in a preference file that is marked private.</span></span> <span data-ttu-id="0e6ec-110">To by měla chránit mezipaměti toohello přístup tak, aby jiné aplikace na zařízení hello nemají toohello tokenu přístupu.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-110">This should protect access toohello cache so that other apps on hello device do not have access toohello token.</span></span> <span data-ttu-id="0e6ec-111">Hello předvoleb je v izolovaném prostoru pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-111">hello preference is sandboxed for hello app.</span></span> <span data-ttu-id="0e6ec-112">Ale pokud někdo získá přístup toohello zařízení, je možné, že získají přístup toohello tokenu mezipaměti jinými způsoby.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-112">However, if someone gains access toohello device, it is possible that they may gain access toohello token cache through other means.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0e6ec-113">Dále je možné chránit hello token s šifrování, pokud data tooyour tokenu přístupu jsou považovány za vysoce citlivé a někdo může získat přístup k toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-113">You can further protect hello token with encryption, if token access tooyour data is considered highly sensitive and someone may gain access toohello device.</span></span> <span data-ttu-id="0e6ec-114">Zcela bezpečné řešení je nad rámec tohoto návodu, hello však a závisí na požadavky na zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-114">A completely secure solution is beyond hello scope of this tutorial, however, and depends on your security requirements.</span></span>
   >
   >
4. <span data-ttu-id="0e6ec-115">V souboru ToDoActivity.java hello, přidejte následující definice hello hello `loadUserTokenCache` metoda.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-115">In hello ToDoActivity.java file, add hello following definition for hello `loadUserTokenCache` method.</span></span>

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. <span data-ttu-id="0e6ec-116">V hello *ToDoActivity.java* souboru, nahraďte hello `authenticate` metoda s hello následující metody, která používá mezipamětí tokenů.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-116">In hello *ToDoActivity.java* file, replace hello `authenticate` method with hello following method, which uses a token cache.</span></span> <span data-ttu-id="0e6ec-117">Změnit zprostředkovatele přihlášení hello, pokud chcete, aby toouse účtu než Google.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-117">Change hello login provider if you want toouse an account other than Google.</span></span>

        private void authenticate() {
            // We first try tooload a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed tooload a token cache, login and create a token cache
            else
            {
                // Login using hello Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. <span data-ttu-id="0e6ec-118">Sestavte aplikaci a test ověřování hello pomocí platného účtu.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-118">Build hello app and test authentication using a valid account.</span></span> <span data-ttu-id="0e6ec-119">Spusťte alespoň dvakrát.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-119">Run it at least twice.</span></span> <span data-ttu-id="0e6ec-120">Při prvním spuštění hello by se zobrazit výzva toosign v a vytvořte mezipamětí tokenů hello.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-120">During hello first run, you should receive a prompt toosign in and create hello token cache.</span></span> <span data-ttu-id="0e6ec-121">Potom pokusí každé spuštění tooload hello tokenu mezipaměti pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-121">After that, each run attempts tooload hello token cache for authentication.</span></span> <span data-ttu-id="0e6ec-122">By neměl být požadované toosign v.</span><span class="sxs-lookup"><span data-stu-id="0e6ec-122">You should not be required toosign in.</span></span>
