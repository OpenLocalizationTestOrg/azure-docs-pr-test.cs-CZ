---
title: "Konektor historie verzí | Microsoft Docs"
description: "Toto téma uvádí všechny verze konektorů pro Forefront Identity Manager (FIM) a Microsoft Identity Manager (MIM)"
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.openlocfilehash: 313145f4d8e5faa91fb3504cb0fd0ba87ca2e379
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connector-version-release-history"></a><span data-ttu-id="e53b8-103">Historie vydaných verzí konektoru</span><span class="sxs-lookup"><span data-stu-id="e53b8-103">Connector Version Release History</span></span>
<span data-ttu-id="e53b8-104">Konektory pro Forefront Identity Manager (FIM) a Microsoft Identity Manager (MIM) jsou často aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="e53b8-104">The Connectors for Forefront Identity Manager (FIM) and Microsoft Identity Manager (MIM) are updated frequently.</span></span>

> [!NOTE]
> <span data-ttu-id="e53b8-105">Toto téma je k dispozici pouze v produktu FIM a MIM.</span><span class="sxs-lookup"><span data-stu-id="e53b8-105">This topic is on FIM and MIM only.</span></span> <span data-ttu-id="e53b8-106">Tyto konektory nejsou podporovány pro instalaci na Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="e53b8-106">These Connectors are not supported for install on Azure AD Connect.</span></span> <span data-ttu-id="e53b8-107">Vydaná konektory jsou předinstalované na službu AADConnect. při upgradu na zadaná sestavení.</span><span class="sxs-lookup"><span data-stu-id="e53b8-107">Released Connectors are preinstalled on AADConnect when upgrading to specified Build.</span></span>

<span data-ttu-id="e53b8-108">Toto téma uvádí všechny verze konektory, které byly vydány.</span><span class="sxs-lookup"><span data-stu-id="e53b8-108">This topic list all versions of the Connectors that have been released.</span></span>

<span data-ttu-id="e53b8-109">Související odkazy:</span><span class="sxs-lookup"><span data-stu-id="e53b8-109">Related links:</span></span>

