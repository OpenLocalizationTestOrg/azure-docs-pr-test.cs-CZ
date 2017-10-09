---
title: "aaaSet vlastní domovskou stránku pro publikovaných aplikací pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje hello základní informace o Azure AD Application Proxy konektory"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 5bb695e904d285c3b440520f107c7bf63ba5cac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a><span data-ttu-id="fd9c5-103">Nastavit vlastní domovskou stránku pro publikovaných aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd9c5-103">Set a custom home page for published apps by using Azure AD Application Proxy</span></span>

<span data-ttu-id="fd9c5-104">Tento článek popisuje jak tooconfigure aplikace toodirect uživatelé tooa vlastní domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-104">This article discusses how tooconfigure apps toodirect users tooa custom home page.</span></span> <span data-ttu-id="fd9c5-105">Při publikování aplikace pomocí Proxy aplikací nastavíte interní adresa URL někdy je to ale není vaši uživatelé měli vidět první stránku hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-105">When you publish an application with Application Proxy, you set an internal URL but sometimes that's not hello page your users should see first.</span></span> <span data-ttu-id="fd9c5-106">Nastavte vlastní domovskou stránku tak, aby vaši uživatelé přejít pravá stránka toohello při přístupu aplikace hello z hello přístupový Panel Azure Active Directory nebo Spouštěč aplikace hello Office 365.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-106">Set a custom home page so that your users go toohello right page when they access hello apps from hello Azure Active Directory Access Panel or hello Office 365 app launcher.</span></span>

<span data-ttu-id="fd9c5-107">Když uživatelé spustí aplikaci hello, budou se přesměruje ve výchozí toohello kořenové domény URL pro hello publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-107">When users launch hello app, they're directed by default toohello root domain URL for hello published app.</span></span> <span data-ttu-id="fd9c5-108">Cílová stránka Hello se obvykle nastavuje jako adresa URL domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-108">hello landing page is typically set as hello home page URL.</span></span> <span data-ttu-id="fd9c5-109">Hello Azure AD PowerShell modulu toodefine vlastní domovskou stránku URL používejte, když chcete, aby uživatelé tooland aplikaci na konkrétní stránky v rámci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-109">Use hello Azure AD PowerShell module toodefine custom home page URLs when you want app users tooland on a specific page within hello app.</span></span> 

<span data-ttu-id="fd9c5-110">Například:</span><span class="sxs-lookup"><span data-stu-id="fd9c5-110">For example:</span></span>
- <span data-ttu-id="fd9c5-111">Uvnitř podnikové sítě, uživatelé přejít příliš*https://ExpenseApp/login/login.aspx* toosign v a přístup k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-111">Inside your corporate network, users go too*https://ExpenseApp/login/login.aspx* toosign in and access your app.</span></span>
- <span data-ttu-id="fd9c5-112">Protože máte dalších prostředků, jako jsou bitové kopie, které Proxy aplikace potřebuje tooaccess na nejvyšší úrovni hello struktura složek hello, můžete publikovat aplikaci hello s *https://ExpenseApp* jako hello interní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-112">Because you have other assets like images that Application Proxy needs tooaccess at hello top level of hello folder structure, you publish hello app with *https://ExpenseApp* as hello internal URL.</span></span>
- <span data-ttu-id="fd9c5-113">Hello výchozí externí adresa URL *https://ExpenseApp-contoso.msappproxy.net*, který neberou toohello přihlašovacích údajů uživatele na stránce.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-113">hello default external URL is *https://ExpenseApp-contoso.msappproxy.net*, which doesn't take your users toohello sign in page.</span></span>  
- <span data-ttu-id="fd9c5-114">Nastavit *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* jako toogive URL domovskou stránku hello uživatelům jednoduché prostředí.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-114">Set *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* as hello home page URL toogive your users a seamless experience.</span></span> 

