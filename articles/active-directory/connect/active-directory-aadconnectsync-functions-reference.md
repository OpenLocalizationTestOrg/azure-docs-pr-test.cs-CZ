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
ms.openlocfilehash: 926f52ef64eb79205dbfb344edc7d9bece2a6947
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a><span data-ttu-id="e07f2-103">Synchronizace Azure AD Connect: odkaz na funkce</span><span class="sxs-lookup"><span data-stu-id="e07f2-103">Azure AD Connect sync: Functions Reference</span></span>
<span data-ttu-id="e07f2-104">Ve službě Azure AD Connect funkce se používají k manipulaci s hodnotou atributu během synchronizace.</span><span class="sxs-lookup"><span data-stu-id="e07f2-104">In Azure AD Connect, functions are used to manipulate an attribute value during synchronization.</span></span>  
<span data-ttu-id="e07f2-105">Syntaxe funkce je vyjádřena v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="e07f2-105">The Syntax of the functions is expressed using the following format:</span></span>  
`<output type> FunctionName(<input type> <position name>, ..)`

<span data-ttu-id="e07f2-106">Pokud funkci je přetížena a přijímá více syntaxe, jsou uvedeny všechny platné syntaxe.</span><span class="sxs-lookup"><span data-stu-id="e07f2-106">If a function is overloaded and accepts multiple syntaxes, all valid syntaxes are listed.</span></span>  
<span data-ttu-id="e07f2-107">Funkce jsou silného typu a jejich ověřte, že typ předané odpovídá zdokumentovaných typu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-107">The functions are strongly typed and they verify that the type passed in matches the documented type.</span></span>  
<span data-ttu-id="e07f2-108">Pokud typ neodpovídá, je vyvolána k chybě.</span><span class="sxs-lookup"><span data-stu-id="e07f2-108">If the type does not match, an error is thrown.</span></span>

<span data-ttu-id="e07f2-109">Typy jsou vyjádřeny s následující syntaxí:</span><span class="sxs-lookup"><span data-stu-id="e07f2-109">The types are expressed with the following syntax:</span></span>

* <span data-ttu-id="e07f2-110">**Koš** – binární</span><span class="sxs-lookup"><span data-stu-id="e07f2-110">**bin** – Binary</span></span>
* <span data-ttu-id="e07f2-111">**BOOL** – Boolean</span><span class="sxs-lookup"><span data-stu-id="e07f2-111">**bool** – Boolean</span></span>
* <span data-ttu-id="e07f2-112">**DT** – datum a čas UTC</span><span class="sxs-lookup"><span data-stu-id="e07f2-112">**dt** – UTC Date/Time</span></span>
* <span data-ttu-id="e07f2-113">**výčet** – výčet známé konstanty</span><span class="sxs-lookup"><span data-stu-id="e07f2-113">**enum** – Enumeration of known constants</span></span>
* <span data-ttu-id="e07f2-114">**Exp** – výraz, který je vyhodnocena jako logická hodnota</span><span class="sxs-lookup"><span data-stu-id="e07f2-114">**exp** – Expression, which is expected to evaluate to a Boolean</span></span>
* <span data-ttu-id="e07f2-115">**mvbin** – vícehodnotových binární</span><span class="sxs-lookup"><span data-stu-id="e07f2-115">**mvbin** – Multi-Valued Binary</span></span>
* <span data-ttu-id="e07f2-116">**mvstr** – vícehodnotových řetězců</span><span class="sxs-lookup"><span data-stu-id="e07f2-116">**mvstr** – Multi-Valued String</span></span>
* <span data-ttu-id="e07f2-117">**mvref** – vícehodnotových odkaz</span><span class="sxs-lookup"><span data-stu-id="e07f2-117">**mvref** – Multi-Valued Reference</span></span>
* <span data-ttu-id="e07f2-118">**Poče** – číselné</span><span class="sxs-lookup"><span data-stu-id="e07f2-118">**num** – Numeric</span></span>
* <span data-ttu-id="e07f2-119">**REF** – odkaz</span><span class="sxs-lookup"><span data-stu-id="e07f2-119">**ref** – Reference</span></span>
* <span data-ttu-id="e07f2-120">**Str –** – řetězec</span><span class="sxs-lookup"><span data-stu-id="e07f2-120">**str** – String</span></span>
* <span data-ttu-id="e07f2-121">**var** – hodnotu typu variant (téměř) žádného jiného typu</span><span class="sxs-lookup"><span data-stu-id="e07f2-121">**var** – A variant of (almost) any other type</span></span>
* <span data-ttu-id="e07f2-122">**void** – nevrací hodnotu</span><span class="sxs-lookup"><span data-stu-id="e07f2-122">**void** – doesn’t return a value</span></span>

<span data-ttu-id="e07f2-123">Funkce s typy **mvbin**, **mvstr**, a **mvref** můžete používat jenom u více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="e07f2-123">The functions with the types **mvbin**, **mvstr**, and **mvref** can only work on multi-valued attributes.</span></span> <span data-ttu-id="e07f2-124">Funkce s **bin**, **str**, a **ref** pracovní jednohodnotové i více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="e07f2-124">Functions with **bin**, **str**, and **ref** work on both single-valued and multi-valued attributes.</span></span>

