---
title: "Ověřování na základě záhlaví s PingAccess pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Publikování aplikací s PingAccess a Proxy aplikace pro podporu ověřování na základě záhlaví."
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
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 58034ab8830cf655199875b448948ea14dc04a70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a><span data-ttu-id="ebd80-103">Ověřování na základě záhlaví pro jednotné přihlašování s Proxy aplikace a PingAccess</span><span class="sxs-lookup"><span data-stu-id="ebd80-103">Header-based authentication for single sign-on with Application Proxy and PingAccess</span></span>

<span data-ttu-id="ebd80-104">Azure Proxy aplikace služby Active Directory a PingAccess společně společně poskytují zákazníkům Azure Active Directory přístup k i další aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-104">Azure Active Directory Application Proxy and PingAccess have partnered together to provide Azure Active Directory customers with access to even more applications.</span></span> <span data-ttu-id="ebd80-105">PingAccess rozšíří [stávající nabídky Proxy aplikace](active-directory-application-proxy-get-started.md) zahrnout jednoho přístupového přihlášení do aplikace, které používají hlavičky pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="ebd80-105">PingAccess expands the [existing Application Proxy offerings](active-directory-application-proxy-get-started.md) to include single sign-on access to applications that use headers for authentication.</span></span>

## <a name="what-is-pingaccess-for-azure-ad"></a><span data-ttu-id="ebd80-106">Co je PingAccess pro Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ebd80-106">What is PingAccess for Azure AD?</span></span>

<span data-ttu-id="ebd80-107">PingAccess pro Azure Active Directory je nabídky PingAccess, která umožňuje poskytnout přístup uživatelů a jednotné přihlašování pro aplikace, které používají hlavičky pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="ebd80-107">PingAccess for Azure Active Directory is an offering of PingAccess that enables you to give users access and single sign-on to applications that use headers for authentication.</span></span> <span data-ttu-id="ebd80-108">Proxy aplikace zpracovává tyto aplikace jako libovolný jiný, k ověření přístupu pomocí služby Azure AD a následné předání přenosy přes službu konektoru.</span><span class="sxs-lookup"><span data-stu-id="ebd80-108">Application Proxy treats these apps like any other, using Azure AD to authenticate access and then passing traffic through the connector service.</span></span> <span data-ttu-id="ebd80-109">PingAccess nachází před aplikace a přeloží tokenu přístupu z Azure AD na hlavičku a aplikace obdrží ověřování ve formátu, který může číst.</span><span class="sxs-lookup"><span data-stu-id="ebd80-109">PingAccess sits in front of the apps and translates the access token from Azure AD into a header so that the application receives the authentication in the format it can read.</span></span>

<span data-ttu-id="ebd80-110">Uživatelé nebudou Všimněte si, něco jinak při přihlášení používat podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-110">Your users won’t notice anything different when they sign in to use your corporate apps.</span></span> <span data-ttu-id="ebd80-111">Mohou i nadále pracovat z odkudkoli na libovolném zařízení.</span><span class="sxs-lookup"><span data-stu-id="ebd80-111">They can still work from anywhere on any device.</span></span> 

<span data-ttu-id="ebd80-112">Vzhledem k tomu, že konektory Proxy aplikace směrovat vzdálené provoz na všechny aplikace bez ohledu na jejich typ ověřování, budete dál automaticky, také Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="ebd80-112">Since the Application Proxy connectors direct remote traffic to all apps regardless of their authentication type, they’ll continue to load balance automatically, as well.</span></span>

## <a name="how-do-i-get-access"></a><span data-ttu-id="ebd80-113">Jak získám přístup?</span><span class="sxs-lookup"><span data-stu-id="ebd80-113">How do I get access?</span></span>

<span data-ttu-id="ebd80-114">Vzhledem k tomu, že se tento scénář nabízí prostřednictvím partnerství mezi Azure Active Directory a PingAccess, potřebujete licence pro obě služby.</span><span class="sxs-lookup"><span data-stu-id="ebd80-114">Since this scenario is offered through a partnership between Azure Active Directory and PingAccess, you need licenses for both services.</span></span> <span data-ttu-id="ebd80-115">Však předplatných Azure Active Directory Premium zahrnují základní PingAccess licencí, které pokrývá až 20 aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-115">However, Azure Active Directory Premium subscriptions include a basic PingAccess license that covers up to 20 applications.</span></span> <span data-ttu-id="ebd80-116">Pokud potřebujete publikovat více než 20 aplikace založené na záhlaví, si můžete zakoupit další licence od PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-116">If you need to publish more than 20 header-based applications, you can purchase an additional license from PingAccess.</span></span> 

