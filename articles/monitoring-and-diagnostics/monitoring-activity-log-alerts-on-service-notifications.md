---
title: "aaaReceive aktivity protokolu výstrahy na oznámení o službách | Microsoft Docs"
description: "SMS, e-mailem nebo webhooku dostat upozornění, když dojde k služby Azure."
author: johnkemnetz
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
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: dd35e8f39d2a522efdae4dfed20779c992c1dd27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Vytvoření aktivity protokolu upozornění na oznámení o službách
## <a name="overview"></a>Přehled
Tento článek ukazuje, jak výstrahy tooset si protokol aktivit pro oznámení o stavu služby pomocí hello portálu Azure.  

Můžete zobrazit upozornění, když Azure odesílá služba stavu oznámení tooyour předplatného Azure. Můžete nakonfigurovat hello výstrahu založenou na:

- Třída Hello oznámení o stavu služby (incident, údržby, informace, atd.).
- Hello služeb vliv.
- Hello oblast(i) vliv.
- Stav Hello hello oznámení (aktivní oproti přeložit).
- úroveň Hello hello oznámení (informační, upozornění, chyby).

Můžete také konfigurovat kdo hello výstraha by měly být odeslány na:

- Vyberte existující skupinu akce.
- Vytvořte novou skupinu akce (který můžete použít pro budoucí výstrahy).

toolearn Další informace o akci skupiny, najdete v části [vytvořit a spravovat skupiny akce](monitoring-action-groups.md).

Informace o tom, jak tooconfigure služby stavu oznámení výstrahy pomocí šablon Azure Resource Manageru, najdete v části [šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-hello-azure-portal"></a>Vytvořit výstrahu na oznámení stavu služby pro novou skupinu akce pomocí hello portálu Azure
1. V hello [portál](https://portal.azure.com), vyberte **monitorování**.

    ![Hello "Sledování" service](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. V hello **protokol aktivit** vyberte **výstrahy**.

    ![Karta "Oznámení" Hello](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Vyberte **přidat aktivitu protokolu upozornění**a vyplňte pole hello.

    ![Hello příkaz "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Zadejte název v hello **výstrahy název aktivity protokolu** pole a zadejte **popis**.

    ![Hello "Přidat aktivitu protokolu výstraha" dialogové okno](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. Hello **předplatné** pole autofills s vaším aktuálním předplatným. Toto předplatné je výstraha použité toosave hello aktivity protokolu. výstrahy prostředek Hello je nasazené toothis předplatného a monitorování události v protokolu hello aktivity pro ni.

6. Vyberte hello **skupiny prostředků** v které hello výstrahy prostředků vytvořili. Tato akce není hello skupinu prostředků, který je monitorován hello výstraha. Místo toho je skupina prostředků hello, kde je umístěný prostředek výstrahy hello.

7. V hello **kategorie události** vyberte **stav služby**. Volitelně vyberte hello **služby**, **oblast**, **typ**, **stav**, a **úroveň** služby oznámení o stavu, které chcete tooreceive.

8. V části **výstrahy prostřednictvím**, vyberte hello **nový** tlačítko akce skupiny. Zadejte název v hello **název skupiny akce** pole a zadejte název v hello **krátký název** pole. krátký název Hello se odkazuje v hello oznámení, která se posílají, když se aktivuje se tato výstraha.

9. Definujte seznam příjemců tím, že poskytuje příjemce hello:

    a. **Název**: Zadejte název, alias nebo identifikátor hello příjemce.

    b. **Typ akce**: Vyberte SMS, e-mailu nebo webhooku.

    c. **Podrobnosti o**: založený na typu akce hello vybrali, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.

10. Vyberte **OK** toocreate hello výstrahy.

Během několika minut hello výstraha je aktivní a začne tootrigger na základě hello podmínek, které zadáte během vytváření.

Informace o schématu hello webhooku pro aktivitu protokolu výstrahy naleznete v tématu [Webhooky Azure aktivity protokolu výstrahy](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Skupina akce Hello definované v těchto kroků je opakovaně použitelné jako stávající skupina akce pro všechny budoucí výstrahy definice.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-hello-azure-portal"></a>Vytvořit výstrahu na oznámení stavu služby pro stávající skupinu akce pomocí hello portálu Azure

1. Postupujte podle kroků 1 až 7 v předchozí části toocreate hello oznámení služby stavu. 

2. V části **výstrahy prostřednictvím**, vyberte hello **existující** tlačítko akce skupiny. Vyberte skupinu hello příslušnou akci.

3. Vyberte **OK** toocreate hello výstrahy.

Během několika minut hello výstraha je aktivní a začne tootrigger na základě hello podmínek, které zadáte během vytváření.

## <a name="manage-your-alerts"></a>Spravovat oznámení

Po vytvoření výstrahy, se zobrazí na hello **výstrahy** části hello **monitorování** okno. Vyberte hello oznámení, které chcete toomanage na:

* Upravte.
* Odstraňte ji.
* Zakázat nebo povolit, pokud chcete zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy hello.

## <a name="next-steps"></a>Další kroky
- Další informace o [oznámení o stavu služby](monitoring-service-notifications.md).
- Další informace o [omezení rychlosti oznámení](monitoring-alerts-rate-limiting.md).
- Zkontrolujte hello [schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).
- Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak tooreceive výstrahy. 
- Další informace o [skupiny akcí](monitoring-action-groups.md).
