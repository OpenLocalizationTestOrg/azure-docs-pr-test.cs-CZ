---
title: "Azure AD Connect sync služby stínové atributy | Microsoft Docs"
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
ms.openlocfilehash: 0b6a7f22d744480a40a878c979986cdd7667109c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a><span data-ttu-id="e0d17-103">Azure AD Connect sync služby stínové atributy</span><span class="sxs-lookup"><span data-stu-id="e0d17-103">Azure AD Connect sync service shadow attributes</span></span>
<span data-ttu-id="e0d17-104">Většina atributy jsou reprezentována stejným způsobem jako ve službě Azure AD, jako jsou ve vaší místní službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e0d17-104">Most attributes are represented the same way in Azure AD as they are in your on-premises Active Directory.</span></span> <span data-ttu-id="e0d17-105">Ale některé atributy mají některé zvláštní zpracování a hodnota atributu ve službě Azure AD může být jiná než co Azure AD Connect synchronizuje.</span><span class="sxs-lookup"><span data-stu-id="e0d17-105">But some attributes have some special handling and the attribute value in Azure AD might be different than what Azure AD Connect synchronizes.</span></span>

## <a name="introducing-shadow-attributes"></a><span data-ttu-id="e0d17-106">Představení stínové atributy</span><span class="sxs-lookup"><span data-stu-id="e0d17-106">Introducing shadow attributes</span></span>
<span data-ttu-id="e0d17-107">Některé atributy mají dva reprezentace ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0d17-107">Some attributes have two representations in Azure AD.</span></span> <span data-ttu-id="e0d17-108">Místní hodnota a vypočítaná hodnota, která jsou uloženy.</span><span class="sxs-lookup"><span data-stu-id="e0d17-108">Both the on-premises value and a calculated value are stored.</span></span> <span data-ttu-id="e0d17-109">Tyto další atributy, se nazývají stínové atributy.</span><span class="sxs-lookup"><span data-stu-id="e0d17-109">These extra attributes are called shadow attributes.</span></span> <span data-ttu-id="e0d17-110">Jsou dva nejběžnější atributy, kde uvidíte toto chování **userPrincipalName** a **proxyAddress**.</span><span class="sxs-lookup"><span data-stu-id="e0d17-110">The two most common attributes where you see this behavior are **userPrincipalName** and **proxyAddress**.</span></span> <span data-ttu-id="e0d17-111">Změna hodnoty atributu se stane, když existují hodnoty v těchto atributech představující-ověření domény.</span><span class="sxs-lookup"><span data-stu-id="e0d17-111">The change in attribute values happens when there are values in these attributes representing non-verified domains.</span></span> <span data-ttu-id="e0d17-112">Ale synchronizační modul v připojení přečte hodnotu v atributu stínové tak z jeho perspektivy, atribut bylo potvrzeno službou Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0d17-112">But the sync engine in Connect reads the value in the shadow attribute so from its perspective, the attribute has been confirmed by Azure AD.</span></span>

<span data-ttu-id="e0d17-113">Nejde zobrazit atributů stínové pomocí Azure portálu nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e0d17-113">You cannot see the shadow attributes using the Azure portal or with PowerShell.</span></span> <span data-ttu-id="e0d17-114">Ale pochopení koncept pomáhá při řešení některých scénářích, kde atribut má jiné hodnoty na místě a v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e0d17-114">But understanding the concept helps you to troubleshoot certain scenarios where the attribute has different values on-premises and in the cloud.</span></span>

