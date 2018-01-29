---
title: "Vytvoření a Správa skupin akce na portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet a spravovat skupiny akce na portálu Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: 05775415e210333cf63565e7b5b554d014f6ba23
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Vytvoření a Správa skupin akce na portálu Azure
## <a name="overview"></a>Přehled ##
Tento článek ukazuje, jak vytvořit a spravovat skupiny akce na portálu Azure.

Seznam akcí, můžete nakonfigurovat skupiny akcí. Tyto skupiny pak lze použít při definování aktivity protokolu výstrahy. Tyto skupiny můžete použít znovu pak Každá výstraha aktivity protokolu, které definujete, zajistíte, že jsou stejné akce trvá pokaždé, když se aktivuje výstraha aktivity protokolu.

Skupinu akce může mít až 10 každý typ akce. Každá akce se skládá z následujících vlastností:

* **Název**: Jedinečný identifikátor v rámci skupiny pro akce.  
* **Typ akce**: Odeslat zprávu SMS, e-mailovou zprávu, volat webhook, jehož, odesílání dat do nástroj na ITSM, volání Azure aplikace nebo spustit runbook služby automatizace.
* **Podrobnosti o**: odpovídající telefonního čísla, e-mailovou adresu, webhooku identifikátor URI nebo ITSM podrobnosti připojení.

Informace o tom, jak pomocí šablony Azure Resource Manager můžete nakonfigurovat skupiny akcí najdete v tématu [šablony správce prostředků skupiny akce](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Vytvořit skupinu akce pomocí portálu Azure ##
1. V [portál](https://portal.azure.com), vyberte **monitorování**. **Monitorování** slučuje okno veškeré monitorování nastavení a data v jednom zobrazení.

    ![Službu "Sledování"](./media/monitoring-action-groups/home-monitor.png)
2. V **nastavení** vyberte **skupiny akcí**.

    ![Na kartě "Akce skupiny"](./media/monitoring-action-groups/action-groups-blade.png)
3. Vyberte **přidat akci skupinu**a vyplňte příslušná pole.

    ![Příkaz "Přidat skupinu akce"](./media/monitoring-action-groups/add-action-group.png)
4. Zadejte název do pole **název skupiny akce** pole a zadejte název do pole **krátký název** pole. Krátký název se používá namísto názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.

      ![Dialogové okno Přidat skupinu akce"](./media/monitoring-action-groups/action-group-define.png)

5. **Předplatné** pole autofills s vaším aktuálním předplatným. Toto předplatné je ten, ve kterém je akce skupinu uložit.

6. Vyberte **skupiny prostředků** ve skupině akce je uložen.

7. Definujte seznam akcí, tím, že poskytuje každá akce:

    a. **Název**: Zadejte jedinečný identifikátor pro tuto akci.

    b. **Typ akce**: Vyberte SMS, e-mailu, webhooku, aplikace Azure, ITSM nebo sady Automation Runbook.

    c. **Podrobnosti o**: v závislosti na typu akce, zadejte telefonní číslo, e-mailovou adresu, webhooku identifikátor URI, aplikace Azure, ITSM připojení nebo sady Automation runbook. Pro akci ITSM kromě určit **pracovní položka** a vyžaduje vaše nástroje ITSM další pole. 

   > [!NOTE]
   > Akce ITSM vyžaduje připojení k ITSM. Naučte se vytvářet [ITSM připojení](../log-analytics/log-analytics-itsmc-overview.md). Akce ITSM aktuálně funguje pouze pro aktivitu protokolu výstrahy. Pro ostatní typy výstrah tato akce je aktuálně žádná operace.

8. Vyberte **OK** vytvořit skupinu akce.

## <a name="manage-your-action-groups"></a>Správa skupin akce ##
Jakmile vytvoříte skupinu akcí, se zobrazí na **skupiny akcí** části **monitorování** okno. Vyberte skupinu akce, kterou chcete spravovat:

* Přidat, upravit nebo odebrat akce.
* Odstraňte skupinu akce.

## <a name="next-steps"></a>Další kroky ##
* Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).  
* Získání [porozumět schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).  
* Další informace o [ITSM konektoru](../log-analytics/log-analytics-itsmc-overview.md)
* Další informace o [omezení rychlosti](monitoring-alerts-rate-limiting.md) výstrah. 
* Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.  
* Zjistěte, jak [Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).
