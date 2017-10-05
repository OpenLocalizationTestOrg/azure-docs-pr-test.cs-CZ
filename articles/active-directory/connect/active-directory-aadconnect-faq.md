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
ms.openlocfilehash: fd5988b2d4170166902bb5cc39603d4a0f83be59
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a><span data-ttu-id="25906-103">Nejčastější dotazy pro Azure Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="25906-103">Frequently asked questions for Azure Active Directory Connect</span></span>

## <a name="general-installation"></a><span data-ttu-id="25906-104">Obecné instalace</span><span class="sxs-lookup"><span data-stu-id="25906-104">General installation</span></span>
<span data-ttu-id="25906-105">**Otázka: bude fungovat instalace Azure AD globálního správce má 2FA povolené?**</span><span class="sxs-lookup"><span data-stu-id="25906-105">**Q: Will installation work if the Azure AD Global Admin has 2FA enabled?**</span></span>  
<span data-ttu-id="25906-106">S sestavení z února 2016 to je podporováno.</span><span class="sxs-lookup"><span data-stu-id="25906-106">With the builds from February 2016, this is supported.</span></span>

<span data-ttu-id="25906-107">**Otázka: je způsob, jak nainstalovat Azure AD Connect bezobslužné?**</span><span class="sxs-lookup"><span data-stu-id="25906-107">**Q: Is there a way to install Azure AD Connect unattended?**</span></span>  
<span data-ttu-id="25906-108">Je podporován pouze pro instalaci Azure AD Connect s použitím Průvodce instalací.</span><span class="sxs-lookup"><span data-stu-id="25906-108">It is only supported to install Azure AD Connect using the installation wizard.</span></span> <span data-ttu-id="25906-109">Bezobslužné a bezobslužné instalace není podporována.</span><span class="sxs-lookup"><span data-stu-id="25906-109">An unattended and silent installation is not supported.</span></span>

<span data-ttu-id="25906-110">**Otázka: je nutné do doménové struktury, kde nelze kontaktovat jednu doménu. Instalace Azure AD Connect**</span><span class="sxs-lookup"><span data-stu-id="25906-110">**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**</span></span>  
<span data-ttu-id="25906-111">S sestavení z února 2016 to je podporováno.</span><span class="sxs-lookup"><span data-stu-id="25906-111">With the builds from February 2016, this is supported.</span></span>

<span data-ttu-id="25906-112">**Otázka: agenta stavu služby AD DS funguje na jádro serveru?**</span><span class="sxs-lookup"><span data-stu-id="25906-112">**Q: Does the AD DS health agent work on server core?**</span></span>  
<span data-ttu-id="25906-113">Ano.</span><span class="sxs-lookup"><span data-stu-id="25906-113">Yes.</span></span> <span data-ttu-id="25906-114">Po instalaci agenta, můžete dokončit proces registrace pomocí následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="25906-114">After installing the agent, you can complete the registration process using the following PowerShell cmdlet:</span></span> 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a><span data-ttu-id="25906-115">Síť</span><span class="sxs-lookup"><span data-stu-id="25906-115">Network</span></span>
<span data-ttu-id="25906-116">**Otázka: je nutné bránu firewall, síťové zařízení, nebo něco jiného, který omezuje maximální dobu, po připojení můžete zůstat otevřené v mé síti. Jak dlouho mají Moje prahová hodnota časového limitu straně klienta se při použití Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="25906-116">**Q: I have a firewall, network device, or something else that limits the maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**</span></span>  
<span data-ttu-id="25906-117">Všechny sítě softwaru, fyzických zařízení nebo cokoliv jiného, který omezuje maximální dobu, po připojení může zůstat otevřené by měl používat prahovou hodnotu alespoň 5 minut (300 sekund) pro připojení mezi serverem, kde je nainstalován klient Azure AD Connect a Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25906-117">All networking software, physical devices, or anything else that limits the maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between the server where the Azure AD Connect client is installed and Azure Active Directory.</span></span> <span data-ttu-id="25906-118">To platí také pro všechny dřív vydaných synchronizace nástroje Microsoft Identity.</span><span class="sxs-lookup"><span data-stu-id="25906-118">This also applies to all previously released Microsoft Identity synchronization tools.</span></span>

