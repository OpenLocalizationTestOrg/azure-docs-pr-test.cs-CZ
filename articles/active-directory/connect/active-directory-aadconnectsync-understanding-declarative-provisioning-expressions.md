---
title: "Azure AD Connect: Výrazů deklarativního zřizování | Microsoft Docs"
description: "Vysvětluje výrazů deklarativního zřizování hello."
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
ms.openlocfilehash: 516bcf1991c608d33aefc19551254d8b2bfc024f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a><span data-ttu-id="1990c-103">Synchronizace Azure AD Connect: Principy výrazů deklarativní zřizování</span><span class="sxs-lookup"><span data-stu-id="1990c-103">Azure AD Connect sync: Understanding Declarative Provisioning Expressions</span></span>
<span data-ttu-id="1990c-104">Synchronizace Azure AD Connect je založený na deklarativní zřizování poprvé dostupné ve verzi produktu Forefront Identity Manager 2010.</span><span class="sxs-lookup"><span data-stu-id="1990c-104">Azure AD Connect sync builds on declarative provisioning first introduced in Forefront Identity Manager 2010.</span></span> <span data-ttu-id="1990c-105">Umožňuje tooimplement obchodní logiky integrace kompletní identity bez nutnosti toowrite hello zkompilovaný kód.</span><span class="sxs-lookup"><span data-stu-id="1990c-105">It allows you tooimplement your complete identity integration business logic without hello need toowrite compiled code.</span></span>

<span data-ttu-id="1990c-106">Nedílnou součást vámi vyžádaných deklarativní zřizování je hello výraz jazyk použitý v toky atributů.</span><span class="sxs-lookup"><span data-stu-id="1990c-106">An essential part of declarative provisioning is hello expression language used in attribute flows.</span></span> <span data-ttu-id="1990c-107">Hello použitý jazyk je podmnožinou Microsoft® Visual Basic for Applications (VBA).</span><span class="sxs-lookup"><span data-stu-id="1990c-107">hello language used is a subset of Microsoft® Visual Basic® for Applications (VBA).</span></span> <span data-ttu-id="1990c-108">Tento jazyk slouží v aplikaci Microsoft Office a uživatelé s prostředím jazyka VBScript také rozpozná ho.</span><span class="sxs-lookup"><span data-stu-id="1990c-108">This language is used in Microsoft Office and users with experience of VBScript will also recognize it.</span></span> <span data-ttu-id="1990c-109">Hello jazyk výrazů deklarativního zřizování je pouze pomocí funkcí a není strukturovaných jazyk.</span><span class="sxs-lookup"><span data-stu-id="1990c-109">hello Declarative Provisioning Expression Language is only using functions and is not a structured language.</span></span> <span data-ttu-id="1990c-110">Neexistují žádné metody nebo příkazy.</span><span class="sxs-lookup"><span data-stu-id="1990c-110">There are no methods or statements.</span></span> <span data-ttu-id="1990c-111">Místo toho jsou vnořené funkce tooexpress programu toku.</span><span class="sxs-lookup"><span data-stu-id="1990c-111">Functions are instead nested tooexpress program flow.</span></span>

