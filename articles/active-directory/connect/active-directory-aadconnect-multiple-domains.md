---
title: "Azure AD Connect více domén"
description: "Tento dokument popisuje nastavení a konfiguraci víc domén nejvyšší úrovně s O365 a Azure AD."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 8e3f496c2868cc3430e0efd47805aec2205168aa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a><span data-ttu-id="9f731-103">Podpora více domén pro federaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f731-103">Multiple Domain Support for Federating with Azure AD</span></span>
<span data-ttu-id="9f731-104">Následující dokumentace obsahuje pokyny k použití více nejvyšší úrovně domén a subdomén při federaci s Office 365 nebo Azure AD domény.</span><span class="sxs-lookup"><span data-stu-id="9f731-104">The following documentation provides guidance on how to use multiple top-level domains and sub-domains when federating with Office 365 or Azure AD domains.</span></span>

## <a name="multiple-top-level-domain-support"></a><span data-ttu-id="9f731-105">Podpora více domén nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="9f731-105">Multiple top-level domain support</span></span>
<span data-ttu-id="9f731-106">Federaci několika, domény nejvyšší úrovně s Azure AD vyžaduje určitou další konfiguraci, který není nutný, pokud federaci s jednu doménu nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="9f731-106">Federating multiple, top-level domains with Azure AD requires some additional configuration that is not required when federating with one top-level domain.</span></span>

<span data-ttu-id="9f731-107">Když domény federovaný pomocí služby Azure AD, několik vlastností, které jsou nastavené na doménu v Azure.</span><span class="sxs-lookup"><span data-stu-id="9f731-107">When a domain is federated with Azure AD, several properties are set on the domain in Azure.</span></span>  <span data-ttu-id="9f731-108">Jeden z nich důležité je IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="9f731-108">One important one is IssuerUri.</span></span>  <span data-ttu-id="9f731-109">Toto je identifikátor URI, který se používá Azure AD k identifikaci domény, který je přidružený token.</span><span class="sxs-lookup"><span data-stu-id="9f731-109">This is a URI that is used by Azure AD to identify the domain that the token is associated with.</span></span>  <span data-ttu-id="9f731-110">Identifikátor URI nepotřebuje přeložit na cokoli ale musí být platný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="9f731-110">The URI doesn’t need to resolve to anything but it must be a valid URI.</span></span>  <span data-ttu-id="9f731-111">Ve výchozím nastavení, Azure AD Nastaví tato hodnota identifikátor federační služby v místní služby AD FS konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9f731-111">By default, Azure AD sets this to the value of the federation service identifier in your on-premises AD FS configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="9f731-112">Identifikátor služby FS je identifikátor URI, který jednoznačně identifikuje federační služby.</span><span class="sxs-lookup"><span data-stu-id="9f731-112">The federation service identifier is a URI that uniquely identifies a federation service.</span></span>  <span data-ttu-id="9f731-113">Služby federation service je instance služby AD FS, která slouží jako služba tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="9f731-113">The federation service is an instance of AD FS that functions as the security token service.</span></span> 
> 
> 

