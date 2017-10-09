---
title: "aaaAuthenticate s Mobile Engagement REST API – ruční instalaci"
description: "Popisuje, jak nastavit toomanually ověřování rozhraní REST API Mobile Engagement"
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
ms.openlocfilehash: 3884f94afcd6b9a62bfcf498fb6ee84bb6e837b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a><span data-ttu-id="c0b53-103">Ověření pomocí Mobile Engagement REST API – ruční instalaci</span><span class="sxs-lookup"><span data-stu-id="c0b53-103">Authenticate with Mobile Engagement REST APIs - manual setup</span></span>
<span data-ttu-id="c0b53-104">Tato dokumentace příloha je příliš[ověřit pomocí rozhraní API REST Mobile Engagement](mobile-engagement-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="c0b53-104">This is an appendix documentation too[Authenticate with Mobile Engagement REST APIs](mobile-engagement-api-authentication.md).</span></span> <span data-ttu-id="c0b53-105">Zajistěte, aby že čtení první tooget hello kontextu.</span><span class="sxs-lookup"><span data-stu-id="c0b53-105">Make sure you read it first tooget hello context.</span></span> <span data-ttu-id="c0b53-106">Popisuje jiný způsob, jak toodo hello jednorázové instalační program pro nastavení ověřování pro použití rozhraní REST API Mobile Engagement hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0b53-106">This describes an alternate way toodo hello One-time setup for setting up your authentication for hello Mobile Engagement REST APIs using hello Azure Portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="c0b53-107">níže uvedené pokyny Hello jsou založené na tomto [služby Active Directory průvodce](../azure-resource-manager/resource-group-create-service-principal-portal.md) a přizpůsobit pro co je vyžadován pro ověření pro zapojení mobilních rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c0b53-107">hello instructions below are based on this [Active Directory guide](../azure-resource-manager/resource-group-create-service-principal-portal.md) and customized for what is required for authentication for Mobile Engagement APIs.</span></span> <span data-ttu-id="c0b53-108">Pokud chcete, aby toounderstand hello následující postup podrobně tak naleznete tooit.</span><span class="sxs-lookup"><span data-stu-id="c0b53-108">So refer tooit if you want toounderstand hello steps below in detail.</span></span> 
> 
> 

1. <span data-ttu-id="c0b53-109">Přihlášení tooyour účet Azure prostřednictvím hello [portálu classic](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="c0b53-109">Login tooyour Azure Account through hello [classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="c0b53-110">Vyberte **služby Active Directory** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="c0b53-110">Select **Active Directory** from hello left pane.</span></span>
   
     ![Vyberte možnost Active Directory][1]
3. <span data-ttu-id="c0b53-112">Zvolte hello **výchozí služby Active Directory** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c0b53-112">Choose hello **Default Active Directory** in your Azure portal.</span></span> 
   
     ![Vybrat adresář.][2]
   
   > [!IMPORTANT]
   > <span data-ttu-id="c0b53-114">Tento postup funguje jenom v případě, že pracujete v hello výchozí služby Active Directory vašeho účtu a nebude fungovat, pokud je to dělají ve službě Active Directory, kterou jste vytvořili ve vašem účtu.</span><span class="sxs-lookup"><span data-stu-id="c0b53-114">This approach works only when you are working in hello default Active Directory of your account and will not work if you are doing this in an Active Directory that you have created in your account.</span></span> 
   > 
   > 
4. <span data-ttu-id="c0b53-115">aplikace hello tooview ve vašem adresáři, klikněte na **aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-115">tooview hello applications in your directory, click on **Applications**.</span></span>
   
     ![Zobrazit aplikace][3]
5. <span data-ttu-id="c0b53-117">Klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-117">Click on **ADD**.</span></span> 
   
     ![Přidání aplikace][4]
6. <span data-ttu-id="c0b53-119">Klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**</span><span class="sxs-lookup"><span data-stu-id="c0b53-119">Click on **Add an application my organization is developing**</span></span>
   
     ![Nová aplikace][5]
7. <span data-ttu-id="c0b53-121">Zadejte název aplikace hello a vyberte hello typu aplikace jako **webové aplikace nebo webové rozhraní API** a klikněte na tlačítko Další hello.</span><span class="sxs-lookup"><span data-stu-id="c0b53-121">Fill in name of hello application and select hello type of application as **WEB APPLICATION AND/OR WEB API** and click hello next button.</span></span>
   
     ![název aplikace][6]
8. <span data-ttu-id="c0b53-123">Můžete zadat všechny fiktivní adresy URL pro **adresa URL přihlašování** a **identifikátor ID URI aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-123">You can provide any dummy URLs for **SIGN-ON URL** and **APP ID URI**.</span></span> <span data-ttu-id="c0b53-124">Nejsou použity pro náš scénář a adresy URL hello sami nejsou ověřené.</span><span class="sxs-lookup"><span data-stu-id="c0b53-124">They are not used for our scenario and hello URLs themselves are not validated.</span></span>  
   
     ![Vlastnosti aplikace][7]
9. <span data-ttu-id="c0b53-126">Na konci hello tohoto budete mít AAD aplikace, které jste zadali dříve jako následující hello názvem hello.</span><span class="sxs-lookup"><span data-stu-id="c0b53-126">At hello end of this, you will have an AAD app with hello name you provided previously like hello following.</span></span> <span data-ttu-id="c0b53-127">Toto je vaše **AD\_aplikace\_název** a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="c0b53-127">This is your **AD\_APP\_NAME** and make a note of it.</span></span>  
   
     ![Název aplikace.][8]
10. <span data-ttu-id="c0b53-129">Klikněte na název aplikace hello a klikněte na **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-129">Click on hello app name and click on **Configure**.</span></span>
    
      ![Konfigurace aplikace][9]
11. <span data-ttu-id="c0b53-131">Poznamenejte si hello ID klienta, který se použije jako **klienta\_ID** pro vaše rozhraní API volání.</span><span class="sxs-lookup"><span data-stu-id="c0b53-131">Make a note of hello CLIENT ID that will be used as **CLIENT\_ID** for your API calls.</span></span> 
    
     ![Konfigurace aplikace][10]
12. <span data-ttu-id="c0b53-133">Projděte dolů toohello **klíče** a přidání klíče se nejlépe doba trvání 2 roky (vypršení platnosti) a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-133">Scroll down toohello **Keys** section and add a key with preferably 2 years (expiry) duration and click **Save**.</span></span> 
    
     ![Konfigurace aplikace][11]
13. <span data-ttu-id="c0b53-135">Zkopírujte okamžitě hello hodnotu, která je pro klíč hello zobrazen, jak se zobrazují pouze nyní a nejsou uložena, se někdy znovu nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="c0b53-135">Immediately copy hello value which is shown for hello key as it is only shown now and is not stored so will not be displayed ever again.</span></span> <span data-ttu-id="c0b53-136">Pokud ho ztratíte pak budete mít toogenerate nový klíč.</span><span class="sxs-lookup"><span data-stu-id="c0b53-136">If you lose it then you will have toogenerate a new key.</span></span> <span data-ttu-id="c0b53-137">Bude jím hello **tajný klíč CLIENT_SECRET** pro vaše rozhraní API volání.</span><span class="sxs-lookup"><span data-stu-id="c0b53-137">This will be hello **CLIENT_SECRET** for your API calls.</span></span> 
    
     ![Konfigurace aplikace][12]
    
    > [!IMPORTANT]
    > <span data-ttu-id="c0b53-139">Tento klíč vyprší na konci hello hello trvání určeného Ano toorenew se, ujistěte se, které je čas hello vycházejí jinak ověřování rozhraní API už nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="c0b53-139">This key will expire at hello end of hello duration that you specified so make sure toorenew it when hello time comes otherwise your API authentication will not work anymore.</span></span> <span data-ttu-id="c0b53-140">Můžete také odstranit a znovu vytvořit tento klíč, pokud se domníváte, že byl napaden.</span><span class="sxs-lookup"><span data-stu-id="c0b53-140">You can also delete and recreate this key if you think that it has been compromised.</span></span>
    > 
    > 
14. <span data-ttu-id="c0b53-141">Klikněte na **zobrazit koncové body** tlačítko nyní které se otevře hello **koncových bodů aplikace** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c0b53-141">Click on **VIEW ENDPOINTS** button now which will open up hello **App Endpoints** dialog box.</span></span> 
    
    ![][13]
15. <span data-ttu-id="c0b53-142">Z dialogového okna koncových bodů aplikace hello, zkopírujte hello **koncový bod TOKENU OAUTH 2.0**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-142">From hello App Endpoints dialog box, copy hello **OAUTH 2.0 TOKEN ENDPOINT**.</span></span> 
    
    ![][14]
16. <span data-ttu-id="c0b53-143">Tento koncový bod se bude nacházet v hello následující formulář, kde je hello identifikátor GUID v adrese URL hello vaše **TENANT_ID** , poznamenejte si ji:</span><span class="sxs-lookup"><span data-stu-id="c0b53-143">This endpoint will be in hello following form where hello GUID in hello URL is your **TENANT_ID** so make a note of it:</span></span> 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. <span data-ttu-id="c0b53-144">Nyní jsme bude pokračovat tooconfigure hello oprávnění v této aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c0b53-144">Now we will proceed tooconfigure hello permissions on this app.</span></span> <span data-ttu-id="c0b53-145">To bude mít tooopen až hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c0b53-145">For this you will have tooopen up hello [Azure portal](https://portal.azure.com).</span></span> 
18. <span data-ttu-id="c0b53-146">Klikněte na **skupiny prostředků** a najde hello **Mobile Engagement** skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c0b53-146">Click on **Resource Groups** and find hello **Mobile Engagement** resource group.</span></span>  
    
    ![][15]
19. <span data-ttu-id="c0b53-147">Klikněte na tlačítko hello **Mobile Engagement** prostředků skupiny a přejděte toohello **nastavení** okno sem.</span><span class="sxs-lookup"><span data-stu-id="c0b53-147">Click hello **Mobile Engagement** resource group and navigate toohello **Settings** blade here.</span></span> 
    
    ![][16]
20. <span data-ttu-id="c0b53-148">Klikněte na **uživatelé** v hello okno nastavení a potom klikněte na **přidat** tooadd uživatele.</span><span class="sxs-lookup"><span data-stu-id="c0b53-148">Click on **Users** in hello Settings blade and then click on **Add** tooadd a user.</span></span> 
    
    ![][17]
21. <span data-ttu-id="c0b53-149">Klikněte na **vybrat roli**</span><span class="sxs-lookup"><span data-stu-id="c0b53-149">Click on **Select a role**</span></span>
    
    ![][18]
22. <span data-ttu-id="c0b53-150">Klikněte na **vlastníka**</span><span class="sxs-lookup"><span data-stu-id="c0b53-150">Click on **Owner**</span></span>
    
    ![][19]
23. <span data-ttu-id="c0b53-151">Vyhledávání pro název vaší aplikace hello **AD\_aplikace\_název** hello vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="c0b53-151">Search for hello name of your application **AD\_APP\_NAME** in hello Search box.</span></span> <span data-ttu-id="c0b53-152">Se nezobrazí ve výchozím nastavení v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="c0b53-152">You will not see this by default here.</span></span> <span data-ttu-id="c0b53-153">Po nalezení, vyberte ho a klikněte na **vyberte** v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="c0b53-153">Once you find it, select it and click on **Select** at hello bottom of hello blade.</span></span> 
    
    ![][20]
24. <span data-ttu-id="c0b53-154">Na hello **přidat přístup** okně se zobrazí jako **1 uživatel, 0 skupiny**.</span><span class="sxs-lookup"><span data-stu-id="c0b53-154">On hello **Add Access** blade, it will show up as **1 user, 0 groups**.</span></span> <span data-ttu-id="c0b53-155">Klikněte na tlačítko **OK** na tuto změnu hello tooconfirm okno.</span><span class="sxs-lookup"><span data-stu-id="c0b53-155">Click **OK** on this blade tooconfirm hello change.</span></span> 
    
    ![][21]

<span data-ttu-id="c0b53-156">Teď jste dokončili hello vyžaduje konfiguraci AAD a jsou všechny hello toocall sadu rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c0b53-156">You have now completed hello required AAD configuration and you are all set toocall hello APIs.</span></span> 

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



