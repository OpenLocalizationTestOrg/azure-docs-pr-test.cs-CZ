---
title: "ověřování na základě aaaHeader s PingAccess pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Publikování aplikací pomocí Proxy aplikace a PingAccess toosupport hlavičky ověřování na základě."
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
ms.openlocfilehash: 38fe3e7a41a71f4ae6c75f014e44c722f773bd22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a><span data-ttu-id="539e9-103">Ověřování na základě záhlaví pro jednotné přihlašování s Proxy aplikace a PingAccess</span><span class="sxs-lookup"><span data-stu-id="539e9-103">Header-based authentication for single sign-on with Application Proxy and PingAccess</span></span>

<span data-ttu-id="539e9-104">Azure Proxy aplikace Active Directory a PingAccess společně společně tooprovide Azure Active Directory zákazníkům přístup tooeven další aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-104">Azure Active Directory Application Proxy and PingAccess have partnered together tooprovide Azure Active Directory customers with access tooeven more applications.</span></span> <span data-ttu-id="539e9-105">PingAccess rozšíří hello [stávající nabídky Proxy aplikace](active-directory-application-proxy-get-started.md) tooapplications tooinclude přístup přihlášení, která použít záhlaví pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="539e9-105">PingAccess expands hello [existing Application Proxy offerings](active-directory-application-proxy-get-started.md) tooinclude single sign-on access tooapplications that use headers for authentication.</span></span>

## <a name="what-is-pingaccess-for-azure-ad"></a><span data-ttu-id="539e9-106">Co je PingAccess pro Azure AD?</span><span class="sxs-lookup"><span data-stu-id="539e9-106">What is PingAccess for Azure AD?</span></span>

<span data-ttu-id="539e9-107">PingAccess pro Azure Active Directory je nabídky PingAccess, která umožňuje toogive uživatelé přístup a jeden přihlašování tooapplications, použít záhlaví pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="539e9-107">PingAccess for Azure Active Directory is an offering of PingAccess that enables you toogive users access and single sign-on tooapplications that use headers for authentication.</span></span> <span data-ttu-id="539e9-108">Proxy aplikací považuje těchto aplikací, stejně jako jakýkoli jiný pomocí Azure AD tooauthenticate přístup a následné předání přenos prostřednictvím služby konektoru hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-108">Application Proxy treats these apps like any other, using Azure AD tooauthenticate access and then passing traffic through hello connector service.</span></span> <span data-ttu-id="539e9-109">PingAccess nachází před hello aplikace a překládá hello přístupového tokenu z Azure AD do záhlaví tak, aby aplikace hello obdržel hello ověřování hello formát, který může číst.</span><span class="sxs-lookup"><span data-stu-id="539e9-109">PingAccess sits in front of hello apps and translates hello access token from Azure AD into a header so that hello application receives hello authentication in hello format it can read.</span></span>

<span data-ttu-id="539e9-110">Uživatelé nebudou Všimněte si, něco jinak při přihlášení toouse podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-110">Your users won’t notice anything different when they sign in toouse your corporate apps.</span></span> <span data-ttu-id="539e9-111">Mohou i nadále pracovat z odkudkoli na libovolném zařízení.</span><span class="sxs-lookup"><span data-stu-id="539e9-111">They can still work from anywhere on any device.</span></span> 

<span data-ttu-id="539e9-112">Vzhledem k tomu, že konektory Proxy aplikace hello přímé vzdálenou komunikaci tooall aplikace bez ohledu na jejich typ ověřování, budou pokračovat vyrovnávání tooload automaticky, stejně.</span><span class="sxs-lookup"><span data-stu-id="539e9-112">Since hello Application Proxy connectors direct remote traffic tooall apps regardless of their authentication type, they’ll continue tooload balance automatically, as well.</span></span>

## <a name="how-do-i-get-access"></a><span data-ttu-id="539e9-113">Jak získám přístup?</span><span class="sxs-lookup"><span data-stu-id="539e9-113">How do I get access?</span></span>

