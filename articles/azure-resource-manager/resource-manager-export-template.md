---
title: "Vyexportování šablony Azure Resource Manageru | Dokumentace Microsoftu"
description: "Pomocí Azure Resource Manageru si můžete vyexportovat šablonu z existující skupiny prostředků."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: c8f19a4f0aadbee2de97bb3ec85c2c85b493a394
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Vyexportování šablony Azure Resource Manageru z existujících prostředků
V tomto článku se naučíte, jak exportovat šablonu Resource Manageru z existujících prostředků ve vašem předplatném. Tuto vygenerovanou šablonu můžete použít k lepšímu pochopení syntaxe šablon.

Existují dva způsoby, jak exportovat šablonu:

* Můžete exportovat **skutečnou šablonu použitou k nasazení**. Exportovaná šablona zahrnuje všechny parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně. Tento přístup je užitečný, pokud jste nasadili prostředky prostřednictvím portálu a chcete vidět šablonu, která tyto prostředky vytvoří. Tato šablona je ihned použitelná. 
* Můžete exportovat **vygenerovanou šablonu, která představuje aktuální stav skupiny prostředků**. Exportovaná šablona není založena na žádné šabloně, kterou jste použili k nasazení. Místo toho export vytvoří šablonu, která je snímkem aktuálního stavu skupiny prostředků. Exportovaná šablona má řadu pevně definovaných hodnot a pravděpodobně méně parametrů, než byste obvykle definovali. Tento přístup je užitečný, pokud jste po nasazení upravili skupinu prostředků. Tato šablona obvykle vyžaduje úpravy, než je možné ji použít.

Toto téma ukazuje oba přístupy prostřednictvím portálu.

## <a name="deploy-resources"></a>Nasazení prostředků
Začněme tím, že do Azure nasadíme prostředky, které můžete použít k exportu v podobě šablony. Pokud už v předplatném máte skupinu prostředků, kterou chcete exportovat do šablony, můžete tuto část přeskočit. Zbývající část tohoto článku předpokládá, že jste nasadili webovou aplikaci a řešení databáze SQL uvedené v této části. Pokud používáte jiné řešení, vaše prostředí může být trochu jiné, ale postup exportu šablony je stejný. 

1. Na webu [Azure Portal](https://portal.azure.com) vyberte **Nový**.
   
      ![výběr možnosti Nový](./media/resource-manager-export-template/new.png)
2. Vyhledejte řešení **Webová aplikace a SQL** a vyberte ho z dostupných možností.
   
      ![vyhledání řešení Webová aplikace a SQL](./media/resource-manager-export-template/webapp-sql.png)

3. Vyberte **Vytvořit**.

      ![výběr možnosti Vytvořit](./media/resource-manager-export-template/create.png)

4. Zadejte požadované hodnoty pro webovou aplikaci a databázi SQL. Vyberte **Vytvořit**.

      ![zadání hodnot webu a SQL](./media/resource-manager-export-template/provide-web-values.png)

Nasazení může trvat minutu. Po dokončení nasazení vaše předplatné obsahuje řešení.

## <a name="view-template-from-deployment-history"></a>Zobrazení šablony z historie nasazení
1. Přejděte do okna nové skupiny prostředků. Všimněte si, že okno zobrazuje výsledek posledního nasazení. Vyberte tento odkaz.
   
      ![okno skupiny prostředků](./media/resource-manager-export-template/select-deployment.png)
2. Zobrazí se vám historie nasazení pro skupinu. Ve vašem případě bude okno pravděpodobně uvádět jenom jedno nasazení. Vyberte ho.
   
     ![poslední nasazení](./media/resource-manager-export-template/select-history.png)
3. Okno zobrazí souhrn vybraného nasazení. Souhrn obsahuje stav nasazení a jeho operací a také hodnoty, které jste zadali pro parametry. Pokud chcete zobrazit šablonu, kterou jste použili k nasazení, vyberte možnost **Zobrazit šablonu**.
   
     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/view-template.png)
