---
title: "Vytvoření několika služeb Azure AD s jednou službou AD FS | Dokumentace Microsoftu"
description: "V tomto dokumentu se naučíte vytvořit federaci několik služeb Azure AD s jednou službou AD FS."
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
ms.openlocfilehash: 436bf5905d2b203dc4cceea97f4fb90593df7111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a><span data-ttu-id="2bb5b-104">Vytvoření federace několika instancí Azure AD s jednou instancí AD FS</span><span class="sxs-lookup"><span data-stu-id="2bb5b-104">Federate multiple instances of Azure AD with single instance of AD FS</span></span>

<span data-ttu-id="2bb5b-105">Jedna farma AD FS s vysokou dostupností může federovat několik doménových struktur, pokud mezi nimi existuje obousměrný vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-105">A single high available AD FS farm can federate multiple forests if they have 2-way trust between them.</span></span> <span data-ttu-id="2bb5b-106">Těchto několik doménových struktur může, ale nemusí odpovídat téže službě Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-106">These multiple forests may or may not correspond to the same Azure Active Directory.</span></span> <span data-ttu-id="2bb5b-107">Tento článek obsahuje pokyny pro konfiguraci federace mezi jedním nasazením služby AD FS a více než jednou doménovou strukturou, přičemž doménové struktury se synchronizují do různých služeb Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-107">This article provides instructions on how to configure federation between a single AD FS deployment and more than one forests that sync to different Azure AD.</span></span>

