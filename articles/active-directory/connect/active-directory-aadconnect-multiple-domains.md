---
title: "aaaAzure AD připojit víc domén"
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
ms.openlocfilehash: 91d87875ceacee4e34f132938e4bb0294fb954e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a><span data-ttu-id="64a8c-103">Podpora více domén pro federaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="64a8c-103">Multiple Domain Support for Federating with Azure AD</span></span>
<span data-ttu-id="64a8c-104">Hello následující dokumentace obsahuje pokyny toouse více nejvyšší úrovně domén a subdomén při federaci s Office 365 nebo Azure AD domén.</span><span class="sxs-lookup"><span data-stu-id="64a8c-104">hello following documentation provides guidance on how toouse multiple top-level domains and sub-domains when federating with Office 365 or Azure AD domains.</span></span>

## <a name="multiple-top-level-domain-support"></a><span data-ttu-id="64a8c-105">Podpora více domén nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="64a8c-105">Multiple top-level domain support</span></span>
<span data-ttu-id="64a8c-106">Federaci několika, domény nejvyšší úrovně s Azure AD vyžaduje určitou další konfiguraci, který není nutný, pokud federaci s jednu doménu nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="64a8c-106">Federating multiple, top-level domains with Azure AD requires some additional configuration that is not required when federating with one top-level domain.</span></span>

<span data-ttu-id="64a8c-107">Když domény federovaný pomocí služby Azure AD, několik vlastností, které jsou nastavené na hello domény v Azure.</span><span class="sxs-lookup"><span data-stu-id="64a8c-107">When a domain is federated with Azure AD, several properties are set on hello domain in Azure.</span></span>  <span data-ttu-id="64a8c-108">Jeden z nich důležité je IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="64a8c-108">One important one is IssuerUri.</span></span>  <span data-ttu-id="64a8c-109">Toto je identifikátor URI, který se používá ve službě Azure AD tooidentify hello domény, který hello tokenu je přidružený.</span><span class="sxs-lookup"><span data-stu-id="64a8c-109">This is a URI that is used by Azure AD tooidentify hello domain that hello token is associated with.</span></span>  <span data-ttu-id="64a8c-110">Hello URI nepotřebuje tooresolve tooanything ale musí být platný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="64a8c-110">hello URI doesn’t need tooresolve tooanything but it must be a valid URI.</span></span>  <span data-ttu-id="64a8c-111">Ve výchozím nastavení, Azure AD Nastaví hodnota toohello identifikátor služby FS hello v místní služby AD FS konfigurace.</span><span class="sxs-lookup"><span data-stu-id="64a8c-111">By default, Azure AD sets this toohello value of hello federation service identifier in your on-premises AD FS configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="64a8c-112">identifikátor služby FS Hello je identifikátor URI, který jednoznačně identifikuje federační služby.</span><span class="sxs-lookup"><span data-stu-id="64a8c-112">hello federation service identifier is a URI that uniquely identifies a federation service.</span></span>  <span data-ttu-id="64a8c-113">Hello federační služby je instance služby AD FS, která funguje jako hello služby tokenů zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="64a8c-113">hello federation service is an instance of AD FS that functions as hello security token service.</span></span> 
> 
> 

<span data-ttu-id="64a8c-114">Zobrazení IssuerUri můžete pomocí příkazu Powershellu hello `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span><span class="sxs-lookup"><span data-stu-id="64a8c-114">You can vew IssuerUri by using hello PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`.</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="64a8c-116">Problém mohou nastat, pokud chceme tooadd více než jedné domény nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="64a8c-116">A problem arises when we want tooadd more than one top-level domain.</span></span>  <span data-ttu-id="64a8c-117">Předpokládejme například, že máte instalace federace mezi službou Azure AD a místní prostředí.</span><span class="sxs-lookup"><span data-stu-id="64a8c-117">For example, let's say you have setup federation between Azure AD and your on-premises environment.</span></span>  <span data-ttu-id="64a8c-118">K tomuto dokumentu používám bmcontoso.com.  Teď přidali domény druhé, nejvyšší úrovně, bmfabrikam.com.</span><span class="sxs-lookup"><span data-stu-id="64a8c-118">For this document I am using bmcontoso.com.  Now I have added a second, top-level domain, bmfabrikam.com.</span></span>

