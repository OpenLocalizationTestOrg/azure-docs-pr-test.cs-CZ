---
title: "Správa dat Azure Automation | Microsoft Docs"
description: "Tento článek obsahuje více témat pro správu prostředí Azure Automation.  Zahrnuje aktuálně uchovávání dat a zálohování Azure Automation zotavení po havárii ve službě Azure Automation."
services: automation
documentationcenter: 
author: georgewallace
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
ms.openlocfilehash: 2afcf918ffa104bd0e13048c152e04992f55ffe1
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/06/2018
---
# <a name="managing-azure-automation-data"></a>Správa dat Azure Automation
Tento článek obsahuje více témat pro správu prostředí Azure Automation.

## <a name="data-retention"></a>Uchovávání dat
Pokud odstraníte prostředek ve službě Azure Automation, se uchovávají 90 dní pro auditování, než jsou trvale odstraněny.  Nelze zobrazit ani používat prostředek. během této doby.  Tato zásada platí také pro prostředky, které patří k účtu automation, který je odstraněn.

Automatizace Azure automaticky odstraní a úlohy, které jsou starší než 90 dnech trvale odstraní.

Následující tabulka shrnuje zásady uchovávání informací pro různé zdroje.

| Data | Zásada |
|:--- |:--- |
| Účty |Trvale odebrán 90 dnů po účet je odstraněn uživatelem. |
| Prostředky |Trvale odebrán 90 dnů po odstranění prostředku uživatelem nebo po 90 dnech od účet, který obsahuje, že je asset odstraní uživatele. |
| Moduly |Trvale odebrán 90 dnů po odstranění modulu uživatelem nebo 90 dnů po účet, který obsahuje, že je modul odstranit uživatelem. |
| Runbooky |Trvale odebrán 90 dnů po odstranění prostředku uživatelem nebo po 90 dnech od účet, který obsahuje, že je prostředek odstraní uživatele. |
| Úlohy |Odstraněné a trvale odebrána po 90 dnech od poslední upravována. To může být po dokončení úlohy, je zastavena nebo je pozastaveno. |
| Soubory konfigurace nebo MOF uzlu |Původní konfigurace uzlu, bude trvale odstraněn 90 dnů po vygenerování nové konfigurace uzlu. |
| Uzlů DSC |Trvale odebrány 90 dnů po zrušení registrace z účtu Automation pomocí portálu Azure uzlu nebo [Unregister-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) rutiny v prostředí Windows PowerShell. Uzly jsou také trvale odstraněny po 90 dnech od účet, který obsahuje, že uzel je odstraní uživatele. |
| Sestavy uzlu |Trvale odebrány 90 dnů po vygenerování nové sestavy pro tento uzel |

Zásady uchovávání informací platí pro všechny uživatele a aktuálně nelze přizpůsobit.

Ale pokud budete potřebovat zachování dat pro delší časové období, můžete dál runbook analýzy protokolů v protokolech úloh.  Další informace najdete v tématu [předávání dat úlohu Azure Automation k analýze protokolů OMS](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Zálohování Azure Automation
Pokud odstraníte účet automation v Microsoft Azure, se odstraní všechny objekty v účtu včetně sady runbook, moduly, konfigurace, nastavení, úlohy a prostředky. Objekty nelze obnovit, po odstranění účtu.  Chcete-li zálohovat obsah vašeho účtu automation před odstraněním jej můžete použít následující informace. 

### <a name="runbooks"></a>Runbooky
Vaše sady runbook můžete exportovat do skriptu soubory pomocí portálu Azure nebo [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) rutiny v prostředí Windows PowerShell.  Tyto soubory skriptu lze importovat do jiného účtu automation, jak je popsáno v [vytvoření nebo import Runbooku](https://msdn.microsoft.com/library/dn643637.aspx).

### <a name="integration-modules"></a>Integrační moduly
Integrační moduly nelze exportovat z Azure Automation.  Je nutné zajistit, že jsou k dispozici mimo účet automation.

### <a name="assets"></a>Prostředky
Nelze exportovat [prostředky](https://msdn.microsoft.com/library/dn939988.aspx) z Azure Automation.  Pomocí portálu Azure, musíte poznamenejte si podrobnosti proměnné, přihlašovací údaje, certifikátů, připojení a plány.  Pak musíte ručně vytvořit všechny prostředky, které jsou používány sady runbook, který importujete do jiné automatizace.

Můžete použít [rutiny Azure](https://msdn.microsoft.com/library/dn690262.aspx) načíst podrobnosti o nezašifrované prostředky a buď je uložíte pro budoucí použití nebo vytvoření ekvivalentní prostředky v jiný účet automation.

Nelze načíst hodnotu zašifrované proměnné nebo přihlašovací údaje pomocí rutiny pole pro heslo.  Pokud nevíte, tyto hodnoty, pak je můžete obnovit z runbooku pomocí [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) a [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) aktivity.

Certifikáty nelze exportovat z Azure Automation.  Je nutné zajistit, že všechny certifikáty, jsou k dispozici mimo Azure.

### <a name="dsc-configurations"></a>Konfigurace DSC
Vaše konfigurace můžete exportovat do skriptu soubory pomocí portálu Azure nebo [Export AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) rutiny v prostředí Windows PowerShell. Tyto konfigurace lze importovat a použít jiný účet automation.

## <a name="geo-replication-in-azure-automation"></a>Geografická replikace v Azure Automation
Geografická replikace, standard v účtech Azure Automation, zálohuje data účtu v jiné zeměpisné oblasti redundanci. Můžete vybrat primární oblasti při nastavování účtu a pak sekundární oblasti je k němu přiřazen automaticky. V případě ztráty dat se průběžně aktualizuje sekundární daty zkopírovanými z primární oblasti.  

Následující tabulka uvádí dvojic dostupné primární a sekundární oblasti.

| Primární | Sekundární |
| --- | --- |
| Střed USA – jih |Střed USA – sever |
| USA – východ 2 |Střed USA |
| Západní Evropa |Severní Evropa |
| Jihovýchodní Asie |Východní Asie |
| Japonsko – východ |Japonsko – západ |

V události nepravděpodobné, že dojde ke ztrátě dat primární oblasti Microsoft se pokusí jej obnovit. Pokud primární data nelze obnovit, pak se provádí geo-převzetí služeb při selhání a ovlivněných zákazníci budou informováni o tom prostřednictvím svoje předplatné.

