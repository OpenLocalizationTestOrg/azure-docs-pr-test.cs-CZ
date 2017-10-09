

## <a name="generate-hello-certificate-signing-request-file"></a><span data-ttu-id="26e65-101">Generování souboru hello žádost o podepsání certifikátu</span><span class="sxs-lookup"><span data-stu-id="26e65-101">Generate hello Certificate Signing Request file</span></span>
<span data-ttu-id="26e65-102">Hello služby nabízených oznámení Apple (APNS) používá certifikáty tooauthenticate nabízených oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e65-102">hello Apple Push Notification Service (APNS) uses certificates tooauthenticate your push notifications.</span></span> <span data-ttu-id="26e65-103">Postupujte podle těchto pokynů toocreate hello nabízený certifikát toosend a přijímat oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e65-103">Follow these instructions toocreate hello necessary push certificate toosend and receive notifications.</span></span> <span data-ttu-id="26e65-104">Další informace o těchto pojmech najdete v oficiální hello [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="26e65-104">For more information on these concepts see hello official [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) documentation.</span></span>

<span data-ttu-id="26e65-105">Generování souboru hello žádost (Podepsání certifikátu), který je používán Apple toogenerate podepsaného nabízeného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="26e65-105">Generate hello Certificate Signing Request (CSR) file, which is used by Apple toogenerate a signed push certificate.</span></span>

1. <span data-ttu-id="26e65-106">Na počítači Mac spusťte nástroj Keychain Access hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-106">On your Mac, run hello Keychain Access tool.</span></span> <span data-ttu-id="26e65-107">Můžete ho otevřít z hello **nástroje** složku nebo hello **jiných** složky na launchpadu hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-107">It can be opened from hello **Utilities** folder or hello **Other** folder on hello launch pad.</span></span>
2. <span data-ttu-id="26e65-108">Klikněte na **Keychain Access**, rozbalte **Průvodce certifikací**, klikněte na **Vyžádat certifikát od certifikační autority...**.</span><span class="sxs-lookup"><span data-stu-id="26e65-108">Click **Keychain Access**, expand **Certificate Assistant**, then click **Request a Certificate from a Certificate Authority...**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. <span data-ttu-id="26e65-109">Vyberte vaše **e-mailovou adresu uživatele** a **běžný název** , ujistěte se, že **uložit toodisk** je vybrána a potom klikněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="26e65-109">Select your **User Email Address** and **Common Name** , make sure that **Saved toodisk** is selected, and then click **Continue**.</span></span> <span data-ttu-id="26e65-110">Nechte hello **certifikační Autority e-mailovou adresu** pole prázdné, není nutné.</span><span class="sxs-lookup"><span data-stu-id="26e65-110">Leave hello **CA Email Address** field blank as it is not required.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. <span data-ttu-id="26e65-111">Zadejte název souboru certifikátu žádosti o Podepsání hello v **uložit jako**, vyberte umístění hello v **kde**, pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="26e65-111">Type a name for hello Certificate Signing Request (CSR) file in **Save As**, select hello location in **Where**, then click **Save**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      <span data-ttu-id="26e65-112">To umožňuje ušetřit soubor CSR hello v umístění hello vybrané; Hello výchozím umístěním je plocha hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-112">This saves hello CSR file in hello selected location; hello default location is in hello Desktop.</span></span> <span data-ttu-id="26e65-113">Mějte na paměti, hello umístění tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="26e65-113">Remember hello location chosen for this file.</span></span>

<span data-ttu-id="26e65-114">Bude v dalším kroku svou aplikaci zaregistrovat u Applu, povolte nabízená oznámení a nahrajte Tento exportovaný CSR toocreate nabízeného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="26e65-114">Next, you will register your app with Apple, enable push notifications, and upload this exported CSR toocreate a push certificate.</span></span>

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="26e65-115">Registrace aplikace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="26e65-115">Register your app for push notifications</span></span>
<span data-ttu-id="26e65-116">toobe možné toosend nabízená oznámení tooan aplikace pro iOS, je nutné zaregistrovat aplikaci s Apple a také zaregistrovat pro nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e65-116">toobe able toosend push notifications tooan iOS app, you must register your application with Apple and also register for push notifications.</span></span>  

1. <span data-ttu-id="26e65-117">Pokud jste ještě nezaregistrovali vaší aplikace, přejděte toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> hello Apple Developer Center, přihlaste se pomocí Apple ID, klikněte na **identifikátory**, pak klikněte na tlačítko **ID aplikace** a nakonec klikněte na hello  **+**  přihlásit tooregister novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="26e65-117">If you have not already registered your app, navigate toohello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> at hello Apple Developer Center, log on with your Apple ID, click **Identifiers**, then click **App IDs**, and finally click on hello **+** sign tooregister a new app.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. <span data-ttu-id="26e65-118">Aktualizovat hello následující tři pole týkající se nové aplikace a pak klikněte na **pokračovat**:</span><span class="sxs-lookup"><span data-stu-id="26e65-118">Update hello following three fields for your new app and then click **Continue**:</span></span>
   
   * <span data-ttu-id="26e65-119">**Název**: Zadejte popisný název pro vaši aplikaci v hello **název** pole hello **popis ID aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="26e65-119">**Name**: Type a descriptive name for your app in hello **Name** field in hello **App ID Description** section.</span></span>
   * <span data-ttu-id="26e65-120">**Identifikátor sady**: v části hello **explicitní ID aplikace** zadejte **identifikátor svazku** v podobě hello `<Organization Identifier>.<Product Name>` jak je uvedeno v hello [distribuce aplikací Průvodce](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span><span class="sxs-lookup"><span data-stu-id="26e65-120">**Bundle Identifier**: Under hello **Explicit App ID** section, enter a **Bundle Identifier** in hello form `<Organization Identifier>.<Product Name>` as mentioned in hello [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span></span> <span data-ttu-id="26e65-121">Hello *identifikátor organizace* a *název produktu* musí odpovídat identifikátoru organizace hello a název produktu, které budete používat při vytváření projektu prostředí XCode.</span><span class="sxs-lookup"><span data-stu-id="26e65-121">hello *Organization Identifier* and *Product Name* you use must match hello organization identifier and product name you will use when you create your XCode project.</span></span> <span data-ttu-id="26e65-122">V níže uvedeném snímku hello *NotificationHubs* se používá jako identifikátor organizace a *GetStarted* slouží jako název produktu hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-122">In hello screeshot below *NotificationHubs* is used as a organization idenitifier and *GetStarted* is used as hello product name.</span></span> <span data-ttu-id="26e65-123">Ujistěte se, že to odpovídá hello hodnoty, které budete používat ve vašem projektu XCode vám umožní vám toouse hello správného profilu publikování s XCode.</span><span class="sxs-lookup"><span data-stu-id="26e65-123">Making sure this matches hello values you will use in your XCode project will allow you toouse hello correct publishing profile with XCode.</span></span> 
   * <span data-ttu-id="26e65-124">**Nabízená oznámení**: Kontrola hello **nabízená oznámení** možnost v hello **App Services** části.</span><span class="sxs-lookup"><span data-stu-id="26e65-124">**Push Notifications**: Check hello **Push Notifications** option in hello **App Services** section, .</span></span>
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      <span data-ttu-id="26e65-125">Tím vygenerujete ID aplikace a budete tooconfirm hello informace.</span><span class="sxs-lookup"><span data-stu-id="26e65-125">This generates your App ID and requests you tooconfirm hello information.</span></span> <span data-ttu-id="26e65-126">Klikněte na tlačítko **zaregistrovat** tooconfirm hello nové ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="26e65-126">Click **Register** tooconfirm hello new App ID.</span></span>
     
      <span data-ttu-id="26e65-127">Po kliknutí na tlačítko **zaregistrovat**, zobrazí se hello **dokončení registrace** obrazovky, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="26e65-127">Once you click **Register**, you will see hello **Registration complete** screen, as shown below.</span></span> <span data-ttu-id="26e65-128">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="26e65-128">Click **Done**.</span></span>
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. <span data-ttu-id="26e65-129">V hello Developer Center, v části ID aplikace vyhledejte ID aplikace hello, který jste právě vytvořili a klikněte na jeho řádek.</span><span class="sxs-lookup"><span data-stu-id="26e65-129">In hello Developer Center, under App IDs, locate hello app ID that you just created, and click on its row.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      <span data-ttu-id="26e65-130">Kliknutím na ID aplikace hello se zobrazí podrobnosti o aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-130">Clicking on hello app ID will display hello app details.</span></span> <span data-ttu-id="26e65-131">Klikněte na tlačítko hello **upravit** tlačítko dole v hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-131">Click hello **Edit** button at hello bottom.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. <span data-ttu-id="26e65-132">Posuňte se toohello dolní části úvodní obrazovka a klikněte na tlačítko hello **vytvořit certifikát...**  tlačítko části hello **Development Push certifikát SSL**.</span><span class="sxs-lookup"><span data-stu-id="26e65-132">Scroll toohello bottom of hello screen, and click hello **Create Certificate...** button under hello section **Development Push SSL Certificate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      <span data-ttu-id="26e65-133">Zobrazí Pomocníka "Přidat iOS certifikátu" hello.</span><span class="sxs-lookup"><span data-stu-id="26e65-133">This displays hello "Add iOS Certificate" assistant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="26e65-134">Tento kurz používá vývojový certifikát.</span><span class="sxs-lookup"><span data-stu-id="26e65-134">This tutorial uses a development certificate.</span></span> <span data-ttu-id="26e65-135">Dobrý den, stejný postup se používá při registraci produkčního certifikátu.</span><span class="sxs-lookup"><span data-stu-id="26e65-135">hello same process is used when registering a production certificate.</span></span> <span data-ttu-id="26e65-136">Ujistěte se, že používáte hello stejný typ certifikátu při odesílání oznámení.</span><span class="sxs-lookup"><span data-stu-id="26e65-136">Just make sure that you use hello same certificate type when sending notifications.</span></span>
   > 
   > 
3. <span data-ttu-id="26e65-137">Klikněte na tlačítko **zvolit soubor**, procházet toohello umístění, kam jste uložili soubor CSR hello, který jste vytvořili v první úloze hello a pak klikněte na tlačítko **generování**.</span><span class="sxs-lookup"><span data-stu-id="26e65-137">Click **Choose File**, browse toohello location where you saved hello CSR file that you created in hello first task, then click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. <span data-ttu-id="26e65-138">Po vytvoření certifikátu hello hello portálu klikněte na tlačítko hello **Stáhnout** tlačítko a klikněte na tlačítko **provádí**.</span><span class="sxs-lookup"><span data-stu-id="26e65-138">After hello certificate is created by hello portal, click hello **Download** button, and click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      <span data-ttu-id="26e65-139">To hello certifikátu a jeho uložení tooyour počítače v složky se staženými soubory.</span><span class="sxs-lookup"><span data-stu-id="26e65-139">This downloads hello certificate and saves it tooyour computer in your Downloads folder.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > <span data-ttu-id="26e65-140">Ve výchozím nastavení, hello stažený soubor vývojářského certifikátu název **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="26e65-140">By default, hello downloaded file a development certificate is named **aps_development.cer**.</span></span>
   > 
   > 
5. <span data-ttu-id="26e65-141">Poklikejte na stažený nabízený certifikát hello **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="26e65-141">Double-click hello downloaded push certificate **aps_development.cer**.</span></span>
   
      <span data-ttu-id="26e65-142">Tím se nainstaluje nový certifikát hello v hello klíčenky, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="26e65-142">This installs hello new certificate in hello Keychain, as shown below:</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > <span data-ttu-id="26e65-143">Název Hello ve vašem certifikátu se může lišit, ale bude předponu **Apple Development iOS Push Services:**.</span><span class="sxs-lookup"><span data-stu-id="26e65-143">hello name in your certificate might be different, but it will be prefixed with **Apple Development iOS Push Services:**.</span></span>
   > 
   > 
6. <span data-ttu-id="26e65-144">V nástroji Keychain Access, klikněte pravým tlačítkem na hello nový nabízený certifikát, který jste vytvořili v hello **certifikáty** kategorie.</span><span class="sxs-lookup"><span data-stu-id="26e65-144">In Keychain Access, right-click hello new push certificate that you created in hello **Certificates** category.</span></span> <span data-ttu-id="26e65-145">Klikněte na tlačítko **exportovat**, název souboru hello, vyberte hello **.p12** formátu a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="26e65-145">Click **Export**, name hello file, select hello **.p12** format, and then click **Save**.</span></span>
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    <span data-ttu-id="26e65-146">Ujistěte se, poznamenejte si název souboru hello a umístění hello exportovaného certifikátu .p12.</span><span class="sxs-lookup"><span data-stu-id="26e65-146">Make a note of hello file name and location of hello exported .p12 certificate.</span></span> <span data-ttu-id="26e65-147">Bude tooenable použít ověřování pomocí služby APNS.</span><span class="sxs-lookup"><span data-stu-id="26e65-147">It will be used tooenable authentication with APNS.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="26e65-148">V tomto kurzu vytvoříme soubor QuickStart.p12.</span><span class="sxs-lookup"><span data-stu-id="26e65-148">This tutorial creates a QuickStart.p12 file.</span></span> <span data-ttu-id="26e65-149">Váš název souboru a umístění se můžou lišit.</span><span class="sxs-lookup"><span data-stu-id="26e65-149">Your file name and location might be different.</span></span>
   > 
   > 

## <a name="create-a-provisioning-profile-for-hello-app"></a><span data-ttu-id="26e65-150">Vytvořit profil pro zřizování pro aplikace hello</span><span class="sxs-lookup"><span data-stu-id="26e65-150">Create a provisioning profile for hello app</span></span>
1. <span data-ttu-id="26e65-151">Zpět v hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, vyberte **profily zřizování**, vyberte **všechny**a potom klikněte na hello  **+**  tlačítko toocreate nový profil.</span><span class="sxs-lookup"><span data-stu-id="26e65-151">Back in hello <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, select **Provisioning Profiles**, select **All**, and then click hello **+** button toocreate a new profile.</span></span> <span data-ttu-id="26e65-152">Spustí se hello **přidání Zřizovacího profilu iOS** Průvodce</span><span class="sxs-lookup"><span data-stu-id="26e65-152">This launches hello **Add iOS Provisiong Profile** Wizard</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. <span data-ttu-id="26e65-153">Vyberte **vývoj aplikací pro iOS** pod **vývoj** jako typ zřizovacího profilu hello a klikněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="26e65-153">Select **iOS App Development** under **Development** as hello provisiong profile type, and click **Continue**.</span></span> 
3. <span data-ttu-id="26e65-154">Potom vyberte právě vytvořené hello ID aplikace hello **ID aplikace** rozevíracího seznamu a klikněte na **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="26e65-154">Next, select hello app ID you just created from hello **App ID** drop-down list, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. <span data-ttu-id="26e65-155">V hello **vyberte certifikáty** obrazovky, vyberte svůj obvyklý vývojářský certifikát použít pro podepisování kódu a klikněte na **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="26e65-155">In hello **Select certificates** screen, select your usual development certificate used for code signing, and click **Continue**.</span></span> <span data-ttu-id="26e65-156">Toto není hello nabízený certifikát, který jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="26e65-156">This is not hello push certificate you just created.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. <span data-ttu-id="26e65-157">Potom vyberte hello **zařízení** toouse pro účely testování a klikněte na tlačítko **pokračovat**</span><span class="sxs-lookup"><span data-stu-id="26e65-157">Next, select hello **Devices** toouse for testing, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. <span data-ttu-id="26e65-158">Nakonec název profilu hello v **název profilu**, klikněte na tlačítko **generování**.</span><span class="sxs-lookup"><span data-stu-id="26e65-158">Finally, pick a name for hello profile in **Profile Name**, click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. <span data-ttu-id="26e65-159">Když je vytvořen nový profil pro zřizování hello klikněte na toodownload ho a nainstalujte ji na vývojovém počítači Xcode.</span><span class="sxs-lookup"><span data-stu-id="26e65-159">When hello new provisioning profile is created click toodownload it and install it on your Xcode development machine.</span></span> <span data-ttu-id="26e65-160">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="26e65-160">Then click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