![Domény](./media/active-directory-multiple-domains/domains.png)

<span data-ttu-id="64a8c-120">Když jsme pokus tooconvert naše toobe domény bmfabrikam.com federovaný, jsme k chybě.</span><span class="sxs-lookup"><span data-stu-id="64a8c-120">When we attempt tooconvert our bmfabrikam.com domain toobe federated, we receive an error.</span></span>  <span data-ttu-id="64a8c-121">Hello důvodem je, Azure AD má omezení, který neumožňuje hello IssuerUri vlastnost toohave hello stejnou hodnotu pro více než jedné domény.</span><span class="sxs-lookup"><span data-stu-id="64a8c-121">hello reason for this is, Azure AD has a constraint that does not allow hello IssuerUri property toohave hello same value for more than one domain.</span></span>  

![Chyba Federation](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a><span data-ttu-id="64a8c-123">Parametr SupportMultipleDomain</span><span class="sxs-lookup"><span data-stu-id="64a8c-123">SupportMultipleDomain Parameter</span></span>
<span data-ttu-id="64a8c-124">tooworkaround, potřebujeme tooadd různých IssuerUri, což lze provést pomocí hello `-SupportMultipleDomain` parametr.</span><span class="sxs-lookup"><span data-stu-id="64a8c-124">tooworkaround this, we need tooadd a different IssuerUri which can be done by using hello `-SupportMultipleDomain` parameter.</span></span>  <span data-ttu-id="64a8c-125">Tento parametr se používá s hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="64a8c-125">This parameter is used with hello following cmdlets:</span></span>

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

<span data-ttu-id="64a8c-126">Tento parametr umožňuje nakonfigurovat hello IssuerUri tak, aby je založena na hello název domény hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64a8c-126">This parameter makes Azure AD configure hello IssuerUri so that it is based on hello name of hello domain.</span></span>  <span data-ttu-id="64a8c-127">Toto je jedinečný mezi adresářů ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64a8c-127">This will be unique across directories in Azure AD.</span></span>  <span data-ttu-id="64a8c-128">Pomocí parametru hello úspěšně umožňuje toocomplete příkaz prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="64a8c-128">Using hello parameter allows hello PowerShell command toocomplete successfully.</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/convert.png)

<span data-ttu-id="64a8c-130">Prohlížení nastavení hello naší nové domény bmfabrikam.com můžete zjistit hello následující:</span><span class="sxs-lookup"><span data-stu-id="64a8c-130">Looking at hello settings of our new bmfabrikam.com domain you can see hello following:</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/settings.png)

<span data-ttu-id="64a8c-132">Všimněte si, že `-SupportMultipleDomain` nezmění hello ostatní koncové body, které jsou stále probíhá konfigurace toopoint tooour federační službu na adfs.bmcontoso.com.</span><span class="sxs-lookup"><span data-stu-id="64a8c-132">Note that `-SupportMultipleDomain` does not change hello other endpoints which are still configured toopoint tooour federation service on adfs.bmcontoso.com.</span></span>

<span data-ttu-id="64a8c-133">Dalším krokem, `-SupportMultipleDomain` nemá je, že zajišťuje, že systém hello AD FS obsahuje správnou hodnotu vystavitele hello v tokeny vydané pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64a8c-133">Another thing that `-SupportMultipleDomain` does is that it ensures that hello AD FS system includes hello proper Issuer value in tokens issued for Azure AD.</span></span> <span data-ttu-id="64a8c-134">Dělá to tak, že část domény hello hello uživatele (UPN) Toto nastavení jako hello domény v hello IssuerUri, tj. přípona https://{upn} / adfs/services/vztah důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="64a8c-134">It does this by taking hello domain portion of hello users UPN and setting this as hello domain in hello IssuerUri, i.e. https://{upn suffix}/adfs/services/trust.</span></span> 