<span data-ttu-id="1990c-112">Další podrobnosti najdete v tématu [Vítejte toohello jazyka Visual Basic pro aplikace referenční příručka jazyka pro Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span><span class="sxs-lookup"><span data-stu-id="1990c-112">For more details, see [Welcome toohello Visual Basic for Applications language reference for Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).</span></span>

<span data-ttu-id="1990c-113">Hello atributy jsou silného typu.</span><span class="sxs-lookup"><span data-stu-id="1990c-113">hello attributes are strongly typed.</span></span> <span data-ttu-id="1990c-114">Funkce přijímá pouze atributy hello správného typu.</span><span class="sxs-lookup"><span data-stu-id="1990c-114">A function only accepts attributes of hello correct type.</span></span> <span data-ttu-id="1990c-115">Je také malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="1990c-115">It is also case-sensitive.</span></span> <span data-ttu-id="1990c-116">Názvy funkcí a názvy atributů musí mít správná velká a malá písmena nebo dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="1990c-116">Both function names and attribute names must have proper casing or an error is thrown.</span></span>

## <a name="language-definitions-and-identifiers"></a><span data-ttu-id="1990c-117">Jazyk definic a identifikátory</span><span class="sxs-lookup"><span data-stu-id="1990c-117">Language definitions and Identifiers</span></span>
* <span data-ttu-id="1990c-118">Funkce mít název, za nímž následují argumenty v závorce: %{FunctionName/ (argument 1, argument N).</span><span class="sxs-lookup"><span data-stu-id="1990c-118">Functions have a name followed by arguments in brackets: FunctionName(argument 1, argument N).</span></span>
* <span data-ttu-id="1990c-119">Atributy jsou určeny hranaté závorky: [attributeName]</span><span class="sxs-lookup"><span data-stu-id="1990c-119">Attributes are identified by square brackets: [attributeName]</span></span>
* <span data-ttu-id="1990c-120">Parametry jsou určeny znaky procenta: % ParameterName %</span><span class="sxs-lookup"><span data-stu-id="1990c-120">Parameters are identified by percent signs: %ParameterName%</span></span>
* <span data-ttu-id="1990c-121">Řetězcové konstanty jsou uzavřeny do uvozovek: například "Contoso" (Poznámka: musíte použít rovné uvozovky "" a není inteligentní uvozovky "")</span><span class="sxs-lookup"><span data-stu-id="1990c-121">String constants are surrounded by quotes: For example, "Contoso" (Note: must use straight quotes "" and not smart quotes “”)</span></span>
* <span data-ttu-id="1990c-122">Číselné hodnoty jsou vyjádřeny bez uvozovek a očekávané toobe decimal.</span><span class="sxs-lookup"><span data-stu-id="1990c-122">Numeric values are expressed without quotes and expected toobe decimal.</span></span> <span data-ttu-id="1990c-123">Hexadecimální hodnoty mají předponu & H.</span><span class="sxs-lookup"><span data-stu-id="1990c-123">Hexadecimal values are prefixed with &H.</span></span> <span data-ttu-id="1990c-124">Například 98052 & HFF</span><span class="sxs-lookup"><span data-stu-id="1990c-124">For example, 98052, &HFF</span></span>
* <span data-ttu-id="1990c-125">Logické hodnoty jsou vyjádřeny pomocí konstant: True, False.</span><span class="sxs-lookup"><span data-stu-id="1990c-125">Boolean values are expressed with constants: True, False.</span></span>
* <span data-ttu-id="1990c-126">Předdefinované konstanty a literály jsou vyjádřeny se pouze jejich název: NULL, Line FEED, IgnoreThisFlow</span><span class="sxs-lookup"><span data-stu-id="1990c-126">Built-in constants and literals are expressed with only their name: NULL, CRLF, IgnoreThisFlow</span></span>

### <a name="functions"></a><span data-ttu-id="1990c-127">Funkce</span><span class="sxs-lookup"><span data-stu-id="1990c-127">Functions</span></span>
<span data-ttu-id="1990c-128">Deklarativní zřizování používá mnoho funkcí tooenable hello možnost tootransform hodnoty atributu.</span><span class="sxs-lookup"><span data-stu-id="1990c-128">Declarative provisioning uses many functions tooenable hello possibility tootransform attribute values.</span></span> <span data-ttu-id="1990c-129">Tyto funkce mohou být vnořené tak hello výsledek z jednoho funkce je předán v tooanother funkce.</span><span class="sxs-lookup"><span data-stu-id="1990c-129">These functions can be nested so hello result from one function is passed in tooanother function.</span></span>

`Function1(Function2(Function3()))`

<span data-ttu-id="1990c-130">Hello úplný seznam funkcí najdete v hello [funkce odkaz](active-directory-aadconnectsync-functions-reference.md).</span><span class="sxs-lookup"><span data-stu-id="1990c-130">hello complete list of functions can be found in hello [function reference](active-directory-aadconnectsync-functions-reference.md).</span></span>

### <a name="parameters"></a><span data-ttu-id="1990c-131">Parametry</span><span class="sxs-lookup"><span data-stu-id="1990c-131">Parameters</span></span>
<span data-ttu-id="1990c-132">Parametr je definována pomocí konektoru nebo pomocí správce pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1990c-132">A parameter is defined either by a Connector or by an administrator using PowerShell.</span></span> <span data-ttu-id="1990c-133">Parametry obvykle obsahují hodnoty, které se liší od toosystem systému, například název hello hello domény hello uživatele nachází v.</span><span class="sxs-lookup"><span data-stu-id="1990c-133">Parameters usually contain values that are different from system toosystem, for example hello name of hello domain hello user is located in.</span></span> <span data-ttu-id="1990c-134">Tyto parametry můžete použít v toky atributů.</span><span class="sxs-lookup"><span data-stu-id="1990c-134">These parameters can be used in attribute flows.</span></span>

<span data-ttu-id="1990c-135">Hello konektor služby Active Directory zadaný hello následující parametry pro příchozí pravidla synchronizace:</span><span class="sxs-lookup"><span data-stu-id="1990c-135">hello Active Directory Connector provided hello following parameters for inbound Synchronization Rules:</span></span>

| <span data-ttu-id="1990c-136">Název parametru</span><span class="sxs-lookup"><span data-stu-id="1990c-136">Parameter Name</span></span> | <span data-ttu-id="1990c-137">Komentář</span><span class="sxs-lookup"><span data-stu-id="1990c-137">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="1990c-138">Domain.Netbios</span><span class="sxs-lookup"><span data-stu-id="1990c-138">Domain.Netbios</span></span> |<span data-ttu-id="1990c-139">Formát pro rozhraní NetBIOS domény hello aktuálně importována, například FABRIKAMSALES</span><span class="sxs-lookup"><span data-stu-id="1990c-139">Netbios format of hello domain currently being imported, for example FABRIKAMSALES</span></span> |
| <span data-ttu-id="1990c-140">Domain.FQDN</span><span class="sxs-lookup"><span data-stu-id="1990c-140">Domain.FQDN</span></span> |<span data-ttu-id="1990c-141">Plně kvalifikovaný název domény formát domény hello aktuálně importována, například sales.fabrikam.com</span><span class="sxs-lookup"><span data-stu-id="1990c-141">FQDN format of hello domain currently being imported, for example sales.fabrikam.com</span></span> |
| <span data-ttu-id="1990c-142">Domain.LDAP</span><span class="sxs-lookup"><span data-stu-id="1990c-142">Domain.LDAP</span></span> |<span data-ttu-id="1990c-143">Formát LDAP hello domény aktuálně importována, například řadič domény = prodej, DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="1990c-143">LDAP format of hello domain currently being imported, for example DC=sales,DC=fabrikam,DC=com</span></span> |
| <span data-ttu-id="1990c-144">Forest.Netbios</span><span class="sxs-lookup"><span data-stu-id="1990c-144">Forest.Netbios</span></span> |<span data-ttu-id="1990c-145">Formát pro rozhraní NetBIOS hello název doménové struktury aktuálně importována, například FABRIKAMCORP</span><span class="sxs-lookup"><span data-stu-id="1990c-145">Netbios format of hello forest name currently being imported, for example FABRIKAMCORP</span></span> |
| <span data-ttu-id="1990c-146">Forest.FQDN</span><span class="sxs-lookup"><span data-stu-id="1990c-146">Forest.FQDN</span></span> |<span data-ttu-id="1990c-147">Plně kvalifikovaný název domény formát hello název doménové struktury aktuálně importována, třeba fabrikam.com</span><span class="sxs-lookup"><span data-stu-id="1990c-147">FQDN format of hello forest name currently being imported, for example fabrikam.com</span></span> |
| <span data-ttu-id="1990c-148">Forest.LDAP</span><span class="sxs-lookup"><span data-stu-id="1990c-148">Forest.LDAP</span></span> |<span data-ttu-id="1990c-149">LDAP formát hello název doménové struktury aktuálně importována, například DC = fabrikam, DC = com</span><span class="sxs-lookup"><span data-stu-id="1990c-149">LDAP format of hello forest name currently being imported, for example DC=fabrikam,DC=com</span></span> |

<span data-ttu-id="1990c-150">Hello systém poskytuje hello následující parametr, který použité tooget hello identifikátor hello konektor běží v současné době:</span><span class="sxs-lookup"><span data-stu-id="1990c-150">hello system provides hello following parameter, which is used tooget hello identifier of hello Connector currently running:</span></span>  
`Connector.ID`

<span data-ttu-id="1990c-151">Tady je příklad, který naplní domény atribut úložiště metaverse hello s názvem netbios hello hello domény, kde se nachází hello uživatele:</span><span class="sxs-lookup"><span data-stu-id="1990c-151">Here is an example that populates hello metaverse attribute domain with hello netbios name of hello domain where hello user is located:</span></span>  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a><span data-ttu-id="1990c-152">Operátory</span><span class="sxs-lookup"><span data-stu-id="1990c-152">Operators</span></span>
<span data-ttu-id="1990c-153">dá se Hello následující operátory:</span><span class="sxs-lookup"><span data-stu-id="1990c-153">hello following operators can be used:</span></span>

* <span data-ttu-id="1990c-154">**Porovnání**: <, < =, <>, =, >, > =</span><span class="sxs-lookup"><span data-stu-id="1990c-154">**Comparison**: <, <=, <>, =, >, >=</span></span>
* <span data-ttu-id="1990c-155">**Matematika**: +, -, \*, -</span><span class="sxs-lookup"><span data-stu-id="1990c-155">**Mathematics**: +, -, \*, -</span></span>
* <span data-ttu-id="1990c-156">**Řetězec**: & (zřetězení)</span><span class="sxs-lookup"><span data-stu-id="1990c-156">**String**: & (concatenate)</span></span>
* <span data-ttu-id="1990c-157">**Logické**: & & (a), || (nebo)</span><span class="sxs-lookup"><span data-stu-id="1990c-157">**Logical**: && (and), || (or)</span></span>
* <span data-ttu-id="1990c-158">**Pořadí vyhodnocení**:)</span><span class="sxs-lookup"><span data-stu-id="1990c-158">**Evaluation order**: ( )</span></span>

<span data-ttu-id="1990c-159">Operátory jsou vyhodnotí levém tooright a mají hello stejnou prioritou vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="1990c-159">Operators are evaluated left tooright and have hello same evaluation priority.</span></span> <span data-ttu-id="1990c-160">To znamená, hello \* (násobitel), nebude hodnocen před - (odčítání).</span><span class="sxs-lookup"><span data-stu-id="1990c-160">That is, hello \* (multiplier) is not evaluated before - (subtraction).</span></span> <span data-ttu-id="1990c-161">2\*(5 + 3) není hello stejné jako 2\*5 + 3.</span><span class="sxs-lookup"><span data-stu-id="1990c-161">2\*(5+3) is not hello same as 2\*5+3.</span></span> <span data-ttu-id="1990c-162">Hello hranatých závorek () se používají toochange hello vyhodnocení pořadí při zbývajících pořadí vyhodnocení tooright není vhodné.</span><span class="sxs-lookup"><span data-stu-id="1990c-162">hello brackets ( ) are used toochange hello evaluation order when left tooright evaluation order isn't appropriate.</span></span>

## <a name="multi-valued-attributes"></a><span data-ttu-id="1990c-163">Více hodnot atributů</span><span class="sxs-lookup"><span data-stu-id="1990c-163">Multi-valued attributes</span></span>
<span data-ttu-id="1990c-164">Funkce Hello mohou pracovat s jednou hodnotou i více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="1990c-164">hello functions can operate on both single-valued and multi-valued attributes.</span></span> <span data-ttu-id="1990c-165">Pro více hodnot atributů hello funkce funguje v každé hodnotě a použije hello stejné funkce tooevery hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1990c-165">For multi-valued attributes, hello function operates over every value and applies hello same function tooevery value.</span></span>

<span data-ttu-id="1990c-166">Například:</span><span class="sxs-lookup"><span data-stu-id="1990c-166">For example:</span></span>  
<span data-ttu-id="1990c-167">`Trim([proxyAddresses])`Proveďte operace Trim každé hodnoty v atributu proxyAddress hello.</span><span class="sxs-lookup"><span data-stu-id="1990c-167">`Trim([proxyAddresses])` Do a Trim of every value in hello proxyAddress attribute.</span></span>  
<span data-ttu-id="1990c-168">`Word([proxyAddresses],1,"@") & "@contoso.com"`Pro každou hodnotu s @-sign, nahraďte hello domény s @contoso.com.</span><span class="sxs-lookup"><span data-stu-id="1990c-168">`Word([proxyAddresses],1,"@") & "@contoso.com"` For every value with an @-sign, replace hello domain with @contoso.com.</span></span>  
<span data-ttu-id="1990c-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Vyhledejte hello adresy SIP a odebere ji z hodnot hello.</span><span class="sxs-lookup"><span data-stu-id="1990c-169">`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])` Look for hello SIP-address and remove it from hello values.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1990c-170">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1990c-170">Next steps</span></span>
* <span data-ttu-id="1990c-171">Další informace o hello konfigurační model v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="1990c-171">Read more about hello configuration model in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>
* <span data-ttu-id="1990c-172">Najdete v tématu Jak deklarativní zřizování je použité out-of-box v [Principy hello výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="1990c-172">See how declarative provisioning is used out-of-box in [Understanding hello default configuration](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
* <span data-ttu-id="1990c-173">V tématu Jak toomake praktická změnit pomocí deklarativní zřizování v [jak toomake toohello změnu výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="1990c-173">See how toomake a practical change using declarative provisioning in [How toomake a change toohello default configuration](active-directory-aadconnectsync-change-the-configuration.md).</span></span>

<span data-ttu-id="1990c-174">**Témata s přehledem**</span><span class="sxs-lookup"><span data-stu-id="1990c-174">**Overview topics**</span></span>

* [<span data-ttu-id="1990c-175">Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="1990c-175">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="1990c-176">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1990c-176">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)

<span data-ttu-id="1990c-177">**Témata odkazů**</span><span class="sxs-lookup"><span data-stu-id="1990c-177">**Reference topics**</span></span>

* [<span data-ttu-id="1990c-178">Synchronizace Azure AD Connect: odkaz na funkce</span><span class="sxs-lookup"><span data-stu-id="1990c-178">Azure AD Connect sync: Functions Reference</span></span>](active-directory-aadconnectsync-functions-reference.md)