<span data-ttu-id="ebd80-117">Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="ebd80-117">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="publish-your-application-in-azure"></a><span data-ttu-id="ebd80-118">Publikování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="ebd80-118">Publish your application in Azure</span></span>

<span data-ttu-id="ebd80-119">Tento článek je určený pro uživatele, kteří jsou při prvním publikování aplikace pomocí tento scénář.</span><span class="sxs-lookup"><span data-stu-id="ebd80-119">This article is intended for people who are publishing an app with this scenario for the first time.</span></span> <span data-ttu-id="ebd80-120">Provede jak začít pracovat s aplikací a PingAccess, kromě publikování kroky.</span><span class="sxs-lookup"><span data-stu-id="ebd80-120">It walks through how to get started with both Application and PingAccess, in addition to the publishing steps.</span></span> <span data-ttu-id="ebd80-121">Pokud jste již nakonfigurovali obě služby, ale chcete aktualizačnímu programu na publikování kroky, můžete přeskočit instalace konektoru a přesunout na [přidat aplikace do Azure AD pomocí Proxy aplikace](#add-your-app-to-Azure-AD-with-Application-Proxy).</span><span class="sxs-lookup"><span data-stu-id="ebd80-121">If you’ve already configured both services but want a refresher on the publishing steps, you can skip the connector installation and move on to [Add your app to Azure AD with Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span></span>

>[!NOTE]
><span data-ttu-id="ebd80-122">Vzhledem k tomu, že tento scénář je spolupráci mezi službou Azure AD a PingAccess, některé pokyny existovat na serveru Identity příkazu Ping.</span><span class="sxs-lookup"><span data-stu-id="ebd80-122">Since this scenario is a partnership between Azure AD and PingAccess, some of the instructions exist on the Ping Identity site.</span></span>

### <a name="install-an-application-proxy-connector"></a><span data-ttu-id="ebd80-123">Instalace konektoru Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="ebd80-123">Install an Application Proxy connector</span></span>

<span data-ttu-id="ebd80-124">Pokud už máte Proxy aplikace povolena a máte nainstalovaný konektor, můžete tuto část přeskočit a přesunout na [přidat aplikace do Azure AD pomocí Proxy aplikace](#add-your-app-to-azure-ad-with-application-proxy).</span><span class="sxs-lookup"><span data-stu-id="ebd80-124">If you already have Application Proxy enabled, and have a connector installed, you can skip this section and move on to [Add your app to Azure AD with Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span></span>

<span data-ttu-id="ebd80-125">Konektor Proxy aplikace je služba systému Windows Server, která přesměruje přenosy z vzdálení zaměstnanci k publikovaným aplikacím.</span><span class="sxs-lookup"><span data-stu-id="ebd80-125">The Application Proxy connector is a Windows Server service that directs the traffic from your remote employees to your published apps.</span></span> <span data-ttu-id="ebd80-126">Podrobné pokyny k instalaci, najdete v části [povolení Proxy aplikace v portálu Azure](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="ebd80-126">For more detailed installation instructions, see [Enable Application Proxy in the Azure portal](active-directory-application-proxy-enable.md).</span></span>

1. <span data-ttu-id="ebd80-127">Přihlaste se k [portál Azure](https://portal.azure.com) jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="ebd80-127">Sign in to the [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="ebd80-128">Vyberte **Azure Active Directory** > **proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-128">Select **Azure Active Directory** > **Application proxy**.</span></span>
3. <span data-ttu-id="ebd80-129">Vyberte **stáhnout konektor** zahájíte stahování konektoru Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-129">Select **Download Connector** to start the Application Proxy connector download.</span></span> <span data-ttu-id="ebd80-130">Postupujte podle pokynů pro instalaci.</span><span class="sxs-lookup"><span data-stu-id="ebd80-130">Follow the installation instructions.</span></span>

   ![Povolení Proxy aplikace a stáhnout konektor](./media/application-proxy-ping-access/install-connector.png)

4. <span data-ttu-id="ebd80-132">Stahování konektoru by měl automaticky povolení Proxy aplikace pro váš adresář, ale pokud ne, můžete si vybrat **povolení Proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-132">Downloading the connector should automatically enable Application Proxy for your directory, but if not you can select **Enable Application Proxy**.</span></span>


### <a name="add-your-app-to-azure-ad-with-application-proxy"></a><span data-ttu-id="ebd80-133">Přidat aplikaci do Azure AD pomocí Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="ebd80-133">Add your app to Azure AD with Application Proxy</span></span>

<span data-ttu-id="ebd80-134">Existují dvě akce, které je třeba provést na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ebd80-134">There are two actions you need to take in the Azure portal.</span></span> <span data-ttu-id="ebd80-135">Nejprve budete muset publikování aplikace pomocí Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-135">First, you need to publish your application with Application Proxy.</span></span> <span data-ttu-id="ebd80-136">Poté která potřebujete shromáždit určité informace o aplikaci, kterou můžete použít během PingAccess kroky.</span><span class="sxs-lookup"><span data-stu-id="ebd80-136">Then, you need to collect some information about the app that you can use during the PingAccess steps.</span></span>

<span data-ttu-id="ebd80-137">Postupujte podle těchto kroků k publikování aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-137">Follow these steps to publish your app.</span></span> <span data-ttu-id="ebd80-138">Pro podrobnější návod kroky 1-8, viz [publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ebd80-138">For a more detailed walkthrough of steps 1-8, see [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

1. <span data-ttu-id="ebd80-139">Jestliže jste v poslední části, přihlaste se k [portál Azure](https://portal.azure.com) jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="ebd80-139">If you didn't in the last section, sign in to the [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="ebd80-140">Vyberte **Azure Active Directory** > **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-140">Select **Azure Active Directory** > **Enterprise applications**.</span></span>
3. <span data-ttu-id="ebd80-141">Vyberte **přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="ebd80-141">Select **Add** at the top of the blade.</span></span>
4. <span data-ttu-id="ebd80-142">Vyberte **místní aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-142">Select **On-premises application**.</span></span>
5. <span data-ttu-id="ebd80-143">Vyplňte požadovaná pole s informacemi o nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-143">Fill out the required fields with information about your new app.</span></span> <span data-ttu-id="ebd80-144">Použijte následující pokyny pro nastavení:</span><span class="sxs-lookup"><span data-stu-id="ebd80-144">Use the following guidance for the settings:</span></span>
   - <span data-ttu-id="ebd80-145">**Interní adresa URL**: normálně zadejte adresu URL, kterou přejdete na přihlašovací stránce aplikace, když jste v podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="ebd80-145">**Internal URL**: Normally you provide the URL that takes you to the app’s sign in page when you’re on the corporate network.</span></span> <span data-ttu-id="ebd80-146">V tomto scénáři musí konektor PingAccess proxy považovat za titulní stránky aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-146">For this scenario the connector needs to treat the PingAccess proxy as the front page of the app.</span></span> <span data-ttu-id="ebd80-147">Použijte tento formát: `https://<host name of your PA server>:<port>`.</span><span class="sxs-lookup"><span data-stu-id="ebd80-147">Use this format: `https://<host name of your PA server>:<port>`.</span></span> <span data-ttu-id="ebd80-148">Port je 3000 ve výchozím nastavení, ale můžete ji nakonfigurovat v PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-148">The port is 3000 by default, but you can configure it in PingAccess.</span></span>
   - <span data-ttu-id="ebd80-149">**Metoda předběžného ověřování**: Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ebd80-149">**Pre-authentication method**: Azure Active Directory</span></span>
   - <span data-ttu-id="ebd80-150">**Převede adresu URL v hlavičkách**: Ne</span><span class="sxs-lookup"><span data-stu-id="ebd80-150">**Translate URL in Headers**: No</span></span>

   >[!NOTE]
   ><span data-ttu-id="ebd80-151">Pokud je vaší první aplikace, použijte ke spuštění a vrátit zpět a aktualizovat toto nastavení, pokud změníte konfiguraci PingAccess port 3000.</span><span class="sxs-lookup"><span data-stu-id="ebd80-151">If this is your first application, use port 3000 to start and come back to update this setting if you change your PingAccess configuration.</span></span> <span data-ttu-id="ebd80-152">Pokud je aplikace druhý nebo novější, to bude nutné naslouchací proces, který jste nakonfigurovali v PingAccess shodovat.</span><span class="sxs-lookup"><span data-stu-id="ebd80-152">If this is your second or later app, this will need to match the Listener you’ve configured in PingAccess.</span></span> <span data-ttu-id="ebd80-153">Další informace o [naslouchací procesy v PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span><span class="sxs-lookup"><span data-stu-id="ebd80-153">Learn more about [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span></span>

6. <span data-ttu-id="ebd80-154">Vyberte **přidat** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="ebd80-154">Select **Add** at the bottom of the blade.</span></span> <span data-ttu-id="ebd80-155">Přidání vaší aplikace a otevře se v nabídce rychlý start.</span><span class="sxs-lookup"><span data-stu-id="ebd80-155">Your application is added, and the quick start menu opens.</span></span>
7. <span data-ttu-id="ebd80-156">V nabídce rychlý start, vyberte **přiřadit uživatele pro testování**, a přidejte alespoň jednoho uživatele k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ebd80-156">In the quick start menu, select **Assign a user for testing**, and add at least one user to the application.</span></span> <span data-ttu-id="ebd80-157">Ujistěte se, že tento testovací účet má přístup k aplikaci místní.</span><span class="sxs-lookup"><span data-stu-id="ebd80-157">Make sure this test account has access to the on-premises application.</span></span>
8. <span data-ttu-id="ebd80-158">Vyberte **přiřadit** uložit přiřazení uživatelských testu.</span><span class="sxs-lookup"><span data-stu-id="ebd80-158">Select **Assign** to save the test user assignment.</span></span>
9. <span data-ttu-id="ebd80-159">V okně správy aplikace, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-159">On the app management blade, select **Single sign-on**.</span></span>
10. <span data-ttu-id="ebd80-160">Zvolte **na základě záhlaví přihlašování** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="ebd80-160">Choose **Header-based sign-on** from the drop-down menu.</span></span> <span data-ttu-id="ebd80-161">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-161">Select **Save**.</span></span>

   >[!TIP]
   ><span data-ttu-id="ebd80-162">Pokud je to na základě záhlaví jednotné přihlašování používáte poprvé, budete muset nainstalovat PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-162">If this is your first time using header-based single sign-on, you need to install PingAccess.</span></span> <span data-ttu-id="ebd80-163">Pokud chcete mít jistotu, že se automaticky přidruží instalace PingAccess vašeho předplatného Azure, použijte odkaz na této stránce jedné přihlašování ke stažení PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-163">To make sure your Azure subscription is automatically associated with your PingAccess installation, use the link on this single sign-on page to download PingAccess.</span></span> <span data-ttu-id="ebd80-164">Můžete otevřít web Stažení teď nebo vraťte na tuto stránku později.</span><span class="sxs-lookup"><span data-stu-id="ebd80-164">You can open the download site now, or come back to this page later.</span></span> 

   ![Vyberte na základě záhlaví přihlášení](./media/application-proxy-ping-access/sso-header.PNG)

11. <span data-ttu-id="ebd80-166">Zavřete okno aplikace Enterprise nebo posuňte se úplně levé straně si vrátit do nabídky Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ebd80-166">Close the Enterprise applications blade or scroll all the way to the left to return to the Azure Active Directory menu.</span></span>
12. <span data-ttu-id="ebd80-167">Vyberte **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-167">Select **App registrations**.</span></span>

   ![Vyberte aplikaci registrace](./media/application-proxy-ping-access/app-registrations.png)

13. <span data-ttu-id="ebd80-169">Vyberte aplikaci, které jste právě přidali, pak **adresy URL odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-169">Select the app you just added, then **Reply URLs**.</span></span>

   ![Vyberte adresy URL odpovědí](./media/application-proxy-ping-access/reply-urls.png)

14. <span data-ttu-id="ebd80-171">Zkontrolujte, zda externí adresu URL, který jste přiřadili do vaší aplikace v kroku 5 je v seznamu adresy URL odpovědí.</span><span class="sxs-lookup"><span data-stu-id="ebd80-171">Check to see if the external URL that you assigned to your app in step 5 is in the Reply URLs list.</span></span> <span data-ttu-id="ebd80-172">Pokud není, přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="ebd80-172">If it’s not, add it now.</span></span>
15. <span data-ttu-id="ebd80-173">V okně Nastavení aplikace vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-173">On the app settings blade, select **Required permissions**.</span></span>

  ![Vyberte požadovaná oprávnění](./media/application-proxy-ping-access/required-permissions.png)

16. <span data-ttu-id="ebd80-175">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-175">Select **Add**.</span></span> <span data-ttu-id="ebd80-176">Rozhraní API, zvolte **Windows Azure Active Directory**, pak **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-176">For the API, choose **Windows Azure Active Directory**, then **Select**.</span></span> <span data-ttu-id="ebd80-177">Oprávnění, zvolte **pro čtení a zápis všech aplikací** a **přihlášení a čtení profilu uživatele**, pak **vyberte** a **provádí**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-177">For the permissions, choose **Read and write all applications** and **Sign in and read user profile**, then **Select** and **Done**.</span></span>  

  ![Vyberte oprávnění](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-the-pingaccess-steps"></a><span data-ttu-id="ebd80-179">Shromažďovat informace o krocích PingAccess</span><span class="sxs-lookup"><span data-stu-id="ebd80-179">Collect information for the PingAccess steps</span></span>

1. <span data-ttu-id="ebd80-180">V okně nastavení vaší aplikace, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-180">On your app settings blade, select **Properties**.</span></span> 

  ![Vyberte vlastnosti](./media/application-proxy-ping-access/properties.png)

2. <span data-ttu-id="ebd80-182">Uložit **Id aplikace** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ebd80-182">Save the **Application Id** value.</span></span> <span data-ttu-id="ebd80-183">Používá se pro ID klienta při konfiguraci PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-183">This is used for the client ID when you configure PingAccess.</span></span>
3. <span data-ttu-id="ebd80-184">V okně Nastavení aplikace vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-184">On the app settings blade, select **Keys**.</span></span>

  ![Vyberte klíče](./media/application-proxy-ping-access/Keys.png)

4. <span data-ttu-id="ebd80-186">Vytvořte klíč zadáním klíče popis a v rozevírací nabídce zvolíte datum vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="ebd80-186">Create a key by entering a key description and choosing an expiration date from the drop-down menu.</span></span>
5. <span data-ttu-id="ebd80-187">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-187">Select **Save**.</span></span> <span data-ttu-id="ebd80-188">Identifikátor GUID se zobrazí v **hodnotu** pole.</span><span class="sxs-lookup"><span data-stu-id="ebd80-188">A GUID appears in the **Value** field.</span></span>

  <span data-ttu-id="ebd80-189">Nyní, uložte tuto hodnotu jako nebudete moci zobrazit znovu po zavření tohoto okna.</span><span class="sxs-lookup"><span data-stu-id="ebd80-189">Save this value now, as you won’t be able to see it again after you close this window.</span></span>

  ![Vytvořit nový klíč.](./media/application-proxy-ping-access/create-keys.png)

6. <span data-ttu-id="ebd80-191">Zavřete okno registrace aplikace nebo posuňte se úplně levé straně si vrátit do nabídky Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ebd80-191">Close the App registrations blade or scroll all the way to the left to return to the Azure Active Directory menu.</span></span>
7. <span data-ttu-id="ebd80-192">Vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-192">Select **Properties**.</span></span>
8. <span data-ttu-id="ebd80-193">Uložit **ID adresáře** identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="ebd80-193">Save the **Directory ID** GUID.</span></span>

### <a name="optional---update-graphapi-to-send-custom-fields"></a><span data-ttu-id="ebd80-194">Volitelné – aktualizace GraphAPI odeslat vlastní pole</span><span class="sxs-lookup"><span data-stu-id="ebd80-194">Optional - Update GraphAPI to send custom fields</span></span>

<span data-ttu-id="ebd80-195">Seznam tokeny zabezpečení, které odesílá Azure AD pro ověřování najdete v tématu [odkaz tokenu Azure AD](./develop/active-directory-token-and-claims.md).</span><span class="sxs-lookup"><span data-stu-id="ebd80-195">For a list of security tokens that Azure AD sends for authentication, see [Azure AD token reference](./develop/active-directory-token-and-claims.md).</span></span> <span data-ttu-id="ebd80-196">Pokud potřebujete vlastních deklarací identity, který odešle další tokeny, pomocí GraphAPI nastavte pole aplikaci *acceptMappedClaims* k **True**.</span><span class="sxs-lookup"><span data-stu-id="ebd80-196">If you need a custom claim that sends other tokens, use GraphAPI to set the app field *acceptMappedClaims* to **True**.</span></span> <span data-ttu-id="ebd80-197">Chcete-li tuto konfiguraci můžete použít Azure AD Graph Explorer nebo MS Graph.</span><span class="sxs-lookup"><span data-stu-id="ebd80-197">You can use either Azure AD Graph Explorer or MS Graph to make this configuration.</span></span> 

<span data-ttu-id="ebd80-198">Tento příklad používá Explorer grafu:</span><span class="sxs-lookup"><span data-stu-id="ebd80-198">This example uses Graph Explorer:</span></span>

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a><span data-ttu-id="ebd80-199">Stáhnout PingAccess a konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="ebd80-199">Download PingAccess and configure your app</span></span>

<span data-ttu-id="ebd80-200">Teď, když jste dokončili všechny kroky instalace služby Azure Active Directory, můžete přesunout ke konfiguraci PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-200">Now that you've completed all the Azure Active Directory setup steps, you can move on to configuring PingAccess.</span></span> 

<span data-ttu-id="ebd80-201">Podrobné kroky pro PingAccess součástí tohoto scénáře pokračovat v dokumentaci příkazu Ping Identity [PingAccess nakonfigurovat pro Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span><span class="sxs-lookup"><span data-stu-id="ebd80-201">The detailed steps for the PingAccess part of this scenario continue in the Ping Identity documentation, [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span></span>

<span data-ttu-id="ebd80-202">Tyto kroky vás provedou procesem získávání PingAccess účtu, pokud jste ještě nemáte, instalace serveru PingAccess a vytvoření připojení k Azure AD OIDC zprostředkovatele s ID adresáře, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ebd80-202">Those steps walk you through the process of getting a PingAccess account if you don't already have one, installing the PingAccess Server, and creating an Azure AD OIDC Provider connection with the Directory ID that you copied from the Azure portal.</span></span> <span data-ttu-id="ebd80-203">Potom hodnoty ID aplikace a klíč použijete k vytvoření relace webu na PingAccess.</span><span class="sxs-lookup"><span data-stu-id="ebd80-203">Then, you use the Application ID and Key values to create a Web Session on PingAccess.</span></span> <span data-ttu-id="ebd80-204">Potom můžete nastavit mapování identit a vytvořit virtuální hostitele, webu nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebd80-204">After that, you can set up identity mapping and create a virtual host, site, and application.</span></span>

### <a name="test-your-app"></a><span data-ttu-id="ebd80-205">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="ebd80-205">Test your app</span></span>

<span data-ttu-id="ebd80-206">Po dokončení těchto kroků, musí být aplikace spuštěná.</span><span class="sxs-lookup"><span data-stu-id="ebd80-206">When you've completed all these steps, your app should be up and running.</span></span> <span data-ttu-id="ebd80-207">Chcete-li otestovat ji, otevřete prohlížeč a přejděte na externí adresu URL, kterou jste vytvořili při publikování aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="ebd80-207">To test it, open a browser and navigate to the external URL that you created when you published the app in Azure.</span></span> <span data-ttu-id="ebd80-208">Přihlaste se pomocí účtu test, který jste přiřadili k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ebd80-208">Sign in with the test account that you assigned to the app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebd80-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebd80-209">Next steps</span></span>

- [<span data-ttu-id="ebd80-210">Konfigurace PingAccess pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="ebd80-210">Configure PingAccess for Azure AD</span></span>](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [<span data-ttu-id="ebd80-211">Jak Azure AD Application Proxy poskytovat jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="ebd80-211">How does Azure AD Application Proxy provide single sign-on?</span></span>](application-proxy-sso-overview.md)
- [<span data-ttu-id="ebd80-212">Řešení potíží s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="ebd80-212">Troubleshoot Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
