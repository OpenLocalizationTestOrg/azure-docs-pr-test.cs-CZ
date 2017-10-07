---
title: "aaaUse Azure portálu toodeploy prostředky Azure | Microsoft Docs"
description: "Pomocí portálu Azure a Azure Resource Manageru toodeploy vašich prostředků."
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
ms.openlocfilehash: 5a5217f94c8dfc0c1ebd613903ea3dcbe1197bfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Nasazení prostředků pomocí šablon Resource Manageru a portálu Azure Portal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Azure Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

Toto téma ukazuje, jak toouse hello [portál Azure](https://portal.azure.com) s [Azure Resource Manager](resource-group-overview.md) toodeploy vašich prostředků Azure. toolearn o správu prostředků, najdete v části [Azure spravovat prostředky prostřednictvím portálu](resource-group-portal.md).

Ne všechny služby v současné době podporuje hello portál nebo Resource Manager. Pro tyto služby, je třeba toouse hello [portálu classic](https://manage.windowsazure.com). Hello stav každé služby, najdete v části [Azure portálu dostupnosti grafu](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Vytvoření skupiny prostředků
1. toocreate prázdné skupině prostředků, vyberte **nový** > **správy** > **skupiny prostředků**.
   
    ![Vytvoření prázdné skupině prostředků](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Poskytněte název a umístění a v případě potřeby vyberte předplatné. Tooprovide umístění pro skupinu prostředků hello vyžadují, protože skupina prostředků hello ukládají metadata o hello prostředky. Kvůli dodržování předpisů můžete toospecify se uloží aby metadata. Doporučujeme obecně platí, že zadáte umístění, kde se bude nacházet většina vašich prostředků. Pomocí hello stejné umístění můžete zjednodušit vaše šablony.
   
    ![nastavené hodnoty skupiny](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Nasadit prostředky z webu Marketplace
Po vytvoření skupiny prostředků, můžete nasadit tooit prostředky z hello Marketplace. Hello Marketplace obsahuje předem definovaná řešení pro běžné scénáře.

1. toostart nasazení, vyberte **nový** a hello typu prostředku byste chtěli toodeploy. Pak, hledejte pro konkrétní verzi hello prostředků hello chcete toodeploy.
   
    ![nasazení prostředků](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Pokud nevidíte konkrétní řešení hello chcete toodeploy, můžete vyhledat hello Marketplace ho.
   
    ![hledání marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. V závislosti na typu hello vybraného prostředku máte kolekci relevantní vlastnosti tooset před nasazením. Tyto možnosti nejsou zobrazeny zde, jak se budou lišit v závislosti na typu prostředku. Pro všechny typy musíte vybrat cílové skupiny prostředků. Hello následující obrázek znázorňuje, jak toocreate webové aplikace a nasadit ji toohello prostředků skupinu, kterou jste vytvořili.
   
    ![Vytvořte skupinu prostředků](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Alternativně můžete rozhodnout toocreate skupinu prostředků, při nasazení vašich prostředků. Vyberte **vytvořit nový** a zadejte název skupiny prostředků hello.
   
    ![Vytvořit novou skupinu prostředků](./media/resource-group-template-deploy-portal/select-new-group.png)
4. Spustí se vaše nasazení. Hello nasazení může trvat několik minut. Po dokončení nasazení hello zobrazí oznámení.
   
    ![zobrazení oznámení](./media/resource-group-template-deploy-portal/view-notification.png)
5. Po nasazení vašich prostředků, můžete přidat další skupiny zdrojů toohello prostředky pomocí hello **přidat** příkazu v okně skupiny prostředků hello.
   
    ![přidání prostředku](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Nasadit prostředky z vlastní šablony
Pokud chcete tooexecute nasazení, ale není použít některou z šablon hello v hello Marketplace, můžete vytvořit vlastní šablonu, která definuje hello infrastrukturu pro vaše řešení. toolearn o vytváření šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

1. Vyberte toodeploy vlastní šablonu prostřednictvím portálu hello **nový**a spusťte hledání **nasazení šablony** dokud ho můžete vybrat z možností hello.
   
    ![nasazení šablony vyhledávání](./media/resource-group-template-deploy-portal/search-template.png)
2. Vyberte **nasazení šablony** z hello dostupné prostředky.
   
    ![Vyberte šablonu nasazení](./media/resource-group-template-deploy-portal/select-template.png)
3. Po spuštění nasazení šablony hello, otevřete hello prázdné šablonu, která je k dispozici pro přizpůsobení.
   
    ![Vytvoření šablony](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    V editoru hello přidáte hello syntaxe JSON, který definuje prostředky hello chcete toodeploy. Vyberte **Uložit** po dokončení. Pokyny v zápisu JSON syntaxe hello najdete v tématu [názorný Průvodce šablonou Resource Manageru](resource-manager-template-walkthrough.md).
   
    ![Úprava šablony](./media/resource-group-template-deploy-portal/edit-template.png)
4. Nebo můžete vybrat existující šablonu z hello [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/). Tyto šablony jsou přispěli hello komunity. Pokrývaly mnoha běžným scénářům a někdo přidat šablonu, která je podobné toowhat se pokoušíte toodeploy. Můžete hledat hello šablony toofind něco, která odpovídá vašemu scénáři.
   
    ![Vyberte šablonu rychlý start](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    Vybranou šablonu hello můžete zobrazit v editoru hello.
5. Po zadání všech hello jiné hodnoty, vyberte **vytvořit** toodeploy hello šablony. 
   
    ![nasazení šablony](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-tooyour-account"></a>Nasadit prostředky ze šablony uložit tooyour účtu
portál Hello umožňuje toosave šablony tooyour účet Azure a znovu nasaďte později. Další informace o práci s těmito uložit šablony [Začínáme se soukromými šablonami na portálu Azure hello](../marketplace-consumer/mytemplates-getstarted.md).

1. Vyberte uložené šablony toofind **Procházet** > **šablony**.
   
    ![Procházet šablony](./media/resource-group-template-deploy-portal/browse-templates.png)
2. Hello seznam šablon uložit tooyour účtu vyberte, kterou chcete toowork na hello.
   
    ![uložené šablony](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Vyberte **nasadit** tooredeploy to uložit šablonu.
   
    ![nasazení uloženého šablony](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Další kroky
* protokoly auditu tooview, najdete v části [auditovat operace s Resource Managerem](resource-group-audit.md).
* chyby nasazení tootroubleshoot, najdete v části [zobrazit operace nasazení](resource-manager-deployment-operations.md).
* tooretrieve šablonu z nasazení nebo skupinu prostředků, najdete v části [šablony exportovat Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).
* Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).

