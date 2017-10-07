---
title: "Azure Active Directory Connect: Časté otázky – | Microsoft Docs"
description: "Tato stránka obsahuje časté otázky k Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 39c0b127d3dcf6f45607ad8b4647a9ad79dfc732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a><span data-ttu-id="91f4c-103">Nejčastější dotazy pro Azure Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="91f4c-103">Frequently asked questions for Azure Active Directory Connect</span></span>

## <a name="general-installation"></a><span data-ttu-id="91f4c-104">Obecné instalace</span><span class="sxs-lookup"><span data-stu-id="91f4c-104">General installation</span></span>
<span data-ttu-id="91f4c-105">**Otázka: bude instalace fungovat, pokud hello Azure AD globálního správce má 2FA povolené?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-105">**Q: Will installation work if hello Azure AD Global Admin has 2FA enabled?**</span></span>  
<span data-ttu-id="91f4c-106">S hello sestavení z února 2016 to je podporováno.</span><span class="sxs-lookup"><span data-stu-id="91f4c-106">With hello builds from February 2016, this is supported.</span></span>

<span data-ttu-id="91f4c-107">**Otázka: je způsob, jak tooinstall bezobslužné Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-107">**Q: Is there a way tooinstall Azure AD Connect unattended?**</span></span>  
<span data-ttu-id="91f4c-108">Je pouze podporované tooinstall Azure AD Connect pomocí Průvodce instalací hello.</span><span class="sxs-lookup"><span data-stu-id="91f4c-108">It is only supported tooinstall Azure AD Connect using hello installation wizard.</span></span> <span data-ttu-id="91f4c-109">Bezobslužné a bezobslužné instalace není podporována.</span><span class="sxs-lookup"><span data-stu-id="91f4c-109">An unattended and silent installation is not supported.</span></span>

<span data-ttu-id="91f4c-110">**Otázka: je nutné do doménové struktury, kde nelze kontaktovat jednu doménu. Instalace Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="91f4c-110">**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**</span></span>  
<span data-ttu-id="91f4c-111">S hello sestavení z února 2016 to je podporováno.</span><span class="sxs-lookup"><span data-stu-id="91f4c-111">With hello builds from February 2016, this is supported.</span></span>

<span data-ttu-id="91f4c-112">**Otázka: hello pracovní agenta stavu služby AD DS na jádro serveru?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-112">**Q: Does hello AD DS health agent work on server core?**</span></span>  
<span data-ttu-id="91f4c-113">Ano.</span><span class="sxs-lookup"><span data-stu-id="91f4c-113">Yes.</span></span> <span data-ttu-id="91f4c-114">Po instalaci agenta hello, může dokončení procesu registrace hello pomocí hello následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="91f4c-114">After installing hello agent, you can complete hello registration process using hello following PowerShell cmdlet:</span></span> 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a><span data-ttu-id="91f4c-115">Síť</span><span class="sxs-lookup"><span data-stu-id="91f4c-115">Network</span></span>
<span data-ttu-id="91f4c-116">**Otázka: je nutné bránu firewall, síťové zařízení, nebo něco jiného, který omezuje hello maximální dobu, po připojení můžete zůstat otevřené v mé síti. Jak dlouho mají Moje prahová hodnota časového limitu straně klienta se při použití Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-116">**Q: I have a firewall, network device, or something else that limits hello maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**</span></span>  
<span data-ttu-id="91f4c-117">Všechny síťové softwaru, fyzických zařízení nebo cokoliv jiného, co omezení, které hello maximální dobu, které připojení zůstat otevřené měli používat prahovou hodnotu alespoň 5 minut (300 sekund) pro připojení mezi hello serveru, kde hello Azure AD Connect je nainstalován klient a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="91f4c-117">All networking software, physical devices, or anything else that limits hello maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between hello server where hello Azure AD Connect client is installed and Azure Active Directory.</span></span> <span data-ttu-id="91f4c-118">To platí také nástrojů pro synchronizaci tooall dřív vydané Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="91f4c-118">This also applies tooall previously released Microsoft Identity synchronization tools.</span></span>