<span data-ttu-id="9f731-114">Pomocí příkazu prostředí PowerShell můžete zobrazení IssuerUri `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span><span class="sxs-lookup"><span data-stu-id="9f731-114">You can vew IssuerUri by using the PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="9f731-116">Problému dojde, pokud chcete přidat více než jedné domény nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="9f731-116">A problem arises when we want to add more than one top-level domain.</span></span>  <span data-ttu-id="9f731-117">Předpokládejme například, že máte instalace federace mezi službou Azure AD a místní prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f731-117">For example, let's say you have setup federation between Azure AD and your on-premises environment.</span></span>  <span data-ttu-id="9f731-118">K tomuto dokumentu používám bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="9f731-118">For this document I am using bmcontoso.com.</span></span>  <span data-ttu-id="9f731-119">Teď přidali domény druhé, nejvyšší úrovně, bmfabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="9f731-119">Now I have added a second, top-level domain, bmfabrikam.com.</span></span>

![Domény](./media/active-directory-multiple-domains/domains.png)

<span data-ttu-id="9f731-121">Když jsme se pokusí převést naše bmfabrikam.com domény na federovanou, jsme k chybě.</span><span class="sxs-lookup"><span data-stu-id="9f731-121">When we attempt to convert our bmfabrikam.com domain to be federated, we receive an error.</span></span>  <span data-ttu-id="9f731-122">Důvodem je, Azure AD má omezení, který neumožňuje vlastnost IssuerUri do mají stejnou hodnotu pro více než jedné domény.</span><span class="sxs-lookup"><span data-stu-id="9f731-122">The reason for this is, Azure AD has a constraint that does not allow the IssuerUri property to have the same value for more than one domain.</span></span>  

![Chyba Federation](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a><span data-ttu-id="9f731-124">Parametr SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="9f731-124">SupportMultipleDomain Parameter</span></span>
<span data-ttu-id="9f731-125">Chcete-li vyřešit, je potřeba přidat jiné IssuerUri, což lze provést pomocí `-SupportMultipleDomain` parametr.</span><span class="sxs-lookup"><span data-stu-id="9f731-125">To workaround this, we need to add a different IssuerUri which can be done by using the `-SupportMultipleDomain` parameter.</span></span>  <span data-ttu-id="9f731-126">Tento parametr se používá s následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="9f731-126">This parameter is used with the following cmdlets:</span></span>

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

<span data-ttu-id="9f731-127">Tento parametr umožňuje nakonfigurovat IssuerUri tak, aby je založen na názvu domény služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f731-127">This parameter makes Azure AD configure the IssuerUri so that it is based on the name of the domain.</span></span>  <span data-ttu-id="9f731-128">Toto je jedinečný mezi adresářů ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f731-128">This will be unique across directories in Azure AD.</span></span>  <span data-ttu-id="9f731-129">Pomocí parametru umožňuje příkaz prostředí PowerShell úspěšně dokončit.</span><span class="sxs-lookup"><span data-stu-id="9f731-129">Using the parameter allows the PowerShell command to complete successfully.</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/convert.png)

<span data-ttu-id="9f731-131">Prohlížení nastavení naší nové bmfabrikam.com domény můžete v tomto tématu:</span><span class="sxs-lookup"><span data-stu-id="9f731-131">Looking at the settings of our new bmfabrikam.com domain you can see the following:</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/settings.png)

<span data-ttu-id="9f731-133">Všimněte si, že `-SupportMultipleDomain` nezmění ostatní koncové body, které jsou stále nakonfigurované tak, aby odkazoval na našem federační službu na adfs.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="9f731-133">Note that `-SupportMultipleDomain` does not change the other endpoints which are still configured to point to our federation service on adfs.bmcontoso.com.</span></span>

<span data-ttu-id="9f731-134">Dalším krokem, `-SupportMultipleDomain` nemá je, že zajišťuje, že v systému služby AD FS obsahuje správnou hodnotu vystavitele v tokeny vydané pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f731-134">Another thing that `-SupportMultipleDomain` does is that it ensures that the AD FS system includes the proper Issuer value in tokens issued for Azure AD.</span></span> <span data-ttu-id="9f731-135">Dělá to tak, že část domény uživatelů (UPN) Toto nastavení jako doménu v IssuerUri, tj. přípona https://{upn} / adfs/services/vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="9f731-135">It does this by taking the domain portion of the users UPN and setting this as the domain in the IssuerUri, i.e. https://{upn suffix}/adfs/services/trust.</span></span> 

<span data-ttu-id="9f731-136">Proto při ověřování do služby Azure AD nebo Office 365, element IssuerUri v token uživatele se používá k vyhledání domény ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f731-136">Thus during authentication to Azure AD or Office 365, the IssuerUri element in the user’s token is used to locate the domain in Azure AD.</span></span>  <span data-ttu-id="9f731-137">Pokud odpovídající nelze nalézt, ověření se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9f731-137">If a match cannot be found the authentication will fail.</span></span> 

<span data-ttu-id="9f731-138">Například pokud uživatele (UPN) je bsimon@bmcontoso.com, IssuerUri element v tokenu AD FS problémů bude nastavena pro http://bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="9f731-138">For example, if a user’s UPN is bsimon@bmcontoso.com, the IssuerUri element in the token AD FS issues will be set to http://bmcontoso.com/adfs/services/trust.</span></span> <span data-ttu-id="9f731-139">To bude odpovídat konfiguraci Azure AD a ověřování bude úspěšný.</span><span class="sxs-lookup"><span data-stu-id="9f731-139">This will match the Azure AD configuration, and authentication will succeed.</span></span>

<span data-ttu-id="9f731-140">Toto je pravidlo vlastní deklarace identity, který implementuje tuto logiku:</span><span class="sxs-lookup"><span data-stu-id="9f731-140">The following is the customized claim rule that implements this logic:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> <span data-ttu-id="9f731-141">Chcete-li použít přepínač - SupportMultipleDomain při pokusu přidat nové nebo převést již přidání domény, musíte mít vaše federovaného vztahu důvěryhodnosti, které je podporují původně instalační program.</span><span class="sxs-lookup"><span data-stu-id="9f731-141">In order to use the -SupportMultipleDomain switch when attempting to add new or convert already added domains, you need to have setup your federated trust to support them originally.</span></span>  
> 
> 

## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a><span data-ttu-id="9f731-142">Postup aktualizace vztah důvěryhodnosti mezi AD FS a Azure</span><span class="sxs-lookup"><span data-stu-id="9f731-142">How to update the trust between AD FS and Azure AD</span></span>
<span data-ttu-id="9f731-143">Pokud jste nenastavili federovaný vztah důvěryhodnosti mezi AD FS a vaší instanci Azure AD, musíte znovu vytvořit tohoto vztahu důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="9f731-143">If you did not setup the federated trust between AD FS and your instance of Azure AD, you may need to re-create this trust.</span></span>  <span data-ttu-id="9f731-144">Důvodem je, že když je původně instalační program bez `-SupportMultipleDomain` parametr, výchozí hodnotou je nastaven IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="9f731-144">This is because, when it is originally setup without the `-SupportMultipleDomain` parameter, the IssuerUri is set with the default value.</span></span>  <span data-ttu-id="9f731-145">Na snímku obrazovky níže můžete zobrazit, jestli že issueruri nastavený na https://adfs.bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="9f731-145">In the screenshot below you can see the IssuerUri is set to https://adfs.bmcontoso.com/adfs/services/trust.</span></span>

<span data-ttu-id="9f731-146">Takže teď, když jsme úspěšně přidali nové domény na portálu Azure AD a pak se pokusíte převést pomocí `Convert-MsolDomaintoFederated -DomainName <your domain>`, jsme zobrazí následující chyba.</span><span class="sxs-lookup"><span data-stu-id="9f731-146">So now, if we have successfully added an new domain in the Azure AD portal and then attempt to convert it using `Convert-MsolDomaintoFederated -DomainName <your domain>`, we get the following error.</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/trust1.png)

<span data-ttu-id="9f731-148">Pokud se pokusíte přidat `-SupportMultipleDomain` přepínač jsme se zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="9f731-148">If you try to add the `-SupportMultipleDomain` switch we will receive the following error:</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/trust2.png)

<span data-ttu-id="9f731-150">Jednoduše pokusu o spuštění `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` v původní doméně také dojde chybě.</span><span class="sxs-lookup"><span data-stu-id="9f731-150">Simply trying to run `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` on the original domain will also result in an error.</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/trust3.png)

<span data-ttu-id="9f731-152">Použijte uvedený postup pro přidání další domény nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="9f731-152">Use the steps below to add an additional top-level domain.</span></span>  <span data-ttu-id="9f731-153">Pokud jste již přidali domény a nepoužili `-SupportMultipleDomain` parametr začněte s kroky pro odebírání a aktualizace vašeho původního domény.</span><span class="sxs-lookup"><span data-stu-id="9f731-153">If you have already added a domain and did not use the `-SupportMultipleDomain` parameter start with the steps for removing and updating your original domain.</span></span>  <span data-ttu-id="9f731-154">Pokud jste nepřidali doména nejvyšší úrovně ještě můžete začít s kroky pro přidání domény pomocí prostředí PowerShell Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="9f731-154">If you have not added a top-level domain yet you can start with the steps for adding a domain using PowerShell of Azure AD Connect.</span></span>

<span data-ttu-id="9f731-155">Pomocí následujících kroků odeberte vztah důvěryhodnosti Microsoft Online a aktualizujte vaší původní doméně.</span><span class="sxs-lookup"><span data-stu-id="9f731-155">Use the following steps to remove the Microsoft Online trust and update your original domain.</span></span>

1. <span data-ttu-id="9f731-156">Na federačním serveru služby AD FS otevřete **Správa služby AD FS.**</span><span class="sxs-lookup"><span data-stu-id="9f731-156">On your AD FS federation server open **AD FS Management.**</span></span> 
2. <span data-ttu-id="9f731-157">Na levé straně, rozbalte položku **vztahy důvěryhodnosti** a **vztahy důvěryhodnosti předávající strany**</span><span class="sxs-lookup"><span data-stu-id="9f731-157">On the left, expand **Trust Relationships** and **Relying Party Trusts**</span></span>
3. <span data-ttu-id="9f731-158">Na pravé straně, odstraňte **Microsoft Office 365 Identity Platform** položku.</span><span class="sxs-lookup"><span data-stu-id="9f731-158">On the right, delete the **Microsoft Office 365 Identity Platform** entry.</span></span>
   <span data-ttu-id="9f731-159">![Odeberte Online Microsoft](./media/active-directory-multiple-domains/trust4.png)</span><span class="sxs-lookup"><span data-stu-id="9f731-159">![Remove Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span></span>
4. <span data-ttu-id="9f731-160">Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spusťte následující příkaz: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="9f731-160">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run the following: `$cred=Get-Credential`.</span></span>  
5. <span data-ttu-id="9f731-161">Zadejte uživatelské jméno a heslo pro globálního správce pro doménu služby Azure AD, které jsou federaci s</span><span class="sxs-lookup"><span data-stu-id="9f731-161">Enter the username and password of a global administrator for the Azure AD domain you are federating with</span></span>
6. <span data-ttu-id="9f731-162">V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="9f731-162">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
7. <span data-ttu-id="9f731-163">V prostředí PowerShell zadejte `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span><span class="sxs-lookup"><span data-stu-id="9f731-163">In PowerShell enter `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span></span>  <span data-ttu-id="9f731-164">Je to pro původní doméně.</span><span class="sxs-lookup"><span data-stu-id="9f731-164">This is for the original domain.</span></span>  <span data-ttu-id="9f731-165">Proto pomocí výše uvedených domén, které má být:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span><span class="sxs-lookup"><span data-stu-id="9f731-165">So using the above domains it would be:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span></span>

<span data-ttu-id="9f731-166">Použijte následující postup pro přidání nové domény nejvyšší úrovně pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f731-166">Use the following steps to add the new top-level domain using PowerShell</span></span>

1. <span data-ttu-id="9f731-167">Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spusťte následující příkaz: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="9f731-167">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run the following: `$cred=Get-Credential`.</span></span>  
2. <span data-ttu-id="9f731-168">Zadejte uživatelské jméno a heslo pro globálního správce pro doménu služby Azure AD, které jsou federaci s</span><span class="sxs-lookup"><span data-stu-id="9f731-168">Enter the username and password of a global administrator for the Azure AD domain you are federating with</span></span>
3. <span data-ttu-id="9f731-169">V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="9f731-169">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
4. <span data-ttu-id="9f731-170">V prostředí PowerShell zadejte`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span><span class="sxs-lookup"><span data-stu-id="9f731-170">In PowerShell enter `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span></span>

<span data-ttu-id="9f731-171">Použijte následující postup pro přidání nové domény nejvyšší úrovně pomocí služby Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="9f731-171">Use the following steps to add the new top-level domain using Azure AD Connect.</span></span>

1. <span data-ttu-id="9f731-172">Spusťte Azure AD Connect z plochy a nabídky start</span><span class="sxs-lookup"><span data-stu-id="9f731-172">Launch Azure AD Connect from the desktop or start menu</span></span>
2. <span data-ttu-id="9f731-173">Zvolte "Přidat další domény Azure AD" ![přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add1.png)</span><span class="sxs-lookup"><span data-stu-id="9f731-173">Choose “Add an additional Azure AD Domain” ![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add1.png)</span></span>
3. <span data-ttu-id="9f731-174">Zadejte služby Azure AD a pověření služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f731-174">Enter your Azure AD and Active Directory credentials</span></span>
4. <span data-ttu-id="9f731-175">Vyberte druhý doménu, kterou chcete nakonfigurovat pro federaci.</span><span class="sxs-lookup"><span data-stu-id="9f731-175">Select the second domain you wish to configure for federation.</span></span>
   <span data-ttu-id="9f731-176">![Přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add2.png)</span><span class="sxs-lookup"><span data-stu-id="9f731-176">![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add2.png)</span></span>
5. <span data-ttu-id="9f731-177">Kliknutím na tlačítko Instalovat</span><span class="sxs-lookup"><span data-stu-id="9f731-177">Click Install</span></span>

### <a name="verify-the-new-top-level-domain"></a><span data-ttu-id="9f731-178">Ověření nové domény nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="9f731-178">Verify the new top-level domain</span></span>
<span data-ttu-id="9f731-179">Pomocí příkazu prostředí PowerShell `Get-MsolDomainFederationSettings -DomainName <your domain>`můžete zobrazit aktualizované IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="9f731-179">By using the PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`you can view the updated IssuerUri.</span></span>  <span data-ttu-id="9f731-180">Následující snímek obrazovky ukazuje federace, nastavení bylo aktualizováno na našem původní http://bmcontoso.com/adfs/services/trust domény</span><span class="sxs-lookup"><span data-stu-id="9f731-180">The screenshot below shows the federation settings were updated on our original domain http://bmcontoso.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="9f731-182">A IssuerUri v naší nové domény byla nastavena na https://bmfabrikam.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="9f731-182">And the IssuerUri on our new domain has been set to https://bmfabrikam.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a><span data-ttu-id="9f731-184">Podpora pro subdomén</span><span class="sxs-lookup"><span data-stu-id="9f731-184">Support for Sub-domains</span></span>
<span data-ttu-id="9f731-185">Když přidáte subdomény, z důvodu domén způsobem Azure AD, které jsou zpracovány, zdědí nastavení nadřazeného objektu.</span><span class="sxs-lookup"><span data-stu-id="9f731-185">When you add a sub-domain, because of the way Azure AD handled domains, it will inherit the settings of the parent.</span></span>  <span data-ttu-id="9f731-186">To znamená, že IssuerUri musí odpovídat nadřazené položky.</span><span class="sxs-lookup"><span data-stu-id="9f731-186">This means that the IssuerUri needs to match the parents.</span></span>

