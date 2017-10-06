---
title: "informace o stavu aaaRetrieving pro úlohu Azure Import/Export | Microsoft Docs"
description: "Zjistěte, jak tooobtain stavu informace pro úlohy služby Microsoft Azure Import/Export."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 22d7e5f0-94da-49b4-a1ac-dd4c14a423c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.openlocfilehash: cbc35660519573d92f641924ac0025c9e577d69b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>Načítání informací o stavu pro úlohu importu/exportu
Můžete volat hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operace tooretrieve informace o obou import a export úloh. Hello vrátí následující informace:

-   Hello aktuální stav úlohy hello.

-   Přibližná procento Hello, každá úloha se dokončila.

-   Hello aktuální stav každé jednotky.

-   Identifikátory URI pro objekty BLOB obsahující protokoly chyb a podrobné informace o protokolování (Pokud je povoleno).

Hello následující části popisují informace hello vrácený hello `Get Job` operaci.

## <a name="job-states"></a>Stavy úlohy
Tabulka Hello a hello stavu obrázek popisují hello stavy, které úloha přejde prostřednictvím během jejich životního cyklu. aktuální stav úlohy hello Hello se dá určit pomocí volání hello `Get Job` operaci.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

Hello následující tabulka popisuje všechny stavy, které může předávat úlohu.

|Stav úlohy|Popis|
|---------------|-----------------|
|`Creating`|Po volání operace Put Job hello, vytvoření úlohy a její stav je nastaven příliš`Creating`. Když úloha hello hello `Creating` stavu hello službu Import/Export předpokládá hello jednotky nebyly uvidíte toohello datového centra. Úlohu může zůstat v hello `Creating` stavu pro až tootwo týdnů, po které se automaticky odstraní službou hello.<br /><br /> Pokud provádíte volání operace vlastnosti úlohy aktualizace hello během úlohy hello hello `Creating` stavu úlohy hello zůstane v hello `Creating` stavu a hello vypršení časového limitu je interval resetování tootwo týdnů.|
|`Shipping`|Po dodáte vašeho balíčku, by měly volat hello aktualizovat vlastnosti úlohy operace aktualizace hello stav úlohy hello příliš`Shipping`. Hello přesouvání stavu lze nastavit pouze v případě hello `DeliveryPackage` (poštovní poskytovatel a sledování Číslo) a hello `ReturnAddress` byly nastaveny pro úlohu hello.<br /><br /> Úloha Hello zůstane v hello stavu přesouvání až tootwo týdny. Pokud byly úspěšně dva týdny, a nebyly přijaty hello jednotky, operátory službu Import/Export hello budete upozorněni.|
|`Received`|Po přijetí všech jednotkách na hello datového centra stav úlohy hello nastavit toohello přijaté stavu.|
|`Transferring`|Jakmile hello jednotky byla u hello datového centra a alespoň jednu jednotku zahájení zpracování, bude stav úlohy hello nastaven toohello `Transferring` stavu. V tématu hello `Drive States` části níže podrobné informace.|
|`Packaging`|Po dokončení zpracování všech jednotkách, hello úlohy budou umístěny do hello `Packaging` stavu, dokud hello jednotky jsou dodané back toohello zákazníka.|
|`Completed`|Po všechny jednotky byly uvidíte back toohello zákazníka, pokud hello úlohy bez chyb, pak hello úlohy bude nastavena toohello `Completed` stavu. Hello úlohy budou automaticky odstraněny po 90 dnech v hello `Completed` stavu.|
|`Closed`|Po všechny jednotky byly uvidíte back toohello zákazníka, pokud zde nejsou žádné chyby během zpracování hello hello úlohy, pak hello úlohy bude nastavena toohello `Closed` stavu. Hello úlohy budou automaticky odstraněny po 90 dnech v hello `Closed` stavu.|

Můžete zrušit úlohu jenom na určité stavy. Zrušené úlohy přeskočí krok copy hello data, ale jinak postupuje hello stejné přechody stavu jako úlohu, která nebyla zrušena.

Hello následující tabulka popisuje chyby, které se můžou vyskytnout pro každý stav úlohy, jakož i hello vliv na hello úlohy, když dojde k chybě.

