---
title: "aaaFederating více Azure AD s jedné služby AD FS | Microsoft Docs"
description: "V tomto dokumentu se dozvíte, jak toofederate více Azure AD s jedné služby AD FS."
keywords: "vytvoření federace, ADFS, AD FS, více klientů, jedna služba AD FS, jedna služba ADFS, federace s více klienty, ADFS s více doménovými strukturami, připojení AAD, federace, federace mezi klienty"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.openlocfilehash: 442192896b3b13f7bf9388396cd3769e194329d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a><span data-ttu-id="a8fb3-104">Vytvoření federace několika instancí Azure AD s jednou instancí AD FS</span><span class="sxs-lookup"><span data-stu-id="a8fb3-104">Federate multiple instances of Azure AD with single instance of AD FS</span></span>

<span data-ttu-id="a8fb3-105">Jedna farma AD FS s vysokou dostupností může federovat několik doménových struktur, pokud mezi nimi existuje obousměrný vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-105">A single high available AD FS farm can federate multiple forests if they have 2-way trust between them.</span></span> <span data-ttu-id="a8fb3-106">Tyto několik doménových struktur může nebo nemusí odpovídat toohello stejné Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-106">These multiple forests may or may not correspond toohello same Azure Active Directory.</span></span> <span data-ttu-id="a8fb3-107">Tento článek obsahuje pokyny jak tooconfigure federace mezi jedno nasazení služby AD FS a více doménových strukturách toodifferent této synchronizace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-107">This article provides instructions on how tooconfigure federation between a single AD FS deployment and more than one forests that sync toodifferent Azure AD.</span></span>