<span data-ttu-id="9f731-187">To umožňuje vyslovte například, že mám bmcontoso.com a poté přidejte corp.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="9f731-187">So lets say for example that I have bmcontoso.com and then add corp.bmcontoso.com.</span></span>  <span data-ttu-id="9f731-188">To znamená, že bude nutné IssuerUri pro uživatele z corp.bmcontoso.com **http://bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="9f731-188">This means that the IssuerUri for a user from corp.bmcontoso.com will need to be **http://bmcontoso.com/adfs/services/trust.**</span></span>  <span data-ttu-id="9f731-189">Ale standardní pravidlo implementována výše pro Azure AD, vygeneruje token s vystavitele jako **http://corp.bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="9f731-189">However the standard rule implemented above for Azure AD, will generate a token with an issuer as **http://corp.bmcontoso.com/adfs/services/trust.**</span></span> <span data-ttu-id="9f731-190">ověření se nezdaří a která nebude shodovala s doménou vyžadované hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9f731-190">which will not match the domain's required value and authentication will fail.</span></span>

### <a name="how-to-enable-support-for-sub-domains"></a><span data-ttu-id="9f731-191">Povolení podpory pro subdomén</span><span class="sxs-lookup"><span data-stu-id="9f731-191">How To enable support for sub-domains</span></span>
<span data-ttu-id="9f731-192">Chcete-li tento problém obejít služby AD FS je třeba aktualizovat vztah důvěryhodnosti předávající strany pro Microsoft Online.</span><span class="sxs-lookup"><span data-stu-id="9f731-192">In order to work around this the AD FS relying party trust for Microsoft Online needs to be updated.</span></span>  <span data-ttu-id="9f731-193">To pokud chcete udělat, musíte nakonfigurovat vlastní pravidlo deklarace identity, aby ji odstraní vypnout všechny subdomén od uživatele (UPN) příponu při vytváření vlastní hodnotu vystavitele.</span><span class="sxs-lookup"><span data-stu-id="9f731-193">To do this, you must configure a custom claim rule so that it strips off any sub-domains from the user’s UPN suffix when constructing the custom Issuer value.</span></span> 

