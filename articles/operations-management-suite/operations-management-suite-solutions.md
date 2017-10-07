---
title: aaaSolutions v Operations Management Suite (OMS) | Microsoft Docs
description: "Řešení rozšířit hello funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů správy zabalené, aby zákazníci můžete přidat pracovní prostor OMS tootheir.  Tento článek poskytuje podrobné informace o tom, jak vlastní řešení vytvořené zákazníci a partneři."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5b5a538f1bc4b5577bec94db08bd43668bc6584a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-management-solutions-in-operations-management-suite-oms-preview"></a>Práce s řešení pro správu v Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro řešení pro správu v OMS, které jsou aktuálně ve verzi preview.    
> 
> 

Řešení pro správu rozšíření hello funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů správy zabalené, aby zákazníci můžete přidat tootheir prostředí.  Kromě toho příliš[řešení od společnosti Microsoft](../log-analytics/log-analytics-add-solutions.md), partnery a zákazníky, můžete vytvořit toobe řešení správy použít ve svém vlastním prostředí nebo provedené dostupné toocustomers prostřednictvím hello komunity.

## <a name="finding-and-installing-management-solutions"></a>Hledání a instalace řešení pro správu
Existuje více metod pro vyhledání a instalace řešení pro správu, jak je popsáno v následující části hello.

### <a name="azure-marketplace"></a>Azure Marketplace
Řešení pro správu od společnosti Microsoft a důvěryhodným partnerům může být nainstalována z hello Azure Marketplace v hello portálu Azure.

