
1. <span data-ttu-id="b5ac8-101">V souboru projektu hello MainPage.xaml.cs přidejte následující hello **pomocí** příkazy:</span><span class="sxs-lookup"><span data-stu-id="b5ac8-101">In hello MainPage.xaml.cs project file, add hello following **using** statements:</span></span>
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. <span data-ttu-id="b5ac8-102">Nahraďte hello **AuthenticateAsync** metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="b5ac8-102">Replace hello **AuthenticateAsync** method with hello following code:</span></span>
   
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
   
    <span data-ttu-id="b5ac8-103">V této verzi **AuthenticateAsync**, aplikace hello pokusí toouse přihlašovací údaje uložené v hello **PasswordVault** tooaccess hello služby.</span><span class="sxs-lookup"><span data-stu-id="b5ac8-103">In this version of **AuthenticateAsync**, hello app tries toouse credentials stored in hello **PasswordVault** tooaccess hello service.</span></span> <span data-ttu-id="b5ac8-104">Regulární přihlášení je také tehdy, pokud žádné uložených přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b5ac8-104">A regular sign-in is also performed when there is no stored credential.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b5ac8-105">Token v mezipaměti jeho platnost vypršela a vypršení platnosti tokenu může také dojít po ověření, když aplikace hello je používán.</span><span class="sxs-lookup"><span data-stu-id="b5ac8-105">A cached token may be expired, and token expiration can also occur after authentication when hello app is in use.</span></span> <span data-ttu-id="b5ac8-106">jak toodetermine Pokud vypršela platnost tokenu, přečtěte si téma toolearn [zkontrolujte vypršela platnost ověřování tokenů](http://aka.ms/jww5vp).</span><span class="sxs-lookup"><span data-stu-id="b5ac8-106">toolearn how toodetermine if a token is expired, see [Check for expired authentication tokens](http://aka.ms/jww5vp).</span></span> <span data-ttu-id="b5ac8-107">Chyb autorizace toohandling řešení souvisejících tooexpiring tokeny, najdete v části hello post [SDK ke správě ukládání do mezipaměti a zpracování vypršení platnosti tokenů v Azure Mobile Services](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="b5ac8-107">For a solution toohandling authorization errors related tooexpiring tokens, see hello post [Caching and handling expired tokens in Azure Mobile Services managed SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span></span> 
   > 
   > 
3. <span data-ttu-id="b5ac8-108">Restartujte aplikace hello dvakrát.</span><span class="sxs-lookup"><span data-stu-id="b5ac8-108">Restart hello app twice.</span></span>
   
    <span data-ttu-id="b5ac8-109">Všimněte si, že na první spuštění hello, přihlaste se pomocí zprostředkovatele hello vyžádáním znovu.</span><span class="sxs-lookup"><span data-stu-id="b5ac8-109">Notice that on hello first start-up, sign-in with hello provider is again required.</span></span> <span data-ttu-id="b5ac8-110">Ale na druhém restartu hello se používají přihlašovací údaje uložené v mezipaměti hello a přihlášení bude přeskočeno.</span><span class="sxs-lookup"><span data-stu-id="b5ac8-110">However, on hello second restart hello cached credentials are used and sign-in is bypassed.</span></span> 