![Federace více klientů s jednou službou AD FS](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> <span data-ttu-id="a8fb3-109">Zpětný zápis zařízení a automatické připojení zařízení nejsou v tomto scénáři podporovány.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-109">Device writeback and automatic device join are not supported in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="a8fb3-110">Azure AD Connect nelze použít tooconfigure federation v tomto scénáři, protože Azure AD Connect můžete konfiguraci federace domén v jednom Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-110">Azure AD Connect cannot be used tooconfigure federation in this scenario as Azure AD Connect can configure federation for domains in a single Azure AD.</span></span>

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a><span data-ttu-id="a8fb3-111">Kroky pro vytvoření federace AD FS s více službami Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8fb3-111">Steps for federating AD FS with multiple Azure AD</span></span>

<span data-ttu-id="a8fb3-112">Vezměte v úvahu, že doménu contoso.com v Azure Active Directory contoso.onmicrosoft.com je již sdružených se službou místně nainstalován v prostředí služby Active Directory v místě contoso.com hello služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-112">Consider a domain contoso.com in Azure Active Directory contoso.onmicrosoft.com is already federated with hello AD FS on-premises installed in contoso.com on-premises Active Directory environment.</span></span> <span data-ttu-id="a8fb3-113">Fabrikam.com je doména ve službě Azure Active Directory fabrikam.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-113">Fabrikam.com is a domain in fabrikam.onmicrosoft.com Azure Active Directory.</span></span>

##<a name="step-1-establish-a-two-way-trust"></a><span data-ttu-id="a8fb3-114">Krok 1: Vytvoření obousměrného vztahu důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="a8fb3-114">Step 1: Establish a two-way trust</span></span>
 
<span data-ttu-id="a8fb3-115">Pro službu AD FS v contoso.com toobe možné tooauthenticate uživatelé v fabrikam.com je potřeba obousměrný vztah důvěryhodnosti mezi contoso.com a fabrikam.com. Postupujte podle obecných zásad hello v tomto [článku](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello obousměrný vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-115">For AD FS in contoso.com toobe able tooauthenticate users in fabrikam.com, a two-way trust is needed between contoso.com and fabrikam.com. Follow hello guideline in this [article](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello two-way trust.</span></span>
 
##<a name="step-2-modify-contosocom-federation-settings"></a><span data-ttu-id="a8fb3-116">Krok 2: Úprava nastavení federace contoso.com</span><span class="sxs-lookup"><span data-stu-id="a8fb3-116">Step 2: Modify contoso.com federation settings</span></span> 
 
<span data-ttu-id="a8fb3-117">Vystavitel Hello výchozí nastavení pro jednu doménu federovaný tooAD FS je "http://ADFSServiceFQDN/adfs/services/trust", například "http://fs.contoso.com/adfs/services/trust".</span><span class="sxs-lookup"><span data-stu-id="a8fb3-117">hello default issuer set for a single domain federated tooAD FS is "http://ADFSServiceFQDN/adfs/services/trust", for example, “http://fs.contoso.com/adfs/services/trust”.</span></span> <span data-ttu-id="a8fb3-118">Azure Active Directory vyžaduje jedinečného vystavitele pro každou federovanou doménu.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-118">Azure Active Directory requires unique issuer for each federated domain.</span></span> <span data-ttu-id="a8fb3-119">Hodnota vystavitele hello hello stejné služby AD FS bude toofederate dvě domény, musí toobe upravit tak, aby je jedinečný pro každou doménu, kterou federates služby AD FS se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-119">Since hello same AD FS is going toofederate two domains, hello issuer value needs toobe modified so that it is unique for each domain AD FS federates with Azure Active Directory.</span></span> 
 
<span data-ttu-id="a8fb3-120">Na serveru hello služby AD FS otevřete Azure AD PowerShell a provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8fb3-120">On hello AD FS server, open Azure AD PowerShell and perform hello following steps:</span></span>
 
<span data-ttu-id="a8fb3-121">Připojit toohello Azure Active Directory, která obsahuje hello domény contoso.com Connect-MsolService hello federační nastavení aktualizace pro doménu contoso.com aktualizace MsolFederatedDomain - DomainName contoso.com – SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="a8fb3-121">Connect toohello Azure Active Directory that contains hello domain contoso.com Connect-MsolService Update hello federation settings for contoso.com Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain</span></span>
 
<span data-ttu-id="a8fb3-122">Změní vystavitele v nastavení federace domén hello příliš "http://contoso.com/adfs/services/trust" a vystavování deklarace identity přidá se pravidlo pro hello Azure AD vztah důvěryhodnosti předávající strany tooissue hello správná hodnota issuerId založená na příponu UPN hello.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-122">Issuer in hello domain federation setting will be changed too"http://contoso.com/adfs/services/trust" and an issuance claim rule will be added for hello Azure AD Relying Party Trust tooissue hello correct issuerId value based on hello UPN suffix.</span></span>
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a><span data-ttu-id="a8fb3-123">Krok 3: Vytvoření federace domény fabrikam.com se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="a8fb3-123">Step 3: Federate fabrikam.com with AD FS</span></span>
 
<span data-ttu-id="a8fb3-124">V Azure AD powershell relace provést následující kroky hello: připojení tooAzure služby Active Directory, který obsahuje fabrikam.com domény hello</span><span class="sxs-lookup"><span data-stu-id="a8fb3-124">In Azure AD powershell session perform hello following steps: Connect tooAzure Active Directory that contains hello domain fabrikam.com</span></span>

    Connect-MsolService
<span data-ttu-id="a8fb3-125">Převeďte hello fabrikam.com spravované domény toofederated:</span><span class="sxs-lookup"><span data-stu-id="a8fb3-125">Convert hello fabrikam.com managed domain toofederated:</span></span>

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
<span data-ttu-id="a8fb3-126">Hello výše operace bude federaci hello domény fabrikam.com s hello stejné služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-126">hello above operation will federate hello domain fabrikam.com with hello same AD FS.</span></span> <span data-ttu-id="a8fb3-127">Nastavení domény hello můžete ověřit pomocí příkazu Get-MsolDomainFederationSettings pro obě domény.</span><span class="sxs-lookup"><span data-stu-id="a8fb3-127">You can verify hello domain settings by using Get-MsolDomainFederationSettings for both domains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8fb3-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a8fb3-128">Next steps</span></span>
[<span data-ttu-id="a8fb3-129">Připojení Active Directory s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8fb3-129">Connect Active Directory with Azure Active Directory</span></span>](active-directory-aadconnect.md)