1. Přihlaste se toohello portálu Azure.
2. V levém podokně hello vyberte **další služby**.
3. Buď přejděte dolů příliš**řešení** nebo typ *řešení* do hello **filtru** dialogové okno.
4. Klikněte na tlačítko hello **+ přidat** tlačítko.
5. Vyhledejte řešení, které vás zajímají buď procházením, kliknutím na tlačítko hello **filtru** tlačítko nebo zadáním v hello **vyhledávání Everthing** pole.
6. Klikněte na jeho podrobné informace tooview položky marketplace.
7. Klikněte na tlačítko **vytvořit** tooopen hello **přidat řešení** podokně.
8. Bude výzvami toorequired informace, jako je hello [OMS pracovní prostor a účet Automation](#oms-workspace-and-automation-account) kromě hello toovalues pro všechny parametry v řešení.
9. Klikněte na tlačítko **vytvořit** tooinstall hello řešení.

### <a name="oms-portal"></a>Portálu OMS
Řešení pro správu od společnosti Microsoft může být nainstalována z hello Galerie řešení na portálu OMS hello.

1. Přihlaste se toohello portálu OMS.
2. Klikněte na tlačítko hello **řešení Galerie** dlaždici.
3. Na stránce hello OMS řešení Galerie informace o jednotlivých k dispozici řešení. Klikněte na název hello hello řešení, které chcete tooadd tooOMS.
4. Na stránce hello hello řešení, které jste zvolili zobrazí se podrobné informace o řešení hello. Klikněte na tlačítko **Přidat**.
5. Nová dlaždice pro hello řešení, které jste přidali, se zobrazí na stránce Přehled v OMS hello a můžete začít používat ho po hello OMS služba zpracovává vaše data.

### <a name="azure-quickstart-templates"></a>Rychlý úvod do šablon pro Azure
Členové komunity hello můžete odeslat tooAzure řešení správy šablony rychlý start.  Můžete stahovat tyto šablony pro pozdější instalaci nebo je zkontrolovat toolearn jak příliš[vytvářet vlastní řešení](#creating-a-solution).

1. Postupujte podle hello procesu popsaného v tématu [OMS pracovní prostor a účet Automation](#oms-workspace-and-automation-account) toolink prostoru a účet.
2. Přejděte příliš[šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).  
3. Hledání řešení, které vás zajímají.
4. Vyberte řešení aplikace hello z hello výsledky tooview její podrobnosti.
5. Klikněte na tlačítko hello **nasazení tooAzure** tlačítko.
6. Bude výzvami tooprovide informace, jako je hello skupinu prostředků a umístění v toovalues přidání všech parametrů v řešení hello.
7. Klikněte na tlačítko **nákupu** tooinstall hello řešení.

### <a name="deploy-azure-resource-manager-template"></a>Nasazení šablony Azure Resource Manageru
Řešení, které jste získali od komunity hello nebo [vytvořit sami](#creating-a-solution) jsou implementované jako šablony Resource Manageru, a můžete použít libovolnou hello standardní metody pro [nasazení šablony](../azure-resource-manager/resource-group-template-deploy-portal.md).  Všimněte si, že před instalací hello řešení, musíte vytvořit a propojit hello [OMS pracovní prostor a účet Automation](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>Pracovní prostor OMS a účet Automation.
Většina řešení pro správu vyžadují [pracovním prostorem OMS](../log-analytics/log-analytics-manage-access.md) toocontain zobrazení a [účet Automation](../automation/automation-security-overview.md#automation-account-overview) toocontain sady runbook a související prostředky. Hello prostoru a účet, musí splňovat následující požadavky hello.

* Řešení lze použít pouze jeden pracovní prostor OMS a jeden účet Automation.  
* Hello pracovním prostorem OMS a účet Automation používá řešení musí být propojená tooone jiné. Pracovní prostor služby OMS může být pouze propojené tooone účet Automation a účet Automation může být pouze propojené tooone pracovním prostorem OMS.
* toobe propojené, hello pracovním prostorem OMS a hello automatizace účet musí být ve stejné skupině prostředků a oblast.  Výjimka Hello je pracovní prostor služby OMS v oblasti Východ USA a a účet Automation v oblasti Východ USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Vytváření propojení mezi pracovním prostorem OMS a účet Automation.
Jak určit pracovní prostor OMS hello a účet Automation závisí na hello metody instalace pro vaše řešení.

* Při instalaci řešení společnosti Microsoft prostřednictvím portálu OMS hello je nainstalován v hello aktuální pracovní prostor OMS a není třeba žádný účet Automation.
* Při instalaci řešení prostřednictvím hello Azure Marketplace, budete vyzváni pracovním prostorem OMS a účet Automation a je vytvořená hello propojení mezi nimi.  
* Pro řešení mimo hello Azure Marketplace je nutné před instalací hello řešení propojit pracovní prostor OMS hello a účet Automation.  Můžete provést výběrem řešení v Azure Marketplace hello a výběr pracovním prostorem OMS hello a účet Automation.  Nemáte tooactually instalaci hello řešení, protože odkaz hello bude vytvořen, jakmile jsou vybrané pracovní prostor OMS hello a účet Automation.  Po vytvoření odkazu hello je pak můžete tento pracovní prostor OMS a účet Automation pro řešení. 

### <a name="verifying-hello-link-between-an-oms-workspace-and-automation-account"></a>Ověření hello propojení mezi pracovním prostorem OMS a účet Automation.
Můžete ověřit hello propojení mezi pracovním prostorem OMS a účtu Automation pomocí hello následující postup.

1. Vyberte účet Automation hello v hello portálu Azure.
2. Posuv toohello dolní části hello **nastavení** podokně.
3. Pokud je oddíl s názvem **OMS prostředky** v hello **nastavení** podokně a pak tento účet je pracovní prostor OMS připojené tooan.
4. Vyberte **prostoru** tooview hello podrobnosti o pracovním prostorem OMS hello propojené toothis účet Automation.

## <a name="listing-management-solutions"></a>Výpis řešení pro správu
Hello použijte následující postup tootooview hello řešení pro správu v hello pracovních prostorů propojené tooyour předplatného Azure.

1. Přihlaste se toohello portálu Azure.
2. V levém podokně hello vyberte **další služby**.
3. Buď přejděte dolů příliš**řešení** nebo typ *řešení* do hello **filtru** dialogové okno.
4. Objeví se řešení, které jsou nainstalovány ve všech vašich pracovních prostorů.

Všimněte si, že se zobrazí pouze řešení Microsoft hello nainstalované v aktuálním prostoru hello pomocí portálu OMS hello.

## <a name="removing-a-management-solution"></a>Odebrání řešení pro správu
Pokud je odebrán řešení pro správu, budou odebrány také všechny prostředky v řešení hello.  

1. Vyhledejte řešení hello v hello Azure portal pomocí postupu hello v [výpis řešení](#listing-solutions).
2. Vyberte řešení hello chcete tooremove.
3. Klikněte na tlačítko hello **odstranit** tlačítko.

## <a name="creating-a-management-solution"></a>Vytváření řešení pro správu
Úplné pokyny k vytváření řešení pro správu jsou k dispozici na [vytváření řešení v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Další kroky
* Hledání [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates) ukázky různých šablonách Resource Manager.
* Vytvořit vlastní [řešení pro správu](operations-management-suite-solutions-creating.md).

