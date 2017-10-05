---
title: "Nasazení prostředků Azure pomocí portálu Azure | Microsoft Docs"
description: "Portál Azure a Azure Resource Manageru použijte k nasazení vašich prostředků."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Nasazení prostředků pomocí šablon Resource Manageru a portálu Azure Portal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Azure Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Toto téma ukazuje, jak používat [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) nasazení vašich prostředků Azure. Další informace o správě prostředků najdete v tématu [Azure spravovat prostředky prostřednictvím portálu](resource-group-portal.md).

Ne všechny služby v současné době podporuje portál nebo Resource Manager. Pro tyto služby, budete muset použít [portálu classic](https://manage.windowsazure.com). Stav jednotlivých služeb, naleznete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků
1. Vytvořit skupinu prostředků prázdný, vyberte **nový** > **správy** > **skupiny prostředků**.
   
    ![Vytvoření prázdné skupině prostředků](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Poskytněte název a umístění a v případě potřeby vyberte předplatné. Je třeba zadat umístění pro skupinu prostředků, protože skupina prostředků ukládá metadata o prostředcích. Kvůli dodržování předpisů můžete určit, kam je uložen aby metadata. Doporučujeme obecně platí, že zadáte umístění, kde se bude nacházet většina vašich prostředků. Pomocí stejného umístění, můžete zjednodušit vaše šablony.
   
    ![nastavené hodnoty skupiny](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Nasadit prostředky z webu Marketplace
Po vytvoření skupiny prostředků, můžete nasadit prostředky k němu z Marketplace. Marketplace obsahuje předem definovaná řešení pro běžné scénáře.

1. Chcete-li spustit nasazení, vyberte **nový** a typ prostředku, kterou chcete nasadit. Poté vyhledejte konkrétní verzi prostředek, který chcete nasadit.
   
    ![nasazení prostředků](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Pokud nevidíte konkrétní řešení, které chcete nasadit, můžete vyhledat na webu Marketplace ho.
   
    ![hledání marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. V závislosti na typu vybraného prostředku máte kolekci relevantní vlastnosti nastavit před nasazením. Tyto možnosti nejsou zobrazeny zde, jak se budou lišit v závislosti na typu prostředku. Pro všechny typy musíte vybrat cílové skupiny prostředků. Následující obrázek ukazuje, jak vytvořit webovou aplikaci a nasadit do skupiny prostředků, kterou jste vytvořili.
   
    ![Vytvořte skupinu prostředků](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Alternativně můžete vytvořit skupinu prostředků, při nasazení vašich prostředků. Vyberte **vytvořit nový** a zadejte název skupiny prostředků.
   
    ![Vytvořit novou skupinu prostředků](./media/resource-group-template-deploy-portal/select-new-group.png)
4. Spustí se vaše nasazení. Nasazení může trvat několik minut. Pokud nasazení úspěšně proběhlo, zobrazí se upozornění.
   
    ![zobrazení oznámení](./media/resource-group-template-deploy-portal/view-notification.png)
5. Po nasazení vašich prostředků, můžete přidat více prostředků do skupiny prostředků pomocí **přidat** příkazu v okně skupiny prostředků.
   
    ![přidání prostředku](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Nasadit prostředky z vlastní šablony
Pokud chcete provést nasazení, ale nechcete použít některou z šablon na webu Marketplace, můžete vytvořit vlastní šablonu, která definuje infrastrukturu pro vaše řešení. Další informace o vytváření šablon najdete v tématu [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

1. Nasadit vlastní šablonu prostřednictvím portálu, vyberte **nový**a spusťte hledání **nasazení šablony** dokud ho můžete vybrat z možností.
   
    ![nasazení šablony vyhledávání](./media/resource-group-template-deploy-portal/search-template.png)
2. Vyberte **nasazení šablony** z dostupných prostředků.
   
    ![Vyberte šablonu nasazení](./media/resource-group-template-deploy-portal/select-template.png)
3. Po spuštění nasazení šablony, otevřete prázdné šablonu, kterou je k dispozici pro přizpůsobení.
   
    ![Vytvoření šablony](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    V editoru přidejte syntaxe JSON, který definuje prostředky, které chcete nasadit. Vyberte **Uložit** po dokončení. Pokyny v zápisu JSON syntaxi najdete v tématu [názorný Průvodce šablonou Resource Manageru](resource-manager-template-walkthrough.md).
   
    ![Úprava šablony](./media/resource-group-template-deploy-portal/edit-template.png)
4. Nebo můžete vybrat existující šablony z [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/). Tyto šablony jsou přispěli komunitou. Pokrývaly mnoha běžným scénářům a někdo přidat šablonu, která je podobná co se pokoušíte nasadit. Můžete hledat šablony najít něco, která odpovídá vašemu scénáři.
   
    ![Vyberte šablonu rychlý start](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    Vybranou šablonu můžete zobrazit v editoru.
5. Po zadání všech hodnot, vyberte **vytvořit** k nasazení šablony. 
   
    ![nasazení šablony](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Nasadit prostředky ze šablony do účtu
Na portálu můžete uložit šablonu k účtu Azure a znovu ji nasaďte později. Další informace o práci s těmito uložit šablony [Začínáme se soukromými šablonami na portálu Azure](../marketplace-consumer/mytemplates-getstarted.md).

1. Chcete-li najít uložené šablony, vyberte **Procházet** > **šablony**.
   
    ![Procházet šablony](./media/resource-group-template-deploy-portal/browse-templates.png)
2. Seznam šablon do účtu vyberte ten, který si přejete pracovat na.
   
    ![uložené šablony](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Vyberte **nasadit** k opětovnému nasazení této šablony uložené.
   
    ![nasazení uloženého šablony](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Další kroky
* Chcete-li zobrazit protokoly auditu, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).
* Chcete-li vyřešit chyby nasazení, přečtěte si téma [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* Pokud chcete načíst šablonu z nasazení nebo skupinu prostředků, najdete v části [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

