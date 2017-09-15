

## <a name="generate-the-certificate-signing-request-file"></a><span data-ttu-id="dcc27-101">Generování souboru s žádostí o podepsání certifikátu</span><span class="sxs-lookup"><span data-stu-id="dcc27-101">Generate the Certificate Signing Request file</span></span>
<span data-ttu-id="dcc27-102">Služba Apple Push Notification Service (APNS) používá k ověřování nabízených oznámení certifikáty.</span><span class="sxs-lookup"><span data-stu-id="dcc27-102">The Apple Push Notification Service (APNS) uses certificates to authenticate your push notifications.</span></span> <span data-ttu-id="dcc27-103">Pokud chcete vytvořit nabízený certifikát pro odesílání a přijímání oznámení, postupujte podle těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="dcc27-103">Follow these instructions to create the necessary push certificate to send and receive notifications.</span></span> <span data-ttu-id="dcc27-104">Další informace o těchto konceptech najdete v oficiální dokumentaci ke službě [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584).</span><span class="sxs-lookup"><span data-stu-id="dcc27-104">For more information on these concepts see the official [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) documentation.</span></span>

<span data-ttu-id="dcc27-105">Vygenerujte soubor s žádostí o podepsání certifikátu (CSR), který Apple používá k vygenerování podepsaného nabízeného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="dcc27-105">Generate the Certificate Signing Request (CSR) file, which is used by Apple to generate a signed push certificate.</span></span>

1. <span data-ttu-id="dcc27-106">V Macu spusťte nástroj Keychain Access.</span><span class="sxs-lookup"><span data-stu-id="dcc27-106">On your Mac, run the Keychain Access tool.</span></span> <span data-ttu-id="dcc27-107">Můžete ho otevřít na Launchpadu ve složce **Nástroje** nebo **Jiné**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-107">It can be opened from the **Utilities** folder or the **Other** folder on the launch pad.</span></span>
2. <span data-ttu-id="dcc27-108">Klikněte na **Keychain Access**, rozbalte **Průvodce certifikací**, klikněte na **Vyžádat certifikát od certifikační autority...**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-108">Click **Keychain Access**, expand **Certificate Assistant**, then click **Request a Certificate from a Certificate Authority...**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. <span data-ttu-id="dcc27-109">Vyberte svoji **uživatelskou e-mailovou adresu** a **běžný název**, zkontrolujte, jestli je vybraná možnost **Uloženo na disk** a potom klikněte na **Pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-109">Select your **User Email Address** and **Common Name** , make sure that **Saved to disk** is selected, and then click **Continue**.</span></span> <span data-ttu-id="dcc27-110">Pole **E-mailová adresa CA** není povinné, takže ho můžete nechat prázdné.</span><span class="sxs-lookup"><span data-stu-id="dcc27-110">Leave the **CA Email Address** field blank as it is not required.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. <span data-ttu-id="dcc27-111">Do pole **Uložit jako** zadejte název souboru CSR, pomocí možnosti **Kam** vyberte umístění a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-111">Type a name for the Certificate Signing Request (CSR) file in **Save As**, select the location in **Where**, then click **Save**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      <span data-ttu-id="dcc27-112">Tím soubor CSR uložíte do vybraného umístění. Výchozím umístěním je plocha.</span><span class="sxs-lookup"><span data-stu-id="dcc27-112">This saves the CSR file in the selected location; the default location is in the Desktop.</span></span> <span data-ttu-id="dcc27-113">Zapamatujte si umístění tohoto souboru.</span><span class="sxs-lookup"><span data-stu-id="dcc27-113">Remember the location chosen for this file.</span></span>

<span data-ttu-id="dcc27-114">V dalším kroku svou aplikaci zaregistrujete u Applu, povolíte nabízená oznámení a odešlete exportovaný soubor CSR k vytvoření nabízeného certifikátu.</span><span class="sxs-lookup"><span data-stu-id="dcc27-114">Next, you will register your app with Apple, enable push notifications, and upload this exported CSR to create a push certificate.</span></span>

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="dcc27-115">Registrace aplikace pro nabízená oznámení</span><span class="sxs-lookup"><span data-stu-id="dcc27-115">Register your app for push notifications</span></span>
<span data-ttu-id="dcc27-116">Abyste mohli odesílat nabízená oznámení do aplikace systému iOS, musíte aplikaci zaregistrovat u Applu a také ji musíte zaregistrovat pro nabízená oznámení.</span><span class="sxs-lookup"><span data-stu-id="dcc27-116">To be able to send push notifications to an iOS app, you must register your application with Apple and also register for push notifications.</span></span>  

