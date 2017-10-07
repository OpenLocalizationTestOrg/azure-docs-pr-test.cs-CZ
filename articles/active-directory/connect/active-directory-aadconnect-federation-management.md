---
title: "aaaActive Directory Federation Services management a přizpůsobení službou Azure AD Connect | Microsoft Docs"
description: "Správa služby AD FS s Azure AD Connect a přizpůsobení uživatele AD FS přihlašování uživatelů s Azure AD Connect a prostředí PowerShell."
keywords: "Služba AD FS, služba AD FS, služby AD FS správy, AAD Connect, připojení, přihlášení, služby AD FS přizpůsobení, opravte federačního vztahu důvěryhodnosti, O365, předávající strany"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 361a2bfd6d7a6993dbe773d6ea039ad1afc6346a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a><span data-ttu-id="25ae3-104">Spravovat a přizpůsobit Active Directory Federation Services přes Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="25ae3-104">Manage and customize Active Directory Federation Services by using Azure AD Connect</span></span>
<span data-ttu-id="25ae3-105">Tento článek popisuje, jak toomanage a přizpůsobit Active Directory Federation Services (AD FS) pomocí služby Azure Active Directory (Azure AD) připojit.</span><span class="sxs-lookup"><span data-stu-id="25ae3-105">This article describes how toomanage and customize Active Directory Federation Services (AD FS) by using Azure Active Directory (Azure AD) Connect.</span></span> <span data-ttu-id="25ae3-106">Zahrnuje také dalších běžných úkolů služby AD FS, které budete potřebovat toodo pro celou konfiguraci farmy služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ae3-106">It also includes other common AD FS tasks that you might need toodo for a complete configuration of an AD FS farm.</span></span>

