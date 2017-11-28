---
title: "zařízení připojená k hybridní tooconfigure aaaHow Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak zařízení připojená k hybridní tooconfigure Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: f97ea436eca2833d8a9843acd19e5c633bc0fc07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-hybrid-azure-active-directory-joined-devices"></a><span data-ttu-id="1a73b-103">Jak zařízení připojená k hybridní tooconfigure Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a73b-103">How tooconfigure hybrid Azure Active Directory joined devices</span></span>

<span data-ttu-id="1a73b-104">Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="1a73b-104">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> <span data-ttu-id="1a73b-105">Další podrobnosti najdete v tématu [Úvod toodevice správy ve službě Azure Active Directory](device-management-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1a73b-105">For more details, see [Introduction toodevice management in Azure Active Directory](device-management-introduction.md).</span></span>

<span data-ttu-id="1a73b-106">Pokud máte prostředí místní služby Active Directory a chcete toojoin tooAzure vaše zařízení připojená k doméně AD, můžete k tomu lze konfigurace hybridní Azure AD, které jsou připojené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a73b-106">If you have an on-premises Active Directory environment and you want toojoin your domain-joined devices tooAzure AD, you can accomplish this by configuring hybrid Azure AD joined devices.</span></span> <span data-ttu-id="1a73b-107">Hello téma vám poskytne hello související kroky.</span><span class="sxs-lookup"><span data-stu-id="1a73b-107">hello topic provides you with hello related steps.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="1a73b-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1a73b-108">Before you begin</span></span>

<span data-ttu-id="1a73b-109">Před zahájením konfigurace zařízení ve vašem prostředí připojená k hybridní Azure AD, byste si měli přečíst s hello Podporované scénáře a omezení hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-109">Before you start configuring hybrid Azure AD joined devices in your environment, you should familiarize yourself with hello supported scenarios and hello constraints.</span></span>  

<span data-ttu-id="1a73b-110">tooimprove hello čitelnost hello popisy, toto téma používá hello následujících podmínek:</span><span class="sxs-lookup"><span data-stu-id="1a73b-110">tooimprove hello readability of hello descriptions, this topic uses hello following term:</span></span> 

- <span data-ttu-id="1a73b-111">**Aktuální zařízení se systémem Windows** -tento termín se vztahuje toodomain připojené zařízení se systémem Windows 10 nebo Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="1a73b-111">**Windows current devices** - This term refers toodomain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="1a73b-112">**Zařízení se systémem Windows nižší úrovně** -tento termín se vztahuje tooall **podporované** připojený k doméně zařízení Windows, která nejsou spuštěné Windows 10 ani systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="1a73b-112">**Windows down-level devices** - This term refers tooall **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="1a73b-113">Aktuální zařízení se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="1a73b-113">Windows current devices</span></span>

- <span data-ttu-id="1a73b-114">Pro zařízení se systémem plochy operační systém Windows hello, doporučujeme používat Windows 10 Anniversary Update (verze 1607) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1a73b-114">For devices running hello Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="1a73b-115">registrace zařízení se systémem Windows aktuální Hello **je** podporované v nefederované prostředích, jako je konfigurace synchronizace hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="1a73b-115">hello registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="1a73b-116">Zařízení se systémem Windows nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="1a73b-116">Windows down-level devices</span></span>

- <span data-ttu-id="1a73b-117">jsou podporovány následující zařízení se systémem Windows nižší úrovně Hello:</span><span class="sxs-lookup"><span data-stu-id="1a73b-117">hello following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="1a73b-118">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="1a73b-118">Windows 8.1</span></span>
    - <span data-ttu-id="1a73b-119">Windows 7</span><span class="sxs-lookup"><span data-stu-id="1a73b-119">Windows 7</span></span>
    - <span data-ttu-id="1a73b-120">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="1a73b-120">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="1a73b-121">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="1a73b-121">Windows Server 2012</span></span>
    - <span data-ttu-id="1a73b-122">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="1a73b-122">Windows Server 2008 R2</span></span>
- <span data-ttu-id="1a73b-123">registrace zařízení se systémem Windows nižší úrovně Hello **je** podporované v prostředích nefederovaných prostřednictvím bezproblémové jednotné přihlašování [Azure Active Directory bezproblémové jednotné přihlašování](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="1a73b-123">hello registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="1a73b-124">registrace zařízení se systémem Windows nižší úrovně Hello **není** podporovaná pro zařízení s použitím profilů roamingu.</span><span class="sxs-lookup"><span data-stu-id="1a73b-124">hello registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="1a73b-125">Pokud se spoléháte na cestovních profilů nebo nastavení, používají systém Windows 10.</span><span class="sxs-lookup"><span data-stu-id="1a73b-125">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="1a73b-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1a73b-126">Prerequisites</span></span>

<span data-ttu-id="1a73b-127">Než začnete, povolení hybridního Azure AD, které jsou připojené k zařízení ve vaší organizaci, je nutné toomake, zda jsou spuštěny na aktuální verzi Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="1a73b-127">Before you start enabling hybrid Azure AD joined devices in your organization, you need toomake sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="1a73b-128">Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="1a73b-128">Azure AD Connect:</span></span>

- <span data-ttu-id="1a73b-129">Zachová hello přidružení mezi hello účet počítače v místní službě Active Directory (AD) a objekt hello zařízení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-129">Keeps hello association between hello computer account in your on-premises Active Directory (AD) and hello device object in Azure AD.</span></span> 
- <span data-ttu-id="1a73b-130">Povoluje další zařízení související s funkcí, jako Windows Hello pro firmy.</span><span class="sxs-lookup"><span data-stu-id="1a73b-130">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="1a73b-131">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="1a73b-131">Configuration steps</span></span>

<span data-ttu-id="1a73b-132">Můžete nakonfigurovat hybridní zařízení připojených k Azure AD pro různé typy zařízení platformy systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1a73b-132">You can configure hybrid Azure AD joined devices for various types of Windows device platforms.</span></span> <span data-ttu-id="1a73b-133">Toto téma obsahuje hello požadované kroky pro všechny scénáře obvyklá konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1a73b-133">This topic includes hello required steps for all typical configuration scenarios.</span></span>  

<span data-ttu-id="1a73b-134">Použijte následující tabulku tooget přehled hello kroky, které jsou požadovány pro váš scénář hello:</span><span class="sxs-lookup"><span data-stu-id="1a73b-134">Use hello following table tooget an overview of hello steps that are required for your scenario:</span></span>  



| <span data-ttu-id="1a73b-135">Kroky</span><span class="sxs-lookup"><span data-stu-id="1a73b-135">Steps</span></span>                                      | <span data-ttu-id="1a73b-136">Synchronizace hodnot hash aktuální a heslo systému Windows</span><span class="sxs-lookup"><span data-stu-id="1a73b-136">Windows current and password hash sync</span></span> | <span data-ttu-id="1a73b-137">Aktuální Windows a federation</span><span class="sxs-lookup"><span data-stu-id="1a73b-137">Windows current and federation</span></span> | <span data-ttu-id="1a73b-138">Windows nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="1a73b-138">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="1a73b-139">Krok 1: Konfigurace spojovacího bodu služby</span><span class="sxs-lookup"><span data-stu-id="1a73b-139">Step 1: Configure service connection point</span></span> | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| <span data-ttu-id="1a73b-143">Krok 2: Nastavení vystavování deklarací identity</span><span class="sxs-lookup"><span data-stu-id="1a73b-143">Step 2: Setup issuance of claims</span></span>           |                                        | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| <span data-ttu-id="1a73b-146">Krok 3: Zapnutí zařízení s Windows 10</span><span class="sxs-lookup"><span data-stu-id="1a73b-146">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Zaškrtnout][1]        |
| <span data-ttu-id="1a73b-148">Krok 4: Řízení nasazení a zavedení</span><span class="sxs-lookup"><span data-stu-id="1a73b-148">Step 4: Control deployment and rollout</span></span>     | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| <span data-ttu-id="1a73b-152">Krok 5: Ověření připojeného k zařízení</span><span class="sxs-lookup"><span data-stu-id="1a73b-152">Step 5: Verify joined devices</span></span>          | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="1a73b-156">Krok 1: Konfigurace spojovacího bodu služby</span><span class="sxs-lookup"><span data-stu-id="1a73b-156">Step 1: Configure service connection point</span></span>

<span data-ttu-id="1a73b-157">objekt Spojovací bod připojení služby Hello používá zařízení během registrace toodiscover hello informace klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-157">hello service connection point (SCP) object is used by your devices during hello registration toodiscover Azure AD tenant information.</span></span> <span data-ttu-id="1a73b-158">Ve vaší místní služby Active Directory (AD) musí existovat hello spojovací bod služby objektu pro zařízení s Azure AD, které jsou připojené k hybridní hello v hello názvový kontext oddílu konfigurace doménové struktury hello počítače.</span><span class="sxs-lookup"><span data-stu-id="1a73b-158">In your on-premises Active Directory (AD), hello SCP object for hello hybrid Azure AD joined devices must exist in hello configuration naming context partition of hello computer's forest.</span></span> <span data-ttu-id="1a73b-159">Není k dispozici pouze jeden názvovém kontextu konfigurace pro každou doménovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="1a73b-159">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="1a73b-160">V konfiguraci s více doménovými strukturami služby Active Directory musí existovat spojovací bod služby hello ve všech doménových strukturách obsahující počítače, připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="1a73b-160">In a multi-forest Active Directory configuration, hello service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="1a73b-161">Můžete použít hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) rutiny tooretrieve hello názvovém kontextu konfigurace doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="1a73b-161">You can use hello [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello configuration naming context of your forest.</span></span>  

<span data-ttu-id="1a73b-162">Pro doménovou strukturu s názvem domény služby Active Directory hello *fabrikam.com*, je v názvovém kontextu konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="1a73b-162">For a forest with hello Active Directory domain name *fabrikam.com*, hello configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="1a73b-163">V doménové struktuře se nachází v objektu hello spojovací bod služby pro hello automatické registrace zařízení připojených k doméně:</span><span class="sxs-lookup"><span data-stu-id="1a73b-163">In your forest, hello SCP object for hello auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="1a73b-164">V závislosti na tom, jak jste nasadili Azure AD Connect hello spojovací bod služby objekt může mít již byla konfigurována.</span><span class="sxs-lookup"><span data-stu-id="1a73b-164">Depending on how you have deployed Azure AD Connect, hello SCP object may have already been configured.</span></span>
<span data-ttu-id="1a73b-165">Můžete ověřit existenci hello hello objekt a načíst hodnoty hello zjišťování pomocí hello následující skript prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="1a73b-165">You can verify hello existence of hello object and retrieve hello discovery values using hello following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="1a73b-166">Hello **$scp. Klíčová slova** výstup zobrazuje informace o klienta hello Azure AD, například:</span><span class="sxs-lookup"><span data-stu-id="1a73b-166">hello **$scp.Keywords** output shows hello Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="1a73b-167">Pokud spojovací bod služby hello neexistuje, můžete ho vytvořit spuštěním hello `Initialize-ADSyncDomainJoinedComputerSync` rutiny na serveru Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="1a73b-167">If hello service connection point does not exist, you can create it by running hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="1a73b-168">Pověření správce podniku je požadovaná toorun tuto rutinu.</span><span class="sxs-lookup"><span data-stu-id="1a73b-168">Enterprise admin credential is required toorun this cmdlet.</span></span>  
<span data-ttu-id="1a73b-169">rutina Hello:</span><span class="sxs-lookup"><span data-stu-id="1a73b-169">hello cmdlet:</span></span>

- <span data-ttu-id="1a73b-170">Vytvoří spojovací bod služby hello v doménové struktuře služby Active Directory hello, Azure AD Connect je připojena k.</span><span class="sxs-lookup"><span data-stu-id="1a73b-170">Creates hello service connection point in hello Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="1a73b-171">Vyžaduje, abyste toospecify hello `AdConnectorAccount` parametr.</span><span class="sxs-lookup"><span data-stu-id="1a73b-171">Requires you toospecify hello `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="1a73b-172">Toto je hello účet, který je nakonfigurovaný jako účet konektoru ve službě Azure AD connect služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1a73b-172">This is hello account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="1a73b-173">Hello skript je ukázkou příklad, pomocí rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-173">hello following script shows an example for using hello cmdlet.</span></span> <span data-ttu-id="1a73b-174">V tento skript `$aadAdminCred = Get-Credential` vyžaduje, abyste tootype uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="1a73b-174">In this script, `$aadAdminCred = Get-Credential` requires you tootype a user name.</span></span> <span data-ttu-id="1a73b-175">Je třeba tooprovide hello uživatelské jméno ve formátu hlavního názvu (UPN) uživatele hello (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="1a73b-175">You need tooprovide hello user name in hello user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="1a73b-176">Hello `Initialize-ADSyncDomainJoinedComputerSync` rutiny:</span><span class="sxs-lookup"><span data-stu-id="1a73b-176">hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="1a73b-177">Používá modul Active Directory PowerShell hello, které závisí na službě Active Directory Web Services spuštěných na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="1a73b-177">Uses hello Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="1a73b-178">Služba Active Directory Web Services je podporována v řadičích domény se systémem Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1a73b-178">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="1a73b-179">Je podporovaná jenom rozhraním hello **MSOnline PowerShell verze modulu 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-179">Is only supported by hello **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="1a73b-180">toodownload tento modul použít [odkaz](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="1a73b-180">toodownload this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="1a73b-181">Pro řadiče domény se systémem Windows Server 2008 nebo dřívějších verzí použijte níže uvedený spojovací bod služby hello toocreate skript hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-181">For domain controllers running Windows Server 2008 or earlier versions, use hello script below toocreate hello service connection point.</span></span>

<span data-ttu-id="1a73b-182">V konfiguraci s více doménovými strukturami byste měli použít následující skript spojovací bod služby hello toocreate v každé doménové struktuře, kde existují počítače hello:</span><span class="sxs-lookup"><span data-stu-id="1a73b-182">In a multi-forest configuration, you should use hello following script toocreate hello service connection point in each forest where computers exist:</span></span>
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="1a73b-183">Krok 2: Nastavení vystavování deklarací identity</span><span class="sxs-lookup"><span data-stu-id="1a73b-183">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="1a73b-184">V federované Azure AD konfigurace zařízení závisí na službě Active Directory Federation Services (AD FS) nebo 3. stran místní federační služby tooauthenticate tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-184">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service tooauthenticate tooAzure AD.</span></span> <span data-ttu-id="1a73b-185">Zařízení ověření tooget přístupu k tokenu tooregister proti hello Azure Active Directory Device Registration Service (Azure DRS).</span><span class="sxs-lookup"><span data-stu-id="1a73b-185">Devices authenticate tooget an access token tooregister against hello Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="1a73b-186">Windows, které aktuální zařízení ověřování pomocí integrovaného ověřování systému Windows tooan aktivní WS-Trust koncový bod (verze 1.3 nebo 2005) hostované hello místní federační služby.</span><span class="sxs-lookup"><span data-stu-id="1a73b-186">Windows current devices authenticate using Integrated Windows Authentication tooan active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by hello on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="1a73b-187">Při použití služby AD FS buď **adfs/services/vztah důvěryhodnosti nebo 13 nebo windowstransport** nebo **služby AD FS nebo služby nebo vztahu důvěryhodnosti/2005/windowstransport** musí být povolena.</span><span class="sxs-lookup"><span data-stu-id="1a73b-187">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="1a73b-188">Pokud používáte hello ověřování webový proxy server, ujistěte se také, že tento koncový bod publikovaný prostřednictvím proxy serveru hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-188">If you are using hello Web Authentication Proxy, also ensure that this endpoint is published through hello proxy.</span></span> <span data-ttu-id="1a73b-189">Uvidíte, jaké koncových bodů jsou povolené prostřednictvím konzoly pro správu služby AD FS hello pod **služby > Koncové body**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-189">You can see what end-points are enabled through hello AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="1a73b-190">Pokud nemáte jako místní federační služby AD FS, postupujte podle pokynů hello vaše dodavatele toomake se, že podporují WS-Trust 1.3 nebo 2005 koncových bodů a že jsou tyto publikovaných prostřednictvím hello Metadata Exchange souboru (MEX).</span><span class="sxs-lookup"><span data-stu-id="1a73b-190">If you don’t have AD FS as your on-premises federation service, follow hello instructions of your vendor toomake sure they support WS-Trust 1.3 or 2005 end-points and that these are published through hello Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="1a73b-191">Hello následující deklarace identity musí existovat v tokenu hello přijatých Azure DRS pro toocomplete registrace zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a73b-191">hello following claims must exist in hello token received by Azure DRS for device registration toocomplete.</span></span> <span data-ttu-id="1a73b-192">Azure DRS vytvoří objekt zařízení ve službě Azure AD pomocí některé z těchto informací, které se pak použije v Azure AD Connect tooassociate hello nově vytvořený objekt zařízení hello počítači účet místní.</span><span class="sxs-lookup"><span data-stu-id="1a73b-192">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect tooassociate hello newly created device object with hello computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="1a73b-193">Pokud máte více než jeden název ověřené domény, je třeba tooprovide hello následující deklarace identity pro počítače:</span><span class="sxs-lookup"><span data-stu-id="1a73b-193">If you have more than one verified domain name, you need tooprovide hello following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="1a73b-194">Pokud jsou již vydání deklaraci identity ImmutableID (například alternativního přihlašovacího ID) je třeba tooprovide jeden odpovídající deklarace identity pro počítače:</span><span class="sxs-lookup"><span data-stu-id="1a73b-194">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need tooprovide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="1a73b-195">V následujících částech hello najdete informace o:</span><span class="sxs-lookup"><span data-stu-id="1a73b-195">In hello following sections, you find information about:</span></span>
 
- <span data-ttu-id="1a73b-196">Hello hodnoty by měla mít jednotlivých deklarací identity</span><span class="sxs-lookup"><span data-stu-id="1a73b-196">hello values each claim should have</span></span>
- <span data-ttu-id="1a73b-197">Jak bude vypadat definice ve službě AD FS</span><span class="sxs-lookup"><span data-stu-id="1a73b-197">How a definition would look like in AD FS</span></span>

<span data-ttu-id="1a73b-198">definice Hello vám pomůže tooverify, zda jsou hodnoty hello přítomen nebo pokud potřebujete toocreate je.</span><span class="sxs-lookup"><span data-stu-id="1a73b-198">hello definition helps you tooverify whether hello values are present or if you need toocreate them.</span></span>

> [!NOTE]
> <span data-ttu-id="1a73b-199">Pokud nepoužíváte místní federačního serveru služby AD FS, postupujte podle dodavatele pokyny toocreate hello odpovídající konfiguraci tooissue tyto deklarace.</span><span class="sxs-lookup"><span data-stu-id="1a73b-199">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions toocreate hello appropriate configuration tooissue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="1a73b-200">Deklarace typu účtu problém</span><span class="sxs-lookup"><span data-stu-id="1a73b-200">Issue account type claim</span></span>

<span data-ttu-id="1a73b-201">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**– Tato deklarace identity musí obsahovat hodnotu **DJ**, který identifikuje hello zařízení jako počítač připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="1a73b-201">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies hello device as a domain-joined computer.</span></span> <span data-ttu-id="1a73b-202">Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1a73b-202">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a><span data-ttu-id="1a73b-203">Problém objectGUID hello počítači účet místního</span><span class="sxs-lookup"><span data-stu-id="1a73b-203">Issue objectGUID of hello computer account on-premises</span></span>

<span data-ttu-id="1a73b-204">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**– Tato deklarace identity musí obsahovat hello **objectGUID** hodnotu hello místní účet počítače.</span><span class="sxs-lookup"><span data-stu-id="1a73b-204">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain hello **objectGUID** value of hello on-premises computer account.</span></span> <span data-ttu-id="1a73b-205">Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1a73b-205">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a><span data-ttu-id="1a73b-206">Problém objectSID hello počítači účet místního</span><span class="sxs-lookup"><span data-stu-id="1a73b-206">Issue objectSID of hello computer account on-premises</span></span>

<span data-ttu-id="1a73b-207">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**– Tato deklarace identity musí obsahovat hello hello **objectSid** hodnotu hello místní účet počítače.</span><span class="sxs-lookup"><span data-stu-id="1a73b-207">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain hello hello **objectSid** value of hello on-premises computer account.</span></span> <span data-ttu-id="1a73b-208">Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="1a73b-208">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="1a73b-209">Vystavovat issuerID pro počítač, pokud ověření více názvů domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a73b-209">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="1a73b-210">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**– Tato deklarace identity musí obsahovat hello identifikátor URI (Uniform Resource) všech hello ověřit názvy domén, které se připojují pomocí hello místní federační služby (služby AD FS nebo 3. stran) vydávající hello token.</span><span class="sxs-lookup"><span data-stu-id="1a73b-210">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain hello Uniform Resource Identifier (URI) of any of hello verified domain names that connect with hello on-premises federation service (AD FS or 3rd party) issuing hello token.</span></span> <span data-ttu-id="1a73b-211">Ve službě AD FS můžete přidat pravidla transformace vystavení, které vypadají stejně, jako hello těch, které jsou níže v tom, že ty, které konkrétní pořadí po hello výše.</span><span class="sxs-lookup"><span data-stu-id="1a73b-211">In AD FS, you can add issuance transform rules that look like hello ones below in that specific order after hello ones above.</span></span> <span data-ttu-id="1a73b-212">Všimněte si daného jedno pravidlo tooexplicitly problém hello pravidla pro uživatele je nezbytné.</span><span class="sxs-lookup"><span data-stu-id="1a73b-212">Please note that one rule tooexplicitly issue hello rule for users is necessary.</span></span> <span data-ttu-id="1a73b-213">V pravidlech hello níže se přidá první pravidlo identifikace uživatele nebo ověřování počítače.</span><span class="sxs-lookup"><span data-stu-id="1a73b-213">In hello rules below, a first rule identifying user vs. computer authentication is added.</span></span>

    @RuleName = "Issue account type with hello value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


<span data-ttu-id="1a73b-214">Ve výše uvedené, hello deklarací</span><span class="sxs-lookup"><span data-stu-id="1a73b-214">In hello claim above,</span></span>

- <span data-ttu-id="1a73b-215">`$<domain>`je adresa URL služby hello AD FS</span><span class="sxs-lookup"><span data-stu-id="1a73b-215">`$<domain>` is hello AD FS service URL</span></span>
- <span data-ttu-id="1a73b-216">`<verified-domain-name>`je zástupný symbol potřebujete tooreplace s jedním názvů ověřené domény ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a73b-216">`<verified-domain-name>` is a placeholder you need tooreplace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="1a73b-217">Další informace o názvech ověřené domény najdete v tématu [přidat tooAzure název vlastní domény služby Active Directory](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="1a73b-217">For more details about verified domain names, see [Add a custom domain name tooAzure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="1a73b-218">tooget seznam vaše společnost ověřené domény, můžete použít hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1a73b-218">tooget a list of your verified company domains, you can use hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="1a73b-220">Vystavovat ImmutableID pro počítač, pokud pro uživatele k dispozici jeden (například alternativního přihlašovacího ID, je nastavena)</span><span class="sxs-lookup"><span data-stu-id="1a73b-220">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="1a73b-221">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**– Tato deklarace identity musí obsahovat platnou hodnotu pro počítače.</span><span class="sxs-lookup"><span data-stu-id="1a73b-221">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="1a73b-222">Ve službě AD FS můžete vytvořit pravidlo pro vystavování transformace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1a73b-222">In AD FS, you can create an issuance transform rule as follows:</span></span>

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a><span data-ttu-id="1a73b-223">Pomocník skriptu toocreate hello AD FS pravidla transformace při vystavení</span><span class="sxs-lookup"><span data-stu-id="1a73b-223">Helper script toocreate hello AD FS issuance transform rules</span></span>

<span data-ttu-id="1a73b-224">Hello následující skript vám pomůže s hello vytvoření hello vystavování transformace pravidel popsaných výše.</span><span class="sxs-lookup"><span data-stu-id="1a73b-224">hello following script helps you with hello creation of hello issuance transform rules described above.</span></span>

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with hello value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a><span data-ttu-id="1a73b-225">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1a73b-225">Remarks</span></span> 

- <span data-ttu-id="1a73b-226">Tento skript připojí hello pravidla toohello existující pravidla.</span><span class="sxs-lookup"><span data-stu-id="1a73b-226">This script appends hello rules toohello existing rules.</span></span> <span data-ttu-id="1a73b-227">Nespouštějte hello skriptu dvakrát protože hello sada pravidel by byl přidán dvakrát.</span><span class="sxs-lookup"><span data-stu-id="1a73b-227">Do not run hello script twice because hello set of rules would be added twice.</span></span> <span data-ttu-id="1a73b-228">Ujistěte se, že neexistují žádná pravidla odpovídající před spuštěním skriptu hello znovu v těchto deklarací identity (v odpovídajících podmínkách hello).</span><span class="sxs-lookup"><span data-stu-id="1a73b-228">Make sure that no corresponding rules exist for these claims (under hello corresponding conditions) before running hello script again.</span></span>

- <span data-ttu-id="1a73b-229">Pokud máte více ověřených názvů domén (jak je znázorněno v portálu hello Azure AD nebo pomocí rutiny Get-MsolDomains hello), nastavte hodnotu hello **$multipleVerifiedDomainNames** v hello skriptu příliš**$true**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-229">If you have multiple verified domain names (as shown in hello Azure AD portal or via hello Get-MsolDomains cmdlet), set hello value of **$multipleVerifiedDomainNames** in hello script too**$true**.</span></span> <span data-ttu-id="1a73b-230">Také se ujistěte, že odeberete všechny existující issuerid deklaraci, která může být vytvořen přes Azure AD Connect nebo prostřednictvím jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="1a73b-230">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="1a73b-231">Tady je příklad pro toto pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1a73b-231">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="1a73b-232">Pokud jste již vystavily **ImmutableID** deklarací identity pro uživatelské účty, nastavte hodnotu hello **$immutableIDAlreadyIssuedforUsers** v hello skriptu příliš**$true**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-232">If you have already issued an **ImmutableID** claim  for user accounts, set hello value of **$immutableIDAlreadyIssuedforUsers** in hello script too**$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="1a73b-233">Krok 3: Zapnutí zařízení se systémem Windows nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="1a73b-233">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="1a73b-234">Pokud některé z vašich zařízení připojených k doméně Windows nižší úrovně zařízení, budete muset:</span><span class="sxs-lookup"><span data-stu-id="1a73b-234">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="1a73b-235">Nastavte zásady v Azure AD tooenable tooregister zařízení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1a73b-235">Set a policy in Azure AD tooenable users tooregister devices.</span></span>
 
- <span data-ttu-id="1a73b-236">Konfigurace vaší místní federační služby tooissue deklarace identity toosupport **integrované ověřování systému Windows (IWA)** pro registraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a73b-236">Configure your on-premises federation service tooissue claims toosupport **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="1a73b-237">Přidejte hello Azure AD zařízení ověřování koncový bod toohello místní intranetové zóny, který certifikát tooavoid výzvy při ověřování zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-237">Add hello Azure AD device authentication end-point toohello local Intranet zones tooavoid certificate prompts when authenticating hello device.</span></span>

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a><span data-ttu-id="1a73b-238">Nastavte zásady v Azure AD tooenable tooregister zařízení uživatelů</span><span class="sxs-lookup"><span data-stu-id="1a73b-238">Set policy in Azure AD tooenable users tooregister devices</span></span>

<span data-ttu-id="1a73b-239">tooregister zařízení se systémem Windows nižší úrovně, je nutné toomake zkontrolujte, jestli je nastavená této hello nastavení uživatelů tooallow tooregister zařízení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-239">tooregister Windows down-level devices, you need toomake sure that hello setting tooallow users tooregister devices in Azure AD is set.</span></span> <span data-ttu-id="1a73b-240">V hello portálu Azure můžete najít toto nastavení v části:</span><span class="sxs-lookup"><span data-stu-id="1a73b-240">In hello Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="1a73b-241">Hello tyto zásady musí být nastaven příliš**všechny**: **uživatelé mohou registrovat svá zařízení s Azure AD**</span><span class="sxs-lookup"><span data-stu-id="1a73b-241">hello following policy must be set too**All**: **Users may register their devices with Azure AD**</span></span>

![Registrace zařízení](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="1a73b-243">Nakonfigurujte místní služby FS</span><span class="sxs-lookup"><span data-stu-id="1a73b-243">Configure on-premises federation service</span></span> 

<span data-ttu-id="1a73b-244">Vaše místní federační služba musí podporovat vydávající hello **authenticationmehod** a **wiaormultiauthn** deklarace identity při přijímání ověření požadavku toohello Azure AD předávající strany, která uchovává resouce_params parametr s hodnotou kódovaného jak je uvedené níže:</span><span class="sxs-lookup"><span data-stu-id="1a73b-244">Your on-premises federation service must support issuing hello **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request toohello Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="1a73b-245">Pokud takový požadavek pochází, hello místní federační služby musí ověřit hello uživatele s využitím integrované ověřování systému Windows a na úspěch, vydá hello následující dvě deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="1a73b-245">When such a request comes, hello on-premises federation service must authenticate hello user using Integrated Windows Authentication and upon success, it must issue hello following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="1a73b-246">Ve službě AD FS musíte přidat tuto metodu ověřování hello předává prostřednictvím pravidel transformace pro vystavování.</span><span class="sxs-lookup"><span data-stu-id="1a73b-246">In AD FS, you must add an issuance transform rule that passes-through hello authentication method.</span></span>  

<span data-ttu-id="1a73b-247">**tooadd toto pravidlo:**</span><span class="sxs-lookup"><span data-stu-id="1a73b-247">**tooadd this rule:**</span></span>

1. <span data-ttu-id="1a73b-248">V konzole pro správu hello AD FS přejděte příliš`AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="1a73b-248">In hello AD FS management console, go too`AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="1a73b-249">Klikněte pravým tlačítkem na objekt hello vztahu důvěryhodnosti předávající strany pro aplikaci Microsoft Office 365 Identity Platform a vyberte **upravit pravidla deklarací identity**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-249">Right-click hello Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="1a73b-250">Na hello **pravidlech transformace vystavení** vyberte **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-250">On hello **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="1a73b-251">V hello **pravidlo deklarace identity** seznam šablon, vyberte **odesílat deklarace pomocí vlastního pravidla**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-251">In hello **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="1a73b-252">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-252">Select **Next**.</span></span>
6. <span data-ttu-id="1a73b-253">V hello **název pravidla deklarací** zadejte **pravidlo deklarace identity metoda ověřování**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-253">In hello **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="1a73b-254">V hello **pravidlo deklarace identity** pole, typ hello následující pravidlo:</span><span class="sxs-lookup"><span data-stu-id="1a73b-254">In hello **Claim rule** box, type hello following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="1a73b-255">Na federačním serveru, zadejte příkaz prostředí PowerShell hello níže po nahrazení  **\<RPObjectName\>**  hello přijímající strany u objektu s názvem pro vztah důvěryhodnosti objektu předávající strany služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-255">On your federation server, type hello PowerShell command below after replacing **\<RPObjectName\>** with hello relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="1a73b-256">Tento objekt obvykle jmenuje **Microsoft Office 365 Identity Platform**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-256">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a><span data-ttu-id="1a73b-257">Přidejte hello Azure AD zařízení ověřování koncový bod toohello místní Intranet zóny</span><span class="sxs-lookup"><span data-stu-id="1a73b-257">Add hello Azure AD device authentication end-point toohello Local Intranet zones</span></span>

<span data-ttu-id="1a73b-258">Při ověřování uživatele v zařízení zaregistrovat tooAzure AD můžete posouvat nastavení zásady tooyour připojená k doméně tooadd hello následující adresu URL toohello zóny místního intranetu v aplikaci Internet Explorer zobrazí výzvu tooavoid certifikátu:</span><span class="sxs-lookup"><span data-stu-id="1a73b-258">tooavoid certificate prompts when users in register devices authenticate tooAzure AD you can push a policy tooyour domain-joined devices tooadd hello following URL toohello Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="1a73b-259">Krok 4: Řízení nasazení a zavedení</span><span class="sxs-lookup"><span data-stu-id="1a73b-259">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="1a73b-260">Když jste dokončili postup hello vyžaduje, jsou připojená k doméně připravené tooautomatically připojení k Azure AD:</span><span class="sxs-lookup"><span data-stu-id="1a73b-260">When you have completed hello required steps, domain-joined devices are ready tooautomatically join Azure AD:</span></span>

- <span data-ttu-id="1a73b-261">Všechny připojené k doméně zařízení se systémem Windows 10 Anniversary Update a Windows Server 2016 automaticky zaregistrovat s Azure AD v zařízení restartujte nebo přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="1a73b-261">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> 

- <span data-ttu-id="1a73b-262">S Azure AD při restartování zařízení hello po dokončení operace připojení hello zaregistrovat nová zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a73b-262">New devices register with Azure AD when hello device restarts after hello domain join operation is completed.</span></span>

- <span data-ttu-id="1a73b-263">Registruje zařízení, které byly dříve Azure AD (například pro Intune) přechod příliš "*připojený k doméně, zaregistrováno v AAD*"; ale trvá delší dobu pro tento proces toocomplete ve všech zařízeních kvůli toohello normálního toku doména a uživatelské aktivity.</span><span class="sxs-lookup"><span data-stu-id="1a73b-263">Devices that were previously Azure AD registered (for example, for Intune) transition too“*Domain Joined, AAD Registered*”; however it takes some time for this process toocomplete across all devices due toohello normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="1a73b-264">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1a73b-264">Remarks</span></span>

- <span data-ttu-id="1a73b-265">Můžete vytvořit zásad skupiny objekt toocontrol hello zavedení automatické registrace Windows 10 a Windows Server 2016 počítačů připojených k doméně.</span><span class="sxs-lookup"><span data-stu-id="1a73b-265">You can use a Group Policy object toocontrol hello rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="1a73b-266">Windows 10. listopadu 2015 aktualizace automaticky spojí se službou Azure AD **pouze** Pokud hello zavedení nastavení objektu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="1a73b-266">Windows 10 November 2015 Update automatically joins with Azure AD **only** if hello rollout Group Policy object is set.</span></span>

- <span data-ttu-id="1a73b-267">toorollout počítačů se systémem Windows nižší úrovně, můžete nasadit [balíček služby Windows Installer](#windows-installer-packages-for-non-windows-10-computers) toocomputers, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="1a73b-267">toorollout of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) toocomputers that you select.</span></span>

- <span data-ttu-id="1a73b-268">Pokud jste push hello zásad skupiny objekt tooWindows 8.1 připojená k doméně, dojde k pokusu o spojení; nedoporučujeme ale, že používáte hello [balíček služby Windows Installer](#windows-installer-packages-for-non-windows-10-computers) toojoin všechna zařízení Windows nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="1a73b-268">If you push hello Group Policy object tooWindows 8.1 domain-joined devices, a join is attempted; however it is recommended that you use hello [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) toojoin all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="1a73b-269">Vytvoření objektu zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="1a73b-269">Create a Group Policy object</span></span> 

<span data-ttu-id="1a73b-270">zavedení hello toocontrol aktuální počítačů se systémem Windows, měli byste nasadit hello **registraci připojený k doméně počítače jako zařízení** zařízení toohello objekt zásad skupiny chcete tooregister.</span><span class="sxs-lookup"><span data-stu-id="1a73b-270">toocontrol hello rollout of Windows current computers, you should deploy hello **Register domain-joined computers as devices** Group Policy object toohello devices you want tooregister.</span></span> <span data-ttu-id="1a73b-271">Například můžete nasadit hello zásad tooan organizační jednotku nebo skupinu zabezpečení tooa.</span><span class="sxs-lookup"><span data-stu-id="1a73b-271">For example, you can deploy hello policy tooan organizational unit or tooa security group.</span></span>

<span data-ttu-id="1a73b-272">**tooset hello zásad:**</span><span class="sxs-lookup"><span data-stu-id="1a73b-272">**tooset hello policy:**</span></span>

1. <span data-ttu-id="1a73b-273">Otevřete **správce serveru**a potom přejděte příliš`Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="1a73b-273">Open **Server Manager**, and then go too`Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="1a73b-274">Přejděte toohello domény uzlu, který odpovídá toohello domény, kam chcete tooactivate automatickou registraci aktuální počítačů se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="1a73b-274">Go toohello domain node that corresponds toohello domain where you want tooactivate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="1a73b-275">Klikněte pravým tlačítkem na **objekty zásad skupiny**a potom vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-275">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="1a73b-276">Zadejte název objektu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="1a73b-276">Type a name for your Group Policy object.</span></span> <span data-ttu-id="1a73b-277">Například * hybridní Azure AD join.</span><span class="sxs-lookup"><span data-stu-id="1a73b-277">For example, *Hybrid Azure AD join.</span></span> 
5. <span data-ttu-id="1a73b-278">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-278">Click **OK**.</span></span>
6. <span data-ttu-id="1a73b-279">Klikněte pravým tlačítkem na váš nový objekt zásad skupiny a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-279">Right-click your new Group Policy object, and then select **Edit**.</span></span>
7. <span data-ttu-id="1a73b-280">Přejděte příliš**konfigurace počítače** > **zásady** > **šablony pro správu** > **Windows Součásti** > **registrace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-280">Go too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> 
8. <span data-ttu-id="1a73b-281">Klikněte pravým tlačítkem na **registraci připojený k doméně počítače jako zařízení**a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-281">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1a73b-282">Tato šablona zásad skupiny byla přejmenována z dřívějších verzí konzoly pro správu zásad skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-282">This Group Policy template has been renamed from earlier versions of hello Group Policy Management console.</span></span> <span data-ttu-id="1a73b-283">Pokud používáte starší verzi konzoly hello, přejděte příliš`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="1a73b-283">If you are using an earlier version of hello console, go too`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="1a73b-284">Vyberte **povoleno**a potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-284">Select **Enabled**, and then click **Apply**.</span></span>
8. <span data-ttu-id="1a73b-285">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-285">Click **OK**.</span></span>
9. <span data-ttu-id="1a73b-286">Odkaz hello tooa umístění objektu zásad skupiny podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="1a73b-286">Link hello Group Policy object tooa location of your choice.</span></span> <span data-ttu-id="1a73b-287">Například můžete propojit jej tooa určité organizační jednotce.</span><span class="sxs-lookup"><span data-stu-id="1a73b-287">For example, you can link it tooa specific organizational unit.</span></span> <span data-ttu-id="1a73b-288">Můžete také propojit ho tooa konkrétní bezpečnostní skupiny počítačů, které automaticky připojit k službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-288">You also could link it tooa specific security group of computers that automatically join with Azure AD.</span></span> <span data-ttu-id="1a73b-289">tooset tuto zásadu pro všechny připojené k doméně Windows 10 a Windows Server 2016 počítače ve vaší organizaci, odkaz hello zásad skupiny objekt toohello domény.</span><span class="sxs-lookup"><span data-stu-id="1a73b-289">tooset this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link hello Group Policy object toohello domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="1a73b-290">Balíčky Instalační služby systému Windows pro počítače s Windows 10</span><span class="sxs-lookup"><span data-stu-id="1a73b-290">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="1a73b-291">toojoin připojený k doméně nižší úrovně počítače se systémem Windows ve federovaném prostředí, můžete stáhnout a nainstalovat tyto balíček Instalační služby systému Windows (.msi) ze služby Stažení softwaru na hello [připojení k pracovní ploše Microsoft pro počítače s Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=53554)stránky.</span><span class="sxs-lookup"><span data-stu-id="1a73b-291">toojoin domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at hello [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="1a73b-292">Balíček hello můžete nasadit pomocí systém distribuce softwaru jako je System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="1a73b-292">You can deploy hello package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="1a73b-293">Hello balíček podporuje možnosti standardní bezobslužnou instalaci hello hello *quiet* parametr.</span><span class="sxs-lookup"><span data-stu-id="1a73b-293">hello package supports hello standard silent install options with hello *quiet* parameter.</span></span> <span data-ttu-id="1a73b-294">System Center Configuration Manager aktuální větev nabízí další výhody z dřívějších verzí, jako je hello možnost tootrack dokončení registrace.</span><span class="sxs-lookup"><span data-stu-id="1a73b-294">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like hello ability tootrack completed registrations.</span></span> <span data-ttu-id="1a73b-295">Další informace najdete v tématu [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="1a73b-295">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="1a73b-296">Instalační program Hello vytvoří naplánovanou úlohu na hello systému, který běží v kontextu uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-296">hello installer creates a scheduled task on hello system that runs in hello user’s context.</span></span> <span data-ttu-id="1a73b-297">Při přihlášení uživatele hello tooWindows, aktivuje se úloha Hello.</span><span class="sxs-lookup"><span data-stu-id="1a73b-297">hello task is triggered when hello user signs in tooWindows.</span></span> <span data-ttu-id="1a73b-298">Úloha Hello bezobslužně spojí hello zařízení s Azure AD s pověřeními uživatele hello po ověřování pomocí integrovaného ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="1a73b-298">hello task silently joins hello device with Azure AD with hello user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="1a73b-299">toosee hello naplánované úlohy, v zařízení hello přejděte příliš**Microsoft** > **k síti na pracovišti**, a potom přejděte toohello Knihovna plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="1a73b-299">toosee hello scheduled task, in hello device, go too**Microsoft** > **Workplace Join**, and then go toohello Task Scheduler library.</span></span>

## <a name="step-5-verify-joined-devices"></a><span data-ttu-id="1a73b-300">Krok 5: Ověření připojeného k zařízení</span><span class="sxs-lookup"><span data-stu-id="1a73b-300">Step 5: Verify joined devices</span></span>

<span data-ttu-id="1a73b-301">Úspěšné připojeného k zařízení ve vaší organizaci můžete zkontrolovat pomocí hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) rutiny v hello [modul Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="1a73b-301">You can check successful joined devices in your organization by using hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in hello [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="1a73b-302">Hello výstup této rutiny zobrazuje zařízení, které jsou zaregistrované a propojit s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a73b-302">hello output of this cmdlet shows devices that are registered and joined with Azure AD.</span></span> <span data-ttu-id="1a73b-303">tooget všechna zařízení, použijte hello **– všechny** parametr a potom filtru je pomocí hello **deviceTrustType** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1a73b-303">tooget all devices, use hello **-All** parameter, and then filter them using hello **deviceTrustType** property.</span></span> <span data-ttu-id="1a73b-304">Připojené k doméně zařízení mít hodnotu **připojený k doméně**.</span><span class="sxs-lookup"><span data-stu-id="1a73b-304">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a73b-305">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a73b-305">Next steps</span></span>

* [<span data-ttu-id="1a73b-306">Úvod toodevice správy ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a73b-306">Introduction toodevice management in Azure Active Directory</span></span>](device-management-introduction.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