1. <span data-ttu-id="dcc27-117">Pokud jste aplikaci ještě nezaregistrovali, přejděte na stránky <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> na webu Apple Developer Center, přihlaste se pomocí Apple ID, klikněte na **Identifiers** (Identifikátory), potom na **App IDs** (ID aplikací) a nakonec klikněte na znak **+** a zaregistrujte novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dcc27-117">If you have not already registered your app, navigate to the <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> at the Apple Developer Center, log on with your Apple ID, click **Identifiers**, then click **App IDs**, and finally click on the **+** sign to register a new app.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)
      
2. <span data-ttu-id="dcc27-118">Aktualizujte následující tři pole týkající se nové aplikace a potom klikněte na **Continue** (Pokračovat):</span><span class="sxs-lookup"><span data-stu-id="dcc27-118">Update the following three fields for your new app and then click **Continue**:</span></span>
   
   * <span data-ttu-id="dcc27-119">**Name** (Název): V části **App ID Description** (Popis ID aplikace) zadejte do pole **Name** (Název) popisný název aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc27-119">**Name**: Type a descriptive name for your app in the **Name** field in the **App ID Description** section.</span></span>
   * <span data-ttu-id="dcc27-120">**Bundle Identifier** (Identifikátor svazku): V části **Explicit App ID** (Explicitní ID aplikace) zadejte **identifikátor svazku** ve formě `<Organization Identifier>.<Product Name>`, jak je uvedeno v [Průvodci distribucí aplikace](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span><span class="sxs-lookup"><span data-stu-id="dcc27-120">**Bundle Identifier**: Under the **Explicit App ID** section, enter a **Bundle Identifier** in the form `<Organization Identifier>.<Product Name>` as mentioned in the [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8).</span></span> <span data-ttu-id="dcc27-121">Použité hodnoty *Organization Identifier* (Identifikátor organizace) a *Product Name* (Název produktu) musí odpovídat identifikátoru organizace a názvu produktu, které budete používat při vytváření projektu prostředí XCode.</span><span class="sxs-lookup"><span data-stu-id="dcc27-121">The *Organization Identifier* and *Product Name* you use must match the organization identifier and product name you will use when you create your XCode project.</span></span> <span data-ttu-id="dcc27-122">Na níže uvedeném snímku se *NotificationHubs* používá jako identifikátor organizace a *GetStarted* slouží jako název produktu.</span><span class="sxs-lookup"><span data-stu-id="dcc27-122">In the screeshot below *NotificationHubs* is used as a organization idenitifier and *GetStarted* is used as the product name.</span></span> <span data-ttu-id="dcc27-123">Ujistěte se, že tyto hodnoty odpovídají hodnotám, které budete používat v projektu XCode. To vám umožní používání správného profilu publikování s XCode.</span><span class="sxs-lookup"><span data-stu-id="dcc27-123">Making sure this matches the values you will use in your XCode project will allow you to use the correct publishing profile with XCode.</span></span> 
   * <span data-ttu-id="dcc27-124">**Push Notifications** (Nabízená oznámení): V části **App Services** (Služby aplikací) zaškrtněte možnost **Push Notifications** (Nabízená oznámení).</span><span class="sxs-lookup"><span data-stu-id="dcc27-124">**Push Notifications**: Check the **Push Notifications** option in the **App Services** section, .</span></span>
     
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
     
      <span data-ttu-id="dcc27-125">Tím vygenerujete ID aplikace a budete vyzváni k potvrzení tohoto údaje.</span><span class="sxs-lookup"><span data-stu-id="dcc27-125">This generates your App ID and requests you to confirm the information.</span></span> <span data-ttu-id="dcc27-126">Kliknutím na **Register** (Zaregistrovat) potvrďte nové ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcc27-126">Click **Register** to confirm the new App ID.</span></span>
     
      <span data-ttu-id="dcc27-127">Po kliknutí na **Register** (Zaregistrovat) se zobrazí obrazovka **Registration complete** (Registrace je dokončena), která je na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="dcc27-127">Once you click **Register**, you will see the **Registration complete** screen, as shown below.</span></span> <span data-ttu-id="dcc27-128">Klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="dcc27-128">Click **Done**.</span></span>
      
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)