<span data-ttu-id="64a8c-135">Proto při ověřování tooAzure AD nebo Office 365, hello IssuerUri element v token uživatele hello je použité toolocate hello domény ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64a8c-135">Thus during authentication tooAzure AD or Office 365, hello IssuerUri element in hello user’s token is used toolocate hello domain in Azure AD.</span></span>  <span data-ttu-id="64a8c-136">Pokud odpovídající nelze nalézt hello ověřování se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="64a8c-136">If a match cannot be found hello authentication will fail.</span></span> 

<span data-ttu-id="64a8c-137">Například pokud uživatele (UPN) je bsimon@bmcontoso.com, toohttp://bmcontoso.com/adfs/services/trust bude nastavena hello IssuerUri element v hello tokenu služby AD FS problémy.</span><span class="sxs-lookup"><span data-stu-id="64a8c-137">For example, if a user’s UPN is bsimon@bmcontoso.com, hello IssuerUri element in hello token AD FS issues will be set toohttp://bmcontoso.com/adfs/services/trust.</span></span> <span data-ttu-id="64a8c-138">To bude odpovídat hello konfigurace Azure AD a ověřování bude úspěšný.</span><span class="sxs-lookup"><span data-stu-id="64a8c-138">This will match hello Azure AD configuration, and authentication will succeed.</span></span>

<span data-ttu-id="64a8c-139">Následující Hello je pravidlo hello vlastní deklarace identity, který implementuje tuto logiku:</span><span class="sxs-lookup"><span data-stu-id="64a8c-139">hello following is hello customized claim rule that implements this logic:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> <span data-ttu-id="64a8c-140">V pořadí toouse hello - SupportMultipleDomain přepínače při pokusu o nové tooadd nebo převést již přidali domén, budete potřebovat toohave nastavení vaší toosupport federovaného vztahu důvěryhodnosti je původně.</span><span class="sxs-lookup"><span data-stu-id="64a8c-140">In order toouse hello -SupportMultipleDomain switch when attempting tooadd new or convert already added domains, you need toohave setup your federated trust toosupport them originally.</span></span>  
> 
> 

## <a name="how-tooupdate-hello-trust-between-ad-fs-and-azure-ad"></a><span data-ttu-id="64a8c-141">Jak tooupdate hello vztah důvěryhodnosti mezi AD FS a Azure</span><span class="sxs-lookup"><span data-stu-id="64a8c-141">How tooupdate hello trust between AD FS and Azure AD</span></span>
<span data-ttu-id="64a8c-142">Pokud jste nenastavili hello federovaný vztah důvěryhodnosti mezi AD FS a vaší instanci Azure AD, pravděpodobně bude třeba toore – vytvoření tohoto vztahu důvěryhodnosti.</span><span class="sxs-lookup"><span data-stu-id="64a8c-142">If you did not setup hello federated trust between AD FS and your instance of Azure AD, you may need toore-create this trust.</span></span>  <span data-ttu-id="64a8c-143">Důvodem je, že když je původně instalační program bez hello `-SupportMultipleDomain` parametr hello IssuerUri nastavena výchozí hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="64a8c-143">This is because, when it is originally setup without hello `-SupportMultipleDomain` parameter, hello IssuerUri is set with hello default value.</span></span>  <span data-ttu-id="64a8c-144">V hello – snímek obrazovky níže se zobrazují, jestli je nastavený hello IssuerUri toohttps://adfs.bmcontoso.com/adfs/services/trust.</span><span class="sxs-lookup"><span data-stu-id="64a8c-144">In hello screenshot below you can see hello IssuerUri is set toohttps://adfs.bmcontoso.com/adfs/services/trust.</span></span>

<span data-ttu-id="64a8c-145">Takže teď, když jsme úspěšně přidali novou doménu portálu hello Azure AD a pokusíte se tooconvert pomocí `Convert-MsolDomaintoFederated -DomainName <your domain>`, se nám získat hello následující chyba.</span><span class="sxs-lookup"><span data-stu-id="64a8c-145">So now, if we have successfully added an new domain in hello Azure AD portal and then attempt tooconvert it using `Convert-MsolDomaintoFederated -DomainName <your domain>`, we get hello following error.</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/trust1.png)

