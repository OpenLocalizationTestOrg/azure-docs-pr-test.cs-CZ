---
title: "aaaTroubleshoot pomalé zálohování souborů a složek ve službě Azure Backup | Microsoft Docs"
description: "Poskytuje řešení potíží s pokyny toohelp diagnostikovat hello příčinu problémů s výkonem Azure Backup"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: e379180a-db13-4e0c-90e4-28e5dd6f5b14
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 21d79bbd03c2706bc43fcc7c14020cffd6b919c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Řešení potíží s pomalým zálohováním souborů a složek ve službě Azure Backup
Tento článek obsahuje pokyny k odstraňování problémů toohelp, můžete diagnostikovat, aby hello příčinu pomalý výkon zálohování pro soubory a složky, pokud používáte Azure Backup. Pokud používáte Azure Backup agent tooback hello soubory, proces zálohování hello může trvat déle než obvykle. Toto zpoždění může být způsobena jednou nebo více hello následující:

* [Existují kritické hello počítače, který se záloha.](#cause1)
* [Jiný proces nebo antivirový software narušuje hello proces zálohování Azure.](#cause2)
* [agent zálohování Hello je spuštěn na virtuální počítač Azure (VM).](#cause3)  
* [Zálohujete velký počet (miliony) soubory.](#cause4)

Než začnete, řešení potíží, doporučujeme stáhnout a nainstalovat hello [nejnovější verze agenta Azure Backup](http://aka.ms/azurebackup_agent). Jsme usnadnění časté aktualizace toohello zálohování agenta toofix různé problémy, přidat funkce a zlepšení výkonu.

Také důrazně doporučujeme, abyste si prošli hello [nejčastější dotazy týkající se služby Azure Backup](backup-azure-backup-faq.md) toomake, zda nejsou některé běžné problémy s konfigurací hello vyskytují.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>

## <a name="cause-performance-bottlenecks-on-hello-computer"></a>Příčina: Kritické body v počítači hello
Kritická místa hello počítače, který je zálohovaných může způsobit zpoždění. Například hello počítače možnost tooread nebo zápisu toodisk nebo dostupnou šířku pásma toosend data přes síť hello mohou způsobit kritická místa.

Systém Windows nabízí integrované nástroj, který se nazývá [sledování výkonu](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) toodetect (Perfmon) tyto kritická místa.

Tady jsou některé čítače výkonu a rozsahy, které může být užitečné při diagnostikování kritická místa pro optimální zálohy.

| Čítač | Status |
| --- | --- |
| Logický Disk (fyzického disku)--% nečinnosti |• 100 % nečinnosti too50 % nečinnosti = stavu v pořádku</br>• 49 % nečinnosti too20 % nečinnosti = upozornění nebo monitorování</br>• nečinnosti too0 19 % % nečinnosti = kritický nebo mimo specifikace |
| Logický Disk (fyzického disku)--% střední Doba disku čtení nebo zápisu |• 0,001 ms too0.015 ms = stavu v pořádku</br>• 0,015 ms too0.025 ms = upozornění nebo monitorování</br>• 0.026 ms nebo již = kritický nebo mimo specifikace |
| Logický Disk (fyzického disku)--aktuální délka fronty disku (pro všechny instance) |80 požadavků po dobu více než 6 minut |
| Paměť – bajty stránkovaného fondu jiný |• Méně než 60 % fondu spotřebované = stavu v pořádku<br>• 61 % too80 % fondu spotřebované = upozornění nebo monitorování</br>• Větší než 80 % fondu spotřebované = kritický nebo mimo specifikace |
| Paměť – bajty stránkovaného fondu |• Méně než 60 % fondu spotřebované = stavu v pořádku</br>• 61 % too80 % fondu spotřebované = upozornění nebo monitorování</br>• Větší než 80 % fondu spotřebované = kritický nebo mimo specifikace |
| Paměť – dostupný počet megabajtů |• 50 % volné paměti k dispozici nebo více = stavu v pořádku</br>• 25 % volné paměti k dispozici = monitorování</br>• 10 % volné paměti k dispozici = upozornění</br>• Méně než 100 MB nebo 5 % volné paměti k dispozici = kritický nebo mimo specifikace |
| Procesor –\%času procesoru (všechny instance) |• Méně než 60 % spotřebované = stavu v pořádku</br>• 61 % too90 % spotřebované = monitorování nebo upozornění</br>• 91 % too100 % spotřebované = kritický |

> [!NOTE]
> Pokud zjistíte, že je infrastruktura hello hello který, doporučujeme, aby defragmentaci disku hello pravidelně pro lepší výkon.
>
>

<a id="cause2"></a>

## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Příčina: Jiného procesu nebo antivirový software zasahovala do činnosti Azure Backup
Jsme viděli několik instancí, kde jiné procesy v systému Windows hello negativně ovlivňovat výkon hello proces agenta Azure Backup. Například pokud používáte agenta Azure Backup hello i jiný program tooback zálohovat data, nebo pokud je spuštěn antivirový software a má zámku na soubory toobe zálohovat, hello více uzamčení souborů může způsobit kolizí. V takovém případě hello zálohování může selhat nebo hello úlohy může trvat déle, než se očekávalo.

Hello nejlepší doporučení v tomto scénáři se tooturn vypnout hello jiných toosee zálohovacího programu, jestli hello čas zálohování pro agenta Azure Backup hello změny. Obvykle, a ujistěte se, že nejsou spuštěné několik úloh zálohování na hello současně je dostatečná tooprevent je ze které mají vliv na sebe navzájem.

Pro antivirové programy, doporučujeme, abyste vyloučili hello následující soubory a umístění:

* C:\Program Files\Microsoft Azure Recovery Services Agent\bin\cbengine.exe jako proces
* Složky C:\Program Files\Microsoft Azure Recovery Services Agent\
* Cesta k pomocnému umístění (Pokud nepoužíváte hello standardní umístění)

<a id="cause3"></a>

## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Příčina: Zálohování agenta spuštěného na virtuální počítač Azure
Pokud používáte hello Backup agent na virtuálním počítači, bude výkon nižší než při jeho spuštění na fyzickém počítači. Očekává se z důvodu omezení tooIOPS.  Však můžete optimalizovat výkon hello přepínání hello datové jednotky, které jsou zálohované tooAzure Storage úrovně Premium. Pracujeme na řešení tohoto problému, a oprava hello bude k dispozici v budoucí verzi.

<a id="cause4"></a>

## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Příčina: Zálohování velký počet (miliony) souborů
Přesunutí velký objem dat bude trvat déle než přesouvání menší objem dat. V některých případech je čas zálohování související toonot pouze hello velikost hello data, ale také počet hello soubory nebo složky. To platí hlavně při milióny malých souborů (několik tooa bajtů několika kilobajtů) jsou zálohovány.

K tomuto chování dochází, protože když jste zálohování dat hello a přesunutím tooAzure, Azure je současně katalogizaci vaše soubory. V některých scénářích výjimečných hello katalogu operace může trvat déle než obvykle.

Hello následující ukazatele vám může pomoct pochopit hello problémová místa a odpovídajícím způsobem pracovat na hello další kroky:

* **Uživatelské rozhraní se zobrazuje průběh pro přenos dat hello**. stále probíhá přenosu dat Hello. Hello šířku pásma sítě nebo hello velikost dat může být příčinou zpoždění.
* **Uživatelského rozhraní není zobrazující průběh pro přenos dat hello**. Otevřete hello protokoly umístěné v C:\Microsoft Azure Recovery Services Agent\Temp a zkontrolujte hello FileProvider::EndData položku v protokolech hello. Tato položka označuje, že dokončení přenosu dat hello probíhá operace katalogu hello. Nemáte zrušení úloh zálohování hello. Místo toho počkejte trochu déle toofinish operaci katalogu hello. Pokud hello potíže potrvají, obraťte se na [podporu Azure](https://portal.azure.com/#create/Microsoft.Support).
