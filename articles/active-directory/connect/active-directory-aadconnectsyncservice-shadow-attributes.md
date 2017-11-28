---
title: "aaaAzure AD Connect sync služby stínové atributy | Microsoft Docs"
description: "Popisuje, jak fungují stínové atributy ve službě Azure AD Connect sync service."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1b8665e7488c6078b655f8a3e35519145bacd898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a><span data-ttu-id="92fb2-103">Azure AD Connect sync služby stínové atributy</span><span class="sxs-lookup"><span data-stu-id="92fb2-103">Azure AD Connect sync service shadow attributes</span></span>
<span data-ttu-id="92fb2-104">Většina atributy jsou reprezentované hello stejný způsobem v Azure AD jsou ve vaší místní službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="92fb2-104">Most attributes are represented hello same way in Azure AD as they are in your on-premises Active Directory.</span></span> <span data-ttu-id="92fb2-105">Ale některé atributy mají některé zvláštní zpracování a hodnota atributu hello ve službě Azure AD může být jiná než co Azure AD Connect synchronizuje.</span><span class="sxs-lookup"><span data-stu-id="92fb2-105">But some attributes have some special handling and hello attribute value in Azure AD might be different than what Azure AD Connect synchronizes.</span></span>

## <a name="introducing-shadow-attributes"></a><span data-ttu-id="92fb2-106">Představení stínové atributy</span><span class="sxs-lookup"><span data-stu-id="92fb2-106">Introducing shadow attributes</span></span>
<span data-ttu-id="92fb2-107">Některé atributy mají dva reprezentace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92fb2-107">Some attributes have two representations in Azure AD.</span></span> <span data-ttu-id="92fb2-108">Hodnota místní hello a vypočítaná hodnota, která jsou uloženy.</span><span class="sxs-lookup"><span data-stu-id="92fb2-108">Both hello on-premises value and a calculated value are stored.</span></span> <span data-ttu-id="92fb2-109">Tyto další atributy, se nazývají stínové atributy.</span><span class="sxs-lookup"><span data-stu-id="92fb2-109">These extra attributes are called shadow attributes.</span></span> <span data-ttu-id="92fb2-110">Nejběžnější atributy Hello dva, kde uvidíte toto chování jsou **userPrincipalName** a **proxyAddress**.</span><span class="sxs-lookup"><span data-stu-id="92fb2-110">hello two most common attributes where you see this behavior are **userPrincipalName** and **proxyAddress**.</span></span> <span data-ttu-id="92fb2-111">Hello změnu hodnoty atributu se stane, když existují hodnoty v těchto atributech představující-ověření domény.</span><span class="sxs-lookup"><span data-stu-id="92fb2-111">hello change in attribute values happens when there are values in these attributes representing non-verified domains.</span></span> <span data-ttu-id="92fb2-112">Ale hello synchronizační modul v připojení čte hello hodnotu v atributu stínové hello tak z jeho perspektivy, hello atribut bylo potvrzeno službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="92fb2-112">But hello sync engine in Connect reads hello value in hello shadow attribute so from its perspective, hello attribute has been confirmed by Azure AD.</span></span>

<span data-ttu-id="92fb2-113">Nejde zobrazit hello stínové atributů s použitím hello Azure portálu nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92fb2-113">You cannot see hello shadow attributes using hello Azure portal or with PowerShell.</span></span> <span data-ttu-id="92fb2-114">Ale Principy hello koncept pomáhá vám tootroubleshoot určité scénáře kde hello atribut má jiné hodnoty na místě a v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="92fb2-114">But understanding hello concept helps you tootroubleshoot certain scenarios where hello attribute has different values on-premises and in hello cloud.</span></span>

