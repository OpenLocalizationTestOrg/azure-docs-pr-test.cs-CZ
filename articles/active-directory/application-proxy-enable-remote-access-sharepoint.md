---
title: "Povolte vzdálený přístup na SharePoint s proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje základní informace o tom, jak integrovat Azure AD Application Proxy serveru SharePoint na místě."
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
ms.date: 07/21/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 97eeec3b3936bcbef6ac3966b890332901bcb153
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a><span data-ttu-id="8b83e-103">Povolte vzdálený přístup na SharePoint s proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b83e-103">Enable remote access to SharePoint with Azure AD Application Proxy</span></span>

<span data-ttu-id="8b83e-104">Tento článek popisuje postup pro integraci serveru SharePoint místní Proxy aplikace služby Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8b83e-104">This article discusses how to integrate an on-premises SharePoint server with Azure Active Directory (Azure AD) Application Proxy.</span></span>

<span data-ttu-id="8b83e-105">Postup povolení vzdáleného přístupu na SharePoint s proxy aplikace služby Azure AD, postupujte podle části v tomto článku krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="8b83e-105">To enable remote access to SharePoint with Azure AD Application Proxy, follow the sections in this article step by step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b83e-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8b83e-106">Prerequisites</span></span>

<span data-ttu-id="8b83e-107">Tento článek předpokládá, že už máte SharePoint 2013 nebo novější ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b83e-107">This article assumes that you already have SharePoint 2013 or newer in your environment.</span></span> <span data-ttu-id="8b83e-108">Kromě toho zvažte následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="8b83e-108">In addition, consider the following prerequisites:</span></span>

* <span data-ttu-id="8b83e-109">SharePoint zahrnuje nativní podpora protokolu Kerberos.</span><span class="sxs-lookup"><span data-stu-id="8b83e-109">SharePoint includes native Kerberos support.</span></span> <span data-ttu-id="8b83e-110">Uživatelé, kteří přistupují k interním lokalitám vzdáleně přes Azure AD Application Proxy tedy můžete předpokládat, tak, aby měl prostředí jednotné přihlašování (SSO).</span><span class="sxs-lookup"><span data-stu-id="8b83e-110">Therefore, users who are accessing internal sites remotely through Azure AD Application Proxy can assume to have a single sign-on (SSO) experience.</span></span>

* <span data-ttu-id="8b83e-111">Budete muset provést několik změny konfigurace serveru SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-111">You need to make a few configuration changes to your SharePoint server.</span></span> <span data-ttu-id="8b83e-112">Doporučujeme používat pracovní prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b83e-112">We recommend using a staging environment.</span></span> <span data-ttu-id="8b83e-113">Tímto způsobem můžete nejdřív aktualizace pracovní server a pak usnadnění cyklus testování před přechodem do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b83e-113">This way, you can make updates to your staging server first, and then facilitate a testing cycle before going into production.</span></span>

* <span data-ttu-id="8b83e-114">Předpokládáme, že jste již nastavili SSL pro službu SharePoint, protože jsme vyžadovat protokol SSL na publikovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8b83e-114">We assume that you have already set up SSL for SharePoint, because we require SSL on the published URL.</span></span> <span data-ttu-id="8b83e-115">Je potřeba mít SSL na interní web, povolena a zkontrolujte, zda odkazy jsou odeslána namapované správně.</span><span class="sxs-lookup"><span data-stu-id="8b83e-115">You need to have SSL enabled on your internal site, to ensure that links are sent/mapped correctly.</span></span> <span data-ttu-id="8b83e-116">Pokud jste nenakonfigurovali SSL, najdete v části [konfigurace protokolu SSL pro službu SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) pokyny.</span><span class="sxs-lookup"><span data-stu-id="8b83e-116">If you haven't configured SSL, see [Configure SSL for SharePoint 2013](https://blogs.msdn.microsoft.com/fabdulwahab/2013/01/20/configure-ssl-for-sharepoint-2013) for instructions.</span></span> <span data-ttu-id="8b83e-117">Taky se ujistěte, že počítač konektor důvěřuje certifikátu, který odešlete.</span><span class="sxs-lookup"><span data-stu-id="8b83e-117">Also, make sure that the connector machine trusts the certificate that you issue.</span></span> <span data-ttu-id="8b83e-118">(Certifikát nemusí být veřejně vydaných.)</span><span class="sxs-lookup"><span data-stu-id="8b83e-118">(The certificate does not need to be publicly issued.)</span></span>

## <a name="step-1-set-up-single-sign-on-to-sharepoint"></a><span data-ttu-id="8b83e-119">Krok 1: Nastavení jednotného přihlašování do služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="8b83e-119">Step 1: Set up single sign-on to SharePoint</span></span>

