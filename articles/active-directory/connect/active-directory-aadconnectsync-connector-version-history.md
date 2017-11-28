---
title: "aaaConnector historie verzí | Microsoft Docs"
description: "Toto téma obsahuje seznam všech verzích hello konektory pro Forefront Identity Manager (FIM) a Microsoft Identity Manager (MIM)"
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
ms.openlocfilehash: 3522f17c30e46542eaa367ecdefdfd2fc47f71a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connector-version-release-history"></a><span data-ttu-id="80551-103">Historie vydaných verzí konektoru</span><span class="sxs-lookup"><span data-stu-id="80551-103">Connector Version Release History</span></span>
<span data-ttu-id="80551-104">Hello konektory pro Forefront Identity Manager (FIM) a Microsoft Identity Manager (MIM) jsou často aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="80551-104">hello Connectors for Forefront Identity Manager (FIM) and Microsoft Identity Manager (MIM) are updated frequently.</span></span>

> [!NOTE]
> <span data-ttu-id="80551-105">Toto téma je k dispozici pouze v produktu FIM a MIM.</span><span class="sxs-lookup"><span data-stu-id="80551-105">This topic is on FIM and MIM only.</span></span> <span data-ttu-id="80551-106">Tyto konektory nejsou podporovány pro instalaci na Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="80551-106">These Connectors are not supported for install on Azure AD Connect.</span></span> <span data-ttu-id="80551-107">Vydaná konektory jsou předinstalovat službu AADConnect. při upgradu toospecified sestavení.</span><span class="sxs-lookup"><span data-stu-id="80551-107">Released Connectors are preinstalled on AADConnect when upgrading toospecified Build.</span></span>

<span data-ttu-id="80551-108">Toto téma uvádí všechny verze hello konektory, které byly vydány.</span><span class="sxs-lookup"><span data-stu-id="80551-108">This topic list all versions of hello Connectors that have been released.</span></span>

<span data-ttu-id="80551-109">Související odkazy:</span><span class="sxs-lookup"><span data-stu-id="80551-109">Related links:</span></span>