<span data-ttu-id="91f4c-119">**Otázka: jsou podporovány domény SLD (jedné domény štítek)?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-119">**Q: Are SLDs (Single Label Domains) supported?**</span></span>  
<span data-ttu-id="91f4c-120">Ne, Azure AD Connect místními doménovými strukturami nebo doménami pomocí domény SLD nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="91f4c-120">No, Azure AD Connect does not support on-premises forests/domains using SLDs.</span></span>

<span data-ttu-id="91f4c-121">**Otázka: je "s tečkami" s názvem pro rozhraní NetBios podporovány?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-121">**Q: Are "dotted" NetBios named supported?**</span></span>  
<span data-ttu-id="91f4c-122">Ne, Azure AD Connect nepodporuje místními doménovými strukturami nebo doménami, kde název pro rozhraní NetBios hello obsahuje tečku "." v názvu hello.</span><span class="sxs-lookup"><span data-stu-id="91f4c-122">No, Azure AD Connect does not support on-premises forests/domains where hello NetBios name contains a period "." in hello name.</span></span>

## <a name="federation"></a><span data-ttu-id="91f4c-123">Federace</span><span class="sxs-lookup"><span data-stu-id="91f4c-123">Federation</span></span>
<span data-ttu-id="91f4c-124">**Otázka: Co mám dělat, když se zobrazí e-mailu, výzva toorenew Moje certifikát Office 365**</span><span class="sxs-lookup"><span data-stu-id="91f4c-124">**Q: What do I do if I receive an email that asking me toorenew my Office 365 certificate**</span></span>  
<span data-ttu-id="91f4c-125">Použít hello pokyny, které je uvedené v hello [obnovení certifikátů](active-directory-aadconnect-o365-certs.md) téma na tom, jak toorenew hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="91f4c-125">Use hello guidance that is outlined in hello [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how toorenew hello certificate.</span></span>

<span data-ttu-id="91f4c-126">**Otázka: je nutné "Automaticky aktualizovat předávající strany" sadu O365 předávající strany. Je nutné provést tootake žádnou akci při navyšování Moje automaticky podpisový certifikát tokenu?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-126">**Q: I have "Automatically update relying party" set for O365 relying party. Do I have tootake any action when my token signing certificate automatically rolls over?**</span></span>  
<span data-ttu-id="91f4c-127">Použít hello pokyny, které je popsané v článku hello [obnovení certifikátů](active-directory-aadconnect-o365-certs.md).</span><span class="sxs-lookup"><span data-stu-id="91f4c-127">Use hello guidance that is outlined in hello article [renew certificates](active-directory-aadconnect-o365-certs.md).</span></span>

## <a name="environment"></a><span data-ttu-id="91f4c-128">Prostředí</span><span class="sxs-lookup"><span data-stu-id="91f4c-128">Environment</span></span>
<span data-ttu-id="91f4c-129">**Otázka: je it podporované toorename hello server po instalaci Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-129">**Q: Is it supported toorename hello server after Azure AD Connect has been installed?**</span></span>  
<span data-ttu-id="91f4c-130">Ne.</span><span class="sxs-lookup"><span data-stu-id="91f4c-130">No.</span></span> <span data-ttu-id="91f4c-131">Změna názvu serveru hello bude příčina hello synchronizační modul toonot možné tooconnect toohello SQL database a hello služba nebude možné toostart.</span><span class="sxs-lookup"><span data-stu-id="91f4c-131">Changing hello server name will cause hello sync engine toonot be able tooconnect toohello SQL database and hello service will not be able toostart.</span></span>

## <a name="identity-data"></a><span data-ttu-id="91f4c-132">Data identit</span><span class="sxs-lookup"><span data-stu-id="91f4c-132">Identity data</span></span>
<span data-ttu-id="91f4c-133">**Otázka: hello atribut UPN (userPrincipalName) ve službě Azure AD neodpovídá hello místní UPN - proč?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-133">**Q: hello UPN (userPrincipalName) attribute in Azure AD does not match hello on-prem UPN - why?**</span></span>  
<span data-ttu-id="91f4c-134">Najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="91f4c-134">See these articles:</span></span>

* [<span data-ttu-id="91f4c-135">Uživatelská jména v Office 365, Azure nebo Intune se neshodují hello místní UPN nebo alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="91f4c-135">User names in Office 365, Azure, or Intune don't match hello on-premises UPN or alternate login ID</span></span>](https://support.microsoft.com/en-us/kb/2523192)
* [<span data-ttu-id="91f4c-136">Změny nejsou synchronizovány pomocí nástroje Azure Active Directory Sync hello po změně hello UPN uživatel účet toouse jiné federované domény</span><span class="sxs-lookup"><span data-stu-id="91f4c-136">Changes aren't synced by hello Azure Active Directory Sync tool after you change hello UPN of a user account toouse a different federated domain</span></span>](https://support.microsoft.com/en-us/kb/2669550)

<span data-ttu-id="91f4c-137">Můžete také konfigurovat Azure AD tooallow hello synchronizační modul tooupdate hello userPrincipalName, jak je popsáno v [funkce služby Azure AD Connect sync](active-directory-aadconnectsyncservice-features.md).</span><span class="sxs-lookup"><span data-stu-id="91f4c-137">You can also configure Azure AD tooallow hello sync engine tooupdate hello userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).</span></span>

<span data-ttu-id="91f4c-138">**Otázka: je it podporované toosoft shodu místní objekty AD skupiny nebo kontaktu s existující objekty Azure AD skupiny nebo kontaktu?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-138">**Q: Is it supported toosoft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**</span></span>  
<span data-ttu-id="91f4c-139">Ne, to není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="91f4c-139">No, this is currently not supported.</span></span>

<span data-ttu-id="91f4c-140">**Otázka: je it podporované toomanually nastavte atribut ImmutableId na existující Azure AD skupiny nebo kontaktu objekty toohard shodovat se tooon místní AD skupiny nebo kontaktu objekty?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-140">**Q: Is it supported toomanually set ImmutableId attribute on existing Azure AD Group/Contact objects toohard match it tooon-premises AD Group/Contact objects?**</span></span>  
<span data-ttu-id="91f4c-141">Ne, to není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="91f4c-141">No, this is currently not supported.</span></span>



## <a name="custom-configuration"></a><span data-ttu-id="91f4c-142">Vlastní konfigurace</span><span class="sxs-lookup"><span data-stu-id="91f4c-142">Custom configuration</span></span>
<span data-ttu-id="91f4c-143">**Otázka: kde jsou hello rutiny prostředí PowerShell pro Azure AD Connect zdokumentovaný?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-143">**Q: Where are hello PowerShell cmdlets for Azure AD Connect documented?**</span></span>  
<span data-ttu-id="91f4c-144">S výjimkou hello hello rutiny popsané v této lokalitě nejsou podporované jinými rutinami prostředí PowerShell nalezen ve službě Azure AD Connect pro použití zákazníka.</span><span class="sxs-lookup"><span data-stu-id="91f4c-144">With hello exception of hello cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.</span></span>

<span data-ttu-id="91f4c-145">**Otázka: je možné použít "Serveru serveru pro export/import" nalezena v *Synchronization Service Manager* konfigurace toomove mezi servery?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-145">**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* toomove configuration between servers?**</span></span>  
<span data-ttu-id="91f4c-146">Ne.</span><span class="sxs-lookup"><span data-stu-id="91f4c-146">No.</span></span> <span data-ttu-id="91f4c-147">Tato možnost nebude načíst všechna nastavení konfigurace a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="91f4c-147">This option will not retrieve all configuration settings and should not be used.</span></span> <span data-ttu-id="91f4c-148">Místo toho by měla použijte hello Průvodce toocreate hello základní konfigurace na druhý server hello a pomocí hello synchronizační pravidlo editor toogenerate prostředí PowerShell skripty toomove žádné vlastní pravidlo mezi servery.</span><span class="sxs-lookup"><span data-stu-id="91f4c-148">You should instead use hello wizard toocreate hello base configuration on hello second server and use hello sync rule editor toogenerate PowerShell scripts toomove any custom rule between servers.</span></span> <span data-ttu-id="91f4c-149">V tématu [kyvu migrace](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="91f4c-149">See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span>

<span data-ttu-id="91f4c-150">**Otázka: je možné hesla do mezipaměti pro stránku hello Azure přihlášení a může to možné, protože obsahuje element input pro heslo pomocí funkce Automatické dokončování hello = "false" atribut?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-150">**Q: Can passwords be cached for hello Azure sign-in page and can this be prevented since it contains a password input element with hello autocomplete = "false" attribute?**</span></span></br>
<span data-ttu-id="91f4c-151">Aktuálně nepodporujeme změny atributy HTML hello hello vstupní pole pro heslo, včetně hello automatického dokončování značky.</span><span class="sxs-lookup"><span data-stu-id="91f4c-151">We currently do not support modifying hello HTML attributes of hello Password input field, including hello autocomplete tag.</span></span> <span data-ttu-id="91f4c-152">Právě pracujeme na funkce, která vám umožní pro vlastní Javascript, která vám umožní tooadd každé pole atributu toohello heslo.</span><span class="sxs-lookup"><span data-stu-id="91f4c-152">We are currently working on a feature that will allow for custom Javascript which will allow you tooadd any attribute toohello password field.</span></span> <span data-ttu-id="91f4c-153">To by měl být k dispozici novější součástí 2017.</span><span class="sxs-lookup"><span data-stu-id="91f4c-153">This should be available later part of 2017.</span></span>

<span data-ttu-id="91f4c-154">**Otázka: na hello Azure přihlašovací stránce se zobrazí uživatelských jmen pro uživatele, kteří mají dříve úspěšně přihlášení.  Lze toto chování vypnout?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-154">**Q: On hello Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</span></span></br>
<span data-ttu-id="91f4c-155">Aktuálně nepodporujeme změny atributů HTML hello hello přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="91f4c-155">We currently do not support modifying hello HTML attributes of hello sign-in page.</span></span> <span data-ttu-id="91f4c-156">Právě pracujeme na funkce, která vám umožní pro vlastní Javascript, která vám umožní tooadd každé pole atributu toohello heslo.</span><span class="sxs-lookup"><span data-stu-id="91f4c-156">We are currently working on a feature that will allow for custom Javascript which will allow you tooadd any attribute toohello password field.</span></span> <span data-ttu-id="91f4c-157">To by měl být k dispozici novější součástí 2017.</span><span class="sxs-lookup"><span data-stu-id="91f4c-157">This should be available later part of 2017.</span></span>

<span data-ttu-id="91f4c-158">**Otázka: je způsob, jak tooprevent souběžných relací?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-158">**Q: Is there a way tooprevent concurrent sessions?**</span></span></br>
<span data-ttu-id="91f4c-159">Ne.</span><span class="sxs-lookup"><span data-stu-id="91f4c-159">No.</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="91f4c-160">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="91f4c-160">Troubleshooting</span></span>
<span data-ttu-id="91f4c-161">**Otázka: jak můžete získat pomoc s Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="91f4c-161">**Q: How can I get help with Azure AD Connect?**</span></span>

[<span data-ttu-id="91f4c-162">Hledání hello znalostní báze Microsoft Knowledge Base (KB)</span><span class="sxs-lookup"><span data-stu-id="91f4c-162">Search hello Microsoft Knowledge Base (KB)</span></span>](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* <span data-ttu-id="91f4c-163">Hledání hello znalostní báze Microsoft Knowledge Base (KB) technické řešení toocommon opravu problémů o podpoře pro Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="91f4c-163">Search hello Microsoft Knowledge Base (KB) for technical solutions toocommon break-fix issues about Support for Azure AD Connect.</span></span>

[<span data-ttu-id="91f4c-164">Fóra Microsoft Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="91f4c-164">Microsoft Azure Active Directory Forums</span></span>](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* <span data-ttu-id="91f4c-165">Můžete vyhledat a procházet technical otázky a odpovědi od komunity hello nebo položte svou vlastní otázku kliknutím [zde](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span><span class="sxs-lookup"><span data-stu-id="91f4c-165">You can search and browse for technical questions and answers from hello community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span></span>

[<span data-ttu-id="91f4c-166">Azure AD Connect zákaznickou podporu</span><span class="sxs-lookup"><span data-stu-id="91f4c-166">Azure AD Connect customer support</span></span>](https://manage.windowsazure.com/?getsupport=true)

* <span data-ttu-id="91f4c-167">Použijte tento odkaz tooget podporu prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="91f4c-167">Use this link tooget support through hello Azure portal.</span></span>