|Stav úlohy|Událost|Řešení / další kroky|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|Byly přijaty jedné nebo více jednotek pro úlohu, ale úloha hello není v hello `Shipping` stav nebo neexistuje žádný záznam hello úlohy ve službě hello.|Hello importu/exportu služby provozní tým bude pokus toocontact hello zákazníka toocreate nebo aktualizujte hello úlohy nezbytné informace toomove hello úlohy dál.<br /><br /> Pokud hello provozní tým nelze toocontact hello zákazníků během dvou týdnů, se pokusí hello provozní tým tooreturn hello jednotky.<br /><br /> V hello událost, která nemůže být vrácen hello jednotky a hello zákazníka nelze získat přístup budou bezpečně zničena hello jednotky za 90 dní.<br /><br /> Všimněte si, že úlohy nelze zpracovat, dokud jeho stav se aktualizuje příliš`Shipping`.|
|`Shipping`|Hello balíček pro úlohu nebyl doručen více než dva týdny.|Hello provozní tým oznámí hello zákazníka hello chybějící balíčku. Podle odpovědi hello zákazníka, hello provozní tým se rozšířit hello toowait interval pro balíček tooarrive hello nebo hello úlohu zrušit.<br /><br /> V případě hello tohoto zákazníka hello nelze kontaktovat nebo nereaguje do 30 dní, hello provozní tým zahájí akce toomove hello úlohu z hello `Shipping` stavu přímo toohello `Closed` stavu.|
|`Completed/Closed`|nikdy jednotky Hello dosaženo hello zpáteční adresu nebo jsou poškozené v dodávky (platí pouze tooan úloha exportu).|Pokud hello jednotky nedojde k dosažení hello zpáteční adresu, hello zákazník má první operace Get Job hello volání nebo zkontrolujte stav úlohy hello v hello portálu tooensure této hello, které byly dodány jednotky. Pokud bylo dodáno hello jednotky, by měl zákazník hello obraťte se na poskytovatele tootry přenosů hello a vyhledejte hello jednotky.<br /><br /> Pokud během dodávky jsou poškozené hello jednotky, hello může zákazník toorequest jiná úloha exportu nebo chybějící objekty BLOB hello stahování.|
|`Transferring/Packaging`|Úloha má nesprávnou nebo chybějící zpáteční adresu.|Hello provozní tým bude oslovení toohello kontaktní osoby pro hello úlohy tooobtain hello správnou adresu.<br /><br /> V případě hello tohoto zákazníka hello není dostupný, hello jednotky budou bezpečně zničena za 90 dní.|
|`Creating / Shipping/ Transferring`|Disk, který se nenachází v seznamu hello jednotky toobe importovat je součástí hello přesouvání balíčku.|Hello další jednotky nebudou zpracovány a bude vrácen toohello zákazníka po dokončení úlohy hello.|

## <a name="drive-states"></a>Stavy jednotky
Tabulka Hello a hello obrázek popisují životní cyklus hello jednotlivé jednotky jako přechází prostřednictvím úlohu import nebo export. Aktuální stav disku hello můžete načíst pomocí volání hello `Get Job` operace a zkontrolujete hello `State` element hello `DriveList` vlastnost.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

Hello následující tabulka popisuje všechny stavy, které může předávat na jednotku.

|Stav disku|Popis|
|-----------------|-----------------|
|`Specified`|Pro úlohu importu při vytvoření úlohy hello se hello operaci Put úlohy hello počáteční stav pro jednotku je hello `Specified` stavu. Pro úlohy exportu, protože není zadána žádná jednotka při vytvoření úlohy hello hello počáteční jednotky stav je hello `Received` stavu.|
|`Received`|jednotka Hello přejde toohello `Received` stavu hello při importu a exportu služby operátor zpracovala hello jednotky, které byly přijaty z hello přesouvání společnosti úlohy importu. Pro úlohy exportu, je stav počáteční jednotky hello hello `Received` stavu.|
|`NeverReceived`|jednotka Hello přesune toohello `NeverReceived` stavu hello při přijetí balíčku pro úlohu ale hello balíček neobsahuje hello jednotky. Jednotku také můžete přesunout do tohoto stavu, pokud to bylo dva týdny, protože služba hello přijala hello přenosů informace, ale hello balíček nebyl ještě byly přijaty hello datového centra.|
|`Transferring`|Jednotku přesune toohello `Transferring` stavu při hello služba zahájí tootransfer dat z jednotky tooWindows hello Azure Storage.|
|`Completed`|Jednotku přesune toohello `Completed` stavu, když má služba hello service úspěšně přenesla všechna data hello bez chyb.|
|`CompletedMoreInfo`|Jednotku přesune toohello `CompletedMoreInfo` stavu při hello služba zjistila některé problémy při kopírování dat buď z nebo toohello jednotku. Hello informace může obsahovat chyby, upozornění a informativní zprávy o přepsání objektů BLOB.|
|`ShippedBack`|jednotka Hello přesune toohello `ShippedBack` stav, když byl dodán z hello data center zpět toohello zpáteční adresu.|

Hello následující tabulka popisuje stavy selhání jednotky hello a hello akcí pro každý stav.

|Stav disku|Událost|Řešení / další krok|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|Jednotku, která je označena jako `NeverReceived` (protože nebyla přijata jako součást úlohy hello dodávky) dorazí v jiné dodávky.|Hello provozní tým přesune hello jednotky toohello `Received` stavu.|
|`N/A`|Jednotku, která není součástí všechny úlohy dorazí na hello datového centra v rámci jiné úlohy.|Hello jednotce budou označeny jako další jednotky a bude vrácen toohello zákazníka po dokončení úlohy hello přidružené k původní balíček hello.|

## <a name="faulted-states"></a>Chybný stav
Pokud se úloha nebo jednotku nezdaří tooprogress normálně prostřednictvím očekávané celou dobu životního cyklu, hello úlohy nebo jednotku bude přesunuta do `Faulted` stavu. V tomto bodě hello provozní tým vás bude kontaktovat zákazníka hello e-mailem nebo telefon. Jakmile hello problém nebude vyřešen, hello došlo k chybě úlohy, nebo jednotku se provedou mimo hello `Faulted` stavu a přesouvat do hello vhodné stavu.

## <a name="next-steps"></a>Další kroky

* [Pomocí REST API služby importu a exportu hello](storage-import-export-using-the-rest-api.md)