4. Resource Manager pro vás načte následujících sedm souborů:
   
   1. **Template** - Šablona, která definuje infrastrukturu pro vaše řešení. Když jste prostřednictvím portálu vytvářeli účet úložiště, Resource Manager k jeho nasazení použil šablonu a tuto šablonu uložil pro budoucí použití.
   2. **Parameters** - Soubor s parametry, který slouží k předávání hodnot během nasazení. Obsahuje hodnoty, které jste zadali při prvním nasazení. Kteroukoli z těchto hodnot můžete při opětovném nasazování šablony změnit.
   3. **CLI** - Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony.
   3. **CLI 2.0** – Soubor skriptu rozhraní příkazového řádku Azure CLI, který můžete použít k nasazení šablony
   4. **PowerShell** - Soubor skriptu Azure PowerShellu, který můžete použít k nasazení šablony.
   5. **.NET** - Třída .NET, kterou můžete použít k nasazení šablony.
   6. **Ruby** - Třída Ruby, kterou můžete použít k nasazení šablony.
      
      Soubory jsou k dispozici prostřednictvím odkazů v rámci okna. Ve výchozím nastavení zobrazí okno šablonu.
      
       ![zobrazení šablony](./media/resource-manager-export-template/see-template.png)
      
Toto je skutečná šablona použitá k vytvoření vaší webové aplikace a databáze SQL. Všimněte si, že obsahuje parametry, které vám umožňují během nasazení zadat jiné hodnoty. Další informace o struktuře šablon najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).

## <a name="export-the-template-from-resource-group"></a>Export šablony ze skupiny prostředků
Pokud jste ručně změnili prostředky nebo přidali prostředky ve více nasazeních, získání šablony z historie nasazení nebude odrážet aktuální stav skupiny prostředků. V této části se dozvíte, jak exportovat šablonu, která odráží aktuální stav skupiny prostředků. 

> [!NOTE]
> Nejde exportovat šablonu pro skupinu prostředků, která má víc než 200 prostředků.
> 
> 

