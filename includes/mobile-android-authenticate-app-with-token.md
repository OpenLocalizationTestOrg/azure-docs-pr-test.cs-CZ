
Hello předchozí příklad ukázal u standardní přihlášení, které vyžaduje klient toocontact hello obou hello identity zprostředkovatele a hello back-end služby Azure při každém spuštění aplikace hello. Tato metoda je neefektivní a může mít problémy související s využití, pokud mnoho zákazníků zkuste toostart aplikace současně. Lepším řešením je, že toocache hello autorizační token vrácený hello služby Azure a zkuste to toouse to před použitím nejprve u založenou na poskytovateli přihlášení.

> [!NOTE]
> Můžete mezipaměti hello token vystavený hello back-end služby Azure bez ohledu na to, jestli používáte ověřování klienta spravovat nebo spravované služby. Tento kurz používá službu spravovat ověřování.
>
>

1. Otevřete soubor ToDoActivity.java hello a přidejte následující příkazy pro import hello:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Přidejte následující členy toohello hello `ToDoActivity` třídy.

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. V souboru ToDoActivity.java hello, přidejte následující definice hello hello `cacheUserToken` metoda.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Tato metoda ukládá v souboru předvoleb, která je označena privátní hello ID uživatele a token. To by měla chránit mezipaměti toohello přístup tak, aby jiné aplikace na zařízení hello nemají toohello tokenu přístupu. Hello předvoleb je v izolovaném prostoru pro aplikace hello. Ale pokud někdo získá přístup toohello zařízení, je možné, že získají přístup toohello tokenu mezipaměti jinými způsoby.

   > [!NOTE]
   > Dále je možné chránit hello token s šifrování, pokud data tooyour tokenu přístupu jsou považovány za vysoce citlivé a někdo může získat přístup k toohello zařízení. Zcela bezpečné řešení je nad rámec tohoto návodu, hello však a závisí na požadavky na zabezpečení.
   >
   >
4. V souboru ToDoActivity.java hello, přidejte následující definice hello hello `loadUserTokenCache` metoda.

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
5. V hello *ToDoActivity.java* souboru, nahraďte hello `authenticate` metoda s hello následující metody, která používá mezipamětí tokenů. Změnit zprostředkovatele přihlášení hello, pokud chcete, aby toouse účtu než Google.

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
6. Sestavte aplikaci a test ověřování hello pomocí platného účtu. Spusťte alespoň dvakrát. Při prvním spuštění hello by se zobrazit výzva toosign v a vytvořte mezipamětí tokenů hello. Potom pokusí každé spuštění tooload hello tokenu mezipaměti pro ověřování. By neměl být požadované toosign v.