* [<span data-ttu-id="80551-110">Stáhněte si nejnovější konektory</span><span class="sxs-lookup"><span data-stu-id="80551-110">Download Latest Connectors</span></span>](http://go.microsoft.com/fwlink/?LinkId=717495)
* <span data-ttu-id="80551-111">[Obecné konektor LDAP](active-directory-aadconnectsync-connector-genericldap.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="80551-111">[Generic LDAP Connector](active-directory-aadconnectsync-connector-genericldap.md) reference documentation</span></span>
* <span data-ttu-id="80551-112">[Obecné konektor SQL](active-directory-aadconnectsync-connector-genericsql.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="80551-112">[Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md) reference documentation</span></span>
* <span data-ttu-id="80551-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="80551-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) reference documentation</span></span>
* <span data-ttu-id="80551-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="80551-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) reference documentation</span></span>
* <span data-ttu-id="80551-115">[Konektoru Lotus Domino](active-directory-aadconnectsync-connector-domino.md) referenční dokumentace</span><span class="sxs-lookup"><span data-stu-id="80551-115">[Lotus Domino Connector](active-directory-aadconnectsync-connector-domino.md) reference documentation</span></span>


## <a name="116040-aadconnect-pending-release"></a><span data-ttu-id="80551-116">1.1.604.0 (AADConnect čeká na uvolnění)</span><span class="sxs-lookup"><span data-stu-id="80551-116">1.1.604.0 (AADConnect Pending Release)</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="80551-117">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="80551-117">Fixed issues:</span></span>

* <span data-ttu-id="80551-118">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="80551-118">Generic Web Services:</span></span>
  * <span data-ttu-id="80551-119">Byl opraven problém brání vytváří při nebyly k dispozici dva nebo víc koncových bodů protokolu SOAP projektu.</span><span class="sxs-lookup"><span data-stu-id="80551-119">Fixed an issue preventing a SOAP project from being created when there were two or more endpoints.</span></span>
* <span data-ttu-id="80551-120">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="80551-120">Generic SQL:</span></span>
  * <span data-ttu-id="80551-121">V operaci hello importu hello GSQL nebyl konvertování času správně, při uložení tooconnector místa.</span><span class="sxs-lookup"><span data-stu-id="80551-121">In hello operation of import hello GSQL was not converting time correctly, when saved tooconnector space.</span></span> <span data-ttu-id="80551-122">Hello výchozí formát data a času pro konektor místo hello GSQL byl změněn z 'rrrr MM-dd: ssZ' too'yyyy-MM-dd: ssZ '.</span><span class="sxs-lookup"><span data-stu-id="80551-122">hello default date and time format for connector space of hello GSQL was changed from 'yyyy-MM-dd hh:mm:ssZ' too'yyyy-MM-dd HH:mm:ssZ'.</span></span>

## <a name="115510-aadconnect-115530"></a><span data-ttu-id="80551-123">1.1.551.0 (AADConnect 1.1.553.0)</span><span class="sxs-lookup"><span data-stu-id="80551-123">1.1.551.0 (AADConnect 1.1.553.0)</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="80551-124">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="80551-124">Fixed issues:</span></span>

* <span data-ttu-id="80551-125">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="80551-125">Generic Web Services:</span></span>
  * <span data-ttu-id="80551-126">Nástroj Wsconfig Hello nepřevádět správně pole Json hello z "Ukázková žádost" pro metodu služby REST hello.</span><span class="sxs-lookup"><span data-stu-id="80551-126">hello Wsconfig tool did not convert correctly hello Json array from "sample request" for hello REST service method.</span></span> <span data-ttu-id="80551-127">Toto pole Json pro požadavek REST hello příčinou problémů s serializace.</span><span class="sxs-lookup"><span data-stu-id="80551-127">This caused problems with serialization this Json array for hello REST request.</span></span>
  * <span data-ttu-id="80551-128">Webové služby konektoru konfigurační nástroj nepodporuje názvy atributů JSON využití místa symbolů</span><span class="sxs-lookup"><span data-stu-id="80551-128">Web Service Connector Configuration Tool does not support usage of space symbols in JSON attribute names</span></span> 
    * <span data-ttu-id="80551-129">Vzor nahrazení lze přidat ručně toohello WSConfigTool.exe.config souboru, například```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span><span class="sxs-lookup"><span data-stu-id="80551-129">A Substitution pattern can be added manually toohello WSConfigTool.exe.config file, e.g. ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span></span>

* <span data-ttu-id="80551-130">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="80551-130">Lotus Notes:</span></span>
  * <span data-ttu-id="80551-131">Když hello možnost **povolit vlastní udělení licence certifikátorům pro organizace nebo organizační jednotky** je zakázána, pak hello konektoru selže při exportu (Update) po exportu hello toku všechny atributy jsou exportovaný tooDomino, ale v době hello Export KeyNotFoundException se vrátí tooSync.</span><span class="sxs-lookup"><span data-stu-id="80551-131">When hello option **Allow custom certifiers for Organization/Organizational Units** is disabled then hello connector fails during export (Update) After hello export flow all attributes are exported tooDomino but at hello time of export a KeyNotFoundException is returned tooSync.</span></span> 
    * <span data-ttu-id="80551-132">To je způsobeno hello přejmenovat operace selže, když se ho pokusí toochange rozlišující název (uživatelské jméno atribut) tak, že změníte jeden z atributů hello níže:</span><span class="sxs-lookup"><span data-stu-id="80551-132">This happens because hello rename operation fails when it tries toochange DN (UserName attribute) by changing one of hello attributes below:</span></span>  
      - <span data-ttu-id="80551-133">Příjmení</span><span class="sxs-lookup"><span data-stu-id="80551-133">LastName</span></span>
      - <span data-ttu-id="80551-134">FirstName</span><span class="sxs-lookup"><span data-stu-id="80551-134">FirstName</span></span>
      - <span data-ttu-id="80551-135">Indexována</span><span class="sxs-lookup"><span data-stu-id="80551-135">MiddleInitial</span></span>
      - <span data-ttu-id="80551-136">AltFullName</span><span class="sxs-lookup"><span data-stu-id="80551-136">AltFullName</span></span>
      - <span data-ttu-id="80551-137">AltFullNameLanguage</span><span class="sxs-lookup"><span data-stu-id="80551-137">AltFullNameLanguage</span></span>
      - <span data-ttu-id="80551-138">organizační jednotky</span><span class="sxs-lookup"><span data-stu-id="80551-138">ou</span></span>
      - <span data-ttu-id="80551-139">altcommonname</span><span class="sxs-lookup"><span data-stu-id="80551-139">altcommonname</span></span>

  * <span data-ttu-id="80551-140">Když **povolit vlastní udělení licence certifikátorům pro organizace nebo organizační jednotky** možnost povolena, ale vyžaduje udělení licence certifikátorům jsou stále prázdné, dojde k KeyNotFoundException.</span><span class="sxs-lookup"><span data-stu-id="80551-140">When **Allow custom certifiers for Organization/Organizational Units** option is enabled, but required certifiers are still empty, then KeyNotFoundException occurs.</span></span>

### <a name="enhancements"></a><span data-ttu-id="80551-141">Vylepšení:</span><span class="sxs-lookup"><span data-stu-id="80551-141">Enhancements:</span></span>

* <span data-ttu-id="80551-142">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="80551-142">Generic SQL:</span></span>
  * <span data-ttu-id="80551-143">**Scénář: přepracovali Neimplementováno:** "*" funkce</span><span class="sxs-lookup"><span data-stu-id="80551-143">**Scenario: redesigned Implemented:** "*" feature</span></span>
  * <span data-ttu-id="80551-144">**Popis řešení:** změnit metodu pro [odkaz na více hodnot atributů zpracování](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="80551-144">**Solution description:** Changed approach for [multi-valued reference attributes handling](active-directory-aadconnectsync-connector-genericsql.md).</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="80551-145">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="80551-145">Fixed issues:</span></span>

* <span data-ttu-id="80551-146">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="80551-146">Generic Web Services:</span></span>
  * <span data-ttu-id="80551-147">Nelze importovat konfiguraci serveru, pokud je k dispozici webovou službu konektoru</span><span class="sxs-lookup"><span data-stu-id="80551-147">Can’t import Server configuration if WebService Connector is present</span></span>
  * <span data-ttu-id="80551-148">Webová služba konektoru nepracuje s více webových služeb</span><span class="sxs-lookup"><span data-stu-id="80551-148">WebService Connector is not working with multiple  Web Services</span></span>

* <span data-ttu-id="80551-149">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="80551-149">Generic SQL:</span></span>
  * <span data-ttu-id="80551-150">Žádné typy objektů jsou uvedené pro jednu hodnotu Odkazovaný atribut</span><span class="sxs-lookup"><span data-stu-id="80551-150">No object types are listed for single value referenced attribute</span></span>
  * <span data-ttu-id="80551-151">Rozdílový import na sledování změn strategie odstraní objekt při hodnota se odebere z tabulky s více hodnotami</span><span class="sxs-lookup"><span data-stu-id="80551-151">Delta import on Change Tracking strategy deletes object when value is removed from multi-value table</span></span>
  * <span data-ttu-id="80551-152">OverflowException v konektoru GSQL s DB2 na AS / 400</span><span class="sxs-lookup"><span data-stu-id="80551-152">OverflowException in GSQL connector with DB2 on AS/400</span></span>

<span data-ttu-id="80551-153">Lotus:</span><span class="sxs-lookup"><span data-stu-id="80551-153">Lotus:</span></span>
  * <span data-ttu-id="80551-154">Přidání možnost tooenable\disable hledání organizační jednotky před otevřením GlobalParameters stránky</span><span class="sxs-lookup"><span data-stu-id="80551-154">Added option tooenable\disable searching OUs before opening GlobalParameters page</span></span>

## <a name="114430"></a><span data-ttu-id="80551-155">1.1.443.0</span><span class="sxs-lookup"><span data-stu-id="80551-155">1.1.443.0</span></span>

<span data-ttu-id="80551-156">Vydáno: 2017 března</span><span class="sxs-lookup"><span data-stu-id="80551-156">Released: 2017 March</span></span>

### <a name="enhancements"></a><span data-ttu-id="80551-157">Vylepšení</span><span class="sxs-lookup"><span data-stu-id="80551-157">Enhancements</span></span>

* <span data-ttu-id="80551-158">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="80551-158">Generic SQL:</span></span></br><span data-ttu-id="80551-159">
  **Scénář příznaky:** je dobře známé omezení s hello konektor SQL, kde jsme pouze povolit typu odkaz tooone objektu a vyžadují křížových odkazů se členy.</span><span class="sxs-lookup"><span data-stu-id="80551-159">
  **Scenario Symptoms:**  It is a well-known limitation with hello SQL Connector where we only allow a reference tooone object type and require cross reference with members.</span></span> </br><span data-ttu-id="80551-160">
**Popis řešení:** v krok zpracování hello odkazů na byly "*" zvolíte možnost, všechny kombinace typů objektů bude vrácen zpět toohello synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="80551-160">
**Solution description:** In hello processing step for references were "*" option is chosen, ALL combinations of object types will be returned back toohello sync engine.</span></span>

>[!Important]
- <span data-ttu-id="80551-161">Tím se vytvoří mnoho zástupné symboly</span><span class="sxs-lookup"><span data-stu-id="80551-161">This will create many placeholders</span></span>
- <span data-ttu-id="80551-162">Je požadováno toomake se, že názvy hello je jedinečný pro různé typy objektů.</span><span class="sxs-lookup"><span data-stu-id="80551-162">It is required toomake sure hello naming is unique cross object types.</span></span>


* <span data-ttu-id="80551-163">Obecné LDAP:</span><span class="sxs-lookup"><span data-stu-id="80551-163">Generic LDAP:</span></span></br><span data-ttu-id="80551-164">
 **Scénář:** na konkrétní oddíl je vybrána pouze několik kontejnerů, pak hello vyhledávání stále bude provedeno v celý oddíl.</span><span class="sxs-lookup"><span data-stu-id="80551-164">
**Scenario:** When only few containers are selected in specific partition, then hello search still will be done in whole partition.</span></span> <span data-ttu-id="80551-165">Konkrétní bude filtrováno podle synchronizační služba, ale ne podle MA, což by mohlo způsobit snížení výkonu.</span><span class="sxs-lookup"><span data-stu-id="80551-165">Specific will be filtered by Synchronization Service, but not by MA which might cause performance degradation.</span></span> </br>

 <span data-ttu-id="80551-166">**Popis řešení:** toomake kód změnit GLDAP konektor možné projít všechny kontejnery a hledání objektů v každé z nich, namísto hledání v celé oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="80551-166">**Solution description:** Changed GLDAP connector's code toomake it possible go through all containers and search objects in each of them, instead of searching in hello whole partition.</span></span>


* <span data-ttu-id="80551-167">Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="80551-167">Lotus Domino:</span></span>

  <span data-ttu-id="80551-168">**Scénář:** podporu odstranění Domino pošty pro odebrání osoba během exportu.</span><span class="sxs-lookup"><span data-stu-id="80551-168">**Scenario:** Domino mail deletion support for a person removal during an export.</span></span> </br><span data-ttu-id="80551-169">
  **Řešení:** podporu odstranění konfigurovat e-mailu pro odebrání osoba během exportu.</span><span class="sxs-lookup"><span data-stu-id="80551-169">
**Solution:** Configurable mail deletion support for a person removal during an export.</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="80551-170">Opravené problémy:</span><span class="sxs-lookup"><span data-stu-id="80551-170">Fixed issues:</span></span>
* <span data-ttu-id="80551-171">Obecné webové služby:</span><span class="sxs-lookup"><span data-stu-id="80551-171">Generic Web Services:</span></span>
 * <span data-ttu-id="80551-172">Při změně adresy URL služby hello ve výchozím SAP wsconfig projekty prostřednictvím webové služby konfigurační nástroj pak se stane hello následující chyba: Nelze najít část cesty hello</span><span class="sxs-lookup"><span data-stu-id="80551-172">When changing hello service URL in Default SAP wsconfig projects through WebService Configuration Tool then hello following error happens: Could not find a part of hello path</span></span>

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* <span data-ttu-id="80551-173">Obecné LDAP:</span><span class="sxs-lookup"><span data-stu-id="80551-173">Generic LDAP:</span></span>
 * <span data-ttu-id="80551-174">Konektor GLDAP nejsou vidět všechny atributy ve službě AD LDS</span><span class="sxs-lookup"><span data-stu-id="80551-174">GLDAP Connector does not see all attributes in AD LDS</span></span>
 * <span data-ttu-id="80551-175">Průvodce zalomení při zjištění žádné atributy UPN ze schématu adresáře LDAP hello</span><span class="sxs-lookup"><span data-stu-id="80551-175">Wizard breaks when no UPN attributes are detected from hello LDAP directory schema</span></span>
 * <span data-ttu-id="80551-176">Rozdílová importy selhání s chybami zjišťování nejsou k dispozici během úplný import, pokud není vybraná atribut "objectclass"</span><span class="sxs-lookup"><span data-stu-id="80551-176">Delta Imports Failing with discovery errors not present during full import, when "objectclass" attribute is not selected</span></span>
 * <span data-ttu-id="80551-177">Na stránce konfigurace "Konfigurace oddílů a hierarchií", nezobrazí všechny objekty, který typ je rovna toohello oddílu pro nové servery v hello obecné</span><span class="sxs-lookup"><span data-stu-id="80551-177">A "Configure Partitions and Hierarchies” configuration page, doesn’t show any objects which type is equal toohello partition for Novel servers in hello Generic</span></span>  
<span data-ttu-id="80551-178">LDAP MA.</span><span class="sxs-lookup"><span data-stu-id="80551-178">LDAP MA.</span></span> <span data-ttu-id="80551-179">Ukázalo se pouze objekty z oddílu RootDSE.</span><span class="sxs-lookup"><span data-stu-id="80551-179">They showed only objects from RootDSE partition.</span></span>


* <span data-ttu-id="80551-180">Obecné SQL:</span><span class="sxs-lookup"><span data-stu-id="80551-180">Generic SQL:</span></span>
 * <span data-ttu-id="80551-181">Opravte pro obecné SQL vodoznak chyb vícehodnotový atribut není importován rozdílový Import</span><span class="sxs-lookup"><span data-stu-id="80551-181">Fix for Generic SQL watermark Delta Import multivalued attribute not imported bug</span></span>
 * <span data-ttu-id="80551-182">Při exportu deleted\added hodnoty atributu s více hodnotami, nejsou deleted\added ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="80551-182">When exporting deleted\added values of multivalued attribute, they are not deleted\added in data source.</span></span>  


* <span data-ttu-id="80551-183">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="80551-183">Lotus Notes:</span></span>
 * <span data-ttu-id="80551-184">Konkrétní pole, které "Úplný název" se zobrazí v úložišti metaverse hello správně ale při exportu tooNotes hello hodnotu pro atribut hello má hodnotu Null nebo prázdný.</span><span class="sxs-lookup"><span data-stu-id="80551-184">A specific field "Full Name" is shown in hello metaverse correctly however when exporting tooNotes hello value for hello attribute is Null or Empty.</span></span>
 * <span data-ttu-id="80551-185">Opravit duplicitní Certifier došlo k chybě</span><span class="sxs-lookup"><span data-stu-id="80551-185">Fix for duplicate Certifier error</span></span>
 * <span data-ttu-id="80551-186">Pokud je vybrána hello objektu bez všechna data na hello konektoru Lotus Domino s jinými objekty potom jsme chyba se zobrazí hello zjišťování při provádění úplné importu.</span><span class="sxs-lookup"><span data-stu-id="80551-186">When hello Object without any data is selected on hello Lotus Domino Connector with other objects then we receive hello Discovery error while performing Full-Import.</span></span>
 * <span data-ttu-id="80551-187">Pokud se rozdílový Import se službou na hello konektoru Lotus Domino na konci hello této spuštění, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe někdy vrátí chybu aplikace.</span><span class="sxs-lookup"><span data-stu-id="80551-187">When Delta Import is being running on hello Lotus Domino Connector, at hello end of that run, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service sometimes returns an Application Error.</span></span>
 * <span data-ttu-id="80551-188">Skupiny členství celkové funguje bez problémů a zůstane zachované, s výjimkou při spuštění hello export tootry tooremove uživatele z členství se zobrazí jako úspěšně dokončený s aktualizací, ale není získat z členství v Lotus Notes ve skutečnosti odebrán uživatel hello.</span><span class="sxs-lookup"><span data-stu-id="80551-188">Group membership overall works fine and is maintained, except when running hello export tootry tooremove a user from membership it shows as successful with an update, but hello user doesn’t actually get removed from membership in Lotus Notes.</span></span>
 * <span data-ttu-id="80551-189">Režim toochoose příležitost exportu jako "Připojení položka v dolní části" byl přidán v konfiguraci grafického uživatelského rozhraní Lotus MA tooappend nové položky v dolní části během exportu hello více hodnot atributů.</span><span class="sxs-lookup"><span data-stu-id="80551-189">An opportunity toochoose mode of export as “Append Item at bottom” was added in configuration GUI of Lotus MA tooappend new items at bottom during hello export for multi-valued attributes.</span></span>
 * <span data-ttu-id="80551-190">Konektor přidá hello potřebné logiky toodelete hello soubor z hello složku pošty a ID trezoru.</span><span class="sxs-lookup"><span data-stu-id="80551-190">Connector will add hello needed logic toodelete hello file from hello Mail Folder and ID Vault.</span></span>
 * <span data-ttu-id="80551-191">Odstraňte členství nefunguje mezi NAB člen.</span><span class="sxs-lookup"><span data-stu-id="80551-191">Delete membership not working for cross NAB member.</span></span>
 * <span data-ttu-id="80551-192">Hodnoty by měla být úspěšně odstraněna z vícehodnotového atributu</span><span class="sxs-lookup"><span data-stu-id="80551-192">Values should be successfully deleted from multi-valued attribute</span></span>

## <a name="111170"></a><span data-ttu-id="80551-193">1.1.117.0</span><span class="sxs-lookup"><span data-stu-id="80551-193">1.1.117.0</span></span>
<span data-ttu-id="80551-194">Vydáno: března 2016</span><span class="sxs-lookup"><span data-stu-id="80551-194">Released: 2016 March</span></span>

<span data-ttu-id="80551-195">**Nový konektor**</span><span class="sxs-lookup"><span data-stu-id="80551-195">**New Connector**</span></span>  
<span data-ttu-id="80551-196">Počáteční verze hello [obecné konektor SQL](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="80551-196">Initial release of hello [Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md).</span></span>

<span data-ttu-id="80551-197">**Nové funkce:**</span><span class="sxs-lookup"><span data-stu-id="80551-197">**New features:**</span></span>

* <span data-ttu-id="80551-198">Generický konektor LDAP:</span><span class="sxs-lookup"><span data-stu-id="80551-198">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="80551-199">Přidaná podpora pro Rozdílový import s Isode.</span><span class="sxs-lookup"><span data-stu-id="80551-199">Added support for delta import with Isode.</span></span>
* <span data-ttu-id="80551-200">Konektor webových služeb:</span><span class="sxs-lookup"><span data-stu-id="80551-200">Web Services Connector:</span></span>
  * <span data-ttu-id="80551-201">Aktualizované hello csEntryChangeResult aktivity a setImportErrorCode aktivity tooallow objekt úrovně chyby toobe vrácený back toohello synchronizační modul.</span><span class="sxs-lookup"><span data-stu-id="80551-201">Updated hello csEntryChangeResult activity and setImportErrorCode activity tooallow object level errors toobe returned back toohello sync engine.</span></span>
  * <span data-ttu-id="80551-202">Aktualizované hello SAP6 a SAP6User šablony toouse hello nový objekt úrovně Chyba funkce.</span><span class="sxs-lookup"><span data-stu-id="80551-202">Updated hello SAP6 and SAP6User templates toouse hello new object level error functionality.</span></span>
* <span data-ttu-id="80551-203">Konektoru Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="80551-203">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="80551-204">Pro export budete potřebovat jeden certifier na adresář.</span><span class="sxs-lookup"><span data-stu-id="80551-204">For export, you need one certifier per address book.</span></span> <span data-ttu-id="80551-205">Můžete teď hello použijte stejné heslo pro všechny udělení licence certifikátorům toomake hello správu jednodušší.</span><span class="sxs-lookup"><span data-stu-id="80551-205">You can now use hello same password for all certifiers toomake hello management easier.</span></span>

<span data-ttu-id="80551-206">**Opravené problémy:**</span><span class="sxs-lookup"><span data-stu-id="80551-206">**Fixed issues:**</span></span>

* <span data-ttu-id="80551-207">Generický konektor LDAP:</span><span class="sxs-lookup"><span data-stu-id="80551-207">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="80551-208">Pro IBM Tivoli DS nebyla zjištěna správně některé atributy typu odkaz.</span><span class="sxs-lookup"><span data-stu-id="80551-208">For IBM Tivoli DS, some reference attributes were not detected correctly.</span></span>
  * <span data-ttu-id="80551-209">Otevřete protokol během Rozdílový import bylo zkráceno prázdné znaky na hello začátku a konci řetězce.</span><span class="sxs-lookup"><span data-stu-id="80551-209">For Open LDAP during a delta import, whitespaces at hello beginning and end of strings were truncated.</span></span>
  * <span data-ttu-id="80551-210">Novell a NetIQ export, přesunout objekt mezi organizační jednotky nebo kontejnery a na hello stejný čas přejmenovat hello objektu se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="80551-210">For Novell and NetIQ, an export that moved an object between OUs/containers and at hello same time renamed hello object failed.</span></span>
* <span data-ttu-id="80551-211">Konektor webových služeb:</span><span class="sxs-lookup"><span data-stu-id="80551-211">Web Services Connector:</span></span>
  * <span data-ttu-id="80551-212">Pokud webová služba hello měli víc koncových bodů pro stejnou vazbu, pak hello konektor není zjistit správně těchto koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="80551-212">If hello web service had multiple end-points for same binding, then hello Connector did not correctly discover these end-points.</span></span>
* <span data-ttu-id="80551-213">Konektoru Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="80551-213">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="80551-214">Exportu hello fullName atribut tooa e-mailu v databázi nebyla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="80551-214">An export of hello fullName attribute tooa mail-in database did not work.</span></span>
  * <span data-ttu-id="80551-215">Export, který jak přidat nebo odebrat člena ze skupiny jen exportovaný hello přidat členy.</span><span class="sxs-lookup"><span data-stu-id="80551-215">An export which both added and removed member from a group only exported hello added members.</span></span>
  * <span data-ttu-id="80551-216">Pokud poznámky k dokumentu je neplatný (hello atribut isValid nastavit toofalse), pak hello konektoru selže.</span><span class="sxs-lookup"><span data-stu-id="80551-216">If a Notes Document is invalid (hello attribute isValid set toofalse), then hello Connector fails.</span></span>

## <a name="older-releases"></a><span data-ttu-id="80551-217">Starší verze</span><span class="sxs-lookup"><span data-stu-id="80551-217">Older releases</span></span>
<span data-ttu-id="80551-218">Před. března 2016 byly vydané hello konektory jako témata týkající se podpory.</span><span class="sxs-lookup"><span data-stu-id="80551-218">Before March 2016, hello Connectors were released as support topics.</span></span>

<span data-ttu-id="80551-219">**Obecné LDAP**</span><span class="sxs-lookup"><span data-stu-id="80551-219">**Generic LDAP**</span></span>

* <span data-ttu-id="80551-220">[KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, září 2015</span><span class="sxs-lookup"><span data-stu-id="80551-220">[KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="80551-221">[KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, března 2015</span><span class="sxs-lookup"><span data-stu-id="80551-221">[KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="80551-222">[KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, leden 2015</span><span class="sxs-lookup"><span data-stu-id="80551-222">[KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 January</span></span>
* <span data-ttu-id="80551-223">[KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, září 2014</span><span class="sxs-lookup"><span data-stu-id="80551-223">[KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 September</span></span>
* <span data-ttu-id="80551-224">[KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, března 2014</span><span class="sxs-lookup"><span data-stu-id="80551-224">[KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 March</span></span>

<span data-ttu-id="80551-225">**Webovým službám**</span><span class="sxs-lookup"><span data-stu-id="80551-225">**WebServices**</span></span>

* <span data-ttu-id="80551-226">[KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, září 2014</span><span class="sxs-lookup"><span data-stu-id="80551-226">[KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="80551-227">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="80551-227">**PowerShell**</span></span>

* <span data-ttu-id="80551-228">[KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, září 2014</span><span class="sxs-lookup"><span data-stu-id="80551-228">[KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="80551-229">**Lotus Domino**</span><span class="sxs-lookup"><span data-stu-id="80551-229">**Lotus Domino**</span></span>

* <span data-ttu-id="80551-230">[KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, září 2015</span><span class="sxs-lookup"><span data-stu-id="80551-230">[KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="80551-231">[KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, března 2015</span><span class="sxs-lookup"><span data-stu-id="80551-231">[KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="80551-232">[KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, srpen 2014</span><span class="sxs-lookup"><span data-stu-id="80551-232">[KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 August</span></span>
* <span data-ttu-id="80551-233">[KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003 února 2014</span><span class="sxs-lookup"><span data-stu-id="80551-233">[KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014 February</span></span>  
* <span data-ttu-id="80551-234">[KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, říjen 2013</span><span class="sxs-lookup"><span data-stu-id="80551-234">[KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 October</span></span>
* <span data-ttu-id="80551-235">[KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, srpen 2013</span><span class="sxs-lookup"><span data-stu-id="80551-235">[KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 August</span></span>

## <a name="next-steps"></a><span data-ttu-id="80551-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80551-236">Next steps</span></span>
<span data-ttu-id="80551-237">Další informace o hello [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.</span><span class="sxs-lookup"><span data-stu-id="80551-237">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="80551-238">Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="80551-238">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