<span data-ttu-id="9f731-194">Následující deklarace identity se to udělat:</span><span class="sxs-lookup"><span data-stu-id="9f731-194">The following claim will do this:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
<span data-ttu-id="9f731-195">Poslední číslo v regulárním výrazu nastavit kolik nadřazené domény v kořenové doméně.</span><span class="sxs-lookup"><span data-stu-id="9f731-195">The last number in the regular expression set the how many parent domains there is in your root domain.</span></span> <span data-ttu-id="9f731-196">I tady mají bmcontoso.com, takže jsou potřebné dvě nadřazené domény.</span><span class="sxs-lookup"><span data-stu-id="9f731-196">Here i have bmcontoso.com so two parent domains are necessary.</span></span> <span data-ttu-id="9f731-197">V případě, že tři nadřazené domény byly budou muset zůstat (tj.: corp.bmcontoso.com), pak číslo by byl tři.</span><span class="sxs-lookup"><span data-stu-id="9f731-197">If three parent domains were to be kept (i.e.: corp.bmcontoso.com), then the number would have been three.</span></span> <span data-ttu-id="9f731-198">Eventualy rozsah můžete zadat shody bude vždy provést tak, aby odpovídala maximální počet domén.</span><span class="sxs-lookup"><span data-stu-id="9f731-198">Eventualy a range can be indicated, the match will always be made to match the maximum of domains.</span></span> <span data-ttu-id="9f731-199">"{2,3}" bude odpovídat dvě až tři domény (např: bmfabrikam.com a corp.bmcontoso.com).</span><span class="sxs-lookup"><span data-stu-id="9f731-199">"{2,3}" will match two to three domains (i.e.: bmfabrikam.com and corp.bmcontoso.com).</span></span>

