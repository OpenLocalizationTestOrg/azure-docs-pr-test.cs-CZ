
1. <span data-ttu-id="dcb0c-101">Přejděte toohello [Google Cloud Console](https://console.developers.google.com/project), přihlaste se pomocí svých přihlašovacích údajů účtu Google.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-101">Navigate toohello [Google Cloud Console](https://console.developers.google.com/project), sign in with your Google account credentials.</span></span> 
2. <span data-ttu-id="dcb0c-102">Klikněte na **Create Project** (Vytvořit projekt), zadejte název projektu a potom klikněte na **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="dcb0c-102">Click **Create Project**, type a project name, then click **Create**.</span></span> <span data-ttu-id="dcb0c-103">Pokud požadovaný, provádět hello ověření SMS a klikněte na **vytvořit** znovu.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-103">If requested, carry out hello SMS Verification, and click **Create** again.</span></span>
   
    ![Vytvoření nového projektu](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-new-project.png)   
   
     <span data-ttu-id="dcb0c-105">Zadejte novou položku **Project name** (Název projektu) a klikněte na **Create project** (Vytvořit projekt).</span><span class="sxs-lookup"><span data-stu-id="dcb0c-105">Type in your new **Project name** and click **Create project**.</span></span>
3. <span data-ttu-id="dcb0c-106">Klikněte na tlačítko hello **nástroje a další** tlačítko a pak klikněte na **informace o projektu**.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-106">Click hello **Utilities and More** button and then click **Project Information**.</span></span> <span data-ttu-id="dcb0c-107">Poznamenejte si hello **číslo projektu**.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-107">Make a note of hello **Project Number**.</span></span> <span data-ttu-id="dcb0c-108">Budete potřebovat tooset tuto hodnotu jako hello `SenderId` proměnné v hello klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-108">You will need tooset this value as hello `SenderId` variable in hello client app.</span></span>
   
    ![Nástroje a další](./media/mobile-services-enable-google-cloud-messaging/notification-hubs-utilities-and-more.png)
4. <span data-ttu-id="dcb0c-110">V hello projektu řídicího panelu, v části **mobilních rozhraní API**, klikněte na tlačítko **Google Cloud Messaging**, klepnutím na další stránku hello **povolit rozhraní API** a přijměte podmínky hello služby.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-110">In hello project dashboard, under **Mobile APIs**, click **Google Cloud Messaging**, then on hello next page click **Enable API** and accept hello terms of service.</span></span> 
   
    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-GCM.png)
   
    ![Povolení GCM](./media/mobile-services-enable-google-cloud-messaging/enable-gcm-2.png) 
5. <span data-ttu-id="dcb0c-113">Na řídicím panelu projektu hello, klikněte na tlačítko **pověření** > **vytvoření pověření** > **klíč rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-113">In hello project dashboard, Click **Credentials** > **Create Credential** > **API Key**.</span></span> 
   
    ![](./media/mobile-services-enable-google-cloud-messaging/mobile-services-google-create-server-key.png)
6. <span data-ttu-id="dcb0c-114">V části **Create a new key** (Vytvořit nový klíč) klikněte na **Server key** (Klíč serveru), zadejte název klíče a potom klikněte na **Create** (Vytvořit).</span><span class="sxs-lookup"><span data-stu-id="dcb0c-114">In **Create a new key**, click **Server key**, type a name for your key, then click **Create**.</span></span>
7. <span data-ttu-id="dcb0c-115">Poznamenejte si hello **klíč rozhraní API** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-115">Make a note of hello **API KEY** value.</span></span>
   
    <span data-ttu-id="dcb0c-116">Budete používat tento tooenable hodnotu klíče rozhraní API Azure tooauthenticate s GCM a odesílání nabízených oznámení jménem vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcb0c-116">You will use this API key value tooenable Azure tooauthenticate with GCM and send push notifications on behalf of your app.</span></span>