<span data-ttu-id="64a8c-147">Pokud se pokusíte tooadd hello `-SupportMultipleDomain` přepínač obdržíme hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="64a8c-147">If you try tooadd hello `-SupportMultipleDomain` switch we will receive hello following error:</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/trust2.png)

<span data-ttu-id="64a8c-149">Jednoduše pokusu o toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` na hello původní domény dojde také chybě.</span><span class="sxs-lookup"><span data-stu-id="64a8c-149">Simply trying toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` on hello original domain will also result in an error.</span></span>

![Chyba Federation](./media/active-directory-multiple-domains/trust3.png)

<span data-ttu-id="64a8c-151">Pomocí kroků hello níže tooadd další domény nejvyšší úrovně.</span><span class="sxs-lookup"><span data-stu-id="64a8c-151">Use hello steps below tooadd an additional top-level domain.</span></span>  <span data-ttu-id="64a8c-152">Pokud jste už přidali domény a nepoužili hello `-SupportMultipleDomain` parametr začněte s hello kroky pro odebírání a aktualizace vašeho původního domény.</span><span class="sxs-lookup"><span data-stu-id="64a8c-152">If you have already added a domain and did not use hello `-SupportMultipleDomain` parameter start with hello steps for removing and updating your original domain.</span></span>  <span data-ttu-id="64a8c-153">Pokud jste nepřidali doména nejvyšší úrovně ještě můžete začít s hello kroky pro přidání domény pomocí prostředí PowerShell Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="64a8c-153">If you have not added a top-level domain yet you can start with hello steps for adding a domain using PowerShell of Azure AD Connect.</span></span>

<span data-ttu-id="64a8c-154">Použijte následující postup tooremove hello Microsoft Online důvěryhodnosti hello a aktualizace vašeho původního domény.</span><span class="sxs-lookup"><span data-stu-id="64a8c-154">Use hello following steps tooremove hello Microsoft Online trust and update your original domain.</span></span>

1. <span data-ttu-id="64a8c-155">Na federačním serveru služby AD FS otevřete **Správa služby AD FS.**</span><span class="sxs-lookup"><span data-stu-id="64a8c-155">On your AD FS federation server open **AD FS Management.**</span></span> 
2. <span data-ttu-id="64a8c-156">Na levé straně hello rozbalte **vztahy důvěryhodnosti** a **vztahy důvěryhodnosti předávající strany**</span><span class="sxs-lookup"><span data-stu-id="64a8c-156">On hello left, expand **Trust Relationships** and **Relying Party Trusts**</span></span>
3. <span data-ttu-id="64a8c-157">Na pravém hello, odstraňte hello **Microsoft Office 365 Identity Platform** položku.</span><span class="sxs-lookup"><span data-stu-id="64a8c-157">On hello right, delete hello **Microsoft Office 365 Identity Platform** entry.</span></span>
   <span data-ttu-id="64a8c-158">![Odeberte Online Microsoft](./media/active-directory-multiple-domains/trust4.png)</span><span class="sxs-lookup"><span data-stu-id="64a8c-158">![Remove Microsoft Online](./media/active-directory-multiple-domains/trust4.png)</span></span>
4. <span data-ttu-id="64a8c-159">Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spustit následující hello: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="64a8c-159">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run hello following: `$cred=Get-Credential`.</span></span>  
5. <span data-ttu-id="64a8c-160">Zadejte hello uživatelské jméno a heslo pro globálního správce pro doménu hello Azure AD, které jsou federaci s</span><span class="sxs-lookup"><span data-stu-id="64a8c-160">Enter hello username and password of a global administrator for hello Azure AD domain you are federating with</span></span>
6. <span data-ttu-id="64a8c-161">V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="64a8c-161">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
7. <span data-ttu-id="64a8c-162">V prostředí PowerShell zadejte `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span><span class="sxs-lookup"><span data-stu-id="64a8c-162">In PowerShell enter `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.</span></span>  <span data-ttu-id="64a8c-163">Toto je pro doménu původního hello.</span><span class="sxs-lookup"><span data-stu-id="64a8c-163">This is for hello original domain.</span></span>  <span data-ttu-id="64a8c-164">Výše domén, které má být použít hello:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span><span class="sxs-lookup"><span data-stu-id="64a8c-164">So using hello above domains it would be:  `Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`</span></span>