1. <span data-ttu-id="dcc27-129">V centru pro vývojáře v části App IDs (ID aplikací) najděte právě vytvořené ID aplikace a klikněte na jeho řádek.</span><span class="sxs-lookup"><span data-stu-id="dcc27-129">In the Developer Center, under App IDs, locate the app ID that you just created, and click on its row.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)
   
      <span data-ttu-id="dcc27-130">Kliknutím na ID aplikace zobrazíte podrobnosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dcc27-130">Clicking on the app ID will display the app details.</span></span> <span data-ttu-id="dcc27-131">V dolní části klikněte na tlačítko **Edit** (Upravit).</span><span class="sxs-lookup"><span data-stu-id="dcc27-131">Click the **Edit** button at the bottom.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)
      
2. <span data-ttu-id="dcc27-132">Přejděte do dolní části obrazovky a klikněte v části **Development Push SSL Certificate** (Vývojový certifikát pro nabízená oznámení SSL) na tlačítko **Create Certificate...** (Vytvořit certifikát...).</span><span class="sxs-lookup"><span data-stu-id="dcc27-132">Scroll to the bottom of the screen, and click the **Create Certificate...** button under the section **Development Push SSL Certificate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
      <span data-ttu-id="dcc27-133">Zobrazí se průvodce Add iOS Certificate (Přidání certifikátu iOS).</span><span class="sxs-lookup"><span data-stu-id="dcc27-133">This displays the "Add iOS Certificate" assistant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dcc27-134">Tento kurz používá vývojový certifikát.</span><span class="sxs-lookup"><span data-stu-id="dcc27-134">This tutorial uses a development certificate.</span></span> <span data-ttu-id="dcc27-135">Stejný postup se používá při registraci produkčního certifikátu.</span><span class="sxs-lookup"><span data-stu-id="dcc27-135">The same process is used when registering a production certificate.</span></span> <span data-ttu-id="dcc27-136">Dejte pozor, abyste při odesílání oznámení používali stejný typ certifikátu.</span><span class="sxs-lookup"><span data-stu-id="dcc27-136">Just make sure that you use the same certificate type when sending notifications.</span></span>
   > 
   > 
3. <span data-ttu-id="dcc27-137">Klikněte na **Choose File** (Zvolit soubor), přejděte do umístění, kam jste uložili soubor CSR vytvořený v první úloze, a potom klikněte na **Generate** (Generovat).</span><span class="sxs-lookup"><span data-stu-id="dcc27-137">Click **Choose File**, browse to the location where you saved the CSR file that you created in the first task, then click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
4. <span data-ttu-id="dcc27-138">Když portál vytvoří certifikát, klikněte na tlačítko **Download** (Stáhnout) a potom na **Done** Hotovo).</span><span class="sxs-lookup"><span data-stu-id="dcc27-138">After the certificate is created by the portal, click the **Download** button, and click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
      <span data-ttu-id="dcc27-139">Tím certifikát stáhnete a uložíte do počítače do složky se staženými soubory.</span><span class="sxs-lookup"><span data-stu-id="dcc27-139">This downloads the certificate and saves it to your computer in your Downloads folder.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > <span data-ttu-id="dcc27-140">Ve výchozím nastavení má stažený soubor vývojářského certifikátu název **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-140">By default, the downloaded file a development certificate is named **aps_development.cer**.</span></span>
   > 
   > 
5. <span data-ttu-id="dcc27-141">Poklikejte na stažený nabízený certifikát **aps_development.cer**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-141">Double-click the downloaded push certificate **aps_development.cer**.</span></span>
   
      <span data-ttu-id="dcc27-142">Tím nový certifikát nainstalujete do Klíčenky, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="dcc27-142">This installs the new certificate in the Keychain, as shown below:</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > <span data-ttu-id="dcc27-143">Název ve vašem certifikátu se může lišit, ale bude mu předcházet text **Apple Development iOS Push Services:**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-143">The name in your certificate might be different, but it will be prefixed with **Apple Development iOS Push Services:**.</span></span>
   > 
   > 