<span data-ttu-id="25906-119">**Otázka: jsou podporovány domény SLD (jedné domény štítek)?**</span><span class="sxs-lookup"><span data-stu-id="25906-119">**Q: Are SLDs (Single Label Domains) supported?**</span></span>  
<span data-ttu-id="25906-120">Ne, Azure AD Connect místními doménovými strukturami nebo doménami pomocí domény SLD nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="25906-120">No, Azure AD Connect does not support on-premises forests/domains using SLDs.</span></span>

<span data-ttu-id="25906-121">**Otázka: je "s tečkami" s názvem pro rozhraní NetBios podporovány?**</span><span class="sxs-lookup"><span data-stu-id="25906-121">**Q: Are "dotted" NetBios named supported?**</span></span>  
<span data-ttu-id="25906-122">Ne, Azure AD Connect nepodporuje místními doménovými strukturami nebo doménami, kde název NetBios obsahuje tečku "." v názvu.</span><span class="sxs-lookup"><span data-stu-id="25906-122">No, Azure AD Connect does not support on-premises forests/domains where the NetBios name contains a period "." in the name.</span></span>

## <a name="federation"></a><span data-ttu-id="25906-123">Federace</span><span class="sxs-lookup"><span data-stu-id="25906-123">Federation</span></span>
<span data-ttu-id="25906-124">**Otázka: Co mám dělat, když se zobrazí e-mailu, zpráva se žádostí o obnovení certifikátu Moje Office 365**</span><span class="sxs-lookup"><span data-stu-id="25906-124">**Q: What do I do if I receive an email that asking me to renew my Office 365 certificate**</span></span>  
<span data-ttu-id="25906-125">Použijte pokyny, které je uvedené v [obnovení certifikátů](active-directory-aadconnect-o365-certs.md) téma o tom, jak obnovit certifikát.</span><span class="sxs-lookup"><span data-stu-id="25906-125">Use the guidance that is outlined in the [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how to renew the certificate.</span></span>

<span data-ttu-id="25906-126">**Otázka: je nutné "Automaticky aktualizovat předávající strany" sadu O365 předávající strany. Je nutné provádět žádnou akci, když můj automaticky podpisový certifikát tokenu navyšování?**</span><span class="sxs-lookup"><span data-stu-id="25906-126">**Q: I have "Automatically update relying party" set for O365 relying party. Do I have to take any action when my token signing certificate automatically rolls over?**</span></span>  
<span data-ttu-id="25906-127">Použijte pokyny, které je popsané v článku [obnovení certifikátů](active-directory-aadconnect-o365-certs.md).</span><span class="sxs-lookup"><span data-stu-id="25906-127">Use the guidance that is outlined in the article [renew certificates](active-directory-aadconnect-o365-certs.md).</span></span>

## <a name="environment"></a><span data-ttu-id="25906-128">Prostředí</span><span class="sxs-lookup"><span data-stu-id="25906-128">Environment</span></span>
<span data-ttu-id="25906-129">**Otázka: je podporována přejmenování serveru po instalaci Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="25906-129">**Q: Is it supported to rename the server after Azure AD Connect has been installed?**</span></span>  
<span data-ttu-id="25906-130">Ne.</span><span class="sxs-lookup"><span data-stu-id="25906-130">No.</span></span> <span data-ttu-id="25906-131">Změna názvu serveru způsobí, že synchronizační modul nebude moct připojit k databázi SQL a služba nebude možné spustit.</span><span class="sxs-lookup"><span data-stu-id="25906-131">Changing the server name will cause the sync engine to not be able to connect to the SQL database and the service will not be able to start.</span></span>

## <a name="identity-data"></a><span data-ttu-id="25906-132">Data identit</span><span class="sxs-lookup"><span data-stu-id="25906-132">Identity data</span></span>
<span data-ttu-id="25906-133">**Otázka: vybraný atribut UPN (userPrincipalName) ve službě Azure AD neodpovídá názvu UPN místní - proč?**</span><span class="sxs-lookup"><span data-stu-id="25906-133">**Q: The UPN (userPrincipalName) attribute in Azure AD does not match the on-prem UPN - why?**</span></span>  
<span data-ttu-id="25906-134">Najdete v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="25906-134">See these articles:</span></span>

* [<span data-ttu-id="25906-135">Uživatelská jména v Office 365, Azure nebo Intune se neshodují místní UPN nebo alternativním přihlašovacím ID</span><span class="sxs-lookup"><span data-stu-id="25906-135">User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID</span></span>](https://support.microsoft.com/en-us/kb/2523192)
* [<span data-ttu-id="25906-136">Změny nejsou synchronizovány pomocí nástroje Azure Active Directory Sync po změně UPN uživatelský účet používat jiné federované domény</span><span class="sxs-lookup"><span data-stu-id="25906-136">Changes aren't synced by the Azure Active Directory Sync tool after you change the UPN of a user account to use a different federated domain</span></span>](https://support.microsoft.com/en-us/kb/2669550)

<span data-ttu-id="25906-137">Můžete také konfigurovat Azure AD umožňuje synchronizační modul aktualizovat userPrincipalName, jak je popsáno v [funkce služby Azure AD Connect sync](active-directory-aadconnectsyncservice-features.md).</span><span class="sxs-lookup"><span data-stu-id="25906-137">You can also configure Azure AD to allow the sync engine to update the userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).</span></span>

<span data-ttu-id="25906-138">**Otázka: je to, že nepodporuje logicky shodu místní AD skupiny nebo kontaktu objekty s existující objekty Azure AD skupiny nebo kontaktu?**</span><span class="sxs-lookup"><span data-stu-id="25906-138">**Q: Is it supported to soft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**</span></span>  
<span data-ttu-id="25906-139">Ne, to není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="25906-139">No, this is currently not supported.</span></span>

<span data-ttu-id="25906-140">**Otázka: je to, že nepodporuje ručně nastavit atribut ImmutableId u stávajících objektů Azure AD skupiny nebo kontaktu pevného odpovídat jej do místní AD skupiny nebo kontaktu objekty?**</span><span class="sxs-lookup"><span data-stu-id="25906-140">**Q: Is it supported to manually set ImmutableId attribute on existing Azure AD Group/Contact objects to hard match it to on-premises AD Group/Contact objects?**</span></span>  
<span data-ttu-id="25906-141">Ne, to není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="25906-141">No, this is currently not supported.</span></span>



## <a name="custom-configuration"></a><span data-ttu-id="25906-142">Vlastní konfigurace</span><span class="sxs-lookup"><span data-stu-id="25906-142">Custom configuration</span></span>
<span data-ttu-id="25906-143">**Otázka: kde popsané rutiny prostředí PowerShell pro Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="25906-143">**Q: Where are the PowerShell cmdlets for Azure AD Connect documented?**</span></span>  
<span data-ttu-id="25906-144">S výjimkou rutiny popsané v této lokalitě nejsou podporované jinými rutinami prostředí PowerShell nalezen ve službě Azure AD Connect pro použití zákazníka.</span><span class="sxs-lookup"><span data-stu-id="25906-144">With the exception of the cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.</span></span>

<span data-ttu-id="25906-145">**Otázka: je možné použít "Serveru serveru pro export/import" nalezena v *Synchronization Service Manager* přesunutí konfigurací mezi servery?**</span><span class="sxs-lookup"><span data-stu-id="25906-145">**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* to move configuration between servers?**</span></span>  
<span data-ttu-id="25906-146">Ne.</span><span class="sxs-lookup"><span data-stu-id="25906-146">No.</span></span> <span data-ttu-id="25906-147">Tato možnost nebude načíst všechna nastavení konfigurace a by se neměla používat.</span><span class="sxs-lookup"><span data-stu-id="25906-147">This option will not retrieve all configuration settings and should not be used.</span></span> <span data-ttu-id="25906-148">Místo toho používejte Průvodce a vytvořte základní konfigurace na druhý server, použijte editor pravidla synchronizace ke generování skriptů prostředí PowerShell přesunout všechny vlastní pravidlo mezi servery.</span><span class="sxs-lookup"><span data-stu-id="25906-148">You should instead use the wizard to create the base configuration on the second server and use the sync rule editor to generate PowerShell scripts to move any custom rule between servers.</span></span> <span data-ttu-id="25906-149">V tématu [kyvu migrace](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="25906-149">See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span>

<span data-ttu-id="25906-150">**Otázka: je možné hesla do mezipaměti Azure přihlašovací stránku a můžete to možné, protože obsahuje element input pro heslo pomocí automatického dokončování = "false" atribut?**</span><span class="sxs-lookup"><span data-stu-id="25906-150">**Q: Can passwords be cached for the Azure sign-in page and can this be prevented since it contains a password input element with the autocomplete = "false" attribute?**</span></span></br>
<span data-ttu-id="25906-151">Aktuálně nepodporujeme změna atributů HTML vstupní pole pro heslo, včetně automatického dokončování značky.</span><span class="sxs-lookup"><span data-stu-id="25906-151">We currently do not support modifying the HTML attributes of the Password input field, including the autocomplete tag.</span></span> <span data-ttu-id="25906-152">Právě pracujeme na funkce, která vám umožní pro vlastní Javascript, která vám umožní přidat všechny atributy, do pole heslo.</span><span class="sxs-lookup"><span data-stu-id="25906-152">We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field.</span></span> <span data-ttu-id="25906-153">To by měl být k dispozici novější součástí 2017.</span><span class="sxs-lookup"><span data-stu-id="25906-153">This should be available later part of 2017.</span></span>

<span data-ttu-id="25906-154">**Otázka: na Azure přihlašovací stránce se zobrazí uživatelská jména pro uživatele, kteří mají dříve úspěšně přihlášení.  Lze toto chování vypnout?**</span><span class="sxs-lookup"><span data-stu-id="25906-154">**Q: On the Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</span></span></br>
<span data-ttu-id="25906-155">Aktuálně nepodporujeme změna atributů HTML přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="25906-155">We currently do not support modifying the HTML attributes of the sign-in page.</span></span> <span data-ttu-id="25906-156">Právě pracujeme na funkce, která vám umožní pro vlastní Javascript, která vám umožní přidat všechny atributy, do pole heslo.</span><span class="sxs-lookup"><span data-stu-id="25906-156">We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field.</span></span> <span data-ttu-id="25906-157">To by měl být k dispozici novější součástí 2017.</span><span class="sxs-lookup"><span data-stu-id="25906-157">This should be available later part of 2017.</span></span>

<span data-ttu-id="25906-158">**Otázka: je způsob, jak zabránit souběžných relací?**</span><span class="sxs-lookup"><span data-stu-id="25906-158">**Q: Is there a way to prevent concurrent sessions?**</span></span></br>
<span data-ttu-id="25906-159">Ne.</span><span class="sxs-lookup"><span data-stu-id="25906-159">No.</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="25906-160">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="25906-160">Troubleshooting</span></span>
<span data-ttu-id="25906-161">**Otázka: jak můžete získat pomoc s Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="25906-161">**Q: How can I get help with Azure AD Connect?**</span></span>

[<span data-ttu-id="25906-162">Znalostní bázi Microsoft Knowledge Base (KB)</span><span class="sxs-lookup"><span data-stu-id="25906-162">Search the Microsoft Knowledge Base (KB)</span></span>](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* <span data-ttu-id="25906-163">Vyhledejte Microsoft Knowledge Base (KB) pro technické řešení pro běžné problémy opravu o podpoře pro Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="25906-163">Search the Microsoft Knowledge Base (KB) for technical solutions to common break-fix issues about Support for Azure AD Connect.</span></span>

[<span data-ttu-id="25906-164">Fóra Microsoft Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="25906-164">Microsoft Azure Active Directory Forums</span></span>](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* <span data-ttu-id="25906-165">Můžete vyhledat a procházet technical otázky a odpovědi od komunity nebo položte svou vlastní otázku kliknutím [zde](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span><span class="sxs-lookup"><span data-stu-id="25906-165">You can search and browse for technical questions and answers from the community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span></span>

[<span data-ttu-id="25906-166">Azure AD Connect zákaznickou podporu</span><span class="sxs-lookup"><span data-stu-id="25906-166">Azure AD Connect customer support</span></span>](https://manage.windowsazure.com/?getsupport=true)

* <span data-ttu-id="25906-167">Použijte tento odkaz k získání podpory prostřednictvím portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="25906-167">Use this link to get support through the Azure portal.</span></span>