<span data-ttu-id="64a8c-165">Použijte následující postup tooadd hello nové nejvyšší úrovně domény pomocí prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="64a8c-165">Use hello following steps tooadd hello new top-level domain using PowerShell</span></span>

1. <span data-ttu-id="64a8c-166">Na počítači, který má [Azure Active Directory modul pro prostředí Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) nainstalován spustit následující hello: `$cred=Get-Credential`.</span><span class="sxs-lookup"><span data-stu-id="64a8c-166">On a machine that has [Azure Active Directory Module for Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) installed on it run hello following: `$cred=Get-Credential`.</span></span>  
2. <span data-ttu-id="64a8c-167">Zadejte hello uživatelské jméno a heslo pro globálního správce pro doménu hello Azure AD, které jsou federaci s</span><span class="sxs-lookup"><span data-stu-id="64a8c-167">Enter hello username and password of a global administrator for hello Azure AD domain you are federating with</span></span>
3. <span data-ttu-id="64a8c-168">V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`</span><span class="sxs-lookup"><span data-stu-id="64a8c-168">In PowerShell enter `Connect-MsolService -Credential $cred`</span></span>
4. <span data-ttu-id="64a8c-169">V prostředí PowerShell zadejte`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span><span class="sxs-lookup"><span data-stu-id="64a8c-169">In PowerShell enter `New-MsolFederatedDomain –SupportMultipleDomain –DomainName`</span></span>

<span data-ttu-id="64a8c-170">Pomocí následujících kroků tooadd hello nové nejvyšší úrovně domény pomocí služby Azure AD Connect hello.</span><span class="sxs-lookup"><span data-stu-id="64a8c-170">Use hello following steps tooadd hello new top-level domain using Azure AD Connect.</span></span>

1. <span data-ttu-id="64a8c-171">Spusťte Azure AD Connect z plochy hello a nabídky start</span><span class="sxs-lookup"><span data-stu-id="64a8c-171">Launch Azure AD Connect from hello desktop or start menu</span></span>
2. <span data-ttu-id="64a8c-172">Zvolte "Přidat další domény Azure AD" ![přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add1.png)</span><span class="sxs-lookup"><span data-stu-id="64a8c-172">Choose “Add an additional Azure AD Domain” ![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add1.png)</span></span>
3. <span data-ttu-id="64a8c-173">Zadejte služby Azure AD a pověření služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="64a8c-173">Enter your Azure AD and Active Directory credentials</span></span>
4. <span data-ttu-id="64a8c-174">Druhá doména vyberte hello chcete tooconfigure pro federaci.</span><span class="sxs-lookup"><span data-stu-id="64a8c-174">Select hello second domain you wish tooconfigure for federation.</span></span>
   <span data-ttu-id="64a8c-175">![Přidat další doménu služby Azure AD](./media/active-directory-multiple-domains/add2.png)</span><span class="sxs-lookup"><span data-stu-id="64a8c-175">![Add an additional Azure AD domain](./media/active-directory-multiple-domains/add2.png)</span></span>
5. <span data-ttu-id="64a8c-176">Kliknutím na tlačítko Instalovat</span><span class="sxs-lookup"><span data-stu-id="64a8c-176">Click Install</span></span>