| <span data-ttu-id="25ae3-107">Téma</span><span class="sxs-lookup"><span data-stu-id="25ae3-107">Topic</span></span> | <span data-ttu-id="25ae3-108">Co pokrývá</span><span class="sxs-lookup"><span data-stu-id="25ae3-108">What it covers</span></span> |
|:--- |:--- |
| <span data-ttu-id="25ae3-109">**Správa služby AD FS**</span><span class="sxs-lookup"><span data-stu-id="25ae3-109">**Manage AD FS**</span></span> | |
| [<span data-ttu-id="25ae3-110">Oprava hello důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="25ae3-110">Repair hello trust</span></span>](#repairthetrust) |<span data-ttu-id="25ae3-111">Jak toorepair hello federační vztah důvěryhodnosti s Office 365.</span><span class="sxs-lookup"><span data-stu-id="25ae3-111">How toorepair hello federation trust with Office 365.</span></span> |
| [<span data-ttu-id="25ae3-112">Vytvořit federaci s Azure AD pomocí alternativního přihlašovacího ID</span><span class="sxs-lookup"><span data-stu-id="25ae3-112">Federate with Azure AD using alternate login ID </span></span>](#alternateid) | <span data-ttu-id="25ae3-113">Konfigurace federace pomocí alternativního přihlašovacího ID</span><span class="sxs-lookup"><span data-stu-id="25ae3-113">Configure federation using alternate login ID</span></span>  |
| [<span data-ttu-id="25ae3-114">Přidejte server služby AD FS</span><span class="sxs-lookup"><span data-stu-id="25ae3-114">Add an AD FS server</span></span>](#addadfsserver) |<span data-ttu-id="25ae3-115">Jak tooexpand služby AD FS, farmy s dalším serverem služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ae3-115">How tooexpand an AD FS farm with an additional AD FS server.</span></span> |
| [<span data-ttu-id="25ae3-116">Přidat server proxy webové aplikace služby AD FS</span><span class="sxs-lookup"><span data-stu-id="25ae3-116">Add an AD FS Web Application Proxy server</span></span>](#addwapserver) |<span data-ttu-id="25ae3-117">Jak tooexpand služby AD FS, farmy s dalším serverem Proxy webových aplikací (WAP).</span><span class="sxs-lookup"><span data-stu-id="25ae3-117">How tooexpand an AD FS farm with an additional Web Application Proxy (WAP) server.</span></span> |
| [<span data-ttu-id="25ae3-118">Přidání federované domény</span><span class="sxs-lookup"><span data-stu-id="25ae3-118">Add a federated domain</span></span>](#addfeddomain) |<span data-ttu-id="25ae3-119">Jak tooadd federovanou doménu.</span><span class="sxs-lookup"><span data-stu-id="25ae3-119">How tooadd a federated domain.</span></span> |
| [<span data-ttu-id="25ae3-120">Aktualizovat certifikát SSL hello</span><span class="sxs-lookup"><span data-stu-id="25ae3-120">Update hello SSL certificate</span></span>](active-directory-aadconnectfed-ssl-update.md)| <span data-ttu-id="25ae3-121">Jak tooupdate hello SSL certificate pro farmu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ae3-121">How tooupdate hello SSL certificate for an AD FS farm.</span></span> |
| <span data-ttu-id="25ae3-122">**Přizpůsobení služby AD FS**</span><span class="sxs-lookup"><span data-stu-id="25ae3-122">**Customize AD FS**</span></span> | |
| [<span data-ttu-id="25ae3-123">Přidat vlastní logo nebo obrázku</span><span class="sxs-lookup"><span data-stu-id="25ae3-123">Add a custom company logo or illustration</span></span>](#customlogo) |<span data-ttu-id="25ae3-124">Jak přihlášení toocustomize služby AD FS stránky s firemní logo a ilustrace.</span><span class="sxs-lookup"><span data-stu-id="25ae3-124">How toocustomize an AD FS sign-in page with a company logo and illustration.</span></span> |
| [<span data-ttu-id="25ae3-125">Přidejte popis přihlášení</span><span class="sxs-lookup"><span data-stu-id="25ae3-125">Add a sign-in description</span></span>](#addsignindescription) |<span data-ttu-id="25ae3-126">Jak tooadd přihlášení stránce popis.</span><span class="sxs-lookup"><span data-stu-id="25ae3-126">How tooadd a sign-in page description.</span></span> |
| [<span data-ttu-id="25ae3-127">Upravit pravidla deklarace identity služby AD FS</span><span class="sxs-lookup"><span data-stu-id="25ae3-127">Modify AD FS claim rules</span></span>](#modclaims) |<span data-ttu-id="25ae3-128">Jak toomodify služby AD FS deklarací pro různé scénáře federace.</span><span class="sxs-lookup"><span data-stu-id="25ae3-128">How toomodify AD FS claims for various federation scenarios.</span></span> |

## <a name="manage-ad-fs"></a><span data-ttu-id="25ae3-129">Správa služby AD FS</span><span class="sxs-lookup"><span data-stu-id="25ae3-129">Manage AD FS</span></span>
<span data-ttu-id="25ae3-130">Pomocí Průvodce hello Azure AD Connect můžete provádět různé AD FS související úlohy v Azure AD Connect s zásah uživatele.</span><span class="sxs-lookup"><span data-stu-id="25ae3-130">You can perform various AD FS-related tasks in Azure AD Connect with minimal user intervention by using hello Azure AD Connect wizard.</span></span> <span data-ttu-id="25ae3-131">Po dokončení instalace služby Azure AD Connect průvodcem hello spuštěné hello průvodce můžete spustit znovu tooperform další úkoly.</span><span class="sxs-lookup"><span data-stu-id="25ae3-131">After you've finished installing Azure AD Connect by running hello wizard, you can run hello wizard again tooperform additional tasks.</span></span>

## <span data-ttu-id="25ae3-132">Oprava hello důvěryhodnosti<a name=repairthetrust></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-132">Repair hello trust <a name=repairthetrust></a></span></span>
<span data-ttu-id="25ae3-133">Můžete použít Azure AD Connect toocheck hello aktuální stav hello služby AD FS a vztah důvěryhodnosti služby Azure AD a proveďte příslušné akce toorepair hello důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="25ae3-133">You can use Azure AD Connect toocheck hello current health of hello AD FS and Azure AD trust and take appropriate actions toorepair hello trust.</span></span> <span data-ttu-id="25ae3-134">Postupujte podle těchto kroků toorepair vaše Azure AD a služby AD FS důvěřují.</span><span class="sxs-lookup"><span data-stu-id="25ae3-134">Follow these steps toorepair your Azure AD and AD FS trust.</span></span>

1. <span data-ttu-id="25ae3-135">Vyberte **AAD opravy a služby AD FS důvěřují** hello seznamu další úkoly.</span><span class="sxs-lookup"><span data-stu-id="25ae3-135">Select **Repair AAD and ADFS Trust** from hello list of additional tasks.</span></span>
   <span data-ttu-id="25ae3-136">![Opravit AAD a AD FS vztah důvěryhodnosti](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span><span class="sxs-lookup"><span data-stu-id="25ae3-136">![Repair AAD and ADFS Trust](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span></span>

2. <span data-ttu-id="25ae3-137">Na hello **připojit tooAzure AD** stránky, zadejte svoje přihlašovací údaje globálního správce pro Azure AD a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-137">On hello **Connect tooAzure AD** page, provide your global administrator credentials for Azure AD, and click **Next**.</span></span>
   <span data-ttu-id="25ae3-138">![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span><span class="sxs-lookup"><span data-stu-id="25ae3-138">![Connect tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span></span>

3. <span data-ttu-id="25ae3-139">Na hello **přihlašovací údaje vzdálený přístup** zadejte hello přihlašovací údaje pro správce domény hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-139">On hello **Remote access credentials** page, enter hello credentials for hello domain administrator.</span></span>

   ![Přihlašovací údaje vzdálený přístup](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    <span data-ttu-id="25ae3-141">Po kliknutí na tlačítko **Další**, Azure AD Connect kontroluje stav certifikátu a zobrazuje všechny problémy.</span><span class="sxs-lookup"><span data-stu-id="25ae3-141">After you click **Next**, Azure AD Connect checks for certificate health and shows any issues.</span></span>

    ![Stav certifikátů](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    <span data-ttu-id="25ae3-143">Hello **připraven tooconfigure** stránka zobrazuje hello seznam akcí, které budou provést toorepair hello důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="25ae3-143">hello **Ready tooconfigure** page shows hello list of actions that will be performed toorepair hello trust.</span></span>

    ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. <span data-ttu-id="25ae3-145">Klikněte na tlačítko **nainstalovat** toorepair hello důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="25ae3-145">Click **Install** toorepair hello trust.</span></span>

> [!NOTE]
> <span data-ttu-id="25ae3-146">Azure AD Connect může pouze opravit nebo zákona o certifikáty, které jsou podepsané svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="25ae3-146">Azure AD Connect can only repair or act on certificates that are self-signed.</span></span> <span data-ttu-id="25ae3-147">Azure AD Connect nelze opravit, certifikáty třetích stran.</span><span class="sxs-lookup"><span data-stu-id="25ae3-147">Azure AD Connect can't repair third-party certificates.</span></span>

## <span data-ttu-id="25ae3-148">Vytvořit federaci s Azure AD pomocí AlternateID<a name=alternateid></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-148">Federate with Azure AD using AlternateID <a name=alternateid></a></span></span>
<span data-ttu-id="25ae3-149">Doporučujeme, aby hello místní Name(UPN) hlavní název uživatele a cloud hello hlavní název uživatele jsou zachovány hello stejné.</span><span class="sxs-lookup"><span data-stu-id="25ae3-149">It is recommended that hello on-premises User Principal Name(UPN) and hello cloud User Principal Name are kept hello same.</span></span> <span data-ttu-id="25ae3-150">Pokud hello místní UPN používá směrovat domény (např.)</span><span class="sxs-lookup"><span data-stu-id="25ae3-150">If hello on-premises UPN uses a non-routable domain (ex.</span></span> <span data-ttu-id="25ae3-151">Contoso.Local) nebo se nedá změnit z důvodu závislosti aplikací toolocal, doporučujeme nastavení alternativního přihlašovacího ID.</span><span class="sxs-lookup"><span data-stu-id="25ae3-151">Contoso.local) or cannot be changed due toolocal application dependencies, we recommend setting up alternate login ID.</span></span> <span data-ttu-id="25ae3-152">Alternativního přihlašovacího ID vám umožní tooconfigure sign-in prostředí, kde uživatelé mohou přihlásit pomocí atributu než jejich UPN, jako je například e-mailu.</span><span class="sxs-lookup"><span data-stu-id="25ae3-152">Alternate login ID allows you tooconfigure a sign-in experience where users can sign in with an attribute other than their UPN, such as mail.</span></span> <span data-ttu-id="25ae3-153">Hello volbu pro hlavní název uživatele v Azure AD Connect výchozí toohello atribut userPrincipalName ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25ae3-153">hello choice for User Principal Name in Azure AD Connect defaults toohello userPrincipalName attribute in Active Directory.</span></span> <span data-ttu-id="25ae3-154">Pokud můžete vybrat jiný atribut pro hlavní název uživatele a jsou federaci pomocí služby AD FS, pak Azure AD Connect se konfigurace služby AD FS pro alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="25ae3-154">If you choose any other attribute for User Principal Name and are federating using AD FS, then Azure AD Connect will configure AD FS for alternate login ID.</span></span> <span data-ttu-id="25ae3-155">Níže je uveden příklad vybrat jiný atribut pro hlavní název uživatele:</span><span class="sxs-lookup"><span data-stu-id="25ae3-155">An example of choosing a different attribute for User Principal Name is shown below:</span></span>

![Výběr atributu alternativní ID](media/active-directory-aadconnect-federation-management/attributeselection.png)

<span data-ttu-id="25ae3-157">Konfigurace alternativního přihlašovacího ID pro službu AD FS zahrnuje dva hlavní kroky:</span><span class="sxs-lookup"><span data-stu-id="25ae3-157">Configuring alternate login ID for AD FS consists of two main steps:</span></span>
1. <span data-ttu-id="25ae3-158">**Konfigurace hello správnou sadu vystavování deklarací**: důvěřovat hello pravidel vystavování deklarací identity v předávající strany hello Azure AD jsou atribut UserPrincipalName hello vybrané upravené toouse hello alternativní ID uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-158">**Configure hello right set of issuance claims**: hello issuance claim rules in hello Azure AD relying party trust are modified toouse hello selected UserPrincipalName attribute as hello alternate ID of hello user.</span></span>
2. <span data-ttu-id="25ae3-159">**Povolit alternativního přihlašovacího ID v konfiguraci služby AD FS hello**: Konfigurace hello AD FS se aktualizuje tak, aby služba AD FS můžete vyhledat uživatele v hello odpovídající doménovými strukturami pomocí hello alternativní ID.</span><span class="sxs-lookup"><span data-stu-id="25ae3-159">**Enable alternate login ID in hello AD FS configuration**: hello AD FS configuration is updated so that AD FS can look up users in hello appropriate forests using hello alternate ID.</span></span> <span data-ttu-id="25ae3-160">Tato konfigurace je podporována pro službu AD FS v systému Windows Server 2012 R2 (s KB2919355) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="25ae3-160">This configuration is supported for AD FS on Windows Server 2012 R2 (with KB2919355) or later.</span></span> <span data-ttu-id="25ae3-161">Pokud server hello AD FS 2012 R2, vyžaduje Azure AD Connect zkontroluje přítomnost hello hello KB.</span><span class="sxs-lookup"><span data-stu-id="25ae3-161">If hello AD FS servers are 2012 R2, Azure AD Connect checks for hello presence of hello required KB.</span></span> <span data-ttu-id="25ae3-162">Pokud hello KB není zjištěna, upozornění se zobrazí po dokončení konfigurace, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="25ae3-162">If hello KB is not detected, a warning will be displayed after configuration completes, as shown below:</span></span>

    ![Upozornění na 2012R2 chybí KB](media/active-directory-aadconnect-federation-management/kbwarning.png)

    <span data-ttu-id="25ae3-164">Konfigurace hello toorectify v případě chybějící KB, nainstalujte hello požadované [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) a pak opravte hello důvěryhodnosti pomocí [opravit AAD a vztah důvěryhodnosti služby AD FS](#repairthetrust).</span><span class="sxs-lookup"><span data-stu-id="25ae3-164">toorectify hello configuration in case of missing KB, install hello required [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) and then repair hello trust using [Repair AAD and AD FS Trust](#repairthetrust).</span></span>

> [!NOTE]
> <span data-ttu-id="25ae3-165">Další informace o toomanually alternateID a kroky konfigurace, přečtěte si [konfigurace alternativního přihlašovacího ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span><span class="sxs-lookup"><span data-stu-id="25ae3-165">For more information on alternateID and steps toomanually configure, read [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span></span>

## <span data-ttu-id="25ae3-166">Přidejte server služby AD FS<a name=addadfsserver></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-166">Add an AD FS server <a name=addadfsserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="25ae3-167">server tooadd služby AD FS, Azure AD Connect vyžaduje certifikát PFX hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-167">tooadd an AD FS server, Azure AD Connect requires hello PFX certificate.</span></span> <span data-ttu-id="25ae3-168">Proto můžete tuto operaci provést pouze v případě, že jste nakonfigurovali farmy hello AD FS pomocí Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="25ae3-168">Therefore, you can perform this operation only if you configured hello AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="25ae3-169">Vyberte **nasadit další federační Server**a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-169">Select **Deploy an additional Federation Server**, and click **Next**.</span></span>

   ![Další federační server](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. <span data-ttu-id="25ae3-171">Na hello **připojit tooAzure AD** stránky, zadejte svoje přihlašovací údaje globálního správce pro Azure AD a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-171">On hello **Connect tooAzure AD** page, enter your global administrator credentials for Azure AD, and click **Next**.</span></span>

   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. <span data-ttu-id="25ae3-173">Zadejte pověření správce domény hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-173">Provide hello domain administrator credentials.</span></span>

   ![Pověření správce domény](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. <span data-ttu-id="25ae3-175">Azure AD Connect požádá o hello heslo souboru PFX hello, který jste zadali při konfiguraci nové farmě služby AD FS službou Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="25ae3-175">Azure AD Connect asks for hello password of hello PFX file that you provided while configuring your new AD FS farm with Azure AD Connect.</span></span> <span data-ttu-id="25ae3-176">Klikněte na tlačítko **zadejte heslo** tooprovide hello heslo pro soubor PFX hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-176">Click **Enter Password** tooprovide hello password for hello PFX file.</span></span>

   ![Heslo certifikátu](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Zadejte certifikát SSL](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. <span data-ttu-id="25ae3-179">Na hello **servery služby AD FS** stránky, zadejte název serveru hello nebo IP adresu toobe přidat toohello AD FS farmy.</span><span class="sxs-lookup"><span data-stu-id="25ae3-179">On hello **AD FS Servers** page, enter hello server name or IP address toobe added toohello AD FS farm.</span></span>

   ![Servery služby AD FS](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. <span data-ttu-id="25ae3-181">Klikněte na tlačítko **Další**a projít hello konečné **konfigurace** stránky.</span><span class="sxs-lookup"><span data-stu-id="25ae3-181">Click **Next**, and go through hello final **Configure** page.</span></span> <span data-ttu-id="25ae3-182">Po dokončení přidávání hello servery toohello AD FS farmy Azure AD Connect bude mu udělená hello možnost tooverify hello připojení.</span><span class="sxs-lookup"><span data-stu-id="25ae3-182">After Azure AD Connect has finished adding hello servers toohello AD FS farm, you will be given hello option tooverify hello connectivity.</span></span>

   ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![Instalace byla dokončena](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <span data-ttu-id="25ae3-185">Přidání serveru AD FS WAP<a name=addwapserver></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-185">Add an AD FS WAP server <a name=addwapserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="25ae3-186">tooadd serveru WAP, Azure AD Connect vyžaduje certifikát PFX hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-186">tooadd a WAP server, Azure AD Connect requires hello PFX certificate.</span></span> <span data-ttu-id="25ae3-187">Pokud jste nakonfigurovali farmy hello AD FS pomocí Azure AD Connect tedy můžete provádět jenom tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="25ae3-187">Therefore, you can only perform this operation if you configured hello AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="25ae3-188">Vyberte **nasadit Proxy webových aplikací** hello seznamu dostupných úloh.</span><span class="sxs-lookup"><span data-stu-id="25ae3-188">Select **Deploy Web Application Proxy** from hello list of available tasks.</span></span>

   ![Nasazení služby Proxy webových aplikací](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. <span data-ttu-id="25ae3-190">Zadejte pověření pro globálního správce Azure hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-190">Provide hello Azure global administrator credentials.</span></span>

   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. <span data-ttu-id="25ae3-192">Na hello **certifikát SSL zadejte** stránky, zadejte heslo hello hello souboru PFX, který jste zadali při konfiguraci farmy hello AD FS službou Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="25ae3-192">On hello **Specify SSL certificate** page, provide hello password for hello PFX file that you provided when you configured hello AD FS farm with Azure AD Connect.</span></span>
   <span data-ttu-id="25ae3-193">![Heslo certifikátu](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span><span class="sxs-lookup"><span data-stu-id="25ae3-193">![Certificate password](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span></span>

    ![Zadejte certifikát SSL](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. <span data-ttu-id="25ae3-195">Přidejte toobe server hello přidán jako WAP server.</span><span class="sxs-lookup"><span data-stu-id="25ae3-195">Add hello server toobe added as a WAP server.</span></span> <span data-ttu-id="25ae3-196">Protože hello WAP server nemusí být připojené k toohello domény, Průvodce hello požádá o přidávaný server toohello přihlašovací údaje pro správu.</span><span class="sxs-lookup"><span data-stu-id="25ae3-196">Because hello WAP server might not be joined toohello domain, hello wizard asks for administrative credentials toohello server being added.</span></span>

   ![Přihlašovací údaje správce serveru](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. <span data-ttu-id="25ae3-198">Na hello **přihlašovací údaje pro vztah důvěryhodnosti Proxy** stránky, zadejte přihlašovací údaje pro správu tooconfigure hello proxy vztahu důvěryhodnosti a přístup hello primární server ve farmě služby AD FS hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-198">On hello **Proxy trust credentials** page, provide administrative credentials tooconfigure hello proxy trust and access hello primary server in hello AD FS farm.</span></span>

   ![Přihlašovací údaje pro vztah důvěryhodnosti proxy](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. <span data-ttu-id="25ae3-200">Na hello **připraven tooconfigure** stránce hello Průvodce zobrazí hello seznam akcí, které budou provedeny.</span><span class="sxs-lookup"><span data-stu-id="25ae3-200">On hello **Ready tooconfigure** page, hello wizard shows hello list of actions that will be performed.</span></span>

   ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. <span data-ttu-id="25ae3-202">Klikněte na tlačítko **nainstalovat** toofinish hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="25ae3-202">Click **Install** toofinish hello configuration.</span></span> <span data-ttu-id="25ae3-203">Po dokončení konfigurace hello hello poskytuje průvodce hello hello tooverify možnost připojení toohello servery.</span><span class="sxs-lookup"><span data-stu-id="25ae3-203">After hello configuration is complete, hello wizard gives you hello option tooverify hello connectivity toohello servers.</span></span> <span data-ttu-id="25ae3-204">Klikněte na tlačítko **ověřte** toocheck připojení.</span><span class="sxs-lookup"><span data-stu-id="25ae3-204">Click **Verify** toocheck connectivity.</span></span>

   ![Instalace byla dokončena](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <span data-ttu-id="25ae3-206">Přidání federované domény<a name=addfeddomain></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-206">Add a federated domain <a name=addfeddomain></a></span></span>

<span data-ttu-id="25ae3-207">Je snadno tooadd toobe domény Federovaná pomocí Azure AD pomocí Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="25ae3-207">It's easy tooadd a domain toobe federated with Azure AD by using Azure AD Connect.</span></span> <span data-ttu-id="25ae3-208">Azure AD Connect přidá hello domény pro federaci a upraví deklarace identity hello pravidla toocorrectly odráží vystavitele hello, pokud máte více domén sdružených se službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="25ae3-208">Azure AD Connect adds hello domain for federation and modifies hello claim rules toocorrectly reflect hello issuer when you have multiple domains federated with Azure AD.</span></span>

1. <span data-ttu-id="25ae3-209">tooadd federovanou doménu, vyberte hello úloh **přidat další doménu služby Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-209">tooadd a federated domain, select hello task **Add an additional Azure AD domain**.</span></span>

   ![Další doménu služby Azure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. <span data-ttu-id="25ae3-211">Na další stránku hello hello průvodci zadejte přihlašovací údaje hello globálního správce pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="25ae3-211">On hello next page of hello wizard, provide hello global administrator credentials for Azure AD.</span></span>

   ![Připojit tooAzure AD](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. <span data-ttu-id="25ae3-213">Na hello **přihlašovací údaje vzdálený přístup** stránky, zadejte přihlašovací údaje správce domény hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-213">On hello **Remote access credentials** page, provide hello domain administrator credentials.</span></span>

   ![Přihlašovací údaje vzdálený přístup](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. <span data-ttu-id="25ae3-215">Na další stránce hello hello Průvodce poskytuje seznam domén služby Azure AD, které můžete vytvořit federaci vašeho místního adresáře s.</span><span class="sxs-lookup"><span data-stu-id="25ae3-215">On hello next page, hello wizard provides a list of Azure AD domains that you can federate your on-premises directory with.</span></span> <span data-ttu-id="25ae3-216">Vyberte doménu hello hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="25ae3-216">Choose hello domain from hello list.</span></span>

   ![Azure AD domain](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    <span data-ttu-id="25ae3-218">Po výběru domény hello hello Průvodce poskytuje k příslušné informace o dalších akcích, které hello průvodce bude trvat a hello dopad hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="25ae3-218">After you choose hello domain, hello wizard provides you with appropriate information about further actions that hello wizard will take and hello impact of hello configuration.</span></span> <span data-ttu-id="25ae3-219">V některých případech Pokud vyberete domény, který není ještě ověřit ve službě Azure AD, Průvodce hello vám poskytne informace toohelp ověření domény hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-219">In some cases, if you select a domain that isn't yet verified in Azure AD, hello wizard provides you with information toohelp you verify hello domain.</span></span> <span data-ttu-id="25ae3-220">V tématu [přidat název tooAzure vaši vlastní doménu služby Active Directory](../active-directory-add-domain.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="25ae3-220">See [Add your custom domain name tooAzure Active Directory](../active-directory-add-domain.md) for more details.</span></span>

5. <span data-ttu-id="25ae3-221">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-221">Click **Next**.</span></span> <span data-ttu-id="25ae3-222">Hello **připraven tooconfigure** stránka zobrazuje hello seznam akcí, které provádí Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="25ae3-222">hello **Ready tooconfigure** page shows hello list of actions that Azure AD Connect will perform.</span></span> <span data-ttu-id="25ae3-223">Klikněte na tlačítko **nainstalovat** toofinish hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="25ae3-223">Click **Install** toofinish hello configuration.</span></span>

   ![Připraveno tooconfigure](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> <span data-ttu-id="25ae3-225">Přidat uživatele z hello federované domény musí být synchronizovány, než bude možné toologin tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="25ae3-225">Users from hello added federated domain must be synchronized before they will be able toologin tooAzure AD.</span></span>

## <a name="ad-fs-customization"></a><span data-ttu-id="25ae3-226">Přizpůsobení AD FS</span><span class="sxs-lookup"><span data-stu-id="25ae3-226">AD FS customization</span></span>
<span data-ttu-id="25ae3-227">Hello následující části obsahují informace o některých běžných úloh hello, můžete mít tooperform při přizpůsobit přihlašovací stránku služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ae3-227">hello following sections provide details about some of hello common tasks that you might have tooperform when you customize your AD FS sign-in page.</span></span>

## <span data-ttu-id="25ae3-228">Přidat vlastní logo nebo obrázku<a name=customlogo></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-228">Add a custom company logo or illustration <a name=customlogo></a></span></span>
<span data-ttu-id="25ae3-229">logo hello toochange hello společnosti, který se zobrazí na hello **přihlášení** stránky, použijte následující syntaxi a rutinu prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-229">toochange hello logo of hello company that's displayed on hello **Sign-in** page, use hello following Windows PowerShell cmdlet and syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="25ae3-230">Hello doporučená dimenze pro hello logo jsou 260 x 35 při 96 dpi a velikost souboru nepřesahovala 10 KB.</span><span class="sxs-lookup"><span data-stu-id="25ae3-230">hello recommended dimensions for hello logo are 260 x 35 @ 96 dpi with a file size no greater than 10 KB.</span></span>

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> <span data-ttu-id="25ae3-231">Hello *TargetName* parametr je povinný.</span><span class="sxs-lookup"><span data-stu-id="25ae3-231">hello *TargetName* parameter is required.</span></span> <span data-ttu-id="25ae3-232">Hello výchozí motiv vydaný se službou AD FS se nazývá výchozí.</span><span class="sxs-lookup"><span data-stu-id="25ae3-232">hello default theme that's released with AD FS is named Default.</span></span>

## <span data-ttu-id="25ae3-233">Přidejte popis přihlášení<a name=addsignindescription></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-233">Add a sign-in description <a name=addsignindescription></a></span></span>
<span data-ttu-id="25ae3-234">tooadd přihlašovací stránce popis toohello **přihlašovací stránce**, použijte následující syntaxi a rutinu prostředí Windows PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-234">tooadd a sign-in page description toohello **Sign-in page**, use hello following Windows PowerShell cmdlet and syntax.</span></span>

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in tooContoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <span data-ttu-id="25ae3-235">Upravit pravidla deklarace identity služby AD FS<a name=modclaims></a></span><span class="sxs-lookup"><span data-stu-id="25ae3-235">Modify AD FS claim rules <a name=modclaims></a></span></span>
<span data-ttu-id="25ae3-236">Služba AD FS podporuje bohaté deklarace jazyk, který můžete použít toocreate vlastní pravidla deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="25ae3-236">AD FS supports a rich claim language that you can use toocreate custom claim rules.</span></span> <span data-ttu-id="25ae3-237">Další informace najdete v tématu [hello Role hello jazyka pravidel deklarací identity](https://technet.microsoft.com/library/dd807118.aspx).</span><span class="sxs-lookup"><span data-stu-id="25ae3-237">For more information, see [hello Role of hello Claim Rule Language](https://technet.microsoft.com/library/dd807118.aspx).</span></span>

<span data-ttu-id="25ae3-238">Hello následující části popisují, jak můžete napsat vlastní pravidla pro některé scénáře, které se týkají tooAzure AD a federační služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ae3-238">hello following sections describe how you can write custom rules for some scenarios that relate tooAzure AD and AD FS federation.</span></span>

### <a name="immutable-id-conditional-on-a-value-being-present-in-hello-attribute"></a><span data-ttu-id="25ae3-239">Neměnné ID podmíněného na hodnotu, která se nachází v atributu hello</span><span class="sxs-lookup"><span data-stu-id="25ae3-239">Immutable ID conditional on a value being present in hello attribute</span></span>
<span data-ttu-id="25ae3-240">Azure AD Connect umožňuje určit atributu toobe použít jako zdrojové ukotvení, pokud jsou objekty synchronizované tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="25ae3-240">Azure AD Connect lets you specify an attribute toobe used as a source anchor when objects are synced tooAzure AD.</span></span> <span data-ttu-id="25ae3-241">Pokud hello hodnoty ve vlastním atributu hello není prázdná, můžete chtít tooissue deklaraci identity neměnné ID.</span><span class="sxs-lookup"><span data-stu-id="25ae3-241">If hello value in hello custom attribute is not empty, you might want tooissue an immutable ID claim.</span></span>

<span data-ttu-id="25ae3-242">Například můžete vybrat **ms-ds-consistencyguid** jako atribut hello hello zdrojové ukotvení a problém **ImmutableID** jako **ms-ds-consistencyguid** v případu hello atribut má hodnotu před ním.</span><span class="sxs-lookup"><span data-stu-id="25ae3-242">For example, you might select **ms-ds-consistencyguid** as hello attribute for hello source anchor and issue **ImmutableID** as **ms-ds-consistencyguid** in case hello attribute has a value against it.</span></span> <span data-ttu-id="25ae3-243">Pokud není žádná hodnota proti hello atribut, **objectGuid** jako hello neměnné ID.</span><span class="sxs-lookup"><span data-stu-id="25ae3-243">If there's no value against hello attribute, issue **objectGuid** as hello immutable ID.</span></span> <span data-ttu-id="25ae3-244">Hello sadu vlastní pravidla deklarací identity můžete vytvořit, jak je popsáno v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-244">You can construct hello set of custom claim rules as described in hello following section.</span></span>

<span data-ttu-id="25ae3-245">**Pravidlo 1: Atributy dotazu**</span><span class="sxs-lookup"><span data-stu-id="25ae3-245">**Rule 1: Query attributes**</span></span>

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

<span data-ttu-id="25ae3-246">V tomto pravidle, že dotazování hello hodnoty **ms-ds-consistencyguid** a **objectGuid** pro hello uživatele ze služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25ae3-246">In this rule, you're querying hello values of **ms-ds-consistencyguid** and **objectGuid** for hello user from Active Directory.</span></span> <span data-ttu-id="25ae3-247">Změnit název příslušné úložiště hello úložiště název tooan ve vašem nasazení služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="25ae3-247">Change hello store name tooan appropriate store name in your AD FS deployment.</span></span> <span data-ttu-id="25ae3-248">Také změnit hello deklarace identity typu tooa správné deklarace typu pro federační, jak jsou definovány pro **objectGuid** a **ms-ds-consistencyguid**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-248">Also change hello claims type tooa proper claims type for your federation, as defined for **objectGuid** and **ms-ds-consistencyguid**.</span></span>

<span data-ttu-id="25ae3-249">Navíc pomocí **přidat** a není **problém**, vyhnete se přidávání odchozí problém pro entitu hello a můžete použít hodnoty hello jako pomocných hodnot.</span><span class="sxs-lookup"><span data-stu-id="25ae3-249">Also, by using **add** and not **issue**, you avoid adding an outgoing issue for hello entity, and can use hello values as intermediate values.</span></span> <span data-ttu-id="25ae3-250">Po stanovení které toouse hodnotu jako text hello neměnné ID vydáte hello deklarace novější pravidlo</span><span class="sxs-lookup"><span data-stu-id="25ae3-250">You will issue hello claim in a later rule after you establish which value toouse as hello immutable ID.</span></span>

<span data-ttu-id="25ae3-251">**Pravidlo 2: Zkontrolujte, jestli ms-ds-consistencyguid existuje pro uživatele hello**</span><span class="sxs-lookup"><span data-stu-id="25ae3-251">**Rule 2: Check if ms-ds-consistencyguid exists for hello user**</span></span>

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

<span data-ttu-id="25ae3-252">Toto pravidlo definuje dočasné příznak **idflag** , je nastaven příliš**useguid** Pokud neexistuje žádné **ms-ds-consistencyguid** vyplněný pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-252">This rule defines a temporary flag called **idflag** that is set too**useguid** if there's no **ms-ds-consistencyguid** populated for hello user.</span></span> <span data-ttu-id="25ae3-253">Logika Hello za to je hello fakt, že služba AD FS neumožňuje prázdné deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="25ae3-253">hello logic behind this is hello fact that AD FS doesn't allow empty claims.</span></span> <span data-ttu-id="25ae3-254">Takže když přidáte http://contoso.com/ws/2016/02/identity/claims/objectguid deklarace identity a http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid v pravidla 1, v níž se **msdsconsistencyguid** pouze v případě deklarace identity Hodnota Hello je vyplněný pro uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-254">So when you add claims http://contoso.com/ws/2016/02/identity/claims/objectguid and http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in Rule 1, you end up with an **msdsconsistencyguid** claim only if hello value is populated for hello user.</span></span> <span data-ttu-id="25ae3-255">Pokud není vyplněné, služby AD FS uvidí, že bude mít prázdnou hodnotu a okamžitě se zahodí.</span><span class="sxs-lookup"><span data-stu-id="25ae3-255">If it isn't populated, AD FS sees that it will have an empty value and drops it immediately.</span></span> <span data-ttu-id="25ae3-256">Všechny objekty se mají **objectGuid**, tak, že deklarace bude vždy existovat po provedení pravidla 1.</span><span class="sxs-lookup"><span data-stu-id="25ae3-256">All objects will have **objectGuid**, so that claim will always be there after Rule 1 is executed.</span></span>

<span data-ttu-id="25ae3-257">**Pravidlo 3: Vystavení ms-ds-consistencyguid jako neměnné ID, pokud je k dispozici**</span><span class="sxs-lookup"><span data-stu-id="25ae3-257">**Rule 3: Issue ms-ds-consistencyguid as immutable ID if it's present**</span></span>

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

<span data-ttu-id="25ae3-258">Jedná se implicitní **existují** zkontrolujte.</span><span class="sxs-lookup"><span data-stu-id="25ae3-258">This is an implicit **Exist** check.</span></span> <span data-ttu-id="25ae3-259">Pokud hodnota hello hello deklarace identity existuje, potom vydat, jako hello neměnné ID.</span><span class="sxs-lookup"><span data-stu-id="25ae3-259">If hello value for hello claim exists, then issue that as hello immutable ID.</span></span> <span data-ttu-id="25ae3-260">předchozí příklad Hello používá hello **nameidentifier** deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="25ae3-260">hello previous example uses hello **nameidentifier** claim.</span></span> <span data-ttu-id="25ae3-261">Budete mít toochange tento typ toohello odpovídající deklarace identity pro neměnné ID hello ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="25ae3-261">You'll have toochange this toohello appropriate claim type for hello immutable ID in your environment.</span></span>

<span data-ttu-id="25ae3-262">**Pravidlo 4: Pokud není k dispozici ms-ds-consistencyGuid vydání objectGuid jako neměnné ID**</span><span class="sxs-lookup"><span data-stu-id="25ae3-262">**Rule 4: Issue objectGuid as immutable ID if ms-ds-consistencyGuid is not present**</span></span>

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

<span data-ttu-id="25ae3-263">V tomto pravidle, se jednoduše kontrola hello dočasné příznak **idflag**.</span><span class="sxs-lookup"><span data-stu-id="25ae3-263">In this rule, you're simply checking hello temporary flag **idflag**.</span></span> <span data-ttu-id="25ae3-264">Rozhodnete, zda na základě deklarace identity hello tooissue na jeho hodnotu.</span><span class="sxs-lookup"><span data-stu-id="25ae3-264">You decide whether tooissue hello claim based on its value.</span></span>

> [!NOTE]
> <span data-ttu-id="25ae3-265">Hello pořadí těchto pravidel je důležité.</span><span class="sxs-lookup"><span data-stu-id="25ae3-265">hello sequence of these rules is important.</span></span>

### <a name="sso-with-a-subdomain-upn"></a><span data-ttu-id="25ae3-266">Jednotné přihlašování s subdoména UPN</span><span class="sxs-lookup"><span data-stu-id="25ae3-266">SSO with a subdomain UPN</span></span>
<span data-ttu-id="25ae3-267">Můžete přidat více než jedné domény toobe Federovaná pomocí Azure AD Connect, jak je popsáno v [přidání nové federované domény](active-directory-aadconnect-federation-management.md#addfeddomain).</span><span class="sxs-lookup"><span data-stu-id="25ae3-267">You can add more than one domain toobe federated by using Azure AD Connect, as described in [Add a new federated domain](active-directory-aadconnect-federation-management.md#addfeddomain).</span></span> <span data-ttu-id="25ae3-268">Deklarace hlavní název (UPN) uživatele hello musí změnit tak, aby odpovídá toohello kořenové domény a není hello subdomén, zprostředkovatele hello vystavitele ID, protože hello federované kořenové domény platí i pro podřízené hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-268">You must modify hello user principal name (UPN) claim so that hello issuer ID corresponds toohello root domain and not hello subdomain, because hello federated root domain also covers hello child.</span></span>

<span data-ttu-id="25ae3-269">Ve výchozím nastavení hello pravidel deklarace identity pro vystavitele, kterého ID je nastaven jako:</span><span class="sxs-lookup"><span data-stu-id="25ae3-269">By default, hello claim rule for issuer ID is set as:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Deklarace ID výchozí vystavitele](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

<span data-ttu-id="25ae3-271">Výchozí pravidlo Hello jednoduše trvá příponu UPN hello a používá je v deklarace ID vystavitele hello.</span><span class="sxs-lookup"><span data-stu-id="25ae3-271">hello default rule simply takes hello UPN suffix and uses it in hello issuer ID claim.</span></span> <span data-ttu-id="25ae3-272">Například Jan je uživatel v sub.contoso.com a je contoso.com sdružených se službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="25ae3-272">For example, John is a user in sub.contoso.com, and contoso.com is federated with Azure AD.</span></span> <span data-ttu-id="25ae3-273">Jan zadá john@sub.contoso.com jako hello uživatelské jméno při přihlášení tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="25ae3-273">John enters john@sub.contoso.com as hello username while signing in tooAzure AD.</span></span> <span data-ttu-id="25ae3-274">pravidlo deklarace identity Hello výchozí vystavitele ID ve službě AD FS ji zpracovává v hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="25ae3-274">hello default issuer ID claim rule in AD FS handles it in hello following manner:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

<span data-ttu-id="25ae3-275">**Hodnoty deklarace identity:** http://sub.contoso.com/adfs/services/trust/</span><span class="sxs-lookup"><span data-stu-id="25ae3-275">**Claim value:**  http://sub.contoso.com/adfs/services/trust/</span></span>

<span data-ttu-id="25ae3-276">toohave pouze hello kořenové domény v hello vystavitele hodnoty deklarace identity, změnit hello deklarace identity pravidlo toomatch hello následující:</span><span class="sxs-lookup"><span data-stu-id="25ae3-276">toohave only hello root domain in hello issuer claim value, change hello claim rule toomatch hello following:</span></span>

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a><span data-ttu-id="25ae3-277">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25ae3-277">Next steps</span></span>
<span data-ttu-id="25ae3-278">Další informace o [možnosti přihlášení uživatele](active-directory-aadconnect-user-signin.md).</span><span class="sxs-lookup"><span data-stu-id="25ae3-278">Learn more about [user sign-in options](active-directory-aadconnect-user-signin.md).</span></span>