6. <span data-ttu-id="dcc27-144">V nástroji Keychain Access, klikněte pravým tlačítkem na nový nabízený certifikát, který jste vytvořili v kategorii **Certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-144">In Keychain Access, right-click the new push certificate that you created in the **Certificates** category.</span></span> <span data-ttu-id="dcc27-145">Klikněte na **Exportovat**, zadejte název souboru, vyberte formát **.p12** a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-145">Click **Export**, name the file, select the **.p12** format, and then click **Save**.</span></span>
   
    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)
   
    <span data-ttu-id="dcc27-146">Poznamenejte si název souboru a umístění exportovaného certifikátu .p12.</span><span class="sxs-lookup"><span data-stu-id="dcc27-146">Make a note of the file name and location of the exported .p12 certificate.</span></span> <span data-ttu-id="dcc27-147">Budete ho potřebovat k povolení ověřování pomocí služby APNS.</span><span class="sxs-lookup"><span data-stu-id="dcc27-147">It will be used to enable authentication with APNS.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dcc27-148">V tomto kurzu vytvoříme soubor QuickStart.p12.</span><span class="sxs-lookup"><span data-stu-id="dcc27-148">This tutorial creates a QuickStart.p12 file.</span></span> <span data-ttu-id="dcc27-149">Váš název souboru a umístění se můžou lišit.</span><span class="sxs-lookup"><span data-stu-id="dcc27-149">Your file name and location might be different.</span></span>
   > 
   > 

## <a name="create-a-provisioning-profile-for-the-app"></a><span data-ttu-id="dcc27-150">Vytvoření zřizovacího profilu pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="dcc27-150">Create a provisioning profile for the app</span></span>
1. <span data-ttu-id="dcc27-151">Na stránkách <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> vyberte **Provisioning Profiles** (Zřizovací profily), potom **All** (Všechny) a nakonec kliknutím na tlačítko **+** vytvořte nový profil.</span><span class="sxs-lookup"><span data-stu-id="dcc27-151">Back in the <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a>, select **Provisioning Profiles**, select **All**, and then click the **+** button to create a new profile.</span></span> <span data-ttu-id="dcc27-152">Tím spustíte průvodce **Add iOS Provisiong Profile** (Přidání zřizovacího profilu iOS).</span><span class="sxs-lookup"><span data-stu-id="dcc27-152">This launches the **Add iOS Provisiong Profile** Wizard</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. <span data-ttu-id="dcc27-153">V části **Development** (Vývoj) vyberte jako typ zřizovacího profilu **iOS App Development** (Vývoj aplikací pro iOS) a klikněte na **Continue** (Pokračovat).</span><span class="sxs-lookup"><span data-stu-id="dcc27-153">Select **iOS App Development** under **Development** as the provisiong profile type, and click **Continue**.</span></span> 
3. <span data-ttu-id="dcc27-154">Potom v rozevíracím seznamu **App ID** (ID aplikace) vyberte právě vytvořené ID aplikace a klikněte na **Continue** (Pokračovat).</span><span class="sxs-lookup"><span data-stu-id="dcc27-154">Next, select the app ID you just created from the **App ID** drop-down list, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. <span data-ttu-id="dcc27-155">Na obrazovce **Select certificates** (Výběr certifikátů) vyberte svůj obvyklý vývojářský certifikát, kterým podepisujete kód, a klikněte na **Continue** (Pokračovat).</span><span class="sxs-lookup"><span data-stu-id="dcc27-155">In the **Select certificates** screen, select your usual development certificate used for code signing, and click **Continue**.</span></span> <span data-ttu-id="dcc27-156">Toto není právě vytvořený nabízený certifikát.</span><span class="sxs-lookup"><span data-stu-id="dcc27-156">This is not the push certificate you just created.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. <span data-ttu-id="dcc27-157">Potom vyberte **zařízení**, která chcete použít pro testování, a klikněte na **Continue** (Pokračovat).</span><span class="sxs-lookup"><span data-stu-id="dcc27-157">Next, select the **Devices** to use for testing, and click **Continue**</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. <span data-ttu-id="dcc27-158">Nakonec do pole **Název profilu** zadejte název profilu a klikněte na **Generovat**.</span><span class="sxs-lookup"><span data-stu-id="dcc27-158">Finally, pick a name for the profile in **Profile Name**, click **Generate**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
7. <span data-ttu-id="dcc27-159">Po vytvoření nového zřizovacího profilu si ho kliknutím stáhněte a nainstalujte na svém vývojovém počítači s XCode.</span><span class="sxs-lookup"><span data-stu-id="dcc27-159">When the new provisioning profile is created click to download it and install it on your Xcode development machine.</span></span> <span data-ttu-id="dcc27-160">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="dcc27-160">Then click **Done**.</span></span>
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)
