---
title: "změny virtuálního počítače aaaMonitor - & mřížky událostí Azure Logic Apps | Microsoft Docs"
description: "Kontrolovat změny konfigurace ve virtuálních počítačích (VM) s použitím mřížky událostí Azure a Logic Apps"
keywords: "aplikace logiky, událostí mřížky, virtuální počítač VM"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 08/16/2017
ms.author: LADocs; estfan
ms.openlocfilehash: f0633e598be6e7880a310e6f8e64f6738cc692b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Sledování změn virtuálního počítače se mřížka událostí Azure a Logic Apps

Můžete spustit automatickou [aplikace logiky pracovního postupu](../logic-apps/logic-apps-what-are-logic-apps.md) při určité události dojde v Azure prostředků nebo jiných prostředků. Tyto prostředky můžete publikovat tyto události tooan [Azure událostí mřížky](../event-grid/overview.md). Pak hello událostí mřížky nabízených oznámení toosubscribers tyto události, které mají fronty, webhooků, nebo [služby event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) jako koncové body. Jako odběratel můžete před spuštěním úlohy tooperform – automatizované pracovní postupy bez psaní jakéhokoli kódu aplikace logiky počkejte tyto události z události mřížky hello.

Tady jsou například některé události, že vydavateli může odesílat toosubscribers prostřednictvím služby Azure událostí mřížky hello:

* Vytvořit, číst, aktualizovat nebo odstranit prostředek. Například můžete sledovat změny, které může platit poplatky na vaše předplatné Azure a vliv na vašem vyúčtování. 
* Přidat nebo odebrat osoby z předplatného Azure.
* Aplikace provede určité akce.
* Zobrazí se nové zprávy ve frontě.

V tomto kurzu vytvoří aplikace logiky, která sleduje změny tooa virtuálního počítače a odešle e-mailů o těchto změnách. Když vytvoříte aplikaci logiky s předplatným události pro prostředek služby Azure, toku událostí z prostředku prostřednictvím událostí mřížky toohello logiku aplikace. Hello kurz vás provede procesem vytváření této logiky aplikace:

![Přehled – monitorování virtuální počítač s událostí mřížky a logiku aplikace](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření aplikace logiky, která sleduje události z mřížky události.
> * Přidejte podmínku, která konkrétně kontroluje změny virtuálního počítače.
> * Odeslání e-mailu, když se změní virtuálního počítače.

## <a name="prerequisites"></a>Požadavky

* E-mailový účet z [všechny e-mailu zprostředkovatele nepodporuje Azure Logic Apps](../connectors/apis-list.md), jako jsou například aplikace Office 365 Outlook, Outlook.com nebo z Gmailu pro odesílání oznámení. Tento kurz používá Office 365 Outlook.

* A [virtuální počítač](https://azure.microsoft.com/services/virtual-machines). Pokud jste tak již neučinili, vytvořte virtuální počítač prostřednictvím [vytvořit virtuální počítač kurzu](https://docs.microsoft.com/azure/virtual-machines/). virtuální počítač hello toomake publikování událostí, můžete [nepotřebujete toodo cokoli jiného](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Vytvoření aplikace logiky, která sleduje události z mřížky události

Nejprve vytvořte aplikaci logiky a přidejte mřížce aktivační událost, která monitoruje hello skupinu prostředků pro virtuální počítač. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). 

2. Z hello levého horního rohu hlavní nabídky Azure hello, zvolte **nový** > **Enterprise integrace** > **aplikace logiky**.

   ![Vytvoření aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. Vytvoření aplikace logiky s hello nastavení zadané v hello následující tabulka:

   ![Zadejte podrobnosti o aplikaci logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Název** | *{logiku--název_aplikace}* | Zadejte název jedinečné logiku aplikace. | 
   | **Předplatné** | *{vaše Azure předplatné}* | Vyberte hello stejného předplatného Azure pro všechny služby v tomto kurzu. | 
   | **Skupina prostředků** | *{vaše Azure-resource-group}* | Vyberte hello stejnou skupinu prostředků Azure pro všechny služby v tomto kurzu. | 
   | **Umístění** | *{vaše Azure oblast}* | Vyberte hello stejné oblasti pro všechny služby v tomto kurzu. | 
   | | | 

4. Až budete připraveni, vyberte **Pin toodashboard**a zvolte **vytvořit**.

   Nyní jste vytvořili prostředek služby Azure pro svou aplikaci logiky. 
   Po Azure nasadí aplikaci logiky, hello logiku aplikace Návrhář ukazuje šablon pro běžné vzory, abyste mohli začít rychleji.

   > [!NOTE] 
   > Když vyberete **Pin toodashboard**, aplikace logiky automaticky otevře v návrháři aplikace logiky. Jinak můžete ručně najít a otevřít aplikaci logiky.

5. Teď zvolte šablonu logiku aplikace. V části **šablony**, zvolte **prázdné aplikace logiky** , můžete vytvořit svou aplikaci logiky od začátku.

   ![Výběr šablony aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   nyní ukazuje Hello logiku aplikace Návrhář [ *konektory* ](../connectors/apis-list.md) a [ *aktivační události* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) používané toostart vaší aplikace logiky a také akce můžete přidat po tooperform úlohy aktivační události. Aktivační událost je událost, která vytvoří instanci aplikace logiky a spustí pracovní postup aplikace logiky. 
   Aplikace logiky musí aktivační událost jako první položka hello.

6. Hello vyhledávacího pole zadejte "události mřížky" jako filtr. Vyberte této aktivační události: **mřížky událostí Azure - na události prostředků**

   ![Vyberte této aktivační události: "Azure událostí mřížky - na události prostředků"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Pokud budete vyzváni, přihlaste se tooAzure mřížky událostí pomocí svých přihlašovacích údajů Azure.

   ![Přihlaste se pomocí přihlašovacích údajů Azure](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Pokud jste přihlášení pomocí osobního účtu Microsoft, jako například @outlook.com nebo @hotmail.com, aktivační události mřížky hello se nemusí zobrazit správně. Jako alternativní řešení, zvolte [připojit pomocí objektu služby](/azure-resource-manager/resource-group-create-service-principal-portal.md), nebo jako člen skupiny hello Azure Active Directory, který je spojen s předplatným Azure, například ověřit *uživatelské jméno* @emailoutlook.onmicrosoft.com.

8. Teď přihlášení k odběru události toopublisher aplikace logiky. Zadejte podrobnosti hello pro vaše předplatné událostí uvedených v následující tabulce hello:

   ![Zadejte podrobnosti pro předplatné událostí](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Předplatné** | *{virtuální počítače Azure předplatnými}* | Vyberte předplatné Azure vydavatel události hello. V tomto kurzu vyberte hello předplatné Azure pro virtuální počítač. | 
   | **Typ prostředku** | Microsoft.Resources.resourceGroups | Vyberte typ prostředku vydavatel události hello. V tomto kurzu vyberte hello zadanou hodnotou, aplikace logiky monitoruje jenom skupiny prostředků. | 
   | **Název prostředku** | *{virtuální machine--název skupiny prostředků}* | Vyberte název prostředku hello vydavatele. V tomto kurzu vyberte hello název skupiny prostředků hello pro virtuální počítač. | 
   | Volitelné nastavení, vyberte **zobrazit rozšířené možnosti**. | *{v tématu Popis}* | * **Předpony filtru**: v tomto kurzu ponechte prázdné toto nastavení. výchozí chování Hello odpovídá všechny hodnoty. Řetězec předpony můžete však zadat jako filtr, například cestu a parametr pro konkrétní zdroje. <p>* **Přípona filtru**: v tomto kurzu ponechte prázdné toto nastavení. výchozí chování Hello odpovídá všechny hodnoty. Řetězec přípony můžete však zadat jako filtr, například příponu názvu souboru, pokud chcete pouze určité typy souborů.<p>* **Název odběru**: Zadejte jedinečný název pro vaše předplatné událostí. |
   | | | 

   Když jste hotovi, vaše mřížky aktivační událost může vypadat jako tento ukázkový:
   
   ![Podrobnosti o aktivační události příklad události mřížky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Uložte svou aplikaci logiky. Na hello návrháře nástrojů, zvolte **Uložit**. toocollapse a skrýt podrobnosti akce v aplikaci logiky zvolte akce hello záhlaví.

   ![Uložení aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Při ukládání aplikace logiky s aktivační signál události mřížky, Azure automaticky vytvoří předplatné události pro prostředek tooyour vybrané aplikace logiky. Takže když hello prostředků publikuje události toohello událostí mřížky, této události mřížky automaticky nabízených oznámení aplikace logiky tooyour hello událostí. Tato událost se aktivuje aplikaci logiky potom vytvoří a spustí instanci hello pracovního postupu, který definujete v tyto další kroky.

Aplikace logiky je nyní za provozu a naslouchá tooevents z hello událostí mřížky, ale nic se neděje dokud nepřidáte toohello akce pracovního postupu. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Přidat podmínku, která kontroluje změny virtuálního počítače

toorun pracovní postup aplikace logiky jenom v případě, že se stane konkrétní události, přidejte podmínku, která kontroluje pro virtuální počítač "" operací zápisu. Při splnění této podmínky svou aplikaci logiky odešle že e-mailu s podrobnostmi o hello aktualizovat virtuální počítač.

1. V návrháři aplikace logiky, vyberte v části aktivační událost mřížky hello, **nový krok** > **přidat podmínku**.

   ![Přidat aplikaci logiky tooyour podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Hello návrhář aplikace na základě logiky přidá prázdný podmínku tooyour pracovním postupu, včetně toofollow cesty akce na základě, zda text hello podmínka vyhodnocena jako true nebo false.

   ![Prázdný podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. V hello **podmínku** vyberte **upravit v rozšířeném režimu**.
Zadejte tento výraz:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   Vaše podmínky nyní vypadá v tomto příkladu:

   ![Prázdný podmínky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Tento výraz kontroluje hello událostí `body` pro `data` objektu, kde hello `operationName` vlastnost je hello `Microsoft.Compute/virtualMachines/write` operaci. 
   Další informace o [schématu události událostí mřížky](../event-grid/event-schema.md).

3. tooprovide popis hello podmínky, zvolte hello **výpustky** (**...** ) tlačítko obrazec hello podmínky, a potom vyberte **přejmenovat**.

   > [!NOTE] 
   > Hello později příklady v tomto kurzu také popisují kroky v pracovním postupu aplikace logiky hello.

4. Teď zvolte **upravit v režimu základní** tak, aby hello výraz automaticky vyřeší, jak je znázorněno:

   ![Stav aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Uložte svou aplikaci logiky.

## <a name="send-email-when-your-virtual-machine-changes"></a>Odeslání e-mailu, když se změní virtuálního počítače

Nyní přidejte [ *akce* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) , abyste měli e-mailu, když hello zadaná podmínka vyhodnocena jako true.

1. V podmínce hello **v případě hodnoty true** vyberte **přidat akci**.

   ![Akce pro když je splněna podmínka pro přidání](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Hello vyhledávacího pole zadejte "e-mailu" jako filtr. Založená na zprostředkovateli e-mailu, najděte a vyberte odpovídající konektor hello. Pak vyberte hello odeslat e-mailu panel. pro váš konektor. Například: 

   * Azure pracovní nebo školní účet, vyberte hello konektor Office 365 Outlook. 
   * Pro osobní účty Microsoft vyberte konektor hello Outlook.com. 
   * Pro účty z Gmailu vyberte konektor z Gmailu hello. 

   Vytvoříme toocontinue s konektorem hello Office 365 Outlook. 
   Pokud chcete použít k jinému zprostředkovateli, hello hello kroky zůstávají stejné, ale uživatelské rozhraní se může objevit jiný. 

   ![Vyberte možnost odeslat e-mailu panel.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Pokud ještě nemáte připojení pro poskytovatele e-mailu, přihlaste se tooyour e-mailový účet po zobrazení dotazu pro ověřování.

4. Zadejte podrobnosti pro e-mailu hello uvedených v následující tabulce hello:

   ![Akce prázdný e-mailu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > tooselect z pole, které jsou k dispozici v pracovním postupu, klikněte do textového pole, že hello **dynamický obsah** seznamu otevře, nebo zvolte **přidávat dynamický obsah**. Pro další pole, zvolte **Další informace naleznete v** pro každý oddíl v seznamu hello. tooclose hello **dynamický obsah** vyberte **přidávat dynamický obsah**.

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **K** | *{příjemce e-mailovou adresu}* |Zadejte hello příjemce e-mailovou adresu. Pro účely testování můžete použít vlastní e-mailovou adresu. | 
   | **Předmět** | Prostředek aktualizovat: **subjektu**| Zadejte obsah hello předmět e-mail hello. V tomto kurzu zadejte hello navrhované text a vyberte hello událostí **subjektu** pole. Zde vaše předmět e-mailu obsahuje název hello hello aktualizovat prostředku (virtuálním počítači). | 
   | **Text** | Skupina prostředků: **tématu** <p>Typ události: **typ události**<p>ID události: **ID**<p>Čas: **čas události** | Zadejte hello obsah textu hello e-mail. V tomto kurzu zadejte hello navrhované text a vyberte hello událostí **tématu**, **typ události**, **ID**, a **čas události** pole tak, aby e-mailu zahrnuje název skupiny prostředků hello, typu události, časové razítko události a ID události pro aktualizaci hello. <p>prázdné řádky tooadd v obsah, stiskněte Shift + Enter. | 
   | | | 

   > [!NOTE] 
   > Pokud vyberete pole, které představuje pole, Návrhář hello automaticky přidá **pro každou** smyčky kolem hello akce, který odkazuje na pole hello. Tímto způsobem svou aplikaci logiky tuto akci provede na jednotlivé položky pole.

   Teď může vaše e-mailu akce vypadat jako tento ukázkový:

   ![Vyberte výstupy tooinclude v e-mailu](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   A aplikace logiky dokončení může vypadat jako tento příklad:

   ![Aplikace logiky dokončení](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Uložte svou aplikaci logiky. toocollapse a skrýt podrobnosti každá akce v aplikaci logiky zvolte akce hello záhlaví.

   ![Uložení aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   Aplikace logiky je nyní za provozu, ale čeká změny tooyour virtuální počítač před jakékoli akce. 
   tootest svou aplikaci logiky teď pokračovat toohello další části.

## <a name="test-your-logic-app-workflow"></a>Testování pracovního postupu aplikace logiky

1. toocheck, že je aplikace logiky získávání hello určené události, aktualizovat virtuální počítač. 

   Například můžete změnit velikost virtuálního počítače v prostředí hello portál Azure nebo [změnit velikost virtuálního počítače v prostředí Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Po chvíli měli byste obdržet e-mailu. Například:

   ![E-mailu o aktualizace virtuálního počítače](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. Spustí tooreview hello a historii aktivační událost pro svou aplikaci logiky, v nabídce aplikace logiky, vyberte **přehled**. tooview další podrobnosti o spuštění, zvolte hello řádek pro tento spustit.

   ![Spuštění aplikace logiky historie](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. tooview hello vstupy a výstupy pro každý krok, rozbalte krok text hello, které chcete tooreview. Tyto informace můžete diagnostikovat a ladit problémy v aplikaci logiky.
 
   ![Podrobnosti o historii spuštění aplikace logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Blahopřejeme, jste vytvořili a spusťte aplikaci logiky, která sleduje události prostředků prostřednictvím mřížce událostí a odešle e-mail, když tyto události dojít. Také jste zjistili, jak snadno můžete vytvořit pracovní postupy, které automatizovat procesy a integrovat systémy a cloudových služeb.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Tento kurz používá prostředky a provádí akce, které platit poplatky na vaše předplatné Azure. Když jste hotovi s hello kurzu a testování, ujistěte se, že tak zakázat nebo odstranit všechny prostředky, kde nechcete, aby tooincur poplatky.

Zastavit můžete svou aplikaci logiky spuštěná a odesílání e-mailu bez odstranění aplikace hello. V nabídce aplikace logiky, vyberte **přehled**. Na panelu nástrojů hello, zvolte **zakázat**.

![Vypnout aplikaci logiky](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>Nejčastější dotazy

**Q**: co jiným virtuálním počítačem monitorování úloh lze provést pomocí událostí mřížky a aplikacích logiky? </br>
**A**: můžete sledovat další změny v konfiguraci, například:

* Virtuální počítač získá přístup na základě rolí práva řízení (RBAC).
* Skupina zabezpečení sítě (NSG) tooa v síťovém rozhraní (NIC) jsou provedeny změny.
* Disky pro virtuální počítač po přidání nebo odebrání.
* Veřejná IP adresa je přiřazen tooa virtuálního počítače síťový adaptér.

## <a name="next-steps"></a>Další kroky

* [Přehled služby Event mřížky](../event-grid/overview.md)
* [Koncepty mřížky událostí](../event-grid/concepts.md)
* [Rychlý úvod: Vytvoření a trasy vlastních událostí s událostí mřížky](../event-grid/custom-event-quickstart.md)
* [Událost schématu události mřížky](../event-grid/event-schema.md)
* [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)
* [Vytvořit pracovní postupy aplikace logiky pomocí předdefinovaných šablon](../logic-apps/logic-apps-use-logic-app-templates.md)