<span data-ttu-id="9f731-200">Pomocí následujících kroků přidáte vlastních deklarací identity pro podporu dílčím doménám domény.</span><span class="sxs-lookup"><span data-stu-id="9f731-200">Use the following steps to add a custom claim to support sub-domains.</span></span>

1. <span data-ttu-id="9f731-201">Správa služby otevřete AD FS</span><span class="sxs-lookup"><span data-stu-id="9f731-201">Open AD FS Management</span></span>
2. <span data-ttu-id="9f731-202">Klikněte pravým tlačítkem na vztah důvěryhodnosti Microsoft Online RP a zvolte pravidla upravit deklarace identity</span><span class="sxs-lookup"><span data-stu-id="9f731-202">Right click the Microsoft Online RP trust and choose Edit Claim rules</span></span>
3. <span data-ttu-id="9f731-203">Vyberte třetí pravidlo deklarace identity a nahraďte ![úpravy deklarací identity](./media/active-directory-multiple-domains/sub1.png)</span><span class="sxs-lookup"><span data-stu-id="9f731-203">Select the third claim rule, and replace ![Edit claim](./media/active-directory-multiple-domains/sub1.png)</span></span>
4. <span data-ttu-id="9f731-204">Nahraďte aktuální deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="9f731-204">Replace the current claim:</span></span>
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Nahraďte deklarace identity](./media/active-directory-multiple-domains/sub2.png)

5. <span data-ttu-id="9f731-206">Klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="9f731-206">Click Ok.</span></span>  <span data-ttu-id="9f731-207">Kliknutím na tlačítko použít.</span><span class="sxs-lookup"><span data-stu-id="9f731-207">Click Apply.</span></span>  <span data-ttu-id="9f731-208">Klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="9f731-208">Click Ok.</span></span>  <span data-ttu-id="9f731-209">Zavřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="9f731-209">Close AD FS Management.</span></span>

