---
title: "Azure AD Connect: Výrazů deklarativního zřizování | Microsoft Docs"
description: "Vysvětluje výrazů deklarativního zřizování."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: e3a03a97b10e04fb85261620879b2102e1db8465
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a><span data-ttu-id="8f666-103">Synchronizace Azure AD Connect: Principy výrazů deklarativní zřizování</span><span class="sxs-lookup"><span data-stu-id="8f666-103">Azure AD Connect sync: Understanding Declarative Provisioning Expressions</span></span>
<span data-ttu-id="8f666-104">Synchronizace Azure AD Connect je založený na deklarativní zřizování poprvé dostupné ve verzi produktu Forefront Identity Manager 2010.</span><span class="sxs-lookup"><span data-stu-id="8f666-104">Azure AD Connect sync builds on declarative provisioning first introduced in Forefront Identity Manager 2010.</span></span> <span data-ttu-id="8f666-105">Umožňuje implementovat obchodní logiku integrace kompletní identity bez nutnosti napsat zkompilovaný kód.</span><span class="sxs-lookup"><span data-stu-id="8f666-105">It allows you to implement your complete identity integration business logic without the need to write compiled code.</span></span>

<span data-ttu-id="8f666-106">Nedílnou součást vámi vyžádaných deklarativní zřizování je výraz jazyk použitý v toky atributů.</span><span class="sxs-lookup"><span data-stu-id="8f666-106">An essential part of declarative provisioning is the expression language used in attribute flows.</span></span> <span data-ttu-id="8f666-107">Použitý jazyk je podmnožinou Microsoft® Visual Basic for Applications (VBA).</span><span class="sxs-lookup"><span data-stu-id="8f666-107">The language used is a subset of Microsoft® Visual Basic® for Applications (VBA).</span></span> <span data-ttu-id="8f666-108">Tento jazyk slouží v aplikaci Microsoft Office a uživatelé s prostředím jazyka VBScript také rozpozná ho.</span><span class="sxs-lookup"><span data-stu-id="8f666-108">This language is used in Microsoft Office and users with experience of VBScript will also recognize it.</span></span> <span data-ttu-id="8f666-109">Výraz jazyka deklarativní zřizování je jenom pomocí funkce a není strukturovaných jazyk.</span><span class="sxs-lookup"><span data-stu-id="8f666-109">The Declarative Provisioning Expression Language is only using functions and is not a structured language.</span></span> <span data-ttu-id="8f666-110">Neexistují žádné metody nebo příkazy.</span><span class="sxs-lookup"><span data-stu-id="8f666-110">There are no methods or statements.</span></span> <span data-ttu-id="8f666-111">Na toku express programu místo toho jsou vnořené funkce.</span><span class="sxs-lookup"><span data-stu-id="8f666-111">Functions are instead nested to express program flow.</span></span>

