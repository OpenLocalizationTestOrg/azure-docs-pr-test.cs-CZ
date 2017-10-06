---
title: "aaaCreate aktivity protokolu výstrahy | Microsoft Docs"
description: "Informování prostřednictvím serveru SMS, webhooku a e-mailu při určité události v protokolu aktivit hello."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: ba0716cc12a0b3a0024ee5562a025f3f153f8982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts"></a>Vytvořit aktivitu protokolu výstrahy

## <a name="overview"></a>Přehled
Výstrahy protokolu aktivit jsou výstrahy, které aktivovat při výskytu nové aktivity protokolu události, který odpovídá hello podmínky zadané v hello výstrahy. Jsou prostředky Azure, takže je lze vytvořit pomocí šablony Azure Resource Manager. Také mohou být vytvořen, aktualizovat nebo odstranit v hello portálu Azure. Tento článek představuje koncepty hello za aktivitu protokolu výstrahy. Poté vám ukáže, jak toouse hello Azure portálu tooset až výstrahu na aktivity protokolu události.

Obvykle vytvoříte aktivity protokolu výstrahy, oznámení tooreceive při:

* Určité změny, ke kterým došlo u prostředků v Azure předplatného, často vymezená tooparticular skupiny prostředků nebo prostředky. Například můžete toobe upozorněni, když se odstraní všechny virtuální počítače v myProductionResourceGroup. Nebo můžete chtít toobe oznámení, pokud žádné nové role přiřazené tooa uživatele v rámci vašeho předplatného.
* Nastane událost stavu služby. Události stavu služby zahrnují upozornění incidenty a události údržby, které se vztahují tooresources v rámci vašeho předplatného.

V obou případech výstrahu protokolu aktivit monitoruje pouze pro události v hello předplatné, ve které hello se vytvoří výstraha.

Můžete nakonfigurovat výstrahu protokolu aktivity podle libovolné vlastnosti nejvyšší úrovně v hello objekt JSON pro aktivitu protokolu události. Portál hello však uvádí nejběžnější možnosti hello:

- **Kategorie**: pro správu, služba stavu, škálování a doporučení. Další informace najdete v tématu [přehled protokol činnosti Azure hello](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). toolearn Další informace o události stavu služby, najdete v části [výstrahy v protokolu aktivit na oznámení o službách](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Skupina prostředků**
- **Prostředek**
- **Typ prostředku**
- **Název operace**: název operace hello řízení přístupu na základě Role Správce prostředků.
- **Úroveň**: hello úroveň závažnosti události hello (Verbose, informační, upozornění, chyby nebo kritický).
- **Stav**: stav hello hello události, obvykle spuštění, se nezdařilo nebo bylo úspěšné.
- **Událost iniciovaná**: hello také známé jako "volající." Hello e-mailovou adresu nebo identifikátor Azure Active Directory hello uživatele, který provedl operaci hello.

>[!NOTE]
>Je nutné zadat alespoň dvě z hello před kritéria v výstrahy, s právě jednu kategorii hello. Nelze vytvořit výstrahu, která aktivuje pokaždé, když dojde k vytvoření události v protokolech aktivity hello.
>
>

Když se aktivuje výstrahu protokolu aktivit, používá akci skupiny toogenerate akce nebo oznámení. Skupinu akcí je opakovaně použitelné sadu příjemců oznámení, jako je například e-mailové adresy, adresy URL webhooku nebo SMS telefonních čísel. příjemci Hello můžete na něj odkazovat z více toocentralize výstrahy a skupiny vaší kanály oznámení. Když definujete výstrahu protokolu aktivit, máte dvě možnosti. Můžete:

* Použijte existující skupinu akce v protokolu upozornění aktivity. 
* Vytvořte novou skupinu akce. 

toolearn Další informace o akci skupiny, najdete v části [vytvořit a spravovat skupiny akce v hello portál Azure](monitoring-action-groups.md).

toolearn Další informace o oznámení o stavu služby, najdete v části [výstrahy v protokolu aktivit na oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-hello-azure-portal"></a>Vytvořit výstrahu na aktivity protokolu události s novou skupinu akce pomocí hello portálu Azure
1. V hello [portál](https://portal.azure.com), vyberte **monitorování**.

    ![Hello "Sledování" service](./media/monitoring-activity-log-alerts/home-monitor.png)
2. V hello **protokol aktivit** vyberte **výstrahy**.

    ![Karta "Oznámení" Hello](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Vyberte **přidat aktivitu protokolu upozornění**a vyplňte pole hello.

4. Zadejte název v hello **výstrahy název aktivity protokolu** a vyberte **popis**.

    ![Hello příkaz "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. Hello **předplatné** pole autofills s vaším aktuálním předplatným. Toto předplatné je hello, jeden v které skupinu akce hello je uložit. výstrahy prostředek Hello je nasazené toothis předplatného a monitorování aktivity protokolu události z něj.

    ![Hello "Přidat aktivitu protokolu výstraha" dialogové okno](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Vyberte hello **skupiny prostředků** v které hello výstrahy prostředků vytvořili. Toto není hello skupinu prostředků, který je monitorován hello výstraha. Místo toho je skupina prostředků hello, kde je umístěný prostředek výstrahy hello.

7. Volitelně vyberte **kategorie události** toomodify hello další filtry, které jsou zobrazeny. Pro správu události hello filtry zahrnují **skupiny prostředků**, **prostředků**, **typ prostředku**, **název operace**, **Úroveň**, **stav**, a **iniciovaná událostí**. Tyto hodnoty identifikovat události, které by měly monitorovat této výstrahy.

    >[!NOTE]
    >Musíte zadat alespoň jeden Dobrý den předcházející kritéria v upozornění. Nelze vytvořit výstrahu, která aktivuje pokaždé, když dojde k vytvoření události v protokolech aktivity hello.
    >
    >

8. Zadejte název v hello **název skupiny akce** pole a zadejte název v hello **krátký název** pole. krátký název Hello je použít místo názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.

9.  Definujte seznam akcí, tím, že poskytuje hello akce:

    a. **Název**: Zadejte název, alias nebo identifikátor hello akce.

    b. **Typ akce**: Vyberte SMS, e-mailu nebo webhooku.

    c. **Podrobnosti o**: založený na typu akce hello, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.

10. Vyberte **OK** toocreate hello výstrahy.

Výstraha Hello trvá několik minut toofully rozšířit a pak se zobrazí jako aktivní. Aktivuje při nové události kritériím hello výstrahy.

Další informace najdete v tématu [Rady pro pochopení hello webhooku schématu použít ve výstrahách aktivity protokolu](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Skupina akce Hello definované v těchto kroků je opakovaně použitelné jako stávající skupina akce pro všechny budoucí výstrahy definice.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-hello-azure-portal"></a>Vytvořit výstrahu na aktivity protokolu události pro stávající skupinu akce pomocí hello portálu Azure
1. Postupujte podle kroků 1 až 7 v předchozí části toocreate hello výstrahu protokolu aktivit.

2. V části **oznámit prostřednictvím**, vyberte hello **existující** tlačítko akce skupiny. Vyberte existující skupinu akce hello seznamu.

3. Vyberte **OK** toocreate hello výstrahy.

Výstraha Hello trvá několik minut toofully rozšířit a pak se zobrazí jako aktivní. Aktivuje při nové události kritériím hello výstrahy.

## <a name="manage-your-alerts"></a>Spravovat oznámení

Po vytvoření výstrahy, je viditelný v části výstrahy hello hello monitorování okna. Vyberte hello oznámení, které chcete toomanage na:

* Upravte.
* Odstraňte ji.
* Zakázat nebo povolit, pokud chcete zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy hello.

## <a name="next-steps"></a>Další kroky
- Získat [přehled výstrah](monitoring-overview-alerts.md).
- Další informace o [omezení rychlosti oznámení](monitoring-alerts-rate-limiting.md).
- Zkontrolujte hello [schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).
- Další informace o [skupiny akcí](monitoring-action-groups.md).  
- Další informace o [oznámení o stavu služby](monitoring-service-notifications.md).
- Vytvoření [aktivity protokolu výstrahy toomonitor všechny operace škálování modul vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Vytvoření [aktivity protokolu výstrahy toomonitor všechny operace škálování nebo škálovatelnou neúspěšné automatické škálování na vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