<span data-ttu-id="8b83e-120">Naše zákazníky má v tomto případě dosažení co nejlepších výsledků jednotného přihlašování pro své aplikace back-end serveru SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-120">Our customers want the best SSO experience for their back-end applications, SharePoint server in this case.</span></span> <span data-ttu-id="8b83e-121">V tomto běžném scénáři Azure AD je uživatel ověřený jenom jednou, protože uživatelé nebudou vyzváni k zadání ověření znovu.</span><span class="sxs-lookup"><span data-stu-id="8b83e-121">In this common Azure AD scenario, the user is authenticated only once, because they will not be prompted for authentication again.</span></span>

<span data-ttu-id="8b83e-122">Pro místní aplikace, které vyžadují nebo používají ověřování systému Windows můžete dosáhnout jednotného přihlašování pomocí ověřovacího protokolu Kerberos a funkci omezené delegování protokolu Kerberos (použitím KCD).</span><span class="sxs-lookup"><span data-stu-id="8b83e-122">For on-premises applications that require or use Windows authentication, you can achieve SSO by using the Kerberos authentication protocol and a feature called Kerberos constrained delegation (KCD).</span></span> <span data-ttu-id="8b83e-123">Použitím KCD, při konfiguraci, umožňuje konektor Proxy aplikace k získání lístku/tokenu systému windows pro uživatele, i když uživatel není přihlášený k systému Windows přímo.</span><span class="sxs-lookup"><span data-stu-id="8b83e-123">KCD, when configured, allows the Application Proxy connector to obtain a windows ticket/token for a user, even if the user hasn’t signed in to Windows directly.</span></span> <span data-ttu-id="8b83e-124">Další informace o použitím KCD najdete v tématu [přehled omezené delegování protokolu Kerberos](https://technet.microsoft.com/library/jj553400.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b83e-124">To learn more about KCD, see [Kerberos Constrained Delegation Overview](https://technet.microsoft.com/library/jj553400.aspx).</span></span>

<span data-ttu-id="8b83e-125">Chcete-li nastavit použitím KCD na serveru služby SharePoint, použijte postupy v následujících částech po sobě jdoucích:</span><span class="sxs-lookup"><span data-stu-id="8b83e-125">To set up KCD for a SharePoint server, use the procedures in the following sequential sections:</span></span>

### <a name="ensure-that-sharepoint-is-running-under-a-service-account"></a><span data-ttu-id="8b83e-126">Ujistěte se, že služby SharePoint běží pod účtem služby</span><span class="sxs-lookup"><span data-stu-id="8b83e-126">Ensure that SharePoint is running under a service account</span></span>

<span data-ttu-id="8b83e-127">Nejprve se ujistěte, že služby SharePoint běží pod účtem služby definované – není místní systém, místní služba nebo síťové služby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-127">First, make sure that SharePoint is running under a defined service account--not local system, local service, or network service.</span></span> <span data-ttu-id="8b83e-128">K tomu, aby hlavních názvů služby (SPN) lze připojit k platný účet.</span><span class="sxs-lookup"><span data-stu-id="8b83e-128">Do this so that you can attach service principal names (SPNs) to a valid account.</span></span> <span data-ttu-id="8b83e-129">Hlavní názvy služby SPN se, jak protokol Kerberos identifikuje různé služby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-129">SPNs are how the Kerberos protocol identifies different services.</span></span> <span data-ttu-id="8b83e-130">A budete potřebovat účet později konfigurovat použitím KCD.</span><span class="sxs-lookup"><span data-stu-id="8b83e-130">And you will need the account later to configure the KCD.</span></span>

<span data-ttu-id="8b83e-131">Aby se zajistilo, že vaše lokality běží pod účtem služby definované, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b83e-131">To ensure that your sites are running under a defined service account, perform the following steps:</span></span>

1. <span data-ttu-id="8b83e-132">Otevřete **Centrální správa služby SharePoint 2013** lokality.</span><span class="sxs-lookup"><span data-stu-id="8b83e-132">Open the **SharePoint 2013 Central Administration** site.</span></span>
2. <span data-ttu-id="8b83e-133">Přejděte na **zabezpečení** a vyberte **konfigurovat účty služby**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-133">Go to **Security** and select **Configure service accounts**.</span></span>
3. <span data-ttu-id="8b83e-134">Vyberte **fond webových aplikací – SharePoint - 80**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-134">Select **Web Application Pool - SharePoint - 80**.</span></span> <span data-ttu-id="8b83e-135">Možnosti můžou mírně lišit na základě názvu vaší webové fondu, nebo pokud fondu webové ve výchozím nastavení používá protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="8b83e-135">The options may be slightly different based on the name of your web pool, or if the web pool uses SSL by default.</span></span>

  ![Možnosti pro konfiguraci účtu služby](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-web-application.png)

4. <span data-ttu-id="8b83e-137">Pokud **vyberte účet pro tuto součást** je **místní služba** nebo **síťové služby**, musíte vytvořit účet.</span><span class="sxs-lookup"><span data-stu-id="8b83e-137">If **Select an account for this component** is **Local Service** or **Network Service**, you need to create an account.</span></span> <span data-ttu-id="8b83e-138">Pokud ne, jste hotovi a můžete přesunout k další části.</span><span class="sxs-lookup"><span data-stu-id="8b83e-138">If not, you're finished and can move to the next section.</span></span>
5. <span data-ttu-id="8b83e-139">Vyberte **zaregistrovat nový spravovaný účet**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-139">Select **Register new managed account**.</span></span> <span data-ttu-id="8b83e-140">Po vytvoření účtu, je nutné nastavit **fond aplikací webové** před použitím účtu.</span><span class="sxs-lookup"><span data-stu-id="8b83e-140">After your account is created, you must set **Web Application Pool** before you can use the account.</span></span>

> [!NOTE]
<span data-ttu-id="8b83e-141">Je potřeba mít dříve vytvořený účet služby Azure AD pro službu.</span><span class="sxs-lookup"><span data-stu-id="8b83e-141">You need to have a previously created Azure AD account for the service.</span></span> <span data-ttu-id="8b83e-142">Doporučujeme povolit změny automatické hesla.</span><span class="sxs-lookup"><span data-stu-id="8b83e-142">We suggest that you allow for an automatic password change.</span></span> <span data-ttu-id="8b83e-143">Další informace o úplnou sadu kroků a řešení problémů najdete v tématu [nakonfigurovat Automatická změna hesla v SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b83e-143">For more information about the full set of steps and troubleshooting issues, see [Configure automatic password change in SharePoint 2013](https://technet.microsoft.com/library/ff724280.aspx).</span></span>

### <a name="configure-sharepoint-for-kerberos"></a><span data-ttu-id="8b83e-144">Konfigurace služby SharePoint pro protokol Kerberos</span><span class="sxs-lookup"><span data-stu-id="8b83e-144">Configure SharePoint for Kerberos</span></span>

<span data-ttu-id="8b83e-145">Můžete použitím KCD provést jednotné přihlašování k serveru SharePoint, a to funguje pouze s protokolem Kerberos.</span><span class="sxs-lookup"><span data-stu-id="8b83e-145">You use KCD to perform single sign-on to the SharePoint server, and this works only with Kerberos.</span></span>

<span data-ttu-id="8b83e-146">Konfigurace webu služby SharePoint pro ověřování protokolu Kerberos:</span><span class="sxs-lookup"><span data-stu-id="8b83e-146">To configure your SharePoint site for Kerberos authentication:</span></span>

1. <span data-ttu-id="8b83e-147">Otevřete **Centrální správa služby SharePoint 2013** lokality.</span><span class="sxs-lookup"><span data-stu-id="8b83e-147">Open the **SharePoint 2013 Central Administration** site.</span></span>
2. <span data-ttu-id="8b83e-148">Přejděte na **Správa aplikací**, vyberte **správu webových aplikací**a vyberte web služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-148">Go to **Application Management**, select **Manage web applications**, and select your SharePoint site.</span></span> <span data-ttu-id="8b83e-149">V tomto příkladu je **SharePoint - 80**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-149">In this example, it is **SharePoint - 80**.</span></span>

  ![Vyberte web služby SharePoint](./media/application-proxy-remote-sharepoint/remote-sharepoint-manage-web-applications.png)

3. <span data-ttu-id="8b83e-151">Klikněte na tlačítko **zprostředkovatele ověřování** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="8b83e-151">Click **Authentication Providers** on the toolbar.</span></span>
4. <span data-ttu-id="8b83e-152">V **zprostředkovatele ověřování** pole, klikněte na tlačítko **výchozí pásmo** zobrazení nastavení.</span><span class="sxs-lookup"><span data-stu-id="8b83e-152">In the **Authentication Providers** box, click **Default Zone** to view the settings.</span></span>
5. <span data-ttu-id="8b83e-153">V **upravit ověřování** dialogové okno pole, posuňte se dolů, dokud neuvidíte **typy ověřování deklarací identity** a ujistěte se, že oba **povolit ověřování systému Windows** a **integrované ověřování systému Windows** jsou vybrány.</span><span class="sxs-lookup"><span data-stu-id="8b83e-153">In the **Edit Authentication** dialog box, scroll down until you see **Claims Authentication Types** and ensure that both **Enable Windows Authentication** and **Integrated Windows Authentication** are selected.</span></span>
6. <span data-ttu-id="8b83e-154">V rozevíracím seznamu, ujistěte se, že **Vyjednat (Kerberos)** je vybrána.</span><span class="sxs-lookup"><span data-stu-id="8b83e-154">In the drop-down box, make sure that **Negotiate (Kerberos)** is selected.</span></span>

  ![Dialog Upravit ověřování](./media/application-proxy-remote-sharepoint/remote-sharepoint-service-edit-authentication.png)

7. <span data-ttu-id="8b83e-156">V dolní části **upravit ověřování** dialogové okno, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-156">At the bottom of the **Edit Authentication** dialog box, click **Save**.</span></span>

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a><span data-ttu-id="8b83e-157">Nastavte hlavní název služby pro účet služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="8b83e-157">Set a service principal name for the SharePoint service account</span></span>

<span data-ttu-id="8b83e-158">Než začnete konfigurovat použitím KCD, musíte k identifikaci služby SharePoint spuštěna jako účet služby, který jste nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="8b83e-158">Before you configure the KCD, you need to identify the SharePoint service running as the service account that you've configured.</span></span> <span data-ttu-id="8b83e-159">To provedete nastavením název SPN.</span><span class="sxs-lookup"><span data-stu-id="8b83e-159">You do this by setting an SPN.</span></span> <span data-ttu-id="8b83e-160">Další informace najdete v tématu [hlavní názvy služby](https://technet.microsoft.com/library/cc961723.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b83e-160">For more information, see [Service Principal Names](https://technet.microsoft.com/library/cc961723.aspx).</span></span>

<span data-ttu-id="8b83e-161">Není ve formátu hlavního názvu služby:</span><span class="sxs-lookup"><span data-stu-id="8b83e-161">The SPN format is:</span></span>

```
<service class>/<host>:<port>
```

<span data-ttu-id="8b83e-162">Ve formátu hlavního názvu služby:</span><span class="sxs-lookup"><span data-stu-id="8b83e-162">In the SPN format:</span></span>

* <span data-ttu-id="8b83e-163">_Třída služby_ je jedinečný název pro službu.</span><span class="sxs-lookup"><span data-stu-id="8b83e-163">_service class_ is a unique name for the service.</span></span> <span data-ttu-id="8b83e-164">Pro službu SharePoint, můžete použít **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-164">For SharePoint, you use **HTTP**.</span></span>

* <span data-ttu-id="8b83e-165">_hostitele_ je plně kvalifikované domény nebo název pro rozhraní NetBIOS hostitele, který je služba spuštěná na.</span><span class="sxs-lookup"><span data-stu-id="8b83e-165">_host_ is the fully qualified domain or NetBIOS name of the host that the service is running on.</span></span> <span data-ttu-id="8b83e-166">Pro web služby SharePoint tento text může být nutné adresu URL webu, v závislosti na verzi služby IIS, který používáte.</span><span class="sxs-lookup"><span data-stu-id="8b83e-166">For a SharePoint site, this text might need to be the URL of the site, depending on the version of IIS that you're using.</span></span>

* <span data-ttu-id="8b83e-167">_port_ je volitelný.</span><span class="sxs-lookup"><span data-stu-id="8b83e-167">_port_ is optional.</span></span>

<span data-ttu-id="8b83e-168">Pokud je plně kvalifikovaný název domény serveru SharePoint:</span><span class="sxs-lookup"><span data-stu-id="8b83e-168">If the FQDN of the SharePoint server is:</span></span>

```
sharepoint.demo.o365identity.us
```

<span data-ttu-id="8b83e-169">Pak je hlavní název služby:</span><span class="sxs-lookup"><span data-stu-id="8b83e-169">Then the SPN is:</span></span>

```
HTTP/ sharepoint.demo.o365identity.us demo
```

<span data-ttu-id="8b83e-170">Může také musíte nastavit SPN pro určité weby na vašem serveru.</span><span class="sxs-lookup"><span data-stu-id="8b83e-170">You might also need to set SPNs for specific sites on your server.</span></span> <span data-ttu-id="8b83e-171">Další informace najdete v tématu [ověřování protokolem Kerberos konfigurace](https://technet.microsoft.com/library/cc263449(v=office.12).aspx).</span><span class="sxs-lookup"><span data-stu-id="8b83e-171">For more information, see [Configure Kerberos authentication](https://technet.microsoft.com/library/cc263449(v=office.12).aspx).</span></span> <span data-ttu-id="8b83e-172">Zaměřit se na část "Vytvořit hlavní názvy služby pro vaší webové aplikace s použitím ověřování protokolu Kerberos."</span><span class="sxs-lookup"><span data-stu-id="8b83e-172">Pay close attention to the section "Create Service Principal Names for your Web applications using Kerberos authentication."</span></span>

<span data-ttu-id="8b83e-173">Nejjednodušším způsobem, jak vám umožní nastavit SPN je postupujte podle formáty hlavního názvu služby, které již mohou být k dispozici pro vaše lokality.</span><span class="sxs-lookup"><span data-stu-id="8b83e-173">The easiest way for you to set SPNs is to follow the SPN formats that may already be present for your sites.</span></span> <span data-ttu-id="8b83e-174">Zkopírujte tyto hlavní názvy služby SPN zaregistrovat u účtu služby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-174">Copy those SPNs to register against the service account.</span></span> <span data-ttu-id="8b83e-175">Použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="8b83e-175">To do this:</span></span>

1. <span data-ttu-id="8b83e-176">Přejděte na web s hlavní název služby z jiného počítače.</span><span class="sxs-lookup"><span data-stu-id="8b83e-176">Browse to the site with the SPN from another machine.</span></span>
 <span data-ttu-id="8b83e-177">Když to uděláte, relevantní sadu lístky protokolu Kerberos se uloží do mezipaměti na počítači.</span><span class="sxs-lookup"><span data-stu-id="8b83e-177">When you do, the relevant set of Kerberos tickets is cached on the machine.</span></span> <span data-ttu-id="8b83e-178">Tyto lístky obsahují SPN je cílový webový server, který jste vyhledali.</span><span class="sxs-lookup"><span data-stu-id="8b83e-178">These tickets contain the SPN of the target site that you browsed to.</span></span>

2. <span data-ttu-id="8b83e-179">Může vyžádat hlavní název služby pro danou lokalitu pomocí nástroje názvem [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html).</span><span class="sxs-lookup"><span data-stu-id="8b83e-179">You can pull the SPN for that site by using a tool called [Klist](http://web.mit.edu/kerberos/krb5-devel/doc/user/user_commands/klist.html).</span></span> <span data-ttu-id="8b83e-180">V příkazovém okně, které běží ve stejné oblasti jako uživatel, který získat přístup k webu v prohlížeči spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8b83e-180">In a command window that's running in the same context as the user who accessed the site in the browser, run the following command:</span></span>
```
Klist
```
<span data-ttu-id="8b83e-181">Klist pak vrátí sadu cíl SPN.</span><span class="sxs-lookup"><span data-stu-id="8b83e-181">Klist then returns the set of target SPNs.</span></span> <span data-ttu-id="8b83e-182">V tomto příkladu je zvýrazněná hodnota hlavní název služby, který je potřeba:</span><span class="sxs-lookup"><span data-stu-id="8b83e-182">In this example, the highlighted value is the SPN that's needed:</span></span>

  ![Příklad Klist výsledky](./media/application-proxy-remote-sharepoint/remote-sharepoint-target-service.png)

4. <span data-ttu-id="8b83e-184">Teď, když máte hlavní název služby, budete muset Ujistěte se, zda je správně nakonfigurovaná na účet služby, které jste nastavili pro webovou aplikaci dříve.</span><span class="sxs-lookup"><span data-stu-id="8b83e-184">Now that you have the SPN, you need to make sure that it's configured correctly on the service account that you set up for the web application earlier.</span></span> <span data-ttu-id="8b83e-185">Spusťte následující příkaz z příkazového řádku jako správce domény:</span><span class="sxs-lookup"><span data-stu-id="8b83e-185">Run the following command from the command prompt as an administrator of the domain:</span></span>

 ```
 setspn -S http/sharepoint.demo.o365identity.us demo\sp_svc
 ```

 <span data-ttu-id="8b83e-186">Tento příkaz nastaví název SPN pro účet služby SharePoint spuštěna jako _demo\sp_svc_.</span><span class="sxs-lookup"><span data-stu-id="8b83e-186">This command sets the SPN for the SharePoint service account running as _demo\sp_svc_.</span></span>

 <span data-ttu-id="8b83e-187">Nahraďte _http/sharepoint.demo.o365identity.us_ s hlavní název serveru a _demo\sp_svc_ pomocí účtu služby ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="8b83e-187">Replace _http/sharepoint.demo.o365identity.us_ with the SPN for your server and _demo\sp_svc_ with the service account in your environment.</span></span> <span data-ttu-id="8b83e-188">Příkaz Setspn hledá SPN před přidáním ho.</span><span class="sxs-lookup"><span data-stu-id="8b83e-188">The Setspn command searches for the SPN before it adds it.</span></span> <span data-ttu-id="8b83e-189">V takovém případě může dojít **duplicitní hodnota SPN** chyby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-189">In this case, you might see a **Duplicate SPN Value** error.</span></span> <span data-ttu-id="8b83e-190">Pokud se zobrazí tato chyba, ujistěte se, že hodnota je přidružen k účtu služby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-190">If you see this error, make sure that the value is associated with the service account.</span></span>

<span data-ttu-id="8b83e-191">Můžete ověřit, že byl přidán SPN spuštěním příkazu Setspn -l možnost.</span><span class="sxs-lookup"><span data-stu-id="8b83e-191">You can verify that the SPN was added by running the Setspn command with the -l option.</span></span> <span data-ttu-id="8b83e-192">Další informace o tomto příkazu najdete v tématu [Setspn](https://technet.microsoft.com/library/cc731241.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b83e-192">To learn more about this command, see [Setspn](https://technet.microsoft.com/library/cc731241.aspx).</span></span>

### <a name="ensure-that-the-connector-is-set-as-a-trusted-delegate-to-sharepoint"></a><span data-ttu-id="8b83e-193">Zkontrolujte, že konektor je nastavená jako důvěryhodné delegáta do služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="8b83e-193">Ensure that the connector is set as a trusted delegate to SharePoint</span></span>

<span data-ttu-id="8b83e-194">Nakonfigurujte použitím KCD tak, aby služba Azure AD Application Proxy můžete delegovat identit uživatelů ve službě SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-194">Configure the KCD so that the Azure AD Application Proxy service can delegate user identities to the SharePoint service.</span></span> <span data-ttu-id="8b83e-195">To provedete povolením konektoru Proxy aplikace pro načtení lístky protokolu Kerberos pro uživatele, kteří byli ověřeni ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b83e-195">You do this by enabling the Application Proxy connector to retrieve Kerberos tickets for your users who have been authenticated in Azure AD.</span></span> <span data-ttu-id="8b83e-196">Potom tento server v takovém případě předá kontextu cílová aplikace nebo služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-196">Then that server passes the context to the target application, or SharePoint in this case.</span></span>

<span data-ttu-id="8b83e-197">Pokud chcete nakonfigurovat použitím KCD, opakujte pro každý počítač konektor následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8b83e-197">To configure the KCD, repeat the following steps for each connector machine:</span></span>

1. <span data-ttu-id="8b83e-198">Přihlaste se jako správce domény na řadič domény a pak otevřete **Active Directory Users and Computers**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-198">Log in as a domain administrator to a DC, and then open **Active Directory Users and Computers**.</span></span>
2. <span data-ttu-id="8b83e-199">Hledání počítače, který konektor běží na.</span><span class="sxs-lookup"><span data-stu-id="8b83e-199">Find the computer that the connector is running on.</span></span> <span data-ttu-id="8b83e-200">V tomto příkladu je stejný server služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-200">In this example, it's the same SharePoint server.</span></span>
3. <span data-ttu-id="8b83e-201">Klikněte dvakrát na počítač a klikněte **delegování** kartě.</span><span class="sxs-lookup"><span data-stu-id="8b83e-201">Double-click the computer, and then click the **Delegation** tab.</span></span>
4. <span data-ttu-id="8b83e-202">Ujistěte se, jestli jsou nastavení delegování nastavené **důvěřovat tomuto počítači pro delegování pouze určeným službám**a potom vyberte **používající libovolný protokol pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-202">Ensure that the delegation settings are set to **Trust this computer for delegation to the specified services only**, and then select **Use any authentication protocol**.</span></span>

  ![Nastavení delegování](./media/application-proxy-remote-sharepoint/remote-sharepoint-delegation-box.png)

5. <span data-ttu-id="8b83e-204">Klikněte **přidat** tlačítko, klikněte na tlačítko **uživatele nebo počítače**a vyhledejte účet služby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-204">Click the **Add** button, click **Users or Computers**, and locate the service account.</span></span>

  ![Přidání názvu SPN pro účet služby](./media/application-proxy-remote-sharepoint/remote-sharepoint-users-computers.png)

6. <span data-ttu-id="8b83e-206">V seznamu SPN vyberte ten, který jste dříve vytvořili pro účet služby.</span><span class="sxs-lookup"><span data-stu-id="8b83e-206">In the list of SPNs, select the one that you created earlier for the service account.</span></span>
7. <span data-ttu-id="8b83e-207">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-207">Click **OK**.</span></span> <span data-ttu-id="8b83e-208">Klikněte na tlačítko **OK** znovu a uložte změny.</span><span class="sxs-lookup"><span data-stu-id="8b83e-208">Click **OK** again to save the changes.</span></span>

## <a name="step-2-enable-remote-access-to-sharepoint"></a><span data-ttu-id="8b83e-209">Krok 2: Povolení vzdáleného přístupu do služby SharePoint</span><span class="sxs-lookup"><span data-stu-id="8b83e-209">Step 2: Enable remote access to SharePoint</span></span>

<span data-ttu-id="8b83e-210">Teď, když jste povolili SharePoint pro protokolu Kerberos a použitím nakonfigurované KCD, jste připravení nastavit jednotné přihlašování do služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-210">Now that you’ve enabled SharePoint for Kerberos and configured KCD, you're ready to set up single sign-on to SharePoint.</span></span> <span data-ttu-id="8b83e-211">Potom z konektoru, můžete publikovat farmy služby SharePoint pro vzdálený přístup prostřednictvím proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b83e-211">Then from the connector, you can publish the SharePoint farm for remote access through Azure AD Application Proxy.</span></span>

<span data-ttu-id="8b83e-212">Pokud chcete provést následující kroky, musíte být členem Role Globální správce v účtu Azure Active Directory vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="8b83e-212">To perform the following steps, you need to be a member of the Global Administrator Role in your organization's Azure Active Directory account.</span></span>

1. <span data-ttu-id="8b83e-213">Přihlaste se k [portál Azure](https://manage.windowsazure.com) a najít klientovi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b83e-213">Sign in to the [Azure portal](https://manage.windowsazure.com) and find your Azure AD tenant.</span></span>
2. <span data-ttu-id="8b83e-214">Klikněte na tlačítko **aplikace**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-214">Click **Applications**, and then click **Add**.</span></span>
3. <span data-ttu-id="8b83e-215">Vyberte **Publikování aplikace, která bude přístupná mimo vaši síť**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-215">Select **Publish an application that will be accessible from outside your network**.</span></span> <span data-ttu-id="8b83e-216">Pokud nevidíte tuto možnost, ujistěte se, že máte Azure AD Basic nebo Premium nastavit v klientovi.</span><span class="sxs-lookup"><span data-stu-id="8b83e-216">If you don’t see this option, make sure that you have Azure AD Basic or Premium set up in the tenant.</span></span>
4. <span data-ttu-id="8b83e-217">Dokončení jednotlivých možností následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8b83e-217">Complete each of the options as follows:</span></span>
 * <span data-ttu-id="8b83e-218">**Název**: použijte jakoukoli hodnotu, která chcete – například **SharePoint**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-218">**Name**: Use any value that you want--for example, **SharePoint**.</span></span>
 * <span data-ttu-id="8b83e-219">**Interní adresa URL**: Toto je adresa URL webu služby SharePoint interně jako **https://SharePoint/**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-219">**Internal URL**: This is the URL of the SharePoint site internally, such as **https://SharePoint/**.</span></span> <span data-ttu-id="8b83e-220">V tomto příkladu, nezapomeňte použít **https**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-220">In this example, make sure to use **https**.</span></span>
 * <span data-ttu-id="8b83e-221">**Metoda předběžného ověření**: vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-221">**Preauthentication Method**: Select **Azure Active Directory**.</span></span>

  ![Možnosti pro přidání aplikace](./media/application-proxy-remote-sharepoint/remote-sharepoint-add-application.png)

5. <span data-ttu-id="8b83e-223">Po publikování aplikace, klikněte **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="8b83e-223">After the app is published, click the **Configure** tab.</span></span>
6. <span data-ttu-id="8b83e-224">Posuňte se dolů možnost **překládat adresy URL v hlavičkách**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-224">Scroll down to the option **Translate URL in Headers**.</span></span> <span data-ttu-id="8b83e-225">Výchozí hodnota je **Ano**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-225">The default value is **YES**.</span></span> <span data-ttu-id="8b83e-226">Změňte ho na **ne**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-226">Change it to **NO**.</span></span>

 <span data-ttu-id="8b83e-227">Používá služby SharePoint _Hlavička hostitele_ hodnotu k vyhledání webu.</span><span class="sxs-lookup"><span data-stu-id="8b83e-227">SharePoint uses the _Host Header_ value to look up the site.</span></span> <span data-ttu-id="8b83e-228">Generuje také odkazy na základě této hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8b83e-228">It also generates links based on this value.</span></span> <span data-ttu-id="8b83e-229">Projeví je zajistit, že odkazy, které generuje SharePoint je publikovaná URL, která je správně nastavený na použití externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8b83e-229">The net effect is to make sure that any link that SharePoint generates is a published URL that is correctly set to use the external URL.</span></span> <span data-ttu-id="8b83e-230">Nastavení na hodnotu **Ano** taky umožňuje konektor předávat požadavek na back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b83e-230">Setting the value to **YES** also enables the connector to forward the request to the back-end application.</span></span> <span data-ttu-id="8b83e-231">Nicméně, nastavte tuto hodnotu na **ne** znamená, že konektor nebude odesílají název interní hostitele.</span><span class="sxs-lookup"><span data-stu-id="8b83e-231">However, setting the value to **NO** means that the connector will not send the internal host name.</span></span> <span data-ttu-id="8b83e-232">Místo toho konektor odešle hlavičku hostitele jako adresu URL publikované na back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="8b83e-232">Instead, the connector sends the host header as the published URL to the back-end application.</span></span>

 <span data-ttu-id="8b83e-233">Také aby se zajistilo, že služby SharePoint je možné zadat tuto adresu URL, musíte dokončit jeden další konfigurace na serveru SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-233">Also, to ensure that SharePoint accepts this URL, you need to complete one more configuration on the SharePoint server.</span></span> <span data-ttu-id="8b83e-234">Můžete to udělat v další části.</span><span class="sxs-lookup"><span data-stu-id="8b83e-234">You'll do that in the next section.</span></span>

7. <span data-ttu-id="8b83e-235">Změna **metodu interního ověřování** k **integrované ověřování systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-235">Change **Internal Authentication Method** to **Integrated Windows Authentication**.</span></span> <span data-ttu-id="8b83e-236">Pokud se klientovi služby Azure AD používá název UPN v cloudu, které se liší od hlavní název uživatele v místě, nezapomeňte aktualizovat **delegované Identity přihlášení** také.</span><span class="sxs-lookup"><span data-stu-id="8b83e-236">If your Azure AD tenant uses a UPN in the cloud that's different from the UPN on-premises, remember to update **Delegated Login Identity** as well.</span></span>
8. <span data-ttu-id="8b83e-237">Nastavit **interní aplikace SPN** na hodnotu, která jste nastavili dříve.</span><span class="sxs-lookup"><span data-stu-id="8b83e-237">Set **Internal Application SPN** to the value that you set earlier.</span></span> <span data-ttu-id="8b83e-238">Například použít **http/sharepoint.demo.o365identity.us**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-238">For example, use **http/sharepoint.demo.o365identity.us**.</span></span>
9. <span data-ttu-id="8b83e-239">Přiřazení aplikace k cíloví uživatelé.</span><span class="sxs-lookup"><span data-stu-id="8b83e-239">Assign the application to your target users.</span></span>

<span data-ttu-id="8b83e-240">Aplikace by měl vypadat podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="8b83e-240">Your application should look similar to the following example:</span></span>

  ![Dokončení aplikace](./media/application-proxy-remote-sharepoint/remote-sharepoint-internal-application-spn.png)

## <a name="step-3-ensure-that-sharepoint-knows-about-the-external-url"></a><span data-ttu-id="8b83e-242">Krok 3: Zajistěte, aby SharePoint ví o externí adresu URL</span><span class="sxs-lookup"><span data-stu-id="8b83e-242">Step 3: Ensure that SharePoint knows about the external URL</span></span>

<span data-ttu-id="8b83e-243">V posledním kroku zajistit, že služby SharePoint můžete najít serveru v závislosti na externí adresu URL, tak, aby se budou vykreslovat odkazy podle této externí adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8b83e-243">Your last step to ensure that SharePoint can find the site based on the external URL, so that it will render links based on that external URL.</span></span> <span data-ttu-id="8b83e-244">To uděláte tak, že nakonfigurujete mapování alternativních adres URL pro web služby SharePoint.</span><span class="sxs-lookup"><span data-stu-id="8b83e-244">You do this by configuring alternate access mappings for the SharePoint site.</span></span>

1. <span data-ttu-id="8b83e-245">Otevřete **Centrální správa služby SharePoint 2013** lokality.</span><span class="sxs-lookup"><span data-stu-id="8b83e-245">Open the **SharePoint 2013 Central Administration** site.</span></span>
2. <span data-ttu-id="8b83e-246">V části **nastavení systému**, vyberte **konfigurace mapování alternativních adres**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-246">Under **System Settings**, select **Configure Alternate Access Mappings**.</span></span>

 <span data-ttu-id="8b83e-247">Tím se otevře **mapování alternativních adres** pole.</span><span class="sxs-lookup"><span data-stu-id="8b83e-247">This opens the **Alternate Access Mappings** box.</span></span>

  ![Alternativní pole mapování adres URL](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access1.png)

3. <span data-ttu-id="8b83e-249">V rozevíracím seznamu vedle položky **kolekce mapování alternativních adres**, vyberte **změnit alternativní přístup mapování kolekci**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-249">In the drop-down list beside **Alternate Access Mapping Collection**, select **Change Alternate Access Mapping Collection**.</span></span>
4. <span data-ttu-id="8b83e-250">Vyberte svou lokalitu – například **SharePoint - 80**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-250">Select your site--for example, **SharePoint - 80**.</span></span>

  ![Výběr lokality](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access2.png)

5. <span data-ttu-id="8b83e-252">Můžete přidat adresu URL publikované jako interní adresa URL nebo veřejnou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8b83e-252">You can choose to add the published URL as either an internal URL or a public URL.</span></span> <span data-ttu-id="8b83e-253">Tento příklad používá veřejnou adresu URL jako extranetu.</span><span class="sxs-lookup"><span data-stu-id="8b83e-253">This example uses a public URL as the extranet.</span></span>
6. <span data-ttu-id="8b83e-254">Klikněte na tlačítko **Upravit veřejné adresy URL** v **Extranet** cestu a pak zadejte cestu pro publikovanou aplikaci, jako v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="8b83e-254">Click **Edit Public URLs** in the **Extranet** path, and then enter the path for the published application, as in the previous step.</span></span> <span data-ttu-id="8b83e-255">Zadejte například **https://sharepoint-iddemo.msappproxy.net**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-255">For example, enter **https://sharepoint-iddemo.msappproxy.net**.</span></span>

  ![Vstupující do cesty](./media/application-proxy-remote-sharepoint/remote-sharepoint-alternate-access3.png)

7. <span data-ttu-id="8b83e-257">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8b83e-257">Click **Save**.</span></span>

 <span data-ttu-id="8b83e-258">Nyní můžete k webu služby SharePoint externě prostřednictvím proxy aplikace služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b83e-258">You can now access the SharePoint site externally via Azure AD Application Proxy.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b83e-259">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b83e-259">Next steps</span></span>

- [<span data-ttu-id="8b83e-260">Jak poskytnout zabezpečený vzdálený přístup k místním aplikacím</span><span class="sxs-lookup"><span data-stu-id="8b83e-260">How to provide secure remote access to on-premises applications</span></span>](active-directory-application-proxy-get-started.md)
- [<span data-ttu-id="8b83e-261">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b83e-261">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
- [<span data-ttu-id="8b83e-262">Publikování serveru SharePoint 2016 a Office Online serveru s proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b83e-262">Publishing SharePoint 2016 and Office Online Server with Azure AD Application Proxy</span></span>](https://blogs.technet.microsoft.com/dawiese/2016/06/09/publishing-sharepoint-2016-and-office-online-server-with-azure-ad-application-proxy/)