### <a name="verify-hello-new-top-level-domain"></a><span data-ttu-id="64a8c-177">Ověření nové domény nejvyšší úrovně hello</span><span class="sxs-lookup"><span data-stu-id="64a8c-177">Verify hello new top-level domain</span></span>
<span data-ttu-id="64a8c-178">Pomocí příkazu Powershellu hello `Get-MsolDomainFederationSettings -DomainName <your domain>`můžete zobrazit hello aktualizovat IssuerUri.</span><span class="sxs-lookup"><span data-stu-id="64a8c-178">By using hello PowerShell command `Get-MsolDomainFederationSettings -DomainName <your domain>`you can view hello updated IssuerUri.</span></span>  <span data-ttu-id="64a8c-179">Následující snímek obrazovky Hello ukazuje, že nastavení federačního hello bylo aktualizováno na našem původní http://bmcontoso.com/adfs/services/trust domény</span><span class="sxs-lookup"><span data-stu-id="64a8c-179">hello screenshot below shows hello federation settings were updated on our original domain http://bmcontoso.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

<span data-ttu-id="64a8c-181">A hello IssuerUri v naší nové domény byl nastaven toohttps://bmfabrikam.com/adfs/services/trust</span><span class="sxs-lookup"><span data-stu-id="64a8c-181">And hello IssuerUri on our new domain has been set toohttps://bmfabrikam.com/adfs/services/trust</span></span>

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a><span data-ttu-id="64a8c-183">Podpora pro subdomén</span><span class="sxs-lookup"><span data-stu-id="64a8c-183">Support for Sub-domains</span></span>
<span data-ttu-id="64a8c-184">Když přidáte subdomény, z důvodu hello způsobem Azure AD zpracovává domén, zdědí nastavení hello nadřazeného hello.</span><span class="sxs-lookup"><span data-stu-id="64a8c-184">When you add a sub-domain, because of hello way Azure AD handled domains, it will inherit hello settings of hello parent.</span></span>  <span data-ttu-id="64a8c-185">To znamená, že hello IssuerUri musí toomatch hello nadřazené položky.</span><span class="sxs-lookup"><span data-stu-id="64a8c-185">This means that hello IssuerUri needs toomatch hello parents.</span></span>

<span data-ttu-id="64a8c-186">To umožňuje vyslovte například, že mám bmcontoso.com a poté přidejte corp.bmcontoso.com.  To znamená že hello IssuerUri pro uživatele z corp.bmcontoso.com bude potřebovat toobe **http://bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="64a8c-186">So lets say for example that I have bmcontoso.com and then add corp.bmcontoso.com.  This means that hello IssuerUri for a user from corp.bmcontoso.com will need toobe **http://bmcontoso.com/adfs/services/trust.**</span></span>  <span data-ttu-id="64a8c-187">Ale standardního pravidla hello implementovat výše pro Azure AD, bude vygenerovat token s vystavitele jako **http://corp.bmcontoso.com/adfs/services/trust.**</span><span class="sxs-lookup"><span data-stu-id="64a8c-187">However hello standard rule implemented above for Azure AD, will generate a token with an issuer as **http://corp.bmcontoso.com/adfs/services/trust.**</span></span> <span data-ttu-id="64a8c-188">které nebude odpovídat požadovaná hodnota hello domény a ověření se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="64a8c-188">which will not match hello domain's required value and authentication will fail.</span></span>

### <a name="how-tooenable-support-for-sub-domains"></a><span data-ttu-id="64a8c-189">Jak tooenable podpora subdomén</span><span class="sxs-lookup"><span data-stu-id="64a8c-189">How tooenable support for sub-domains</span></span>
<span data-ttu-id="64a8c-190">V pořadí toowork kolem tohoto hello vztah důvěryhodnosti předávající strany pro Microsoft Online služby AD FS musí toobe aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="64a8c-190">In order toowork around this hello AD FS relying party trust for Microsoft Online needs toobe updated.</span></span>  <span data-ttu-id="64a8c-191">toodo, musíte nakonfigurovat vlastní pravidlo deklarace identity, aby ji odstraní vypnout všechny subdomén z hello uživatele (UPN) příponu při vytváření vlastní hodnotu vystavitele hello.</span><span class="sxs-lookup"><span data-stu-id="64a8c-191">toodo this, you must configure a custom claim rule so that it strips off any sub-domains from hello user’s UPN suffix when constructing hello custom Issuer value.</span></span> 

