---
title: aaaManaging data Azure Automation | Microsoft Docs
description: "Tento článek obsahuje více témat pro správu prostředí Azure Automation.  Zahrnuje aktuálně uchovávání dat a zálohování Azure Automation zotavení po havárii ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: 2896f129-82e3-43ce-b9ee-a3860be0423a
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/02/201
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: 46a164d864c4956c90ab689ca159fff6f6c08028
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-automation-data"></a>Správa dat Azure Automation
Tento článek obsahuje více témat pro správu prostředí Azure Automation.

## <a name="data-retention"></a>Uchovávání dat
Pokud odstraníte prostředek ve službě Azure Automation, se uchovávají 90 dní pro auditování, než jsou trvale odstraněny.  Nelze zobrazit ani použít hello prostředků během této doby.  Tato zásada platí také tooresources, které patří tooan účet automation, který je odstraněn.

Automatizace Azure automaticky odstraní a úlohy, které jsou starší než 90 dnech trvale odstraní.

Hello následující tabulka shrnuje hello zásady uchovávání informací pro různé zdroje.

| Data | Zásada |
|:--- |:--- |
| Účty |Trvale odebrán 90 dnů po hello účet je odstraněný uživatel. |
| Prostředky |Trvale odebrán 90 dnů po hello asset je odstranit uživatelem, nebo po 90 dnech od hello je účet, který obsahuje hello asset odstraní uživatele. |
| Moduly |Trvale odebrán 90 dnů po hello modul je odstranit uživatelem, nebo po 90 dnech od hello je účet, který obsahuje modul hello odstraní uživatele. |
| Runbooky |Trvale odebrán 90 dnů po hello prostředek je odstraněn uživatelem, nebo po 90 dnech od hello je účet, který obsahuje prostředek hello odstraní uživatele. |
| Úlohy |Odstraněné a trvale odebrána po 90 dnech od poslední upravována. To může být po hello úloha dokončí, je zastavena nebo je pozastaveno. |
| Soubory konfigurace nebo MOF uzlu |Původní konfigurace uzlu, bude trvale odstraněn 90 dnů po vygenerování nové konfigurace uzlu. |
| Uzlů DSC |Trvale odebrány 90 dnů po zrušení registrace z účtu Automation pomocí portálu Azure nebo hello hello uzlu [Unregister-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) rutiny v prostředí Windows PowerShell. Uzly jsou také trvale odstraněny po 90 dnech od hello účet, který obsahuje, že uzel hello je odstraněn uživatelem. |
| Sestavy uzlu |Trvale odebrány 90 dnů po vygenerování nové sestavy pro tento uzel |

zásady uchovávání informací Hello platí tooall uživatelů a aktuálně nelze přizpůsobit.

Pokud potřebujete tooretain dat pro delší časové období, však může předat runbook úlohy protokoly tooLog Analytics.  Další informace najdete v tématu [předávat tooOMS data úlohy automatizace Azure Log Analytics](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Zálohování Azure Automation
Když odstraníte účet automation v Microsoft Azure, jsou všechny objekty v účtu hello odstraněny, včetně sady runbook, moduly, konfigurace, nastavení, úlohy a prostředky. objekty Hello nelze obnovit, po odstranění účtu hello.  Můžete použít následující informace toobackup hello obsah vašeho účtu automation před odstraněním jej hello. 

### <a name="runbooks"></a>Runbooky
Můžete exportovat soubory tooscript sady runbook pomocí portálu pro správu Azure hello nebo hello [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) rutiny v prostředí Windows PowerShell.  Tyto soubory skriptu lze importovat do jiného účtu automation, jak je popsáno v [vytvoření nebo import Runbooku](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Integrační moduly
Integrační moduly nelze exportovat z Azure Automation.  Je nutné zajistit, že jsou k dispozici mimo hello účet automation.

### <a name="assets"></a>Prostředky
Nelze exportovat [prostředky](https://msdn.microsoft.com/library/dn939988.aspx) z Azure Automation.  Pomocí hello portálu pro správu Azure, musíte si poznamenat hello podrobnosti proměnné, přihlašovací údaje, certifikátů, připojení a plány.  Pak musíte ručně vytvořit všechny prostředky, které jsou používány sady runbook, který importujete do jiné automatizace.

Můžete použít [rutiny Azure](https://msdn.microsoft.com/library/dn690262.aspx) tooretrieve podrobnosti nezašifrované prostředky a buď je uložíte pro budoucí reference nebo vytvořit ekvivalentní prostředky v jiný účet automation.

Nelze načíst hodnotu hello zašifrované proměnné nebo pole pro heslo hello přihlašovací údaje pomocí rutiny.  Pokud nevíte, tyto hodnoty, pak je můžete obnovit ze sady runbook pomocí hello [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) a [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) aktivity.

Certifikáty nelze exportovat z Azure Automation.  Je nutné zajistit, že všechny certifikáty, jsou k dispozici mimo Azure.

### <a name="dsc-configurations"></a>Konfigurace DSC
Můžete exportovat soubory tooscript konfigurace pomocí hello portálu pro správu Azure nebo hello [Export AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) rutiny v prostředí Windows PowerShell. Tyto konfigurace lze importovat a použít jiný účet automation.

## <a name="geo-replication-in-azure-automation"></a>Geografická replikace v Azure Automation
Geografická replikace, standard v účtech Azure Automation, zálohuje účet data tooa jiné zeměpisné oblasti redundanci. Můžete vybrat primární oblasti při nastavování účtu a pak sekundární oblasti je přiřazen tooit automaticky. v případě ztráty dat se průběžně aktualizuje Hello sekundární daty zkopírovanými z primární oblasti hello.  

Hello následující tabulka ukazuje dvojic dostupné primární a sekundární oblasti hello.

| Primární | Sekundární |
| --- | --- |
| Střed USA – jih |Střed USA – sever |
| USA – východ 2 |Střed USA |
| Západní Evropa |Severní Evropa |
| Jihovýchodní Asie |Východní Asie |
| Japonsko – východ |Japonsko – západ |

V případě pravděpodobně hello, že dojde ke ztrátě dat primární oblasti, Microsoft se pokusí toorecover ho. Pokud hello primární data nelze obnovit, se provádí geo-převzetí služeb při selhání a hello ovlivněných zákazníci budou informováni o tom prostřednictvím svoje předplatné.

