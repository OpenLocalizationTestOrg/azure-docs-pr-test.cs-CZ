---
title: "Synchronizace Azure AD Connect: referenční dokumentace funkcí | Microsoft Docs"
description: "Odkaz výrazů deklarativního zřizování v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: fbe0df856ca2efda965650fb85c7e831a0be32c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a><span data-ttu-id="46ba9-103">Synchronizace Azure AD Connect: odkaz na funkce</span><span class="sxs-lookup"><span data-stu-id="46ba9-103">Azure AD Connect sync: Functions Reference</span></span>
<span data-ttu-id="46ba9-104">Ve službě Azure AD Connect jsou funkce použité toomanipulate hodnota atributu během synchronizace.</span><span class="sxs-lookup"><span data-stu-id="46ba9-104">In Azure AD Connect, functions are used toomanipulate an attribute value during synchronization.</span></span>  
<span data-ttu-id="46ba9-105">Hello syntaxe funkce hello je vyjádřit pomocí hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="46ba9-105">hello Syntax of hello functions is expressed using hello following format:</span></span>  
`<output type> FunctionName(<input type> <position name>, ..)`

<span data-ttu-id="46ba9-106">Pokud funkci je přetížena a přijímá více syntaxe, jsou uvedeny všechny platné syntaxe.</span><span class="sxs-lookup"><span data-stu-id="46ba9-106">If a function is overloaded and accepts multiple syntaxes, all valid syntaxes are listed.</span></span>  
<span data-ttu-id="46ba9-107">Hello funkce jsou silného typu a jejich ověřte, že hello typ předaný typ odpovídá hello popsané.</span><span class="sxs-lookup"><span data-stu-id="46ba9-107">hello functions are strongly typed and they verify that hello type passed in matches hello documented type.</span></span>  
<span data-ttu-id="46ba9-108">Pokud hello typ neodpovídá, je vyvolána k chybě.</span><span class="sxs-lookup"><span data-stu-id="46ba9-108">If hello type does not match, an error is thrown.</span></span>

<span data-ttu-id="46ba9-109">Hello typy jsou vyjádřeny s hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="46ba9-109">hello types are expressed with hello following syntax:</span></span>

* <span data-ttu-id="46ba9-110">**Koš** – binární</span><span class="sxs-lookup"><span data-stu-id="46ba9-110">**bin** – Binary</span></span>
* <span data-ttu-id="46ba9-111">**BOOL** – Boolean</span><span class="sxs-lookup"><span data-stu-id="46ba9-111">**bool** – Boolean</span></span>
* <span data-ttu-id="46ba9-112">**DT** – datum a čas UTC</span><span class="sxs-lookup"><span data-stu-id="46ba9-112">**dt** – UTC Date/Time</span></span>
* <span data-ttu-id="46ba9-113">**výčet** – výčet známé konstanty</span><span class="sxs-lookup"><span data-stu-id="46ba9-113">**enum** – Enumeration of known constants</span></span>
* <span data-ttu-id="46ba9-114">**Exp** – výraz, který je očekáván tooevaluate tooa logická hodnota</span><span class="sxs-lookup"><span data-stu-id="46ba9-114">**exp** – Expression, which is expected tooevaluate tooa Boolean</span></span>
* <span data-ttu-id="46ba9-115">**mvbin** – vícehodnotových binární</span><span class="sxs-lookup"><span data-stu-id="46ba9-115">**mvbin** – Multi-Valued Binary</span></span>
* <span data-ttu-id="46ba9-116">**mvstr** – vícehodnotových řetězců</span><span class="sxs-lookup"><span data-stu-id="46ba9-116">**mvstr** – Multi-Valued String</span></span>
* <span data-ttu-id="46ba9-117">**mvref** – vícehodnotových odkaz</span><span class="sxs-lookup"><span data-stu-id="46ba9-117">**mvref** – Multi-Valued Reference</span></span>
* <span data-ttu-id="46ba9-118">**Poče** – číselné</span><span class="sxs-lookup"><span data-stu-id="46ba9-118">**num** – Numeric</span></span>
* <span data-ttu-id="46ba9-119">**REF** – odkaz</span><span class="sxs-lookup"><span data-stu-id="46ba9-119">**ref** – Reference</span></span>
* <span data-ttu-id="46ba9-120">**Str –** – řetězec</span><span class="sxs-lookup"><span data-stu-id="46ba9-120">**str** – String</span></span>
* <span data-ttu-id="46ba9-121">**var** – hodnotu typu variant (téměř) žádného jiného typu</span><span class="sxs-lookup"><span data-stu-id="46ba9-121">**var** – A variant of (almost) any other type</span></span>
* <span data-ttu-id="46ba9-122">**void** – nevrací hodnotu</span><span class="sxs-lookup"><span data-stu-id="46ba9-122">**void** – doesn’t return a value</span></span>

<span data-ttu-id="46ba9-123">Hello funkce s typy hello **mvbin**, **mvstr**, a **mvref** můžete používat jenom u více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="46ba9-123">hello functions with hello types **mvbin**, **mvstr**, and **mvref** can only work on multi-valued attributes.</span></span> <span data-ttu-id="46ba9-124">Funkce s **bin**, **str**, a **ref** pracovní jednohodnotové i více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="46ba9-124">Functions with **bin**, **str**, and **ref** work on both single-valued and multi-valued attributes.</span></span>