* [<span data-ttu-id="e53b8-110">Stáhněte si nejnovější konektory</span><span class="sxs-lookup"><span data-stu-id="e53b8-110">Download Latest Connectors</span></span>](http://go.microsoft.com/fwlink/?LinkId=717495)
* <span data-ttu-id="e53b8-111">[Obecné konektor LDAP](active-directory-aadconnectsync-connector-genericldap.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e53b8-111">[Generic LDAP Connector](active-directory-aadconnectsync-connector-genericldap.md) reference documentation</span></span>
* <span data-ttu-id="e53b8-112">[Obecné konektor SQL](active-directory-aadconnectsync-connector-genericsql.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e53b8-112">[Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md) reference documentation</span></span>
* <span data-ttu-id="e53b8-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e53b8-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) reference documentation</span></span>
* <span data-ttu-id="e53b8-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e53b8-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) reference documentation</span></span>
* <span data-ttu-id="e53b8-115">[Konektoru Lotus Domino](active-directory-aadconnectsync-connector-domino.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="e53b8-115">[Lotus Domino Connector](active-directory-aadconnectsync-connector-domino.md) reference documentation</span></span>


## <a name="116040-aadconnect-pending-release"></a><span data-ttu-id="e53b8-116">1.1.604.0 (AADConnect čeká na uvolnění)</span><span class="sxs-lookup"><span data-stu-id="e53b8-116">1.1.604.0 (AADConnect Pending Release)</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="e53b8-117">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="e53b8-117">Fixed issues:</span></span>

* <span data-ttu-id="e53b8-118">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="e53b8-118">Generic Web Services:</span></span>
  * <span data-ttu-id="e53b8-119">Byl opraven problém brání vytváří při nebyly k dispozici dva nebo víc koncových bodů protokolu SOAP projektu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-119">Fixed an issue preventing a SOAP project from being created when there were two or more endpoints.</span></span>
* <span data-ttu-id="e53b8-120">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="e53b8-120">Generic SQL:</span></span>
  * <span data-ttu-id="e53b8-121">Operace importu GSQL nebyl konvertování času správně, když se uloží do prostoru konektoru.</span><span class="sxs-lookup"><span data-stu-id="e53b8-121">In the operation of import the GSQL was not converting time correctly, when saved to connector space.</span></span> <span data-ttu-id="e53b8-122">Výchozí formát data a času pro konektor místa GSQL byl změněn z "rrrr MM-dd: ssZ" na "rrrr MM-dd: ssZ..</span><span class="sxs-lookup"><span data-stu-id="e53b8-122">The default date and time format for connector space of the GSQL was changed from 'yyyy-MM-dd hh:mm:ssZ' to 'yyyy-MM-dd HH:mm:ssZ'.</span></span>

## <a name="115510-aadconnect-115530"></a><span data-ttu-id="e53b8-123">1.1.551.0 (AADConnect 1.1.553.0)</span><span class="sxs-lookup"><span data-stu-id="e53b8-123">1.1.551.0 (AADConnect 1.1.553.0)</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="e53b8-124">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="e53b8-124">Fixed issues:</span></span>

* <span data-ttu-id="e53b8-125">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="e53b8-125">Generic Web Services:</span></span>
  * <span data-ttu-id="e53b8-126">Nástroj Wsconfig nepřevádět správně pole Json z "Ukázková žádost" pro metodu služby REST.</span><span class="sxs-lookup"><span data-stu-id="e53b8-126">The Wsconfig tool did not convert correctly the Json array from "sample request" for the REST service method.</span></span> <span data-ttu-id="e53b8-127">Příčinou problémů s serializace toto pole Json pro požadavek REST.</span><span class="sxs-lookup"><span data-stu-id="e53b8-127">This caused problems with serialization this Json array for the REST request.</span></span>
  * <span data-ttu-id="e53b8-128">Webové služby konektoru konfigurační nástroj nepodporuje názvy atributů JSON využití místa symbolů</span><span class="sxs-lookup"><span data-stu-id="e53b8-128">Web Service Connector Configuration Tool does not support usage of space symbols in JSON attribute names</span></span> 
    * <span data-ttu-id="e53b8-129">Vzor nahrazení lze přidat ručně soubor WSConfigTool.exe.config, například```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span><span class="sxs-lookup"><span data-stu-id="e53b8-129">A Substitution pattern can be added manually to the WSConfigTool.exe.config file, e.g. ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span></span>

* <span data-ttu-id="e53b8-130">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="e53b8-130">Lotus Notes:</span></span>
  * <span data-ttu-id="e53b8-131">Pokud možnost **povolit vlastní udělení licence certifikátorům pro organizace nebo organizační jednotky** je po exportu toku Domino, ale v době exportu, exportují se všechny atributy zakázán konektoru selže při exportu (Update) KeyNotFoundException dochází k synchronizaci.</span><span class="sxs-lookup"><span data-stu-id="e53b8-131">When the option **Allow custom certifiers for Organization/Organizational Units** is disabled then the connector fails during export (Update) After the export flow all attributes are exported to Domino but at the time of export a KeyNotFoundException is returned to Sync.</span></span> 
    * <span data-ttu-id="e53b8-132">K tomu dojde, protože operace přejmenování se nezdařila při pokusu změnit rozlišující název (uživatelské jméno atribut) změnou jeden z atributů níže:</span><span class="sxs-lookup"><span data-stu-id="e53b8-132">This happens because the rename operation fails when it tries to change DN (UserName attribute) by changing one of the attributes below:</span></span>  
      - <span data-ttu-id="e53b8-133">Příjmení</span><span class="sxs-lookup"><span data-stu-id="e53b8-133">LastName</span></span>
      - <span data-ttu-id="e53b8-134">FirstName</span><span class="sxs-lookup"><span data-stu-id="e53b8-134">FirstName</span></span>
      - <span data-ttu-id="e53b8-135">Indexována</span><span class="sxs-lookup"><span data-stu-id="e53b8-135">MiddleInitial</span></span>
      - <span data-ttu-id="e53b8-136">AltFullName</span><span class="sxs-lookup"><span data-stu-id="e53b8-136">AltFullName</span></span>
      - <span data-ttu-id="e53b8-137">AltFullNameLanguage</span><span class="sxs-lookup"><span data-stu-id="e53b8-137">AltFullNameLanguage</span></span>
      - <span data-ttu-id="e53b8-138">organizační jednotky</span><span class="sxs-lookup"><span data-stu-id="e53b8-138">ou</span></span>
      - <span data-ttu-id="e53b8-139">altcommonname</span><span class="sxs-lookup"><span data-stu-id="e53b8-139">altcommonname</span></span>

  * <span data-ttu-id="e53b8-140">Když **povolit vlastní udělení licence certifikátorům pro organizace nebo organizační jednotky** možnost povolena, ale vyžaduje udělení licence certifikátorům jsou stále prázdné, dojde k KeyNotFoundException.</span><span class="sxs-lookup"><span data-stu-id="e53b8-140">When **Allow custom certifiers for Organization/Organizational Units** option is enabled, but required certifiers are still empty, then KeyNotFoundException occurs.</span></span>

### <a name="enhancements"></a><span data-ttu-id="e53b8-141">Vylepšení:</span><span class="sxs-lookup"><span data-stu-id="e53b8-141">Enhancements:</span></span>

* <span data-ttu-id="e53b8-142">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="e53b8-142">Generic SQL:</span></span>
  * <span data-ttu-id="e53b8-143">**Scénář: přepracovali Neimplementováno:** "*" funkce</span><span class="sxs-lookup"><span data-stu-id="e53b8-143">**Scenario: redesigned Implemented:** "*" feature</span></span>
  * <span data-ttu-id="e53b8-144">**Popis řešení:** změnit metodu pro [odkaz na více hodnot atributů zpracování](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="e53b8-144">**Solution description:** Changed approach for [multi-valued reference attributes handling](active-directory-aadconnectsync-connector-genericsql.md).</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="e53b8-145">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="e53b8-145">Fixed issues:</span></span>

* <span data-ttu-id="e53b8-146">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="e53b8-146">Generic Web Services:</span></span>
  * <span data-ttu-id="e53b8-147">Nelze importovat konfiguraci serveru, pokud je k dispozici webovou službu konektoru</span><span class="sxs-lookup"><span data-stu-id="e53b8-147">Can’t import Server configuration if WebService Connector is present</span></span>
  * <span data-ttu-id="e53b8-148">Webová služba konektoru nepracuje s více webových služeb</span><span class="sxs-lookup"><span data-stu-id="e53b8-148">WebService Connector is not working with multiple  Web Services</span></span>

* <span data-ttu-id="e53b8-149">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="e53b8-149">Generic SQL:</span></span>
  * <span data-ttu-id="e53b8-150">Žádné typy objektů jsou uvedené pro jednu hodnotu Odkazovaný atribut</span><span class="sxs-lookup"><span data-stu-id="e53b8-150">No object types are listed for single value referenced attribute</span></span>
  * <span data-ttu-id="e53b8-151">Rozdílový import na sledování změn strategie odstraní objekt při hodnota se odebere z tabulky s více hodnotami</span><span class="sxs-lookup"><span data-stu-id="e53b8-151">Delta import on Change Tracking strategy deletes object when value is removed from multi-value table</span></span>
  * <span data-ttu-id="e53b8-152">OverflowException v konektoru GSQL s DB2 na AS / 400</span><span class="sxs-lookup"><span data-stu-id="e53b8-152">OverflowException in GSQL connector with DB2 on AS/400</span></span>

<span data-ttu-id="e53b8-153">Lotus:</span><span class="sxs-lookup"><span data-stu-id="e53b8-153">Lotus:</span></span>
  * <span data-ttu-id="e53b8-154">Přidání možnosti enable\disable hledání organizační jednotky před otevřením GlobalParameters stránky</span><span class="sxs-lookup"><span data-stu-id="e53b8-154">Added option to enable\disable searching OUs before opening GlobalParameters page</span></span>

## <a name="114430"></a><span data-ttu-id="e53b8-155">1.1.443.0</span><span class="sxs-lookup"><span data-stu-id="e53b8-155">1.1.443.0</span></span>

<span data-ttu-id="e53b8-156">Vydáno: 2017 března</span><span class="sxs-lookup"><span data-stu-id="e53b8-156">Released: 2017 March</span></span>

### <a name="enhancements"></a><span data-ttu-id="e53b8-157">Vylepšení</span><span class="sxs-lookup"><span data-stu-id="e53b8-157">Enhancements</span></span>

* <span data-ttu-id="e53b8-158">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="e53b8-158">Generic SQL:</span></span></br><span data-ttu-id="e53b8-159">
  **Scénář příznaky:** je dobře známé omezení s konektorem SQL, které jsme pouze povolit odkaz na typ jeden objekt a vyžadují křížových odkazů se členy.</span><span class="sxs-lookup"><span data-stu-id="e53b8-159">
  **Scenario Symptoms:**  It is a well-known limitation with the SQL Connector where we only allow a reference to one object type and require cross reference with members.</span></span> </br><span data-ttu-id="e53b8-160">
**Popis řešení:** v kroku zpracování odkazy byly "*" zvolíte možnost, všechny kombinace typů objektu se vrátí zpět synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="e53b8-160">
**Solution description:** In the processing step for references were "*" option is chosen, ALL combinations of object types will be returned back to the sync engine.</span></span>

>[!Important]
- <span data-ttu-id="e53b8-161">Tím se vytvoří mnoho zástupné symboly</span><span class="sxs-lookup"><span data-stu-id="e53b8-161">This will create many placeholders</span></span>
- <span data-ttu-id="e53b8-162">Je vyžadována zajistěte, aby byl pojmenování jedinečný pro různé typy objektů.</span><span class="sxs-lookup"><span data-stu-id="e53b8-162">It is required to make sure the naming is unique cross object types.</span></span>


* <span data-ttu-id="e53b8-163">Obecné LDAP:</span><span class="sxs-lookup"><span data-stu-id="e53b8-163">Generic LDAP:</span></span></br><span data-ttu-id="e53b8-164">
 **Scénář:** na konkrétní oddíl je vybrána pouze několik kontejnerů, pak vyhledávání stále bude provedeno v celý oddíl.</span><span class="sxs-lookup"><span data-stu-id="e53b8-164">
**Scenario:** When only few containers are selected in specific partition, then the search still will be done in whole partition.</span></span> <span data-ttu-id="e53b8-165">Konkrétní bude filtrováno podle synchronizační služba, ale ne podle MA, což by mohlo způsobit snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-165">Specific will be filtered by Synchronization Service, but not by MA which might cause performance degradation.</span></span> </br>

 <span data-ttu-id="e53b8-166">**Popis řešení:** kód změnit GLDAP konektor bylo možné přejít přes všechny kontejnery a hledání objektů v každé z nich, namísto hledání v celé oddílu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-166">**Solution description:** Changed GLDAP connector's code to make it possible go through all containers and search objects in each of them, instead of searching in the whole partition.</span></span>


* <span data-ttu-id="e53b8-167">Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="e53b8-167">Lotus Domino:</span></span>

  <span data-ttu-id="e53b8-168">**Scénář:** podporu odstranění Domino pošty pro odebrání osoba během exportu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-168">**Scenario:** Domino mail deletion support for a person removal during an export.</span></span> </br><span data-ttu-id="e53b8-169">
  **Řešení:** podporu odstranění konfigurovat e-mailu pro odebrání osoba během exportu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-169">
**Solution:** Configurable mail deletion support for a person removal during an export.</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="e53b8-170">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="e53b8-170">Fixed issues:</span></span>
* <span data-ttu-id="e53b8-171">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="e53b8-171">Generic Web Services:</span></span>
 * <span data-ttu-id="e53b8-172">Při změně ve výchozí adresa URL služby SAP wsconfig projekty prostřednictvím webové služby konfigurační nástroj a dochází k následující chybě: Nelze najít součást cesty</span><span class="sxs-lookup"><span data-stu-id="e53b8-172">When changing the service URL in Default SAP wsconfig projects through WebService Configuration Tool then the following error happens: Could not find a part of the path</span></span>

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* <span data-ttu-id="e53b8-173">Obecné LDAP:</span><span class="sxs-lookup"><span data-stu-id="e53b8-173">Generic LDAP:</span></span>
 * <span data-ttu-id="e53b8-174">Konektor GLDAP nejsou vidět všechny atributy ve službě AD LDS</span><span class="sxs-lookup"><span data-stu-id="e53b8-174">GLDAP Connector does not see all attributes in AD LDS</span></span>
 * <span data-ttu-id="e53b8-175">Průvodce zalomení při zjištění žádné atributy UPN ze schématu adresáře LDAP</span><span class="sxs-lookup"><span data-stu-id="e53b8-175">Wizard breaks when no UPN attributes are detected from the LDAP directory schema</span></span>
 * <span data-ttu-id="e53b8-176">Rozdílová importy selhání s chybami zjišťování nejsou k dispozici během úplný import, pokud není vybraná atribut "objectclass"</span><span class="sxs-lookup"><span data-stu-id="e53b8-176">Delta Imports Failing with discovery errors not present during full import, when "objectclass" attribute is not selected</span></span>
 * <span data-ttu-id="e53b8-177">Na stránce konfigurace "Konfigurace oddílů a hierarchií", nezobrazí všechny objekty typů, které se rovná oddílu pro nové servery v obecné</span><span class="sxs-lookup"><span data-stu-id="e53b8-177">A "Configure Partitions and Hierarchies” configuration page, doesn’t show any objects which type is equal to the partition for Novel servers in the Generic</span></span>  
<span data-ttu-id="e53b8-178">LDAP MA.</span><span class="sxs-lookup"><span data-stu-id="e53b8-178">LDAP MA.</span></span> <span data-ttu-id="e53b8-179">Ukázalo se pouze objekty z oddílu RootDSE.</span><span class="sxs-lookup"><span data-stu-id="e53b8-179">They showed only objects from RootDSE partition.</span></span>


* <span data-ttu-id="e53b8-180">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="e53b8-180">Generic SQL:</span></span>
 * <span data-ttu-id="e53b8-181">Opravte pro obecné SQL vodoznak chyb vícehodnotový atribut není importován rozdílový Import</span><span class="sxs-lookup"><span data-stu-id="e53b8-181">Fix for Generic SQL watermark Delta Import multivalued attribute not imported bug</span></span>
 * <span data-ttu-id="e53b8-182">Při exportu deleted\added hodnoty atributu s více hodnotami, nejsou deleted\added ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="e53b8-182">When exporting deleted\added values of multivalued attribute, they are not deleted\added in data source.</span></span>  


* <span data-ttu-id="e53b8-183">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="e53b8-183">Lotus Notes:</span></span>
 * <span data-ttu-id="e53b8-184">Konkrétní pole "Úplný název" se zobrazí správně v úložišti metaverse ale při exportu do poznámky hodnotu pro atribut má hodnotu Null nebo prázdný.</span><span class="sxs-lookup"><span data-stu-id="e53b8-184">A specific field "Full Name" is shown in the metaverse correctly however when exporting to Notes the value for the attribute is Null or Empty.</span></span>
 * <span data-ttu-id="e53b8-185">Opravit duplicitní Certifier došlo k chybě</span><span class="sxs-lookup"><span data-stu-id="e53b8-185">Fix for duplicate Certifier error</span></span>
 * <span data-ttu-id="e53b8-186">Když je vybraný objekt bez všechna data na konektoru Lotus Domino s jinými objekty potom jsme zobrazí zjišťování Chyba při provádění úplné importu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-186">When the Object without any data is selected on the Lotus Domino Connector with other objects then we receive the Discovery error while performing Full-Import.</span></span>
 * <span data-ttu-id="e53b8-187">Pokud se rozdílový Import se systémem konektoru Lotus Domino na konci tohoto spustit Microsoft.IdentityManagement.MA.LotusDomino.Service.exe služby někdy vrátí chyby aplikace.</span><span class="sxs-lookup"><span data-stu-id="e53b8-187">When Delta Import is being running on the Lotus Domino Connector, at the end of that run, the Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service sometimes returns an Application Error.</span></span>
 * <span data-ttu-id="e53b8-188">Skupiny členství celkové funguje bez problémů a zůstane zachované, s výjimkou při spuštění exportu do pokuste se odebrat uživatele z členství se zobrazí jako úspěšně dokončený s aktualizací, ale není ve skutečnosti získat odstranění uživatele z členství v Lotus Notes.</span><span class="sxs-lookup"><span data-stu-id="e53b8-188">Group membership overall works fine and is maintained, except when running the export to try to remove a user from membership it shows as successful with an update, but the user doesn’t actually get removed from membership in Lotus Notes.</span></span>
 * <span data-ttu-id="e53b8-189">Možnost zvolit režim exportu "Připojení položka v dolní části" se přidala v konfiguraci grafického uživatelského rozhraní systému Lotus MA pro připojení nové položky v dolní části během exportu pro více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="e53b8-189">An opportunity to choose mode of export as “Append Item at bottom” was added in configuration GUI of Lotus MA to append new items at bottom during the export for multi-valued attributes.</span></span>
 * <span data-ttu-id="e53b8-190">Konektor přidá logice potřebné k odstranění tohoto souboru ze složky e-mailu a ID trezoru.</span><span class="sxs-lookup"><span data-stu-id="e53b8-190">Connector will add the needed logic to delete the file from the Mail Folder and ID Vault.</span></span>
 * <span data-ttu-id="e53b8-191">Odstraňte členství nefunguje mezi NAB člen.</span><span class="sxs-lookup"><span data-stu-id="e53b8-191">Delete membership not working for cross NAB member.</span></span>
 * <span data-ttu-id="e53b8-192">Hodnoty by měla být úspěšně odstraněna z vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="e53b8-192">Values should be successfully deleted from multi-valued attribute</span></span>

## <a name="111170"></a><span data-ttu-id="e53b8-193">1.1.117.0</span><span class="sxs-lookup"><span data-stu-id="e53b8-193">1.1.117.0</span></span>
<span data-ttu-id="e53b8-194">Vydáno: března 2016</span><span class="sxs-lookup"><span data-stu-id="e53b8-194">Released: 2016 March</span></span>

<span data-ttu-id="e53b8-195">**Nový konektor**</span><span class="sxs-lookup"><span data-stu-id="e53b8-195">**New Connector**</span></span>  
<span data-ttu-id="e53b8-196">Počáteční verze [obecné konektor SQL](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="e53b8-196">Initial release of the [Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md).</span></span>

<span data-ttu-id="e53b8-197">**Nové funkce:**</span><span class="sxs-lookup"><span data-stu-id="e53b8-197">**New features:**</span></span>

* <span data-ttu-id="e53b8-198">Generický konektor LDAP:</span><span class="sxs-lookup"><span data-stu-id="e53b8-198">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="e53b8-199">Přidaná podpora pro Rozdílový import s Isode.</span><span class="sxs-lookup"><span data-stu-id="e53b8-199">Added support for delta import with Isode.</span></span>
* <span data-ttu-id="e53b8-200">Konektor webových služeb:</span><span class="sxs-lookup"><span data-stu-id="e53b8-200">Web Services Connector:</span></span>
  * <span data-ttu-id="e53b8-201">Aktualizovat csEntryChangeResult aktivity a aktivity setImportErrorCode umožňující objekt úrovně chyby vrátit zpět do synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="e53b8-201">Updated the csEntryChangeResult activity and setImportErrorCode activity to allow object level errors to be returned back to the sync engine.</span></span>
  * <span data-ttu-id="e53b8-202">Aktualizovat SAP6 a SAP6User šablony využívat nové funkce úrovně Chyba objektu.</span><span class="sxs-lookup"><span data-stu-id="e53b8-202">Updated the SAP6 and SAP6User templates to use the new object level error functionality.</span></span>
* <span data-ttu-id="e53b8-203">Konektoru Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="e53b8-203">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="e53b8-204">Pro export budete potřebovat jeden certifier na adresář.</span><span class="sxs-lookup"><span data-stu-id="e53b8-204">For export, you need one certifier per address book.</span></span> <span data-ttu-id="e53b8-205">Nyní můžete stejné heslo pro všechny udělení licence certifikátorům v zájmu snazší správy.</span><span class="sxs-lookup"><span data-stu-id="e53b8-205">You can now use the same password for all certifiers to make the management easier.</span></span>

<span data-ttu-id="e53b8-206">**Opravené problémy:**</span><span class="sxs-lookup"><span data-stu-id="e53b8-206">**Fixed issues:**</span></span>

* <span data-ttu-id="e53b8-207">Generický konektor LDAP:</span><span class="sxs-lookup"><span data-stu-id="e53b8-207">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="e53b8-208">Pro IBM Tivoli DS nebyla zjištěna správně některé atributy typu odkaz.</span><span class="sxs-lookup"><span data-stu-id="e53b8-208">For IBM Tivoli DS, some reference attributes were not detected correctly.</span></span>
  * <span data-ttu-id="e53b8-209">Otevřete protokol během Rozdílový import bylo zkráceno mezery na začátku a konci řetězce.</span><span class="sxs-lookup"><span data-stu-id="e53b8-209">For Open LDAP during a delta import, whitespaces at the beginning and end of strings were truncated.</span></span>
  * <span data-ttu-id="e53b8-210">Novell a NetIQ přejmenovat, přesunout objekt mezi organizační jednotky nebo kontejnery a současně export objektu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="e53b8-210">For Novell and NetIQ, an export that moved an object between OUs/containers and at the same time renamed the object failed.</span></span>
* <span data-ttu-id="e53b8-211">Konektor webových služeb:</span><span class="sxs-lookup"><span data-stu-id="e53b8-211">Web Services Connector:</span></span>
  * <span data-ttu-id="e53b8-212">Pokud webová služba měli víc koncových bodů pro stejnou vazbu, pak konektor není zjistit správně těchto koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="e53b8-212">If the web service had multiple end-points for same binding, then the Connector did not correctly discover these end-points.</span></span>
* <span data-ttu-id="e53b8-213">Konektoru Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="e53b8-213">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="e53b8-214">Exportu atribut fullName k e-mailu v databázi nebyla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="e53b8-214">An export of the fullName attribute to a mail-in database did not work.</span></span>
  * <span data-ttu-id="e53b8-215">Exportu, což jak přidat nebo odebrat člena ze skupiny exportovat pouze přidaných členů.</span><span class="sxs-lookup"><span data-stu-id="e53b8-215">An export which both added and removed member from a group only exported the added members.</span></span>
  * <span data-ttu-id="e53b8-216">Pokud dokument poznámky je neplatný (isValid atributu nastavena na hodnotu false), pak konektor selže.</span><span class="sxs-lookup"><span data-stu-id="e53b8-216">If a Notes Document is invalid (the attribute isValid set to false), then the Connector fails.</span></span>

## <a name="older-releases"></a><span data-ttu-id="e53b8-217">Starší verze</span><span class="sxs-lookup"><span data-stu-id="e53b8-217">Older releases</span></span>
<span data-ttu-id="e53b8-218">Před. března 2016 byly vydané konektory jako témata týkající se podpory.</span><span class="sxs-lookup"><span data-stu-id="e53b8-218">Before March 2016, the Connectors were released as support topics.</span></span>

<span data-ttu-id="e53b8-219">**Obecné LDAP**</span><span class="sxs-lookup"><span data-stu-id="e53b8-219">**Generic LDAP**</span></span>

* <span data-ttu-id="e53b8-220">[KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, září 2015</span><span class="sxs-lookup"><span data-stu-id="e53b8-220">[KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="e53b8-221">[KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, března 2015</span><span class="sxs-lookup"><span data-stu-id="e53b8-221">[KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="e53b8-222">[KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, leden 2015</span><span class="sxs-lookup"><span data-stu-id="e53b8-222">[KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 January</span></span>
* <span data-ttu-id="e53b8-223">[KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, září 2014</span><span class="sxs-lookup"><span data-stu-id="e53b8-223">[KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 September</span></span>
* <span data-ttu-id="e53b8-224">[KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, března 2014</span><span class="sxs-lookup"><span data-stu-id="e53b8-224">[KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 March</span></span>

<span data-ttu-id="e53b8-225">**Webovým službám**</span><span class="sxs-lookup"><span data-stu-id="e53b8-225">**WebServices**</span></span>

* <span data-ttu-id="e53b8-226">[KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, září 2014</span><span class="sxs-lookup"><span data-stu-id="e53b8-226">[KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="e53b8-227">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="e53b8-227">**PowerShell**</span></span>

* <span data-ttu-id="e53b8-228">[KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, září 2014</span><span class="sxs-lookup"><span data-stu-id="e53b8-228">[KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="e53b8-229">**Lotus Domino**</span><span class="sxs-lookup"><span data-stu-id="e53b8-229">**Lotus Domino**</span></span>

* <span data-ttu-id="e53b8-230">[KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, září 2015</span><span class="sxs-lookup"><span data-stu-id="e53b8-230">[KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="e53b8-231">[KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, března 2015</span><span class="sxs-lookup"><span data-stu-id="e53b8-231">[KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="e53b8-232">[KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, srpen 2014</span><span class="sxs-lookup"><span data-stu-id="e53b8-232">[KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 August</span></span>
* <span data-ttu-id="e53b8-233">[KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003 února 2014</span><span class="sxs-lookup"><span data-stu-id="e53b8-233">[KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014 February</span></span>  
* <span data-ttu-id="e53b8-234">[KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, říjen 2013</span><span class="sxs-lookup"><span data-stu-id="e53b8-234">[KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 October</span></span>
* <span data-ttu-id="e53b8-235">[KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, srpen 2013</span><span class="sxs-lookup"><span data-stu-id="e53b8-235">[KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 August</span></span>

## <a name="next-steps"></a><span data-ttu-id="e53b8-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e53b8-236">Next steps</span></span>
<span data-ttu-id="e53b8-237">Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e53b8-237">Learn more about the [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="e53b8-238">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="e53b8-238">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
