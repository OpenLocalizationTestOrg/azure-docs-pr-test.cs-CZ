---
title: "aaaUse Azure portálu toomanage prostředky Azure | Microsoft Docs"
description: "Pomocí portálu Azure a Azure Resource Manageru toomanage vašich prostředků. Ukazuje, jak toowork s prostředky toomonitor řídicí panely."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 0c89a197a31c5b6dd03ba457cb4d1fdf9f6d00f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-resources-through-portal"></a>Spravovat prostředky prostřednictvím portálu Azure
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure Portal](resource-group-portal.md) 
> * [REST API](resource-manager-rest-api.md)
> 
> 

Toto téma ukazuje, jak toouse hello [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) toomanage vašich prostředků Azure. v tématu toolearn o nasazení prostředků prostřednictvím portálu hello [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).

Ne všechny služby v současné době podporuje hello portál nebo Resource Manager. Pro tyto služby, je třeba toouse hello [portálu classic](https://manage.windowsazure.com). Hello stav každé služby, najdete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="manage-resource-groups"></a>Správa skupin prostředků

Skupina prostředků je kontejner, který obsahuje související prostředky pro řešení s Azure. Skupina prostředků Hello může zahrnovat všechny hello prostředky pro řešení hello nebo jenom prostředky, které chcete toomanage jako skupina. Můžete určit, jak mají prostředky tooallocate tooresource skupin podle díky hello nejvhodnější pro vaši organizaci. Obecně platí, přidejte prostředky, které sdílejí hello stejný životní cyklus toohello skupina stejné prostředků, můžete snadno nasadit, aktualizovat a odstranit jejich jako skupina. 

Skupina prostředků Hello ukládají metadata o hello prostředky. Proto když zadáte umístění pro skupinu prostředků hello, určíte se uloží aby metadata. Pro dodržování předpisů, pravděpodobně bude třeba tooensure data uložená v určité oblasti.

1. Vyberte všechny skupiny zdrojů hello ve vašem předplatném toosee **skupiny prostředků**.
   
    ![Procházet skupiny prostředků](./media/resource-group-portal/browse-groups.png)
2. Vyberte skupinu prostředků prázdný toocreate **přidat**.
   
    ![Přidat skupinu prostředků](./media/resource-group-portal/add-resource-group.png)
3. Zadejte název a umístění pro novou skupinu prostředků hello. Vyberte **Vytvořit**.
   
    ![Vytvořte skupinu prostředků](./media/resource-group-portal/create-empty-group.png)
4. Může být nutné tooselect **aktualizovat** toosee hello nedávno vytvořen prostředek skupiny.
   
    ![Aktualizace skupiny prostředků](./media/resource-group-portal/refresh-resource-groups.png)
5. Vyberte toocustomize hello informace zobrazené pro vaší skupiny prostředků **sloupce**.
   
    ![přizpůsobení sloupců](./media/resource-group-portal/select-columns.png)
6. Vyberte sloupce tooadd hello a pak vyberte **aktualizace**.
   
    ![Přidání sloupců](./media/resource-group-portal/add-columns.png)
7. toolearn o nasazení prostředků tooyour novou skupinu prostředků, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).
8. Pro skupinu prostředků tooa rychlý přístup budete moct připnout hello okno tooyour řídicího panelu.
   
    ![Skupina prostředků PIN kódu](./media/resource-group-portal/pin-group.png)