<span data-ttu-id="e0d17-115">Abyste lépe pochopili chování, podívejte se na tomto příkladu ze společnosti Fabrikam:</span><span class="sxs-lookup"><span data-stu-id="e0d17-115">To better understand the behavior, look at this example from Fabrikam:</span></span>  
![Domény](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
<span data-ttu-id="e0d17-117">Mají více přípon UPN ve svojí místní službě Active Directory, ale jeden pouze ověřit.</span><span class="sxs-lookup"><span data-stu-id="e0d17-117">They have multiple UPN suffixes in their on-premises Active Directory, but they have only verified one.</span></span>

### <a name="userprincipalname"></a><span data-ttu-id="e0d17-118">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="e0d17-118">userPrincipalName</span></span>
<span data-ttu-id="e0d17-119">Uživatel má následující hodnoty atributu v doméně ověřit:</span><span class="sxs-lookup"><span data-stu-id="e0d17-119">A user has the following attribute values in a non-verified domain:</span></span>

| <span data-ttu-id="e0d17-120">Atribut</span><span class="sxs-lookup"><span data-stu-id="e0d17-120">Attribute</span></span> | <span data-ttu-id="e0d17-121">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e0d17-121">Value</span></span> |
| --- | --- |
| <span data-ttu-id="e0d17-122">userPrincipalName na místě</span><span class="sxs-lookup"><span data-stu-id="e0d17-122">on-premises userPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="e0d17-123">Azure AD shadowUserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="e0d17-123">Azure AD shadowUserPrincipalName</span></span> | lee.sperry@fabrikam.com |
| <span data-ttu-id="e0d17-124">Azure AD userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="e0d17-124">Azure AD userPrincipalName</span></span> | lee.sperry@fabrikam.onmicrosoft.com |

<span data-ttu-id="e0d17-125">Atribut userPrincipalName je hodnota, která se zobrazí po pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e0d17-125">The userPrincipalName attribute is the value you see when using PowerShell.</span></span>

<span data-ttu-id="e0d17-126">Vzhledem k tomu, že hodnota atributu skutečné místní je uložená ve službě Azure AD, když ověření domény fabrikam.com, aktualizuje Azure AD s hodnotou z shadowUserPrincipalName atribut userPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="e0d17-126">Since the real on-premises attribute value is stored in Azure AD, when you verify the fabrikam.com domain, Azure AD updates the userPrincipalName attribute with the value from the shadowUserPrincipalName.</span></span> <span data-ttu-id="e0d17-127">Nemáte synchronizujte jakékoli změny z Azure AD Connect pro tyto hodnoty aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e0d17-127">You do not have to synchronize any changes from Azure AD Connect for these values to be updated.</span></span>

### <a name="proxyaddresses"></a><span data-ttu-id="e0d17-128">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="e0d17-128">proxyAddresses</span></span>
<span data-ttu-id="e0d17-129">Stejný postup pro pouze včetně ověřených domén dochází k také pro proxyAddresses, ale s některé další logiku.</span><span class="sxs-lookup"><span data-stu-id="e0d17-129">The same process for only including verified domains also occurs for proxyAddresses, but with some additional logic.</span></span> <span data-ttu-id="e0d17-130">Kontrolu ověřených domén se stane pouze pro poštovní schránky uživatele.</span><span class="sxs-lookup"><span data-stu-id="e0d17-130">The check for verified domains only happens for mailbox users.</span></span> <span data-ttu-id="e0d17-131">Poštovní uživatele nebo kontaktujte představují uživateli v jiné organizaci Exchange a všechny hodnoty v poli proxyAddresses. můžete přidat do těchto objektů.</span><span class="sxs-lookup"><span data-stu-id="e0d17-131">A mail-enabled user or contact represent a user in another Exchange organization and you can add any values in proxyAddresses to these objects.</span></span>

<span data-ttu-id="e0d17-132">Uživatel poštovní schránky, buď místní nebo v systému Exchange Online se zobrazí pouze hodnoty ověřených domén.</span><span class="sxs-lookup"><span data-stu-id="e0d17-132">For a mailbox user, either on-premises or in Exchange Online, only values for verified domains appear.</span></span> <span data-ttu-id="e0d17-133">Ho může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="e0d17-133">It could look like this:</span></span>

| <span data-ttu-id="e0d17-134">Atribut</span><span class="sxs-lookup"><span data-stu-id="e0d17-134">Attribute</span></span> | <span data-ttu-id="e0d17-135">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e0d17-135">Value</span></span> |
| --- | --- |
| <span data-ttu-id="e0d17-136">místní proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="e0d17-136">on-premises proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| <span data-ttu-id="e0d17-137">Exchange Online proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="e0d17-137">Exchange Online proxyAddresses</span></span> | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

<span data-ttu-id="e0d17-138">V takovém případě  **smtp:abbie.spencer@fabrikam.com**  byla odebrána, vzhledem k tomu, že doména nebyla ověřena.</span><span class="sxs-lookup"><span data-stu-id="e0d17-138">In this case **smtp:abbie.spencer@fabrikam.com** was removed since that domain has not been verified.</span></span> <span data-ttu-id="e0d17-139">Ale Exchange také přidat  **SIP:abbie.spencer@fabrikamonline.com** .</span><span class="sxs-lookup"><span data-stu-id="e0d17-139">But Exchange also added **SIP:abbie.spencer@fabrikamonline.com**.</span></span> <span data-ttu-id="e0d17-140">Společnost Fabrikam nepoužíval Lync nebo Skype na místě, ale Azure AD a Exchange Online připravte se na to.</span><span class="sxs-lookup"><span data-stu-id="e0d17-140">Fabrikam has not used Lync/Skype on-premises, but Azure AD and Exchange Online prepare for it.</span></span>

<span data-ttu-id="e0d17-141">Tato logika pro proxyAddresses se označuje jako **ProxyCalc**.</span><span class="sxs-lookup"><span data-stu-id="e0d17-141">This logic for proxyAddresses is referred to as **ProxyCalc**.</span></span> <span data-ttu-id="e0d17-142">Vyvolání ProxyCalc každé změně na uživatele při:</span><span class="sxs-lookup"><span data-stu-id="e0d17-142">ProxyCalc is invoked with every change on a user when:</span></span>

- <span data-ttu-id="e0d17-143">Uživatel byl přiřazen plánu služby, které zahrnuje Exchange Online i v případě, že uživatel nebyl licenci na Exchange.</span><span class="sxs-lookup"><span data-stu-id="e0d17-143">The user has been assigned a service plan that includes Exchange Online even if the user was not licensed for Exchange.</span></span> <span data-ttu-id="e0d17-144">Například pokud uživatel je přiřazen Office E3 SKU, ale pouze byl přiřazen SharePoint Online.</span><span class="sxs-lookup"><span data-stu-id="e0d17-144">For example, if the user is assigned the Office E3 SKU, but only was assigned SharePoint Online.</span></span> <span data-ttu-id="e0d17-145">To platí i v případě, že poštovní schránky je stále místně.</span><span class="sxs-lookup"><span data-stu-id="e0d17-145">This is true even if your mailbox is still on-premises.</span></span>
- <span data-ttu-id="e0d17-146">MsExchRecipientTypeDetails atributu obsahuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e0d17-146">The attribute msExchRecipientTypeDetails has a value.</span></span>
- <span data-ttu-id="e0d17-147">Můžete provést změnu proxyAddresses nebo userPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="e0d17-147">You make a change to proxyAddresses or userPrincipalName.</span></span>

<span data-ttu-id="e0d17-148">ProxyCalc může trvat nějakou dobu zpracování změn na uživatele a není synchronní s procesem exportu Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="e0d17-148">ProxyCalc might take some time to process a change on a user and is not synchronous with the Azure AD Connect export process.</span></span>

> [!NOTE]
> <span data-ttu-id="e0d17-149">ProxyCalc logic obsahuje některé další chování pro pokročilé scénáře, které nejsou popsané v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="e0d17-149">The ProxyCalc logic has some additional behaviors for advanced scenarios not documented in this topic.</span></span> <span data-ttu-id="e0d17-150">Toto téma poskytuje pro pochopit chování a není dokumentu všechny interní logiku.</span><span class="sxs-lookup"><span data-stu-id="e0d17-150">This topic is provided for you to understand the behavior and not document all internal logic.</span></span>

### <a name="quarantined-attribute-values"></a><span data-ttu-id="e0d17-151">Hodnoty atributů v karanténě</span><span class="sxs-lookup"><span data-stu-id="e0d17-151">Quarantined attribute values</span></span>
<span data-ttu-id="e0d17-152">Stínové atributy se také používají po duplicitními hodnotami atributů.</span><span class="sxs-lookup"><span data-stu-id="e0d17-152">Shadow attributes are also used when there are duplicate attribute values.</span></span> <span data-ttu-id="e0d17-153">Další informace najdete v tématu [odolnosti duplicitní atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span><span class="sxs-lookup"><span data-stu-id="e0d17-153">For more information, see [duplicate attribute resiliency](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="e0d17-154">Viz také</span><span class="sxs-lookup"><span data-stu-id="e0d17-154">See also</span></span>
* [<span data-ttu-id="e0d17-155">Synchronizace služby Azure AD Connect</span><span class="sxs-lookup"><span data-stu-id="e0d17-155">Azure AD Connect sync</span></span>](active-directory-aadconnectsync-whatis.md)
* <span data-ttu-id="e0d17-156">[Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="e0d17-156">[Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
