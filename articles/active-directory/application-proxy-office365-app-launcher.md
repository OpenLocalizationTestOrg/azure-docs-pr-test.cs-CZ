---
title: "Nastavit vlastní domovskou stránku pro publikovaných aplikací pomocí proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje základní informace o Azure AD Application Proxy konektory"
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
ms.openlocfilehash: 9069166259265f5d2b43043b75039e239f397f6c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a><span data-ttu-id="e27c8-103">Nastavit vlastní domovskou stránku pro publikovaných aplikací pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e27c8-103">Set a custom home page for published apps by using Azure AD Application Proxy</span></span>

<span data-ttu-id="e27c8-104">Tento článek popisuje postup konfigurace aplikace, které uživatele nasměrují na vlastní domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="e27c8-104">This article discusses how to configure apps to direct users to a custom home page.</span></span> <span data-ttu-id="e27c8-105">Při publikování aplikace pomocí Proxy aplikací, můžete nastavit interní adresu URL ale někdy, není na stránku, kterou uživatelé měli vidět nejdřív.</span><span class="sxs-lookup"><span data-stu-id="e27c8-105">When you publish an application with Application Proxy, you set an internal URL but sometimes that's not the page your users should see first.</span></span> <span data-ttu-id="e27c8-106">Nastavte vlastní domovskou stránku tak, aby vaši uživatelé přejít na stránku správné při přístupu aplikace z panelu Azure Active Directory přístup nebo Spouštěč aplikace Office 365.</span><span class="sxs-lookup"><span data-stu-id="e27c8-106">Set a custom home page so that your users go to the right page when they access the apps from the Azure Active Directory Access Panel or the Office 365 app launcher.</span></span>

<span data-ttu-id="e27c8-107">Když uživatelé spustí aplikaci, budou se přesměruje ve výchozím nastavení adresa URL kořenové domény pro publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="e27c8-107">When users launch the app, they're directed by default to the root domain URL for the published app.</span></span> <span data-ttu-id="e27c8-108">Cílová stránka se obvykle nastavuje jako adresu URL pro domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="e27c8-108">The landing page is typically set as the home page URL.</span></span> <span data-ttu-id="e27c8-109">Modul Azure AD PowerShell slouží k definování adresy URL vlastní domovskou stránku, pokud chcete aplikace uživatelům zobrazovat na konkrétní stránky v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e27c8-109">Use the Azure AD PowerShell module to define custom home page URLs when you want app users to land on a specific page within the app.</span></span> 

<span data-ttu-id="e27c8-110">Například:</span><span class="sxs-lookup"><span data-stu-id="e27c8-110">For example:</span></span>
- <span data-ttu-id="e27c8-111">Uvnitř podnikové sítě, uživatelé přejít na *https://ExpenseApp/login/login.aspx* přihlásit a přístup k vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e27c8-111">Inside your corporate network, users go to *https://ExpenseApp/login/login.aspx* to sign in and access your app.</span></span>
- <span data-ttu-id="e27c8-112">Protože máte další prostředky, jako jsou bitové kopie, které Proxy aplikace potřebuje přístup k na nejvyšší úrovni struktura složek, můžete publikovat aplikaci s *https://ExpenseApp* jako interní adresa URL.</span><span class="sxs-lookup"><span data-stu-id="e27c8-112">Because you have other assets like images that Application Proxy needs to access at the top level of the folder structure, you publish the app with *https://ExpenseApp* as the internal URL.</span></span>
- <span data-ttu-id="e27c8-113">Externí adresa URL výchozí *https://ExpenseApp-contoso.msappproxy.net*, který neberou uživatelům na přihlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="e27c8-113">The default external URL is *https://ExpenseApp-contoso.msappproxy.net*, which doesn't take your users to the sign in page.</span></span>  
- <span data-ttu-id="e27c8-114">Nastavit *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* jako adresu URL domovské stránky umožnit uživatelům jednoduché prostředí.</span><span class="sxs-lookup"><span data-stu-id="e27c8-114">Set *https://ExpenseApp-contoso.msappproxy.net/login/login.aspx* as the home page URL to give your users a seamless experience.</span></span> 