>[!NOTE]
><span data-ttu-id="fd9c5-115">Když poskytnete uživatelům přístup toopublished aplikace, hello aplikace se zobrazují v hello [přístupový Panel Azure AD](active-directory-saas-access-panel-introduction.md) a hello [Spouštěč aplikace Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span><span class="sxs-lookup"><span data-stu-id="fd9c5-115">When you give users access toopublished apps, hello apps are displayed in hello [Azure AD Access Panel](active-directory-saas-access-panel-introduction.md) and hello [Office 365 app launcher](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="fd9c5-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="fd9c5-116">Before you start</span></span>

<span data-ttu-id="fd9c5-117">Před nastavením URL domovskou stránku hello, mějte na paměti hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="fd9c5-117">Before you set hello home page URL, keep in mind hello following requirements:</span></span>

* <span data-ttu-id="fd9c5-118">Zkontrolujte, zda tento hello cesta, kterou zadáte cestu subdomény hello kořenové domény adresy URL.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-118">Ensure that hello path you specify is a subdomain path of hello root domain URL.</span></span>

  <span data-ttu-id="fd9c5-119">Pokud adresa URL hello kořenové domény, například https://apps.contoso.com/app1/, hello domovskou stránku adresa URL, kterou nakonfigurujete musí začínat https://apps.contoso.com/app1/.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-119">If hello root-domain URL is, for example, https://apps.contoso.com/app1/, hello home page URL that you configure must start with https://apps.contoso.com/app1/.</span></span>

* <span data-ttu-id="fd9c5-120">Pokud změníte toohello publikované aplikace, změnit hello může resetovat hello hodnotu adresy URL domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-120">If you make a change toohello published app, hello change might reset hello value of hello home page URL.</span></span> <span data-ttu-id="fd9c5-121">Při aktualizaci aplikace hello v hello budoucí doporučujeme znovu zkontrolovat a, v případě potřeby aktualizujte adresu URL hello domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-121">When you update hello app in hello future, you should recheck and, if necessary, update hello home page URL.</span></span>

## <a name="change-hello-home-page-in-hello-azure-portal"></a><span data-ttu-id="fd9c5-122">Změnit domovskou stránku hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fd9c5-122">Change hello home page in hello Azure portal</span></span>

1. <span data-ttu-id="fd9c5-123">Přihlaste se toohello [portál Azure](https://portal.azure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-123">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="fd9c5-124">Přejděte příliš**Azure Active Directory** > **registrace aplikace** a vyberte aplikaci ze seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-124">Navigate too**Azure Active Directory** > **App registrations** and choose your application from hello list.</span></span> 
3. <span data-ttu-id="fd9c5-125">Vyberte **vlastnosti** z nastavení hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-125">Select **Properties** from hello settings.</span></span>
4. <span data-ttu-id="fd9c5-126">Aktualizace hello **adresa URL domovské stránky** pole s novou cestu.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-126">Update hello **Home page URL** field with your new path.</span></span> 

   ![Zadejte novou adresu URL domovské stránky](./media/application-proxy-office365-app-launcher/homepage.png)

5. <span data-ttu-id="fd9c5-128">Vyberte **uložit**</span><span class="sxs-lookup"><span data-stu-id="fd9c5-128">Select **Save**</span></span>

## <a name="change-hello-home-page-with-powershell"></a><span data-ttu-id="fd9c5-129">Změnit hello domovskou stránku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fd9c5-129">Change hello home page with PowerShell</span></span>

### <a name="install-hello-azure-ad-powershell-module"></a><span data-ttu-id="fd9c5-130">Instalace modulu Azure AD PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="fd9c5-130">Install hello Azure AD PowerShell module</span></span>

<span data-ttu-id="fd9c5-131">Před definujete adresu URL vlastní domovskou stránku pomocí prostředí PowerShell, nainstalujte modul Azure AD PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-131">Before you define a custom home page URL by using PowerShell, install hello Azure AD PowerShell module.</span></span> <span data-ttu-id="fd9c5-132">Hello balíček si můžete stáhnout z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), která používá hello koncový bod rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-132">You can download hello package from hello [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), which uses hello Graph API endpoint.</span></span> 

<span data-ttu-id="fd9c5-133">tooinstall hello balíček, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fd9c5-133">tooinstall hello package, follow these steps:</span></span>

1. <span data-ttu-id="fd9c5-134">Otevřete standardní okno prostředí PowerShell a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="fd9c5-134">Open a standard PowerShell window, and then run hello following command:</span></span>

    ```
     Install-Module -Name AzureAD
    ```
    <span data-ttu-id="fd9c5-135">Pokud používáte hello příkaz jako bez oprávnění správce, použijte hello `-scope currentuser` možnost.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-135">If you're running hello command as a non-admin, use hello `-scope currentuser` option.</span></span>
2. <span data-ttu-id="fd9c5-136">Během instalace hello vyberte **Y** tooinstall dva balíčky z Nuget.org. Oba balíčky jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-136">During hello installation, select **Y** tooinstall two packages from Nuget.org. Both packages are required.</span></span> 

### <a name="find-hello-objectid-of-hello-app"></a><span data-ttu-id="fd9c5-137">Najde hello ObjectID aplikace hello</span><span class="sxs-lookup"><span data-stu-id="fd9c5-137">Find hello ObjectID of hello app</span></span>

<span data-ttu-id="fd9c5-138">Získat hello ObjectID hello aplikace a pak vyhledejte aplikace hello podle jeho domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-138">Obtain hello ObjectID of hello app, and then search for hello app by its home page.</span></span>

1. <span data-ttu-id="fd9c5-139">Otevřete prostředí PowerShell a naimportovat modul hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-139">Open PowerShell and import hello Azure AD module.</span></span>

    ```
    Import-Module AzureAD
    ```

2. <span data-ttu-id="fd9c5-140">Přihlaste se toohello modulu Azure AD jako správce klienta hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-140">Sign in toohello Azure AD module as hello tenant administrator.</span></span>

    ```
    Connect-AzureAD
    ```
3. <span data-ttu-id="fd9c5-141">Najít hello aplikace založené na jeho adresa URL domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-141">Find hello app based on its home page URL.</span></span> <span data-ttu-id="fd9c5-142">Adresa URL hello můžete najít v portálu hello přechodem příliš**Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-142">You can find hello URL in hello portal by going too**Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="fd9c5-143">Tento příklad používá *sharepoint iddemo*.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-143">This example uses *sharepoint-iddemo*.</span></span>

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. <span data-ttu-id="fd9c5-144">Měli byste obdržet výsledek, který je podobný toohello té, kterou tady vidíte.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-144">You should get a result that's similar toohello one shown here.</span></span> <span data-ttu-id="fd9c5-145">Zkopírujte hello ObjectID GUID toouse v další části hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-145">Copy hello ObjectID GUID toouse in hello next section.</span></span>

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-hello-home-page-url"></a><span data-ttu-id="fd9c5-146">Aktualizovat adresu URL domovskou stránku hello</span><span class="sxs-lookup"><span data-stu-id="fd9c5-146">Update hello home page URL</span></span>

<span data-ttu-id="fd9c5-147">V hello provádět stejné modul prostředí PowerShell, který jste použili v kroku 1, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fd9c5-147">In hello same PowerShell module that you used for step 1, perform hello following steps:</span></span>

1. <span data-ttu-id="fd9c5-148">Potvrďte, že máte hello opravte aplikace a nahradit *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* s hello ObjectID, kterou jste zkopírovali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-148">Confirm that you have hello correct app, and replace *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* with hello ObjectID that you copied in hello preceding step.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 <span data-ttu-id="fd9c5-149">Teď, když jste potvrzení aplikace hello jste připravené tooupdate hello domovskou stránku, následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-149">Now that you've confirmed hello app, you're ready tooupdate hello home page, as follows.</span></span>

2. <span data-ttu-id="fd9c5-150">Vytvořte objekt toohold prázdné aplikace hello změny, které chcete toomake.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-150">Create a blank application object toohold hello changes that you want toomake.</span></span> <span data-ttu-id="fd9c5-151">Tato proměnná obsahuje hello hodnoty, které chcete tooupdate.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-151">This variable holds hello values that you want tooupdate.</span></span> <span data-ttu-id="fd9c5-152">Nic se vytvoří v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-152">Nothing is created in this step.</span></span>

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. <span data-ttu-id="fd9c5-153">Hello domovskou stránku URL toohello hodnotu, která chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-153">Set hello home page URL toohello value that you want.</span></span> <span data-ttu-id="fd9c5-154">Hello hodnota musí být cesta subdomény hello publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-154">hello value must be a subdomain path of hello published app.</span></span> <span data-ttu-id="fd9c5-155">Například pokud změníte hello adresu URL domovské stránky z *https://sharepoint-iddemo.msappproxy.net/* příliš*https://sharepoint-iddemo.msappproxy.net/hybrid/*, aplikace uživatelé přejít přímo toohello vlastní Domovská stránka.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-155">For example, if you change hello home page URL from *https://sharepoint-iddemo.msappproxy.net/* too*https://sharepoint-iddemo.msappproxy.net/hybrid/*, app users go directly toohello custom home page.</span></span>

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. <span data-ttu-id="fd9c5-156">Ujistěte se, hello aktualizace pomocí hello GUID (ObjectID), kterou jste zkopírovali v "krok 1: najít hello ObjectID hello aplikace."</span><span class="sxs-lookup"><span data-stu-id="fd9c5-156">Make hello update by using hello GUID (ObjectID) that you copied in "Step 1: Find hello ObjectID of hello app."</span></span>

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. <span data-ttu-id="fd9c5-157">tooconfirm, že změna hello bylo úspěšné, restartujte aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-157">tooconfirm that hello change was successful, restart hello app.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
><span data-ttu-id="fd9c5-158">Všechny změny provedené v aplikaci toohello může resetovat URL domovskou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-158">Any changes that you make toohello app might reset hello home page URL.</span></span> <span data-ttu-id="fd9c5-159">Pokud adresa URL domovské stránky obnoví, opakujte krok 2.</span><span class="sxs-lookup"><span data-stu-id="fd9c5-159">If your home page URL resets, repeat step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd9c5-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd9c5-160">Next steps</span></span>

- [<span data-ttu-id="fd9c5-161">Povolit tooSharePoint vzdáleného přístupu s proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="fd9c5-161">Enable remote access tooSharePoint with Azure AD Application Proxy</span></span>](application-proxy-enable-remote-access-sharepoint.md)
- [<span data-ttu-id="fd9c5-162">Povolení Proxy aplikace v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fd9c5-162">Enable Application Proxy in hello Azure portal</span></span>](active-directory-application-proxy-enable.md)
