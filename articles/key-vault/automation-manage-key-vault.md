---
title: "aaaManage Azure Key Vault pomocí Azure Automation | Microsoft Docs"
description: "Informace o tom, jak hello služby Azure Automation lze použít toomanage Azure Key Vault."
services: Key-Vault, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 4e780762-19b6-4ca6-b894-ebb44c538f35
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
ms.openlocfilehash: 7f46ecc1206a96e8aeb1d086285461cb5b205472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-key-vault-using-azure-automation"></a>Správa Azure Key Vault pomocí Azure Automation.
Tento průvodce vás seznámí toohello služba Azure Automation a jak může být použité toosimplify správy klíčů a tajných klíčů v Azure Key Vault.

## <a name="what-is-azure-automation"></a>Co je Azure Automation?
[Služby Azure Automation](../automation/automation-intro.md) je služba Azure pro zjednodušenou správu cloudu pomocí automatizace procesů a konfigurace požadovaného stavu. Pomocí Azure Automation, může být ruční, opakované, dlouhotrvajících a náchylné úlohy automatizované tooincrease spolehlivost, efektivitu a toovalue čas pro vaši organizaci.

Azure Automation nabízí modulu provádění vysoce spolehlivou a vysokou dostupností pracovního postupu, která je škálovatelná toomeet vašim potřebám. Ve službě Azure Automation procesů může být spuštěna ručně, 3. stran systémy, nebo v naplánovaných intervalech tak, aby úlohy dojít přesně v případě potřeby.

Snížit provozní režie a uvolněte IT a spustit automaticky DevOps zaměstnanci toofocus na práci, kterou přidá obchodní hodnotu přesunutím vaší toobe úlohy správy cloudu Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Jak Azure Automation pomoci spravovat Azure Key Vault?
Key Vault můžete spravovat ve službě Azure Automation pomocí hello [rutin AzureRM Key Vault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) a [rutin Azure Classic Key Vault](https://msdn.microsoft.com/library/azure/dn868052.aspx). Hello Azure modul pro správu classic Key Vault je dostupný automaticky ve službě Azure Automation a můžete importovat hello [AzureRM KeyVault modulu](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) do Azure Automation, aby mohli provést řadu vaší správy Key Vault úlohy v rámci služby hello. Tyto rutiny ve službě Azure Automation pomocí hello rutin pro ostatní služby Azure, tooautomate složité úlohy může také párovat napříč službami Azure a systémech 3. stran.

Pomocí rutiny Azure Key Vault hello můžete provádět tyto úlohy mimo jiné: 

* Vytvoření a konfigurace trezoru klíčů
* Vytvoření nebo import klíče
* Vytvořit nebo aktualizovat tajný klíč
* Aktualizovat atributy klíče
* Získat klíč nebo tajný klíč
* Odstranění klíče nebo tajného klíče

Zde je několik příkladů použití prostředí PowerShell toomanage Key Vault:  

* [Azure Key Vault - krok za krokem](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Nastavení a konfiguraci Azure Key Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)

## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili základy hello Azure Automation a jak může být použité toomanage Azure Key Vault, postupujte podle těchto odkazů toolearn Další informace o Azure Automation.

* V tématu hello Azure Automation [kurzu Začínáme](../automation/automation-first-runbook-graphical.md).
* V tématu hello [Azure Key Vault PowerShell skripty](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).