<span data-ttu-id="64a8c-192">Hello následující deklarace identity se to udělat:</span><span class="sxs-lookup"><span data-stu-id="64a8c-192">hello following claim will do this:</span></span>

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
<span data-ttu-id="64a8c-193">číslo poslední Hello v regulárním výrazu hello nastavit hello kolik nadřazené domény v kořenové doméně.</span><span class="sxs-lookup"><span data-stu-id="64a8c-193">hello last number in hello regular expression set hello how many parent domains there is in your root domain.</span></span> <span data-ttu-id="64a8c-194">I tady mají bmcontoso.com, takže jsou potřebné dvě nadřazené domény.</span><span class="sxs-lookup"><span data-stu-id="64a8c-194">Here i have bmcontoso.com so two parent domains are necessary.</span></span> <span data-ttu-id="64a8c-195">Pokud tři nadřazené domény byly toobe zachovány (tj.: corp.bmcontoso.com), pak hello číslo by byl tři.</span><span class="sxs-lookup"><span data-stu-id="64a8c-195">If three parent domains were toobe kept (i.e.: corp.bmcontoso.com), then hello number would have been three.</span></span> <span data-ttu-id="64a8c-196">Můžete zadat Eventualy rozsah, hello shody bude vždy provést toomatch hello maximální počet domén.</span><span class="sxs-lookup"><span data-stu-id="64a8c-196">Eventualy a range can be indicated, hello match will always be made toomatch hello maximum of domains.</span></span> <span data-ttu-id="64a8c-197">"{2,3}" bude odpovídat dvěma doménami toothree (tj.: bmfabrikam.com a corp.bmcontoso.com).</span><span class="sxs-lookup"><span data-stu-id="64a8c-197">"{2,3}" will match two toothree domains (i.e.: bmfabrikam.com and corp.bmcontoso.com).</span></span>

<span data-ttu-id="64a8c-198">Pomocí následujících kroků tooadd hello vlastních deklarací identity toosupport subdomén.</span><span class="sxs-lookup"><span data-stu-id="64a8c-198">Use hello following steps tooadd a custom claim toosupport sub-domains.</span></span>

1. <span data-ttu-id="64a8c-199">Správa služby otevřete AD FS</span><span class="sxs-lookup"><span data-stu-id="64a8c-199">Open AD FS Management</span></span>
2. <span data-ttu-id="64a8c-200">Klikněte pravým tlačítkem na vztah důvěryhodnosti Microsoft Online RP hello a zvolte pravidla upravit deklarace identity</span><span class="sxs-lookup"><span data-stu-id="64a8c-200">Right click hello Microsoft Online RP trust and choose Edit Claim rules</span></span>
3. <span data-ttu-id="64a8c-201">Vyberte hello třetí pravidlo deklarace identity a nahraďte ![úpravy deklarací identity](./media/active-directory-multiple-domains/sub1.png)</span><span class="sxs-lookup"><span data-stu-id="64a8c-201">Select hello third claim rule, and replace ![Edit claim](./media/active-directory-multiple-domains/sub1.png)</span></span>
4. <span data-ttu-id="64a8c-202">Nahraďte hello aktuální deklarace identity:</span><span class="sxs-lookup"><span data-stu-id="64a8c-202">Replace hello current claim:</span></span>
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Nahraďte deklarace identity](./media/active-directory-multiple-domains/sub2.png)

5. <span data-ttu-id="64a8c-204">Klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="64a8c-204">Click Ok.</span></span>  <span data-ttu-id="64a8c-205">Kliknutím na tlačítko použít.</span><span class="sxs-lookup"><span data-stu-id="64a8c-205">Click Apply.</span></span>  <span data-ttu-id="64a8c-206">Klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="64a8c-206">Click Ok.</span></span>  <span data-ttu-id="64a8c-207">Zavřete správu služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="64a8c-207">Close AD FS Management.</span></span>