>[!NOTE]
><span data-ttu-id="e27c8-115">Pokud jste uživatelům přístup k publikovaným aplikacím, aplikace se zobrazují v [přístupový Panel Azure AD](active-directory-saas-access-panel-introduction.md) a [Spouštěč aplikace Office 365](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span><span class="sxs-lookup"><span data-stu-id="e27c8-115">When you give users access to published apps, the apps are displayed in the [Azure AD Access Panel](active-directory-saas-access-panel-introduction.md) and the [Office 365 app launcher](https://blogs.office.com/2016/09/27/introducing-the-new-office-365-app-launcher).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="e27c8-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="e27c8-116">Before you start</span></span>

<span data-ttu-id="e27c8-117">Než budete nastavit adresu URL domovskou stránku, mějte na paměti následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="e27c8-117">Before you set the home page URL, keep in mind the following requirements:</span></span>

* <span data-ttu-id="e27c8-118">Zajistěte, aby cestu, kterou zadáte cestu subdomény kořenové domény adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e27c8-118">Ensure that the path you specify is a subdomain path of the root domain URL.</span></span>

  <span data-ttu-id="e27c8-119">Pokud adresa URL kořenové domény, například https://apps.contoso.com/app1/, adresu URL domovskou stránku, kterou nakonfigurujete musí začínat https://apps.contoso.com/app1/.</span><span class="sxs-lookup"><span data-stu-id="e27c8-119">If the root-domain URL is, for example, https://apps.contoso.com/app1/, the home page URL that you configure must start with https://apps.contoso.com/app1/.</span></span>

* <span data-ttu-id="e27c8-120">Pokud změníte publikované aplikace, může změna resetovat hodnotu adresy URL domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e27c8-120">If you make a change to the published app, the change might reset the value of the home page URL.</span></span> <span data-ttu-id="e27c8-121">Při aktualizaci aplikace v budoucnu, měli znovu zkontrolovat a v případě potřeby aktualizujte adresu URL domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e27c8-121">When you update the app in the future, you should recheck and, if necessary, update the home page URL.</span></span>

## <a name="change-the-home-page-in-the-azure-portal"></a><span data-ttu-id="e27c8-122">Změnit domovské stránce portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e27c8-122">Change the home page in the Azure portal</span></span>

1. <span data-ttu-id="e27c8-123">Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.</span><span class="sxs-lookup"><span data-stu-id="e27c8-123">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="e27c8-124">Přejděte na **Azure Active Directory** > **registrace aplikace** a vyberte aplikaci ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="e27c8-124">Navigate to **Azure Active Directory** > **App registrations** and choose your application from the list.</span></span> 
3. <span data-ttu-id="e27c8-125">Vyberte **vlastnosti** z nastavení.</span><span class="sxs-lookup"><span data-stu-id="e27c8-125">Select **Properties** from the settings.</span></span>
4. <span data-ttu-id="e27c8-126">Aktualizace **adresa URL domovské stránky** pole s novou cestu.</span><span class="sxs-lookup"><span data-stu-id="e27c8-126">Update the **Home page URL** field with your new path.</span></span> 

   ![Zadejte novou adresu URL domovské stránky](./media/application-proxy-office365-app-launcher/homepage.png)

5. <span data-ttu-id="e27c8-128">Vyberte **uložit**</span><span class="sxs-lookup"><span data-stu-id="e27c8-128">Select **Save**</span></span>

## <a name="change-the-home-page-with-powershell"></a><span data-ttu-id="e27c8-129">Změnit domovskou stránku pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e27c8-129">Change the home page with PowerShell</span></span>

### <a name="install-the-azure-ad-powershell-module"></a><span data-ttu-id="e27c8-130">Instalace modulu Azure AD PowerShell</span><span class="sxs-lookup"><span data-stu-id="e27c8-130">Install the Azure AD PowerShell module</span></span>

<span data-ttu-id="e27c8-131">Před definujete adresu URL vlastní domovskou stránku pomocí prostředí PowerShell, nainstalujte modul Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e27c8-131">Before you define a custom home page URL by using PowerShell, install the Azure AD PowerShell module.</span></span> <span data-ttu-id="e27c8-132">Si můžete stáhnout balíček z [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), který používá koncový bod rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="e27c8-132">You can download the package from the [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAD/2.0.0.131), which uses the Graph API endpoint.</span></span> 

<span data-ttu-id="e27c8-133">K instalaci balíčku, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e27c8-133">To install the package, follow these steps:</span></span>

1. <span data-ttu-id="e27c8-134">Otevřete standardní okno prostředí PowerShell a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e27c8-134">Open a standard PowerShell window, and then run the following command:</span></span>

    ```
     Install-Module -Name AzureAD
    ```
    <span data-ttu-id="e27c8-135">Pokud příkaz spouštíte jako bez oprávnění správce, použijte `-scope currentuser` možnost.</span><span class="sxs-lookup"><span data-stu-id="e27c8-135">If you're running the command as a non-admin, use the `-scope currentuser` option.</span></span>
2. <span data-ttu-id="e27c8-136">Během instalace, vyberte **Y** instalace dva balíčky z Nuget.org. Oba balíčky jsou povinné.</span><span class="sxs-lookup"><span data-stu-id="e27c8-136">During the installation, select **Y** to install two packages from Nuget.org. Both packages are required.</span></span> 

### <a name="find-the-objectid-of-the-app"></a><span data-ttu-id="e27c8-137">Najít ObjectID aplikace</span><span class="sxs-lookup"><span data-stu-id="e27c8-137">Find the ObjectID of the app</span></span>

<span data-ttu-id="e27c8-138">Získejte ObjectID aplikace a pak vyhledejte aplikaci podle jeho domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e27c8-138">Obtain the ObjectID of the app, and then search for the app by its home page.</span></span>

1. <span data-ttu-id="e27c8-139">Otevřete prostředí PowerShell a naimportujte modul Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e27c8-139">Open PowerShell and import the Azure AD module.</span></span>

    ```
    Import-Module AzureAD
    ```

2. <span data-ttu-id="e27c8-140">Přihlaste se k modulu Azure AD jako správce klienta.</span><span class="sxs-lookup"><span data-stu-id="e27c8-140">Sign in to the Azure AD module as the tenant administrator.</span></span>

    ```
    Connect-AzureAD
    ```
3. <span data-ttu-id="e27c8-141">Najděte aplikaci, podle jeho adresa URL domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e27c8-141">Find the app based on its home page URL.</span></span> <span data-ttu-id="e27c8-142">Adresu URL můžete najít na portálu přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e27c8-142">You can find the URL in the portal by going to **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span> <span data-ttu-id="e27c8-143">Tento příklad používá *sharepoint iddemo*.</span><span class="sxs-lookup"><span data-stu-id="e27c8-143">This example uses *sharepoint-iddemo*.</span></span>

    ```
    Get-AzureADApplication | where { $_.Homepage -like “sharepoint-iddemo” } | fl DisplayName, Homepage, ObjectID
    ```
4. <span data-ttu-id="e27c8-144">Měli byste obdržet výsledek, který je podobný znázorněno zde.</span><span class="sxs-lookup"><span data-stu-id="e27c8-144">You should get a result that's similar to the one shown here.</span></span> <span data-ttu-id="e27c8-145">Zkopírujte identifikátor GUID ObjectID pro použití v další části.</span><span class="sxs-lookup"><span data-stu-id="e27c8-145">Copy the ObjectID GUID to use in the next section.</span></span>

    ```
    DisplayName : SharePoint
    Homepage    : https://sharepoint-iddemo.msappproxy.net/
    ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

### <a name="update-the-home-page-url"></a><span data-ttu-id="e27c8-146">Aktualizovat adresu URL domovské stránky</span><span class="sxs-lookup"><span data-stu-id="e27c8-146">Update the home page URL</span></span>

<span data-ttu-id="e27c8-147">Ve stejném modulu prostředí PowerShell, který jste použili v kroku 1 proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e27c8-147">In the same PowerShell module that you used for step 1, perform the following steps:</span></span>

1. <span data-ttu-id="e27c8-148">Zkontrolujte, jestli máte správné aplikace a nahradit *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* s ID objektu, který jste zkopírovali v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="e27c8-148">Confirm that you have the correct app, and replace *8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4* with the ObjectID that you copied in the preceding step.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4.
    ```

 <span data-ttu-id="e27c8-149">Teď, když jste potvrdí aplikace, budete připraveni k aktualizaci domovské stránce následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e27c8-149">Now that you've confirmed the app, you're ready to update the home page, as follows.</span></span>

2. <span data-ttu-id="e27c8-150">Vytvořte objekt prázdné aplikace pro uložení změny, které chcete nastavit.</span><span class="sxs-lookup"><span data-stu-id="e27c8-150">Create a blank application object to hold the changes that you want to make.</span></span> <span data-ttu-id="e27c8-151">Tato proměnná obsahuje hodnoty, které chcete aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e27c8-151">This variable holds the values that you want to update.</span></span> <span data-ttu-id="e27c8-152">Nic se vytvoří v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="e27c8-152">Nothing is created in this step.</span></span>

    ```
    $appnew = New-Object “Microsoft.Open.AzureAD.Model.Application”
    ```

3. <span data-ttu-id="e27c8-153">Na hodnotu, která chcete nastavte adresu URL domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e27c8-153">Set the home page URL to the value that you want.</span></span> <span data-ttu-id="e27c8-154">Hodnota musí být cesta subdomény publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="e27c8-154">The value must be a subdomain path of the published app.</span></span> <span data-ttu-id="e27c8-155">Například, pokud změníte adresu URL domovské stránky z *https://sharepoint-iddemo.msappproxy.net/* k *https://sharepoint-iddemo.msappproxy.net/hybrid/*, uživatelům aplikace přejít přímo na domovské stránce vlastní .</span><span class="sxs-lookup"><span data-stu-id="e27c8-155">For example, if you change the home page URL from *https://sharepoint-iddemo.msappproxy.net/* to *https://sharepoint-iddemo.msappproxy.net/hybrid/*, app users go directly to the custom home page.</span></span>

    ```
    $homepage = “https://sharepoint-iddemo.msappproxy.net/hybrid/”
    ```
4. <span data-ttu-id="e27c8-156">Proveďte aktualizace pomocí identifikátoru GUID (ObjectID), kterou jste zkopírovali v "krok 1: najít ObjectID aplikace."</span><span class="sxs-lookup"><span data-stu-id="e27c8-156">Make the update by using the GUID (ObjectID) that you copied in "Step 1: Find the ObjectID of the app."</span></span>

    ```
    Set-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4 -Homepage $homepage
    ```
5. <span data-ttu-id="e27c8-157">Potvrďte, že tato změna byla úspěšná, restartujte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e27c8-157">To confirm that the change was successful, restart the app.</span></span>

    ```
    Get-AzureADApplication -ObjectId 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
    ```

>[!NOTE]
><span data-ttu-id="e27c8-158">Veškeré změny, které provedete aplikace může resetovat adresu URL domovské stránky.</span><span class="sxs-lookup"><span data-stu-id="e27c8-158">Any changes that you make to the app might reset the home page URL.</span></span> <span data-ttu-id="e27c8-159">Pokud adresa URL domovské stránky obnoví, opakujte krok 2.</span><span class="sxs-lookup"><span data-stu-id="e27c8-159">If your home page URL resets, repeat step 2.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e27c8-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e27c8-160">Next steps</span></span>

- [<span data-ttu-id="e27c8-161">Povolte vzdálený přístup na SharePoint s proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e27c8-161">Enable remote access to SharePoint with Azure AD Application Proxy</span></span>](application-proxy-enable-remote-access-sharepoint.md)
- [<span data-ttu-id="e27c8-162">Povolení Proxy aplikace na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e27c8-162">Enable Application Proxy in the Azure portal</span></span>](active-directory-application-proxy-enable.md)