<span data-ttu-id="92fb2-115">toobetter pochopit hello chování, podívejte se na tomto příkladu ze společnosti Fabrikam:</span><span class="sxs-lookup"><span data-stu-id="92fb2-115">toobetter understand hello behavior, look at this example from Fabrikam:</span></span>  
![Domény](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
<span data-ttu-id="92fb2-117">Mají více přípon UPN ve svojí místní službě Active Directory, ale jeden pouze ověřit.</span><span class="sxs-lookup"><span data-stu-id="92fb2-117">They have multiple UPN suffixes in their on-premises Active Directory, but they have only verified one.</span></span>

### <a name="userprincipalname"></a><span data-ttu-id="92fb2-118">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="92fb2-118">userPrincipalName</span></span>
<span data-ttu-id="92fb2-119">Uživatel, který má následující hodnoty atributu v doméně neověřený hello:</span><span class="sxs-lookup"><span data-stu-id="92fb2-119">A user has hello following attribute values in a non-verified domain:</span></span>

| <span data-ttu-id="92fb2-120">Atribut</span><span class="sxs-lookup"><span data-stu-id="92fb2-120">Attribute</span></span> | <span data-ttu-id="92fb2-121">Hodnota</span><span class="sxs-lookup"><span data-stu-id="92fb2-121">Value</span></span> |
| --- | --- |
| <span data-ttu-id="92fb2-122">userPrincipalName na místě</span><span class="sxs-lookup"><span data-stu-id="92fb2-122">on-premises userPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="92fb2-123">Azure AD shadowUserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="92fb2-123">Azure AD shadowUserPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="92fb2-124">Azure AD userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="92fb2-124">Azure AD userPrincipalName</span></span> | lee.sperry@fabrikam.onmicrosoft.com |

<span data-ttu-id="92fb2-125">atribut userPrincipalName Hello je hello hodnotu, která se zobrazí po pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="92fb2-125">hello userPrincipalName attribute is hello value you see when using PowerShell.</span></span>

<span data-ttu-id="92fb2-126">Vzhledem k tomu hodnota atributu skutečné místní hello je uložená ve službě Azure AD, když ověříte hello fabrikam.com domény, Azure AD aktualizuje atribut userPrincipalName hello s hodnotou hello z hello shadowUserPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="92fb2-126">Since hello real on-premises attribute value is stored in Azure AD, when you verify hello fabrikam.com domain, Azure AD updates hello userPrincipalName attribute with hello value from hello shadowUserPrincipalName.</span></span> <span data-ttu-id="92fb2-127">Nemáte toosynchronize změny z Azure AD Connect pro toobe tyto hodnoty aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="92fb2-127">You do not have toosynchronize any changes from Azure AD Connect for these values toobe updated.</span></span>

### <a name="proxyaddresses"></a><span data-ttu-id="92fb2-128">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="92fb2-128">proxyAddresses</span></span>
<span data-ttu-id="92fb2-129">Dobrý den, stejný proces pro pouze včetně ověřených domén rovněž nastane proxyAddresses, ale s některé další logiku.</span><span class="sxs-lookup"><span data-stu-id="92fb2-129">hello same process for only including verified domains also occurs for proxyAddresses, but with some additional logic.</span></span> <span data-ttu-id="92fb2-130">Zkontrolujte Hello ověřených domén se stane pouze pro poštovní schránky uživatele.</span><span class="sxs-lookup"><span data-stu-id="92fb2-130">hello check for verified domains only happens for mailbox users.</span></span> <span data-ttu-id="92fb2-131">Poštovní uživatele nebo kontaktujte představují uživateli v jiné organizaci Exchange a můžete přidat všechny hodnoty v proxyAddresses toothese objekty.</span><span class="sxs-lookup"><span data-stu-id="92fb2-131">A mail-enabled user or contact represent a user in another Exchange organization and you can add any values in proxyAddresses toothese objects.</span></span>

<span data-ttu-id="92fb2-132">Uživatel poštovní schránky, buď místní nebo v systému Exchange Online se zobrazí pouze hodnoty ověřených domén.</span><span class="sxs-lookup"><span data-stu-id="92fb2-132">For a mailbox user, either on-premises or in Exchange Online, only values for verified domains appear.</span></span> <span data-ttu-id="92fb2-133">Ho může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="92fb2-133">It could look like this:</span></span>

| <span data-ttu-id="92fb2-134">Atribut</span><span class="sxs-lookup"><span data-stu-id="92fb2-134">Attribute</span></span> | <span data-ttu-id="92fb2-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="92fb2-135">Value</span></span> |
| --- | --- |
| <span data-ttu-id="92fb2-136">místní proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="92fb2-136">on-premises proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| <span data-ttu-id="92fb2-137">Exchange Online proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="92fb2-137">Exchange Online proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

<span data-ttu-id="92fb2-138">V takovém případě  **smtp:abbie.spencer@fabrikam.com**  byla odebrána, vzhledem k tomu, že doména nebyla ověřena.</span><span class="sxs-lookup"><span data-stu-id="92fb2-138">In this case **smtp:abbie.spencer@fabrikam.com** was removed since that domain has not been verified.</span></span> <span data-ttu-id="92fb2-139">Ale Exchange také přidat  **SIP:abbie.spencer@fabrikamonline.com** . Společnost Fabrikam nepoužíval Lync nebo Skype na místě, ale Azure AD a Exchange Online připravte se na to.</span><span class="sxs-lookup"><span data-stu-id="92fb2-139">But Exchange also added **SIP:abbie.spencer@fabrikamonline.com**. Fabrikam has not used Lync/Skype on-premises, but Azure AD and Exchange Online prepare for it.</span></span>

<span data-ttu-id="92fb2-140">Tato logika pro proxyAddresses je odkazované tooas **ProxyCalc**.</span><span class="sxs-lookup"><span data-stu-id="92fb2-140">This logic for proxyAddresses is referred tooas **ProxyCalc**.</span></span> <span data-ttu-id="92fb2-141">Vyvolání ProxyCalc každé změně na uživatele při:</span><span class="sxs-lookup"><span data-stu-id="92fb2-141">ProxyCalc is invoked with every change on a user when:</span></span>

- <span data-ttu-id="92fb2-142">Hello uživatel má přiřazeno plánu služby, které zahrnuje Exchange Online i v případě, že uživatel hello nebyl licenci pro Exchange.</span><span class="sxs-lookup"><span data-stu-id="92fb2-142">hello user has been assigned a service plan that includes Exchange Online even if hello user was not licensed for Exchange.</span></span> <span data-ttu-id="92fb2-143">Například pokud uživatel hello je přiřazen hello Office E3 SKU, ale byl přiřazen pouze SharePoint Online.</span><span class="sxs-lookup"><span data-stu-id="92fb2-143">For example, if hello user is assigned hello Office E3 SKU, but only was assigned SharePoint Online.</span></span> <span data-ttu-id="92fb2-144">To platí i v případě, že poštovní schránky je stále místně.</span><span class="sxs-lookup"><span data-stu-id="92fb2-144">This is true even if your mailbox is still on-premises.</span></span>
- <span data-ttu-id="92fb2-145">msExchRecipientTypeDetails Hello atributu obsahuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="92fb2-145">hello attribute msExchRecipientTypeDetails has a value.</span></span>
- <span data-ttu-id="92fb2-146">Musíte provést změnu tooproxyAddresses nebo userPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="92fb2-146">You make a change tooproxyAddresses or userPrincipalName.</span></span>

<span data-ttu-id="92fb2-147">ProxyCalc může trvat některé čas tooprocess změnu na uživatele a není synchronní s procesem exportu hello Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="92fb2-147">ProxyCalc might take some time tooprocess a change on a user and is not synchronous with hello Azure AD Connect export process.</span></span>

> [!NOTE]
> <span data-ttu-id="92fb2-148">Hello ProxyCalc logic obsahuje některé další chování pro pokročilé scénáře, které nejsou popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="92fb2-148">hello ProxyCalc logic has some additional behaviors for advanced scenarios not documented in this topic.</span></span> <span data-ttu-id="92fb2-149">Toto téma je pro vás toounderstand hello chování a není dokumentu všechny interní logiku.</span><span class="sxs-lookup"><span data-stu-id="92fb2-149">This topic is provided for you toounderstand hello behavior and not document all internal logic.</span></span>

### <a name="quarantined-attribute-values"></a><span data-ttu-id="92fb2-150">Hodnoty atributů v karanténě</span><span class="sxs-lookup"><span data-stu-id="92fb2-150">Quarantined attribute values</span></span>
<span data-ttu-id="92fb2-151">Stínové atributy se také používají po duplicitními hodnotami atributů.</span><span class="sxs-lookup"><span data-stu-id="92fb2-151">Shadow attributes are also used when there are duplicate attribute values.</span></span> <span data-ttu-id="92fb2-152">Další informace najdete v tématu [odolnosti duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="92fb2-152">For more information, see [duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="92fb2-153">Viz také</span><span class="sxs-lookup"><span data-stu-id="92fb2-153">See also</span></span>
* [<span data-ttu-id="92fb2-154">Synchronizace služby Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="92fb2-154">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="92fb2-155">[Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="92fb2-155">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