<span data-ttu-id="539e9-114">Vzhledem k tomu, že se tento scénář nabízí prostřednictvím partnerství mezi Azure Active Directory a PingAccess, potřebujete licence pro obě služby.</span><span class="sxs-lookup"><span data-stu-id="539e9-114">Since this scenario is offered through a partnership between Azure Active Directory and PingAccess, you need licenses for both services.</span></span> <span data-ttu-id="539e9-115">Však předplatných Azure Active Directory Premium zahrnují základní PingAccess licencí, které pokrývá až too20 aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-115">However, Azure Active Directory Premium subscriptions include a basic PingAccess license that covers up too20 applications.</span></span> <span data-ttu-id="539e9-116">Pokud potřebujete více než 20 aplikace založené na záhlaví toopublish, si můžete zakoupit další licence od PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-116">If you need toopublish more than 20 header-based applications, you can purchase an additional license from PingAccess.</span></span> 

<span data-ttu-id="539e9-117">Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).</span><span class="sxs-lookup"><span data-stu-id="539e9-117">For more information, see [Azure Active Directory editions](active-directory-editions.md).</span></span>

## <a name="publish-your-application-in-azure"></a><span data-ttu-id="539e9-118">Publikování aplikace v Azure</span><span class="sxs-lookup"><span data-stu-id="539e9-118">Publish your application in Azure</span></span>

