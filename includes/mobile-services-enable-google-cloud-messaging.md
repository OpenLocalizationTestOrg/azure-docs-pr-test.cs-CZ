
1. <span data-ttu-id="bcda2-101">Přejděte na [Google Cloud Console](https://console.developers.google.com/project) a přihlaste se pomocí svých přihlašovacích údajů k účtu Google.</span><span class="sxs-lookup"><span data-stu-id="bcda2-101">Navigate to the [Google Cloud Console](https://console.developers.google.com/project), sign in with your Google account credentials.</span></span> 
2. <span data-ttu-id="bcda2-102">Klikněte na **Create Project** (Vytvořit projekt), zadejte název projektu a potom klikněte na **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="bcda2-102">Click **Create Project**, type a project name, then click **Create**.</span></span> <span data-ttu-id="bcda2-103">Pokud se zobrazí výzva, proveďte ověření SMS a znovu klikněte na **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="bcda2-103">If requested, carry out the SMS Verification, and click **Create** again.</span></span>
   
    ![Vytvoření nového projektu](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     <span data-ttu-id="bcda2-105">Zadejte novou položku **Project name** (Název projektu) a klikněte na **Create project** (Vytvořit projekt).</span><span class="sxs-lookup"><span data-stu-id="bcda2-105">Type in your new **Project name** and click **Create project**.</span></span>
3. <span data-ttu-id="bcda2-106">Klikněte na tlačítko **Utilities and More** (Nástroje a další) a potom na **Project Information** (Informace o projektu).</span><span class="sxs-lookup"><span data-stu-id="bcda2-106">Click the **Utilities and More** button and then click **Project Information**.</span></span> <span data-ttu-id="bcda2-107">Poznamenejte si hodnotu **Project Number** (Číslo projektu).</span><span class="sxs-lookup"><span data-stu-id="bcda2-107">Make a note of the **Project Number**.</span></span> <span data-ttu-id="bcda2-108">Tuto hodnotu budete muset nastavit jako proměnnou `SenderId` v klientské aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bcda2-108">You will need to set this value as the `SenderId` variable in the client app.</span></span>
   
    ![Nástroje a další](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. <span data-ttu-id="bcda2-110">Na řídicím panelu projektu v části **Mobile APIs** (Mobilní rozhraní API) klikněte na **Google Cloud Messaging** a potom na další stránce klikněte na **Enable API** (Povolit rozhraní API) a přijměte podmínky služby.</span><span class="sxs-lookup"><span data-stu-id="bcda2-110">In the project dashboard, under **Mobile APIs**, click **Google Cloud Messaging**, then on the next page click **Enable API** and accept the terms of service.</span></span> 
   
    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. <span data-ttu-id="bcda2-113">Na řídicím panelu projektu klikněte na **Credentials**(Přihlašovací údaje)  > **Create Credential**(Vytvořit přihlašovací údaje)  > **API Key** (Klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="bcda2-113">In the project dashboard, Click **Credentials** > **Create Credential** > **API Key**.</span></span> 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. <span data-ttu-id="bcda2-114">V části **Create a new key** (Vytvořit nový klíč) klikněte na **Server key** (Klíč serveru), zadejte název klíče a potom klikněte na **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="bcda2-114">In **Create a new key**, click **Server key**, type a name for your key, then click **Create**.</span></span>
7. <span data-ttu-id="bcda2-115">Poznamenejte si hodnotu položky **API KEY** (Klíč rozhraní API).</span><span class="sxs-lookup"><span data-stu-id="bcda2-115">Make a note of the **API KEY** value.</span></span>
   
    <span data-ttu-id="bcda2-116">Pomocí této hodnoty klíče API potom povolíte službě Azure ověření na serveru GCM a odesílání nabízených oznámení jménem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="bcda2-116">You will use this API key value to enable Azure to authenticate with GCM and send push notifications on behalf of your app.</span></span>