<span data-ttu-id="8f666-112">Další podrobnosti najdete v tématu [Vítá vás Visual Basic pro aplikace referenční příručka jazyka pro Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f666-112">For more details, see [Welcome to the Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span></span>

<span data-ttu-id="8f666-113">Atributy jsou silného typu.</span><span class="sxs-lookup"><span data-stu-id="8f666-113">The attributes are strongly typed.</span></span> <span data-ttu-id="8f666-114">Funkce přijímá pouze atributy správného typu.</span><span class="sxs-lookup"><span data-stu-id="8f666-114">A function only accepts attributes of the correct type.</span></span> <span data-ttu-id="8f666-115">Je také malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="8f666-115">It is also case-sensitive.</span></span> <span data-ttu-id="8f666-116">Názvy funkcí a názvy atributů musí mít správná velká a malá písmena nebo dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="8f666-116">Both function names and attribute names must have proper casing or an error is thrown.</span></span>

## <a name="language-definitions-and-identifiers"></a><span data-ttu-id="8f666-117">Jazyk definic a identifikátory</span><span class="sxs-lookup"><span data-stu-id="8f666-117">Language definitions and Identifiers</span></span>
* <span data-ttu-id="8f666-118">Funkce mít název, za nímž následují argumenty v závorce: %{FunctionName/ (argument 1, argument N).</span><span class="sxs-lookup"><span data-stu-id="8f666-118">Functions have a name followed by arguments in brackets: FunctionName(argument 1, argument N).</span></span>
* <span data-ttu-id="8f666-119">Atributy jsou určeny hranaté závorky: [attributeName]</span><span class="sxs-lookup"><span data-stu-id="8f666-119">Attributes are identified by square brackets: [attributeName]</span></span>
* <span data-ttu-id="8f666-120">Parametry jsou určeny znaky procenta: % ParameterName %</span><span class="sxs-lookup"><span data-stu-id="8f666-120">Parameters are identified by percent signs: %ParameterName%</span></span>
* <span data-ttu-id="8f666-121">Řetězcové konstanty jsou uzavřeny do uvozovek: například "Contoso" (Poznámka: musíte použít rovné uvozovky "" a není inteligentní uvozovky "")</span><span class="sxs-lookup"><span data-stu-id="8f666-121">String constants are surrounded by quotes: For example, "Contoso" (Note: must use straight quotes "" and not smart quotes “”)</span></span>
* <span data-ttu-id="8f666-122">Číselné hodnoty jsou vyjádřené bez uvozovek a očekává se decimal.</span><span class="sxs-lookup"><span data-stu-id="8f666-122">Numeric values are expressed without quotes and expected to be decimal.</span></span> <span data-ttu-id="8f666-123">Hexadecimální hodnoty mají předponu & H.</span><span class="sxs-lookup"><span data-stu-id="8f666-123">Hexadecimal values are prefixed with &H.</span></span> <span data-ttu-id="8f666-124">Například 98052 & HFF</span><span class="sxs-lookup"><span data-stu-id="8f666-124">For example, 98052, &HFF</span></span>
* <span data-ttu-id="8f666-125">Logické hodnoty jsou vyjádřeny pomocí konstant: True, False.</span><span class="sxs-lookup"><span data-stu-id="8f666-125">Boolean values are expressed with constants: True, False.</span></span>
* <span data-ttu-id="8f666-126">Předdefinované konstanty a literály jsou vyjádřeny se pouze jejich název: NULL, Line FEED, IgnoreThisFlow</span><span class="sxs-lookup"><span data-stu-id="8f666-126">Built-in constants and literals are expressed with only their name: NULL, CRLF, IgnoreThisFlow</span></span>

### <a name="functions"></a><span data-ttu-id="8f666-127">Funkce</span><span class="sxs-lookup"><span data-stu-id="8f666-127">Functions</span></span>
<span data-ttu-id="8f666-128">Chcete-li povolit možnost transformace hodnot atributů deklarativní zřizování používá mnoho funkcí.</span><span class="sxs-lookup"><span data-stu-id="8f666-128">Declarative provisioning uses many functions to enable the possibility to transform attribute values.</span></span> <span data-ttu-id="8f666-129">Tyto funkce mohou být vnořené, takže výsledek z jednoho funkce je předán v jiné funkci.</span><span class="sxs-lookup"><span data-stu-id="8f666-129">These functions can be nested so the result from one function is passed in to another function.</span></span>

`Function1(Function2(Function3()))`

<span data-ttu-id="8f666-130">Úplný seznam funkcí najdete v [funkce odkaz](active-directory-aadconnectsync-functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="8f666-130">The complete list of functions can be found in the [function reference](active-directory-aadconnectsync-functions-reference.md).</span></span>

### <a name="parameters"></a><span data-ttu-id="8f666-131">Parametry</span><span class="sxs-lookup"><span data-stu-id="8f666-131">Parameters</span></span>
<span data-ttu-id="8f666-132">Parametr je definována pomocí konektoru nebo pomocí správce pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8f666-132">A parameter is defined either by a Connector or by an administrator using PowerShell.</span></span> <span data-ttu-id="8f666-133">Parametry obvykle obsahují hodnoty, které se liší na systému, například název domény uživatele nachází v.</span><span class="sxs-lookup"><span data-stu-id="8f666-133">Parameters usually contain values that are different from system to system, for example the name of the domain the user is located in.</span></span> <span data-ttu-id="8f666-134">Tyto parametry můžete použít v toky atributů.</span><span class="sxs-lookup"><span data-stu-id="8f666-134">These parameters can be used in attribute flows.</span></span>

<span data-ttu-id="8f666-135">Konektor služby Active Directory k dispozici následující parametry pro příchozí pravidla synchronizace:</span><span class="sxs-lookup"><span data-stu-id="8f666-135">The Active Directory Connector provided the following parameters for inbound Synchronization Rules:</span></span>

| <span data-ttu-id="8f666-136">Název parametru</span><span class="sxs-lookup"><span data-stu-id="8f666-136">Parameter Name</span></span> | <span data-ttu-id="8f666-137">Komentář</span><span class="sxs-lookup"><span data-stu-id="8f666-137">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="8f666-138">Domain.Netbios</span><span class="sxs-lookup"><span data-stu-id="8f666-138">Domain.Netbios</span></span> |<span data-ttu-id="8f666-139">Formát pro rozhraní NetBIOS domény aktuálně importována, například FABRIKAMSALES</span><span class="sxs-lookup"><span data-stu-id="8f666-139">Netbios format of the domain currently being imported, for example FABRIKAMSALES</span></span> |
| <span data-ttu-id="8f666-140">Domain.FQDN</span><span class="sxs-lookup"><span data-stu-id="8f666-140">Domain.FQDN</span></span> |<span data-ttu-id="8f666-141">Plně kvalifikovaný název domény formát domény aktuálně importována, například sales.fabrikam.com</span><span class="sxs-lookup"><span data-stu-id="8f666-141">FQDN format of the domain currently being imported, for example sales.fabrikam.com</span></span> |
| <span data-ttu-id="8f666-142">Domain.LDAP</span><span class="sxs-lookup"><span data-stu-id="8f666-142">Domain.LDAP</span></span> |<span data-ttu-id="8f666-143">Formát LDAP domény aktuálně importována, například řadič domény = prodej, DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="8f666-143">LDAP format of the domain currently being imported, for example DC=sales,DC=fabrikam,DC=com</span></span> |
| <span data-ttu-id="8f666-144">Forest.Netbios</span><span class="sxs-lookup"><span data-stu-id="8f666-144">Forest.Netbios</span></span> |<span data-ttu-id="8f666-145">Formát názvu doménové struktury aktuálně importována, například FABRIKAMCORP pro rozhraní NetBIOS</span><span class="sxs-lookup"><span data-stu-id="8f666-145">Netbios format of the forest name currently being imported, for example FABRIKAMCORP</span></span> |
| <span data-ttu-id="8f666-146">Forest.FQDN</span><span class="sxs-lookup"><span data-stu-id="8f666-146">Forest.FQDN</span></span> |<span data-ttu-id="8f666-147">Formát názvu doménové struktury aktuálně importována, třeba fabrikam.com plně kvalifikovaný název domény</span><span class="sxs-lookup"><span data-stu-id="8f666-147">FQDN format of the forest name currently being imported, for example fabrikam.com</span></span> |
| <span data-ttu-id="8f666-148">Forest.LDAP</span><span class="sxs-lookup"><span data-stu-id="8f666-148">Forest.LDAP</span></span> |<span data-ttu-id="8f666-149">LDAP formát názvu doménové struktury aktuálně importována, například DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="8f666-149">LDAP format of the forest name currently being imported, for example DC=fabrikam,DC=com</span></span> |

<span data-ttu-id="8f666-150">Systém poskytuje následující parametr, který se použije k získání identifikátor konektoru aktuálně spuštěna:</span><span class="sxs-lookup"><span data-stu-id="8f666-150">The system provides the following parameter, which is used to get the identifier of the Connector currently running:</span></span>  
`Connector.ID`

<span data-ttu-id="8f666-151">Tady je příklad, který naplní atribut úložiště metaverse domény s názvem netbios domény, kde se uživatel nachází:</span><span class="sxs-lookup"><span data-stu-id="8f666-151">Here is an example that populates the metaverse attribute domain with the netbios name of the domain where the user is located:</span></span>  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a><span data-ttu-id="8f666-152">Operátory</span><span class="sxs-lookup"><span data-stu-id="8f666-152">Operators</span></span>
<span data-ttu-id="8f666-153">Můžete použít následující operátory:</span><span class="sxs-lookup"><span data-stu-id="8f666-153">The following operators can be used:</span></span>

* <span data-ttu-id="8f666-154">**Porovnání**: <, < =, <>, =, >, > =</span><span class="sxs-lookup"><span data-stu-id="8f666-154">**Comparison**: <, <=, <>, =, >, >=</span></span>
* <span data-ttu-id="8f666-155">**Matematika**: +, -, \*, -</span><span class="sxs-lookup"><span data-stu-id="8f666-155">**Mathematics**: +, -, \*, -</span></span>
* <span data-ttu-id="8f666-156">**Řetězec**: & (zřetězení)</span><span class="sxs-lookup"><span data-stu-id="8f666-156">**String**: & (concatenate)</span></span>
* <span data-ttu-id="8f666-157">**Logické**: & & (a), || (nebo)</span><span class="sxs-lookup"><span data-stu-id="8f666-157">**Logical**: && (and), || (or)</span></span>
* <span data-ttu-id="8f666-158">**Pořadí vyhodnocení**:)</span><span class="sxs-lookup"><span data-stu-id="8f666-158">**Evaluation order**: ( )</span></span>

<span data-ttu-id="8f666-159">Operátory se vyhodnocují zleva doprava a se stejnou prioritou vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="8f666-159">Operators are evaluated left to right and have the same evaluation priority.</span></span> <span data-ttu-id="8f666-160">To znamená \* (násobitel), nebude hodnocen před - (odčítání).</span><span class="sxs-lookup"><span data-stu-id="8f666-160">That is, the \* (multiplier) is not evaluated before - (subtraction).</span></span> <span data-ttu-id="8f666-161">2\*(5 + 3) není stejný jako 2\*5 + 3.</span><span class="sxs-lookup"><span data-stu-id="8f666-161">2\*(5+3) is not the same as 2\*5+3.</span></span> <span data-ttu-id="8f666-162">Chcete-li změnit pořadí vyhodnocení při zleva na pořadí správné vyhodnocení není vhodné se používají hranatých závorek ().</span><span class="sxs-lookup"><span data-stu-id="8f666-162">The brackets ( ) are used to change the evaluation order when left to right evaluation order isn't appropriate.</span></span>

## <a name="multi-valued-attributes"></a><span data-ttu-id="8f666-163">Více hodnot atributů</span><span class="sxs-lookup"><span data-stu-id="8f666-163">Multi-valued attributes</span></span>
<span data-ttu-id="8f666-164">Funkce mohou pracovat s jednou hodnotou i více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="8f666-164">The functions can operate on both single-valued and multi-valued attributes.</span></span> <span data-ttu-id="8f666-165">Pro více hodnot atributů funkce funguje v každé hodnotě a platí stejnou funkci ke každé hodnotě.</span><span class="sxs-lookup"><span data-stu-id="8f666-165">For multi-valued attributes, the function operates over every value and applies the same function to every value.</span></span>

<span data-ttu-id="8f666-166">Například:</span><span class="sxs-lookup"><span data-stu-id="8f666-166">For example:</span></span>  
<span data-ttu-id="8f666-167">`Trim([proxyAddresses])`Proveďte operace Trim každé hodnoty v atributu proxyAddress.</span><span class="sxs-lookup"><span data-stu-id="8f666-167">`Trim([proxyAddresses])` Do a Trim of every value in the proxyAddress attribute.</span></span>  
<span data-ttu-id="8f666-168">`Word([proxyAddresses],1,"@") & "@contoso.com"`Pro každou hodnotu s @-sign, nahraďte doméně s @contoso.com.</span><span class="sxs-lookup"><span data-stu-id="8f666-168">`Word([proxyAddresses],1,"@") & "@contoso.com"` For every value with an @-sign, replace the domain with @contoso.com.</span></span>  
<span data-ttu-id="8f666-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Vyhledejte adresu SIP a odebere ji z hodnot.</span><span class="sxs-lookup"><span data-stu-id="8f666-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Look for the SIP-address and remove it from the values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f666-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f666-170">Next steps</span></span>
* <span data-ttu-id="8f666-171">Další informace o konfiguraci modelu v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="8f666-171">Read more about the configuration model in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>
* <span data-ttu-id="8f666-172">Najdete v tématu Jak deklarativní zřizování je použité out-of-box v [Principy výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="8f666-172">See how declarative provisioning is used out-of-box in [Understanding the default configuration](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
* <span data-ttu-id="8f666-173">Informace o tom, praktické změnit pomocí deklarativní zřizování v [jak provést změnu výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="8f666-173">See how to make a practical change using declarative provisioning in [How to make a change to the default configuration](active-directory-aadconnectsync-change-the-configuration.md).</span></span>

<span data-ttu-id="8f666-174">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="8f666-174">**Overview topics**</span></span>

* [<span data-ttu-id="8f666-175">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="8f666-175">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="8f666-176">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f666-176">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<span data-ttu-id="8f666-177">**Témata odkazů**</span><span class="sxs-lookup"><span data-stu-id="8f666-177">**Reference topics**</span></span>

* [<span data-ttu-id="8f666-178">Synchronizace Azure AD Connect: odkaz na funkce</span><span class="sxs-lookup"><span data-stu-id="8f666-178">Azure AD Connect sync: Functions Reference</span></span>](active-directory-aadconnectsync-functions-reference.md)