<span data-ttu-id="539e9-119">Tento článek je určený pro uživatele, kteří jsou publikování aplikace pomocí tento scénář pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="539e9-119">This article is intended for people who are publishing an app with this scenario for hello first time.</span></span> <span data-ttu-id="539e9-120">Ji provede jak tooget pracovat s aplikací a PingAccess, kromě toohello publikování kroky.</span><span class="sxs-lookup"><span data-stu-id="539e9-120">It walks through how tooget started with both Application and PingAccess, in addition toohello publishing steps.</span></span> <span data-ttu-id="539e9-121">Pokud jste již nakonfigurovali obě služby, ale chcete aktualizačnímu programu na hello publikování kroky, můžete přeskočit hello instalace konektoru a přesunout příliš[přidat vaší aplikace tooAzure AD s Proxy aplikace](#add-your-app-to-Azure-AD-with-Application-Proxy).</span><span class="sxs-lookup"><span data-stu-id="539e9-121">If you’ve already configured both services but want a refresher on hello publishing steps, you can skip hello connector installation and move on too[Add your app tooAzure AD with Application Proxy](#add-your-app-to-Azure-AD-with-Application-Proxy).</span></span>

>[!NOTE]
><span data-ttu-id="539e9-122">Vzhledem k tomu, že tento scénář je spolupráci mezi službou Azure AD a PingAccess, některé pokyny hello nebudou na hello Ping Identity lokality.</span><span class="sxs-lookup"><span data-stu-id="539e9-122">Since this scenario is a partnership between Azure AD and PingAccess, some of hello instructions exist on hello Ping Identity site.</span></span>

### <a name="install-an-application-proxy-connector"></a><span data-ttu-id="539e9-123">Instalace konektoru Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="539e9-123">Install an Application Proxy connector</span></span>

<span data-ttu-id="539e9-124">Pokud už máte Proxy aplikace povolena a máte nainstalovaný konektor, můžete tuto část přeskočit a přesunout příliš[přidat vaší aplikace tooAzure AD s Proxy aplikace](#add-your-app-to-azure-ad-with-application-proxy).</span><span class="sxs-lookup"><span data-stu-id="539e9-124">If you already have Application Proxy enabled, and have a connector installed, you can skip this section and move on too[Add your app tooAzure AD with Application Proxy](#add-your-app-to-azure-ad-with-application-proxy).</span></span>

<span data-ttu-id="539e9-125">konektor Proxy aplikace Hello je Windows Server služba, která přesměruje přenosy hello z vaší vzdálení zaměstnanci tooyour publikované aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-125">hello Application Proxy connector is a Windows Server service that directs hello traffic from your remote employees tooyour published apps.</span></span> <span data-ttu-id="539e9-126">Podrobné pokyny k instalaci, najdete v části [povolení Proxy aplikace v portálu Azure hello](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="539e9-126">For more detailed installation instructions, see [Enable Application Proxy in hello Azure portal](active-directory-application-proxy-enable.md).</span></span>

1. <span data-ttu-id="539e9-127">Přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="539e9-127">Sign in toohello [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="539e9-128">Vyberte **Azure Active Directory** > **proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="539e9-128">Select **Azure Active Directory** > **Application proxy**.</span></span>
3. <span data-ttu-id="539e9-129">Vyberte **stáhnout konektor** stažení konektoru Proxy aplikace hello toostart.</span><span class="sxs-lookup"><span data-stu-id="539e9-129">Select **Download Connector** toostart hello Application Proxy connector download.</span></span> <span data-ttu-id="539e9-130">Postupujte podle pokynů pro instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-130">Follow hello installation instructions.</span></span>

   ![Povolení Proxy aplikace a stáhnout konektor hello](./media/application-proxy-ping-access/install-connector.png)

4. <span data-ttu-id="539e9-132">Stahování konektoru hello by měl automaticky povolení Proxy aplikace pro váš adresář, ale pokud ne, můžete si vybrat **povolení Proxy aplikace**.</span><span class="sxs-lookup"><span data-stu-id="539e9-132">Downloading hello connector should automatically enable Application Proxy for your directory, but if not you can select **Enable Application Proxy**.</span></span>


### <a name="add-your-app-tooazure-ad-with-application-proxy"></a><span data-ttu-id="539e9-133">Přidání vaší aplikace tooAzure AD s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="539e9-133">Add your app tooAzure AD with Application Proxy</span></span>

<span data-ttu-id="539e9-134">Existují dvě akce je nutné tootake v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="539e9-134">There are two actions you need tootake in hello Azure portal.</span></span> <span data-ttu-id="539e9-135">Nejprve je třeba toopublish vaší aplikace pomocí Proxy aplikací.</span><span class="sxs-lookup"><span data-stu-id="539e9-135">First, you need toopublish your application with Application Proxy.</span></span> <span data-ttu-id="539e9-136">Pak musíte toocollect některé informace o aplikaci hello, který můžete použít během hello PingAccess kroky.</span><span class="sxs-lookup"><span data-stu-id="539e9-136">Then, you need toocollect some information about hello app that you can use during hello PingAccess steps.</span></span>

<span data-ttu-id="539e9-137">Postupujte podle těchto kroků toopublish vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-137">Follow these steps toopublish your app.</span></span> <span data-ttu-id="539e9-138">Pro podrobnější návod kroky 1-8, viz [publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="539e9-138">For a more detailed walkthrough of steps 1-8, see [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md).</span></span>

1. <span data-ttu-id="539e9-139">Jestliže jste v poslední části hello, přihlaste se toohello [portál Azure](https://portal.azure.com) jako globální správce.</span><span class="sxs-lookup"><span data-stu-id="539e9-139">If you didn't in hello last section, sign in toohello [Azure portal](https://portal.azure.com) as a global administrator.</span></span>
2. <span data-ttu-id="539e9-140">Vyberte **Azure Active Directory** > **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="539e9-140">Select **Azure Active Directory** > **Enterprise applications**.</span></span>
3. <span data-ttu-id="539e9-141">Vyberte **přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-141">Select **Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="539e9-142">Vyberte **místní aplikace**.</span><span class="sxs-lookup"><span data-stu-id="539e9-142">Select **On-premises application**.</span></span>
5. <span data-ttu-id="539e9-143">Vyplňte pole hello požadované informace o nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-143">Fill out hello required fields with information about your new app.</span></span> <span data-ttu-id="539e9-144">Použijte následující pokyny pro nastavení hello hello:</span><span class="sxs-lookup"><span data-stu-id="539e9-144">Use hello following guidance for hello settings:</span></span>
   - <span data-ttu-id="539e9-145">**Interní adresa URL**: normálně je zadat adresu URL hello, kterým přejdete toohello aplikace přihlašovací stránce když jste v podnikové síti hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-145">**Internal URL**: Normally you provide hello URL that takes you toohello app’s sign in page when you’re on hello corporate network.</span></span> <span data-ttu-id="539e9-146">V tomto scénáři musí hello konektor tootreat hello PingAccess proxy jako předního stránku hello aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-146">For this scenario hello connector needs tootreat hello PingAccess proxy as hello front page of hello app.</span></span> <span data-ttu-id="539e9-147">Použijte tento formát: `https://<host name of your PA server>:<port>`.</span><span class="sxs-lookup"><span data-stu-id="539e9-147">Use this format: `https://<host name of your PA server>:<port>`.</span></span> <span data-ttu-id="539e9-148">Hello port je 3000 ve výchozím nastavení, ale můžete ji nakonfigurovat v PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-148">hello port is 3000 by default, but you can configure it in PingAccess.</span></span>
   - <span data-ttu-id="539e9-149">**Metoda předběžného ověřování**: Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="539e9-149">**Pre-authentication method**: Azure Active Directory</span></span>
   - <span data-ttu-id="539e9-150">**Převede adresu URL v hlavičkách**: Ne</span><span class="sxs-lookup"><span data-stu-id="539e9-150">**Translate URL in Headers**: No</span></span>

   >[!NOTE]
   ><span data-ttu-id="539e9-151">Pokud je vaší první aplikace, použijte port 3000 toostart a vraťte tooupdate toto nastavení je-li změnit konfiguraci svého PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-151">If this is your first application, use port 3000 toostart and come back tooupdate this setting if you change your PingAccess configuration.</span></span> <span data-ttu-id="539e9-152">Pokud je aplikace druhý nebo novější, to bude nutné toomatch hello jste nakonfigurovali v PingAccess naslouchací proces.</span><span class="sxs-lookup"><span data-stu-id="539e9-152">If this is your second or later app, this will need toomatch hello Listener you’ve configured in PingAccess.</span></span> <span data-ttu-id="539e9-153">Další informace o [naslouchací procesy v PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span><span class="sxs-lookup"><span data-stu-id="539e9-153">Learn more about [listeners in PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).</span></span>

6. <span data-ttu-id="539e9-154">Vyberte **přidat** na hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-154">Select **Add** at hello bottom of hello blade.</span></span> <span data-ttu-id="539e9-155">Přidání vaší aplikace a otevře se nabídka hello rychlý start.</span><span class="sxs-lookup"><span data-stu-id="539e9-155">Your application is added, and hello quick start menu opens.</span></span>
7. <span data-ttu-id="539e9-156">V nabídce rychlý start hello vyberte **přiřadit uživatele pro testování**a přidejte alespoň jeden uživatel toohello aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-156">In hello quick start menu, select **Assign a user for testing**, and add at least one user toohello application.</span></span> <span data-ttu-id="539e9-157">Ujistěte se, zda že má tento účet testovací přístup toohello místní aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-157">Make sure this test account has access toohello on-premises application.</span></span>
8. <span data-ttu-id="539e9-158">Vyberte **přiřadit** toosave hello testovací uživatele přiřazení.</span><span class="sxs-lookup"><span data-stu-id="539e9-158">Select **Assign** toosave hello test user assignment.</span></span>
9. <span data-ttu-id="539e9-159">V okně správy aplikace hello, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="539e9-159">On hello app management blade, select **Single sign-on**.</span></span>
10. <span data-ttu-id="539e9-160">Zvolte **na základě záhlaví přihlašování** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-160">Choose **Header-based sign-on** from hello drop-down menu.</span></span> <span data-ttu-id="539e9-161">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="539e9-161">Select **Save**.</span></span>

   >[!TIP]
   ><span data-ttu-id="539e9-162">Pokud je to poprvé pomocí na základě záhlaví jednotné přihlašování, musíte tooinstall PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-162">If this is your first time using header-based single sign-on, you need tooinstall PingAccess.</span></span> <span data-ttu-id="539e9-163">toomake opravdu vašeho předplatného Azure se automaticky přidruží PingAccess instalace, použití hello odkaz na tuto stránku přihlášení toodownload PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-163">toomake sure your Azure subscription is automatically associated with your PingAccess installation, use hello link on this single sign-on page toodownload PingAccess.</span></span> <span data-ttu-id="539e9-164">Můžete nyní otevřete stránku pro stažení hello nebo vraťte toothis stránku později.</span><span class="sxs-lookup"><span data-stu-id="539e9-164">You can open hello download site now, or come back toothis page later.</span></span> 

   ![Vyberte na základě záhlaví přihlášení](./media/application-proxy-ping-access/sso-header.PNG)

11. <span data-ttu-id="539e9-166">Zavřete okno aplikace hello Enterprise nebo přejděte všechny hello způsob toohello levém tooreturn toohello Azure Active Directory nabídky.</span><span class="sxs-lookup"><span data-stu-id="539e9-166">Close hello Enterprise applications blade or scroll all hello way toohello left tooreturn toohello Azure Active Directory menu.</span></span>
12. <span data-ttu-id="539e9-167">Vyberte **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="539e9-167">Select **App registrations**.</span></span>

   ![Vyberte aplikaci registrace](./media/application-proxy-ping-access/app-registrations.png)

13. <span data-ttu-id="539e9-169">Vyberte hello aplikace, které jste právě přidali, pak **adresy URL odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="539e9-169">Select hello app you just added, then **Reply URLs**.</span></span>

   ![Vyberte adresy URL odpovědí](./media/application-proxy-ping-access/reply-urls.png)

14. <span data-ttu-id="539e9-171">Zkontrolujte toosee, pokud hello externí adresu URL můžete přiřazené tooyour aplikaci v kroku 5, je v seznamu adresy URL odpovědí hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-171">Check toosee if hello external URL that you assigned tooyour app in step 5 is in hello Reply URLs list.</span></span> <span data-ttu-id="539e9-172">Pokud není, přidejte ji.</span><span class="sxs-lookup"><span data-stu-id="539e9-172">If it’s not, add it now.</span></span>
15. <span data-ttu-id="539e9-173">V okně Nastavení aplikace hello, vyberte **požadovaná oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="539e9-173">On hello app settings blade, select **Required permissions**.</span></span>

  ![Vyberte požadovaná oprávnění](./media/application-proxy-ping-access/required-permissions.png)

16. <span data-ttu-id="539e9-175">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="539e9-175">Select **Add**.</span></span> <span data-ttu-id="539e9-176">Hello rozhraní API, zvolte **Windows Azure Active Directory**, pak **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="539e9-176">For hello API, choose **Windows Azure Active Directory**, then **Select**.</span></span> <span data-ttu-id="539e9-177">Hello oprávnění, zvolte **pro čtení a zápis všech aplikací** a **přihlášení a čtení profilu uživatele**, pak **vyberte** a **provádí**.</span><span class="sxs-lookup"><span data-stu-id="539e9-177">For hello permissions, choose **Read and write all applications** and **Sign in and read user profile**, then **Select** and **Done**.</span></span>  

  ![Vyberte oprávnění](./media/application-proxy-ping-access/select-permissions.png)

### <a name="collect-information-for-hello-pingaccess-steps"></a><span data-ttu-id="539e9-179">Shromažďovat informace o krocích PingAccess hello</span><span class="sxs-lookup"><span data-stu-id="539e9-179">Collect information for hello PingAccess steps</span></span>

1. <span data-ttu-id="539e9-180">V okně nastavení vaší aplikace, vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="539e9-180">On your app settings blade, select **Properties**.</span></span> 

  ![Vyberte vlastnosti](./media/application-proxy-ping-access/properties.png)

2. <span data-ttu-id="539e9-182">Uložit hello **Id aplikace** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="539e9-182">Save hello **Application Id** value.</span></span> <span data-ttu-id="539e9-183">Používá se pro ID klienta hello při konfiguraci PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-183">This is used for hello client ID when you configure PingAccess.</span></span>
3. <span data-ttu-id="539e9-184">V okně Nastavení aplikace hello, vyberte **klíče**.</span><span class="sxs-lookup"><span data-stu-id="539e9-184">On hello app settings blade, select **Keys**.</span></span>

  ![Vyberte klíče](./media/application-proxy-ping-access/Keys.png)

4. <span data-ttu-id="539e9-186">Vytvořte klíč zadáním klíče popis a datum vypršení platnosti výběr z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="539e9-186">Create a key by entering a key description and choosing an expiration date from hello drop-down menu.</span></span>
5. <span data-ttu-id="539e9-187">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="539e9-187">Select **Save**.</span></span> <span data-ttu-id="539e9-188">Identifikátor GUID se zobrazí v hello **hodnotu** pole.</span><span class="sxs-lookup"><span data-stu-id="539e9-188">A GUID appears in hello **Value** field.</span></span>

  <span data-ttu-id="539e9-189">Uložit tato hodnota, nebudou moct toosee ho znovu po toto okno zavřít.</span><span class="sxs-lookup"><span data-stu-id="539e9-189">Save this value now, as you won’t be able toosee it again after you close this window.</span></span>

  ![Vytvořit nový klíč.](./media/application-proxy-ping-access/create-keys.png)

6. <span data-ttu-id="539e9-191">Zavřete okno registrace aplikace hello nebo přejděte všechny hello způsob toohello levém tooreturn toohello Azure Active Directory nabídky.</span><span class="sxs-lookup"><span data-stu-id="539e9-191">Close hello App registrations blade or scroll all hello way toohello left tooreturn toohello Azure Active Directory menu.</span></span>
7. <span data-ttu-id="539e9-192">Vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="539e9-192">Select **Properties**.</span></span>
8. <span data-ttu-id="539e9-193">Uložit hello **ID adresáře** identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="539e9-193">Save hello **Directory ID** GUID.</span></span>

### <a name="optional---update-graphapi-toosend-custom-fields"></a><span data-ttu-id="539e9-194">Volitelné – aktualizace GraphAPI toosend vlastní pole</span><span class="sxs-lookup"><span data-stu-id="539e9-194">Optional - Update GraphAPI toosend custom fields</span></span>

<span data-ttu-id="539e9-195">Seznam tokeny zabezpečení, které odesílá Azure AD pro ověřování najdete v tématu [odkaz tokenu Azure AD](./develop/active-directory-token-and-claims.md).</span><span class="sxs-lookup"><span data-stu-id="539e9-195">For a list of security tokens that Azure AD sends for authentication, see [Azure AD token reference](./develop/active-directory-token-and-claims.md).</span></span> <span data-ttu-id="539e9-196">Pokud potřebujete vlastní deklarace identity, který odesílá dalších tokenů, použijte pole aplikaci hello tooset GraphAPI *acceptMappedClaims* příliš**True**.</span><span class="sxs-lookup"><span data-stu-id="539e9-196">If you need a custom claim that sends other tokens, use GraphAPI tooset hello app field *acceptMappedClaims* too**True**.</span></span> <span data-ttu-id="539e9-197">Tuto konfiguraci můžete použít Azure AD Graph Explorer nebo MS Graph toomake.</span><span class="sxs-lookup"><span data-stu-id="539e9-197">You can use either Azure AD Graph Explorer or MS Graph toomake this configuration.</span></span> 

<span data-ttu-id="539e9-198">Tento příklad používá Explorer grafu:</span><span class="sxs-lookup"><span data-stu-id="539e9-198">This example uses Graph Explorer:</span></span>

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application> 

{
  "acceptMappedClaims":true
}
```

## <a name="download-pingaccess-and-configure-your-app"></a><span data-ttu-id="539e9-199">Stáhnout PingAccess a konfigurace aplikace</span><span class="sxs-lookup"><span data-stu-id="539e9-199">Download PingAccess and configure your app</span></span>

<span data-ttu-id="539e9-200">Teď, když jste dokončili všechny kroky instalace hello Azure Active Directory, můžete přesunout na tooconfiguring PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-200">Now that you've completed all hello Azure Active Directory setup steps, you can move on tooconfiguring PingAccess.</span></span> 

<span data-ttu-id="539e9-201">Hello podrobné kroky pro hello PingAccess součástí tohoto scénáře pokračovat v hello dokumentace ke službě Ping Identity, [PingAccess nakonfigurovat pro Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span><span class="sxs-lookup"><span data-stu-id="539e9-201">hello detailed steps for hello PingAccess part of this scenario continue in hello Ping Identity documentation, [Configure PingAccess for Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html).</span></span>

<span data-ttu-id="539e9-202">Tyto kroky vás provedou procesem hello získávání PingAccess účtu, pokud jste ještě nemáte, instalace hello PingAccess serveru a vytvoření připojení k Azure AD OIDC zprostředkovatele s hello ID adresáře, který jste zkopírovali ze hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="539e9-202">Those steps walk you through hello process of getting a PingAccess account if you don't already have one, installing hello PingAccess Server, and creating an Azure AD OIDC Provider connection with hello Directory ID that you copied from hello Azure portal.</span></span> <span data-ttu-id="539e9-203">Poté použijte hello ID aplikace a klíč hodnoty toocreate relace webu na PingAccess.</span><span class="sxs-lookup"><span data-stu-id="539e9-203">Then, you use hello Application ID and Key values toocreate a Web Session on PingAccess.</span></span> <span data-ttu-id="539e9-204">Potom můžete nastavit mapování identit a vytvořit virtuální hostitele, webu nebo aplikace.</span><span class="sxs-lookup"><span data-stu-id="539e9-204">After that, you can set up identity mapping and create a virtual host, site, and application.</span></span>

### <a name="test-your-app"></a><span data-ttu-id="539e9-205">Testování aplikace</span><span class="sxs-lookup"><span data-stu-id="539e9-205">Test your app</span></span>

<span data-ttu-id="539e9-206">Po dokončení těchto kroků, musí být aplikace spuštěná.</span><span class="sxs-lookup"><span data-stu-id="539e9-206">When you've completed all these steps, your app should be up and running.</span></span> <span data-ttu-id="539e9-207">tootest, otevřete prohlížeč a přejděte externí adresu URL toohello, kterou jste vytvořili při publikování aplikace hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="539e9-207">tootest it, open a browser and navigate toohello external URL that you created when you published hello app in Azure.</span></span> <span data-ttu-id="539e9-208">Přihlaste se pomocí účtu testovací hello přiřazené toohello aplikaci jste.</span><span class="sxs-lookup"><span data-stu-id="539e9-208">Sign in with hello test account that you assigned toohello app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="539e9-209">Další kroky</span><span class="sxs-lookup"><span data-stu-id="539e9-209">Next steps</span></span>

- [<span data-ttu-id="539e9-210">Konfigurace PingAccess pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="539e9-210">Configure PingAccess for Azure AD</span></span>](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [<span data-ttu-id="539e9-211">Jak Azure AD Application Proxy poskytovat jednotné přihlašování?</span><span class="sxs-lookup"><span data-stu-id="539e9-211">How does Azure AD Application Proxy provide single sign-on?</span></span>](application-proxy-sso-overview.md)
- [<span data-ttu-id="539e9-212">Řešení potíží s Proxy aplikace</span><span class="sxs-lookup"><span data-stu-id="539e9-212">Troubleshoot Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)
