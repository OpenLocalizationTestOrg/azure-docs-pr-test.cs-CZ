---
title: "aaaCreate a spravovat skupiny akce v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat skupiny akce v hello portálu Azure."
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
ms.openlocfilehash: 97e0b22bea7787fff6856f895a7e6256c177efd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-action-groups-in-hello-azure-portal"></a>Vytvoření a Správa skupin akce v hello portálu Azure
## <a name="overview"></a>Přehled ##
Tento článek ukazuje, jak toocreate a spravovat skupiny akce v hello portálu Azure.

Seznam akcí, můžete nakonfigurovat skupiny akcí. Tyto skupiny pak lze použít při definování aktivity protokolu výstrahy. Tyto skupiny můžete použít znovu pak Každá výstraha aktivity protokolu, které definujete, zajistit, že hello pořízení stejné akce jsou pokaždé, když se aktivuje výstraha hello aktivity protokolu.

Skupinu akce může mít až too10 každý typ akce. Každá akce se skládá z hello následující vlastnosti:

* **Název**: Jedinečný identifikátor v rámci skupiny akce hello.  
* **Typ akce**: Odeslat zprávu SMS, e-mailovou zprávu nebo volat webhook, jehož.  
* **Podrobnosti o**: hello odpovídající telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.

Informace o tom toouse Azure Resource Manager šablony tooconfigure akce skupinách naleznete v tématu [šablony správce prostředků skupiny akce](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-hello-azure-portal"></a>Vytvořit skupinu akce pomocí hello portálu Azure ##
1. V hello [portál](https://portal.azure.com), vyberte **monitorování**. Hello **monitorování** slučuje okno veškeré monitorování nastavení a data v jednom zobrazení.

    ![Hello "Sledování" service](./media/monitoring-action-groups/home-monitor.png)
2. V hello **protokol aktivit** vyberte **skupiny akcí**.

    ![Karta "Akce skupiny" Hello](./media/monitoring-action-groups/action-groups-blade.png)
3. Vyberte **přidat akci skupinu**a vyplňte pole hello.

    ![příkaz "Přidat skupinu akce" Hello](./media/monitoring-action-groups/add-action-group.png)
4. Zadejte název v hello **název skupiny akce** pole a zadejte název v hello **krátký název** pole. krátký název Hello je použít místo názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.

      ![Dialogové okno Hello přidat akci skupiny"](./media/monitoring-action-groups/action-group-define.png)

5. Hello **předplatné** pole autofills s vaším aktuálním předplatným. Toto předplatné je hello, jeden v které skupinu akce hello je uložit.

6. Vyberte hello **skupiny prostředků** v akci, která hello skupinu uložit.

7. Definujte seznam akcí, tím, že poskytuje každá akce:

    a. **Název**: Zadejte jedinečný identifikátor pro tuto akci.

    b. **Typ akce**: Vyberte SMS, e-mailu nebo webhooku.

    c. **Podrobnosti o**: založený na typu akce hello, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.

8. Vyberte **OK** toocreate hello akce skupiny.

## <a name="manage-your-action-groups"></a>Správa skupin akce ##
Jakmile vytvoříte skupinu akcí, se zobrazí na hello **skupiny akcí** části hello **monitorování** okno. Vyberte skupinu hello akce, kterou chcete toomanage na:

* Přidat, upravit nebo odebrat akce.
* Odstraňte skupinu akce hello.

## <a name="next-steps"></a>Další kroky ##
* Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).  
* Získání [pochopení hello aktivity protokolu výstrahy webhooku schématu](monitoring-activity-log-alerts-webhook.md).  
* Další informace o [omezení rychlosti](monitoring-alerts-rate-limiting.md) výstrah. 
* Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak tooreceive výstrahy.  
* Zjistěte, jak příliš[Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).