1. Pokud chcete zobrazit šablonu pro skupinu prostředků, vyberte **Skript automatizace**.
   
      ![export skupiny prostředků](./media/resource-manager-export-template/select-automation.png)
   
     Resource Manager vyhodnotí prostředky ve skupině prostředků a vygeneruje pro tyto prostředky šablonu. Ne všechny typy prostředků podporují funkci exportu šablony. Může se zobrazit chyba oznamující, že došlo k problému s exportem. Postup řešení těchto problémů najdete v části [Oprava problémů s exportem](#fix-export-issues).
2. Znovu se zobrazí šest souborů, které můžete použít k opětovnému nasazení řešení. Tentokrát je však šablona trochu jiná. Všimněte si, že vygenerovaná šablona obsahuje méně parametrů než šablona v předchozí části. Navíc je v této šabloně mnoho hodnot (jako hodnoty umístění a skladové položky) pevně zakódovaných místo toho, aby přijímaly hodnotu parametru. Před opětovným použitím této šablony možná budete chtít šablonu upravit, aby lépe využívala parametry. 
   
3. Máte pár možností, jak s touto šablonou dále pracovat. Šablonu si můžete stáhnout a pracovat na ní místně v editoru JSON nebo si ji můžete uložit do knihovny a pracovat s ní prostřednictvím portálu.
   
     Pokud se vám s editorem JSON, jako je [VS Code](https://code.visualstudio.com/) nebo [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), dobře pracuje, můžete si ji stáhnout místně a použít tento editor. Pokud chcete pracovat místně, vyberte **Stáhnout**.
   
      ![stažení šablony](./media/resource-manager-export-template/download-template.png)
   
     Pokud editor JSON nechcete používat, můžete preferovat úpravy šablony prostřednictvím portálu. Ve zbývající části tohoto tématu se předpokládá, že máte šablonu uloženou v knihovně na portálu. Stejné změny syntaxe ale můžete v šabloně provést, ať pracujete místně v editoru JSON nebo prostřednictvím portálu. Pokud chcete pracovat přes portál, vyberte **Přidat do knihovny**.
   
      ![přidání do knihovny](./media/resource-manager-export-template/add-to-library.png)
   
     Když přidáváte šablonu do knihovny, uveďte její název a popis. Potom vyberte **Uložit**.
   
     ![nastavení hodnot šablony](./media/resource-manager-export-template/save-library-template.png)
4. Pokud chcete šablonu uloženou v knihovně zobrazit, vyberte **Další služby**, napište **Šablony**, abyste vyfiltrovali výsledky, a vyberte **Šablony**.
   
      ![vyhledání šablon](./media/resource-manager-export-template/find-templates.png)
5. Vyberte šablonu s názvem, který jste uložili.
   
      ![výběr šablony](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-the-template"></a>Přizpůsobení šablony
Vyexportovaná šablona funguje správně, pokud chcete vytvořit stejnou webovou aplikaci a databázi SQL pro všechna nasazení. Resource Manager ale nabízí možnosti pro ještě flexibilnější nasazení šablon. V tomto článku zjistíte, jak přidat parametry pro jméno a heslo správce databáze. Stejný postup můžete použít k přidání větší flexibility pro ostatní hodnoty v šabloně.

1. Vyberte **Upravit** pro přizpůsobení šablony.
   
     ![zobrazení šablony](./media/resource-manager-export-template/select-edit.png)
2. Vyberte šablonu.
   
     ![Úprava šablony](./media/resource-manager-export-template/select-added-template.png)
3. Abyste mohli předat hodnoty, které byste mohli chtít zadat během nasazování, přidejte následující dva parametry do části **parameters** v šabloně:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. Pokud chcete použít nové parametry, nahraďte definici SQL serveru v části **resources**. Všimněte si, že **administratorLogin** a **administratorLoginPassword** jsou teď hodnoty parametrů.

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. Po dokončení úprav šablony vyberte **OK**.
7. Uložte změny šablony kliknutím na **Uložit**.
   
     ![uložení šablony](./media/resource-manager-export-template/save-template.png)
8. Pokud chcete aktualizovanou šablonu znovu nasadit, vyberte **Nasadit**.
   
     ![nasazení šablony](./media/resource-manager-export-template/redeploy-template.png)
9. Zadejte hodnoty parametrů a vyberte skupinu prostředků, do které prostředky nasadíte.


## <a name="fix-export-issues"></a>Oprava problémů s exportem
Ne všechny typy prostředků podporují funkci exportu šablony. Tento problém můžete vyřešit ručním přidáním chybějících prostředků zpět do šablony. Chybová zpráva obsahuje typy prostředků, které se nedají exportovat. Vyhledejte tyto typy prostředků v [referenčních informacích k šablonám](/azure/templates/). Pokud například chcete ručně přidat bránu virtuální sítě, přečtěte si [referenční informace k šablonám o prostředku Microsoft.Network/virtualNetworkGateways](/azure/templates/microsoft.network/virtualnetworkgateways).

> [!NOTE]
> K problémům s exportem může dojít pouze při exportu ze skupiny prostředků, ne z historie nasazení. Pokud vaše poslední nasazení přesně reprezentuje aktuální stav skupiny prostředků, měli byste šablonu exportovat z historie nasazení, ne ze skupiny zdrojů. Ze skupiny zdrojů exportujte pouze tehdy, pokud jste ve skupině prostředků provedli změny, které nejsou definovány v jedné šabloně.
> 
> 

## <a name="next-steps"></a>Další kroky
Naučili jste se, jak vyexportovat šablonu z prostředků, které jste vytvořili na portálu.

* Šablonu můžete nasadit pomocí těchto možností: [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) nebo [REST API](resource-group-template-deploy-rest.md).
* Informace o tom, jak vyexportovat šablonu prostřednictvím PowerShellu, najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](powershell-azure-resource-manager.md).
* Informace o tom, jak vyexportovat šablonu prostřednictvím rozhraní příkazového řádku Azure CLI, najdete v tématu věnovaném [Použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).

