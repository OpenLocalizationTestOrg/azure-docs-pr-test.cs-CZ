---
title: "Jak nakonfigurovat hybridní připojená k Azure Active Directory zařízení | Microsoft Docs"
description: "Naučte se konfigurovat hybridní Azure Active Directory připojené zařízení."
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
ms.openlocfilehash: 4580075df9fce74664b22aa24065ba1885692384
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-configure-hybrid-azure-active-directory-joined-devices"></a><span data-ttu-id="edda8-103">Postup konfigurace hybridní Azure Active Directory připojené zařízení</span><span class="sxs-lookup"><span data-stu-id="edda8-103">How to configure hybrid Azure Active Directory joined devices</span></span>

<span data-ttu-id="edda8-104">Se správou zařízení ve službě Azure Active Directory (Azure AD) můžete zajistit, že vaši uživatelé přistupují k prostředkům ze zařízení, která splňují vaše standardy zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="edda8-104">With device management in Azure Active Directory (Azure AD), you can ensure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> <span data-ttu-id="edda8-105">Další podrobnosti najdete v tématu [Úvod do správy zařízení v Azure Active Directory](device-management-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="edda8-105">For more details, see [Introduction to device management in Azure Active Directory](device-management-introduction.md).</span></span>

<span data-ttu-id="edda8-106">Pokud máte prostředí místní služby Active Directory a chcete pro připojení zařízení připojených k doméně ke službě Azure AD, můžete k tomu lze konfigurace hybridní Azure AD, které jsou připojené k zařízení.</span><span class="sxs-lookup"><span data-stu-id="edda8-106">If you have an on-premises Active Directory environment and you want to join your domain-joined devices to Azure AD, you can accomplish this by configuring hybrid Azure AD joined devices.</span></span> <span data-ttu-id="edda8-107">Téma vám poskytne související kroky.</span><span class="sxs-lookup"><span data-stu-id="edda8-107">The topic provides you with the related steps.</span></span> 


## <a name="before-you-begin"></a><span data-ttu-id="edda8-108">Než začnete</span><span class="sxs-lookup"><span data-stu-id="edda8-108">Before you begin</span></span>

<span data-ttu-id="edda8-109">Před zahájením konfigurace zařízení služby Azure AD, které jsou připojené k hybridní ve vašem prostředí, by měl Seznamte se s Podporované scénáře a omezení.</span><span class="sxs-lookup"><span data-stu-id="edda8-109">Before you start configuring hybrid Azure AD joined devices in your environment, you should familiarize yourself with the supported scenarios and the constraints.</span></span>  

<span data-ttu-id="edda8-110">Toto téma ke zlepšení čitelnosti popisy, používá následující období:</span><span class="sxs-lookup"><span data-stu-id="edda8-110">To improve the readability of the descriptions, this topic uses the following term:</span></span> 

- <span data-ttu-id="edda8-111">**Aktuální zařízení se systémem Windows** -tento termín se vztahuje k doméně, zařízení se systémem Windows 10 nebo Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="edda8-111">**Windows current devices** - This term refers to domain-joined devices running Windows 10 or Windows Server 2016.</span></span>
- <span data-ttu-id="edda8-112">**Zařízení se systémem Windows nižší úrovně** -tento termín se vztahuje na všechny **podporované** připojený k doméně zařízení Windows, která nejsou spuštěné Windows 10 ani systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="edda8-112">**Windows down-level devices** - This term refers to all **supported** domain-joined Windows devices that are neither running Windows 10 nor Windows Server 2016.</span></span>  


### <a name="windows-current-devices"></a><span data-ttu-id="edda8-113">Aktuální zařízení se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="edda8-113">Windows current devices</span></span>

- <span data-ttu-id="edda8-114">Pro zařízení se systémem Windows desktop operačního systému, doporučujeme používat Windows 10 Anniversary Update (verze 1607) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="edda8-114">For devices running the Windows desktop operating system, we recommend using Windows 10 Anniversary Update (version 1607) or later.</span></span> 
- <span data-ttu-id="edda8-115">Registrace zařízení se systémem Windows aktuální **je** podporované v nefederované prostředích, jako je konfigurace synchronizace hodnoty hash hesla.</span><span class="sxs-lookup"><span data-stu-id="edda8-115">The registration of Windows current devices **is** supported in non-federated environments such as password hash sync configurations.</span></span>  


### <a name="windows-down-level-devices"></a><span data-ttu-id="edda8-116">Zařízení se systémem Windows nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="edda8-116">Windows down-level devices</span></span>

- <span data-ttu-id="edda8-117">Jsou podporovány následující zařízení se systémem Windows nižší úrovně:</span><span class="sxs-lookup"><span data-stu-id="edda8-117">The following Windows down-level devices are supported:</span></span>
    - <span data-ttu-id="edda8-118">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="edda8-118">Windows 8.1</span></span>
    - <span data-ttu-id="edda8-119">Windows 7</span><span class="sxs-lookup"><span data-stu-id="edda8-119">Windows 7</span></span>
    - <span data-ttu-id="edda8-120">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="edda8-120">Windows Server 2012 R2</span></span>
    - <span data-ttu-id="edda8-121">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="edda8-121">Windows Server 2012</span></span>
    - <span data-ttu-id="edda8-122">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="edda8-122">Windows Server 2008 R2</span></span>
- <span data-ttu-id="edda8-123">Registrace zařízení se systémem Windows nižší úrovně **je** podporované v prostředích nefederovaných prostřednictvím bezproblémové jednotné přihlašování [Azure Active Directory bezproblémové jednotné přihlašování](https://aka.ms/hybrid/sso).</span><span class="sxs-lookup"><span data-stu-id="edda8-123">The registration of Windows down-level devices **is** supported in non-federated environments through Seamless Single Sign On [Azure Active Directory Seamless Single Sign-On](https://aka.ms/hybrid/sso).</span></span>
- <span data-ttu-id="edda8-124">Registrace zařízení se systémem Windows nižší úrovně **není** podporovaná pro zařízení s použitím profilů roamingu.</span><span class="sxs-lookup"><span data-stu-id="edda8-124">The registration of Windows down-level devices **is not** supported for devices using roaming profiles.</span></span> <span data-ttu-id="edda8-125">Pokud se spoléháte na cestovních profilů nebo nastavení, používají systém Windows 10.</span><span class="sxs-lookup"><span data-stu-id="edda8-125">If you are relying on roaming of profiles or settings, use Windows 10.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="edda8-126">Požadavky</span><span class="sxs-lookup"><span data-stu-id="edda8-126">Prerequisites</span></span>

<span data-ttu-id="edda8-127">Než začnete, povolení hybridního Azure AD, které jsou připojené k zařízení ve vaší organizaci, musíte zajistit, že používáte aktuální verzi Azure AD connect.</span><span class="sxs-lookup"><span data-stu-id="edda8-127">Before you start enabling hybrid Azure AD joined devices in your organization, you need to make sure that you are running an up-to-date version of Azure AD connect.</span></span>

<span data-ttu-id="edda8-128">Azure AD Connect:</span><span class="sxs-lookup"><span data-stu-id="edda8-128">Azure AD Connect:</span></span>

- <span data-ttu-id="edda8-129">Zachová přidružení mezi účet počítače v místní službě Active Directory (AD) a objektu zařízení ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edda8-129">Keeps the association between the computer account in your on-premises Active Directory (AD) and the device object in Azure AD.</span></span> 
- <span data-ttu-id="edda8-130">Povoluje další zařízení související s funkcí, jako Windows Hello pro firmy.</span><span class="sxs-lookup"><span data-stu-id="edda8-130">Enables other device related features like Windows Hello for Business.</span></span>



## <a name="configuration-steps"></a><span data-ttu-id="edda8-131">Kroky konfigurace</span><span class="sxs-lookup"><span data-stu-id="edda8-131">Configuration steps</span></span>

<span data-ttu-id="edda8-132">Můžete nakonfigurovat hybridní zařízení připojených k Azure AD pro různé typy zařízení platformy systému Windows.</span><span class="sxs-lookup"><span data-stu-id="edda8-132">You can configure hybrid Azure AD joined devices for various types of Windows device platforms.</span></span> <span data-ttu-id="edda8-133">Toto téma obsahuje požadované kroky pro všechny scénáře obvyklá konfigurace.</span><span class="sxs-lookup"><span data-stu-id="edda8-133">This topic includes the required steps for all typical configuration scenarios.</span></span>  

<span data-ttu-id="edda8-134">Následující tabulku použijte k získání přehledu kroky, které jsou požadovány pro váš scénář:</span><span class="sxs-lookup"><span data-stu-id="edda8-134">Use the following table to get an overview of the steps that are required for your scenario:</span></span>  



| <span data-ttu-id="edda8-135">Kroky</span><span class="sxs-lookup"><span data-stu-id="edda8-135">Steps</span></span>                                      | <span data-ttu-id="edda8-136">Synchronizace hodnot hash aktuální a heslo systému Windows</span><span class="sxs-lookup"><span data-stu-id="edda8-136">Windows current and password hash sync</span></span> | <span data-ttu-id="edda8-137">Aktuální Windows a federation</span><span class="sxs-lookup"><span data-stu-id="edda8-137">Windows current and federation</span></span> | <span data-ttu-id="edda8-138">Windows nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="edda8-138">Windows down-level</span></span> |
| :--                                        | :-:                                    | :-:                            | :-:                |
| <span data-ttu-id="edda8-139">Krok 1: Konfigurace spojovacího bodu služby</span><span class="sxs-lookup"><span data-stu-id="edda8-139">Step 1: Configure service connection point</span></span> | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| <span data-ttu-id="edda8-143">Krok 2: Nastavení vystavování deklarací identity</span><span class="sxs-lookup"><span data-stu-id="edda8-143">Step 2: Setup issuance of claims</span></span>           |                                        | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| <span data-ttu-id="edda8-146">Krok 3: Zapnutí zařízení s Windows 10</span><span class="sxs-lookup"><span data-stu-id="edda8-146">Step 3: Enable non-Windows 10 devices</span></span>      |                                        |                                | ![Zaškrtnout][1]        |
| <span data-ttu-id="edda8-148">Krok 4: Řízení nasazení a zavedení</span><span class="sxs-lookup"><span data-stu-id="edda8-148">Step 4: Control deployment and rollout</span></span>     | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |
| <span data-ttu-id="edda8-152">Krok 5: Ověření připojeného k zařízení</span><span class="sxs-lookup"><span data-stu-id="edda8-152">Step 5: Verify joined devices</span></span>          | ![Zaškrtnout][1]                            | ![Zaškrtnout][1]                    | ![Zaškrtnout][1]        |



## <a name="step-1-configure-service-connection-point"></a><span data-ttu-id="edda8-156">Krok 1: Konfigurace spojovacího bodu služby</span><span class="sxs-lookup"><span data-stu-id="edda8-156">Step 1: Configure service connection point</span></span>

<span data-ttu-id="edda8-157">Objekt připojení Spojovací bod služby se používá vaše zařízení během registrace ke zjišťování informace klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edda8-157">The service connection point (SCP) object is used by your devices during the registration to discover Azure AD tenant information.</span></span> <span data-ttu-id="edda8-158">Ve vaší místní služby Active Directory (AD) musí existovat objekt spojovací bod služby pro zařízení Azure AD, které jsou připojené k hybridní v kontextu názvové počítače doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="edda8-158">In your on-premises Active Directory (AD), the SCP object for the hybrid Azure AD joined devices must exist in the configuration naming context partition of the computer's forest.</span></span> <span data-ttu-id="edda8-159">Není k dispozici pouze jeden názvovém kontextu konfigurace pro každou doménovou strukturu.</span><span class="sxs-lookup"><span data-stu-id="edda8-159">There is only one configuration naming context per forest.</span></span> <span data-ttu-id="edda8-160">V konfiguraci s více doménovými strukturami služby Active Directory musí existovat spojovací bod služby ve všech doménových strukturách obsahující počítače, připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="edda8-160">In a multi-forest Active Directory configuration, the service connection point must exist in all forests containing domain-joined computers.</span></span>

<span data-ttu-id="edda8-161">Můžete použít [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) rutiny načíst názvovém kontextu konfigurace doménové struktury.</span><span class="sxs-lookup"><span data-stu-id="edda8-161">You can use the [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet to retrieve the configuration naming context of your forest.</span></span>  

<span data-ttu-id="edda8-162">Pro doménovou strukturu s názvem domény služby Active Directory *fabrikam.com*, je v názvovém kontextu konfigurace:</span><span class="sxs-lookup"><span data-stu-id="edda8-162">For a forest with the Active Directory domain name *fabrikam.com*, the configuration naming context is:</span></span>

`CN=Configuration,DC=fabrikam,DC=com`

<span data-ttu-id="edda8-163">V doménové struktuře se nachází v objektu spojovací bod služby pro automatickou registraci zařízení připojená k doméně:</span><span class="sxs-lookup"><span data-stu-id="edda8-163">In your forest, the SCP object for the auto-registration of domain-joined devices is located at:</span></span>  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

<span data-ttu-id="edda8-164">V závislosti na tom, jak jste nasadili Azure AD Connect objekt spojovací bod služby může mít již byla konfigurována.</span><span class="sxs-lookup"><span data-stu-id="edda8-164">Depending on how you have deployed Azure AD Connect, the SCP object may have already been configured.</span></span>
<span data-ttu-id="edda8-165">Můžete ověřit existenci objekt a načíst hodnoty zjišťování pomocí následujícího skriptu prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="edda8-165">You can verify the existence of the object and retrieve the discovery values using the following Windows PowerShell script:</span></span> 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

<span data-ttu-id="edda8-166">**$Scp. Klíčová slova** výstup zobrazuje informace o klientovi Azure AD, například:</span><span class="sxs-lookup"><span data-stu-id="edda8-166">The **$scp.Keywords** output shows the Azure AD tenant information, for example:</span></span>

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

<span data-ttu-id="edda8-167">Pokud spojovací bod služby neexistuje, můžete ho vytvořit spuštěním `Initialize-ADSyncDomainJoinedComputerSync` rutiny na serveru Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="edda8-167">If the service connection point does not exist, you can create it by running the `Initialize-ADSyncDomainJoinedComputerSync` cmdlet on your Azure AD Connect server.</span></span> <span data-ttu-id="edda8-168">Ke spuštění této rutiny je vyžadováno přihlašovací údaje podnikového správce.</span><span class="sxs-lookup"><span data-stu-id="edda8-168">Enterprise admin credential is required to run this cmdlet.</span></span>  
<span data-ttu-id="edda8-169">Rutinu:</span><span class="sxs-lookup"><span data-stu-id="edda8-169">The cmdlet:</span></span>

- <span data-ttu-id="edda8-170">Vytvoří spojovací bod služby v doménové struktuře služby Active Directory, Azure AD Connect je připojena k.</span><span class="sxs-lookup"><span data-stu-id="edda8-170">Creates the service connection point in the Active Directory forest Azure AD Connect is connected to.</span></span> 
- <span data-ttu-id="edda8-171">Vyžaduje, abyste zadali `AdConnectorAccount` parametr.</span><span class="sxs-lookup"><span data-stu-id="edda8-171">Requires you to specify the `AdConnectorAccount` parameter.</span></span> <span data-ttu-id="edda8-172">Toto je účet, který je nakonfigurovaný jako účet konektoru ve službě Azure AD connect služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="edda8-172">This is the account that is configured as Active Directory connector account in Azure AD connect.</span></span> 


<span data-ttu-id="edda8-173">Tento skript je ukázkou příklad, pomocí rutiny.</span><span class="sxs-lookup"><span data-stu-id="edda8-173">The following script shows an example for using the cmdlet.</span></span> <span data-ttu-id="edda8-174">V tento skript `$aadAdminCred = Get-Credential` vyžaduje, abyste zadejte uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="edda8-174">In this script, `$aadAdminCred = Get-Credential` requires you to type a user name.</span></span> <span data-ttu-id="edda8-175">Je třeba zadat uživatelské jméno ve formátu hlavního názvu (UPN) uživatele (`user@example.com`).</span><span class="sxs-lookup"><span data-stu-id="edda8-175">You need to provide the user name in the user principal name (UPN) format (`user@example.com`).</span></span> 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

<span data-ttu-id="edda8-176">`Initialize-ADSyncDomainJoinedComputerSync` Rutiny:</span><span class="sxs-lookup"><span data-stu-id="edda8-176">The `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:</span></span>

- <span data-ttu-id="edda8-177">Používá modul Active Directory PowerShell, které závisí na službě Active Directory Web Services spuštěných na řadiči domény.</span><span class="sxs-lookup"><span data-stu-id="edda8-177">Uses the Active Directory PowerShell module, which relies on Active Directory Web Services running on a domain controller.</span></span> <span data-ttu-id="edda8-178">Služba Active Directory Web Services je podporována v řadičích domény se systémem Windows Server 2008 R2 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="edda8-178">Active Directory Web Services is supported on domain controllers running Windows Server 2008 R2 and later.</span></span>
- <span data-ttu-id="edda8-179">Je podporován pouze serverem **MSOnline PowerShell verze modulu 1.1.166.0**.</span><span class="sxs-lookup"><span data-stu-id="edda8-179">Is only supported by the **MSOnline PowerShell module version 1.1.166.0**.</span></span> <span data-ttu-id="edda8-180">Chcete-li stáhnout tento modul, použijte toto [odkaz](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span><span class="sxs-lookup"><span data-stu-id="edda8-180">To download this module, use this [link](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).</span></span>   

<span data-ttu-id="edda8-181">Pro řadiče domény se systémem Windows Server 2008 nebo dřívějších verzí použijte níže uvedeném skriptu k vytvoření spojovací bod služby.</span><span class="sxs-lookup"><span data-stu-id="edda8-181">For domain controllers running Windows Server 2008 or earlier versions, use the script below to create the service connection point.</span></span>

<span data-ttu-id="edda8-182">V konfiguraci s více doménovými strukturami používejte následující skript k vytvoření spojovací bod služby v každé doménové struktuře, kde existují počítače:</span><span class="sxs-lookup"><span data-stu-id="edda8-182">In a multi-forest configuration, you should use the following script to create the service connection point in each forest where computers exist:</span></span>
 
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


## <a name="step-2-setup-issuance-of-claims"></a><span data-ttu-id="edda8-183">Krok 2: Nastavení vystavování deklarací identity</span><span class="sxs-lookup"><span data-stu-id="edda8-183">Step 2: Setup issuance of claims</span></span>

<span data-ttu-id="edda8-184">V federované Azure AD konfigurace zařízení závisí na službě Active Directory Federation Services (AD FS) nebo 3. stran místní služby FS k ověření do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edda8-184">In a federated Azure AD configuration, devices rely on Active Directory Federation Services (AD FS) or a 3rd party on-premises federation service to authenticate to Azure AD.</span></span> <span data-ttu-id="edda8-185">Zařízení ověření získání přístupového tokenu k registraci proti Azure Active Directory Device Registration Service (Azure DRS).</span><span class="sxs-lookup"><span data-stu-id="edda8-185">Devices authenticate to get an access token to register against the Azure Active Directory Device Registration Service (Azure DRS).</span></span>

<span data-ttu-id="edda8-186">Windows, které aktuální zařízení ověřování pomocí integrovaného ověřování systému Windows na aktivní koncový bod WS-Trust (verze 1.3 nebo 2005) hostované službou federation service na místě.</span><span class="sxs-lookup"><span data-stu-id="edda8-186">Windows current devices authenticate using Integrated Windows Authentication to an active WS-Trust endpoint (either 1.3 or 2005 versions) hosted by the on-premises federation service.</span></span>

> [!NOTE]
> <span data-ttu-id="edda8-187">Při použití služby AD FS buď **adfs/services/vztah důvěryhodnosti nebo 13 nebo windowstransport** nebo **služby AD FS nebo služby nebo vztahu důvěryhodnosti/2005/windowstransport** musí být povolena.</span><span class="sxs-lookup"><span data-stu-id="edda8-187">When using AD FS, either **adfs/services/trust/13/windowstransport** or **adfs/services/trust/2005/windowstransport** must be enabled.</span></span> <span data-ttu-id="edda8-188">Pokud používáte webový proxy server ověřování, ujistěte se také, že tento koncový bod publikovaný prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="edda8-188">If you are using the Web Authentication Proxy, also ensure that this endpoint is published through the proxy.</span></span> <span data-ttu-id="edda8-189">Uvidíte, jaké koncových bodů jsou povolené prostřednictvím konzoly pro správu služby AD FS v části **služby > Koncové body**.</span><span class="sxs-lookup"><span data-stu-id="edda8-189">You can see what end-points are enabled through the AD FS management console under **Service > Endpoints**.</span></span>
>
><span data-ttu-id="edda8-190">Pokud nemáte jako místní federační služby AD FS, postupujte podle pokynů dodavatele a ujistěte se, že podporují WS-Trust 1.3 nebo 2005 koncových bodů a že jsou tyto publikovaných prostřednictvím soubor metadat Exchange (MEX).</span><span class="sxs-lookup"><span data-stu-id="edda8-190">If you don’t have AD FS as your on-premises federation service, follow the instructions of your vendor to make sure they support WS-Trust 1.3 or 2005 end-points and that these are published through the Metadata Exchange file (MEX).</span></span>

<span data-ttu-id="edda8-191">Následující deklarace identity musí existovat v tokenu přijatých Azure DRS pro registraci zařízení pro dokončení.</span><span class="sxs-lookup"><span data-stu-id="edda8-191">The following claims must exist in the token received by Azure DRS for device registration to complete.</span></span> <span data-ttu-id="edda8-192">Azure DRS vytvoří objekt zařízení ve službě Azure AD pomocí některé z těchto informací, které se pak použije přes Azure AD Connect k objektu nově vytvořený zařízení přidružit počítači účet místní.</span><span class="sxs-lookup"><span data-stu-id="edda8-192">Azure DRS will create a device object in Azure AD with some of this information which is then used by Azure AD Connect to associate the newly created device object with the computer account on-premises.</span></span>

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

<span data-ttu-id="edda8-193">Pokud máte více než jeden název ověřené domény, budete muset zadat následující deklarace identity pro počítače:</span><span class="sxs-lookup"><span data-stu-id="edda8-193">If you have more than one verified domain name, you need to provide the following claim for computers:</span></span>

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

<span data-ttu-id="edda8-194">Pokud jsou již vydání deklaraci identity ImmutableID (například alternativního přihlašovacího ID), budete muset zadat jeden odpovídající deklarace identity pro počítače:</span><span class="sxs-lookup"><span data-stu-id="edda8-194">If you are already issuing an ImmutableID claim (e.g., alternate login ID) you need to provide one corresponding claim for computers:</span></span>

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

<span data-ttu-id="edda8-195">V následujících částech najdete informace o:</span><span class="sxs-lookup"><span data-stu-id="edda8-195">In the following sections, you find information about:</span></span>
 
- <span data-ttu-id="edda8-196">Hodnoty, které by měly mít jednotlivých deklarací identity</span><span class="sxs-lookup"><span data-stu-id="edda8-196">The values each claim should have</span></span>
- <span data-ttu-id="edda8-197">Jak bude vypadat definice ve službě AD FS</span><span class="sxs-lookup"><span data-stu-id="edda8-197">How a definition would look like in AD FS</span></span>

<span data-ttu-id="edda8-198">Definice pomáhá ověřte, zda jsou hodnoty přítomen nebo pokud potřebujete k jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="edda8-198">The definition helps you to verify whether the values are present or if you need to create them.</span></span>

> [!NOTE]
> <span data-ttu-id="edda8-199">Pokud nepoužíváte místní federačního serveru služby AD FS, postupujte podle pokynů od dodavatele vytvořit odpovídající konfiguraci vystavovat tyto deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="edda8-199">If you don’t use AD FS for your on-premises federation server, follow your vendor's instructions to create the appropriate configuration to issue these claims.</span></span>

### <a name="issue-account-type-claim"></a><span data-ttu-id="edda8-200">Deklarace typu účtu problém</span><span class="sxs-lookup"><span data-stu-id="edda8-200">Issue account type claim</span></span>

<span data-ttu-id="edda8-201">**`http://schemas.microsoft.com/ws/2012/01/accounttype`**– Tato deklarace identity musí obsahovat hodnotu **DJ**, které identifikují zařízení jako počítač připojený k doméně.</span><span class="sxs-lookup"><span data-stu-id="edda8-201">**`http://schemas.microsoft.com/ws/2012/01/accounttype`** - This claim must contain a value of **DJ**, which identifies the device as a domain-joined computer.</span></span> <span data-ttu-id="edda8-202">Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="edda8-202">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a><span data-ttu-id="edda8-203">Problém objectGUID na počítači účet místního</span><span class="sxs-lookup"><span data-stu-id="edda8-203">Issue objectGUID of the computer account on-premises</span></span>

<span data-ttu-id="edda8-204">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**– Tato deklarace identity musí obsahovat **objectGUID** hodnota účtu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="edda8-204">**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`** - This claim must contain the **objectGUID** value of the on-premises computer account.</span></span> <span data-ttu-id="edda8-205">Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="edda8-205">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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
 
### <a name="issue-objectsid-of-the-computer-account-on-premises"></a><span data-ttu-id="edda8-206">Problém objectSID na počítači účet místního</span><span class="sxs-lookup"><span data-stu-id="edda8-206">Issue objectSID of the computer account on-premises</span></span>

<span data-ttu-id="edda8-207">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**– Tato deklarace identity musí obsahovat **objectSid** hodnota účtu místního počítače.</span><span class="sxs-lookup"><span data-stu-id="edda8-207">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`** - This claim must contain the the **objectSid** value of the on-premises computer account.</span></span> <span data-ttu-id="edda8-208">Ve službě AD FS můžete přidat pravidlo transformace vystavování, které vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="edda8-208">In AD FS, you can add an issuance transform rule that looks like this:</span></span>

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

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a><span data-ttu-id="edda8-209">Vystavovat issuerID pro počítač, pokud ověření více názvů domén ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="edda8-209">Issue issuerID for computer when multiple verified domain names in Azure AD</span></span>

<span data-ttu-id="edda8-210">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**– Tato deklarace identity musí obsahovat identifikátor URI (Uniform Resource) všech názvy ověřené domény, které se připojují pomocí místní služby federation service (AD FS nebo 3. stran) vydání tokenu.</span><span class="sxs-lookup"><span data-stu-id="edda8-210">**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`** - This claim must contain the Uniform Resource Identifier (URI) of any of the verified domain names that connect with the on-premises federation service (AD FS or 3rd party) issuing the token.</span></span> <span data-ttu-id="edda8-211">Ve službě AD FS můžete přidat pravidla transformace vystavení, které vypadají jako jsou níže v tomto konkrétní pořadí po ty výše.</span><span class="sxs-lookup"><span data-stu-id="edda8-211">In AD FS, you can add issuance transform rules that look like the ones below in that specific order after the ones above.</span></span> <span data-ttu-id="edda8-212">Upozorňujeme, že je nutné tento jedno pravidlo explicitně vystavit pravidlo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="edda8-212">Please note that one rule to explicitly issue the rule for users is necessary.</span></span> <span data-ttu-id="edda8-213">V pravidlech níže se přidá první pravidlo identifikace uživatele nebo ověřování počítače.</span><span class="sxs-lookup"><span data-stu-id="edda8-213">In the rules below, a first rule identifying user vs. computer authentication is added.</span></span>

    @RuleName = "Issue account type with the value User when its not a computer"
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
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
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


<span data-ttu-id="edda8-214">Ve výše uvedené, deklarace identity</span><span class="sxs-lookup"><span data-stu-id="edda8-214">In the claim above,</span></span>

- <span data-ttu-id="edda8-215">`$<domain>`je adresa URL služby AD FS</span><span class="sxs-lookup"><span data-stu-id="edda8-215">`$<domain>` is the AD FS service URL</span></span>
- <span data-ttu-id="edda8-216">`<verified-domain-name>`je zástupný symbol, který potřebujete nahradit s jedním názvů ověřené domény ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="edda8-216">`<verified-domain-name>` is a placeholder you need to replace with one of your verified domain names in Azure AD</span></span>



<span data-ttu-id="edda8-217">Další informace o názvech ověřené domény najdete v tématu [přidání vlastního názvu domény do Azure Active Directory](active-directory-add-domain.md).</span><span class="sxs-lookup"><span data-stu-id="edda8-217">For more details about verified domain names, see [Add a custom domain name to Azure Active Directory](active-directory-add-domain.md).</span></span>  
<span data-ttu-id="edda8-218">Chcete-li získat seznam ověřených společnosti domény, můžete použít [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) rutiny.</span><span class="sxs-lookup"><span data-stu-id="edda8-218">To get a list of your verified company domains, you can use the [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet.</span></span> 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a><span data-ttu-id="edda8-220">Vystavovat ImmutableID pro počítač, pokud pro uživatele k dispozici jeden (například alternativního přihlašovacího ID, je nastavena)</span><span class="sxs-lookup"><span data-stu-id="edda8-220">Issue ImmutableID for computer when one for users exist (e.g. alternate login ID is set)</span></span>

<span data-ttu-id="edda8-221">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**– Tato deklarace identity musí obsahovat platnou hodnotu pro počítače.</span><span class="sxs-lookup"><span data-stu-id="edda8-221">**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`** - This claim must contain a valid value for computers.</span></span> <span data-ttu-id="edda8-222">Ve službě AD FS můžete vytvořit pravidlo pro vystavování transformace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="edda8-222">In AD FS, you can create an issuance transform rule as follows:</span></span>

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

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a><span data-ttu-id="edda8-223">Pomocník skript pro vytvoření pravidel transformace pro vystavení služby AD FS</span><span class="sxs-lookup"><span data-stu-id="edda8-223">Helper script to create the AD FS issuance transform rules</span></span>

<span data-ttu-id="edda8-224">Následující skript pomáhá s vytvoření vystavení transformace pravidel popsaných výše.</span><span class="sxs-lookup"><span data-stu-id="edda8-224">The following script helps you with the creation of the issuance transform rules described above.</span></span>

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
    $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
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
    
    @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
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

### <a name="remarks"></a><span data-ttu-id="edda8-225">Poznámky</span><span class="sxs-lookup"><span data-stu-id="edda8-225">Remarks</span></span> 

- <span data-ttu-id="edda8-226">Tento skript připojí k existující pravidla pravidla.</span><span class="sxs-lookup"><span data-stu-id="edda8-226">This script appends the rules to the existing rules.</span></span> <span data-ttu-id="edda8-227">Nelze spustit skript dvakrát vzhledem k tomu, že sadu pravidel, které by byl přidán dvakrát.</span><span class="sxs-lookup"><span data-stu-id="edda8-227">Do not run the script twice because the set of rules would be added twice.</span></span> <span data-ttu-id="edda8-228">Ujistěte se, že neexistují žádná pravidla odpovídající před spuštěním skriptu znovu v těchto deklarací identity (v odpovídajících podmínkách).</span><span class="sxs-lookup"><span data-stu-id="edda8-228">Make sure that no corresponding rules exist for these claims (under the corresponding conditions) before running the script again.</span></span>

- <span data-ttu-id="edda8-229">Pokud máte více názvů ověřené domény (jak je znázorněno na portálu Azure AD nebo pomocí rutiny Get-MsolDomains), nastavte hodnotu **$multipleVerifiedDomainNames** ve skriptu **$true**.</span><span class="sxs-lookup"><span data-stu-id="edda8-229">If you have multiple verified domain names (as shown in the Azure AD portal or via the Get-MsolDomains cmdlet), set the value of **$multipleVerifiedDomainNames** in the script to **$true**.</span></span> <span data-ttu-id="edda8-230">Také se ujistěte, že odeberete všechny existující issuerid deklaraci, která může být vytvořen přes Azure AD Connect nebo prostřednictvím jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="edda8-230">Also make sure that you remove any existing issuerid claim that might have been created by Azure AD Connect or via other means.</span></span> <span data-ttu-id="edda8-231">Tady je příklad pro toto pravidlo:</span><span class="sxs-lookup"><span data-stu-id="edda8-231">Here is an example for this rule:</span></span>


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- <span data-ttu-id="edda8-232">Pokud jste již vystavily **ImmutableID** deklarace identity pro uživatelské účty, nastavte hodnotu **$immutableIDAlreadyIssuedforUsers** ve skriptu **$true**.</span><span class="sxs-lookup"><span data-stu-id="edda8-232">If you have already issued an **ImmutableID** claim  for user accounts, set the value of **$immutableIDAlreadyIssuedforUsers** in the script to **$true**.</span></span>

## <a name="step-3-enable-windows-down-level-devices"></a><span data-ttu-id="edda8-233">Krok 3: Zapnutí zařízení se systémem Windows nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="edda8-233">Step 3: Enable Windows down-level devices</span></span>

<span data-ttu-id="edda8-234">Pokud některé z vašich zařízení připojených k doméně Windows nižší úrovně zařízení, budete muset:</span><span class="sxs-lookup"><span data-stu-id="edda8-234">If some of your domain-joined devices Windows down-level devices, you need to:</span></span>

- <span data-ttu-id="edda8-235">Nastavte zásady ve službě Azure AD umožňuje uživatelům registrovat zařízení.</span><span class="sxs-lookup"><span data-stu-id="edda8-235">Set a policy in Azure AD to enable users to register devices.</span></span>
 
- <span data-ttu-id="edda8-236">Místní službu FS vystavovat deklarace identity pro podporu nakonfigurovat **integrované ověřování systému Windows (IWA)** pro registraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="edda8-236">Configure your on-premises federation service to issue claims to support **Integrated Windows Authentication (IWA)** for device registration.</span></span>
 
- <span data-ttu-id="edda8-237">Přidáte koncový bod ověřování zařízení služby Azure AD do místní zóny intranetu, abyste se vyhnuli výzev ohledně certifikátů při ověřování zařízení.</span><span class="sxs-lookup"><span data-stu-id="edda8-237">Add the Azure AD device authentication end-point to the local Intranet zones to avoid certificate prompts when authenticating the device.</span></span>

### <a name="set-policy-in-azure-ad-to-enable-users-to-register-devices"></a><span data-ttu-id="edda8-238">Nastavit zásady ve službě Azure AD umožňuje uživatelům registrovat zařízení</span><span class="sxs-lookup"><span data-stu-id="edda8-238">Set policy in Azure AD to enable users to register devices</span></span>

<span data-ttu-id="edda8-239">Chcete-li zaregistrovat zařízení se systémem Windows nižší úrovně, ujistěte se, že bude uživatelům k registraci zařízení ve službě Azure AD povolit nastavení.</span><span class="sxs-lookup"><span data-stu-id="edda8-239">To register Windows down-level devices, you need to make sure that the setting to allow users to register devices in Azure AD is set.</span></span> <span data-ttu-id="edda8-240">Na portálu Azure můžete najít toto nastavení v části:</span><span class="sxs-lookup"><span data-stu-id="edda8-240">In the Azure portal, you can find this setting under:</span></span>

`Azure Active Directory > Users and groups > Device settings`
    
<span data-ttu-id="edda8-241">Tyto zásady musí být nastavena na **všechny**: **uživatelé mohou registrovat svá zařízení s Azure AD**</span><span class="sxs-lookup"><span data-stu-id="edda8-241">The following policy must be set to **All**: **Users may register their devices with Azure AD**</span></span>

![Registrace zařízení](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a><span data-ttu-id="edda8-243">Nakonfigurujte místní služby FS</span><span class="sxs-lookup"><span data-stu-id="edda8-243">Configure on-premises federation service</span></span> 

<span data-ttu-id="edda8-244">Vaše místní federační služba musí podporovat vydání **authenticationmehod** a **wiaormultiauthn** deklarací při přijímání požadavek ověřování Azure AD předávající strany, která uchovává resouce_params parametr s hodnotou kódovaného, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="edda8-244">Your on-premises federation service must support issuing the **authenticationmehod** and **wiaormultiauthn** claims when receiving an authentication request to the Azure AD relying party holding a resouce_params parameter with an encoded value as shown below:</span></span>

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

<span data-ttu-id="edda8-245">Pokud takový požadavek pochází, místní federační služby musí ověřit uživatele pomocí integrovaného ověřování systému Windows a na úspěch, vydá následující dvě deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="edda8-245">When such a request comes, the on-premises federation service must authenticate the user using Integrated Windows Authentication and upon success, it must issue the following two claims:</span></span>

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

<span data-ttu-id="edda8-246">Ve službě AD FS musíte přidat pravidel transformace vystavení, který předává prostřednictvím metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="edda8-246">In AD FS, you must add an issuance transform rule that passes-through the authentication method.</span></span>  

<span data-ttu-id="edda8-247">**Chcete-li přidat toto pravidlo:**</span><span class="sxs-lookup"><span data-stu-id="edda8-247">**To add this rule:**</span></span>

1. <span data-ttu-id="edda8-248">V konzole pro správu služby AD FS, přejděte na `AD FS > Trust Relationships > Relying Party Trusts`.</span><span class="sxs-lookup"><span data-stu-id="edda8-248">In the AD FS management console, go to `AD FS > Trust Relationships > Relying Party Trusts`.</span></span>
2. <span data-ttu-id="edda8-249">Klikněte pravým tlačítkem na objekt pro vztah důvěryhodnosti předávající strany ze serveru Microsoft Office 365 Identity Platform a vyberte **upravit pravidla deklarací identity**.</span><span class="sxs-lookup"><span data-stu-id="edda8-249">Right-click the Microsoft Office 365 Identity Platform relying party trust object, and then select **Edit Claim Rules**.</span></span>
3. <span data-ttu-id="edda8-250">Na **pravidlech transformace vystavení** vyberte **přidat pravidlo**.</span><span class="sxs-lookup"><span data-stu-id="edda8-250">On the **Issuance Transform Rules** tab, select **Add Rule**.</span></span>
4. <span data-ttu-id="edda8-251">V **pravidlo deklarace identity** seznam šablon, vyberte **odesílat deklarace pomocí vlastního pravidla**.</span><span class="sxs-lookup"><span data-stu-id="edda8-251">In the **Claim rule** template list, select **Send Claims Using a Custom Rule**.</span></span>
5. <span data-ttu-id="edda8-252">Vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="edda8-252">Select **Next**.</span></span>
6. <span data-ttu-id="edda8-253">V **název pravidla deklarací** zadejte **pravidlo deklarace identity metoda ověřování**.</span><span class="sxs-lookup"><span data-stu-id="edda8-253">In the **Claim rule name** box, type **Auth Method Claim Rule**.</span></span>
7. <span data-ttu-id="edda8-254">V **pravidlo deklarace identity** zadejte následující pravidlo:</span><span class="sxs-lookup"><span data-stu-id="edda8-254">In the **Claim rule** box, type the following rule:</span></span>

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. <span data-ttu-id="edda8-255">Na federačním serveru, zadejte příkaz prostředí PowerShell níže po nahrazení  **\<RPObjectName\>**  s předávající strany název objektu pro vztah důvěryhodnosti objektu předávající strany služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edda8-255">On your federation server, type the PowerShell command below after replacing **\<RPObjectName\>** with the relying party object name for your Azure AD relying party trust object.</span></span> <span data-ttu-id="edda8-256">Tento objekt obvykle jmenuje **Microsoft Office 365 Identity Platform**.</span><span class="sxs-lookup"><span data-stu-id="edda8-256">This object usually is named **Microsoft Office 365 Identity Platform**.</span></span>
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-end-point-to-the-local-intranet-zones"></a><span data-ttu-id="edda8-257">Přidejte do zóny místního intranetu koncový bod ověřování zařízení služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="edda8-257">Add the Azure AD device authentication end-point to the Local Intranet zones</span></span>

<span data-ttu-id="edda8-258">Aby se zabránilo certifikát vyzve při ověřování uživatelů v registraci zařízení do služby Azure AD můžete posouvat nastavení zásad pro vaše zařízení připojených k doméně, přidat následující adresy URL do zóny místního intranetu v aplikaci Internet Explorer:</span><span class="sxs-lookup"><span data-stu-id="edda8-258">To avoid certificate prompts when users in register devices authenticate to Azure AD you can push a policy to your domain-joined devices to add the following URL to the Local Intranet zone in Internet Explorer:</span></span>

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a><span data-ttu-id="edda8-259">Krok 4: Řízení nasazení a zavedení</span><span class="sxs-lookup"><span data-stu-id="edda8-259">Step 4: Control deployment and rollout</span></span>

<span data-ttu-id="edda8-260">Po dokončení požadovaných kroků připojená k doméně jsou připravené k automatickému připojení Azure AD:</span><span class="sxs-lookup"><span data-stu-id="edda8-260">When you have completed the required steps, domain-joined devices are ready to automatically join Azure AD:</span></span>

- <span data-ttu-id="edda8-261">Všechny připojené k doméně zařízení se systémem Windows 10 Anniversary Update a Windows Server 2016 automaticky zaregistrovat s Azure AD v zařízení restartujte nebo přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="edda8-261">All domain-joined devices running Windows 10 Anniversary Update and Windows Server 2016 automatically register with Azure AD at device restart or user sign-in.</span></span> 

- <span data-ttu-id="edda8-262">Zaregistrovat nová zařízení s Azure AD při restartování zařízení po dokončení operace připojení k doméně.</span><span class="sxs-lookup"><span data-stu-id="edda8-262">New devices register with Azure AD when the device restarts after the domain join operation is completed.</span></span>

- <span data-ttu-id="edda8-263">Registruje zařízení, které byly dříve Azure AD (například pro Intune) přechod do "*připojený k doméně, zaregistrováno v AAD*"; však bude trvat delší dobu pro tento proces dokončit ve všech zařízeních z důvodu normálního toku domény a aktivity uživatelů.</span><span class="sxs-lookup"><span data-stu-id="edda8-263">Devices that were previously Azure AD registered (for example, for Intune) transition to “*Domain Joined, AAD Registered*”; however it takes some time for this process to complete across all devices due to the normal flow of domain and user activity.</span></span>

### <a name="remarks"></a><span data-ttu-id="edda8-264">Poznámky</span><span class="sxs-lookup"><span data-stu-id="edda8-264">Remarks</span></span>

- <span data-ttu-id="edda8-265">Můžete objekt zásad skupiny a ovládejte automatické registrace Windows 10 a Windows Server 2016 počítačů připojených k doméně.</span><span class="sxs-lookup"><span data-stu-id="edda8-265">You can use a Group Policy object to control the rollout of automatic registration of Windows 10 and Windows Server 2016 domain-joined computers.</span></span>

- <span data-ttu-id="edda8-266">Windows 10. listopadu 2015 aktualizace automaticky spojí se službou Azure AD **pouze** nastaveného zavedení objekt zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="edda8-266">Windows 10 November 2015 Update automatically joins with Azure AD **only** if the rollout Group Policy object is set.</span></span>

- <span data-ttu-id="edda8-267">K zavedení počítačů se systémem Windows nižší úrovně, můžete nasadit [balíček služby Windows Installer](#windows-installer-packages-for-non-windows-10-computers) do počítačů, které jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="edda8-267">To rollout of Windows down-level computers, you can deploy a [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) to computers that you select.</span></span>

- <span data-ttu-id="edda8-268">Pokud jste nabízená objekt zásad skupiny Windows 8.1 připojená k doméně, dojde k pokusu o spojení; nedoporučujeme ale, že používáte [balíček služby Windows Installer](#windows-installer-packages-for-non-windows-10-computers) připojit všechna zařízení Windows nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="edda8-268">If you push the Group Policy object to Windows 8.1 domain-joined devices, a join is attempted; however it is recommended that you use the [Windows Installer package](#windows-installer-packages-for-non-windows-10-computers) to join all your Windows down-level devices.</span></span> 

### <a name="create-a-group-policy-object"></a><span data-ttu-id="edda8-269">Vytvoření objektu zásad skupiny</span><span class="sxs-lookup"><span data-stu-id="edda8-269">Create a Group Policy object</span></span> 

<span data-ttu-id="edda8-270">Ovládejte aktuální počítačů se systémem Windows, měli byste nasadit **registraci připojený k doméně počítače jako zařízení** objekt zásad skupiny zařízení, který chcete zaregistrovat.</span><span class="sxs-lookup"><span data-stu-id="edda8-270">To control the rollout of Windows current computers, you should deploy the **Register domain-joined computers as devices** Group Policy object to the devices you want to register.</span></span> <span data-ttu-id="edda8-271">Například můžete nasadit zásady do organizační jednotky nebo do skupiny zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="edda8-271">For example, you can deploy the policy to an organizational unit or to a security group.</span></span>

<span data-ttu-id="edda8-272">**Nastavení zásad:**</span><span class="sxs-lookup"><span data-stu-id="edda8-272">**To set the policy:**</span></span>

1. <span data-ttu-id="edda8-273">Otevřete **správce serveru**a pak přejděte na `Tools > Group Policy Management`.</span><span class="sxs-lookup"><span data-stu-id="edda8-273">Open **Server Manager**, and then go to `Tools > Group Policy Management`.</span></span>
2. <span data-ttu-id="edda8-274">Přejděte na uzel domény, který odpovídá k doméně, ve které chcete aktivovat automatickou registraci aktuální počítačů se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="edda8-274">Go to the domain node that corresponds to the domain where you want to activate auto-registration of Windows current computers.</span></span>
3. <span data-ttu-id="edda8-275">Klikněte pravým tlačítkem na **objekty zásad skupiny**a potom vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="edda8-275">Right-click **Group Policy Objects**, and then select **New**.</span></span>
4. <span data-ttu-id="edda8-276">Zadejte název objektu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="edda8-276">Type a name for your Group Policy object.</span></span> <span data-ttu-id="edda8-277">Například * hybridní Azure AD join.</span><span class="sxs-lookup"><span data-stu-id="edda8-277">For example, *Hybrid Azure AD join.</span></span> 
5. <span data-ttu-id="edda8-278">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="edda8-278">Click **OK**.</span></span>
6. <span data-ttu-id="edda8-279">Klikněte pravým tlačítkem na váš nový objekt zásad skupiny a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="edda8-279">Right-click your new Group Policy object, and then select **Edit**.</span></span>
7. <span data-ttu-id="edda8-280">Přejděte na **konfigurace počítače** > **zásady** > **šablony pro správu** > **součásti systému Windows** > **registrace zařízení**.</span><span class="sxs-lookup"><span data-stu-id="edda8-280">Go to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Device Registration**.</span></span> 
8. <span data-ttu-id="edda8-281">Klikněte pravým tlačítkem na **registraci připojený k doméně počítače jako zařízení**a potom vyberte **upravit**.</span><span class="sxs-lookup"><span data-stu-id="edda8-281">Right-click **Register domain-joined computers as devices**, and then select **Edit**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="edda8-282">Tato šablona zásad skupiny byla přejmenována z dřívějších verzí konzoly pro správu zásad skupiny.</span><span class="sxs-lookup"><span data-stu-id="edda8-282">This Group Policy template has been renamed from earlier versions of the Group Policy Management console.</span></span> <span data-ttu-id="edda8-283">Pokud používáte starší verzi konzoly, přejděte na `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span><span class="sxs-lookup"><span data-stu-id="edda8-283">If you are using an earlier version of the console, go to `Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`.</span></span> 

7. <span data-ttu-id="edda8-284">Vyberte **povoleno**a potom klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="edda8-284">Select **Enabled**, and then click **Apply**.</span></span>
8. <span data-ttu-id="edda8-285">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="edda8-285">Click **OK**.</span></span>
9. <span data-ttu-id="edda8-286">Propojte objekt zásad skupiny k umístění podle vaší volby.</span><span class="sxs-lookup"><span data-stu-id="edda8-286">Link the Group Policy object to a location of your choice.</span></span> <span data-ttu-id="edda8-287">Například můžete propojit k určité organizační jednotce.</span><span class="sxs-lookup"><span data-stu-id="edda8-287">For example, you can link it to a specific organizational unit.</span></span> <span data-ttu-id="edda8-288">Můžete také může ho propojit s konkrétní skupiny zabezpečení počítačů, které automaticky připojit k službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edda8-288">You also could link it to a specific security group of computers that automatically join with Azure AD.</span></span> <span data-ttu-id="edda8-289">Nastavení této zásady pro všechny připojené k doméně Windows 10 a Windows Server 2016 počítače ve vaší organizaci, propojte objekt zásad skupiny do domény.</span><span class="sxs-lookup"><span data-stu-id="edda8-289">To set this policy for all domain-joined Windows 10 and Windows Server 2016 computers in your organization, link the Group Policy object to the domain.</span></span>

### <a name="windows-installer-packages-for-non-windows-10-computers"></a><span data-ttu-id="edda8-290">Balíčky Instalační služby systému Windows pro počítače s Windows 10</span><span class="sxs-lookup"><span data-stu-id="edda8-290">Windows Installer packages for non-Windows 10 computers</span></span>

<span data-ttu-id="edda8-291">K připojení k doméně počítače se systémem Windows nižší úrovně ve federovaném prostředí, můžete stáhnout a nainstalovat tyto balíček Instalační služby systému Windows (.msi) ze stažení softwaru společnosti Microsoft [připojení k síti na pracovišti Microsoft pro počítače s Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=53554) stránka.</span><span class="sxs-lookup"><span data-stu-id="edda8-291">To join domain-joined Windows down-level computers in a federated environment, you can download and install these Windows Installer package (.msi) from Download Center at the [Microsoft Workplace Join for non-Windows 10 computers](https://www.microsoft.com/en-us/download/details.aspx?id=53554) page.</span></span>

<span data-ttu-id="edda8-292">Balíček lze nasadit pomocí systém distribuce softwaru jako je System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="edda8-292">You can deploy the package by using a software distribution system like System Center Configuration Manager.</span></span> <span data-ttu-id="edda8-293">Balíček podporuje možnosti standardní bezobslužnou instalaci s *quiet* parametr.</span><span class="sxs-lookup"><span data-stu-id="edda8-293">The package supports the standard silent install options with the *quiet* parameter.</span></span> <span data-ttu-id="edda8-294">System Center Configuration Manager aktuální větev nabízí další výhody z dřívějších verzí, jako je schopnost sledovat dokončené registrace.</span><span class="sxs-lookup"><span data-stu-id="edda8-294">System Center Configuration Manager Current Branch offers additional benefits from earlier versions, like the ability to track completed registrations.</span></span> <span data-ttu-id="edda8-295">Další informace najdete v tématu [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="edda8-295">For more information, see [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).</span></span>

<span data-ttu-id="edda8-296">Instalační program vytvoří naplánovanou úlohu na systém, který běží v kontextu uživatele.</span><span class="sxs-lookup"><span data-stu-id="edda8-296">The installer creates a scheduled task on the system that runs in the user’s context.</span></span> <span data-ttu-id="edda8-297">Úloha se aktivuje, když se uživatel přihlásí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="edda8-297">The task is triggered when the user signs in to Windows.</span></span> <span data-ttu-id="edda8-298">Úloha bezobslužně připojí zařízení s Azure AD s pověřeními uživatele po ověřování pomocí integrovaného ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="edda8-298">The task silently joins the device with Azure AD with the user credentials after authenticating using Integrated Windows Authentication.</span></span> <span data-ttu-id="edda8-299">Chcete-li zobrazit naplánované úlohy, v zařízení, přejděte na **Microsoft** > **k síti na pracovišti**a potom přejděte na Knihovna plánovače úloh.</span><span class="sxs-lookup"><span data-stu-id="edda8-299">To see the scheduled task, in the device, go to **Microsoft** > **Workplace Join**, and then go to the Task Scheduler library.</span></span>

## <a name="step-5-verify-joined-devices"></a><span data-ttu-id="edda8-300">Krok 5: Ověření připojeného k zařízení</span><span class="sxs-lookup"><span data-stu-id="edda8-300">Step 5: Verify joined devices</span></span>

<span data-ttu-id="edda8-301">Úspěšné připojeného k zařízení ve vaší organizaci můžete zkontrolovat pomocí [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) rutiny v [modul Azure Active Directory PowerShell](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="edda8-301">You can check successful joined devices in your organization by using the [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet in the [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).</span></span>

<span data-ttu-id="edda8-302">Výstup této rutiny zobrazuje zařízení, které jsou zaregistrované a propojit s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="edda8-302">The output of this cmdlet shows devices that are registered and joined with Azure AD.</span></span> <span data-ttu-id="edda8-303">Všechna zařízení, použijte **– všechny** parametr a pak je filtrování pomocí **deviceTrustType** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="edda8-303">To get all devices, use the **-All** parameter, and then filter them using the **deviceTrustType** property.</span></span> <span data-ttu-id="edda8-304">Připojené k doméně zařízení mít hodnotu **připojený k doméně**.</span><span class="sxs-lookup"><span data-stu-id="edda8-304">Domain joined devices have a value of **Domain Joined**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edda8-305">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edda8-305">Next steps</span></span>

* [<span data-ttu-id="edda8-306">Úvod do správy zařízení v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="edda8-306">Introduction to device management in Azure Active Directory</span></span>](device-management-introduction.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