9. řídicí panel Hello zobrazí hello skupinu prostředků a její prostředky. Můžete vybrat skupiny prostředků hello nebo všechny jeho prostředky toonavigate toohello položky.
   
    ![Skupina prostředků PIN kódu](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Značka prostředky
Můžete použít skupiny tooresource značky a prostředky toologically uspořádání vaše prostředky. Informace o práci s značky najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Sledování prostředků
Když vyberete prostředek, uvede okna prostředků hello výchozí grafů a tabulek pro monitorování tohoto typu prostředku.

1. Vyberte prostředek a Všimněte si, hello **monitorování** části. Obsahuje grafy, které jsou relevantní toohello typ prostředku. Hello následující obrázek znázorňuje hello výchozí údaje pro účet úložiště.
   
    ![zobrazení monitorování](./media/resource-group-portal/show-monitoring.png)
2. Oddíl hello okno tooyour řídicí panel můžete připnout výběrem hello tečkami (...) nad sekcí hello. Můžete také přizpůsobit hello velikost hello oddíl v okně hello nebo ji úplně odebrat. Hello následující obrázek ukazuje, jak přizpůsobit toopin, nebo odeberte hello procesoru a paměti části.
   
    ![části kódu PIN](./media/resource-group-portal/pin-cpu-section.png)
3. Po Připnutí hello části toohello řídicí panel, uvidíte na řídicím panelu hello hello souhrnu. A okamžitě ji vyberete trvá toomore údaje o datech hello.
   
    ![Zobrazení řídicí panel](./media/resource-group-portal/view-startboard.png)
4. toocompletely přizpůsobit hello data monitorování prostřednictvím hello portálu, přejděte tooyour výchozí řídicí panely a vyberte **novým řídicím panelem**.
   
    ![řídicí panel](./media/resource-group-portal/dashboard.png)
5. Pojmenujte váš nový řídicí panel a přetáhněte dlaždice na řídicím panelu hello. dlaždice Hello jsou filtrovány podle různé možnosti.
   
    ![řídicí panel](./media/resource-group-portal/create-dashboard.png)
   
     toolearn o práci s řídicí panely, najdete v části [vytvoření a sdílení řídicích panelů v hello portál Azure](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Správa prostředků
V okně hello prostředku najdete v části hello možností pro správu prostředků hello. portál Hello zobrazí možnosti správy pro tuto konkrétní typ prostředku. Zobrazí příkazy pro správu hello napříč hello horní části okna prostředků hello a na levé straně hello.

![Správa prostředků](./media/resource-group-portal/manage-resources.png)

Z těchto možností můžete provádět operace, jako je například spuštění a zastavení virtuálního počítače nebo znovu konfigurovat vlastnosti hello hello virtuálního počítače.

## <a name="move-resources"></a>Přesunutí prostředků
Pokud potřebujete skupinu prostředků tooanother toomove prostředky nebo jiné předplatné, přečtěte si [přesunout skupiny prostředků toonew prostředků nebo předplatného](resource-group-move-resources.md).

## <a name="lock-resources"></a>Uzamčení prostředků
Můžete uzamknout předplatné, skupinu prostředků nebo prostředek tooprevent ostatní uživatelé ve vaší organizaci neúmyslnému odstranění nebo úprava důležitých prostředků. Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Zobrazit vaše předplatné a náklady
Pro všechny vaše prostředky můžete zobrazit informace o předplatném a hello zahrnuté náklady. Vyberte **odběry** a chcete, aby toosee předplatné hello. Může mít pouze jeden tooselect předplatné.

![předplatné](./media/resource-group-portal/select-subscription.png)

V okně hello předplatné najdete v části pracovní tempo.

![rychlost zápisu](./media/resource-group-portal/burn-rate.png)

A rozdělení nákladů podle typů prostředků.

![náklady na prostředek](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Export šablony
Po nastavení vaší skupiny prostředků, může být vhodné šablony Resource Manageru hello tooview pro skupinu prostředků hello. Export šablony hello nabízí dvě výhody:

1. Snadno můžete automatizovat budoucí nasazení řešení hello protože hello šablona obsahuje všechny hello kompletní infrastrukturu.
2. Můžete seznámit se syntaxí šablony prohlížením hello JSON JavaScript Object Notation () představující řešení.

Podrobné pokyny najdete v tématu [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Odstranit skupinu prostředků nebo prostředky
Odstranění skupiny prostředků se odstraní všechny hello prostředky, které jsou v něm obsažena. Můžete také odstranit jednotlivé prostředky ve skupině prostředků. Pokud odstraníte skupinu prostředků, protože může být prostředky další skupiny zdrojů, které jsou propojené tooit chcete tooexercise upozornění. Správce prostředků nedojde k odstranění propojené prostředky, ale nemusí správně fungovat bez hello očekává prostředků.

![Odstranění skupiny](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Další kroky
* Protokoly aktivity tooview, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).
* tooview podrobnosti o nasazení, najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* toodeploy prostředky prostřednictvím portálu hello, najdete v části [nasazení prostředků pomocí šablony Resource Manageru a portálu Azure](resource-group-template-deploy-portal.md).
* toomanage tooresources přístup, najdete v části [používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](../active-directory/role-based-access-control-configure.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