## <a name="functions-reference"></a><span data-ttu-id="46ba9-125">Reference k funkcím</span><span class="sxs-lookup"><span data-stu-id="46ba9-125">Functions Reference</span></span>
| <span data-ttu-id="46ba9-126">Seznam funkcí</span><span class="sxs-lookup"><span data-stu-id="46ba9-126">List of functions</span></span> |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="46ba9-127">**Certifikát**</span><span class="sxs-lookup"><span data-stu-id="46ba9-127">**Certificate**</span></span> | | | | |
| [<span data-ttu-id="46ba9-128">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="46ba9-128">CertExtensionOids</span></span>](#certextensionoids) |[<span data-ttu-id="46ba9-129">CertFormat</span><span class="sxs-lookup"><span data-stu-id="46ba9-129">CertFormat</span></span>](#certformat) |[<span data-ttu-id="46ba9-130">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="46ba9-130">CertFriendlyName</span></span>](#certfriendlyname) |[<span data-ttu-id="46ba9-131">CertHashString</span><span class="sxs-lookup"><span data-stu-id="46ba9-131">CertHashString</span></span>](#certhashstring) | |
| [<span data-ttu-id="46ba9-132">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="46ba9-132">CertIssuer</span></span>](#certissuer) |[<span data-ttu-id="46ba9-133">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="46ba9-133">CertIssuerDN</span></span>](#certissuerdn) |[<span data-ttu-id="46ba9-134">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-134">CertIssuerOid</span></span>](#certissueroid) |[<span data-ttu-id="46ba9-135">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="46ba9-135">CertKeyAlgorithm</span></span>](#certkeyalgorithm) | |
| [<span data-ttu-id="46ba9-136">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="46ba9-136">CertKeyAlgorithmParams</span></span>](#certkeyalgorithmparams) |[<span data-ttu-id="46ba9-137">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="46ba9-137">CertNameInfo</span></span>](#certnameinfo) |[<span data-ttu-id="46ba9-138">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="46ba9-138">CertNotAfter</span></span>](#certnotafter) |[<span data-ttu-id="46ba9-139">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="46ba9-139">CertNotBefore</span></span>](#certnotbefore) | |
| [<span data-ttu-id="46ba9-140">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-140">CertPublicKeyOid</span></span>](#certpublickeyoid) |[<span data-ttu-id="46ba9-141">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-141">CertPublicKeyParametersOid</span></span>](#certpublickeyparametersoid) |[<span data-ttu-id="46ba9-142">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="46ba9-142">CertSerialNumber</span></span>](#certserialnumber) |[<span data-ttu-id="46ba9-143">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-143">CertSignatureAlgorithmOid</span></span>](#certsignaturealgorithmoid) | |
| [<span data-ttu-id="46ba9-144">CertSubject</span><span class="sxs-lookup"><span data-stu-id="46ba9-144">CertSubject</span></span>](#certsubject) |[<span data-ttu-id="46ba9-145">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="46ba9-145">CertSubjectNameDN</span></span>](#certsubjectnamedn) |[<span data-ttu-id="46ba9-146">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-146">CertSubjectNameOid</span></span>](#certsubjectnameoid) |[<span data-ttu-id="46ba9-147">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="46ba9-147">CertThumbprint</span></span>](#certthumbprint) | |
[<span data-ttu-id="46ba9-148">CertVersion</span><span class="sxs-lookup"><span data-stu-id="46ba9-148"> CertVersion</span></span>](#certversion) |[<span data-ttu-id="46ba9-149">IsCert</span><span class="sxs-lookup"><span data-stu-id="46ba9-149">IsCert</span></span>](#iscert) | | | |
| <span data-ttu-id="46ba9-150">**Převod**</span><span class="sxs-lookup"><span data-stu-id="46ba9-150">**Conversion**</span></span> | | | | |
| [<span data-ttu-id="46ba9-151">CBool –</span><span class="sxs-lookup"><span data-stu-id="46ba9-151">CBool</span></span>](#cbool) |[<span data-ttu-id="46ba9-152">CDate –</span><span class="sxs-lookup"><span data-stu-id="46ba9-152">CDate</span></span>](#cdate) |[<span data-ttu-id="46ba9-153">CGuid</span><span class="sxs-lookup"><span data-stu-id="46ba9-153">CGuid</span></span>](#cguid) |[<span data-ttu-id="46ba9-154">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="46ba9-154">ConvertFromBase64</span></span>](#convertfrombase64) | |
| [<span data-ttu-id="46ba9-155">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="46ba9-155">ConvertToBase64</span></span>](#converttobase64) |[<span data-ttu-id="46ba9-156">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="46ba9-156">ConvertFromUTF8Hex</span></span>](#convertfromutf8hex) |[<span data-ttu-id="46ba9-157">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="46ba9-157">ConvertToUTF8Hex</span></span>](#converttoutf8hex) |[<span data-ttu-id="46ba9-158">CNum</span><span class="sxs-lookup"><span data-stu-id="46ba9-158">CNum</span></span>](#cnum) | |
| [<span data-ttu-id="46ba9-159">CRef –</span><span class="sxs-lookup"><span data-stu-id="46ba9-159">CRef</span></span>](#cref) |[<span data-ttu-id="46ba9-160">CStr</span><span class="sxs-lookup"><span data-stu-id="46ba9-160">CStr</span></span>](#cstr) |[<span data-ttu-id="46ba9-161">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="46ba9-161">StringFromGuid</span></span>](#StringFromGuid) |[<span data-ttu-id="46ba9-162">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="46ba9-162">StringFromSid</span></span>](#stringfromsid) | |
| <span data-ttu-id="46ba9-163">**Datum a čas**</span><span class="sxs-lookup"><span data-stu-id="46ba9-163">**Date / Time**</span></span> | | | | |
| [<span data-ttu-id="46ba9-164">Funkce DateAdd</span><span class="sxs-lookup"><span data-stu-id="46ba9-164">DateAdd</span></span>](#dateadd) |[<span data-ttu-id="46ba9-165">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="46ba9-165">DateFromNum</span></span>](#datefromnum) |[<span data-ttu-id="46ba9-166">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="46ba9-166">FormatDateTime</span></span>](#formatdatetime) |[<span data-ttu-id="46ba9-167">Nyní</span><span class="sxs-lookup"><span data-stu-id="46ba9-167">Now</span></span>](#now) | |
| [<span data-ttu-id="46ba9-168">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="46ba9-168">NumFromDate</span></span>](#numfromdate) | | | | |
| <span data-ttu-id="46ba9-169">**Adresář**</span><span class="sxs-lookup"><span data-stu-id="46ba9-169">**Directory**</span></span> | | | | |
| [<span data-ttu-id="46ba9-170">DNComponent</span><span class="sxs-lookup"><span data-stu-id="46ba9-170">DNComponent</span></span>](#dncomponent) |[<span data-ttu-id="46ba9-171">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="46ba9-171">DNComponentRev</span></span>](#dncomponentrev) |[<span data-ttu-id="46ba9-172">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="46ba9-172">EscapeDNComponent</span></span>](#escapedncomponent) | | |
| <span data-ttu-id="46ba9-173">**Vyhodnocení**</span><span class="sxs-lookup"><span data-stu-id="46ba9-173">**Evaluation**</span></span> | | | | |
| [<span data-ttu-id="46ba9-174">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="46ba9-174">IsBitSet</span></span>](#isbitset) |[<span data-ttu-id="46ba9-175">IsDate</span><span class="sxs-lookup"><span data-stu-id="46ba9-175">IsDate</span></span>](#isdate) |[<span data-ttu-id="46ba9-176">IsEmpty –</span><span class="sxs-lookup"><span data-stu-id="46ba9-176">IsEmpty</span></span>](#isempty) |[<span data-ttu-id="46ba9-177">IsGuid</span><span class="sxs-lookup"><span data-stu-id="46ba9-177">IsGuid</span></span>](#isguid) | |
| [<span data-ttu-id="46ba9-178">IsNull</span><span class="sxs-lookup"><span data-stu-id="46ba9-178">IsNull</span></span>](#isnull) |[<span data-ttu-id="46ba9-179">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="46ba9-179">IsNullOrEmpty</span></span>](#isnullorempty) |[<span data-ttu-id="46ba9-180">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="46ba9-180">IsNumeric</span></span>](#isnumeric) |[<span data-ttu-id="46ba9-181">IsPresent</span><span class="sxs-lookup"><span data-stu-id="46ba9-181">IsPresent</span></span>](#ispresent) | |
| [<span data-ttu-id="46ba9-182">IsString</span><span class="sxs-lookup"><span data-stu-id="46ba9-182">IsString</span></span>](#isstring) | | | | |
| <span data-ttu-id="46ba9-183">**Matematické**</span><span class="sxs-lookup"><span data-stu-id="46ba9-183">**Math**</span></span> | | | | |
| [<span data-ttu-id="46ba9-184">BitAnd</span><span class="sxs-lookup"><span data-stu-id="46ba9-184">BitAnd</span></span>](#bitand) |[<span data-ttu-id="46ba9-185">BitOr</span><span class="sxs-lookup"><span data-stu-id="46ba9-185">BitOr</span></span>](#bitor) |[<span data-ttu-id="46ba9-186">RandomNum</span><span class="sxs-lookup"><span data-stu-id="46ba9-186">RandomNum</span></span>](#randomnum) | | |
| <span data-ttu-id="46ba9-187">**Více hodnot**</span><span class="sxs-lookup"><span data-stu-id="46ba9-187">**Multi-valued**</span></span> | | | | |
| [<span data-ttu-id="46ba9-188">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="46ba9-188">Contains</span></span>](#contains) |[<span data-ttu-id="46ba9-189">Počet</span><span class="sxs-lookup"><span data-stu-id="46ba9-189">Count</span></span>](#count) |[<span data-ttu-id="46ba9-190">Položka</span><span class="sxs-lookup"><span data-stu-id="46ba9-190">Item</span></span>](#item) |[<span data-ttu-id="46ba9-191">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="46ba9-191">ItemOrNull</span></span>](#itemornull) | |
| [<span data-ttu-id="46ba9-192">Spojení</span><span class="sxs-lookup"><span data-stu-id="46ba9-192">Join</span></span>](#join) |[<span data-ttu-id="46ba9-193">Removeduplicates –</span><span class="sxs-lookup"><span data-stu-id="46ba9-193">RemoveDuplicates</span></span>](#removeduplicates) |[<span data-ttu-id="46ba9-194">Rozdělení</span><span class="sxs-lookup"><span data-stu-id="46ba9-194">Split</span></span>](#split) | | |
| <span data-ttu-id="46ba9-195">**Tok programu**</span><span class="sxs-lookup"><span data-stu-id="46ba9-195">**Program Flow**</span></span> | | | | |
| [<span data-ttu-id="46ba9-196">Chyba</span><span class="sxs-lookup"><span data-stu-id="46ba9-196">Error</span></span>](#error) |[<span data-ttu-id="46ba9-197">IIF</span><span class="sxs-lookup"><span data-stu-id="46ba9-197">IIF</span></span>](#iif) |[<span data-ttu-id="46ba9-198">Výběr</span><span class="sxs-lookup"><span data-stu-id="46ba9-198">Select</span></span>](#select) |[<span data-ttu-id="46ba9-199">Přepínače</span><span class="sxs-lookup"><span data-stu-id="46ba9-199">Switch</span></span>](#switch) | |
| [<span data-ttu-id="46ba9-200">Kde</span><span class="sxs-lookup"><span data-stu-id="46ba9-200">Where</span></span>](#where) |[<span data-ttu-id="46ba9-201">S</span><span class="sxs-lookup"><span data-stu-id="46ba9-201">With</span></span>](#with) | | | |
| <span data-ttu-id="46ba9-202">**Text**</span><span class="sxs-lookup"><span data-stu-id="46ba9-202">**Text**</span></span> | | | | |
| [<span data-ttu-id="46ba9-203">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="46ba9-203">GUID</span></span>](#guid) |[<span data-ttu-id="46ba9-204">InStr</span><span class="sxs-lookup"><span data-stu-id="46ba9-204">InStr</span></span>](#instr) |[<span data-ttu-id="46ba9-205">InStrRev</span><span class="sxs-lookup"><span data-stu-id="46ba9-205">InStrRev</span></span>](#instrrev) |[<span data-ttu-id="46ba9-206">LCase</span><span class="sxs-lookup"><span data-stu-id="46ba9-206">LCase</span></span>](#lcase) | |
| [<span data-ttu-id="46ba9-207">Vlevo</span><span class="sxs-lookup"><span data-stu-id="46ba9-207">Left</span></span>](#left) |[<span data-ttu-id="46ba9-208">Len</span><span class="sxs-lookup"><span data-stu-id="46ba9-208">Len</span></span>](#len) |[<span data-ttu-id="46ba9-209">LTrim</span><span class="sxs-lookup"><span data-stu-id="46ba9-209">LTrim</span></span>](#ltrim) |[<span data-ttu-id="46ba9-210">Mid –</span><span class="sxs-lookup"><span data-stu-id="46ba9-210">Mid</span></span>](#mid) | |
| [<span data-ttu-id="46ba9-211">PadLeft</span><span class="sxs-lookup"><span data-stu-id="46ba9-211">PadLeft</span></span>](#padleft) |[<span data-ttu-id="46ba9-212">PadRight –</span><span class="sxs-lookup"><span data-stu-id="46ba9-212">PadRight</span></span>](#padright) |[<span data-ttu-id="46ba9-213">PCase</span><span class="sxs-lookup"><span data-stu-id="46ba9-213">PCase</span></span>](#pcase) |[<span data-ttu-id="46ba9-214">Nahradit</span><span class="sxs-lookup"><span data-stu-id="46ba9-214">Replace</span></span>](#replace) | |
| [<span data-ttu-id="46ba9-215">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="46ba9-215">ReplaceChars</span></span>](#replacechars) |[<span data-ttu-id="46ba9-216">Vpravo</span><span class="sxs-lookup"><span data-stu-id="46ba9-216">Right</span></span>](#right) |[<span data-ttu-id="46ba9-217">RTrim</span><span class="sxs-lookup"><span data-stu-id="46ba9-217">RTrim</span></span>](#rtrim) |[<span data-ttu-id="46ba9-218">Uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="46ba9-218">Trim</span></span>](#trim) | |
| [<span data-ttu-id="46ba9-219">UCase</span><span class="sxs-lookup"><span data-stu-id="46ba9-219">UCase</span></span>](#ucase) |[<span data-ttu-id="46ba9-220">Aplikace Word</span><span class="sxs-lookup"><span data-stu-id="46ba9-220">Word</span></span>](#word) | | | |

- - -
### <a name="bitand"></a><span data-ttu-id="46ba9-221">BitAnd</span><span class="sxs-lookup"><span data-stu-id="46ba9-221">BitAnd</span></span>
<span data-ttu-id="46ba9-222">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-222">**Description:**</span></span>  
<span data-ttu-id="46ba9-223">Hello BitAnd – funkce nastaví na hodnotu určeného bits.</span><span class="sxs-lookup"><span data-stu-id="46ba9-223">hello BitAnd function sets specified bits on a value.</span></span>

<span data-ttu-id="46ba9-224">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-224">**Syntax:**</span></span>  
`num BitAnd(num value1, num value2)`

* <span data-ttu-id="46ba9-225">Hodnota1, hodnota2: číselných hodnot, které by měly být logickým společně</span><span class="sxs-lookup"><span data-stu-id="46ba9-225">value1, value2: numeric values that should be AND’ed together</span></span>

<span data-ttu-id="46ba9-226">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-226">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-227">Tato funkce převede binární reprezentace oba parametry toohello a nastaví trochu na:</span><span class="sxs-lookup"><span data-stu-id="46ba9-227">This function converts both parameters toohello binary representation and sets a bit to:</span></span>

* <span data-ttu-id="46ba9-228">0 – Pokud jeden nebo oba hello odpovídající bitů *maska* a *příznak* mají hodnotu 0</span><span class="sxs-lookup"><span data-stu-id="46ba9-228">0 - if one or both of hello corresponding bits in *mask* and *flag* are 0</span></span>
* <span data-ttu-id="46ba9-229">1 – Pokud jsou obě odpovídající bitů hello 1.</span><span class="sxs-lookup"><span data-stu-id="46ba9-229">1 - if both of hello corresponding bits are 1.</span></span>

<span data-ttu-id="46ba9-230">Jinými slovy vrátí hodnotu 0 ve všech případech, s výjimkou případů, kdy hello odpovídající oba parametry bitů 1.</span><span class="sxs-lookup"><span data-stu-id="46ba9-230">In other words, it returns 0 in all cases except when hello corresponding bits of both parameters are 1.</span></span>

<span data-ttu-id="46ba9-231">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-231">**Example:**</span></span>  
`BitAnd(&HF, &HF7)`  
<span data-ttu-id="46ba9-232">Vrátí 7, protože hexadecimální "F" a "F7" vyhodnotit toothis hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-232">Returns 7 because hexadecimal "F" AND "F7" evaluate toothis value.</span></span>

- - -
### <a name="bitor"></a><span data-ttu-id="46ba9-233">BitOr</span><span class="sxs-lookup"><span data-stu-id="46ba9-233">BitOr</span></span>
<span data-ttu-id="46ba9-234">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-234">**Description:**</span></span>  
<span data-ttu-id="46ba9-235">Hello BitOr – funkce nastaví na hodnotu určeného bits.</span><span class="sxs-lookup"><span data-stu-id="46ba9-235">hello BitOr function sets specified bits on a value.</span></span>

<span data-ttu-id="46ba9-236">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-236">**Syntax:**</span></span>  
`num BitOr(num value1, num value2)`

* <span data-ttu-id="46ba9-237">Hodnota1, hodnota2: číselných hodnot, které by měly být společně sjednocením (OR)</span><span class="sxs-lookup"><span data-stu-id="46ba9-237">value1, value2: numeric values that should be OR’ed together</span></span>

<span data-ttu-id="46ba9-238">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-238">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-239">Tato funkce převede binární reprezentace oba parametry toohello a nastaví bit too1, pokud jsou jedno nebo obě hello odpovídající bitů maska a příznak 1 a too0 Pokud jsou obě odpovídající bitů hello 0.</span><span class="sxs-lookup"><span data-stu-id="46ba9-239">This function converts both parameters toohello binary representation and sets a bit too1 if one or both of hello corresponding bits in mask and flag are 1, and too0 if both of hello corresponding bits are 0.</span></span> <span data-ttu-id="46ba9-240">Jinými slovy vrátí 1 ve všech případech, s výjimkou případů, kdy hello odpovídající bity oba parametry mají hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="46ba9-240">In other words, it returns 1 in all cases except where hello corresponding bits of both parameters are 0.</span></span>

- - -
### <a name="cbool"></a><span data-ttu-id="46ba9-241">CBool –</span><span class="sxs-lookup"><span data-stu-id="46ba9-241">CBool</span></span>
<span data-ttu-id="46ba9-242">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-242">**Description:**</span></span>  
<span data-ttu-id="46ba9-243">Hello CBool – funkce vrátí že logickou hodnotu na základě hello vyhodnocení výrazu</span><span class="sxs-lookup"><span data-stu-id="46ba9-243">hello CBool function returns a Boolean based on hello evaluated expression</span></span>

<span data-ttu-id="46ba9-244">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-244">**Syntax:**</span></span>  
`bool CBool(exp Expression)`

<span data-ttu-id="46ba9-245">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-245">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-246">Pokud hello výraz vyhodnotí tooa nenulovou hodnotu, pak CBool – vrátí hodnotu True, jinak vrátí hodnotu False.</span><span class="sxs-lookup"><span data-stu-id="46ba9-246">If hello expression evaluates tooa nonzero value, then CBool returns True, else it returns False.</span></span>

<span data-ttu-id="46ba9-247">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-247">**Example:**</span></span>  
`CBool([attrib1] = [attrib2])`  

<span data-ttu-id="46ba9-248">Vrátí hodnotu True, pokud oba atributy mít hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-248">Returns True if both attributes have hello same value.</span></span>

- - -
### <a name="cdate"></a><span data-ttu-id="46ba9-249">CDate –</span><span class="sxs-lookup"><span data-stu-id="46ba9-249">CDate</span></span>
<span data-ttu-id="46ba9-250">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-250">**Description:**</span></span>  
<span data-ttu-id="46ba9-251">Hello CDate – funkce vrátí z řetězce na datum a čas UTC.</span><span class="sxs-lookup"><span data-stu-id="46ba9-251">hello CDate function returns a UTC DateTime from a string.</span></span> <span data-ttu-id="46ba9-252">Data a času není typu nativní atribut synchronizované, ale je používána některé funkce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-252">DateTime is not a native attribute type in Sync but is used by some functions.</span></span>

<span data-ttu-id="46ba9-253">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-253">**Syntax:**</span></span>  
`dt CDate(str value)`

* <span data-ttu-id="46ba9-254">Hodnota: Řetězec datum, čas a volitelně časové pásmo</span><span class="sxs-lookup"><span data-stu-id="46ba9-254">Value: A string with a date, time, and optionally time zone</span></span>

<span data-ttu-id="46ba9-255">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-255">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-256">Hello byl vrácen řetězec je vždy ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="46ba9-256">hello returned string is always in UTC.</span></span>

<span data-ttu-id="46ba9-257">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-257">**Example:**</span></span>  
`CDate([employeeStartTime])`  
<span data-ttu-id="46ba9-258">Vrací hodnotu DateTime podle času zahájení hello zaměstnance</span><span class="sxs-lookup"><span data-stu-id="46ba9-258">Returns a DateTime based on hello employee’s start time</span></span>

`CDate("2013-01-10 4:00 PM -8")`  
<span data-ttu-id="46ba9-259">Vrátí data a času představující "2013-01-11 12:00:00"</span><span class="sxs-lookup"><span data-stu-id="46ba9-259">Returns a DateTime representing "2013-01-11 12:00 AM"</span></span>








- - -
### <a name="certextensionoids"></a><span data-ttu-id="46ba9-260">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="46ba9-260">CertExtensionOids</span></span>
<span data-ttu-id="46ba9-261">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-261">**Description:**</span></span>  
<span data-ttu-id="46ba9-262">Vrátí hello Oid hodnoty všech hello důležitých rozšíření certifikátů objektu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-262">Returns hello Oid values of all hello critical extensions of a certificate object.</span></span>

<span data-ttu-id="46ba9-263">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-263">**Syntax:**</span></span>  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-264">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-264">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-265">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-265">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certformat"></a><span data-ttu-id="46ba9-266">CertFormat</span><span class="sxs-lookup"><span data-stu-id="46ba9-266">CertFormat</span></span>
<span data-ttu-id="46ba9-267">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-267">**Description:**</span></span>  
<span data-ttu-id="46ba9-268">Vrátí hello název formátu hello tohoto certifikátu x.509 v3.</span><span class="sxs-lookup"><span data-stu-id="46ba9-268">Returns hello name of hello format of this X.509v3 certificate.</span></span>

<span data-ttu-id="46ba9-269">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-269">**Syntax:**</span></span>  
`str CertFormat(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-270">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-270">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-271">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-271">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certfriendlyname"></a><span data-ttu-id="46ba9-272">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="46ba9-272">CertFriendlyName</span></span>
<span data-ttu-id="46ba9-273">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-273">**Description:**</span></span>  
<span data-ttu-id="46ba9-274">Vrátí hello přidružené alias pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="46ba9-274">Returns hello associated alias for a certificate.</span></span>

<span data-ttu-id="46ba9-275">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-275">**Syntax:**</span></span>  
`str CertFriendlyName(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-276">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-276">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-277">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-277">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certhashstring"></a><span data-ttu-id="46ba9-278">CertHashString</span><span class="sxs-lookup"><span data-stu-id="46ba9-278">CertHashString</span></span>
<span data-ttu-id="46ba9-279">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-279">**Description:**</span></span>  
<span data-ttu-id="46ba9-280">Vrátí hodnotu hash SHA1 certifikátu x.509 v3 hello hello jako řetězec hexadecimálních znaků.</span><span class="sxs-lookup"><span data-stu-id="46ba9-280">Returns hello SHA1 hash value for hello X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="46ba9-281">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-281">**Syntax:**</span></span>  
`str CertHashString(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-282">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-282">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-283">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-283">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuer"></a><span data-ttu-id="46ba9-284">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="46ba9-284">CertIssuer</span></span>
<span data-ttu-id="46ba9-285">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-285">**Description:**</span></span>  
<span data-ttu-id="46ba9-286">Vrátí hello název hello certifikační autority, která vystavila certifikát x.509 v3 hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-286">Returns hello name of hello certificate authority that issued hello X.509v3 certificate.</span></span>

<span data-ttu-id="46ba9-287">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-287">**Syntax:**</span></span>  
`str CertIssuer(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-288">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-288">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-289">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-289">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuerdn"></a><span data-ttu-id="46ba9-290">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="46ba9-290">CertIssuerDN</span></span>
<span data-ttu-id="46ba9-291">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-291">**Description:**</span></span>  
<span data-ttu-id="46ba9-292">Vrátí hello rozlišující název vystavitele certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-292">Returns hello distinguished name of hello certificate issuer.</span></span>

<span data-ttu-id="46ba9-293">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-293">**Syntax:**</span></span>  
`str CertIssuerDN(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-294">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-294">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-295">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-295">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissueroid"></a><span data-ttu-id="46ba9-296">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-296">CertIssuerOid</span></span>
<span data-ttu-id="46ba9-297">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-297">**Description:**</span></span>  
<span data-ttu-id="46ba9-298">Vrátí hello Oid hello vystavitele certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-298">Returns hello Oid of hello certificate issuer.</span></span>

<span data-ttu-id="46ba9-299">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-299">**Syntax:**</span></span>  
`str CertIssuerOid(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-300">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-300">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-301">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-301">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithm"></a><span data-ttu-id="46ba9-302">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="46ba9-302">CertKeyAlgorithm</span></span>
<span data-ttu-id="46ba9-303">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-303">**Description:**</span></span>  
<span data-ttu-id="46ba9-304">Vrací informace hello algoritmus klíče pro tento certifikát x.509 v3 jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-304">Returns hello key algorithm information for this X.509v3 certificate as a string.</span></span>

<span data-ttu-id="46ba9-305">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-305">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-306">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-306">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-307">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-307">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithmparams"></a><span data-ttu-id="46ba9-308">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="46ba9-308">CertKeyAlgorithmParams</span></span>
<span data-ttu-id="46ba9-309">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-309">**Description:**</span></span>  
<span data-ttu-id="46ba9-310">Vrátí parametry hello algoritmus klíče pro certifikát x.509 v3 hello jako řetězec hexadecimálních znaků.</span><span class="sxs-lookup"><span data-stu-id="46ba9-310">Returns hello key algorithm parameters for hello X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="46ba9-311">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-311">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-312">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-312">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-313">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-313">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnameinfo"></a><span data-ttu-id="46ba9-314">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="46ba9-314">CertNameInfo</span></span>
<span data-ttu-id="46ba9-315">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-315">**Description:**</span></span>  
<span data-ttu-id="46ba9-316">Vrátí hello předmět a vystavitele názvy z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-316">Returns hello subject and issuer names from a certificate.</span></span>

<span data-ttu-id="46ba9-317">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-317">**Syntax:**</span></span>  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   <span data-ttu-id="46ba9-318">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-318">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-319">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-319">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
*   <span data-ttu-id="46ba9-320">X509NameType: hello X509NameType hodnoty pro předmět hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-320">X509NameType: hello X509NameType value for hello subject.</span></span>
*   <span data-ttu-id="46ba9-321">includesIssuerName: název vystavitele hello true tooinclude; jinak hodnota false.</span><span class="sxs-lookup"><span data-stu-id="46ba9-321">includesIssuerName: true tooinclude hello issuer name; otherwise, false.</span></span>

- - -
### <a name="certnotafter"></a><span data-ttu-id="46ba9-322">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="46ba9-322">CertNotAfter</span></span>
<span data-ttu-id="46ba9-323">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-323">**Description:**</span></span>  
<span data-ttu-id="46ba9-324">Vrátí hello datum v místní čase, po kterém certifikát je již neplatný.</span><span class="sxs-lookup"><span data-stu-id="46ba9-324">Returns hello date in local time after which a certificate is no longer valid.</span></span>

<span data-ttu-id="46ba9-325">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-325">**Syntax:**</span></span>  
`dt CertNotAfter(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-326">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-326">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-327">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-327">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnotbefore"></a><span data-ttu-id="46ba9-328">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="46ba9-328">CertNotBefore</span></span>
<span data-ttu-id="46ba9-329">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-329">**Description:**</span></span>  
<span data-ttu-id="46ba9-330">Vrátí hello datum v místní čase, při kterém certifikát začne platit.</span><span class="sxs-lookup"><span data-stu-id="46ba9-330">Returns hello date in local time on which a certificate becomes valid.</span></span>

<span data-ttu-id="46ba9-331">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-331">**Syntax:**</span></span>  
`dt CertNotBefore(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-332">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-332">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-333">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-333">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyoid"></a><span data-ttu-id="46ba9-334">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-334">CertPublicKeyOid</span></span>
<span data-ttu-id="46ba9-335">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-335">**Description:**</span></span>  
<span data-ttu-id="46ba9-336">Vrátí hello Oid hello veřejný klíč pro certifikát x.509 v3 hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-336">Returns hello Oid of hello public key for hello X.509v3 certificate.</span></span>

<span data-ttu-id="46ba9-337">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-337">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-338">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-338">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-339">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-339">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyparametersoid"></a><span data-ttu-id="46ba9-340">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-340">CertPublicKeyParametersOid</span></span>
<span data-ttu-id="46ba9-341">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-341">**Description:**</span></span>  
<span data-ttu-id="46ba9-342">Vrátí hello Oid parametrů hello veřejného klíče pro certifikát x.509 v3 hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-342">Returns hello Oid of hello public key parameters for hello X.509v3 certificate.</span></span>

<span data-ttu-id="46ba9-343">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-343">**Syntax:**</span></span>  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-344">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-344">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-345">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-345">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certserialnumber"></a><span data-ttu-id="46ba9-346">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="46ba9-346">CertSerialNumber</span></span>
<span data-ttu-id="46ba9-347">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-347">**Description:**</span></span>  
<span data-ttu-id="46ba9-348">Vrátí hello sériové číslo certifikátu x.509 v3 hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-348">Returns hello serial number of hello X.509v3 certificate.</span></span>

<span data-ttu-id="46ba9-349">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-349">**Syntax:**</span></span>  
`str CertSerialNumber(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-350">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-350">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-351">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-351">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsignaturealgorithmoid"></a><span data-ttu-id="46ba9-352">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-352">CertSignatureAlgorithmOid</span></span>
<span data-ttu-id="46ba9-353">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-353">**Description:**</span></span>  
<span data-ttu-id="46ba9-354">Vrátí hello Oid algoritmu hello používá toocreate hello podpisu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-354">Returns hello Oid of hello algorithm used toocreate hello signature of a certificate.</span></span>

<span data-ttu-id="46ba9-355">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-355">**Syntax:**</span></span>  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-356">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-356">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-357">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-357">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubject"></a><span data-ttu-id="46ba9-358">CertSubject</span><span class="sxs-lookup"><span data-stu-id="46ba9-358">CertSubject</span></span>
<span data-ttu-id="46ba9-359">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-359">**Description:**</span></span>  
<span data-ttu-id="46ba9-360">Získá hello rozlišující název subjektu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-360">Gets hello subject distinguished name from a certificate.</span></span>

<span data-ttu-id="46ba9-361">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-361">**Syntax:**</span></span>  
`str CertSubject(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-362">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-362">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-363">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-363">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnamedn"></a><span data-ttu-id="46ba9-364">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="46ba9-364">CertSubjectNameDN</span></span>
<span data-ttu-id="46ba9-365">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-365">**Description:**</span></span>  
<span data-ttu-id="46ba9-366">Vrátí hello rozlišující název subjektu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-366">Returns hello subject distinguished name from a certificate.</span></span>

<span data-ttu-id="46ba9-367">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-367">**Syntax:**</span></span>  
`str CertSubjectNameDN(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-368">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-368">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-369">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-369">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnameoid"></a><span data-ttu-id="46ba9-370">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="46ba9-370">CertSubjectNameOid</span></span>
<span data-ttu-id="46ba9-371">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-371">**Description:**</span></span>  
<span data-ttu-id="46ba9-372">Vrátí hello Oid hello název subjektu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-372">Returns hello Oid of hello subject name from a certificate.</span></span>

<span data-ttu-id="46ba9-373">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-373">**Syntax:**</span></span>  
`str CertSubjectNameOid(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-374">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-374">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-375">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-375">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certthumbprint"></a><span data-ttu-id="46ba9-376">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="46ba9-376">CertThumbprint</span></span>
<span data-ttu-id="46ba9-377">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-377">**Description:**</span></span>  
<span data-ttu-id="46ba9-378">Vrátí hello kryptografického otisku certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-378">Returns hello thumbprint of a certificate.</span></span>

<span data-ttu-id="46ba9-379">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-379">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-380">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-380">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-381">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-381">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certversion"></a><span data-ttu-id="46ba9-382">CertVersion</span><span class="sxs-lookup"><span data-stu-id="46ba9-382">CertVersion</span></span>
<span data-ttu-id="46ba9-383">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-383">**Description:**</span></span>  
<span data-ttu-id="46ba9-384">Vrátí hello verze formátu X.509 certifikátu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-384">Returns hello X.509 format version of a certificate.</span></span>

<span data-ttu-id="46ba9-385">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-385">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-386">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-386">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-387">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-387">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="cguid"></a><span data-ttu-id="46ba9-388">CGuid</span><span class="sxs-lookup"><span data-stu-id="46ba9-388">CGuid</span></span>
<span data-ttu-id="46ba9-389">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-389">**Description:**</span></span>  
<span data-ttu-id="46ba9-390">Hello CGuid funkce převede řetězcovou reprezentaci hello binární reprezentace tooits identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="46ba9-390">hello CGuid function converts hello string representation of a GUID tooits binary representation.</span></span>

<span data-ttu-id="46ba9-391">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-391">**Syntax:**</span></span>  
`bin CGuid(str GUID)`

* <span data-ttu-id="46ba9-392">Řetězec formátu v tomto vzoru: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="46ba9-392">A String formatted in this pattern: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

- - -
### <a name="contains"></a><span data-ttu-id="46ba9-393">Contains</span><span class="sxs-lookup"><span data-stu-id="46ba9-393">Contains</span></span>
<span data-ttu-id="46ba9-394">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-394">**Description:**</span></span>  
<span data-ttu-id="46ba9-395">Hello obsahuje funkce najde řetězec uvnitř vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-395">hello Contains function finds a string inside a multi-valued attribute</span></span>

<span data-ttu-id="46ba9-396">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-396">**Syntax:**</span></span>  
<span data-ttu-id="46ba9-397">`num Contains (mvstring attribute, str search)`-malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="46ba9-397">`num Contains (mvstring attribute, str search)` - case-sensitive</span></span>  
`num Contains (mvstring attribute, str search, enum Casetype)`  
<span data-ttu-id="46ba9-398">`num Contains (mvref attribute, str search)`-malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="46ba9-398">`num Contains (mvref attribute, str search)` - case-sensitive</span></span>

* <span data-ttu-id="46ba9-399">Atribut: toosearch hello více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="46ba9-399">attribute: hello multi-valued attribute toosearch.</span></span>
* <span data-ttu-id="46ba9-400">hledání: řetězec toofind v atributu hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-400">search: string toofind in hello attribute.</span></span>
* <span data-ttu-id="46ba9-401">Casetype: CaseInsensitive nebo CaseSensitive.</span><span class="sxs-lookup"><span data-stu-id="46ba9-401">Casetype: CaseInsensitive or CaseSensitive.</span></span>

<span data-ttu-id="46ba9-402">Vrátí index hello více hodnot atributů, kde hello řetězce byl nalezen.</span><span class="sxs-lookup"><span data-stu-id="46ba9-402">Returns index in hello multi-valued attribute where hello string was found.</span></span> <span data-ttu-id="46ba9-403">0 je vrácena, pokud řetězec hello nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="46ba9-403">0 is returned if hello string is not found.</span></span>

<span data-ttu-id="46ba9-404">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-404">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-405">Pro více hodnot řetězec atributy hello hledáním dílčích řetězců v hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="46ba9-405">For multi-valued string attributes, hello search finds substrings in hello values.</span></span>  
<span data-ttu-id="46ba9-406">Pro atributy typu odkaz hello vyhledávaná řetězec musí přesně shodovat toobe hodnota hello považovány za shodné.</span><span class="sxs-lookup"><span data-stu-id="46ba9-406">For reference attributes, hello searched string must exactly match hello value toobe considered a match.</span></span>

<span data-ttu-id="46ba9-407">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-407">**Example:**</span></span>  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
<span data-ttu-id="46ba9-408">Pokud atribut proxyAddresses hello má primární e-mailovou adresu (indikován velká písmena "SMTP:"), pak se vraťte hello proxyAddress atribut, jinak vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-408">If hello proxyAddresses attribute has a primary email address (indicated by uppercase "SMTP:"), then return hello proxyAddress attribute, else return an error.</span></span>

- - -
### <a name="convertfrombase64"></a><span data-ttu-id="46ba9-409">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="46ba9-409">ConvertFromBase64</span></span>
<span data-ttu-id="46ba9-410">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-410">**Description:**</span></span>  
<span data-ttu-id="46ba9-411">Hello ConvertFromBase64 funkce převede hello zadat regulární řetězec s kódováním base64 hodnotu tooa.</span><span class="sxs-lookup"><span data-stu-id="46ba9-411">hello ConvertFromBase64 function converts hello specified base64 encoded value tooa regular string.</span></span>

<span data-ttu-id="46ba9-412">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-412">**Syntax:**</span></span>  
<span data-ttu-id="46ba9-413">`str ConvertFromBase64(str source)`-předpokládá pro kódování Unicode</span><span class="sxs-lookup"><span data-stu-id="46ba9-413">`str ConvertFromBase64(str source)` - assumes Unicode for encoding</span></span>  
`str ConvertFromBase64(str source, enum Encoding)`

* <span data-ttu-id="46ba9-414">Zdroj: řetězec kódování Base64</span><span class="sxs-lookup"><span data-stu-id="46ba9-414">source: Base64 encoded string</span></span>  
* <span data-ttu-id="46ba9-415">Kódování: Znakové sady Unicode, ASCII, UTF8</span><span class="sxs-lookup"><span data-stu-id="46ba9-415">Encoding: Unicode, ASCII, UTF8</span></span>

<span data-ttu-id="46ba9-416">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="46ba9-416">**Example**</span></span>  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

<span data-ttu-id="46ba9-417">Vrátí oba příklady "*Hello, world!*"</span><span class="sxs-lookup"><span data-stu-id="46ba9-417">Both examples return "*Hello world!*"</span></span>

- - -
### <a name="convertfromutf8hex"></a><span data-ttu-id="46ba9-418">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="46ba9-418">ConvertFromUTF8Hex</span></span>
<span data-ttu-id="46ba9-419">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-419">**Description:**</span></span>  
<span data-ttu-id="46ba9-420">Hello ConvertFromUTF8Hex funkce převede hello zadaný řetězec tooa UTF8 šestnáctkově kódování.</span><span class="sxs-lookup"><span data-stu-id="46ba9-420">hello ConvertFromUTF8Hex function converts hello specified UTF8 Hex encoded value tooa string.</span></span>

<span data-ttu-id="46ba9-421">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-421">**Syntax:**</span></span>  
`str ConvertFromUTF8Hex(str source)`

* <span data-ttu-id="46ba9-422">Zdroj: UTF8 2bajtová kódovaného palčivost</span><span class="sxs-lookup"><span data-stu-id="46ba9-422">source: UTF8 2-byte encoded sting</span></span>

<span data-ttu-id="46ba9-423">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-423">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-424">Hello rozdíl mezi tato funkce a ConvertFromBase64([],UTF8) ve výsledku této hello je popisný hello rozlišující název atributu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-424">hello difference between this function and ConvertFromBase64([],UTF8) in that hello result is friendly for hello DN attribute.</span></span>  
<span data-ttu-id="46ba9-425">Tento formát se službou Azure Active Directory používá jako rozlišující název.</span><span class="sxs-lookup"><span data-stu-id="46ba9-425">This format is used by Azure Active Directory as DN.</span></span>

<span data-ttu-id="46ba9-426">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-426">**Example:**</span></span>  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
<span data-ttu-id="46ba9-427">Vrátí "*Hello, world!*"</span><span class="sxs-lookup"><span data-stu-id="46ba9-427">Returns "*Hello world!*"</span></span>

- - -
### <a name="converttobase64"></a><span data-ttu-id="46ba9-428">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="46ba9-428">ConvertToBase64</span></span>
<span data-ttu-id="46ba9-429">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-429">**Description:**</span></span>  
<span data-ttu-id="46ba9-430">Hello ConvertToBase64 funkce převede tooa řetězec base64 řetězec znaků Unicode.</span><span class="sxs-lookup"><span data-stu-id="46ba9-430">hello ConvertToBase64 function converts a string tooa Unicode base64 string.</span></span>  
<span data-ttu-id="46ba9-431">Převede hello hodnotu v poli celých čísel tooits ekvivalentní řetězcovou reprezentaci, který je zakódován pomocí kódování base-64 číslic.</span><span class="sxs-lookup"><span data-stu-id="46ba9-431">Converts hello value of an array of integers tooits equivalent string representation that is encoded with base-64 digits.</span></span>

<span data-ttu-id="46ba9-432">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-432">**Syntax:**</span></span>  
`str ConvertToBase64(str source)`

<span data-ttu-id="46ba9-433">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-433">**Example:**</span></span>  
`ConvertToBase64("Hello world!")`  
<span data-ttu-id="46ba9-434">Vrátí "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span><span class="sxs-lookup"><span data-stu-id="46ba9-434">Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span></span>

- - -
### <a name="converttoutf8hex"></a><span data-ttu-id="46ba9-435">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="46ba9-435">ConvertToUTF8Hex</span></span>
<span data-ttu-id="46ba9-436">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-436">**Description:**</span></span>  
<span data-ttu-id="46ba9-437">Hello ConvertToUTF8Hex funkce převede řetězec tooa UTF8 šestnáctkově kódovaný hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-437">hello ConvertToUTF8Hex function converts a string tooa UTF8 Hex encoded value.</span></span>

<span data-ttu-id="46ba9-438">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-438">**Syntax:**</span></span>  
`str ConvertToUTF8Hex(str source)`

<span data-ttu-id="46ba9-439">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-439">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-440">výstupní formát Hello této funkce se službou Azure Active Directory používá jako atribut formátu DN.</span><span class="sxs-lookup"><span data-stu-id="46ba9-440">hello output format of this function is used by Azure Active Directory as DN attribute format.</span></span>

<span data-ttu-id="46ba9-441">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-441">**Example:**</span></span>  
`ConvertToUTF8Hex("Hello world!")`  
<span data-ttu-id="46ba9-442">Vrátí 48656C6C6F20776F726C6421</span><span class="sxs-lookup"><span data-stu-id="46ba9-442">Returns 48656C6C6F20776F726C6421</span></span>

- - -
### <a name="count"></a><span data-ttu-id="46ba9-443">Počet</span><span class="sxs-lookup"><span data-stu-id="46ba9-443">Count</span></span>
<span data-ttu-id="46ba9-444">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-444">**Description:**</span></span>  
<span data-ttu-id="46ba9-445">Hello funkce Count vrátí hello počet elementů ve více hodnotami atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-445">hello Count function returns hello number of elements in a multi-valued attribute</span></span>

<span data-ttu-id="46ba9-446">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-446">**Syntax:**</span></span>  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a><span data-ttu-id="46ba9-447">CNum</span><span class="sxs-lookup"><span data-stu-id="46ba9-447">CNum</span></span>
<span data-ttu-id="46ba9-448">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-448">**Description:**</span></span>  
<span data-ttu-id="46ba9-449">Hello CNum funkce přebírá řetězec a vrátí číselným datovým typem.</span><span class="sxs-lookup"><span data-stu-id="46ba9-449">hello CNum function takes a string and returns a numeric data type.</span></span>

<span data-ttu-id="46ba9-450">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-450">**Syntax:**</span></span>  
`num CNum(str value)`

- - -
### <a name="cref"></a><span data-ttu-id="46ba9-451">CRef –</span><span class="sxs-lookup"><span data-stu-id="46ba9-451">CRef</span></span>
<span data-ttu-id="46ba9-452">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-452">**Description:**</span></span>  
<span data-ttu-id="46ba9-453">Převede atribut typu odkaz tooa řetězec</span><span class="sxs-lookup"><span data-stu-id="46ba9-453">Converts a string tooa reference attribute</span></span>

<span data-ttu-id="46ba9-454">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-454">**Syntax:**</span></span>  
`ref CRef(str value)`

<span data-ttu-id="46ba9-455">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-455">**Example:**</span></span>  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a><span data-ttu-id="46ba9-456">CStr</span><span class="sxs-lookup"><span data-stu-id="46ba9-456">CStr</span></span>
<span data-ttu-id="46ba9-457">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-457">**Description:**</span></span>  
<span data-ttu-id="46ba9-458">CStr – Funkce Hello převede tooa string – datový typ.</span><span class="sxs-lookup"><span data-stu-id="46ba9-458">hello CStr function converts tooa string data type.</span></span>

<span data-ttu-id="46ba9-459">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-459">**Syntax:**</span></span>  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* <span data-ttu-id="46ba9-460">hodnota: může být číselnou hodnotu, atribut odkaz nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="46ba9-460">value: Can be a numeric value, reference attribute, or Boolean.</span></span>

<span data-ttu-id="46ba9-461">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-461">**Example:**</span></span>  
`CStr([dn])`  
<span data-ttu-id="46ba9-462">Může vrátit "cn = Jan, dc = contoso, dc = com"</span><span class="sxs-lookup"><span data-stu-id="46ba9-462">Could return "cn=Joe,dc=contoso,dc=com"</span></span>

- - -
### <a name="dateadd"></a><span data-ttu-id="46ba9-463">Funkce DateAdd</span><span class="sxs-lookup"><span data-stu-id="46ba9-463">DateAdd</span></span>
<span data-ttu-id="46ba9-464">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-464">**Description:**</span></span>  
<span data-ttu-id="46ba9-465">Vrátí datum obsahující toowhich datum, byl přidán do zadaného časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-465">Returns a Date containing a date toowhich a specified time interval has been added.</span></span>

<span data-ttu-id="46ba9-466">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-466">**Syntax:**</span></span>  
`dt DateAdd(str interval, num value, dt date)`

* <span data-ttu-id="46ba9-467">interval: řetězec výraz, který je hello interval, když chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="46ba9-467">interval: String expression that is hello interval of time you want tooadd.</span></span> <span data-ttu-id="46ba9-468">Hello řetězec musí mít jednu z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="46ba9-468">hello string must have one of hello following values:</span></span>
  * <span data-ttu-id="46ba9-469">rrrr roku</span><span class="sxs-lookup"><span data-stu-id="46ba9-469">yyyy Year</span></span>
  * <span data-ttu-id="46ba9-470">q čtvrtletí.</span><span class="sxs-lookup"><span data-stu-id="46ba9-470">q Quarter</span></span>
  * <span data-ttu-id="46ba9-471">m měsíc</span><span class="sxs-lookup"><span data-stu-id="46ba9-471">m Month</span></span>
  * <span data-ttu-id="46ba9-472">y den roku</span><span class="sxs-lookup"><span data-stu-id="46ba9-472">y Day of year</span></span>
  * <span data-ttu-id="46ba9-473">d den</span><span class="sxs-lookup"><span data-stu-id="46ba9-473">d Day</span></span>
  * <span data-ttu-id="46ba9-474">w den v týdnu</span><span class="sxs-lookup"><span data-stu-id="46ba9-474">w Weekday</span></span>
  * <span data-ttu-id="46ba9-475">TT týden</span><span class="sxs-lookup"><span data-stu-id="46ba9-475">ww Week</span></span>
  * <span data-ttu-id="46ba9-476">h hodinu</span><span class="sxs-lookup"><span data-stu-id="46ba9-476">h Hour</span></span>
  * <span data-ttu-id="46ba9-477">n minutu</span><span class="sxs-lookup"><span data-stu-id="46ba9-477">n Minute</span></span>
  * <span data-ttu-id="46ba9-478">s druhou</span><span class="sxs-lookup"><span data-stu-id="46ba9-478">s Second</span></span>
* <span data-ttu-id="46ba9-479">hodnota: hello počet jednotek chcete tooadd.</span><span class="sxs-lookup"><span data-stu-id="46ba9-479">value: hello number of units you want tooadd.</span></span> <span data-ttu-id="46ba9-480">Může být kladná (tooget data v budoucnu hello) nebo záporné (tooget data v minulosti hello).</span><span class="sxs-lookup"><span data-stu-id="46ba9-480">It can be positive (tooget dates in hello future) or negative (tooget dates in hello past).</span></span>
* <span data-ttu-id="46ba9-481">datum: data a času představující datum toowhich hello interval je přidána.</span><span class="sxs-lookup"><span data-stu-id="46ba9-481">date: DateTime representing date toowhich hello interval is added.</span></span>

<span data-ttu-id="46ba9-482">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-482">**Example:**</span></span>  
`DateAdd("m", 3, CDate("2001-01-01"))`  
<span data-ttu-id="46ba9-483">Přidá 3 měsíce a vrátí hodnotu DateTime představující "2001-04-01".</span><span class="sxs-lookup"><span data-stu-id="46ba9-483">Adds 3 months and returns a DateTime representing "2001-04-01".</span></span>

- - -
### <a name="datefromnum"></a><span data-ttu-id="46ba9-484">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="46ba9-484">DateFromNum</span></span>
<span data-ttu-id="46ba9-485">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-485">**Description:**</span></span>  
<span data-ttu-id="46ba9-486">Hello DateFromNum funkce převede hodnotu v AD na datum formátu tooa typu datum a čas.</span><span class="sxs-lookup"><span data-stu-id="46ba9-486">hello DateFromNum function converts a value in AD’s date format tooa DateTime type.</span></span>

<span data-ttu-id="46ba9-487">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-487">**Syntax:**</span></span>  
`dt DateFromNum(num value)`

<span data-ttu-id="46ba9-488">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-488">**Example:**</span></span>  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
<span data-ttu-id="46ba9-489">Vrací hodnotu DateTime představující 2012-01-01 23:00:00</span><span class="sxs-lookup"><span data-stu-id="46ba9-489">Returns a DateTime representing 2012-01-01 23:00:00</span></span>

- - -
### <a name="dncomponent"></a><span data-ttu-id="46ba9-490">DNComponent</span><span class="sxs-lookup"><span data-stu-id="46ba9-490">DNComponent</span></span>
<span data-ttu-id="46ba9-491">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-491">**Description:**</span></span>  
<span data-ttu-id="46ba9-492">Hello DNComponent funkce vrátí hodnotu hello zadanou součástí rozlišující název budete zleva.</span><span class="sxs-lookup"><span data-stu-id="46ba9-492">hello DNComponent function returns hello value of a specified DN component going from left.</span></span>

<span data-ttu-id="46ba9-493">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-493">**Syntax:**</span></span>  
`str DNComponent(ref dn, num ComponentNumber)`

* <span data-ttu-id="46ba9-494">rozlišující název: hello odkaz na atribut toointerpret</span><span class="sxs-lookup"><span data-stu-id="46ba9-494">dn: hello reference attribute toointerpret</span></span>
* <span data-ttu-id="46ba9-495">ComponentNumber: hello součástí tooreturn hello rozlišující název</span><span class="sxs-lookup"><span data-stu-id="46ba9-495">ComponentNumber: hello component in hello DN tooreturn</span></span>

<span data-ttu-id="46ba9-496">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-496">**Example:**</span></span>  
`DNComponent([dn],1)`  
<span data-ttu-id="46ba9-497">Pokud je rozlišující název "cn = Jan, ou =...," vrátí Jan</span><span class="sxs-lookup"><span data-stu-id="46ba9-497">If dn is "cn=Joe,ou=…," it returns Joe</span></span>

- - -
### <a name="dncomponentrev"></a><span data-ttu-id="46ba9-498">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="46ba9-498">DNComponentRev</span></span>
<span data-ttu-id="46ba9-499">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-499">**Description:**</span></span>  
<span data-ttu-id="46ba9-500">Hello DNComponentRev funkce vrátí hodnotu hello zadanou součástí rozlišující název budete zprava (hello end).</span><span class="sxs-lookup"><span data-stu-id="46ba9-500">hello DNComponentRev function returns hello value of a specified DN component going from right (hello end).</span></span>

<span data-ttu-id="46ba9-501">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-501">**Syntax:**</span></span>  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* <span data-ttu-id="46ba9-502">rozlišující název: hello odkaz na atribut toointerpret</span><span class="sxs-lookup"><span data-stu-id="46ba9-502">dn: hello reference attribute toointerpret</span></span>
* <span data-ttu-id="46ba9-503">ComponentNumber - hello součástí tooreturn hello rozlišující název</span><span class="sxs-lookup"><span data-stu-id="46ba9-503">ComponentNumber - hello component in hello DN tooreturn</span></span>
* <span data-ttu-id="46ba9-504">Možnosti: Řadič domény – ignorovat všechny součásti s "dc ="</span><span class="sxs-lookup"><span data-stu-id="46ba9-504">Options: DC – Ignore all components with "dc="</span></span>

<span data-ttu-id="46ba9-505">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-505">**Example:**</span></span>  
<span data-ttu-id="46ba9-506">Pokud rozlišující název "cn Jan, ou = Atlantu, ou = GA, ou = = USA, řadič domény = contoso, dc = com" pak</span><span class="sxs-lookup"><span data-stu-id="46ba9-506">If dn is "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" then</span></span>  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
<span data-ttu-id="46ba9-507">Obě vrací nás.</span><span class="sxs-lookup"><span data-stu-id="46ba9-507">Both return US.</span></span>

- - -
### <a name="error"></a><span data-ttu-id="46ba9-508">Chyba</span><span class="sxs-lookup"><span data-stu-id="46ba9-508">Error</span></span>
<span data-ttu-id="46ba9-509">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-509">**Description:**</span></span>  
<span data-ttu-id="46ba9-510">Hello Chyba funkce je použité tooreturn o vlastní chybě.</span><span class="sxs-lookup"><span data-stu-id="46ba9-510">hello Error function is used tooreturn a custom error.</span></span>

<span data-ttu-id="46ba9-511">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-511">**Syntax:**</span></span>  
`void Error(str ErrorMessage)`

<span data-ttu-id="46ba9-512">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-512">**Example:**</span></span>  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
<span data-ttu-id="46ba9-513">Pokud hello atribut accountName není k dispozici, vyvolá chybu, hello objektu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-513">If hello attribute accountName is not present, throw an error on hello object.</span></span>

- - -
### <a name="escapedncomponent"></a><span data-ttu-id="46ba9-514">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="46ba9-514">EscapeDNComponent</span></span>
<span data-ttu-id="46ba9-515">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-515">**Description:**</span></span>  
<span data-ttu-id="46ba9-516">Hello EscapeDNComponent funkce využívá jednu součást názvu domény a řídicí sekvence, může být reprezentován v protokolu LDAP.</span><span class="sxs-lookup"><span data-stu-id="46ba9-516">hello EscapeDNComponent function takes one component of a DN and escapes it so it can be represented in LDAP.</span></span>

<span data-ttu-id="46ba9-517">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-517">**Syntax:**</span></span>  
`str EscapeDNComponent(str value)`

<span data-ttu-id="46ba9-518">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-518">**Example:**</span></span>  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
<span data-ttu-id="46ba9-519">Zajistí, že objekt hello lze vytvořit v adresáři LDAP, i v případě, že atribut hello displayName obsahuje znaky, které je nutné uvést v protokolu LDAP.</span><span class="sxs-lookup"><span data-stu-id="46ba9-519">Makes sure hello object can be created in an LDAP directory even if hello displayName attribute has characters that must be escaped in LDAP.</span></span>

- - -
### <a name="formatdatetime"></a><span data-ttu-id="46ba9-520">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="46ba9-520">FormatDateTime</span></span>
<span data-ttu-id="46ba9-521">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-521">**Description:**</span></span>  
<span data-ttu-id="46ba9-522">Funkce FormatDateTime Hello je použité tooformat řetězec tooa DateTime s zadaný formát</span><span class="sxs-lookup"><span data-stu-id="46ba9-522">hello FormatDateTime function is used tooformat a DateTime tooa string with a specified format</span></span>

<span data-ttu-id="46ba9-523">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-523">**Syntax:**</span></span>  
`str FormatDateTime(dt value, str format)`

* <span data-ttu-id="46ba9-524">hodnota: hodnotu ve formátu data a času hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-524">value: a value in hello DateTime format</span></span>
* <span data-ttu-id="46ba9-525">formát: řetězec představující tooconvert formátu hello k.</span><span class="sxs-lookup"><span data-stu-id="46ba9-525">format: a string representing hello format tooconvert to.</span></span>

<span data-ttu-id="46ba9-526">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-526">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-527">Hello možné hodnoty pro formát hello naleznete zde: [uživatelem definované formátu data a času (formát funkce)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span><span class="sxs-lookup"><span data-stu-id="46ba9-527">hello possible values for hello format can be found here: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span></span>

<span data-ttu-id="46ba9-528">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-528">**Example:**</span></span>  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
<span data-ttu-id="46ba9-529">Výsledkem "2007-12-25".</span><span class="sxs-lookup"><span data-stu-id="46ba9-529">Results in "2007-12-25".</span></span>

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
<span data-ttu-id="46ba9-530">Může mít za následek "20140905081453.0Z"</span><span class="sxs-lookup"><span data-stu-id="46ba9-530">Can result in "20140905081453.0Z"</span></span>

- - -
### <a name="guid"></a><span data-ttu-id="46ba9-531">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="46ba9-531">GUID</span></span>
<span data-ttu-id="46ba9-532">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-532">**Description:**</span></span>  
<span data-ttu-id="46ba9-533">Funkce Hello GUID generuje nový náhodný identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="46ba9-533">hello function GUID generates a new random GUID</span></span>

<span data-ttu-id="46ba9-534">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-534">**Syntax:**</span></span>  
`str GUID()`

- - -
### <a name="iif"></a><span data-ttu-id="46ba9-535">IIF</span><span class="sxs-lookup"><span data-stu-id="46ba9-535">IIF</span></span>
<span data-ttu-id="46ba9-536">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-536">**Description:**</span></span>  
<span data-ttu-id="46ba9-537">Hello funkce IIF vrátí jednu z možných hodnot na základě podmínky pro zadanou sadu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-537">hello IIF function returns one of a set of possible values based on a specified condition.</span></span>

<span data-ttu-id="46ba9-538">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-538">**Syntax:**</span></span>  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* <span data-ttu-id="46ba9-539">Podmínka: Libovolná hodnota nebo výraz, který lze vyhodnotit tootrue, nebo hodnotu NEPRAVDA.</span><span class="sxs-lookup"><span data-stu-id="46ba9-539">condition: any value or expression that can be evaluated tootrue or false.</span></span>
* <span data-ttu-id="46ba9-540">valueIfTrue: Pokud hello podmínka vyhodnocena jako tootrue, hello vrátil hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-540">valueIfTrue: If hello condition evaluates tootrue, hello returned value.</span></span>
* <span data-ttu-id="46ba9-541">valueIfFalse: Pokud hello podmínka vyhodnocena jako toofalse, hello vrátil hodnotu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-541">valueIfFalse: If hello condition evaluates toofalse, hello returned value.</span></span>

<span data-ttu-id="46ba9-542">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-542">**Example:**</span></span>  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 <span data-ttu-id="46ba9-543">Pokud je uživatel hello učně, vrátí přidat hello alias uživatele s "t-" vrátí toohello začátku ho jiný alias hello uživatele, jako je.</span><span class="sxs-lookup"><span data-stu-id="46ba9-543">If hello user is an intern, returns hello alias of a user with "t-" added toohello beginning of it, else returns hello user’s alias as is.</span></span>

- - -
### <a name="instr"></a><span data-ttu-id="46ba9-544">InStr</span><span class="sxs-lookup"><span data-stu-id="46ba9-544">InStr</span></span>
<span data-ttu-id="46ba9-545">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-545">**Description:**</span></span>  
<span data-ttu-id="46ba9-546">Hello InStr Funkce hello první výskyt je dílčí řetězec najde v řetězci</span><span class="sxs-lookup"><span data-stu-id="46ba9-546">hello InStr function finds hello first occurrence of a substring in a string</span></span>

<span data-ttu-id="46ba9-547">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-547">**Syntax:**</span></span>  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* <span data-ttu-id="46ba9-548">prohledávaný_řetězec: řetězec toobe vyhledávat</span><span class="sxs-lookup"><span data-stu-id="46ba9-548">stringcheck: string toobe searched</span></span>
* <span data-ttu-id="46ba9-549">hledaný_řetězec: řetězec toobe nalezen</span><span class="sxs-lookup"><span data-stu-id="46ba9-549">stringmatch: string toobe found</span></span>
* <span data-ttu-id="46ba9-550">spustit: počáteční pozice toofind hello substring</span><span class="sxs-lookup"><span data-stu-id="46ba9-550">start: starting position toofind hello substring</span></span>
* <span data-ttu-id="46ba9-551">porovnání: vbTextCompare nebo vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="46ba9-551">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="46ba9-552">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-552">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-553">Vrátí pozici hello kde nebyl nalezen dílčí řetězec hello nebo 0, pokud nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="46ba9-553">Returns hello position where hello substring was found or 0 if not found.</span></span>

<span data-ttu-id="46ba9-554">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-554">**Example:**</span></span>  
`InStr("hello quick brown fox","quick")`  
<span data-ttu-id="46ba9-555">Too5 hodnoty</span><span class="sxs-lookup"><span data-stu-id="46ba9-555">Evalues too5</span></span>

`InStr("repEated","e",3,vbBinaryCompare)`  
<span data-ttu-id="46ba9-556">Vyhodnotí too7</span><span class="sxs-lookup"><span data-stu-id="46ba9-556">Evaluates too7</span></span>

- - -
### <a name="instrrev"></a><span data-ttu-id="46ba9-557">InStrRev</span><span class="sxs-lookup"><span data-stu-id="46ba9-557">InStrRev</span></span>
<span data-ttu-id="46ba9-558">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-558">**Description:**</span></span>  
<span data-ttu-id="46ba9-559">Hello funkce InStrRev hello posledního výskytu dílčí řetězec najde v řetězci</span><span class="sxs-lookup"><span data-stu-id="46ba9-559">hello InStrRev function finds hello last occurrence of a substring in a string</span></span>

<span data-ttu-id="46ba9-560">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-560">**Syntax:**</span></span>  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* <span data-ttu-id="46ba9-561">prohledávaný_řetězec: řetězec toobe vyhledávat</span><span class="sxs-lookup"><span data-stu-id="46ba9-561">stringcheck: string toobe searched</span></span>
* <span data-ttu-id="46ba9-562">hledaný_řetězec: řetězec toobe nalezen</span><span class="sxs-lookup"><span data-stu-id="46ba9-562">stringmatch: string toobe found</span></span>
* <span data-ttu-id="46ba9-563">spustit: počáteční pozice toofind hello substring</span><span class="sxs-lookup"><span data-stu-id="46ba9-563">start: starting position toofind hello substring</span></span>
* <span data-ttu-id="46ba9-564">porovnání: vbTextCompare nebo vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="46ba9-564">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="46ba9-565">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-565">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-566">Vrátí pozici hello kde nebyl nalezen dílčí řetězec hello nebo 0, pokud nebyl nalezen.</span><span class="sxs-lookup"><span data-stu-id="46ba9-566">Returns hello position where hello substring was found or 0 if not found.</span></span>

<span data-ttu-id="46ba9-567">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-567">**Example:**</span></span>  
`InStrRev("abbcdbbbef","bb")`  
<span data-ttu-id="46ba9-568">Vrátí hodnotu 7</span><span class="sxs-lookup"><span data-stu-id="46ba9-568">Returns 7</span></span>

- - -
### <a name="isbitset"></a><span data-ttu-id="46ba9-569">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="46ba9-569">IsBitSet</span></span>
<span data-ttu-id="46ba9-570">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-570">**Description:**</span></span>  
<span data-ttu-id="46ba9-571">Funkce Hello IsBitSet testů, pokud je trochu nastavená nebo ne</span><span class="sxs-lookup"><span data-stu-id="46ba9-571">hello function IsBitSet Tests if a bit is set or not</span></span>

<span data-ttu-id="46ba9-572">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-572">**Syntax:**</span></span>  
`bool IsBitSet(num value, num flag)`

* <span data-ttu-id="46ba9-573">hodnota: číselnou hodnotu, která je evaluated.flag: číselnou hodnotu, která má hello bit toobe vyhodnocení</span><span class="sxs-lookup"><span data-stu-id="46ba9-573">value: a numeric value that is evaluated.flag: a numeric value that has hello bit toobe evaluated</span></span>

<span data-ttu-id="46ba9-574">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-574">**Example:**</span></span>  
`IsBitSet(&HF,4)`  
<span data-ttu-id="46ba9-575">Vrátí hodnotu True, protože je nastaven bit "4" v šestnáctkové hodnoty hello "F"</span><span class="sxs-lookup"><span data-stu-id="46ba9-575">Returns True because bit "4" is set in hello hexadecimal value "F"</span></span>

- - -
### <a name="isdate"></a><span data-ttu-id="46ba9-576">IsDate</span><span class="sxs-lookup"><span data-stu-id="46ba9-576">IsDate</span></span>
<span data-ttu-id="46ba9-577">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-577">**Description:**</span></span>  
<span data-ttu-id="46ba9-578">Pokud hello výraz může být vyhodnotí jako typ DateTime, pak hello Funkce IsDate vyhodnotí tooTrue.</span><span class="sxs-lookup"><span data-stu-id="46ba9-578">If hello expression can be evaluates as a DateTime type, then hello IsDate function evaluates tooTrue.</span></span>

<span data-ttu-id="46ba9-579">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-579">**Syntax:**</span></span>  
`bool IsDate(var Expression)`

<span data-ttu-id="46ba9-580">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-580">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-581">Toodetermine použít, pokud CDate() může být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="46ba9-581">Used toodetermine if CDate() can be successful.</span></span>

- - -
### <a name="iscert"></a><span data-ttu-id="46ba9-582">IsCert</span><span class="sxs-lookup"><span data-stu-id="46ba9-582">IsCert</span></span>
<span data-ttu-id="46ba9-583">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-583">**Description:**</span></span>  
<span data-ttu-id="46ba9-584">Vrátí hodnotu true Pokud serializovat lze hello nezpracovaná data do objektu certifikát .NET X509Certificate2.</span><span class="sxs-lookup"><span data-stu-id="46ba9-584">Returns true if hello raw data can be serialized into .NET X509Certificate2 certificate object.</span></span>

<span data-ttu-id="46ba9-585">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-585">**Syntax:**</span></span>  
`bool CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="46ba9-586">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-586">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="46ba9-587">Hello bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="46ba9-587">hello byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
- - -
### <a name="isempty"></a><span data-ttu-id="46ba9-588">IsEmpty –</span><span class="sxs-lookup"><span data-stu-id="46ba9-588">IsEmpty</span></span>
<span data-ttu-id="46ba9-589">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-589">**Description:**</span></span>  
<span data-ttu-id="46ba9-590">Pokud atribut hello je k dispozici v hello CS nebo více hodnot, ale vyhodnotí tooan prázdný řetězec, vyhodnotí hello IsEmpty – funkce tooTrue.</span><span class="sxs-lookup"><span data-stu-id="46ba9-590">If hello attribute is present in hello CS or MV but evaluates tooan empty string, then hello IsEmpty function evaluates tooTrue.</span></span>

<span data-ttu-id="46ba9-591">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-591">**Syntax:**</span></span>  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a><span data-ttu-id="46ba9-592">IsGuid</span><span class="sxs-lookup"><span data-stu-id="46ba9-592">IsGuid</span></span>
<span data-ttu-id="46ba9-593">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-593">**Description:**</span></span>  
<span data-ttu-id="46ba9-594">Pokud řetězec hello může být převedená tooa identifikátor GUID, funkce IsGuid hello vyhodnotit tootrue.</span><span class="sxs-lookup"><span data-stu-id="46ba9-594">If hello string could be converted tooa GUID, then hello IsGuid function evaluated tootrue.</span></span>

<span data-ttu-id="46ba9-595">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-595">**Syntax:**</span></span>  
`bool IsGuid(str GUID)`

<span data-ttu-id="46ba9-596">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-596">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-597">Identifikátor GUID je definován jako jeden z těchto vzorů následující řetězec: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="46ba9-597">A GUID is defined as a string following one of these patterns: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

<span data-ttu-id="46ba9-598">Toodetermine použít, pokud CGuid() může být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="46ba9-598">Used toodetermine if CGuid() can be successful.</span></span>

<span data-ttu-id="46ba9-599">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-599">**Example:**</span></span>  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
<span data-ttu-id="46ba9-600">Pokud hello StrAttribute má formát identifikátoru GUID, vrátit binární reprezentace, jinak vrátí hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="46ba9-600">If hello StrAttribute has a GUID format, return a binary representation, otherwise return a Null.</span></span>

- - -
### <a name="isnull"></a><span data-ttu-id="46ba9-601">IsNull</span><span class="sxs-lookup"><span data-stu-id="46ba9-601">IsNull</span></span>
<span data-ttu-id="46ba9-602">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-602">**Description:**</span></span>  
<span data-ttu-id="46ba9-603">Pokud hello výraz vyhodnocen jako tooNull, hello IsNull funkce vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="46ba9-603">If hello expression evaluates tooNull, then hello IsNull function returns true.</span></span>

<span data-ttu-id="46ba9-604">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-604">**Syntax:**</span></span>  
`bool IsNull(var Expression)`

<span data-ttu-id="46ba9-605">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-605">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-606">Pro atribut s hodnotou Null se vyjádří hello absenci hello atributu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-606">For an attribute, a Null is expressed by hello absence of hello attribute.</span></span>

<span data-ttu-id="46ba9-607">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-607">**Example:**</span></span>  
`IsNull([displayName])`  
<span data-ttu-id="46ba9-608">Vrátí hodnotu True, pokud není k dispozici v hello CS nebo MV hello atribut.</span><span class="sxs-lookup"><span data-stu-id="46ba9-608">Returns True if hello attribute is not present in hello CS or MV.</span></span>

- - -
### <a name="isnullorempty"></a><span data-ttu-id="46ba9-609">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="46ba9-609">IsNullOrEmpty</span></span>
<span data-ttu-id="46ba9-610">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-610">**Description:**</span></span>  
<span data-ttu-id="46ba9-611">Pokud hello výraz má hodnotu null nebo prázdný řetězec, hello IsNullOrEmpty funkce vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="46ba9-611">If hello expression is null or an empty string, then hello IsNullOrEmpty function returns true.</span></span>

<span data-ttu-id="46ba9-612">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-612">**Syntax:**</span></span>  
`bool IsNullOrEmpty(var Expression)`

<span data-ttu-id="46ba9-613">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-613">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-614">Pro atribut to by vyhodnocovaly tooTrue Pokud atribut hello chybí nebo je k dispozici, ale prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-614">For an attribute, this would evaluate tooTrue if hello attribute is absent or is present but is an empty string.</span></span>  
<span data-ttu-id="46ba9-615">inverzní Hello této funkce je s názvem IsPresent.</span><span class="sxs-lookup"><span data-stu-id="46ba9-615">hello inverse of this function is named IsPresent.</span></span>

<span data-ttu-id="46ba9-616">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-616">**Example:**</span></span>  
`IsNullOrEmpty([displayName])`  
<span data-ttu-id="46ba9-617">Vrátí hodnotu True, pokud není přítomen nebo je prázdný řetězec v hello CS nebo MV hello atributu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-617">Returns True if hello attribute is not present or is an empty string in hello CS or MV.</span></span>

- - -
### <a name="isnumeric"></a><span data-ttu-id="46ba9-618">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="46ba9-618">IsNumeric</span></span>
<span data-ttu-id="46ba9-619">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-619">**Description:**</span></span>  
<span data-ttu-id="46ba9-620">Hello Funkce IsNumeric vrací logickou hodnotu udávající, zda výraz může být vyhodnocen jako typ number.</span><span class="sxs-lookup"><span data-stu-id="46ba9-620">hello IsNumeric function returns a Boolean value indicating whether an expression can be evaluated as a number type.</span></span>

<span data-ttu-id="46ba9-621">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-621">**Syntax:**</span></span>  
`bool IsNumeric(var Expression)`

<span data-ttu-id="46ba9-622">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-622">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-623">Toodetermine použít, pokud CNum() může být úspěšné tooparse hello výraz.</span><span class="sxs-lookup"><span data-stu-id="46ba9-623">Used toodetermine if CNum() can be successful tooparse hello expression.</span></span>

- - -
### <a name="isstring"></a><span data-ttu-id="46ba9-624">IsString</span><span class="sxs-lookup"><span data-stu-id="46ba9-624">IsString</span></span>
<span data-ttu-id="46ba9-625">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-625">**Description:**</span></span>  
<span data-ttu-id="46ba9-626">Pokud hello výraz může být vyhodnotí tooa řetězec typu, potom funkce IsString hello vyhodnotí tooTrue.</span><span class="sxs-lookup"><span data-stu-id="46ba9-626">If hello expression can be evaluated tooa string type, then hello IsString function evaluates tooTrue.</span></span>

<span data-ttu-id="46ba9-627">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-627">**Syntax:**</span></span>  
`bool IsString(var expression)`

<span data-ttu-id="46ba9-628">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-628">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-629">Toodetermine použít, pokud CStr() může být úspěšné tooparse hello výraz.</span><span class="sxs-lookup"><span data-stu-id="46ba9-629">Used toodetermine if CStr() can be successful tooparse hello expression.</span></span>

- - -
### <a name="ispresent"></a><span data-ttu-id="46ba9-630">IsPresent</span><span class="sxs-lookup"><span data-stu-id="46ba9-630">IsPresent</span></span>
<span data-ttu-id="46ba9-631">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-631">**Description:**</span></span>  
<span data-ttu-id="46ba9-632">Pokud je výsledkem výrazu hello tooa řetězec, který není Null a není prázdná, pak hello IsPresent funkce vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="46ba9-632">If hello expression evaluates tooa string that is not Null and is not empty, then hello IsPresent function returns true.</span></span>

<span data-ttu-id="46ba9-633">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-633">**Syntax:**</span></span>  
`bool IsPresent(var expression)`

<span data-ttu-id="46ba9-634">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-634">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-635">inverzní Hello této funkce je s názvem IsNullOrEmpty.</span><span class="sxs-lookup"><span data-stu-id="46ba9-635">hello inverse of this function is named IsNullOrEmpty.</span></span>

<span data-ttu-id="46ba9-636">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-636">**Example:**</span></span>  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a><span data-ttu-id="46ba9-637">Položka</span><span class="sxs-lookup"><span data-stu-id="46ba9-637">Item</span></span>
<span data-ttu-id="46ba9-638">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-638">**Description:**</span></span>  
<span data-ttu-id="46ba9-639">Hello položky funkce vrátí jednu položku z více hodnot řetězec nebo atributu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-639">hello Item function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="46ba9-640">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-640">**Syntax:**</span></span>  
`var Item(mvstr attribute, num index)`

* <span data-ttu-id="46ba9-641">Atribut: vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-641">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="46ba9-642">index: položky tooan indexu v řetězci hello více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="46ba9-642">index: index tooan item in hello multi-valued string.</span></span>

<span data-ttu-id="46ba9-643">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-643">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-644">Hello funkce Item je užitečné společně s hello obsahuje funkce, protože hello pozdější funkce vrátí hello index tooan položku hello více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="46ba9-644">hello Item function is useful together with hello Contains function since hello latter function returns hello index tooan item in hello multi-valued attribute.</span></span>

<span data-ttu-id="46ba9-645">Vrátí chybu, pokud index je mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="46ba9-645">Throws an error if index is out of bounds.</span></span>

<span data-ttu-id="46ba9-646">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-646">**Example:**</span></span>  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
<span data-ttu-id="46ba9-647">Vrátí hello primární e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-647">Returns hello primary email address.</span></span>

- - -
### <a name="itemornull"></a><span data-ttu-id="46ba9-648">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="46ba9-648">ItemOrNull</span></span>
<span data-ttu-id="46ba9-649">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-649">**Description:**</span></span>  
<span data-ttu-id="46ba9-650">Hello ItemOrNull funkce vrátí jednu položku z více hodnot řetězec nebo atributu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-650">hello ItemOrNull function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="46ba9-651">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-651">**Syntax:**</span></span>  
`var ItemOrNull(mvstr attribute, num index)`

* <span data-ttu-id="46ba9-652">Atribut: vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-652">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="46ba9-653">index: položky tooan indexu v řetězci hello více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="46ba9-653">index: index tooan item in hello multi-valued string.</span></span>

<span data-ttu-id="46ba9-654">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-654">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-655">Hello ItemOrNull funkce je užitečné společně s hello obsahuje funkce, protože hello pozdější funkce vrátí hello index tooan položku hello více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="46ba9-655">hello ItemOrNull function is useful together with hello Contains function since hello latter function returns hello index tooan item in hello multi-valued attribute.</span></span>

<span data-ttu-id="46ba9-656">Pokud index je mimo rozsah, vrátí hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="46ba9-656">If index is out of bounds, then returns a Null value.</span></span>

- - -
### <a name="join"></a><span data-ttu-id="46ba9-657">Spojit</span><span class="sxs-lookup"><span data-stu-id="46ba9-657">Join</span></span>
<span data-ttu-id="46ba9-658">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-658">**Description:**</span></span>  
<span data-ttu-id="46ba9-659">Funkce spojení Hello přebírá řetězec s více hodnotami a vrátí řetězec s hodnotou jednoho zadaného oddělovače vložen mezi každou položku.</span><span class="sxs-lookup"><span data-stu-id="46ba9-659">hello Join function takes a multi-valued string and returns a single-valued string with specified separator inserted between each item.</span></span>

<span data-ttu-id="46ba9-660">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-660">**Syntax:**</span></span>  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* <span data-ttu-id="46ba9-661">Atribut: obsahující více hodnot atributů řetězce toobe připojený.</span><span class="sxs-lookup"><span data-stu-id="46ba9-661">attribute: Multi-valued attribute containing strings toobe joined.</span></span>
* <span data-ttu-id="46ba9-662">oddělovač: žádné řetězce, použité tooseparate hello dílčích řetězců v hello vrátil řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-662">delimiter: Any string, used tooseparate hello substrings in hello returned string.</span></span> <span data-ttu-id="46ba9-663">Pokud tento parametr vynechán, hello znak mezery ("") se používá.</span><span class="sxs-lookup"><span data-stu-id="46ba9-663">If omitted, hello space character (" ") is used.</span></span> <span data-ttu-id="46ba9-664">Pokud oddělovač obsahuje řetězec nulové délky ("") nebo Nothing, jsou všechny položky v seznamu hello zřetězených žádný oddělovač.</span><span class="sxs-lookup"><span data-stu-id="46ba9-664">If Delimiter is a zero-length string ("") or Nothing, all items in hello list are concatenated with no delimiters.</span></span>

<span data-ttu-id="46ba9-665">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="46ba9-665">**Remarks**</span></span>  
<span data-ttu-id="46ba9-666">Rozdíly mezi hello spojení a rozdělení funkce není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="46ba9-666">There is parity between hello Join and Split functions.</span></span> <span data-ttu-id="46ba9-667">Hello funkce spojení trvá pole řetězců a připojí pomocí oddělovačem, tooreturn jeden řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-667">hello Join function takes an array of strings and joins them using a delimiter string, tooreturn a single string.</span></span> <span data-ttu-id="46ba9-668">Hello funkce rozdělení přebírá řetězec a která ho odděluje v hello oddělovač, tooreturn pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="46ba9-668">hello Split function takes a string and separates it at hello delimiter, tooreturn an array of strings.</span></span> <span data-ttu-id="46ba9-669">Klíčovým rozdílem je ale, že připojení lze zřetězení řetězců s libovolným řetězcem oddělovač, rozdělení můžete oddělit pouze řetězců pomocí jednoho znaku oddělovač.</span><span class="sxs-lookup"><span data-stu-id="46ba9-669">However, a key difference is that Join can concatenate strings with any delimiter string, Split can only separate strings using a single character delimiter.</span></span>

<span data-ttu-id="46ba9-670">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-670">**Example:**</span></span>  
`Join([proxyAddresses],",")`  
<span data-ttu-id="46ba9-671">Může vrátit: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="46ba9-671">Could return: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span></span>

- - -
### <a name="lcase"></a><span data-ttu-id="46ba9-672">LCase</span><span class="sxs-lookup"><span data-stu-id="46ba9-672">LCase</span></span>
<span data-ttu-id="46ba9-673">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-673">**Description:**</span></span>  
<span data-ttu-id="46ba9-674">Hello funkce LCase převede všechny znaky v případě toolower řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-674">hello LCase function converts all characters in a string toolower case.</span></span>

<span data-ttu-id="46ba9-675">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-675">**Syntax:**</span></span>  
`str LCase(str value)`

<span data-ttu-id="46ba9-676">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-676">**Example:**</span></span>  
`LCase("TeSt")`  
<span data-ttu-id="46ba9-677">Vrátí hodnotu "test".</span><span class="sxs-lookup"><span data-stu-id="46ba9-677">Returns "test".</span></span>

- - -
### <a name="left"></a><span data-ttu-id="46ba9-678">Vlevo</span><span class="sxs-lookup"><span data-stu-id="46ba9-678">Left</span></span>
<span data-ttu-id="46ba9-679">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-679">**Description:**</span></span>  
<span data-ttu-id="46ba9-680">Left – Hello funkce vrátí zadaný počet znaků zleva hello řetězce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-680">hello Left function returns a specified number of characters from hello left of a string.</span></span>

<span data-ttu-id="46ba9-681">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-681">**Syntax:**</span></span>  
`str Left(str string, num NumChars)`

* <span data-ttu-id="46ba9-682">řetězec: hello tooreturn znaky řetězce z</span><span class="sxs-lookup"><span data-stu-id="46ba9-682">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="46ba9-683">NumChars: číslo určující počet hello tooreturn znaky z hello začátku (vlevo) řetězce</span><span class="sxs-lookup"><span data-stu-id="46ba9-683">NumChars: a number identifying hello number of characters tooreturn from hello beginning (left) of string</span></span>

<span data-ttu-id="46ba9-684">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-684">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-685">Řetězec obsahující hello první numChars znaků v řetězci:</span><span class="sxs-lookup"><span data-stu-id="46ba9-685">A string containing hello first numChars characters in string:</span></span>

* <span data-ttu-id="46ba9-686">Pokud numChars = 0, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-686">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="46ba9-687">Pokud numChars < 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-687">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="46ba9-688">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-688">If string is null, return empty string.</span></span>

<span data-ttu-id="46ba9-689">Pokud řetězec obsahuje méně znaků, než je zadáno v numChars hello číslo, vrátí se identické toostring řetězec, (který je, obsahující všechny znaky v parametru 1).</span><span class="sxs-lookup"><span data-stu-id="46ba9-689">If string contains fewer characters than hello number specified in numChars, a string identical toostring (that is, containing all characters in parameter 1) is returned.</span></span>

<span data-ttu-id="46ba9-690">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-690">**Example:**</span></span>  
`Left("John Doe", 3)`  
<span data-ttu-id="46ba9-691">Vrátí "Joh".</span><span class="sxs-lookup"><span data-stu-id="46ba9-691">Returns "Joh".</span></span>

- - -
### <a name="len"></a><span data-ttu-id="46ba9-692">Len</span><span class="sxs-lookup"><span data-stu-id="46ba9-692">Len</span></span>
<span data-ttu-id="46ba9-693">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-693">**Description:**</span></span>  
<span data-ttu-id="46ba9-694">Hello funkce Len vrátí počet znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="46ba9-694">hello Len function returns number of characters in a string.</span></span>

<span data-ttu-id="46ba9-695">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-695">**Syntax:**</span></span>  
`num Len(str value)`

<span data-ttu-id="46ba9-696">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-696">**Example:**</span></span>  
`Len("John Doe")`  
<span data-ttu-id="46ba9-697">Vrátí 8</span><span class="sxs-lookup"><span data-stu-id="46ba9-697">Returns 8</span></span>

- - -
### <a name="ltrim"></a><span data-ttu-id="46ba9-698">LTrim</span><span class="sxs-lookup"><span data-stu-id="46ba9-698">LTrim</span></span>
<span data-ttu-id="46ba9-699">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-699">**Description:**</span></span>  
<span data-ttu-id="46ba9-700">Hello funkce LTrim odebere úvodní prázdné znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="46ba9-700">hello LTrim function removes leading white spaces from a string.</span></span>

<span data-ttu-id="46ba9-701">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-701">**Syntax:**</span></span>  
`str LTrim(str value)`

<span data-ttu-id="46ba9-702">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-702">**Example:**</span></span>  
`LTrim(" Test ")`  
<span data-ttu-id="46ba9-703">Vrátí "Test"</span><span class="sxs-lookup"><span data-stu-id="46ba9-703">Returns "Test "</span></span>

- - -
### <a name="mid"></a><span data-ttu-id="46ba9-704">Mid –</span><span class="sxs-lookup"><span data-stu-id="46ba9-704">Mid</span></span>
<span data-ttu-id="46ba9-705">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-705">**Description:**</span></span>  
<span data-ttu-id="46ba9-706">Hello Mid funkce vrací zadaný počet znaků od zadané pozici v řetězci.</span><span class="sxs-lookup"><span data-stu-id="46ba9-706">hello Mid function returns a specified number of characters from a specified position in a string.</span></span>

<span data-ttu-id="46ba9-707">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-707">**Syntax:**</span></span>  
`str Mid(str string, num start, num NumChars)`

* <span data-ttu-id="46ba9-708">řetězec: hello tooreturn znaky řetězce z</span><span class="sxs-lookup"><span data-stu-id="46ba9-708">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="46ba9-709">spustit: číslo určující počáteční pozici v tooreturn znaky řetězce z hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-709">start: a number identifying hello starting position in string tooreturn characters from</span></span>
* <span data-ttu-id="46ba9-710">NumChars: číslo určující počet hello tooreturn znaků z pozice v řetězci</span><span class="sxs-lookup"><span data-stu-id="46ba9-710">NumChars: a number identifying hello number of characters tooreturn from position in string</span></span>

<span data-ttu-id="46ba9-711">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-711">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-712">Vrátí numChars znaků od počáteční pozice v řetězci.</span><span class="sxs-lookup"><span data-stu-id="46ba9-712">Return numChars characters starting from position start in string.</span></span>  
<span data-ttu-id="46ba9-713">Řetězec obsahující numChars znaků od počáteční pozice v řetězci:</span><span class="sxs-lookup"><span data-stu-id="46ba9-713">A string containing numChars characters from position start in string:</span></span>

* <span data-ttu-id="46ba9-714">Pokud numChars = 0, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-714">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="46ba9-715">Pokud numChars < 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-715">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="46ba9-716">Pokud spuštění > Dobrý den délky řetězce, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-716">If start > hello length of string, return input string.</span></span>
* <span data-ttu-id="46ba9-717">Pokud spuštění < = 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-717">If start <= 0, return input string.</span></span>
* <span data-ttu-id="46ba9-718">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-718">If string is null, return empty string.</span></span>

<span data-ttu-id="46ba9-719">Pokud nejsou k dispozici numChar znaků zbývající v řetězci od počáteční pozice, jako jsou vráceny nejdříve mnoho znaků.</span><span class="sxs-lookup"><span data-stu-id="46ba9-719">If there are not numChar characters remaining in string from position start, as many characters as possible are returned.</span></span>

<span data-ttu-id="46ba9-720">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-720">**Example:**</span></span>  
`Mid("John Doe", 3, 5)`  
<span data-ttu-id="46ba9-721">Vrátí "hn proveďte".</span><span class="sxs-lookup"><span data-stu-id="46ba9-721">Returns "hn Do".</span></span>

`Mid("John Doe", 6, 999)`  
<span data-ttu-id="46ba9-722">Vrátí "Doe"</span><span class="sxs-lookup"><span data-stu-id="46ba9-722">Returns "Doe"</span></span>

- - -
### <a name="now"></a><span data-ttu-id="46ba9-723">Nyní</span><span class="sxs-lookup"><span data-stu-id="46ba9-723">Now</span></span>
<span data-ttu-id="46ba9-724">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-724">**Description:**</span></span>  
<span data-ttu-id="46ba9-725">Hello nyní funkce vrací hodnotu DateTime zadání hello aktuální datum a čas, podle tooyour počítače systémové datum a čas.</span><span class="sxs-lookup"><span data-stu-id="46ba9-725">hello Now function returns a DateTime specifying hello current date and time, according tooyour computer's system date and time.</span></span>

<span data-ttu-id="46ba9-726">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-726">**Syntax:**</span></span>  
`dt Now()`

- - -
### <a name="numfromdate"></a><span data-ttu-id="46ba9-727">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="46ba9-727">NumFromDate</span></span>
<span data-ttu-id="46ba9-728">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-728">**Description:**</span></span>  
<span data-ttu-id="46ba9-729">Hello NumFromDate funkce vrátí datum ve formátu data na AD.</span><span class="sxs-lookup"><span data-stu-id="46ba9-729">hello NumFromDate function returns a date in AD’s date format.</span></span>

<span data-ttu-id="46ba9-730">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-730">**Syntax:**</span></span>  
`num NumFromDate(dt value)`

<span data-ttu-id="46ba9-731">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-731">**Example:**</span></span>  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
<span data-ttu-id="46ba9-732">Vrátí 129699324000000000</span><span class="sxs-lookup"><span data-stu-id="46ba9-732">Returns 129699324000000000</span></span>

- - -
### <a name="padleft"></a><span data-ttu-id="46ba9-733">padLeft</span><span class="sxs-lookup"><span data-stu-id="46ba9-733">PadLeft</span></span>
<span data-ttu-id="46ba9-734">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-734">**Description:**</span></span>  
<span data-ttu-id="46ba9-735">Hello PadLeft funkce doleva-dotyková zařízení řetězec tooa Zadaná délka pomocí zadané odsazení znaku.</span><span class="sxs-lookup"><span data-stu-id="46ba9-735">hello PadLeft function left-pads a string tooa specified length using a provided padding character.</span></span>

<span data-ttu-id="46ba9-736">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-736">**Syntax:**</span></span>  
`str PadLeft(str string, num length, str padCharacter)`

* <span data-ttu-id="46ba9-737">řetězec: hello toopad řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-737">string: hello string toopad.</span></span>
* <span data-ttu-id="46ba9-738">Délka: celé číslo představující hello potřeby délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-738">length: An integer representing hello desired length of string.</span></span>
* <span data-ttu-id="46ba9-739">padCharacter: řetězec, který se skládá z jednoho znaku toouse jako znak odsazení hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-739">padCharacter: A string consisting of a single character toouse as hello pad character</span></span>

<span data-ttu-id="46ba9-740">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-740">**Remarks:**</span></span>

* <span data-ttu-id="46ba9-741">Pokud hello délka řetězce je menší než délka, pak padCharacter je opakovaně připojením toohello začátku (vlevo) řetězce, dokud má stejné toolength délka.</span><span class="sxs-lookup"><span data-stu-id="46ba9-741">If hello length of string is less than length, then padCharacter is repeatedly appended toohello beginning (left) of string until it has a length equal toolength.</span></span>
* <span data-ttu-id="46ba9-742">padCharacter může být znak mezery, ale jeho nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="46ba9-742">PadCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="46ba9-743">Pokud hello délka řetězce je rovna tooor větší než délka, je vrácen řetězec s beze změny.</span><span class="sxs-lookup"><span data-stu-id="46ba9-743">If hello length of string is equal tooor greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="46ba9-744">Pokud řetězec má délku větší než nebo rovna toolength, je vrácena identické toostring řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-744">If string has a length greater than or equal toolength, a string identical toostring is returned.</span></span>
* <span data-ttu-id="46ba9-745">Pokud hello délka řetězce je menší než délka, nové řetězec hello potřeby délka je vrácena obsahující odsazenou s padCharacter řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-745">If hello length of string is less than length, then a new string of hello desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="46ba9-746">Pokud má řetězec hodnotu null, funkce hello vrací prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-746">If string is null, hello function returns an empty string.</span></span>

<span data-ttu-id="46ba9-747">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-747">**Example:**</span></span>  
`PadLeft("User", 10, "0")`  
<span data-ttu-id="46ba9-748">Vrátí "000000User".</span><span class="sxs-lookup"><span data-stu-id="46ba9-748">Returns "000000User".</span></span>

- - -
### <a name="padright"></a><span data-ttu-id="46ba9-749">PadRight –</span><span class="sxs-lookup"><span data-stu-id="46ba9-749">PadRight</span></span>
<span data-ttu-id="46ba9-750">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-750">**Description:**</span></span>  
<span data-ttu-id="46ba9-751">Hello PadRight – funkce vpravo-dotyková zařízení řetězec tooa Zadaná délka pomocí zadané odsazení znaku.</span><span class="sxs-lookup"><span data-stu-id="46ba9-751">hello PadRight function right-pads a string tooa specified length using a provided padding character.</span></span>

<span data-ttu-id="46ba9-752">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-752">**Syntax:**</span></span>  
`str PadRight(str string, num length, str padCharacter)`

* <span data-ttu-id="46ba9-753">řetězec: hello toopad řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-753">string: hello string toopad.</span></span>
* <span data-ttu-id="46ba9-754">Délka: celé číslo představující hello potřeby délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-754">length: An integer representing hello desired length of string.</span></span>
* <span data-ttu-id="46ba9-755">padCharacter: řetězec, který se skládá z jednoho znaku toouse jako znak odsazení hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-755">padCharacter: A string consisting of a single character toouse as hello pad character</span></span>

<span data-ttu-id="46ba9-756">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-756">**Remarks:**</span></span>

* <span data-ttu-id="46ba9-757">Pokud hello délka řetězce je menší než délka, pak padCharacter je opakovaně připojením toohello konci (vpravo) řetězec, dokud má stejné toolength délka.</span><span class="sxs-lookup"><span data-stu-id="46ba9-757">If hello length of string is less than length, then padCharacter is repeatedly appended toohello end (right) of string until it has a length equal toolength.</span></span>
* <span data-ttu-id="46ba9-758">padCharacter může být znak mezery, ale jeho nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="46ba9-758">padCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="46ba9-759">Pokud hello délka řetězce je rovna tooor větší než délka, je vrácen řetězec s beze změny.</span><span class="sxs-lookup"><span data-stu-id="46ba9-759">If hello length of string is equal tooor greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="46ba9-760">Pokud řetězec má délku větší než nebo rovna toolength, je vrácena identické toostring řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-760">If string has a length greater than or equal toolength, a string identical toostring is returned.</span></span>
* <span data-ttu-id="46ba9-761">Pokud hello délka řetězce je menší než délka, nové řetězec hello potřeby délka je vrácena obsahující odsazenou s padCharacter řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-761">If hello length of string is less than length, then a new string of hello desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="46ba9-762">Pokud má řetězec hodnotu null, funkce hello vrací prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-762">If string is null, hello function returns an empty string.</span></span>

<span data-ttu-id="46ba9-763">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-763">**Example:**</span></span>  
`PadRight("User", 10, "0")`  
<span data-ttu-id="46ba9-764">Vrátí "User000000".</span><span class="sxs-lookup"><span data-stu-id="46ba9-764">Returns "User000000".</span></span>

- - -
### <a name="pcase"></a><span data-ttu-id="46ba9-765">PCase</span><span class="sxs-lookup"><span data-stu-id="46ba9-765">PCase</span></span>
<span data-ttu-id="46ba9-766">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-766">**Description:**</span></span>  
<span data-ttu-id="46ba9-767">Převede hello první znak každého slova mezerami v případě tooupper řetězec Hello PCase funkce a všechny ostatní znaky jsou převedeny toolower případu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-767">hello PCase function converts hello first character of each space delimited word in a string tooupper case, and all other characters are converted toolower case.</span></span>

<span data-ttu-id="46ba9-768">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-768">**Syntax:**</span></span>  
`String PCase(string)`

<span data-ttu-id="46ba9-769">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-769">**Remarks:**</span></span>

* <span data-ttu-id="46ba9-770">Tato funkce nenabízí aktuálně tooconvert správné velká a malá písmena slovo, které je zcela velká, jako je například zkratka.</span><span class="sxs-lookup"><span data-stu-id="46ba9-770">This function does not currently provide proper casing tooconvert a word that is entirely uppercase, such as an acronym.</span></span>

<span data-ttu-id="46ba9-771">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-771">**Example:**</span></span>  
`PCase("TEsT")`  
<span data-ttu-id="46ba9-772">Vrátí hodnotu "Test".</span><span class="sxs-lookup"><span data-stu-id="46ba9-772">Returns "Test".</span></span>

`PCase(LCase("TEST"))`  
<span data-ttu-id="46ba9-773">Vrátí "Test"</span><span class="sxs-lookup"><span data-stu-id="46ba9-773">Returns "Test"</span></span>

- - -
### <a name="randomnum"></a><span data-ttu-id="46ba9-774">RandomNum</span><span class="sxs-lookup"><span data-stu-id="46ba9-774">RandomNum</span></span>
<span data-ttu-id="46ba9-775">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-775">**Description:**</span></span>  
<span data-ttu-id="46ba9-776">Hello RandomNum funkce vrátí náhodné číslo o zadaný interval.</span><span class="sxs-lookup"><span data-stu-id="46ba9-776">hello RandomNum function returns a random number between a specified interval.</span></span>

<span data-ttu-id="46ba9-777">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-777">**Syntax:**</span></span>  
`num RandomNum(num start, num end)`

* <span data-ttu-id="46ba9-778">spustit: číslo identifikující hello nižší limit toogenerate náhodná hodnota hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-778">start: a number identifying hello lower limit of hello random value toogenerate</span></span>
* <span data-ttu-id="46ba9-779">end: číslo identifikující hello horní limit toogenerate náhodná hodnota hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-779">end: a number identifying hello upper limit of hello random value toogenerate</span></span>

<span data-ttu-id="46ba9-780">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-780">**Example:**</span></span>  
`Random(100,999)`  
<span data-ttu-id="46ba9-781">734 může vrátit.</span><span class="sxs-lookup"><span data-stu-id="46ba9-781">Can return 734.</span></span>

- - -
### <a name="removeduplicates"></a><span data-ttu-id="46ba9-782">Removeduplicates –</span><span class="sxs-lookup"><span data-stu-id="46ba9-782">RemoveDuplicates</span></span>
<span data-ttu-id="46ba9-783">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-783">**Description:**</span></span>  
<span data-ttu-id="46ba9-784">Hello removeduplicates – funkce přebírá řetězec s více hodnotami a ujistěte se, že každá hodnota je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="46ba9-784">hello RemoveDuplicates function takes a multi-valued string and make sure each value is unique.</span></span>

<span data-ttu-id="46ba9-785">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-785">**Syntax:**</span></span>  
`mvstr RemoveDuplicates(mvstr attribute)`

<span data-ttu-id="46ba9-786">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-786">**Example:**</span></span>  
`RemoveDuplicates([proxyAddresses])`  
<span data-ttu-id="46ba9-787">Vrátí atribut upravený proxyAddress, kde byly odebrány všechny duplicitní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="46ba9-787">Returns a sanitized proxyAddress attribute where all duplicate values have been removed.</span></span>

- - -
### <a name="replace"></a><span data-ttu-id="46ba9-788">Nahradit</span><span class="sxs-lookup"><span data-stu-id="46ba9-788">Replace</span></span>
<span data-ttu-id="46ba9-789">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-789">**Description:**</span></span>  
<span data-ttu-id="46ba9-790">Hello funkce Nahradit nahradí všechny výskyty řetězce tooanother řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-790">hello Replace function replaces all occurrences of a string tooanother string.</span></span>

<span data-ttu-id="46ba9-791">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-791">**Syntax:**</span></span>  
`str Replace(str string, str OldValue, str NewValue)`

* <span data-ttu-id="46ba9-792">řetězec: řetězec tooreplace hodnoty v.</span><span class="sxs-lookup"><span data-stu-id="46ba9-792">string: A string tooreplace values in.</span></span>
* <span data-ttu-id="46ba9-793">OldValue: hello řetězec toosearch pro a tooreplace.</span><span class="sxs-lookup"><span data-stu-id="46ba9-793">OldValue: hello string toosearch for and tooreplace.</span></span>
* <span data-ttu-id="46ba9-794">NewValue: tooreplace řetězec hello k.</span><span class="sxs-lookup"><span data-stu-id="46ba9-794">NewValue: hello string tooreplace to.</span></span>

<span data-ttu-id="46ba9-795">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-795">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-796">Funkce Hello rozpozná hello následující zvláštní monikery:</span><span class="sxs-lookup"><span data-stu-id="46ba9-796">hello function recognizes hello following special monikers:</span></span>

* <span data-ttu-id="46ba9-797">\n – nový řádek</span><span class="sxs-lookup"><span data-stu-id="46ba9-797">\n – New Line</span></span>
* <span data-ttu-id="46ba9-798">\r – znaky CR</span><span class="sxs-lookup"><span data-stu-id="46ba9-798">\r – Carriage Return</span></span>
* <span data-ttu-id="46ba9-799">\t – karta</span><span class="sxs-lookup"><span data-stu-id="46ba9-799">\t – Tab</span></span>

<span data-ttu-id="46ba9-800">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-800">**Example:**</span></span>  
`Replace([address],"\r\n",", ")`  
<span data-ttu-id="46ba9-801">Nahradí Line FEED čárkami a místo a to by mohlo vést příliš "Jeden Microsoft způsob, Redmond, WA, USA"</span><span class="sxs-lookup"><span data-stu-id="46ba9-801">Replaces CRLF with a comma and space, and could lead too"One Microsoft Way, Redmond, WA, USA"</span></span>

- - -
### <a name="replacechars"></a><span data-ttu-id="46ba9-802">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="46ba9-802">ReplaceChars</span></span>
<span data-ttu-id="46ba9-803">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-803">**Description:**</span></span>  
<span data-ttu-id="46ba9-804">Hello ReplaceChars funkce nahradí všechny výskyty znaků v hello ReplacePattern řetězec nalezen.</span><span class="sxs-lookup"><span data-stu-id="46ba9-804">hello ReplaceChars function replaces all occurrences of characters found in hello ReplacePattern string.</span></span>

<span data-ttu-id="46ba9-805">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-805">**Syntax:**</span></span>  
`str ReplaceChars(str string, str ReplacePattern)`

* <span data-ttu-id="46ba9-806">řetězec: tooreplace řetězec znaků.</span><span class="sxs-lookup"><span data-stu-id="46ba9-806">string: A string tooreplace characters in.</span></span>
* <span data-ttu-id="46ba9-807">ReplacePattern: řetězec obsahující slovník s tooreplace znaků.</span><span class="sxs-lookup"><span data-stu-id="46ba9-807">ReplacePattern: a string containing a dictionary with characters tooreplace.</span></span>

<span data-ttu-id="46ba9-808">Formát Hello je {zdroj1}: {target1}, {zdroj2}: {target2}, {zdrojN}, {targetN} Pokud je zdrojem hello znak toofind a cíle hello řetězec tooreplace s.</span><span class="sxs-lookup"><span data-stu-id="46ba9-808">hello format is {source1}:{target1},{source2}:{target2},{sourceN},{targetN} where source is hello character toofind and target hello string tooreplace with.</span></span>

<span data-ttu-id="46ba9-809">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-809">**Remarks:**</span></span>

* <span data-ttu-id="46ba9-810">Funkce Hello použije všechny výskyty definované zdroje a nahradí hello cíle.</span><span class="sxs-lookup"><span data-stu-id="46ba9-810">hello function takes each occurrence of defined sources and replaces them with hello targets.</span></span>
* <span data-ttu-id="46ba9-811">Zdroj Hello musí mít přesně jeden znak (unicode).</span><span class="sxs-lookup"><span data-stu-id="46ba9-811">hello source must be exactly one (unicode) character.</span></span>
* <span data-ttu-id="46ba9-812">Hello zdroj nesmí být prázdný nebo delší než jeden znak (Chyba analýzy).</span><span class="sxs-lookup"><span data-stu-id="46ba9-812">hello source cannot be empty or longer than one character (parsing error).</span></span>
* <span data-ttu-id="46ba9-813">cíl Hello může mít více znaků, například ö:oe, β:ss.</span><span class="sxs-lookup"><span data-stu-id="46ba9-813">hello target can have multiple characters, for example ö:oe, β:ss.</span></span>
* <span data-ttu-id="46ba9-814">cíl Hello může být prázdný, která určuje, že má být odebrána hello znak.</span><span class="sxs-lookup"><span data-stu-id="46ba9-814">hello target can be empty indicating that hello character should be removed.</span></span>
* <span data-ttu-id="46ba9-815">Zdroj Hello je malá a velká písmena a musí být přesně shodovat.</span><span class="sxs-lookup"><span data-stu-id="46ba9-815">hello source is case-sensitive and must be an exact match.</span></span>
* <span data-ttu-id="46ba9-816">Hello, (čárkou) a: (dvojtečka) jsou vyhrazené znaky a nelze jej nahradit pomocí této funkce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-816">hello , (comma) and : (colon) are reserved characters and cannot be replaced using this function.</span></span>
* <span data-ttu-id="46ba9-817">Mezery a další bílé znaky v řetězci ReplacePattern hello se ignorují.</span><span class="sxs-lookup"><span data-stu-id="46ba9-817">Spaces and other white characters in hello ReplacePattern string are ignored.</span></span>

<span data-ttu-id="46ba9-818">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-818">**Example:**</span></span>  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
<span data-ttu-id="46ba9-819">Vrátí Raksmorgas</span><span class="sxs-lookup"><span data-stu-id="46ba9-819">Returns Raksmorgas</span></span>

`ReplaceChars("O’Neil",%ReplaceString%)`  
<span data-ttu-id="46ba9-820">Vrací "ONeil", jeden značek hello je definovaný toobe odebrat.</span><span class="sxs-lookup"><span data-stu-id="46ba9-820">Returns "ONeil", hello single tick is defined toobe removed.</span></span>

- - -
### <a name="right"></a><span data-ttu-id="46ba9-821">Vpravo</span><span class="sxs-lookup"><span data-stu-id="46ba9-821">Right</span></span>
<span data-ttu-id="46ba9-822">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-822">**Description:**</span></span>  
<span data-ttu-id="46ba9-823">Dobrý den správné funkce vrátí zadaný počet znaků z hello vpravo (koncového) řetězce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-823">hello Right function returns a specified number of characters from hello right (end) of a string.</span></span>

<span data-ttu-id="46ba9-824">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-824">**Syntax:**</span></span>  
`str Right(str string, num NumChars)`

* <span data-ttu-id="46ba9-825">řetězec: hello tooreturn znaky řetězce z</span><span class="sxs-lookup"><span data-stu-id="46ba9-825">string: hello string tooreturn characters from</span></span>
* <span data-ttu-id="46ba9-826">NumChars: číslo určující počet hello tooreturn znaky z hello konci (vpravo) řetězce</span><span class="sxs-lookup"><span data-stu-id="46ba9-826">NumChars: a number identifying hello number of characters tooreturn from hello end (right) of string</span></span>

<span data-ttu-id="46ba9-827">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-827">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-828">Od poslední pozice hello řetězce jsou vráceny NumChars znaky.</span><span class="sxs-lookup"><span data-stu-id="46ba9-828">NumChars characters are returned from hello last position of string.</span></span>

<span data-ttu-id="46ba9-829">Řetězec obsahující hello poslední numChars znaků v řetězci:</span><span class="sxs-lookup"><span data-stu-id="46ba9-829">A string containing hello last numChars characters in string:</span></span>

* <span data-ttu-id="46ba9-830">Pokud numChars = 0, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-830">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="46ba9-831">Pokud numChars < 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-831">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="46ba9-832">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-832">If string is null, return empty string.</span></span>

<span data-ttu-id="46ba9-833">Pokud řetězec obsahuje menší počet znaků, než číslo zadané v NumChars text hello, vrátí se identické toostring řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-833">If string contains fewer characters than hello number specified in NumChars, a string identical toostring is returned.</span></span>

<span data-ttu-id="46ba9-834">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-834">**Example:**</span></span>  
`Right("John Doe", 3)`  
<span data-ttu-id="46ba9-835">Vrátí "Doe".</span><span class="sxs-lookup"><span data-stu-id="46ba9-835">Returns "Doe".</span></span>

- - -
### <a name="rtrim"></a><span data-ttu-id="46ba9-836">RTrim</span><span class="sxs-lookup"><span data-stu-id="46ba9-836">RTrim</span></span>
<span data-ttu-id="46ba9-837">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-837">**Description:**</span></span>  
<span data-ttu-id="46ba9-838">Hello funkce RTrim odebere koncové prázdné znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="46ba9-838">hello RTrim function removes trailing white spaces from a string.</span></span>

<span data-ttu-id="46ba9-839">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-839">**Syntax:**</span></span>  
`str RTrim(str value)`

<span data-ttu-id="46ba9-840">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-840">**Example:**</span></span>  
`RTrim(" Test ")`  
<span data-ttu-id="46ba9-841">Vrátí hodnotu "Test".</span><span class="sxs-lookup"><span data-stu-id="46ba9-841">Returns " Test".</span></span>

- - -
### <a name="select"></a><span data-ttu-id="46ba9-842">Vyberte</span><span class="sxs-lookup"><span data-stu-id="46ba9-842">Select</span></span>
<span data-ttu-id="46ba9-843">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-843">**Description:**</span></span>  
<span data-ttu-id="46ba9-844">Proces všech hodnot v více hodnot atributů (nebo výstupní výrazu) založený na zadanou funkci.</span><span class="sxs-lookup"><span data-stu-id="46ba9-844">Process all values in a multi-valued attribute (or output of an expression) based on function specified.</span></span>

<span data-ttu-id="46ba9-845">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-845">**Syntax:**</span></span>  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* <span data-ttu-id="46ba9-846">Položka: reprezentuje element v hello vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-846">item: Represents an element in hello multi-valued attribute</span></span>
* <span data-ttu-id="46ba9-847">Atribut: hello vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-847">attribute: hello multi-valued attribute</span></span>
* <span data-ttu-id="46ba9-848">výraz: výraz, který vrátí kolekce hodnot</span><span class="sxs-lookup"><span data-stu-id="46ba9-848">expression: an expression that returns a collection of values</span></span>
* <span data-ttu-id="46ba9-849">podmínky: žádné funkce, který dokáže zpracovat položku v atributu hello</span><span class="sxs-lookup"><span data-stu-id="46ba9-849">condition: any function that can process an item in hello attribute</span></span>

<span data-ttu-id="46ba9-850">**Příklady:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-850">**Examples:**</span></span>  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
<span data-ttu-id="46ba9-851">Vrátí všechny hodnoty hello v hello více hodnot atributů otherPhone po odebrání pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="46ba9-851">Return all hello values in hello multi-valued attribute otherPhone after hyphens (-) have been removed.</span></span>

- - -
### <a name="split"></a><span data-ttu-id="46ba9-852">Rozdělení</span><span class="sxs-lookup"><span data-stu-id="46ba9-852">Split</span></span>
<span data-ttu-id="46ba9-853">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-853">**Description:**</span></span>  
<span data-ttu-id="46ba9-854">Hello funkce rozdělení přebírá řetězec oddělené s oddělovačem a udělá z něj řetězec s více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="46ba9-854">hello Split function takes a string separated with a delimiter and makes it a multi-valued string.</span></span>

<span data-ttu-id="46ba9-855">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-855">**Syntax:**</span></span>  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* <span data-ttu-id="46ba9-856">hodnota: hello řetězec s tooseparate znak oddělovače.</span><span class="sxs-lookup"><span data-stu-id="46ba9-856">value: hello string with a delimiter character tooseparate.</span></span>
* <span data-ttu-id="46ba9-857">oddělovač: jeden znak toobe používá jako oddělovač hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-857">delimiter: single character toobe used as hello delimiter.</span></span>
* <span data-ttu-id="46ba9-858">limit: maximální počet hodnot, které můžou vrátit.</span><span class="sxs-lookup"><span data-stu-id="46ba9-858">limit: maximum number of values that can return.</span></span>

<span data-ttu-id="46ba9-859">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-859">**Example:**</span></span>  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
<span data-ttu-id="46ba9-860">Vrátí řetězec s více hodnotami s 2 elementy užitečné pro atribut proxyAddress hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-860">Returns a multi-valued string with 2 elements useful for hello proxyAddress attribute.</span></span>

- - -
### <a name="stringfromguid"></a><span data-ttu-id="46ba9-861">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="46ba9-861">StringFromGuid</span></span>
<span data-ttu-id="46ba9-862">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-862">**Description:**</span></span>  
<span data-ttu-id="46ba9-863">Hello funkce StringFromGuid trvá binární GUID a převede jej tooa řetězec</span><span class="sxs-lookup"><span data-stu-id="46ba9-863">hello StringFromGuid function takes a binary GUID and converts it tooa string</span></span>

<span data-ttu-id="46ba9-864">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-864">**Syntax:**</span></span>  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a><span data-ttu-id="46ba9-865">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="46ba9-865">StringFromSid</span></span>
<span data-ttu-id="46ba9-866">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-866">**Description:**</span></span>  
<span data-ttu-id="46ba9-867">Hello StringFromSid funkce převede bajtové pole obsahující řetězce tooa identifikátor zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="46ba9-867">hello StringFromSid function converts a byte array containing a security identifier tooa string.</span></span>

<span data-ttu-id="46ba9-868">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-868">**Syntax:**</span></span>  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a><span data-ttu-id="46ba9-869">Přepínač</span><span class="sxs-lookup"><span data-stu-id="46ba9-869">Switch</span></span>
<span data-ttu-id="46ba9-870">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-870">**Description:**</span></span>  
<span data-ttu-id="46ba9-871">Přepínač Funkce Hello je použité tooreturn jednu hodnotu na základě vyhodnocených podmínek.</span><span class="sxs-lookup"><span data-stu-id="46ba9-871">hello Switch function is used tooreturn a single value based on evaluated conditions.</span></span>

<span data-ttu-id="46ba9-872">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-872">**Syntax:**</span></span>  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* <span data-ttu-id="46ba9-873">Expr: výraz typu Variant chcete tooevaluate.</span><span class="sxs-lookup"><span data-stu-id="46ba9-873">expr: Variant expression you want tooevaluate.</span></span>
* <span data-ttu-id="46ba9-874">hodnota: hodnota toobe vrátit, pokud je hello odpovídající výraz hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="46ba9-874">value: Value toobe returned if hello corresponding expression is True.</span></span>

<span data-ttu-id="46ba9-875">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-875">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-876">Hello argument funkce přepínač seznam se skládá z dvojic výrazy a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="46ba9-876">hello Switch function argument list consists of pairs of expressions and values.</span></span> <span data-ttu-id="46ba9-877">z levé tooright se vyhodnotí výrazy Hello a je vrácena hodnota hello přidružené hello první výraz tooevaluate tooTrue.</span><span class="sxs-lookup"><span data-stu-id="46ba9-877">hello expressions are evaluated from left tooright, and hello value associated with hello first expression tooevaluate tooTrue is returned.</span></span> <span data-ttu-id="46ba9-878">Pokud hello částí nejsou spárovány správně, dojde k chybě spuštění.</span><span class="sxs-lookup"><span data-stu-id="46ba9-878">If hello parts aren't properly paired, a run-time error occurs.</span></span>

<span data-ttu-id="46ba9-879">Například pokud Výraz1 hodnotu True, vrátí přepínač value1.</span><span class="sxs-lookup"><span data-stu-id="46ba9-879">For example, if expr1 is True, Switch returns value1.</span></span> <span data-ttu-id="46ba9-880">Pokud výraz-1 je False, ale výraz 2 má hodnotu True, vrátí přepínač hodnotu 2 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="46ba9-880">If expr-1 is False, but expr-2 is True, Switch returns value-2, and so on.</span></span>

<span data-ttu-id="46ba9-881">Přepínač vrátí žádnou pokud:</span><span class="sxs-lookup"><span data-stu-id="46ba9-881">Switch returns a Nothing if:</span></span>

* <span data-ttu-id="46ba9-882">Žádný z výrazů hello mají hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="46ba9-882">None of hello expressions are True.</span></span>
* <span data-ttu-id="46ba9-883">První výraz True Hello má odpovídající hodnotu, která má hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="46ba9-883">hello first True expression has a corresponding value that is Null.</span></span>

<span data-ttu-id="46ba9-884">Přepínač vyhodnotí všechny výrazy, i když vrací pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="46ba9-884">Switch evaluates all expressions, even though it returns only one of them.</span></span> <span data-ttu-id="46ba9-885">Z tohoto důvodu je třeba dávat pozor na nežádoucí vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="46ba9-885">For this reason, you should watch for undesirable side effects.</span></span> <span data-ttu-id="46ba9-886">Například pokud hello vyhodnocení jakýkoli výraz výsledkem dělení nulou, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="46ba9-886">For example, if hello evaluation of any expression results in a division by zero error, an error occurs.</span></span>

<span data-ttu-id="46ba9-887">Hodnota může být také hello Chyba funkce, která by vrátit vlastní řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-887">Value can also be hello Error function, which would return a custom string.</span></span>

<span data-ttu-id="46ba9-888">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-888">**Example:**</span></span>  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
<span data-ttu-id="46ba9-889">Vrátí hello jazyk používaný v některé hlavní města, jinak vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-889">Returns hello language spoken in some major cities, otherwise returns an Error.</span></span>

- - -
### <a name="trim"></a><span data-ttu-id="46ba9-890">Uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="46ba9-890">Trim</span></span>
<span data-ttu-id="46ba9-891">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-891">**Description:**</span></span>  
<span data-ttu-id="46ba9-892">funkce Trim Hello odebere úvodní a koncové mezery z řetězce.</span><span class="sxs-lookup"><span data-stu-id="46ba9-892">hello Trim function removes leading and trailing white spaces from a string.</span></span>

<span data-ttu-id="46ba9-893">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-893">**Syntax:**</span></span>  
`str Trim(str value)`  

<span data-ttu-id="46ba9-894">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-894">**Example:**</span></span>  
`Trim(" Test ")`  
<span data-ttu-id="46ba9-895">Vrátí hodnotu "Test".</span><span class="sxs-lookup"><span data-stu-id="46ba9-895">Returns "Test".</span></span>

`Trim([proxyAddresses])`  
<span data-ttu-id="46ba9-896">Odebere úvodní a koncové mezery pro každou hodnotu v atributu proxyAddress hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-896">Removes leading and trailing spaces for each value in hello proxyAddress attribute.</span></span>

- - -
### <a name="ucase"></a><span data-ttu-id="46ba9-897">UCase</span><span class="sxs-lookup"><span data-stu-id="46ba9-897">UCase</span></span>
<span data-ttu-id="46ba9-898">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-898">**Description:**</span></span>  
<span data-ttu-id="46ba9-899">Hello funkce UCase převede všechny znaky v případě tooupper řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-899">hello UCase function converts all characters in a string tooupper case.</span></span>

<span data-ttu-id="46ba9-900">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-900">**Syntax:**</span></span>  
`str UCase(str string)`

<span data-ttu-id="46ba9-901">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-901">**Example:**</span></span>  
`UCase("TeSt")`  
<span data-ttu-id="46ba9-902">Vrátí hodnotu "TEST".</span><span class="sxs-lookup"><span data-stu-id="46ba9-902">Returns "TEST".</span></span>

- - -
### <a name="where"></a><span data-ttu-id="46ba9-903">kde</span><span class="sxs-lookup"><span data-stu-id="46ba9-903">Where</span></span>

<span data-ttu-id="46ba9-904">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-904">**Description:**</span></span>  
<span data-ttu-id="46ba9-905">Vrátí podmnožinu hodnoty z více hodnot atributů (nebo výstupní výrazu) na základě konkrétní podmínky.</span><span class="sxs-lookup"><span data-stu-id="46ba9-905">Returns a subset of values from a multi-valued attribute (or output of an expression) based on specific condition.</span></span>

<span data-ttu-id="46ba9-906">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-906">**Syntax:**</span></span>  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* <span data-ttu-id="46ba9-907">Položka: reprezentuje element v hello vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-907">item: Represents an element in hello multi-valued attribute</span></span>
* <span data-ttu-id="46ba9-908">Atribut: hello vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="46ba9-908">attribute: hello multi-valued attribute</span></span>
* <span data-ttu-id="46ba9-909">podmínky: žádné výraz, který lze vyhodnotit tootrue, nebo hodnotu NEPRAVDA</span><span class="sxs-lookup"><span data-stu-id="46ba9-909">condition: any expression that can be evaluated tootrue or false</span></span>
* <span data-ttu-id="46ba9-910">výraz: výraz, který vrátí kolekce hodnot</span><span class="sxs-lookup"><span data-stu-id="46ba9-910">expression: an expression that returns a collection of values</span></span>

<span data-ttu-id="46ba9-911">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-911">**Example:**</span></span>  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
<span data-ttu-id="46ba9-912">Návratové hodnoty hello certifikátu v userCertificate hello více hodnot atributů, které nejsou vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="46ba9-912">Return hello certificate values in hello multi-valued attribute userCertificate which aren’t expired.</span></span>

- - -
### <a name="with"></a><span data-ttu-id="46ba9-913">S</span><span class="sxs-lookup"><span data-stu-id="46ba9-913">With</span></span>
<span data-ttu-id="46ba9-914">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-914">**Description:**</span></span>  
<span data-ttu-id="46ba9-915">Hello pomocí funkce poskytuje způsob toosimplify pomocí proměnné toorepresent dílčím výrazu, která se zobrazí jeden nebo více časy ve výrazu komplexní hello složitý výraz.</span><span class="sxs-lookup"><span data-stu-id="46ba9-915">hello With function provides a way toosimplify a complex expression by using a variable toorepresent a subexpression which appears one or more times in hello complex expression.</span></span>

<span data-ttu-id="46ba9-916">**Syntaxe:**
`With(var variable, exp subExpression, exp complexExpression)`</span><span class="sxs-lookup"><span data-stu-id="46ba9-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span></span>  
* <span data-ttu-id="46ba9-917">Proměnná: představuje hello dílčím výrazu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-917">variable: Represents hello subexpression.</span></span>
* <span data-ttu-id="46ba9-918">dílčím výrazu: reprezentována proměnná dílčím výrazu.</span><span class="sxs-lookup"><span data-stu-id="46ba9-918">subExpression: subexpression represented by variable.</span></span>
* <span data-ttu-id="46ba9-919">complexExpression: složitý výraz.</span><span class="sxs-lookup"><span data-stu-id="46ba9-919">complexExpression: A complex expression.</span></span>

<span data-ttu-id="46ba9-920">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-920">**Example:**</span></span>  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
<span data-ttu-id="46ba9-921">Je funkčně srovnatelný:</span><span class="sxs-lookup"><span data-stu-id="46ba9-921">Is functionally equivalent to:</span></span>  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
<span data-ttu-id="46ba9-922">Která vrací pouze hodnoty neprošlé certifikátu v atributu userCertificate hello.</span><span class="sxs-lookup"><span data-stu-id="46ba9-922">Which returns only unexpired certificate values in hello userCertificate attribute.</span></span>


- - -
### <a name="word"></a><span data-ttu-id="46ba9-923">Word</span><span class="sxs-lookup"><span data-stu-id="46ba9-923">Word</span></span>
<span data-ttu-id="46ba9-924">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-924">**Description:**</span></span>  
<span data-ttu-id="46ba9-925">Hello Word funkce vrátí slovo obsažené v řetězci, na základě parametrů popisující hello oddělovače toouse a číslo tooreturn hello aplikace word.</span><span class="sxs-lookup"><span data-stu-id="46ba9-925">hello Word function returns a word contained within a string, based on parameters describing hello delimiters toouse and hello word number tooreturn.</span></span>

<span data-ttu-id="46ba9-926">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-926">**Syntax:**</span></span>  
`str Word(str string, num WordNumber, str delimiters)`

* <span data-ttu-id="46ba9-927">řetězec: hello řetězec tooreturn slova.</span><span class="sxs-lookup"><span data-stu-id="46ba9-927">string: hello string tooreturn a word from.</span></span>
* <span data-ttu-id="46ba9-928">WordNumber: by měl vrátit číslo identifikující číslo, které aplikace word.</span><span class="sxs-lookup"><span data-stu-id="46ba9-928">WordNumber: a number identifying which word number should return.</span></span>
* <span data-ttu-id="46ba9-929">oddělovače: řetězec představující hello delimiter(s), která má být použít tooidentify slova</span><span class="sxs-lookup"><span data-stu-id="46ba9-929">delimiters: a string representing hello delimiter(s) that should be used tooidentify words</span></span>

<span data-ttu-id="46ba9-930">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-930">**Remarks:**</span></span>  
<span data-ttu-id="46ba9-931">Jednotlivé řetězce znaků v řetězci oddělených hello jeden hello znaků v oddělovače jsou označeny jako slova:</span><span class="sxs-lookup"><span data-stu-id="46ba9-931">Each string of characters in string separated by hello one of hello characters in delimiters are identified as words:</span></span>

* <span data-ttu-id="46ba9-932">Pokud počet < 1, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-932">If number < 1, returns empty string.</span></span>
* <span data-ttu-id="46ba9-933">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-933">If string is null, returns empty string.</span></span>

<span data-ttu-id="46ba9-934">Pokud řetězec obsahuje méně než číslo slova nebo řetězec neobsahuje žádné slova se identifikovanou pomocí oddělovače, je vrácen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="46ba9-934">If string contains less than number words, or string does not contain any words identified by delimiters, an empty string is returned.</span></span>

<span data-ttu-id="46ba9-935">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="46ba9-935">**Example:**</span></span>  
`Word("hello quick brown fox",3," ")`  
<span data-ttu-id="46ba9-936">Vrátí "hnědá"</span><span class="sxs-lookup"><span data-stu-id="46ba9-936">Returns "brown"</span></span>

`Word("This,string!has&many separators",3,",!&#")`  
<span data-ttu-id="46ba9-937">Vrátí "má"</span><span class="sxs-lookup"><span data-stu-id="46ba9-937">Would return "has"</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46ba9-938">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="46ba9-938">Additional Resources</span></span>
* [<span data-ttu-id="46ba9-939">Principy výrazů deklarativního zřizování</span><span class="sxs-lookup"><span data-stu-id="46ba9-939">Understanding Declarative Provisioning Expressions</span></span>](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [<span data-ttu-id="46ba9-940">Azure AD Connect Sync: Možnosti přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="46ba9-940">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="46ba9-941">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="46ba9-941">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