## <a name="functions-reference"></a><span data-ttu-id="e07f2-125">Reference k funkcím</span><span class="sxs-lookup"><span data-stu-id="e07f2-125">Functions Reference</span></span>
| <span data-ttu-id="e07f2-126">Seznam funkcí</span><span class="sxs-lookup"><span data-stu-id="e07f2-126">List of functions</span></span> |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="e07f2-127">**Certifikát**</span><span class="sxs-lookup"><span data-stu-id="e07f2-127">**Certificate**</span></span> | | | | |
| [<span data-ttu-id="e07f2-128">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="e07f2-128">CertExtensionOids</span></span>](#certextensionoids) |[<span data-ttu-id="e07f2-129">CertFormat</span><span class="sxs-lookup"><span data-stu-id="e07f2-129">CertFormat</span></span>](#certformat) |[<span data-ttu-id="e07f2-130">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="e07f2-130">CertFriendlyName</span></span>](#certfriendlyname) |[<span data-ttu-id="e07f2-131">CertHashString</span><span class="sxs-lookup"><span data-stu-id="e07f2-131">CertHashString</span></span>](#certhashstring) | |
| [<span data-ttu-id="e07f2-132">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="e07f2-132">CertIssuer</span></span>](#certissuer) |[<span data-ttu-id="e07f2-133">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="e07f2-133">CertIssuerDN</span></span>](#certissuerdn) |[<span data-ttu-id="e07f2-134">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-134">CertIssuerOid</span></span>](#certissueroid) |[<span data-ttu-id="e07f2-135">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="e07f2-135">CertKeyAlgorithm</span></span>](#certkeyalgorithm) | |
| [<span data-ttu-id="e07f2-136">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="e07f2-136">CertKeyAlgorithmParams</span></span>](#certkeyalgorithmparams) |[<span data-ttu-id="e07f2-137">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="e07f2-137">CertNameInfo</span></span>](#certnameinfo) |[<span data-ttu-id="e07f2-138">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="e07f2-138">CertNotAfter</span></span>](#certnotafter) |[<span data-ttu-id="e07f2-139">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="e07f2-139">CertNotBefore</span></span>](#certnotbefore) | |
| [<span data-ttu-id="e07f2-140">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-140">CertPublicKeyOid</span></span>](#certpublickeyoid) |[<span data-ttu-id="e07f2-141">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-141">CertPublicKeyParametersOid</span></span>](#certpublickeyparametersoid) |[<span data-ttu-id="e07f2-142">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="e07f2-142">CertSerialNumber</span></span>](#certserialnumber) |[<span data-ttu-id="e07f2-143">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-143">CertSignatureAlgorithmOid</span></span>](#certsignaturealgorithmoid) | |
| [<span data-ttu-id="e07f2-144">CertSubject</span><span class="sxs-lookup"><span data-stu-id="e07f2-144">CertSubject</span></span>](#certsubject) |[<span data-ttu-id="e07f2-145">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="e07f2-145">CertSubjectNameDN</span></span>](#certsubjectnamedn) |[<span data-ttu-id="e07f2-146">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-146">CertSubjectNameOid</span></span>](#certsubjectnameoid) |[<span data-ttu-id="e07f2-147">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="e07f2-147">CertThumbprint</span></span>](#certthumbprint) | |
[<span data-ttu-id="e07f2-148">CertVersion</span><span class="sxs-lookup"><span data-stu-id="e07f2-148"> CertVersion</span></span>](#certversion) |[<span data-ttu-id="e07f2-149">IsCert</span><span class="sxs-lookup"><span data-stu-id="e07f2-149">IsCert</span></span>](#iscert) | | | |
| <span data-ttu-id="e07f2-150">**Převod**</span><span class="sxs-lookup"><span data-stu-id="e07f2-150">**Conversion**</span></span> | | | | |
| [<span data-ttu-id="e07f2-151">CBool –</span><span class="sxs-lookup"><span data-stu-id="e07f2-151">CBool</span></span>](#cbool) |[<span data-ttu-id="e07f2-152">CDate –</span><span class="sxs-lookup"><span data-stu-id="e07f2-152">CDate</span></span>](#cdate) |[<span data-ttu-id="e07f2-153">CGuid</span><span class="sxs-lookup"><span data-stu-id="e07f2-153">CGuid</span></span>](#cguid) |[<span data-ttu-id="e07f2-154">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="e07f2-154">ConvertFromBase64</span></span>](#convertfrombase64) | |
| [<span data-ttu-id="e07f2-155">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="e07f2-155">ConvertToBase64</span></span>](#converttobase64) |[<span data-ttu-id="e07f2-156">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="e07f2-156">ConvertFromUTF8Hex</span></span>](#convertfromutf8hex) |[<span data-ttu-id="e07f2-157">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="e07f2-157">ConvertToUTF8Hex</span></span>](#converttoutf8hex) |[<span data-ttu-id="e07f2-158">CNum</span><span class="sxs-lookup"><span data-stu-id="e07f2-158">CNum</span></span>](#cnum) | |
| [<span data-ttu-id="e07f2-159">CRef –</span><span class="sxs-lookup"><span data-stu-id="e07f2-159">CRef</span></span>](#cref) |[<span data-ttu-id="e07f2-160">CStr</span><span class="sxs-lookup"><span data-stu-id="e07f2-160">CStr</span></span>](#cstr) |[<span data-ttu-id="e07f2-161">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="e07f2-161">StringFromGuid</span></span>](#StringFromGuid) |[<span data-ttu-id="e07f2-162">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="e07f2-162">StringFromSid</span></span>](#stringfromsid) | |
| <span data-ttu-id="e07f2-163">**Datum a čas**</span><span class="sxs-lookup"><span data-stu-id="e07f2-163">**Date / Time**</span></span> | | | | |
| [<span data-ttu-id="e07f2-164">Funkce DateAdd</span><span class="sxs-lookup"><span data-stu-id="e07f2-164">DateAdd</span></span>](#dateadd) |[<span data-ttu-id="e07f2-165">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="e07f2-165">DateFromNum</span></span>](#datefromnum) |[<span data-ttu-id="e07f2-166">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="e07f2-166">FormatDateTime</span></span>](#formatdatetime) |[<span data-ttu-id="e07f2-167">Nyní</span><span class="sxs-lookup"><span data-stu-id="e07f2-167">Now</span></span>](#now) | |
| [<span data-ttu-id="e07f2-168">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="e07f2-168">NumFromDate</span></span>](#numfromdate) | | | | |
| <span data-ttu-id="e07f2-169">**Adresář**</span><span class="sxs-lookup"><span data-stu-id="e07f2-169">**Directory**</span></span> | | | | |
| [<span data-ttu-id="e07f2-170">DNComponent</span><span class="sxs-lookup"><span data-stu-id="e07f2-170">DNComponent</span></span>](#dncomponent) |[<span data-ttu-id="e07f2-171">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="e07f2-171">DNComponentRev</span></span>](#dncomponentrev) |[<span data-ttu-id="e07f2-172">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="e07f2-172">EscapeDNComponent</span></span>](#escapedncomponent) | | |
| <span data-ttu-id="e07f2-173">**Vyhodnocení**</span><span class="sxs-lookup"><span data-stu-id="e07f2-173">**Evaluation**</span></span> | | | | |
| [<span data-ttu-id="e07f2-174">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="e07f2-174">IsBitSet</span></span>](#isbitset) |[<span data-ttu-id="e07f2-175">IsDate</span><span class="sxs-lookup"><span data-stu-id="e07f2-175">IsDate</span></span>](#isdate) |[<span data-ttu-id="e07f2-176">IsEmpty –</span><span class="sxs-lookup"><span data-stu-id="e07f2-176">IsEmpty</span></span>](#isempty) |[<span data-ttu-id="e07f2-177">IsGuid</span><span class="sxs-lookup"><span data-stu-id="e07f2-177">IsGuid</span></span>](#isguid) | |
| [<span data-ttu-id="e07f2-178">IsNull</span><span class="sxs-lookup"><span data-stu-id="e07f2-178">IsNull</span></span>](#isnull) |[<span data-ttu-id="e07f2-179">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="e07f2-179">IsNullOrEmpty</span></span>](#isnullorempty) |[<span data-ttu-id="e07f2-180">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="e07f2-180">IsNumeric</span></span>](#isnumeric) |[<span data-ttu-id="e07f2-181">IsPresent</span><span class="sxs-lookup"><span data-stu-id="e07f2-181">IsPresent</span></span>](#ispresent) | |
| [<span data-ttu-id="e07f2-182">IsString</span><span class="sxs-lookup"><span data-stu-id="e07f2-182">IsString</span></span>](#isstring) | | | | |
| <span data-ttu-id="e07f2-183">**Matematické**</span><span class="sxs-lookup"><span data-stu-id="e07f2-183">**Math**</span></span> | | | | |
| [<span data-ttu-id="e07f2-184">BitAnd</span><span class="sxs-lookup"><span data-stu-id="e07f2-184">BitAnd</span></span>](#bitand) |[<span data-ttu-id="e07f2-185">BitOr</span><span class="sxs-lookup"><span data-stu-id="e07f2-185">BitOr</span></span>](#bitor) |[<span data-ttu-id="e07f2-186">RandomNum</span><span class="sxs-lookup"><span data-stu-id="e07f2-186">RandomNum</span></span>](#randomnum) | | |
| <span data-ttu-id="e07f2-187">**Více hodnot**</span><span class="sxs-lookup"><span data-stu-id="e07f2-187">**Multi-valued**</span></span> | | | | |
| [<span data-ttu-id="e07f2-188">Obsahuje</span><span class="sxs-lookup"><span data-stu-id="e07f2-188">Contains</span></span>](#contains) |[<span data-ttu-id="e07f2-189">Počet</span><span class="sxs-lookup"><span data-stu-id="e07f2-189">Count</span></span>](#count) |[<span data-ttu-id="e07f2-190">Položka</span><span class="sxs-lookup"><span data-stu-id="e07f2-190">Item</span></span>](#item) |[<span data-ttu-id="e07f2-191">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="e07f2-191">ItemOrNull</span></span>](#itemornull) | |
| [<span data-ttu-id="e07f2-192">Spojení</span><span class="sxs-lookup"><span data-stu-id="e07f2-192">Join</span></span>](#join) |[<span data-ttu-id="e07f2-193">Removeduplicates –</span><span class="sxs-lookup"><span data-stu-id="e07f2-193">RemoveDuplicates</span></span>](#removeduplicates) |[<span data-ttu-id="e07f2-194">Rozdělení</span><span class="sxs-lookup"><span data-stu-id="e07f2-194">Split</span></span>](#split) | | |
| <span data-ttu-id="e07f2-195">**Tok programu**</span><span class="sxs-lookup"><span data-stu-id="e07f2-195">**Program Flow**</span></span> | | | | |
| [<span data-ttu-id="e07f2-196">Chyba</span><span class="sxs-lookup"><span data-stu-id="e07f2-196">Error</span></span>](#error) |[<span data-ttu-id="e07f2-197">IIF</span><span class="sxs-lookup"><span data-stu-id="e07f2-197">IIF</span></span>](#iif) |[<span data-ttu-id="e07f2-198">Výběr</span><span class="sxs-lookup"><span data-stu-id="e07f2-198">Select</span></span>](#select) |[<span data-ttu-id="e07f2-199">Přepínače</span><span class="sxs-lookup"><span data-stu-id="e07f2-199">Switch</span></span>](#switch) | |
| [<span data-ttu-id="e07f2-200">Kde</span><span class="sxs-lookup"><span data-stu-id="e07f2-200">Where</span></span>](#where) |[<span data-ttu-id="e07f2-201">S</span><span class="sxs-lookup"><span data-stu-id="e07f2-201">With</span></span>](#with) | | | |
| <span data-ttu-id="e07f2-202">**Text**</span><span class="sxs-lookup"><span data-stu-id="e07f2-202">**Text**</span></span> | | | | |
| [<span data-ttu-id="e07f2-203">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="e07f2-203">GUID</span></span>](#guid) |[<span data-ttu-id="e07f2-204">InStr</span><span class="sxs-lookup"><span data-stu-id="e07f2-204">InStr</span></span>](#instr) |[<span data-ttu-id="e07f2-205">InStrRev</span><span class="sxs-lookup"><span data-stu-id="e07f2-205">InStrRev</span></span>](#instrrev) |[<span data-ttu-id="e07f2-206">LCase</span><span class="sxs-lookup"><span data-stu-id="e07f2-206">LCase</span></span>](#lcase) | |
| [<span data-ttu-id="e07f2-207">Vlevo</span><span class="sxs-lookup"><span data-stu-id="e07f2-207">Left</span></span>](#left) |[<span data-ttu-id="e07f2-208">Len</span><span class="sxs-lookup"><span data-stu-id="e07f2-208">Len</span></span>](#len) |[<span data-ttu-id="e07f2-209">LTrim</span><span class="sxs-lookup"><span data-stu-id="e07f2-209">LTrim</span></span>](#ltrim) |[<span data-ttu-id="e07f2-210">Mid –</span><span class="sxs-lookup"><span data-stu-id="e07f2-210">Mid</span></span>](#mid) | |
| [<span data-ttu-id="e07f2-211">PadLeft</span><span class="sxs-lookup"><span data-stu-id="e07f2-211">PadLeft</span></span>](#padleft) |[<span data-ttu-id="e07f2-212">PadRight –</span><span class="sxs-lookup"><span data-stu-id="e07f2-212">PadRight</span></span>](#padright) |[<span data-ttu-id="e07f2-213">PCase</span><span class="sxs-lookup"><span data-stu-id="e07f2-213">PCase</span></span>](#pcase) |[<span data-ttu-id="e07f2-214">Nahradit</span><span class="sxs-lookup"><span data-stu-id="e07f2-214">Replace</span></span>](#replace) | |
| [<span data-ttu-id="e07f2-215">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="e07f2-215">ReplaceChars</span></span>](#replacechars) |[<span data-ttu-id="e07f2-216">Vpravo</span><span class="sxs-lookup"><span data-stu-id="e07f2-216">Right</span></span>](#right) |[<span data-ttu-id="e07f2-217">RTrim</span><span class="sxs-lookup"><span data-stu-id="e07f2-217">RTrim</span></span>](#rtrim) |[<span data-ttu-id="e07f2-218">Uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="e07f2-218">Trim</span></span>](#trim) | |
| [<span data-ttu-id="e07f2-219">UCase</span><span class="sxs-lookup"><span data-stu-id="e07f2-219">UCase</span></span>](#ucase) |[<span data-ttu-id="e07f2-220">Aplikace Word</span><span class="sxs-lookup"><span data-stu-id="e07f2-220">Word</span></span>](#word) | | | |

- - -
### <a name="bitand"></a><span data-ttu-id="e07f2-221">BitAnd</span><span class="sxs-lookup"><span data-stu-id="e07f2-221">BitAnd</span></span>
<span data-ttu-id="e07f2-222">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-222">**Description:**</span></span>  
<span data-ttu-id="e07f2-223">Funkce BitAnd nastaví na hodnotu určeného bits.</span><span class="sxs-lookup"><span data-stu-id="e07f2-223">The BitAnd function sets specified bits on a value.</span></span>

<span data-ttu-id="e07f2-224">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-224">**Syntax:**</span></span>  
`num BitAnd(num value1, num value2)`

* <span data-ttu-id="e07f2-225">Hodnota1, hodnota2: číselných hodnot, které by měly být logickým společně</span><span class="sxs-lookup"><span data-stu-id="e07f2-225">value1, value2: numeric values that should be AND’ed together</span></span>

<span data-ttu-id="e07f2-226">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-226">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-227">Tato funkce převede oba parametry binární reprezentace a nastaví trochu na:</span><span class="sxs-lookup"><span data-stu-id="e07f2-227">This function converts both parameters to the binary representation and sets a bit to:</span></span>

* <span data-ttu-id="e07f2-228">0 – Pokud jeden nebo oba odpovídající bity *maska* a *příznak* mají hodnotu 0</span><span class="sxs-lookup"><span data-stu-id="e07f2-228">0 - if one or both of the corresponding bits in *mask* and *flag* are 0</span></span>
* <span data-ttu-id="e07f2-229">1 – Pokud jsou obě odpovídající bity 1.</span><span class="sxs-lookup"><span data-stu-id="e07f2-229">1 - if both of the corresponding bits are 1.</span></span>

<span data-ttu-id="e07f2-230">Jinými slovy vrátí hodnotu 0 ve všech případech kromě případů, kdy jsou odpovídající bity oba parametry 1.</span><span class="sxs-lookup"><span data-stu-id="e07f2-230">In other words, it returns 0 in all cases except when the corresponding bits of both parameters are 1.</span></span>

<span data-ttu-id="e07f2-231">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-231">**Example:**</span></span>  
`BitAnd(&HF, &HF7)`  
<span data-ttu-id="e07f2-232">Vrátí hodnotu 7, protože hexadecimální "F" a "F7" vyhodnocení na tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-232">Returns 7 because hexadecimal "F" AND "F7" evaluate to this value.</span></span>

- - -
### <a name="bitor"></a><span data-ttu-id="e07f2-233">BitOr</span><span class="sxs-lookup"><span data-stu-id="e07f2-233">BitOr</span></span>
<span data-ttu-id="e07f2-234">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-234">**Description:**</span></span>  
<span data-ttu-id="e07f2-235">Funkce BitOr nastaví na hodnotu určeného bits.</span><span class="sxs-lookup"><span data-stu-id="e07f2-235">The BitOr function sets specified bits on a value.</span></span>

<span data-ttu-id="e07f2-236">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-236">**Syntax:**</span></span>  
`num BitOr(num value1, num value2)`

* <span data-ttu-id="e07f2-237">Hodnota1, hodnota2: číselných hodnot, které by měly být společně sjednocením (OR)</span><span class="sxs-lookup"><span data-stu-id="e07f2-237">value1, value2: numeric values that should be OR’ed together</span></span>

<span data-ttu-id="e07f2-238">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-238">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-239">Tato funkce převede oba parametry binární reprezentace a nastaví trochu 1, pokud jeden nebo oba odpovídající bity maska a příznak je 1 a na 0, pokud jsou obě odpovídající bity 0.</span><span class="sxs-lookup"><span data-stu-id="e07f2-239">This function converts both parameters to the binary representation and sets a bit to 1 if one or both of the corresponding bits in mask and flag are 1, and to 0 if both of the corresponding bits are 0.</span></span> <span data-ttu-id="e07f2-240">Jinými slovy vrátí 1 ve všech případech, s výjimkou případů, kdy odpovídající bity oba parametry mají hodnotu 0.</span><span class="sxs-lookup"><span data-stu-id="e07f2-240">In other words, it returns 1 in all cases except where the corresponding bits of both parameters are 0.</span></span>

- - -
### <a name="cbool"></a><span data-ttu-id="e07f2-241">CBool –</span><span class="sxs-lookup"><span data-stu-id="e07f2-241">CBool</span></span>
<span data-ttu-id="e07f2-242">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-242">**Description:**</span></span>  
<span data-ttu-id="e07f2-243">Vrátí logickou hodnotu podle vyhodnocený výraz CBool – funkce</span><span class="sxs-lookup"><span data-stu-id="e07f2-243">The CBool function returns a Boolean based on the evaluated expression</span></span>

<span data-ttu-id="e07f2-244">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-244">**Syntax:**</span></span>  
`bool CBool(exp Expression)`

<span data-ttu-id="e07f2-245">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-245">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-246">Pokud se výraz vyhodnotí nenulovou hodnotu, pak CBool – vrátí hodnotu True, jinak vrátí hodnotu False.</span><span class="sxs-lookup"><span data-stu-id="e07f2-246">If the expression evaluates to a nonzero value, then CBool returns True, else it returns False.</span></span>

<span data-ttu-id="e07f2-247">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-247">**Example:**</span></span>  
`CBool([attrib1] = [attrib2])`  

<span data-ttu-id="e07f2-248">Vrátí hodnotu True, pokud oba atributy mají stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-248">Returns True if both attributes have the same value.</span></span>

- - -
### <a name="cdate"></a><span data-ttu-id="e07f2-249">CDate –</span><span class="sxs-lookup"><span data-stu-id="e07f2-249">CDate</span></span>
<span data-ttu-id="e07f2-250">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-250">**Description:**</span></span>  
<span data-ttu-id="e07f2-251">CDate – funkce vrátí z řetězce na datum a čas UTC.</span><span class="sxs-lookup"><span data-stu-id="e07f2-251">The CDate function returns a UTC DateTime from a string.</span></span> <span data-ttu-id="e07f2-252">Data a času není typu nativní atribut synchronizované, ale je používána některé funkce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-252">DateTime is not a native attribute type in Sync but is used by some functions.</span></span>

<span data-ttu-id="e07f2-253">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-253">**Syntax:**</span></span>  
`dt CDate(str value)`

* <span data-ttu-id="e07f2-254">Hodnota: Řetězec datum, čas a volitelně časové pásmo</span><span class="sxs-lookup"><span data-stu-id="e07f2-254">Value: A string with a date, time, and optionally time zone</span></span>

<span data-ttu-id="e07f2-255">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-255">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-256">Vrácený řetězec je vždy ve standardu UTC.</span><span class="sxs-lookup"><span data-stu-id="e07f2-256">The returned string is always in UTC.</span></span>

<span data-ttu-id="e07f2-257">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-257">**Example:**</span></span>  
`CDate([employeeStartTime])`  
<span data-ttu-id="e07f2-258">Vrátí hodnotu DateTime na základě na zaměstnance počáteční čas</span><span class="sxs-lookup"><span data-stu-id="e07f2-258">Returns a DateTime based on the employee’s start time</span></span>

`CDate("2013-01-10 4:00 PM -8")`  
<span data-ttu-id="e07f2-259">Vrátí data a času představující "2013-01-11 12:00:00"</span><span class="sxs-lookup"><span data-stu-id="e07f2-259">Returns a DateTime representing "2013-01-11 12:00 AM"</span></span>








- - -
### <a name="certextensionoids"></a><span data-ttu-id="e07f2-260">CertExtensionOids</span><span class="sxs-lookup"><span data-stu-id="e07f2-260">CertExtensionOids</span></span>
<span data-ttu-id="e07f2-261">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-261">**Description:**</span></span>  
<span data-ttu-id="e07f2-262">Vrátí hodnoty Oid všech důležitých rozšíření certifikátů objektu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-262">Returns the Oid values of all the critical extensions of a certificate object.</span></span>

<span data-ttu-id="e07f2-263">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-263">**Syntax:**</span></span>  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-264">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-264">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-265">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-265">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certformat"></a><span data-ttu-id="e07f2-266">CertFormat</span><span class="sxs-lookup"><span data-stu-id="e07f2-266">CertFormat</span></span>
<span data-ttu-id="e07f2-267">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-267">**Description:**</span></span>  
<span data-ttu-id="e07f2-268">Vrátí název formátu tento certifikát x.509 v3.</span><span class="sxs-lookup"><span data-stu-id="e07f2-268">Returns the name of the format of this X.509v3 certificate.</span></span>

<span data-ttu-id="e07f2-269">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-269">**Syntax:**</span></span>  
`str CertFormat(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-270">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-270">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-271">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-271">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certfriendlyname"></a><span data-ttu-id="e07f2-272">CertFriendlyName</span><span class="sxs-lookup"><span data-stu-id="e07f2-272">CertFriendlyName</span></span>
<span data-ttu-id="e07f2-273">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-273">**Description:**</span></span>  
<span data-ttu-id="e07f2-274">Vrátí související aliasu pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="e07f2-274">Returns the associated alias for a certificate.</span></span>

<span data-ttu-id="e07f2-275">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-275">**Syntax:**</span></span>  
`str CertFriendlyName(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-276">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-276">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-277">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-277">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certhashstring"></a><span data-ttu-id="e07f2-278">CertHashString</span><span class="sxs-lookup"><span data-stu-id="e07f2-278">CertHashString</span></span>
<span data-ttu-id="e07f2-279">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-279">**Description:**</span></span>  
<span data-ttu-id="e07f2-280">Vrátí hodnotu hash SHA1 certifikátu x.509 v3 jako řetězec hexadecimálních znaků.</span><span class="sxs-lookup"><span data-stu-id="e07f2-280">Returns the SHA1 hash value for the X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="e07f2-281">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-281">**Syntax:**</span></span>  
`str CertHashString(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-282">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-282">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-283">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-283">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuer"></a><span data-ttu-id="e07f2-284">CertIssuer</span><span class="sxs-lookup"><span data-stu-id="e07f2-284">CertIssuer</span></span>
<span data-ttu-id="e07f2-285">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-285">**Description:**</span></span>  
<span data-ttu-id="e07f2-286">Vrací název certifikační autority, která vystavila certifikát x.509 v3.</span><span class="sxs-lookup"><span data-stu-id="e07f2-286">Returns the name of the certificate authority that issued the X.509v3 certificate.</span></span>

<span data-ttu-id="e07f2-287">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-287">**Syntax:**</span></span>  
`str CertIssuer(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-288">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-288">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-289">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-289">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissuerdn"></a><span data-ttu-id="e07f2-290">CertIssuerDN</span><span class="sxs-lookup"><span data-stu-id="e07f2-290">CertIssuerDN</span></span>
<span data-ttu-id="e07f2-291">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-291">**Description:**</span></span>  
<span data-ttu-id="e07f2-292">Vrátí rozlišující název vystavitele certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-292">Returns the distinguished name of the certificate issuer.</span></span>

<span data-ttu-id="e07f2-293">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-293">**Syntax:**</span></span>  
`str CertIssuerDN(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-294">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-294">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-295">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-295">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certissueroid"></a><span data-ttu-id="e07f2-296">CertIssuerOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-296">CertIssuerOid</span></span>
<span data-ttu-id="e07f2-297">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-297">**Description:**</span></span>  
<span data-ttu-id="e07f2-298">Vrátí identifikátor vystavitele certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-298">Returns the Oid of the certificate issuer.</span></span>

<span data-ttu-id="e07f2-299">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-299">**Syntax:**</span></span>  
`str CertIssuerOid(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-300">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-300">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-301">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-301">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithm"></a><span data-ttu-id="e07f2-302">CertKeyAlgorithm</span><span class="sxs-lookup"><span data-stu-id="e07f2-302">CertKeyAlgorithm</span></span>
<span data-ttu-id="e07f2-303">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-303">**Description:**</span></span>  
<span data-ttu-id="e07f2-304">Vrátí informace o algoritmus klíče pro tento certifikát x.509 v3 jako řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-304">Returns the key algorithm information for this X.509v3 certificate as a string.</span></span>

<span data-ttu-id="e07f2-305">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-305">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-306">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-306">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-307">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-307">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certkeyalgorithmparams"></a><span data-ttu-id="e07f2-308">CertKeyAlgorithmParams</span><span class="sxs-lookup"><span data-stu-id="e07f2-308">CertKeyAlgorithmParams</span></span>
<span data-ttu-id="e07f2-309">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-309">**Description:**</span></span>  
<span data-ttu-id="e07f2-310">Vrátí parametry algoritmus klíče pro certifikát x.509 v3 jako řetězec hexadecimálních znaků.</span><span class="sxs-lookup"><span data-stu-id="e07f2-310">Returns the key algorithm parameters for the X.509v3 certificate as a hexadecimal string.</span></span>

<span data-ttu-id="e07f2-311">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-311">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-312">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-312">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-313">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-313">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnameinfo"></a><span data-ttu-id="e07f2-314">CertNameInfo</span><span class="sxs-lookup"><span data-stu-id="e07f2-314">CertNameInfo</span></span>
<span data-ttu-id="e07f2-315">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-315">**Description:**</span></span>  
<span data-ttu-id="e07f2-316">Vrátí subjektu a vystavitele názvy z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-316">Returns the subject and issuer names from a certificate.</span></span>

<span data-ttu-id="e07f2-317">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-317">**Syntax:**</span></span>  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   <span data-ttu-id="e07f2-318">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-318">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-319">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-319">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
*   <span data-ttu-id="e07f2-320">X509NameType: X509NameType hodnoty pro předmět.</span><span class="sxs-lookup"><span data-stu-id="e07f2-320">X509NameType: The X509NameType value for the subject.</span></span>
*   <span data-ttu-id="e07f2-321">includesIssuerName: hodnota true, mají-li zahrnout název vystavitele; jinak hodnota false.</span><span class="sxs-lookup"><span data-stu-id="e07f2-321">includesIssuerName: true to include the issuer name; otherwise, false.</span></span>

- - -
### <a name="certnotafter"></a><span data-ttu-id="e07f2-322">CertNotAfter</span><span class="sxs-lookup"><span data-stu-id="e07f2-322">CertNotAfter</span></span>
<span data-ttu-id="e07f2-323">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-323">**Description:**</span></span>  
<span data-ttu-id="e07f2-324">Vrátí datum v místní čase, po kterém certifikát je již neplatný.</span><span class="sxs-lookup"><span data-stu-id="e07f2-324">Returns the date in local time after which a certificate is no longer valid.</span></span>

<span data-ttu-id="e07f2-325">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-325">**Syntax:**</span></span>  
`dt CertNotAfter(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-326">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-326">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-327">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-327">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certnotbefore"></a><span data-ttu-id="e07f2-328">CertNotBefore</span><span class="sxs-lookup"><span data-stu-id="e07f2-328">CertNotBefore</span></span>
<span data-ttu-id="e07f2-329">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-329">**Description:**</span></span>  
<span data-ttu-id="e07f2-330">Vrátí datum v místní čase, při kterém certifikát začne platit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-330">Returns the date in local time on which a certificate becomes valid.</span></span>

<span data-ttu-id="e07f2-331">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-331">**Syntax:**</span></span>  
`dt CertNotBefore(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-332">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-332">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-333">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-333">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyoid"></a><span data-ttu-id="e07f2-334">CertPublicKeyOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-334">CertPublicKeyOid</span></span>
<span data-ttu-id="e07f2-335">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-335">**Description:**</span></span>  
<span data-ttu-id="e07f2-336">Vrátí identifikátor veřejné klíče pro certifikát x.509 v3.</span><span class="sxs-lookup"><span data-stu-id="e07f2-336">Returns the Oid of the public key for the X.509v3 certificate.</span></span>

<span data-ttu-id="e07f2-337">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-337">**Syntax:**</span></span>  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-338">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-338">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-339">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-339">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certpublickeyparametersoid"></a><span data-ttu-id="e07f2-340">CertPublicKeyParametersOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-340">CertPublicKeyParametersOid</span></span>
<span data-ttu-id="e07f2-341">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-341">**Description:**</span></span>  
<span data-ttu-id="e07f2-342">Vrátí identifikátor veřejné parametrů klíče pro certifikát x.509 v3.</span><span class="sxs-lookup"><span data-stu-id="e07f2-342">Returns the Oid of the public key parameters for the X.509v3 certificate.</span></span>

<span data-ttu-id="e07f2-343">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-343">**Syntax:**</span></span>  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-344">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-344">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-345">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-345">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certserialnumber"></a><span data-ttu-id="e07f2-346">CertSerialNumber</span><span class="sxs-lookup"><span data-stu-id="e07f2-346">CertSerialNumber</span></span>
<span data-ttu-id="e07f2-347">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-347">**Description:**</span></span>  
<span data-ttu-id="e07f2-348">Vrátí sériové číslo certifikátu x.509 v3.</span><span class="sxs-lookup"><span data-stu-id="e07f2-348">Returns the serial number of the X.509v3 certificate.</span></span>

<span data-ttu-id="e07f2-349">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-349">**Syntax:**</span></span>  
`str CertSerialNumber(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-350">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-350">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-351">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-351">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsignaturealgorithmoid"></a><span data-ttu-id="e07f2-352">CertSignatureAlgorithmOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-352">CertSignatureAlgorithmOid</span></span>
<span data-ttu-id="e07f2-353">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-353">**Description:**</span></span>  
<span data-ttu-id="e07f2-354">Vrátí identifikátor algoritmus použitý k vytvoření podpisu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-354">Returns the Oid of the algorithm used to create the signature of a certificate.</span></span>

<span data-ttu-id="e07f2-355">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-355">**Syntax:**</span></span>  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-356">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-356">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-357">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-357">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubject"></a><span data-ttu-id="e07f2-358">CertSubject</span><span class="sxs-lookup"><span data-stu-id="e07f2-358">CertSubject</span></span>
<span data-ttu-id="e07f2-359">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-359">**Description:**</span></span>  
<span data-ttu-id="e07f2-360">Získá název předmětu rozlišující z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-360">Gets the subject distinguished name from a certificate.</span></span>

<span data-ttu-id="e07f2-361">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-361">**Syntax:**</span></span>  
`str CertSubject(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-362">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-362">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-363">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-363">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnamedn"></a><span data-ttu-id="e07f2-364">CertSubjectNameDN</span><span class="sxs-lookup"><span data-stu-id="e07f2-364">CertSubjectNameDN</span></span>
<span data-ttu-id="e07f2-365">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-365">**Description:**</span></span>  
<span data-ttu-id="e07f2-366">Vrátí název předmětu rozlišující z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-366">Returns the subject distinguished name from a certificate.</span></span>

<span data-ttu-id="e07f2-367">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-367">**Syntax:**</span></span>  
`str CertSubjectNameDN(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-368">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-368">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-369">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-369">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certsubjectnameoid"></a><span data-ttu-id="e07f2-370">CertSubjectNameOid</span><span class="sxs-lookup"><span data-stu-id="e07f2-370">CertSubjectNameOid</span></span>
<span data-ttu-id="e07f2-371">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-371">**Description:**</span></span>  
<span data-ttu-id="e07f2-372">Vrátí identifikátor název subjektu z certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-372">Returns the Oid of the subject name from a certificate.</span></span>

<span data-ttu-id="e07f2-373">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-373">**Syntax:**</span></span>  
`str CertSubjectNameOid(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-374">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-374">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-375">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-375">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certthumbprint"></a><span data-ttu-id="e07f2-376">CertThumbprint</span><span class="sxs-lookup"><span data-stu-id="e07f2-376">CertThumbprint</span></span>
<span data-ttu-id="e07f2-377">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-377">**Description:**</span></span>  
<span data-ttu-id="e07f2-378">Vrátí kryptografický otisk certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-378">Returns the thumbprint of a certificate.</span></span>

<span data-ttu-id="e07f2-379">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-379">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-380">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-380">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-381">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-381">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="certversion"></a><span data-ttu-id="e07f2-382">CertVersion</span><span class="sxs-lookup"><span data-stu-id="e07f2-382">CertVersion</span></span>
<span data-ttu-id="e07f2-383">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-383">**Description:**</span></span>  
<span data-ttu-id="e07f2-384">Vrátí verze formátu X.509 certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-384">Returns the X.509 format version of a certificate.</span></span>

<span data-ttu-id="e07f2-385">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-385">**Syntax:**</span></span>  
`str CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-386">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-386">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-387">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-387">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>

- - -
### <a name="cguid"></a><span data-ttu-id="e07f2-388">CGuid</span><span class="sxs-lookup"><span data-stu-id="e07f2-388">CGuid</span></span>
<span data-ttu-id="e07f2-389">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-389">**Description:**</span></span>  
<span data-ttu-id="e07f2-390">Funkce CGuid převede řetězcovou reprezentaci identifikátor GUID jeho binární reprezentace.</span><span class="sxs-lookup"><span data-stu-id="e07f2-390">The CGuid function converts the string representation of a GUID to its binary representation.</span></span>

<span data-ttu-id="e07f2-391">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-391">**Syntax:**</span></span>  
`bin CGuid(str GUID)`

* <span data-ttu-id="e07f2-392">Řetězec formátu v tomto vzoru: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="e07f2-392">A String formatted in this pattern: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

- - -
### <a name="contains"></a><span data-ttu-id="e07f2-393">Contains</span><span class="sxs-lookup"><span data-stu-id="e07f2-393">Contains</span></span>
<span data-ttu-id="e07f2-394">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-394">**Description:**</span></span>  
<span data-ttu-id="e07f2-395">Obsahuje funkce najde řetězec uvnitř vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-395">The Contains function finds a string inside a multi-valued attribute</span></span>

<span data-ttu-id="e07f2-396">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-396">**Syntax:**</span></span>  
<span data-ttu-id="e07f2-397">`num Contains (mvstring attribute, str search)`-malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="e07f2-397">`num Contains (mvstring attribute, str search)` - case-sensitive</span></span>  
`num Contains (mvstring attribute, str search, enum Casetype)`  
<span data-ttu-id="e07f2-398">`num Contains (mvref attribute, str search)`-malá a velká písmena</span><span class="sxs-lookup"><span data-stu-id="e07f2-398">`num Contains (mvref attribute, str search)` - case-sensitive</span></span>

* <span data-ttu-id="e07f2-399">Atribut: více hodnot atributů pro vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="e07f2-399">attribute: the multi-valued attribute to search.</span></span>
* <span data-ttu-id="e07f2-400">hledání: řetězec, který má v atributu najít.</span><span class="sxs-lookup"><span data-stu-id="e07f2-400">search: string to find in the attribute.</span></span>
* <span data-ttu-id="e07f2-401">Casetype: CaseInsensitive nebo CaseSensitive.</span><span class="sxs-lookup"><span data-stu-id="e07f2-401">Casetype: CaseInsensitive or CaseSensitive.</span></span>

<span data-ttu-id="e07f2-402">Vrátí index atribut více hodnot, kde byl nalezen řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-402">Returns index in the multi-valued attribute where the string was found.</span></span> <span data-ttu-id="e07f2-403">0 je vrácena, pokud není nalezen řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-403">0 is returned if the string is not found.</span></span>

<span data-ttu-id="e07f2-404">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-404">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-405">Pro více hodnot řetězec atributy hledáním dílčích řetězců v hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e07f2-405">For multi-valued string attributes, the search finds substrings in the values.</span></span>  
<span data-ttu-id="e07f2-406">Pro atributy typu odkaz vyhledávaná řetězec musí přesně shodovat hodnota, která má být považovány za shodné.</span><span class="sxs-lookup"><span data-stu-id="e07f2-406">For reference attributes, the searched string must exactly match the value to be considered a match.</span></span>

<span data-ttu-id="e07f2-407">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-407">**Example:**</span></span>  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
<span data-ttu-id="e07f2-408">Pokud má atribut proxyAddresses primární e-mailovou adresu (indikován velká písmena "SMTP:"), pak vrátit atribut proxyAddress, jinak vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-408">If the proxyAddresses attribute has a primary email address (indicated by uppercase "SMTP:"), then return the proxyAddress attribute, else return an error.</span></span>

- - -
### <a name="convertfrombase64"></a><span data-ttu-id="e07f2-409">ConvertFromBase64</span><span class="sxs-lookup"><span data-stu-id="e07f2-409">ConvertFromBase64</span></span>
<span data-ttu-id="e07f2-410">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-410">**Description:**</span></span>  
<span data-ttu-id="e07f2-411">Funkce ConvertFromBase64 převede hodnotu zadaného kódováním base64 regulární řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-411">The ConvertFromBase64 function converts the specified base64 encoded value to a regular string.</span></span>

<span data-ttu-id="e07f2-412">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-412">**Syntax:**</span></span>  
<span data-ttu-id="e07f2-413">`str ConvertFromBase64(str source)`-předpokládá pro kódování Unicode</span><span class="sxs-lookup"><span data-stu-id="e07f2-413">`str ConvertFromBase64(str source)` - assumes Unicode for encoding</span></span>  
`str ConvertFromBase64(str source, enum Encoding)`

* <span data-ttu-id="e07f2-414">Zdroj: řetězec kódování Base64</span><span class="sxs-lookup"><span data-stu-id="e07f2-414">source: Base64 encoded string</span></span>  
* <span data-ttu-id="e07f2-415">Kódování: Znakové sady Unicode, ASCII, UTF8</span><span class="sxs-lookup"><span data-stu-id="e07f2-415">Encoding: Unicode, ASCII, UTF8</span></span>

<span data-ttu-id="e07f2-416">**Příklad**</span><span class="sxs-lookup"><span data-stu-id="e07f2-416">**Example**</span></span>  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

<span data-ttu-id="e07f2-417">Vrátí oba příklady "*Hello, world!*"</span><span class="sxs-lookup"><span data-stu-id="e07f2-417">Both examples return "*Hello world!*"</span></span>

- - -
### <a name="convertfromutf8hex"></a><span data-ttu-id="e07f2-418">ConvertFromUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="e07f2-418">ConvertFromUTF8Hex</span></span>
<span data-ttu-id="e07f2-419">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-419">**Description:**</span></span>  
<span data-ttu-id="e07f2-420">Funkce ConvertFromUTF8Hex převede zadanou hodnotu UTF8 šestnáctkově kódovaný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-420">The ConvertFromUTF8Hex function converts the specified UTF8 Hex encoded value to a string.</span></span>

<span data-ttu-id="e07f2-421">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-421">**Syntax:**</span></span>  
`str ConvertFromUTF8Hex(str source)`

* <span data-ttu-id="e07f2-422">Zdroj: UTF8 2bajtová kódovaného palčivost</span><span class="sxs-lookup"><span data-stu-id="e07f2-422">source: UTF8 2-byte encoded sting</span></span>

<span data-ttu-id="e07f2-423">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-423">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-424">Rozdíl mezi tato funkce a ConvertFromBase64([],UTF8) v tom, že výsledkem je popisný rozlišující název atributu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-424">The difference between this function and ConvertFromBase64([],UTF8) in that the result is friendly for the DN attribute.</span></span>  
<span data-ttu-id="e07f2-425">Tento formát se službou Azure Active Directory používá jako rozlišující název.</span><span class="sxs-lookup"><span data-stu-id="e07f2-425">This format is used by Azure Active Directory as DN.</span></span>

<span data-ttu-id="e07f2-426">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-426">**Example:**</span></span>  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
<span data-ttu-id="e07f2-427">Vrátí "*Hello, world!*"</span><span class="sxs-lookup"><span data-stu-id="e07f2-427">Returns "*Hello world!*"</span></span>

- - -
### <a name="converttobase64"></a><span data-ttu-id="e07f2-428">ConvertToBase64</span><span class="sxs-lookup"><span data-stu-id="e07f2-428">ConvertToBase64</span></span>
<span data-ttu-id="e07f2-429">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-429">**Description:**</span></span>  
<span data-ttu-id="e07f2-430">Funkce ConvertToBase64 převede řetězec na řetězec ve formátu base64.</span><span class="sxs-lookup"><span data-stu-id="e07f2-430">The ConvertToBase64 function converts a string to a Unicode base64 string.</span></span>  
<span data-ttu-id="e07f2-431">Převede hodnotu pole celých čísel na jeho ekvivalentní řetězcovou reprezentaci, který je zakódován pomocí kódování base-64 číslic.</span><span class="sxs-lookup"><span data-stu-id="e07f2-431">Converts the value of an array of integers to its equivalent string representation that is encoded with base-64 digits.</span></span>

<span data-ttu-id="e07f2-432">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-432">**Syntax:**</span></span>  
`str ConvertToBase64(str source)`

<span data-ttu-id="e07f2-433">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-433">**Example:**</span></span>  
`ConvertToBase64("Hello world!")`  
<span data-ttu-id="e07f2-434">Vrátí "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span><span class="sxs-lookup"><span data-stu-id="e07f2-434">Returns "SABlAGwAbABvACAAdwBvAHIAbABkACEA"</span></span>

- - -
### <a name="converttoutf8hex"></a><span data-ttu-id="e07f2-435">ConvertToUTF8Hex</span><span class="sxs-lookup"><span data-stu-id="e07f2-435">ConvertToUTF8Hex</span></span>
<span data-ttu-id="e07f2-436">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-436">**Description:**</span></span>  
<span data-ttu-id="e07f2-437">Funkce ConvertToUTF8Hex převede řetězec na hodnotu UTF8 šestnáctkově kódování.</span><span class="sxs-lookup"><span data-stu-id="e07f2-437">The ConvertToUTF8Hex function converts a string to a UTF8 Hex encoded value.</span></span>

<span data-ttu-id="e07f2-438">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-438">**Syntax:**</span></span>  
`str ConvertToUTF8Hex(str source)`

<span data-ttu-id="e07f2-439">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-439">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-440">Výstupní formát této funkce se službou Azure Active Directory používá jako atribut formátu DN.</span><span class="sxs-lookup"><span data-stu-id="e07f2-440">The output format of this function is used by Azure Active Directory as DN attribute format.</span></span>

<span data-ttu-id="e07f2-441">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-441">**Example:**</span></span>  
`ConvertToUTF8Hex("Hello world!")`  
<span data-ttu-id="e07f2-442">Vrátí 48656C6C6F20776F726C6421</span><span class="sxs-lookup"><span data-stu-id="e07f2-442">Returns 48656C6C6F20776F726C6421</span></span>

- - -
### <a name="count"></a><span data-ttu-id="e07f2-443">Počet</span><span class="sxs-lookup"><span data-stu-id="e07f2-443">Count</span></span>
<span data-ttu-id="e07f2-444">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-444">**Description:**</span></span>  
<span data-ttu-id="e07f2-445">Funkce Count vrátí počet elementů ve více hodnotami atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-445">The Count function returns the number of elements in a multi-valued attribute</span></span>

<span data-ttu-id="e07f2-446">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-446">**Syntax:**</span></span>  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a><span data-ttu-id="e07f2-447">CNum</span><span class="sxs-lookup"><span data-stu-id="e07f2-447">CNum</span></span>
<span data-ttu-id="e07f2-448">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-448">**Description:**</span></span>  
<span data-ttu-id="e07f2-449">Funkce CNum přebírá řetězec a vrátí číselným datovým typem.</span><span class="sxs-lookup"><span data-stu-id="e07f2-449">The CNum function takes a string and returns a numeric data type.</span></span>

<span data-ttu-id="e07f2-450">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-450">**Syntax:**</span></span>  
`num CNum(str value)`

- - -
### <a name="cref"></a><span data-ttu-id="e07f2-451">CRef –</span><span class="sxs-lookup"><span data-stu-id="e07f2-451">CRef</span></span>
<span data-ttu-id="e07f2-452">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-452">**Description:**</span></span>  
<span data-ttu-id="e07f2-453">Převede řetězec na atribut typu odkaz</span><span class="sxs-lookup"><span data-stu-id="e07f2-453">Converts a string to a reference attribute</span></span>

<span data-ttu-id="e07f2-454">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-454">**Syntax:**</span></span>  
`ref CRef(str value)`

<span data-ttu-id="e07f2-455">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-455">**Example:**</span></span>  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a><span data-ttu-id="e07f2-456">CStr</span><span class="sxs-lookup"><span data-stu-id="e07f2-456">CStr</span></span>
<span data-ttu-id="e07f2-457">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-457">**Description:**</span></span>  
<span data-ttu-id="e07f2-458">Funkci CStr převede na datový typ řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-458">The CStr function converts to a string data type.</span></span>

<span data-ttu-id="e07f2-459">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-459">**Syntax:**</span></span>  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* <span data-ttu-id="e07f2-460">hodnota: může být číselnou hodnotu, atribut odkaz nebo logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="e07f2-460">value: Can be a numeric value, reference attribute, or Boolean.</span></span>

<span data-ttu-id="e07f2-461">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-461">**Example:**</span></span>  
`CStr([dn])`  
<span data-ttu-id="e07f2-462">Může vrátit "cn = Jan, dc = contoso, dc = com"</span><span class="sxs-lookup"><span data-stu-id="e07f2-462">Could return "cn=Joe,dc=contoso,dc=com"</span></span>

- - -
### <a name="dateadd"></a><span data-ttu-id="e07f2-463">Funkce DateAdd</span><span class="sxs-lookup"><span data-stu-id="e07f2-463">DateAdd</span></span>
<span data-ttu-id="e07f2-464">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-464">**Description:**</span></span>  
<span data-ttu-id="e07f2-465">Vrátí datum obsahující datum, do které byl přidán zadaného časového intervalu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-465">Returns a Date containing a date to which a specified time interval has been added.</span></span>

<span data-ttu-id="e07f2-466">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-466">**Syntax:**</span></span>  
`dt DateAdd(str interval, num value, dt date)`

* <span data-ttu-id="e07f2-467">interval: řetězec výraz, který je interval času, který chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="e07f2-467">interval: String expression that is the interval of time you want to add.</span></span> <span data-ttu-id="e07f2-468">Řetězec musí mít jednu z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="e07f2-468">The string must have one of the following values:</span></span>
  * <span data-ttu-id="e07f2-469">rrrr roku</span><span class="sxs-lookup"><span data-stu-id="e07f2-469">yyyy Year</span></span>
  * <span data-ttu-id="e07f2-470">q čtvrtletí.</span><span class="sxs-lookup"><span data-stu-id="e07f2-470">q Quarter</span></span>
  * <span data-ttu-id="e07f2-471">m měsíc</span><span class="sxs-lookup"><span data-stu-id="e07f2-471">m Month</span></span>
  * <span data-ttu-id="e07f2-472">y den roku</span><span class="sxs-lookup"><span data-stu-id="e07f2-472">y Day of year</span></span>
  * <span data-ttu-id="e07f2-473">d den</span><span class="sxs-lookup"><span data-stu-id="e07f2-473">d Day</span></span>
  * <span data-ttu-id="e07f2-474">w den v týdnu</span><span class="sxs-lookup"><span data-stu-id="e07f2-474">w Weekday</span></span>
  * <span data-ttu-id="e07f2-475">TT týden</span><span class="sxs-lookup"><span data-stu-id="e07f2-475">ww Week</span></span>
  * <span data-ttu-id="e07f2-476">h hodinu</span><span class="sxs-lookup"><span data-stu-id="e07f2-476">h Hour</span></span>
  * <span data-ttu-id="e07f2-477">n minutu</span><span class="sxs-lookup"><span data-stu-id="e07f2-477">n Minute</span></span>
  * <span data-ttu-id="e07f2-478">s druhou</span><span class="sxs-lookup"><span data-stu-id="e07f2-478">s Second</span></span>
* <span data-ttu-id="e07f2-479">hodnota: počet jednotek, které chcete přidat.</span><span class="sxs-lookup"><span data-stu-id="e07f2-479">value: The number of units you want to add.</span></span> <span data-ttu-id="e07f2-480">Může být (Chcete-li získat data v budoucnosti) kladné a záporné (Chcete-li získat data v minulosti).</span><span class="sxs-lookup"><span data-stu-id="e07f2-480">It can be positive (to get dates in the future) or negative (to get dates in the past).</span></span>
* <span data-ttu-id="e07f2-481">datum: data a času představující datum, ke kterému se přidá interval.</span><span class="sxs-lookup"><span data-stu-id="e07f2-481">date: DateTime representing date to which the interval is added.</span></span>

<span data-ttu-id="e07f2-482">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-482">**Example:**</span></span>  
`DateAdd("m", 3, CDate("2001-01-01"))`  
<span data-ttu-id="e07f2-483">Přidá 3 měsíce a vrátí hodnotu DateTime představující "2001-04-01".</span><span class="sxs-lookup"><span data-stu-id="e07f2-483">Adds 3 months and returns a DateTime representing "2001-04-01".</span></span>

- - -
### <a name="datefromnum"></a><span data-ttu-id="e07f2-484">DateFromNum</span><span class="sxs-lookup"><span data-stu-id="e07f2-484">DateFromNum</span></span>
<span data-ttu-id="e07f2-485">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-485">**Description:**</span></span>  
<span data-ttu-id="e07f2-486">Funkce DateFromNum převádí na hodnotu v AD na datum formátu na typ DateTime.</span><span class="sxs-lookup"><span data-stu-id="e07f2-486">The DateFromNum function converts a value in AD’s date format to a DateTime type.</span></span>

<span data-ttu-id="e07f2-487">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-487">**Syntax:**</span></span>  
`dt DateFromNum(num value)`

<span data-ttu-id="e07f2-488">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-488">**Example:**</span></span>  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
<span data-ttu-id="e07f2-489">Vrací hodnotu DateTime představující 2012-01-01 23:00:00</span><span class="sxs-lookup"><span data-stu-id="e07f2-489">Returns a DateTime representing 2012-01-01 23:00:00</span></span>

- - -
### <a name="dncomponent"></a><span data-ttu-id="e07f2-490">DNComponent</span><span class="sxs-lookup"><span data-stu-id="e07f2-490">DNComponent</span></span>
<span data-ttu-id="e07f2-491">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-491">**Description:**</span></span>  
<span data-ttu-id="e07f2-492">Funkce DNComponent vrací hodnotu zadaného rozlišující název součásti budete zleva.</span><span class="sxs-lookup"><span data-stu-id="e07f2-492">The DNComponent function returns the value of a specified DN component going from left.</span></span>

<span data-ttu-id="e07f2-493">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-493">**Syntax:**</span></span>  
`str DNComponent(ref dn, num ComponentNumber)`

* <span data-ttu-id="e07f2-494">rozlišující název: odkaz na atribut interpretovat</span><span class="sxs-lookup"><span data-stu-id="e07f2-494">dn: the reference attribute to interpret</span></span>
* <span data-ttu-id="e07f2-495">ComponentNumber: Součástí DN vrátit</span><span class="sxs-lookup"><span data-stu-id="e07f2-495">ComponentNumber: The component in the DN to return</span></span>

<span data-ttu-id="e07f2-496">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-496">**Example:**</span></span>  
`DNComponent([dn],1)`  
<span data-ttu-id="e07f2-497">Pokud je rozlišující název "cn = Jan, ou =...," vrátí Jan</span><span class="sxs-lookup"><span data-stu-id="e07f2-497">If dn is "cn=Joe,ou=…," it returns Joe</span></span>

- - -
### <a name="dncomponentrev"></a><span data-ttu-id="e07f2-498">DNComponentRev</span><span class="sxs-lookup"><span data-stu-id="e07f2-498">DNComponentRev</span></span>
<span data-ttu-id="e07f2-499">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-499">**Description:**</span></span>  
<span data-ttu-id="e07f2-500">Funkce DNComponentRev vrací hodnotu zadaného rozlišující název složky přecházející z pravé (end).</span><span class="sxs-lookup"><span data-stu-id="e07f2-500">The DNComponentRev function returns the value of a specified DN component going from right (the end).</span></span>

<span data-ttu-id="e07f2-501">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-501">**Syntax:**</span></span>  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* <span data-ttu-id="e07f2-502">rozlišující název: odkaz na atribut interpretovat</span><span class="sxs-lookup"><span data-stu-id="e07f2-502">dn: the reference attribute to interpret</span></span>
* <span data-ttu-id="e07f2-503">ComponentNumber - součástí DN vrátit</span><span class="sxs-lookup"><span data-stu-id="e07f2-503">ComponentNumber - The component in the DN to return</span></span>
* <span data-ttu-id="e07f2-504">Možnosti: Řadič domény – ignorovat všechny součásti s "dc ="</span><span class="sxs-lookup"><span data-stu-id="e07f2-504">Options: DC – Ignore all components with "dc="</span></span>

<span data-ttu-id="e07f2-505">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-505">**Example:**</span></span>  
<span data-ttu-id="e07f2-506">Pokud rozlišující název "cn Jan, ou = Atlantu, ou = GA, ou = = USA, řadič domény = contoso, dc = com" pak</span><span class="sxs-lookup"><span data-stu-id="e07f2-506">If dn is "cn=Joe,ou=Atlanta,ou=GA,ou=US, dc=contoso,dc=com" then</span></span>  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
<span data-ttu-id="e07f2-507">Obě vrací nás.</span><span class="sxs-lookup"><span data-stu-id="e07f2-507">Both return US.</span></span>

- - -
### <a name="error"></a><span data-ttu-id="e07f2-508">Chyba</span><span class="sxs-lookup"><span data-stu-id="e07f2-508">Error</span></span>
<span data-ttu-id="e07f2-509">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-509">**Description:**</span></span>  
<span data-ttu-id="e07f2-510">Chyba funkce se používá k vrácení o vlastní chybě.</span><span class="sxs-lookup"><span data-stu-id="e07f2-510">The Error function is used to return a custom error.</span></span>

<span data-ttu-id="e07f2-511">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-511">**Syntax:**</span></span>  
`void Error(str ErrorMessage)`

<span data-ttu-id="e07f2-512">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-512">**Example:**</span></span>  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
<span data-ttu-id="e07f2-513">Pokud atribut accountName není k dispozici, vyvolá chybu v objektu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-513">If the attribute accountName is not present, throw an error on the object.</span></span>

- - -
### <a name="escapedncomponent"></a><span data-ttu-id="e07f2-514">EscapeDNComponent</span><span class="sxs-lookup"><span data-stu-id="e07f2-514">EscapeDNComponent</span></span>
<span data-ttu-id="e07f2-515">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-515">**Description:**</span></span>  
<span data-ttu-id="e07f2-516">Funkce EscapeDNComponent použije jednu součást názvu domény a řídicí sekvence, může být reprezentován v protokolu LDAP.</span><span class="sxs-lookup"><span data-stu-id="e07f2-516">The EscapeDNComponent function takes one component of a DN and escapes it so it can be represented in LDAP.</span></span>

<span data-ttu-id="e07f2-517">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-517">**Syntax:**</span></span>  
`str EscapeDNComponent(str value)`

<span data-ttu-id="e07f2-518">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-518">**Example:**</span></span>  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
<span data-ttu-id="e07f2-519">Zajistí, že objekt lze vytvořit v adresáři LDAP, i v případě, že atribut displayName obsahuje znaky, které je nutné uvést v protokolu LDAP.</span><span class="sxs-lookup"><span data-stu-id="e07f2-519">Makes sure the object can be created in an LDAP directory even if the displayName attribute has characters that must be escaped in LDAP.</span></span>

- - -
### <a name="formatdatetime"></a><span data-ttu-id="e07f2-520">FormatDateTime</span><span class="sxs-lookup"><span data-stu-id="e07f2-520">FormatDateTime</span></span>
<span data-ttu-id="e07f2-521">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-521">**Description:**</span></span>  
<span data-ttu-id="e07f2-522">Funkce FormatDateTime slouží k formátování hodnotu DateTime na řetězec s zadaný formát</span><span class="sxs-lookup"><span data-stu-id="e07f2-522">The FormatDateTime function is used to format a DateTime to a string with a specified format</span></span>

<span data-ttu-id="e07f2-523">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-523">**Syntax:**</span></span>  
`str FormatDateTime(dt value, str format)`

* <span data-ttu-id="e07f2-524">hodnota: hodnotu ve formátu data a času</span><span class="sxs-lookup"><span data-stu-id="e07f2-524">value: a value in the DateTime format</span></span>
* <span data-ttu-id="e07f2-525">formát: řetězec představující převést na formát.</span><span class="sxs-lookup"><span data-stu-id="e07f2-525">format: a string representing the format to convert to.</span></span>

<span data-ttu-id="e07f2-526">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-526">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-527">Možné hodnoty pro formát naleznete zde: [uživatelem definované formátu data a času (formát funkce)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span><span class="sxs-lookup"><span data-stu-id="e07f2-527">The possible values for the format can be found here: [User-Defined Date/Time Formats (Format Function)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)</span></span>

<span data-ttu-id="e07f2-528">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-528">**Example:**</span></span>  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
<span data-ttu-id="e07f2-529">Výsledkem "2007-12-25".</span><span class="sxs-lookup"><span data-stu-id="e07f2-529">Results in "2007-12-25".</span></span>

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
<span data-ttu-id="e07f2-530">Může mít za následek "20140905081453.0Z"</span><span class="sxs-lookup"><span data-stu-id="e07f2-530">Can result in "20140905081453.0Z"</span></span>

- - -
### <a name="guid"></a><span data-ttu-id="e07f2-531">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="e07f2-531">GUID</span></span>
<span data-ttu-id="e07f2-532">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-532">**Description:**</span></span>  
<span data-ttu-id="e07f2-533">Funkce GUID se generuje nový náhodný identifikátor GUID</span><span class="sxs-lookup"><span data-stu-id="e07f2-533">The function GUID generates a new random GUID</span></span>

<span data-ttu-id="e07f2-534">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-534">**Syntax:**</span></span>  
`str GUID()`

- - -
### <a name="iif"></a><span data-ttu-id="e07f2-535">IIF</span><span class="sxs-lookup"><span data-stu-id="e07f2-535">IIF</span></span>
<span data-ttu-id="e07f2-536">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-536">**Description:**</span></span>  
<span data-ttu-id="e07f2-537">Funkce IIF vrátí jednu z možných hodnot na základě podmínky pro zadanou sadu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-537">The IIF function returns one of a set of possible values based on a specified condition.</span></span>

<span data-ttu-id="e07f2-538">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-538">**Syntax:**</span></span>  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* <span data-ttu-id="e07f2-539">Podmínka: všechny hodnoty nebo výrazy, které lze vyhodnotit na hodnotu true nebo false.</span><span class="sxs-lookup"><span data-stu-id="e07f2-539">condition: any value or expression that can be evaluated to true or false.</span></span>
* <span data-ttu-id="e07f2-540">valueIfTrue: Pokud je podmínka vyhodnocena jako true, vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e07f2-540">valueIfTrue: If the condition evaluates to true, the returned value.</span></span>
* <span data-ttu-id="e07f2-541">valueIfFalse: Pokud je podmínka vyhodnocena jako false, vrácené hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e07f2-541">valueIfFalse: If the condition evaluates to false, the returned value.</span></span>

<span data-ttu-id="e07f2-542">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-542">**Example:**</span></span>  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 <span data-ttu-id="e07f2-543">Pokud je uživatel učně, vrátí vrátí alias uživatele s "t-" přidat na začátek ho jiný alias uživatele, jako je.</span><span class="sxs-lookup"><span data-stu-id="e07f2-543">If the user is an intern, returns the alias of a user with "t-" added to the beginning of it, else returns the user’s alias as is.</span></span>

- - -
### <a name="instr"></a><span data-ttu-id="e07f2-544">InStr</span><span class="sxs-lookup"><span data-stu-id="e07f2-544">InStr</span></span>
<span data-ttu-id="e07f2-545">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-545">**Description:**</span></span>  
<span data-ttu-id="e07f2-546">Funkce InStr první výskyt je dílčí řetězec najde v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-546">The InStr function finds the first occurrence of a substring in a string</span></span>

<span data-ttu-id="e07f2-547">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-547">**Syntax:**</span></span>  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* <span data-ttu-id="e07f2-548">prohledávaný_řetězec: řetězec, který má být prohledán</span><span class="sxs-lookup"><span data-stu-id="e07f2-548">stringcheck: string to be searched</span></span>
* <span data-ttu-id="e07f2-549">hledaný_řetězec: řetězec, který má být nalezena</span><span class="sxs-lookup"><span data-stu-id="e07f2-549">stringmatch: string to be found</span></span>
* <span data-ttu-id="e07f2-550">spustit: počáteční pozice k vyhledání dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="e07f2-550">start: starting position to find the substring</span></span>
* <span data-ttu-id="e07f2-551">porovnání: vbTextCompare nebo vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="e07f2-551">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="e07f2-552">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-552">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-553">Vrátí pozice, kdy nebyl nalezen dílčí řetězec, nebo 0, pokud není nalezena.</span><span class="sxs-lookup"><span data-stu-id="e07f2-553">Returns the position where the substring was found or 0 if not found.</span></span>

<span data-ttu-id="e07f2-554">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-554">**Example:**</span></span>  
`InStr("The quick brown fox","quick")`  
<span data-ttu-id="e07f2-555">Hodnoty 5</span><span class="sxs-lookup"><span data-stu-id="e07f2-555">Evalues to 5</span></span>

`InStr("repEated","e",3,vbBinaryCompare)`  
<span data-ttu-id="e07f2-556">Vyhodnotí 7</span><span class="sxs-lookup"><span data-stu-id="e07f2-556">Evaluates to 7</span></span>

- - -
### <a name="instrrev"></a><span data-ttu-id="e07f2-557">InStrRev</span><span class="sxs-lookup"><span data-stu-id="e07f2-557">InStrRev</span></span>
<span data-ttu-id="e07f2-558">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-558">**Description:**</span></span>  
<span data-ttu-id="e07f2-559">Funkce InStrRev posledního výskytu dílčí řetězec najde v řetězci</span><span class="sxs-lookup"><span data-stu-id="e07f2-559">The InStrRev function finds the last occurrence of a substring in a string</span></span>

<span data-ttu-id="e07f2-560">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-560">**Syntax:**</span></span>  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* <span data-ttu-id="e07f2-561">prohledávaný_řetězec: řetězec, který má být prohledán</span><span class="sxs-lookup"><span data-stu-id="e07f2-561">stringcheck: string to be searched</span></span>
* <span data-ttu-id="e07f2-562">hledaný_řetězec: řetězec, který má být nalezena</span><span class="sxs-lookup"><span data-stu-id="e07f2-562">stringmatch: string to be found</span></span>
* <span data-ttu-id="e07f2-563">spustit: počáteční pozice k vyhledání dílčí řetězec</span><span class="sxs-lookup"><span data-stu-id="e07f2-563">start: starting position to find the substring</span></span>
* <span data-ttu-id="e07f2-564">porovnání: vbTextCompare nebo vbBinaryCompare</span><span class="sxs-lookup"><span data-stu-id="e07f2-564">compare: vbTextCompare or vbBinaryCompare</span></span>

<span data-ttu-id="e07f2-565">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-565">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-566">Vrátí pozice, kdy nebyl nalezen dílčí řetězec, nebo 0, pokud není nalezena.</span><span class="sxs-lookup"><span data-stu-id="e07f2-566">Returns the position where the substring was found or 0 if not found.</span></span>

<span data-ttu-id="e07f2-567">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-567">**Example:**</span></span>  
`InStrRev("abbcdbbbef","bb")`  
<span data-ttu-id="e07f2-568">Vrátí hodnotu 7</span><span class="sxs-lookup"><span data-stu-id="e07f2-568">Returns 7</span></span>

- - -
### <a name="isbitset"></a><span data-ttu-id="e07f2-569">IsBitSet</span><span class="sxs-lookup"><span data-stu-id="e07f2-569">IsBitSet</span></span>
<span data-ttu-id="e07f2-570">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-570">**Description:**</span></span>  
<span data-ttu-id="e07f2-571">Funkce IsBitSet testů, pokud chvíli je nastavena, nebo ne</span><span class="sxs-lookup"><span data-stu-id="e07f2-571">The function IsBitSet Tests if a bit is set or not</span></span>

<span data-ttu-id="e07f2-572">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-572">**Syntax:**</span></span>  
`bool IsBitSet(num value, num flag)`

* <span data-ttu-id="e07f2-573">hodnota: číselnou hodnotu, která je evaluated.flag: číselnou hodnotu, která má bit, který se má vyhodnotit</span><span class="sxs-lookup"><span data-stu-id="e07f2-573">value: a numeric value that is evaluated.flag: a numeric value that has the bit to be evaluated</span></span>

<span data-ttu-id="e07f2-574">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-574">**Example:**</span></span>  
`IsBitSet(&HF,4)`  
<span data-ttu-id="e07f2-575">Vrátí hodnotu True, protože je nastaven bit "4" v šestnáctkové hodnoty "F"</span><span class="sxs-lookup"><span data-stu-id="e07f2-575">Returns True because bit "4" is set in the hexadecimal value "F"</span></span>

- - -
### <a name="isdate"></a><span data-ttu-id="e07f2-576">IsDate</span><span class="sxs-lookup"><span data-stu-id="e07f2-576">IsDate</span></span>
<span data-ttu-id="e07f2-577">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-577">**Description:**</span></span>  
<span data-ttu-id="e07f2-578">Pokud výraz může být vyhodnotí jako typ DateTime, pak Funkce IsDate vyhodnocen jako True.</span><span class="sxs-lookup"><span data-stu-id="e07f2-578">If the expression can be evaluates as a DateTime type, then the IsDate function evaluates to True.</span></span>

<span data-ttu-id="e07f2-579">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-579">**Syntax:**</span></span>  
`bool IsDate(var Expression)`

<span data-ttu-id="e07f2-580">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-580">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-581">Použít k určení, pokud CDate() může být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e07f2-581">Used to determine if CDate() can be successful.</span></span>

- - -
### <a name="iscert"></a><span data-ttu-id="e07f2-582">IsCert</span><span class="sxs-lookup"><span data-stu-id="e07f2-582">IsCert</span></span>
<span data-ttu-id="e07f2-583">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-583">**Description:**</span></span>  
<span data-ttu-id="e07f2-584">Vrátí hodnotu true Pokud serializovat lze nezpracovaná data do objektu certifikát .NET X509Certificate2.</span><span class="sxs-lookup"><span data-stu-id="e07f2-584">Returns true if the raw data can be serialized into .NET X509Certificate2 certificate object.</span></span>

<span data-ttu-id="e07f2-585">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-585">**Syntax:**</span></span>  
`bool CertThumbprint(binary certificateRawData)`  
*   <span data-ttu-id="e07f2-586">certificateRawData: reprezentace bajtové pole certifikátu X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-586">certificateRawData: Byte array representation of an X.509 certificate.</span></span> <span data-ttu-id="e07f2-587">Bajtové pole může být kódovaný binárního souboru (DER) nebo data s kódováním Base64 X.509.</span><span class="sxs-lookup"><span data-stu-id="e07f2-587">The byte array can be binary (DER) encoded or Base64-encoded X.509 data.</span></span>
- - -
### <a name="isempty"></a><span data-ttu-id="e07f2-588">IsEmpty –</span><span class="sxs-lookup"><span data-stu-id="e07f2-588">IsEmpty</span></span>
<span data-ttu-id="e07f2-589">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-589">**Description:**</span></span>  
<span data-ttu-id="e07f2-590">Pokud atribut je k dispozici v CS nebo více hodnot, ale vyhodnocen jako prázdný řetězec, IsEmpty – funkce vyhodnotí na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="e07f2-590">If the attribute is present in the CS or MV but evaluates to an empty string, then the IsEmpty function evaluates to True.</span></span>

<span data-ttu-id="e07f2-591">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-591">**Syntax:**</span></span>  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a><span data-ttu-id="e07f2-592">IsGuid</span><span class="sxs-lookup"><span data-stu-id="e07f2-592">IsGuid</span></span>
<span data-ttu-id="e07f2-593">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-593">**Description:**</span></span>  
<span data-ttu-id="e07f2-594">Pokud řetězec může převést na identifikátor GUID, funkce IsGuid vyhodnocena jako true.</span><span class="sxs-lookup"><span data-stu-id="e07f2-594">If the string could be converted to a GUID, then the IsGuid function evaluated to true.</span></span>

<span data-ttu-id="e07f2-595">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-595">**Syntax:**</span></span>  
`bool IsGuid(str GUID)`

<span data-ttu-id="e07f2-596">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-596">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-597">Identifikátor GUID je definován jako jeden z těchto vzorů následující řetězec: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span><span class="sxs-lookup"><span data-stu-id="e07f2-597">A GUID is defined as a string following one of these patterns: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx or {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</span></span>

<span data-ttu-id="e07f2-598">Použít k určení, pokud CGuid() může být úspěšné.</span><span class="sxs-lookup"><span data-stu-id="e07f2-598">Used to determine if CGuid() can be successful.</span></span>

<span data-ttu-id="e07f2-599">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-599">**Example:**</span></span>  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
<span data-ttu-id="e07f2-600">Pokud StrAttribute má formát identifikátoru GUID, vrátit binární reprezentace, jinak vrátí hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="e07f2-600">If the StrAttribute has a GUID format, return a binary representation, otherwise return a Null.</span></span>

- - -
### <a name="isnull"></a><span data-ttu-id="e07f2-601">IsNull</span><span class="sxs-lookup"><span data-stu-id="e07f2-601">IsNull</span></span>
<span data-ttu-id="e07f2-602">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-602">**Description:**</span></span>  
<span data-ttu-id="e07f2-603">Pokud je výsledkem na hodnotu Null, vrátí hodnotu true Funkce IsNull.</span><span class="sxs-lookup"><span data-stu-id="e07f2-603">If the expression evaluates to Null, then the IsNull function returns true.</span></span>

<span data-ttu-id="e07f2-604">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-604">**Syntax:**</span></span>  
`bool IsNull(var Expression)`

<span data-ttu-id="e07f2-605">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-605">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-606">Pro atribut s hodnotou Null se vyjádří chybí atribut.</span><span class="sxs-lookup"><span data-stu-id="e07f2-606">For an attribute, a Null is expressed by the absence of the attribute.</span></span>

<span data-ttu-id="e07f2-607">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-607">**Example:**</span></span>  
`IsNull([displayName])`  
<span data-ttu-id="e07f2-608">Vrátí hodnotu True, pokud atribut neexistuje v CS nebo více hodnot.</span><span class="sxs-lookup"><span data-stu-id="e07f2-608">Returns True if the attribute is not present in the CS or MV.</span></span>

- - -
### <a name="isnullorempty"></a><span data-ttu-id="e07f2-609">IsNullOrEmpty</span><span class="sxs-lookup"><span data-stu-id="e07f2-609">IsNullOrEmpty</span></span>
<span data-ttu-id="e07f2-610">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-610">**Description:**</span></span>  
<span data-ttu-id="e07f2-611">Pokud výraz má hodnotu null nebo prázdný řetězec, IsNullOrEmpty funkce vrátí hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="e07f2-611">If the expression is null or an empty string, then the IsNullOrEmpty function returns true.</span></span>

<span data-ttu-id="e07f2-612">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-612">**Syntax:**</span></span>  
`bool IsNullOrEmpty(var Expression)`

<span data-ttu-id="e07f2-613">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-613">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-614">Pro atribut to by vyhodnotit na hodnotu True pokud atribut chybí nebo je k dispozici, ale prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-614">For an attribute, this would evaluate to True if the attribute is absent or is present but is an empty string.</span></span>  
<span data-ttu-id="e07f2-615">Inverzní této funkce je s názvem IsPresent.</span><span class="sxs-lookup"><span data-stu-id="e07f2-615">The inverse of this function is named IsPresent.</span></span>

<span data-ttu-id="e07f2-616">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-616">**Example:**</span></span>  
`IsNullOrEmpty([displayName])`  
<span data-ttu-id="e07f2-617">Vrátí hodnotu True, pokud atribut neexistuje, nebo je prázdný řetězec v CS nebo více hodnot.</span><span class="sxs-lookup"><span data-stu-id="e07f2-617">Returns True if the attribute is not present or is an empty string in the CS or MV.</span></span>

- - -
### <a name="isnumeric"></a><span data-ttu-id="e07f2-618">IsNumeric</span><span class="sxs-lookup"><span data-stu-id="e07f2-618">IsNumeric</span></span>
<span data-ttu-id="e07f2-619">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-619">**Description:**</span></span>  
<span data-ttu-id="e07f2-620">Funkce IsNumeric vrací logickou hodnotu udávající, zda výraz může být vyhodnocen jako typ number.</span><span class="sxs-lookup"><span data-stu-id="e07f2-620">The IsNumeric function returns a Boolean value indicating whether an expression can be evaluated as a number type.</span></span>

<span data-ttu-id="e07f2-621">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-621">**Syntax:**</span></span>  
`bool IsNumeric(var Expression)`

<span data-ttu-id="e07f2-622">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-622">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-623">Použít k určení, pokud CNum() může být úspěšné analyzovat výraz.</span><span class="sxs-lookup"><span data-stu-id="e07f2-623">Used to determine if CNum() can be successful to parse the expression.</span></span>

- - -
### <a name="isstring"></a><span data-ttu-id="e07f2-624">IsString</span><span class="sxs-lookup"><span data-stu-id="e07f2-624">IsString</span></span>
<span data-ttu-id="e07f2-625">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-625">**Description:**</span></span>  
<span data-ttu-id="e07f2-626">Pokud výraz může být vyhodnocen jako typ string, funkce IsString vyhodnocuje na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="e07f2-626">If the expression can be evaluated to a string type, then the IsString function evaluates to True.</span></span>

<span data-ttu-id="e07f2-627">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-627">**Syntax:**</span></span>  
`bool IsString(var expression)`

<span data-ttu-id="e07f2-628">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-628">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-629">Použít k určení, pokud CStr() může být úspěšné analyzovat výraz.</span><span class="sxs-lookup"><span data-stu-id="e07f2-629">Used to determine if CStr() can be successful to parse the expression.</span></span>

- - -
### <a name="ispresent"></a><span data-ttu-id="e07f2-630">IsPresent</span><span class="sxs-lookup"><span data-stu-id="e07f2-630">IsPresent</span></span>
<span data-ttu-id="e07f2-631">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-631">**Description:**</span></span>  
<span data-ttu-id="e07f2-632">Pokud je výsledkem na řetězec, který není Null a není prázdný, vrátí funkce IsPresent hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="e07f2-632">If the expression evaluates to a string that is not Null and is not empty, then the IsPresent function returns true.</span></span>

<span data-ttu-id="e07f2-633">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-633">**Syntax:**</span></span>  
`bool IsPresent(var expression)`

<span data-ttu-id="e07f2-634">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-634">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-635">Inverzní této funkce je s názvem IsNullOrEmpty.</span><span class="sxs-lookup"><span data-stu-id="e07f2-635">The inverse of this function is named IsNullOrEmpty.</span></span>

<span data-ttu-id="e07f2-636">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-636">**Example:**</span></span>  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a><span data-ttu-id="e07f2-637">Položka</span><span class="sxs-lookup"><span data-stu-id="e07f2-637">Item</span></span>
<span data-ttu-id="e07f2-638">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-638">**Description:**</span></span>  
<span data-ttu-id="e07f2-639">Funkce Item vrátí jednu položku z více hodnot řetězec nebo atributu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-639">The Item function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="e07f2-640">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-640">**Syntax:**</span></span>  
`var Item(mvstr attribute, num index)`

* <span data-ttu-id="e07f2-641">Atribut: vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-641">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="e07f2-642">index: index položky v řetězci s více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="e07f2-642">index: index to an item in the multi-valued string.</span></span>

<span data-ttu-id="e07f2-643">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-643">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-644">Funkce Item je užitečné společně s obsahuje funkce, protože pozdější funkce vrátí index položky v více hodnotami atributu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-644">The Item function is useful together with the Contains function since the latter function returns the index to an item in the multi-valued attribute.</span></span>

<span data-ttu-id="e07f2-645">Vrátí chybu, pokud index je mimo rozsah.</span><span class="sxs-lookup"><span data-stu-id="e07f2-645">Throws an error if index is out of bounds.</span></span>

<span data-ttu-id="e07f2-646">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-646">**Example:**</span></span>  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
<span data-ttu-id="e07f2-647">Vrátí primární e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-647">Returns the primary email address.</span></span>

- - -
### <a name="itemornull"></a><span data-ttu-id="e07f2-648">ItemOrNull</span><span class="sxs-lookup"><span data-stu-id="e07f2-648">ItemOrNull</span></span>
<span data-ttu-id="e07f2-649">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-649">**Description:**</span></span>  
<span data-ttu-id="e07f2-650">Funkce ItemOrNull vrací jednu položku z více hodnot řetězec nebo atributu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-650">The ItemOrNull function returns one item from a multi-valued string/attribute.</span></span>

<span data-ttu-id="e07f2-651">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-651">**Syntax:**</span></span>  
`var ItemOrNull(mvstr attribute, num index)`

* <span data-ttu-id="e07f2-652">Atribut: vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-652">attribute: multi-valued attribute</span></span>
* <span data-ttu-id="e07f2-653">index: index položky v řetězci s více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="e07f2-653">index: index to an item in the multi-valued string.</span></span>

<span data-ttu-id="e07f2-654">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-654">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-655">Funkce ItemOrNull je užitečné společně s obsahuje funkce, protože pozdější funkce vrátí index položky v více hodnotami atributu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-655">The ItemOrNull function is useful together with the Contains function since the latter function returns the index to an item in the multi-valued attribute.</span></span>

<span data-ttu-id="e07f2-656">Pokud index je mimo rozsah, vrátí hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="e07f2-656">If index is out of bounds, then returns a Null value.</span></span>

- - -
### <a name="join"></a><span data-ttu-id="e07f2-657">Spojit</span><span class="sxs-lookup"><span data-stu-id="e07f2-657">Join</span></span>
<span data-ttu-id="e07f2-658">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-658">**Description:**</span></span>  
<span data-ttu-id="e07f2-659">Funkce spojení přebírá řetězec s více hodnotami a vrátí řetězec s hodnotou jednoho zadaného oddělovače vložen mezi každou položku.</span><span class="sxs-lookup"><span data-stu-id="e07f2-659">The Join function takes a multi-valued string and returns a single-valued string with specified separator inserted between each item.</span></span>

<span data-ttu-id="e07f2-660">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-660">**Syntax:**</span></span>  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* <span data-ttu-id="e07f2-661">Atribut: více hodnot atributů obsahující řetězce, který se má spojit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-661">attribute: Multi-valued attribute containing strings to be joined.</span></span>
* <span data-ttu-id="e07f2-662">oddělovač: libovolný řetězec, který slouží k oddělení dílčích řetězců v vrácený řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-662">delimiter: Any string, used to separate the substrings in the returned string.</span></span> <span data-ttu-id="e07f2-663">Pokud tento parametr vynechán, znak mezery ("") se používá.</span><span class="sxs-lookup"><span data-stu-id="e07f2-663">If omitted, the space character (" ") is used.</span></span> <span data-ttu-id="e07f2-664">Pokud oddělovač obsahuje řetězec nulové délky ("") nebo Nothing, všechny položky v seznamu jsou zřetězeny žádný oddělovač.</span><span class="sxs-lookup"><span data-stu-id="e07f2-664">If Delimiter is a zero-length string ("") or Nothing, all items in the list are concatenated with no delimiters.</span></span>

<span data-ttu-id="e07f2-665">**Poznámky**</span><span class="sxs-lookup"><span data-stu-id="e07f2-665">**Remarks**</span></span>  
<span data-ttu-id="e07f2-666">Funkce připojení a rozdělení jsou rovnocenné.</span><span class="sxs-lookup"><span data-stu-id="e07f2-666">There is parity between the Join and Split functions.</span></span> <span data-ttu-id="e07f2-667">Funkce spojení trvá pole řetězců a připojí pomocí řetězec oddělovač vrátit jeden řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-667">The Join function takes an array of strings and joins them using a delimiter string, to return a single string.</span></span> <span data-ttu-id="e07f2-668">Funkce rozdělení přebírá řetězec a která ho odděluje v oddělovač vrátit pole řetězců.</span><span class="sxs-lookup"><span data-stu-id="e07f2-668">The Split function takes a string and separates it at the delimiter, to return an array of strings.</span></span> <span data-ttu-id="e07f2-669">Klíčovým rozdílem je ale, že připojení lze zřetězení řetězců s libovolným řetězcem oddělovač, rozdělení můžete oddělit pouze řetězců pomocí jednoho znaku oddělovač.</span><span class="sxs-lookup"><span data-stu-id="e07f2-669">However, a key difference is that Join can concatenate strings with any delimiter string, Split can only separate strings using a single character delimiter.</span></span>

<span data-ttu-id="e07f2-670">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-670">**Example:**</span></span>  
`Join([proxyAddresses],",")`  
<span data-ttu-id="e07f2-671">Může vrátit: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="e07f2-671">Could return: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"</span></span>

- - -
### <a name="lcase"></a><span data-ttu-id="e07f2-672">LCase</span><span class="sxs-lookup"><span data-stu-id="e07f2-672">LCase</span></span>
<span data-ttu-id="e07f2-673">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-673">**Description:**</span></span>  
<span data-ttu-id="e07f2-674">Funkce LCase převede všechny znaky v řetězci na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="e07f2-674">The LCase function converts all characters in a string to lower case.</span></span>

<span data-ttu-id="e07f2-675">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-675">**Syntax:**</span></span>  
`str LCase(str value)`

<span data-ttu-id="e07f2-676">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-676">**Example:**</span></span>  
`LCase("TeSt")`  
<span data-ttu-id="e07f2-677">Vrátí hodnotu "test".</span><span class="sxs-lookup"><span data-stu-id="e07f2-677">Returns "test".</span></span>

- - -
### <a name="left"></a><span data-ttu-id="e07f2-678">Vlevo</span><span class="sxs-lookup"><span data-stu-id="e07f2-678">Left</span></span>
<span data-ttu-id="e07f2-679">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-679">**Description:**</span></span>  
<span data-ttu-id="e07f2-680">Levé funkce vrátí zadaný počet znaků z řetězce levé straně.</span><span class="sxs-lookup"><span data-stu-id="e07f2-680">The Left function returns a specified number of characters from the left of a string.</span></span>

<span data-ttu-id="e07f2-681">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-681">**Syntax:**</span></span>  
`str Left(str string, num NumChars)`

* <span data-ttu-id="e07f2-682">řetězec: řetězec, který vrátí znaky z</span><span class="sxs-lookup"><span data-stu-id="e07f2-682">string: the string to return characters from</span></span>
* <span data-ttu-id="e07f2-683">NumChars: číslo určující počet znaků, mají být vráceny od začátku (vlevo) řetězce</span><span class="sxs-lookup"><span data-stu-id="e07f2-683">NumChars: a number identifying the number of characters to return from the beginning (left) of string</span></span>

<span data-ttu-id="e07f2-684">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-684">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-685">Řetězec obsahující první numChars znaky v řetězci:</span><span class="sxs-lookup"><span data-stu-id="e07f2-685">A string containing the first numChars characters in string:</span></span>

* <span data-ttu-id="e07f2-686">Pokud numChars = 0, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-686">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="e07f2-687">Pokud numChars < 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-687">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="e07f2-688">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-688">If string is null, return empty string.</span></span>

<span data-ttu-id="e07f2-689">Pokud řetězec obsahuje méně znaků, než číslo zadané v numChars, je vrácen řetězec identické řetězce, (který je, obsahující všechny znaky v parametru 1).</span><span class="sxs-lookup"><span data-stu-id="e07f2-689">If string contains fewer characters than the number specified in numChars, a string identical to string (that is, containing all characters in parameter 1) is returned.</span></span>

<span data-ttu-id="e07f2-690">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-690">**Example:**</span></span>  
`Left("John Doe", 3)`  
<span data-ttu-id="e07f2-691">Vrátí "Joh".</span><span class="sxs-lookup"><span data-stu-id="e07f2-691">Returns "Joh".</span></span>

- - -
### <a name="len"></a><span data-ttu-id="e07f2-692">Len</span><span class="sxs-lookup"><span data-stu-id="e07f2-692">Len</span></span>
<span data-ttu-id="e07f2-693">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-693">**Description:**</span></span>  
<span data-ttu-id="e07f2-694">Funkce Len vrátí počet znaků v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-694">The Len function returns number of characters in a string.</span></span>

<span data-ttu-id="e07f2-695">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-695">**Syntax:**</span></span>  
`num Len(str value)`

<span data-ttu-id="e07f2-696">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-696">**Example:**</span></span>  
`Len("John Doe")`  
<span data-ttu-id="e07f2-697">Vrátí 8</span><span class="sxs-lookup"><span data-stu-id="e07f2-697">Returns 8</span></span>

- - -
### <a name="ltrim"></a><span data-ttu-id="e07f2-698">LTrim</span><span class="sxs-lookup"><span data-stu-id="e07f2-698">LTrim</span></span>
<span data-ttu-id="e07f2-699">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-699">**Description:**</span></span>  
<span data-ttu-id="e07f2-700">Funkce LTrim odebere úvodní prázdné znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-700">The LTrim function removes leading white spaces from a string.</span></span>

<span data-ttu-id="e07f2-701">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-701">**Syntax:**</span></span>  
`str LTrim(str value)`

<span data-ttu-id="e07f2-702">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-702">**Example:**</span></span>  
`LTrim(" Test ")`  
<span data-ttu-id="e07f2-703">Vrátí "Test"</span><span class="sxs-lookup"><span data-stu-id="e07f2-703">Returns "Test "</span></span>

- - -
### <a name="mid"></a><span data-ttu-id="e07f2-704">Mid –</span><span class="sxs-lookup"><span data-stu-id="e07f2-704">Mid</span></span>
<span data-ttu-id="e07f2-705">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-705">**Description:**</span></span>  
<span data-ttu-id="e07f2-706">Funkci část vrátí zadaný počet znaků z určené pozice v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-706">The Mid function returns a specified number of characters from a specified position in a string.</span></span>

<span data-ttu-id="e07f2-707">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-707">**Syntax:**</span></span>  
`str Mid(str string, num start, num NumChars)`

* <span data-ttu-id="e07f2-708">řetězec: řetězec, který vrátí znaky z</span><span class="sxs-lookup"><span data-stu-id="e07f2-708">string: the string to return characters from</span></span>
* <span data-ttu-id="e07f2-709">spustit: číslo určující počáteční pozice v řetězci vrátit znaků z</span><span class="sxs-lookup"><span data-stu-id="e07f2-709">start: a number identifying the starting position in string to return characters from</span></span>
* <span data-ttu-id="e07f2-710">NumChars: číslo určující počet znaků k návratu z pozice v řetězci</span><span class="sxs-lookup"><span data-stu-id="e07f2-710">NumChars: a number identifying the number of characters to return from position in string</span></span>

<span data-ttu-id="e07f2-711">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-711">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-712">Vrátí numChars znaků od počáteční pozice v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-712">Return numChars characters starting from position start in string.</span></span>  
<span data-ttu-id="e07f2-713">Řetězec obsahující numChars znaků od počáteční pozice v řetězci:</span><span class="sxs-lookup"><span data-stu-id="e07f2-713">A string containing numChars characters from position start in string:</span></span>

* <span data-ttu-id="e07f2-714">Pokud numChars = 0, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-714">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="e07f2-715">Pokud numChars < 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-715">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="e07f2-716">Pokud spuštění > délky řetězce, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-716">If start > the length of string, return input string.</span></span>
* <span data-ttu-id="e07f2-717">Pokud spuštění < = 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-717">If start <= 0, return input string.</span></span>
* <span data-ttu-id="e07f2-718">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-718">If string is null, return empty string.</span></span>

<span data-ttu-id="e07f2-719">Pokud nejsou k dispozici numChar znaků zbývající v řetězci od počáteční pozice, jako jsou vráceny nejdříve mnoho znaků.</span><span class="sxs-lookup"><span data-stu-id="e07f2-719">If there are not numChar characters remaining in string from position start, as many characters as possible are returned.</span></span>

<span data-ttu-id="e07f2-720">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-720">**Example:**</span></span>  
`Mid("John Doe", 3, 5)`  
<span data-ttu-id="e07f2-721">Vrátí "hn proveďte".</span><span class="sxs-lookup"><span data-stu-id="e07f2-721">Returns "hn Do".</span></span>

`Mid("John Doe", 6, 999)`  
<span data-ttu-id="e07f2-722">Vrátí "Doe"</span><span class="sxs-lookup"><span data-stu-id="e07f2-722">Returns "Doe"</span></span>

- - -
### <a name="now"></a><span data-ttu-id="e07f2-723">Nyní</span><span class="sxs-lookup"><span data-stu-id="e07f2-723">Now</span></span>
<span data-ttu-id="e07f2-724">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-724">**Description:**</span></span>  
<span data-ttu-id="e07f2-725">Funkce nyní vrací hodnotu DateTime, zadáte aktuální datum a čas podle systémového data a času v počítači.</span><span class="sxs-lookup"><span data-stu-id="e07f2-725">The Now function returns a DateTime specifying the current date and time, according to your computer's system date and time.</span></span>

<span data-ttu-id="e07f2-726">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-726">**Syntax:**</span></span>  
`dt Now()`

- - -
### <a name="numfromdate"></a><span data-ttu-id="e07f2-727">NumFromDate</span><span class="sxs-lookup"><span data-stu-id="e07f2-727">NumFromDate</span></span>
<span data-ttu-id="e07f2-728">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-728">**Description:**</span></span>  
<span data-ttu-id="e07f2-729">Funkce NumFromDate vrátí datum ve formátu data na AD.</span><span class="sxs-lookup"><span data-stu-id="e07f2-729">The NumFromDate function returns a date in AD’s date format.</span></span>

<span data-ttu-id="e07f2-730">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-730">**Syntax:**</span></span>  
`num NumFromDate(dt value)`

<span data-ttu-id="e07f2-731">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-731">**Example:**</span></span>  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
<span data-ttu-id="e07f2-732">Vrátí 129699324000000000</span><span class="sxs-lookup"><span data-stu-id="e07f2-732">Returns 129699324000000000</span></span>

- - -
### <a name="padleft"></a><span data-ttu-id="e07f2-733">padLeft</span><span class="sxs-lookup"><span data-stu-id="e07f2-733">PadLeft</span></span>
<span data-ttu-id="e07f2-734">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-734">**Description:**</span></span>  
<span data-ttu-id="e07f2-735">PadLeft funkce doleva-dotyková zařízení řetězec na určenou délku pomocí zadané odsazení znaku.</span><span class="sxs-lookup"><span data-stu-id="e07f2-735">The PadLeft function left-pads a string to a specified length using a provided padding character.</span></span>

<span data-ttu-id="e07f2-736">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-736">**Syntax:**</span></span>  
`str PadLeft(str string, num length, str padCharacter)`

* <span data-ttu-id="e07f2-737">řetězec: řetězec k vyplnění.</span><span class="sxs-lookup"><span data-stu-id="e07f2-737">string: the string to pad.</span></span>
* <span data-ttu-id="e07f2-738">Délka: celé číslo představující požadovanou délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-738">length: An integer representing the desired length of string.</span></span>
* <span data-ttu-id="e07f2-739">padCharacter: řetězec, který se skládá z jednoho znaku používat jako znak odsazení</span><span class="sxs-lookup"><span data-stu-id="e07f2-739">padCharacter: A string consisting of a single character to use as the pad character</span></span>

<span data-ttu-id="e07f2-740">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-740">**Remarks:**</span></span>

* <span data-ttu-id="e07f2-741">Pokud délka řetězce je menší než délka, pak padCharacter opakovaně připojí se k začátku (vlevo) řetězce, dokud ho má délku rovna délce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-741">If the length of string is less than length, then padCharacter is repeatedly appended to the beginning (left) of string until it has a length equal to length.</span></span>
* <span data-ttu-id="e07f2-742">padCharacter může být znak mezery, ale jeho nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e07f2-742">PadCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="e07f2-743">Pokud délka řetězce je rovna nebo větší než délka, je vrácen řetězec s beze změny.</span><span class="sxs-lookup"><span data-stu-id="e07f2-743">If the length of string is equal to or greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="e07f2-744">Pokud řetězec má délku větší než nebo rovna hodnotě Délka, bude vrácena řetězec identické na řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-744">If string has a length greater than or equal to length, a string identical to string is returned.</span></span>
* <span data-ttu-id="e07f2-745">Pokud délka řetězce je menší než délka, bude vrácena nový řetězec má požadovanou délku obsahující řetězec doplněno padCharacter.</span><span class="sxs-lookup"><span data-stu-id="e07f2-745">If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="e07f2-746">Pokud má řetězec hodnotu null, funkce vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-746">If string is null, the function returns an empty string.</span></span>

<span data-ttu-id="e07f2-747">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-747">**Example:**</span></span>  
`PadLeft("User", 10, "0")`  
<span data-ttu-id="e07f2-748">Vrátí "000000User".</span><span class="sxs-lookup"><span data-stu-id="e07f2-748">Returns "000000User".</span></span>

- - -
### <a name="padright"></a><span data-ttu-id="e07f2-749">PadRight –</span><span class="sxs-lookup"><span data-stu-id="e07f2-749">PadRight</span></span>
<span data-ttu-id="e07f2-750">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-750">**Description:**</span></span>  
<span data-ttu-id="e07f2-751">PadRight – funkce vpravo-dotyková zařízení řetězec na určenou délku pomocí zadané odsazení znaku.</span><span class="sxs-lookup"><span data-stu-id="e07f2-751">The PadRight function right-pads a string to a specified length using a provided padding character.</span></span>

<span data-ttu-id="e07f2-752">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-752">**Syntax:**</span></span>  
`str PadRight(str string, num length, str padCharacter)`

* <span data-ttu-id="e07f2-753">řetězec: řetězec k vyplnění.</span><span class="sxs-lookup"><span data-stu-id="e07f2-753">string: the string to pad.</span></span>
* <span data-ttu-id="e07f2-754">Délka: celé číslo představující požadovanou délku řetězce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-754">length: An integer representing the desired length of string.</span></span>
* <span data-ttu-id="e07f2-755">padCharacter: řetězec, který se skládá z jednoho znaku používat jako znak odsazení</span><span class="sxs-lookup"><span data-stu-id="e07f2-755">padCharacter: A string consisting of a single character to use as the pad character</span></span>

<span data-ttu-id="e07f2-756">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-756">**Remarks:**</span></span>

* <span data-ttu-id="e07f2-757">Pokud délka řetězce je menší než délka, pak padCharacter opakovaně připojí se k konci (vpravo) řetězec dokud má délku rovna délce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-757">If the length of string is less than length, then padCharacter is repeatedly appended to the end (right) of string until it has a length equal to length.</span></span>
* <span data-ttu-id="e07f2-758">padCharacter může být znak mezery, ale jeho nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="e07f2-758">padCharacter can be a space character, but it cannot be a null value.</span></span>
* <span data-ttu-id="e07f2-759">Pokud délka řetězce je rovna nebo větší než délka, je vrácen řetězec s beze změny.</span><span class="sxs-lookup"><span data-stu-id="e07f2-759">If the length of string is equal to or greater than length, string is returned unchanged.</span></span>
* <span data-ttu-id="e07f2-760">Pokud řetězec má délku větší než nebo rovna hodnotě Délka, bude vrácena řetězec identické na řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-760">If string has a length greater than or equal to length, a string identical to string is returned.</span></span>
* <span data-ttu-id="e07f2-761">Pokud délka řetězce je menší než délka, bude vrácena nový řetězec má požadovanou délku obsahující řetězec doplněno padCharacter.</span><span class="sxs-lookup"><span data-stu-id="e07f2-761">If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter.</span></span>
* <span data-ttu-id="e07f2-762">Pokud má řetězec hodnotu null, funkce vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-762">If string is null, the function returns an empty string.</span></span>

<span data-ttu-id="e07f2-763">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-763">**Example:**</span></span>  
`PadRight("User", 10, "0")`  
<span data-ttu-id="e07f2-764">Vrátí "User000000".</span><span class="sxs-lookup"><span data-stu-id="e07f2-764">Returns "User000000".</span></span>

- - -
### <a name="pcase"></a><span data-ttu-id="e07f2-765">PCase</span><span class="sxs-lookup"><span data-stu-id="e07f2-765">PCase</span></span>
<span data-ttu-id="e07f2-766">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-766">**Description:**</span></span>  
<span data-ttu-id="e07f2-767">Funkce PCase převede první znak každého slova mezerami řetězce na velká písmena a všechny ostatní znaky jsou převedeny na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="e07f2-767">The PCase function converts the first character of each space delimited word in a string to upper case, and all other characters are converted to lower case.</span></span>

<span data-ttu-id="e07f2-768">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-768">**Syntax:**</span></span>  
`String PCase(string)`

<span data-ttu-id="e07f2-769">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-769">**Remarks:**</span></span>

* <span data-ttu-id="e07f2-770">Tato funkce v současné době neobsahuje správné velká a malá písmena převést slovo, které je zcela velká, jako je například zkratka.</span><span class="sxs-lookup"><span data-stu-id="e07f2-770">This function does not currently provide proper casing to convert a word that is entirely uppercase, such as an acronym.</span></span>

<span data-ttu-id="e07f2-771">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-771">**Example:**</span></span>  
`PCase("TEsT")`  
<span data-ttu-id="e07f2-772">Vrátí hodnotu "Test".</span><span class="sxs-lookup"><span data-stu-id="e07f2-772">Returns "Test".</span></span>

`PCase(LCase("TEST"))`  
<span data-ttu-id="e07f2-773">Vrátí "Test"</span><span class="sxs-lookup"><span data-stu-id="e07f2-773">Returns "Test"</span></span>

- - -
### <a name="randomnum"></a><span data-ttu-id="e07f2-774">RandomNum</span><span class="sxs-lookup"><span data-stu-id="e07f2-774">RandomNum</span></span>
<span data-ttu-id="e07f2-775">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-775">**Description:**</span></span>  
<span data-ttu-id="e07f2-776">Funkce RandomNum Vrátí náhodné číslo mezi zadaného intervalu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-776">The RandomNum function returns a random number between a specified interval.</span></span>

<span data-ttu-id="e07f2-777">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-777">**Syntax:**</span></span>  
`num RandomNum(num start, num end)`

* <span data-ttu-id="e07f2-778">spustit: číslo určující dolní limit náhodná hodnota ke generování</span><span class="sxs-lookup"><span data-stu-id="e07f2-778">start: a number identifying the lower limit of the random value to generate</span></span>
* <span data-ttu-id="e07f2-779">end: číslo určující horní limit počtu náhodná hodnota ke generování</span><span class="sxs-lookup"><span data-stu-id="e07f2-779">end: a number identifying the upper limit of the random value to generate</span></span>

<span data-ttu-id="e07f2-780">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-780">**Example:**</span></span>  
`Random(100,999)`  
<span data-ttu-id="e07f2-781">734 může vrátit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-781">Can return 734.</span></span>

- - -
### <a name="removeduplicates"></a><span data-ttu-id="e07f2-782">Removeduplicates –</span><span class="sxs-lookup"><span data-stu-id="e07f2-782">RemoveDuplicates</span></span>
<span data-ttu-id="e07f2-783">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-783">**Description:**</span></span>  
<span data-ttu-id="e07f2-784">Removeduplicates – funkce přebírá řetězec s více hodnotami a ujistěte se, že každá hodnota je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="e07f2-784">The RemoveDuplicates function takes a multi-valued string and make sure each value is unique.</span></span>

<span data-ttu-id="e07f2-785">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-785">**Syntax:**</span></span>  
`mvstr RemoveDuplicates(mvstr attribute)`

<span data-ttu-id="e07f2-786">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-786">**Example:**</span></span>  
`RemoveDuplicates([proxyAddresses])`  
<span data-ttu-id="e07f2-787">Vrátí atribut upravený proxyAddress, kde byly odebrány všechny duplicitní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e07f2-787">Returns a sanitized proxyAddress attribute where all duplicate values have been removed.</span></span>

- - -
### <a name="replace"></a><span data-ttu-id="e07f2-788">Nahradit</span><span class="sxs-lookup"><span data-stu-id="e07f2-788">Replace</span></span>
<span data-ttu-id="e07f2-789">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-789">**Description:**</span></span>  
<span data-ttu-id="e07f2-790">Funkce Nahradit nahradí všechny výskyty řetězce jiným řetězcem.</span><span class="sxs-lookup"><span data-stu-id="e07f2-790">The Replace function replaces all occurrences of a string to another string.</span></span>

<span data-ttu-id="e07f2-791">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-791">**Syntax:**</span></span>  
`str Replace(str string, str OldValue, str NewValue)`

* <span data-ttu-id="e07f2-792">řetězec: řetězec tak, aby nahraďte hodnoty v.</span><span class="sxs-lookup"><span data-stu-id="e07f2-792">string: A string to replace values in.</span></span>
* <span data-ttu-id="e07f2-793">OldValue: Řetězec, Hledat a nahradit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-793">OldValue: The string to search for and to replace.</span></span>
* <span data-ttu-id="e07f2-794">NewValue: Řetězec, který chcete nahradit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-794">NewValue: The string to replace to.</span></span>

<span data-ttu-id="e07f2-795">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-795">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-796">Funkce rozpozná následující zvláštní monikery:</span><span class="sxs-lookup"><span data-stu-id="e07f2-796">The function recognizes the following special monikers:</span></span>

* <span data-ttu-id="e07f2-797">\n – nový řádek</span><span class="sxs-lookup"><span data-stu-id="e07f2-797">\n – New Line</span></span>
* <span data-ttu-id="e07f2-798">\r – znaky CR</span><span class="sxs-lookup"><span data-stu-id="e07f2-798">\r – Carriage Return</span></span>
* <span data-ttu-id="e07f2-799">\t – karta</span><span class="sxs-lookup"><span data-stu-id="e07f2-799">\t – Tab</span></span>

<span data-ttu-id="e07f2-800">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-800">**Example:**</span></span>  
`Replace([address],"\r\n",", ")`  
<span data-ttu-id="e07f2-801">Nahradí Line FEED čárkami a místa a může vést k "Jeden Microsoft způsob, Redmond, WA, USA"</span><span class="sxs-lookup"><span data-stu-id="e07f2-801">Replaces CRLF with a comma and space, and could lead to "One Microsoft Way, Redmond, WA, USA"</span></span>

- - -
### <a name="replacechars"></a><span data-ttu-id="e07f2-802">ReplaceChars</span><span class="sxs-lookup"><span data-stu-id="e07f2-802">ReplaceChars</span></span>
<span data-ttu-id="e07f2-803">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-803">**Description:**</span></span>  
<span data-ttu-id="e07f2-804">Funkce ReplaceChars nahradí všechny výskyty znaků v řetězci ReplacePattern nalezena.</span><span class="sxs-lookup"><span data-stu-id="e07f2-804">The ReplaceChars function replaces all occurrences of characters found in the ReplacePattern string.</span></span>

<span data-ttu-id="e07f2-805">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-805">**Syntax:**</span></span>  
`str ReplaceChars(str string, str ReplacePattern)`

* <span data-ttu-id="e07f2-806">řetězec: řetězec pro náhradu znaků v.</span><span class="sxs-lookup"><span data-stu-id="e07f2-806">string: A string to replace characters in.</span></span>
* <span data-ttu-id="e07f2-807">ReplacePattern: řetězec obsahující slovník s znaků, který má nahradit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-807">ReplacePattern: a string containing a dictionary with characters to replace.</span></span>

<span data-ttu-id="e07f2-808">Formát je {zdroj1}: {target1}, {zdroj2}: {target2}, {zdrojN}, {targetN} kde zdroj je znak, který má najít a cíle řetězec, který má nahradit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-808">The format is {source1}:{target1},{source2}:{target2},{sourceN},{targetN} where source is the character to find and target the string to replace with.</span></span>

<span data-ttu-id="e07f2-809">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-809">**Remarks:**</span></span>

* <span data-ttu-id="e07f2-810">Funkce přebírá všechny výskyty definované zdroje a nahradí je cíle.</span><span class="sxs-lookup"><span data-stu-id="e07f2-810">The function takes each occurrence of defined sources and replaces them with the targets.</span></span>
* <span data-ttu-id="e07f2-811">Zdroj musí mít přesně jeden znak (unicode).</span><span class="sxs-lookup"><span data-stu-id="e07f2-811">The source must be exactly one (unicode) character.</span></span>
* <span data-ttu-id="e07f2-812">Zdroj nemůže být prázdný nebo delší než jeden znak (Chyba analýzy).</span><span class="sxs-lookup"><span data-stu-id="e07f2-812">The source cannot be empty or longer than one character (parsing error).</span></span>
* <span data-ttu-id="e07f2-813">Cíl může mít více znaků, například ö:oe, β:ss.</span><span class="sxs-lookup"><span data-stu-id="e07f2-813">The target can have multiple characters, for example ö:oe, β:ss.</span></span>
* <span data-ttu-id="e07f2-814">Cílem může být prázdný, která určuje, že znak, který má být odebrána.</span><span class="sxs-lookup"><span data-stu-id="e07f2-814">The target can be empty indicating that the character should be removed.</span></span>
* <span data-ttu-id="e07f2-815">Zdroj je malá a velká písmena a musí být přesně shodovat.</span><span class="sxs-lookup"><span data-stu-id="e07f2-815">The source is case-sensitive and must be an exact match.</span></span>
* <span data-ttu-id="e07f2-816">(Čárkou) a: (dvojtečka) jsou vyhrazené znaky a nelze jej nahradit pomocí této funkce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-816">The , (comma) and : (colon) are reserved characters and cannot be replaced using this function.</span></span>
* <span data-ttu-id="e07f2-817">Mezery a další bílé znaky v řetězci ReplacePattern se ignorují.</span><span class="sxs-lookup"><span data-stu-id="e07f2-817">Spaces and other white characters in the ReplacePattern string are ignored.</span></span>

<span data-ttu-id="e07f2-818">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-818">**Example:**</span></span>  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
<span data-ttu-id="e07f2-819">Vrátí Raksmorgas</span><span class="sxs-lookup"><span data-stu-id="e07f2-819">Returns Raksmorgas</span></span>

`ReplaceChars("O’Neil",%ReplaceString%)`  
<span data-ttu-id="e07f2-820">Vrátí "ONeil", jeden značek je definována odeberou.</span><span class="sxs-lookup"><span data-stu-id="e07f2-820">Returns "ONeil", the single tick is defined to be removed.</span></span>

- - -
### <a name="right"></a><span data-ttu-id="e07f2-821">Vpravo</span><span class="sxs-lookup"><span data-stu-id="e07f2-821">Right</span></span>
<span data-ttu-id="e07f2-822">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-822">**Description:**</span></span>  
<span data-ttu-id="e07f2-823">Správné funkce vrátí zadaný počet znaků zprava (koncového) řetězce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-823">The Right function returns a specified number of characters from the right (end) of a string.</span></span>

<span data-ttu-id="e07f2-824">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-824">**Syntax:**</span></span>  
`str Right(str string, num NumChars)`

* <span data-ttu-id="e07f2-825">řetězec: řetězec, který vrátí znaky z</span><span class="sxs-lookup"><span data-stu-id="e07f2-825">string: the string to return characters from</span></span>
* <span data-ttu-id="e07f2-826">NumChars: číslo určující počet znaků k návratu z konci (vpravo) řetězce</span><span class="sxs-lookup"><span data-stu-id="e07f2-826">NumChars: a number identifying the number of characters to return from the end (right) of string</span></span>

<span data-ttu-id="e07f2-827">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-827">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-828">Od poslední pozice řetězce jsou vráceny NumChars znaky.</span><span class="sxs-lookup"><span data-stu-id="e07f2-828">NumChars characters are returned from the last position of string.</span></span>

<span data-ttu-id="e07f2-829">Řetězec obsahující poslední numChars znaky v řetězci:</span><span class="sxs-lookup"><span data-stu-id="e07f2-829">A string containing the last numChars characters in string:</span></span>

* <span data-ttu-id="e07f2-830">Pokud numChars = 0, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-830">If numChars = 0, return empty string.</span></span>
* <span data-ttu-id="e07f2-831">Pokud numChars < 0, vrátí vstupní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-831">If numChars < 0, return input string.</span></span>
* <span data-ttu-id="e07f2-832">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-832">If string is null, return empty string.</span></span>

<span data-ttu-id="e07f2-833">Pokud řetězec obsahuje méně znaků, než číslo zadané v NumChars, je vrácen řetězec identické na řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-833">If string contains fewer characters than the number specified in NumChars, a string identical to string is returned.</span></span>

<span data-ttu-id="e07f2-834">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-834">**Example:**</span></span>  
`Right("John Doe", 3)`  
<span data-ttu-id="e07f2-835">Vrátí "Doe".</span><span class="sxs-lookup"><span data-stu-id="e07f2-835">Returns "Doe".</span></span>

- - -
### <a name="rtrim"></a><span data-ttu-id="e07f2-836">RTrim</span><span class="sxs-lookup"><span data-stu-id="e07f2-836">RTrim</span></span>
<span data-ttu-id="e07f2-837">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-837">**Description:**</span></span>  
<span data-ttu-id="e07f2-838">Funkce RTrim odebere koncové prázdné znaky v řetězci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-838">The RTrim function removes trailing white spaces from a string.</span></span>

<span data-ttu-id="e07f2-839">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-839">**Syntax:**</span></span>  
`str RTrim(str value)`

<span data-ttu-id="e07f2-840">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-840">**Example:**</span></span>  
`RTrim(" Test ")`  
<span data-ttu-id="e07f2-841">Vrátí hodnotu "Test".</span><span class="sxs-lookup"><span data-stu-id="e07f2-841">Returns " Test".</span></span>

- - -
### <a name="select"></a><span data-ttu-id="e07f2-842">Vyberte</span><span class="sxs-lookup"><span data-stu-id="e07f2-842">Select</span></span>
<span data-ttu-id="e07f2-843">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-843">**Description:**</span></span>  
<span data-ttu-id="e07f2-844">Proces všech hodnot v více hodnot atributů (nebo výstupní výrazu) založený na zadanou funkci.</span><span class="sxs-lookup"><span data-stu-id="e07f2-844">Process all values in a multi-valued attribute (or output of an expression) based on function specified.</span></span>

<span data-ttu-id="e07f2-845">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-845">**Syntax:**</span></span>  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* <span data-ttu-id="e07f2-846">Položka: reprezentuje element v více hodnotami atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-846">item: Represents an element in the multi-valued attribute</span></span>
* <span data-ttu-id="e07f2-847">Atribut: vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-847">attribute: the multi-valued attribute</span></span>
* <span data-ttu-id="e07f2-848">výraz: výraz, který vrátí kolekce hodnot</span><span class="sxs-lookup"><span data-stu-id="e07f2-848">expression: an expression that returns a collection of values</span></span>
* <span data-ttu-id="e07f2-849">podmínky: žádné funkce, který dokáže zpracovat položku v atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-849">condition: any function that can process an item in the attribute</span></span>

<span data-ttu-id="e07f2-850">**Příklady:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-850">**Examples:**</span></span>  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
<span data-ttu-id="e07f2-851">Vrátí všechny hodnoty ve více hodnot atributů otherPhone po odebrání pomlčky (-).</span><span class="sxs-lookup"><span data-stu-id="e07f2-851">Return all the values in the multi-valued attribute otherPhone after hyphens (-) have been removed.</span></span>

- - -
### <a name="split"></a><span data-ttu-id="e07f2-852">Rozdělení</span><span class="sxs-lookup"><span data-stu-id="e07f2-852">Split</span></span>
<span data-ttu-id="e07f2-853">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-853">**Description:**</span></span>  
<span data-ttu-id="e07f2-854">Funkce rozdělení přebírá řetězec oddělené s oddělovačem a udělá z něj řetězec s více hodnotami.</span><span class="sxs-lookup"><span data-stu-id="e07f2-854">The Split function takes a string separated with a delimiter and makes it a multi-valued string.</span></span>

<span data-ttu-id="e07f2-855">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-855">**Syntax:**</span></span>  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* <span data-ttu-id="e07f2-856">hodnota: řetězec s oddělovací znak pro oddělení.</span><span class="sxs-lookup"><span data-stu-id="e07f2-856">value: the string with a delimiter character to separate.</span></span>
* <span data-ttu-id="e07f2-857">oddělovač: jeden znak, který má být použit jako oddělovač.</span><span class="sxs-lookup"><span data-stu-id="e07f2-857">delimiter: single character to be used as the delimiter.</span></span>
* <span data-ttu-id="e07f2-858">limit: maximální počet hodnot, které můžou vrátit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-858">limit: maximum number of values that can return.</span></span>

<span data-ttu-id="e07f2-859">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-859">**Example:**</span></span>  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
<span data-ttu-id="e07f2-860">Vrátí řetězec s více hodnotami s 2 elementy užitečné pro atribut proxyAddress.</span><span class="sxs-lookup"><span data-stu-id="e07f2-860">Returns a multi-valued string with 2 elements useful for the proxyAddress attribute.</span></span>

- - -
### <a name="stringfromguid"></a><span data-ttu-id="e07f2-861">StringFromGuid</span><span class="sxs-lookup"><span data-stu-id="e07f2-861">StringFromGuid</span></span>
<span data-ttu-id="e07f2-862">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-862">**Description:**</span></span>  
<span data-ttu-id="e07f2-863">Funkce StringFromGuid trvá binární GUID a převede jej na řetězec</span><span class="sxs-lookup"><span data-stu-id="e07f2-863">The StringFromGuid function takes a binary GUID and converts it to a string</span></span>

<span data-ttu-id="e07f2-864">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-864">**Syntax:**</span></span>  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a><span data-ttu-id="e07f2-865">StringFromSid</span><span class="sxs-lookup"><span data-stu-id="e07f2-865">StringFromSid</span></span>
<span data-ttu-id="e07f2-866">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-866">**Description:**</span></span>  
<span data-ttu-id="e07f2-867">Funkce StringFromSid převede pole bajtů obsahující identifikátor zabezpečení na řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-867">The StringFromSid function converts a byte array containing a security identifier to a string.</span></span>

<span data-ttu-id="e07f2-868">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-868">**Syntax:**</span></span>  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a><span data-ttu-id="e07f2-869">Přepínač</span><span class="sxs-lookup"><span data-stu-id="e07f2-869">Switch</span></span>
<span data-ttu-id="e07f2-870">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-870">**Description:**</span></span>  
<span data-ttu-id="e07f2-871">Přepínač funkce slouží k vrátit jednu hodnotu na základě vyhodnocených podmínek.</span><span class="sxs-lookup"><span data-stu-id="e07f2-871">The Switch function is used to return a single value based on evaluated conditions.</span></span>

<span data-ttu-id="e07f2-872">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-872">**Syntax:**</span></span>  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* <span data-ttu-id="e07f2-873">Expr: výraz typu Variant, kterou chcete vyhodnotit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-873">expr: Variant expression you want to evaluate.</span></span>
* <span data-ttu-id="e07f2-874">hodnota: hodnota, která má být vrácen, pokud odpovídající výraz hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="e07f2-874">value: Value to be returned if the corresponding expression is True.</span></span>

<span data-ttu-id="e07f2-875">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-875">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-876">Seznam argumentů funkce přepínače se skládá z dvojic výrazy a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="e07f2-876">The Switch function argument list consists of pairs of expressions and values.</span></span> <span data-ttu-id="e07f2-877">Výrazy jsou vyhodnocovány zleva doprava a je vrácena hodnota přidružená k výrazu první vyhodnotit na hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="e07f2-877">The expressions are evaluated from left to right, and the value associated with the first expression to evaluate to True is returned.</span></span> <span data-ttu-id="e07f2-878">Pokud části nejsou spárovány správně, dojde k chybě spuštění.</span><span class="sxs-lookup"><span data-stu-id="e07f2-878">If the parts aren't properly paired, a run-time error occurs.</span></span>

<span data-ttu-id="e07f2-879">Například pokud Výraz1 hodnotu True, vrátí přepínač value1.</span><span class="sxs-lookup"><span data-stu-id="e07f2-879">For example, if expr1 is True, Switch returns value1.</span></span> <span data-ttu-id="e07f2-880">Pokud výraz-1 je False, ale výraz 2 má hodnotu True, vrátí přepínač hodnotu 2 a tak dále.</span><span class="sxs-lookup"><span data-stu-id="e07f2-880">If expr-1 is False, but expr-2 is True, Switch returns value-2, and so on.</span></span>

<span data-ttu-id="e07f2-881">Přepínač vrátí žádnou pokud:</span><span class="sxs-lookup"><span data-stu-id="e07f2-881">Switch returns a Nothing if:</span></span>

* <span data-ttu-id="e07f2-882">Žádný z výrazů mají hodnotu True.</span><span class="sxs-lookup"><span data-stu-id="e07f2-882">None of the expressions are True.</span></span>
* <span data-ttu-id="e07f2-883">První výraz, který hodnota True, má odpovídající hodnotu, která má hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="e07f2-883">The first True expression has a corresponding value that is Null.</span></span>

<span data-ttu-id="e07f2-884">Přepínač vyhodnotí všechny výrazy, i když vrací pouze jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="e07f2-884">Switch evaluates all expressions, even though it returns only one of them.</span></span> <span data-ttu-id="e07f2-885">Z tohoto důvodu je třeba dávat pozor na nežádoucí vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="e07f2-885">For this reason, you should watch for undesirable side effects.</span></span> <span data-ttu-id="e07f2-886">Například pokud vyhodnocování všech výrazu je výsledkem dělení nulou, dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="e07f2-886">For example, if the evaluation of any expression results in a division by zero error, an error occurs.</span></span>

<span data-ttu-id="e07f2-887">Hodnota může být také chybovou funkci, která by vrátit vlastní řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-887">Value can also be the Error function, which would return a custom string.</span></span>

<span data-ttu-id="e07f2-888">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-888">**Example:**</span></span>  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
<span data-ttu-id="e07f2-889">Vrátí jazyk používaný v některé hlavní města, jinak vrátí chybu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-889">Returns the language spoken in some major cities, otherwise returns an Error.</span></span>

- - -
### <a name="trim"></a><span data-ttu-id="e07f2-890">Uvolnění dočasné paměti</span><span class="sxs-lookup"><span data-stu-id="e07f2-890">Trim</span></span>
<span data-ttu-id="e07f2-891">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-891">**Description:**</span></span>  
<span data-ttu-id="e07f2-892">Funkce Trim odebere úvodní a koncové mezery z řetězce.</span><span class="sxs-lookup"><span data-stu-id="e07f2-892">The Trim function removes leading and trailing white spaces from a string.</span></span>

<span data-ttu-id="e07f2-893">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-893">**Syntax:**</span></span>  
`str Trim(str value)`  

<span data-ttu-id="e07f2-894">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-894">**Example:**</span></span>  
`Trim(" Test ")`  
<span data-ttu-id="e07f2-895">Vrátí hodnotu "Test".</span><span class="sxs-lookup"><span data-stu-id="e07f2-895">Returns "Test".</span></span>

`Trim([proxyAddresses])`  
<span data-ttu-id="e07f2-896">Odebere úvodní a koncové mezery pro každou hodnotu v atributu proxyAddress.</span><span class="sxs-lookup"><span data-stu-id="e07f2-896">Removes leading and trailing spaces for each value in the proxyAddress attribute.</span></span>

- - -
### <a name="ucase"></a><span data-ttu-id="e07f2-897">UCase</span><span class="sxs-lookup"><span data-stu-id="e07f2-897">UCase</span></span>
<span data-ttu-id="e07f2-898">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-898">**Description:**</span></span>  
<span data-ttu-id="e07f2-899">Funkce UCase převede všechny znaky v řetězci na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="e07f2-899">The UCase function converts all characters in a string to upper case.</span></span>

<span data-ttu-id="e07f2-900">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-900">**Syntax:**</span></span>  
`str UCase(str string)`

<span data-ttu-id="e07f2-901">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-901">**Example:**</span></span>  
`UCase("TeSt")`  
<span data-ttu-id="e07f2-902">Vrátí hodnotu "TEST".</span><span class="sxs-lookup"><span data-stu-id="e07f2-902">Returns "TEST".</span></span>

- - -
### <a name="where"></a><span data-ttu-id="e07f2-903">kde</span><span class="sxs-lookup"><span data-stu-id="e07f2-903">Where</span></span>

<span data-ttu-id="e07f2-904">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-904">**Description:**</span></span>  
<span data-ttu-id="e07f2-905">Vrátí podmnožinu hodnoty z více hodnot atributů (nebo výstupní výrazu) na základě konkrétní podmínky.</span><span class="sxs-lookup"><span data-stu-id="e07f2-905">Returns a subset of values from a multi-valued attribute (or output of an expression) based on specific condition.</span></span>

<span data-ttu-id="e07f2-906">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-906">**Syntax:**</span></span>  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* <span data-ttu-id="e07f2-907">Položka: reprezentuje element v více hodnotami atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-907">item: Represents an element in the multi-valued attribute</span></span>
* <span data-ttu-id="e07f2-908">Atribut: vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="e07f2-908">attribute: the multi-valued attribute</span></span>
* <span data-ttu-id="e07f2-909">podmínky: žádné výraz, který lze vyhodnotit na hodnotu true nebo false</span><span class="sxs-lookup"><span data-stu-id="e07f2-909">condition: any expression that can be evaluated to true or false</span></span>
* <span data-ttu-id="e07f2-910">výraz: výraz, který vrátí kolekce hodnot</span><span class="sxs-lookup"><span data-stu-id="e07f2-910">expression: an expression that returns a collection of values</span></span>

<span data-ttu-id="e07f2-911">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-911">**Example:**</span></span>  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
<span data-ttu-id="e07f2-912">Návratové hodnoty certifikátu v userCertificate více hodnot atributů, které nejsou vypršela platnost.</span><span class="sxs-lookup"><span data-stu-id="e07f2-912">Return the certificate values in the multi-valued attribute userCertificate which aren’t expired.</span></span>

- - -
### <a name="with"></a><span data-ttu-id="e07f2-913">S</span><span class="sxs-lookup"><span data-stu-id="e07f2-913">With</span></span>
<span data-ttu-id="e07f2-914">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-914">**Description:**</span></span>  
<span data-ttu-id="e07f2-915">Funkce s poskytuje způsob, jak pomocí proměnné představují dílčím výrazu, která se zobrazí jeden nebo více časy komplexní výrazu zjednodušit složitý výraz.</span><span class="sxs-lookup"><span data-stu-id="e07f2-915">The With function provides a way to simplify a complex expression by using a variable to represent a subexpression which appears one or more times in the complex expression.</span></span>

<span data-ttu-id="e07f2-916">**Syntaxe:**
`With(var variable, exp subExpression, exp complexExpression)`</span><span class="sxs-lookup"><span data-stu-id="e07f2-916">**Syntax:**
`With(var variable, exp subExpression, exp complexExpression)`</span></span>  
* <span data-ttu-id="e07f2-917">Proměnná: představuje dílčím výrazu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-917">variable: Represents the subexpression.</span></span>
* <span data-ttu-id="e07f2-918">dílčím výrazu: reprezentována proměnná dílčím výrazu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-918">subExpression: subexpression represented by variable.</span></span>
* <span data-ttu-id="e07f2-919">complexExpression: složitý výraz.</span><span class="sxs-lookup"><span data-stu-id="e07f2-919">complexExpression: A complex expression.</span></span>

<span data-ttu-id="e07f2-920">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-920">**Example:**</span></span>  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
<span data-ttu-id="e07f2-921">Je funkčně srovnatelný:</span><span class="sxs-lookup"><span data-stu-id="e07f2-921">Is functionally equivalent to:</span></span>  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
<span data-ttu-id="e07f2-922">Která vrací pouze hodnoty neprošlé certifikátu v userCertificate atributu.</span><span class="sxs-lookup"><span data-stu-id="e07f2-922">Which returns only unexpired certificate values in the userCertificate attribute.</span></span>


- - -
### <a name="word"></a><span data-ttu-id="e07f2-923">Word</span><span class="sxs-lookup"><span data-stu-id="e07f2-923">Word</span></span>
<span data-ttu-id="e07f2-924">**Popis:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-924">**Description:**</span></span>  
<span data-ttu-id="e07f2-925">Funkce aplikace Word vrací slovo obsažené v řetězci, na základě parametrů popisující oddělovače použití a word číslo, které má vrátit.</span><span class="sxs-lookup"><span data-stu-id="e07f2-925">The Word function returns a word contained within a string, based on parameters describing the delimiters to use and the word number to return.</span></span>

<span data-ttu-id="e07f2-926">**Syntaxe:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-926">**Syntax:**</span></span>  
`str Word(str string, num WordNumber, str delimiters)`

* <span data-ttu-id="e07f2-927">řetězec: řetězec, který má vrátit slova.</span><span class="sxs-lookup"><span data-stu-id="e07f2-927">string: the string to return a word from.</span></span>
* <span data-ttu-id="e07f2-928">WordNumber: by měl vrátit číslo identifikující číslo, které aplikace word.</span><span class="sxs-lookup"><span data-stu-id="e07f2-928">WordNumber: a number identifying which word number should return.</span></span>
* <span data-ttu-id="e07f2-929">oddělovače: řetězec představující delimiter(s), který se má použít k identifikaci slova</span><span class="sxs-lookup"><span data-stu-id="e07f2-929">delimiters: a string representing the delimiter(s) that should be used to identify words</span></span>

<span data-ttu-id="e07f2-930">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-930">**Remarks:**</span></span>  
<span data-ttu-id="e07f2-931">Jednotlivé řetězce znaků v řetězci oddělených jeden znaků v oddělovače jsou označeny jako slova:</span><span class="sxs-lookup"><span data-stu-id="e07f2-931">Each string of characters in string separated by the one of the characters in delimiters are identified as words:</span></span>

* <span data-ttu-id="e07f2-932">Pokud počet < 1, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-932">If number < 1, returns empty string.</span></span>
* <span data-ttu-id="e07f2-933">Pokud má řetězec hodnotu null, vrátí prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-933">If string is null, returns empty string.</span></span>

<span data-ttu-id="e07f2-934">Pokud řetězec obsahuje méně než číslo slova nebo řetězec neobsahuje žádné slova se identifikovanou pomocí oddělovače, je vrácen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="e07f2-934">If string contains less than number words, or string does not contain any words identified by delimiters, an empty string is returned.</span></span>

<span data-ttu-id="e07f2-935">**Příklad:**</span><span class="sxs-lookup"><span data-stu-id="e07f2-935">**Example:**</span></span>  
`Word("The quick brown fox",3," ")`  
<span data-ttu-id="e07f2-936">Vrátí "hnědá"</span><span class="sxs-lookup"><span data-stu-id="e07f2-936">Returns "brown"</span></span>

`Word("This,string!has&many separators",3,",!&#")`  
<span data-ttu-id="e07f2-937">Vrátí "má"</span><span class="sxs-lookup"><span data-stu-id="e07f2-937">Would return "has"</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e07f2-938">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e07f2-938">Additional Resources</span></span>
* [<span data-ttu-id="e07f2-939">Principy výrazů deklarativního zřizování</span><span class="sxs-lookup"><span data-stu-id="e07f2-939">Understanding Declarative Provisioning Expressions</span></span>](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [<span data-ttu-id="e07f2-940">Azure AD Connect Sync: Možnosti přizpůsobení synchronizace</span><span class="sxs-lookup"><span data-stu-id="e07f2-940">Azure AD Connect Sync: Customizing Synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="e07f2-941">Integrování místních identit do služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e07f2-941">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
