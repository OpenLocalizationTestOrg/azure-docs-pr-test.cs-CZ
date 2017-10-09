
1. V souboru projektu hello MainPage.xaml.cs přidejte následující hello **pomocí** příkazy:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Nahraďte hello **AuthenticateAsync** metoda s hello následující kód:
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses hello Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use hello PasswordVault toosecurely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try tooget an existing credential from hello vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from hello stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set hello user from hello stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check toodetermine if hello token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with hello identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);
   
                    // Create and store hello user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    V této verzi **AuthenticateAsync**, aplikace hello pokusí toouse přihlašovací údaje uložené v hello **PasswordVault** tooaccess hello služby. Regulární přihlášení je také tehdy, pokud žádné uložených přihlašovacích údajů.
   
   > [!NOTE]
   > Token v mezipaměti jeho platnost vypršela a vypršení platnosti tokenu může také dojít po ověření, když aplikace hello je používán. jak toodetermine Pokud vypršela platnost tokenu, přečtěte si téma toolearn [zkontrolujte vypršela platnost ověřování tokenů](http://aka.ms/jww5vp). Chyb autorizace toohandling řešení souvisejících tooexpiring tokeny, najdete v části hello post [SDK ke správě ukládání do mezipaměti a zpracování vypršení platnosti tokenů v Azure Mobile Services](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. Restartujte aplikace hello dvakrát.
   
    Všimněte si, že na první spuštění hello, přihlaste se pomocí zprostředkovatele hello vyžádáním znovu. Ale na druhém restartu hello se používají přihlašovací údaje uložené v mezipaměti hello a přihlášení bude přeskočeno. 