![Federace více klientů s jednou službou AD FS](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> <span data-ttu-id="2bb5b-109">Zpětný zápis zařízení a automatické připojení zařízení nejsou v tomto scénáři podporovány.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-109">Device writeback and automatic device join are not supported in this scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb5b-110">V tomto scénáři nelze ke konfigurování federace použít Azure AD Connect, protože Azure AD Connect může konfigurovat federaci pro domény v jedné službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-110">Azure AD Connect cannot be used to configure federation in this scenario as Azure AD Connect can configure federation for domains in a single Azure AD.</span></span>

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a><span data-ttu-id="2bb5b-111">Kroky pro vytvoření federace AD FS s více službami Azure AD</span><span class="sxs-lookup"><span data-stu-id="2bb5b-111">Steps for federating AD FS with multiple Azure AD</span></span>

<span data-ttu-id="2bb5b-112">Předpokládejme, že doména contoso.com ve službě Azure Active Directory contoso.onmicrosoft.com je již federovaná s místní službou AD FS nainstalovanou v místním prostředí služby Active Directory contoso.com.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-112">Consider a domain contoso.com in Azure Active Directory contoso.onmicrosoft.com is already federated with the AD FS on-premises installed in contoso.com on-premises Active Directory environment.</span></span> <span data-ttu-id="2bb5b-113">Fabrikam.com je doména ve službě Azure Active Directory fabrikam.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-113">Fabrikam.com is a domain in fabrikam.onmicrosoft.com Azure Active Directory.</span></span>

##<a name="step-1-establish-a-two-way-trust"></a><span data-ttu-id="2bb5b-114">Krok 1: Vytvoření obousměrného vztahu důvěryhodnosti</span><span class="sxs-lookup"><span data-stu-id="2bb5b-114">Step 1: Establish a two-way trust</span></span>
 
<span data-ttu-id="2bb5b-115">Aby služba AD FS v doméně contoso.com mohla ověřovat uživatele v doméně fabrikam.com, je potřebný obousměrný vztah důvěryhodnosti mezi doménami contoso.com a fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-115">For AD FS in contoso.com to be able to authenticate users in fabrikam.com, a two-way trust is needed between contoso.com and fabrikam.com.</span></span> <span data-ttu-id="2bb5b-116">Při vytváření obousměrného vztahu důvěryhodnosti postupujte podle pokynů v tomto [článku](https://technet.microsoft.com/library/cc816590.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bb5b-116">Follow the guideline in this [article](https://technet.microsoft.com/library/cc816590.aspx) to create the two-way trust.</span></span>
 
##<a name="step-2-modify-contosocom-federation-settings"></a><span data-ttu-id="2bb5b-117">Krok 2: Úprava nastavení federace contoso.com</span><span class="sxs-lookup"><span data-stu-id="2bb5b-117">Step 2: Modify contoso.com federation settings</span></span> 
 
<span data-ttu-id="2bb5b-118">Výchozí vystavitel nastavený pro jednu doménu federovanou se službou AD FS je „http://plně_kvalifikovaný_název_domény_služby_AD_FS/adfs/services/trust“, například „http://fs.contoso.com/adfs/services/trust“.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-118">The default issuer set for a single domain federated to AD FS is "http://ADFSServiceFQDN/adfs/services/trust", for example, “http://fs.contoso.com/adfs/services/trust”.</span></span> <span data-ttu-id="2bb5b-119">Azure Active Directory vyžaduje jedinečného vystavitele pro každou federovanou doménu.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-119">Azure Active Directory requires unique issuer for each federated domain.</span></span> <span data-ttu-id="2bb5b-120">Vzhledem k tomu, že stejná služba AD FS bude federovat dvě domény, hodnota vystavitele musí být upravena, aby byla jedinečná pro každou doménu, kterou služba AD FS federuje s Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-120">Since the same AD FS is going to federate two domains, the issuer value needs to be modified so that it is unique for each domain AD FS federates with Azure Active Directory.</span></span> 
 
<span data-ttu-id="2bb5b-121">Na serveru AD FS otevřete prostředí Azure AD PowerShell a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2bb5b-121">On the AD FS server, open Azure AD PowerShell and perform the following steps:</span></span>
 
<span data-ttu-id="2bb5b-122">Připojte se ke službě Azure Active Directory obsahující doménu contoso.com: Connect-MsolService Aktualizujte nastavení federace pro doménu contoso.com: Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="2bb5b-122">Connect to the Azure Active Directory that contains the domain contoso.com Connect-MsolService Update the federation settings for contoso.com Update-MsolFederatedDomain -DomainName contoso.com –SupportMultipleDomain</span></span>
 
<span data-ttu-id="2bb5b-123">Vystavitel v nastavení federace domény se změní na „http://contoso.com/adfs/services/trust“ a přidá se pravidlo deklarace identity vystavování pro vztah důvěryhodnosti přijímající strany Azure AD, aby se vydávala správná hodnota issuerId na základě přípony hlavního názvu uživatele (UPN).</span><span class="sxs-lookup"><span data-stu-id="2bb5b-123">Issuer in the domain federation setting will be changed to "http://contoso.com/adfs/services/trust" and an issuance claim rule will be added for the Azure AD Relying Party Trust to issue the correct issuerId value based on the UPN suffix.</span></span>
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a><span data-ttu-id="2bb5b-124">Krok 3: Vytvoření federace domény fabrikam.com se službou AD FS</span><span class="sxs-lookup"><span data-stu-id="2bb5b-124">Step 3: Federate fabrikam.com with AD FS</span></span>
 
<span data-ttu-id="2bb5b-125">V relaci prostředí Azure AD PowerShell proveďte následující kroky: Připojte se ke službě Azure Active Directory, která obsahuje doménu fabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-125">In Azure AD powershell session perform the following steps: Connect to Azure Active Directory that contains the domain fabrikam.com</span></span>

    Connect-MsolService
<span data-ttu-id="2bb5b-126">Převeďte spravovanou doménu fabrikam.com na federovanou:</span><span class="sxs-lookup"><span data-stu-id="2bb5b-126">Convert the fabrikam.com managed domain to federated:</span></span>

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
<span data-ttu-id="2bb5b-127">Uvedená operace vytvoří federaci domény fabrikam.com se stejnou službou AD FS.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-127">The above operation will federate the domain fabrikam.com with the same AD FS.</span></span> <span data-ttu-id="2bb5b-128">Nastavení domény můžete ověřit pomocí příkazu Get-MsolDomainFederationSettings pro obě domény.</span><span class="sxs-lookup"><span data-stu-id="2bb5b-128">You can verify the domain settings by using Get-MsolDomainFederationSettings for both domains.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bb5b-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bb5b-129">Next steps</span></span>
[<span data-ttu-id="2bb5b-130">Připojení Active Directory s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2bb5b-130">Connect Active Directory with Azure Active Directory</span></span>](active-directory-aadconnect.md)
