
1. <span data-ttu-id="5b257-101">V souboru projektu MainPage.xaml.cs, přidejte následující **pomocí** příkazy:</span><span class="sxs-lookup"><span data-stu-id="5b257-101">In the MainPage.xaml.cs project file, add the following **using** statements:</span></span>
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. <span data-ttu-id="5b257-102">Nahraďte **AuthenticateAsync** metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="5b257-102">Replace the **AuthenticateAsync** method with the following code:</span></span>
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);
   
                    // Create and store the user credentials.
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
   
    <span data-ttu-id="5b257-103">V této verzi **AuthenticateAsync**, aplikace se pokusí použít přihlašovací údaje uložené v **PasswordVault** přístup ke službě.</span><span class="sxs-lookup"><span data-stu-id="5b257-103">In this version of **AuthenticateAsync**, the app tries to use credentials stored in the **PasswordVault** to access the service.</span></span> <span data-ttu-id="5b257-104">Regulární přihlášení je také tehdy, pokud žádné uložených přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="5b257-104">A regular sign-in is also performed when there is no stored credential.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="5b257-105">Token v mezipaměti jeho platnost vypršela a vypršení platnosti tokenu může také dojít po ověření, když aplikace je používána.</span><span class="sxs-lookup"><span data-stu-id="5b257-105">A cached token may be expired, and token expiration can also occur after authentication when the app is in use.</span></span> <span data-ttu-id="5b257-106">Zjistěte, jak určit, pokud vypršela platnost tokenu, najdete v tématu [zkontrolujte vypršela platnost ověřování tokenů](http://aka.ms/jww5vp).</span><span class="sxs-lookup"><span data-stu-id="5b257-106">To learn how to determine if a token is expired, see [Check for expired authentication tokens](http://aka.ms/jww5vp).</span></span> <span data-ttu-id="5b257-107">Řešení pro zpracování chyb autorizace související s tokeny kterým vyprší platnost, najdete v příspěvku [SDK ke správě ukládání do mezipaměti a zpracování vypršení platnosti tokenů v Azure Mobile Services](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b257-107">For a solution to handling authorization errors related to expiring tokens, see the post [Caching and handling expired tokens in Azure Mobile Services managed SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span></span> 
   > 
   > 
3. <span data-ttu-id="5b257-108">Restartujte aplikaci dvakrát.</span><span class="sxs-lookup"><span data-stu-id="5b257-108">Restart the app twice.</span></span>
   
    <span data-ttu-id="5b257-109">Všimněte si, že na první spuštění, přihlaste se pomocí zprostředkovatele vyžádáním znovu.</span><span class="sxs-lookup"><span data-stu-id="5b257-109">Notice that on the first start-up, sign-in with the provider is again required.</span></span> <span data-ttu-id="5b257-110">Ale na druhém restartu se používají přihlašovací údaje v mezipaměti a přihlašování bude přeskočeno.</span><span class="sxs-lookup"><span data-stu-id="5b257-110">However, on the second restart the cached credentials are used and sign-in is bypassed.</span></span> 

