---
title: "Ověření pomocí Mobile Engagement REST API – ruční instalaci"
description: "Popisuje, jak ručně nastavit ověřování rozhraní REST API Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 9d6132e1a01be489b8e8e28a0219cf8a0b50b318
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a><span data-ttu-id="68ccf-103">Ověření pomocí Mobile Engagement REST API – ruční instalaci</span><span class="sxs-lookup"><span data-stu-id="68ccf-103">Authenticate with Mobile Engagement REST APIs - manual setup</span></span>
<span data-ttu-id="68ccf-104">Toto je dokumentaci příloha [ověřit pomocí rozhraní API REST Mobile Engagement](mobile-engagement-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="68ccf-104">This is an appendix documentation to [Authenticate with Mobile Engagement REST APIs](mobile-engagement-api-authentication.md).</span></span> <span data-ttu-id="68ccf-105">Zajistěte, aby že si ji nejprve se získat kontext přečíst.</span><span class="sxs-lookup"><span data-stu-id="68ccf-105">Make sure you read it first to get the context.</span></span> <span data-ttu-id="68ccf-106">Popisuje jiný způsob, jak provést jednorázové nastavení pro nastavení ověřování pro použití portálu Azure Mobile Engagement REST API.</span><span class="sxs-lookup"><span data-stu-id="68ccf-106">This describes an alternate way to do the One-time setup for setting up your authentication for the Mobile Engagement REST APIs using the Azure Portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="68ccf-107">Následující pokyny jsou založené na tomto [služby Active Directory průvodce](../azure-resource-manager/resource-group-create-service-principal-portal.md) a přizpůsobit pro co je vyžadován pro ověření pro zapojení mobilních rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="68ccf-107">The instructions below are based on this [Active Directory guide](../azure-resource-manager/resource-group-create-service-principal-portal.md) and customized for what is required for authentication for Mobile Engagement APIs.</span></span> <span data-ttu-id="68ccf-108">Proto na ni odkazuje Pokud chcete zjistit, následující postup podrobně.</span><span class="sxs-lookup"><span data-stu-id="68ccf-108">So refer to it if you want to understand the steps below in detail.</span></span> 
> 
> 

1. <span data-ttu-id="68ccf-109">Přihlášení k účtu Azure prostřednictvím [portálu classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="68ccf-109">Login to your Azure Account through the [classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="68ccf-110">Vyberte **služby Active Directory** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="68ccf-110">Select **Active Directory** from the left pane.</span></span>
   
     ![Vyberte možnost Active Directory][1]
3. <span data-ttu-id="68ccf-112">Vyberte **výchozí služby Active Directory** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="68ccf-112">Choose the **Default Active Directory** in your Azure portal.</span></span> 
   
     ![Vybrat adresář.][2]
   
   > [!IMPORTANT]
   > <span data-ttu-id="68ccf-114">Tento postup funguje jenom v případě, že pracujete v výchozí služby Active Directory vašeho účtu a nebude fungovat, pokud je to dělají ve službě Active Directory, kterou jste vytvořili ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="68ccf-114">This approach works only when you are working in the default Active Directory of your account and will not work if you are doing this in an Active Directory that you have created in your account.</span></span> 
   > 
   > 
4. <span data-ttu-id="68ccf-115">Chcete-li zobrazit aplikace ve vašem adresáři, klikněte na **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-115">To view the applications in your directory, click on **Applications**.</span></span>
   
     ![Zobrazit aplikace][3]
5. <span data-ttu-id="68ccf-117">Klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-117">Click on **ADD**.</span></span> 
   
     ![Přidání aplikace][4]
6. <span data-ttu-id="68ccf-119">Klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**</span><span class="sxs-lookup"><span data-stu-id="68ccf-119">Click on **Add an application my organization is developing**</span></span>
   
     ![Nová aplikace][5]
7. <span data-ttu-id="68ccf-121">Zadejte název aplikace a vyberte typ aplikace jako **webové aplikace nebo webové rozhraní API** a klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="68ccf-121">Fill in name of the application and select the type of application as **WEB APPLICATION AND/OR WEB API** and click the next button.</span></span>
   
     ![název aplikace][6]
8. <span data-ttu-id="68ccf-123">Můžete zadat všechny fiktivní adresy URL pro **adresa URL přihlašování** a **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-123">You can provide any dummy URLs for **SIGN-ON URL** and **APP ID URI**.</span></span> <span data-ttu-id="68ccf-124">Nejsou použity pro náš scénář a adresy URL sami nejsou ověřené.</span><span class="sxs-lookup"><span data-stu-id="68ccf-124">They are not used for our scenario and the URLs themselves are not validated.</span></span>  
   
     ![Vlastnosti aplikace][7]
9. <span data-ttu-id="68ccf-126">Na konci tohoto budete mít aplikaci AAD pomocí názvu, který jste zadali dříve jako následující.</span><span class="sxs-lookup"><span data-stu-id="68ccf-126">At the end of this, you will have an AAD app with the name you provided previously like the following.</span></span> <span data-ttu-id="68ccf-127">Toto je vaše **AD\_aplikace\_název** a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="68ccf-127">This is your **AD\_APP\_NAME** and make a note of it.</span></span>  
   
     ![Název aplikace.][8]
10. <span data-ttu-id="68ccf-129">Klikněte na název aplikace a klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-129">Click on the app name and click on **Configure**.</span></span>
    
      ![Konfigurace aplikace][9]
11. <span data-ttu-id="68ccf-131">Poznamenejte si ID klienta, který se použije jako **klienta\_ID** pro vaše rozhraní API volání.</span><span class="sxs-lookup"><span data-stu-id="68ccf-131">Make a note of the CLIENT ID that will be used as **CLIENT\_ID** for your API calls.</span></span> 
    
     ![Konfigurace aplikace][10]
12. <span data-ttu-id="68ccf-133">Přejděte dolů k položce **klíče** a přidání klíče se nejlépe doba trvání 2 roky (vypršení platnosti) a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-133">Scroll down to the **Keys** section and add a key with preferably 2 years (expiry) duration and click **Save**.</span></span> 
    
     ![Konfigurace aplikace][11]
13. <span data-ttu-id="68ccf-135">Okamžitě zkopírujte hodnotu, která je pro klíč zobrazen, jak se zobrazují pouze nyní a nejsou uložena, se někdy znovu nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="68ccf-135">Immediately copy the value which is shown for the key as it is only shown now and is not stored so will not be displayed ever again.</span></span> <span data-ttu-id="68ccf-136">Pokud ho ztratíte budete muset vygenerovat nový klíč.</span><span class="sxs-lookup"><span data-stu-id="68ccf-136">If you lose it then you will have to generate a new key.</span></span> <span data-ttu-id="68ccf-137">To bude **tajný klíč CLIENT_SECRET** pro vaše rozhraní API volání.</span><span class="sxs-lookup"><span data-stu-id="68ccf-137">This will be the **CLIENT_SECRET** for your API calls.</span></span> 
    
     ![Konfigurace aplikace][12]
    
    > [!IMPORTANT]
    > <span data-ttu-id="68ccf-139">Tento klíč vyprší na konci dobu, která jste zadali, ujistěte se, že ho obnovit, když nastane čas jinak ověřování rozhraní API nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="68ccf-139">This key will expire at the end of the duration that you specified so make sure to renew it when the time comes otherwise your API authentication will not work anymore.</span></span> <span data-ttu-id="68ccf-140">Můžete také odstranit a znovu vytvořit tento klíč, pokud se domníváte, že byl napaden.</span><span class="sxs-lookup"><span data-stu-id="68ccf-140">You can also delete and recreate this key if you think that it has been compromised.</span></span>
    > 
    > 
14. <span data-ttu-id="68ccf-141">Klikněte na **zobrazit koncové body** tlačítko nyní které se otevře **koncových bodů aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="68ccf-141">Click on **VIEW ENDPOINTS** button now which will open up the **App Endpoints** dialog box.</span></span> 
    
    ![][13]
15. <span data-ttu-id="68ccf-142">Z dialogového okna aplikace koncové body zkopírovat **koncový bod TOKENU OAUTH 2.0**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-142">From the App Endpoints dialog box, copy the **OAUTH 2.0 TOKEN ENDPOINT**.</span></span> 
    
    ![][14]
16. <span data-ttu-id="68ccf-143">Tento koncový bod bude ve formuláři následující, kde je identifikátor GUID v adrese URL vaší **TENANT_ID** , poznamenejte si ji:</span><span class="sxs-lookup"><span data-stu-id="68ccf-143">This endpoint will be in the following form where the GUID in the URL is your **TENANT_ID** so make a note of it:</span></span> 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. <span data-ttu-id="68ccf-144">Nyní budeme pokračovat nakonfigurovat oprávnění v této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="68ccf-144">Now we will proceed to configure the permissions on this app.</span></span> <span data-ttu-id="68ccf-145">Pro to budete muset otevře [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="68ccf-145">For this you will have to open up the [Azure portal](https://portal.azure.com).</span></span> 
18. <span data-ttu-id="68ccf-146">Klikněte na **skupiny prostředků** a najděte **Mobile Engagement** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="68ccf-146">Click on **Resource Groups** and find the **Mobile Engagement** resource group.</span></span>  
    
    ![][15]
19. <span data-ttu-id="68ccf-147">Klikněte na tlačítko **Mobile Engagement** prostředků skupiny a přejděte do **nastavení** okno sem.</span><span class="sxs-lookup"><span data-stu-id="68ccf-147">Click the **Mobile Engagement** resource group and navigate to the **Settings** blade here.</span></span> 
    
    ![][16]
20. <span data-ttu-id="68ccf-148">Klikněte na **uživatelé** v okně Nastavení a pak klikněte na **přidat** přidat uživatele.</span><span class="sxs-lookup"><span data-stu-id="68ccf-148">Click on **Users** in the Settings blade and then click on **Add** to add a user.</span></span> 
    
    ![][17]
21. <span data-ttu-id="68ccf-149">Klikněte na **vybrat roli**</span><span class="sxs-lookup"><span data-stu-id="68ccf-149">Click on **Select a role**</span></span>
    
    ![][18]
22. <span data-ttu-id="68ccf-150">Klikněte na **vlastníka**</span><span class="sxs-lookup"><span data-stu-id="68ccf-150">Click on **Owner**</span></span>
    
    ![][19]
23. <span data-ttu-id="68ccf-151">Vyhledávání pro název vaší aplikace **AD\_aplikace\_název** do vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="68ccf-151">Search for the name of your application **AD\_APP\_NAME** in the Search box.</span></span> <span data-ttu-id="68ccf-152">Se nezobrazí ve výchozím nastavení v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="68ccf-152">You will not see this by default here.</span></span> <span data-ttu-id="68ccf-153">Po nalezení, vyberte ho a klikněte na **vyberte** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="68ccf-153">Once you find it, select it and click on **Select** at the bottom of the blade.</span></span> 
    
    ![][20]
24. <span data-ttu-id="68ccf-154">Na **přidat přístup** okně se zobrazí jako **1 uživatel, 0 skupiny**.</span><span class="sxs-lookup"><span data-stu-id="68ccf-154">On the **Add Access** blade, it will show up as **1 user, 0 groups**.</span></span> <span data-ttu-id="68ccf-155">Klikněte na tlačítko **OK** v tomto okně pro potvrzení změny.</span><span class="sxs-lookup"><span data-stu-id="68ccf-155">Click **OK** on this blade to confirm the change.</span></span> 
    
    ![][21]

<span data-ttu-id="68ccf-156">Teď jste dokončili požadované konfigurace AAD a jsou všechny nastavíte na volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="68ccf-156">You have now completed the required AAD configuration and you are all set to call the APIs.</span></span> 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



