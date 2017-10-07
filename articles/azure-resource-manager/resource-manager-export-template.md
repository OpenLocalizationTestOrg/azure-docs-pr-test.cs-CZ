---
title: "šablony Azure Resource Manageru aaaExport | Microsoft Docs"
description: "Pomocí Azure Resource Manageru tooexport šablonu z existující skupinu prostředků."
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
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: 94daa4812da2fec705044ca31c8e74e6d59bd53f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Vyexportování šablony Azure Resource Manageru z existujících prostředků
V tomto článku se dozvíte, jak tooexport šablony Resource Manageru ze stávajících prostředků ve vašem předplatném. Můžete použít tento vygenerované šablony toogain lépe porozumět syntaxe šablony.

Existují dva způsoby tooexport šablonu:

* Můžete exportovat hello **skutečné šablonu použít pro nasazení**. Hello vyexportované šablony zahrnuje všechny hello parametry a proměnné přesně tak, jak jsou uvedeny v původní šabloně hello. Tento přístup je užitečné, pokud jste nasadili prostředky prostřednictvím portálu hello a chcete toosee hello šablony toocreate tyto prostředky. Tato šablona je ihned použitelná. 
* Můžete exportovat **generované šablonu, která představuje hello aktuální stav skupiny prostředků hello**. Exportovaná šablona Hello není založena na všechny šablony, který jste použili pro nasazení. Místo toho vytvoří šablonu, která je snímek hello skupinu prostředků. Exportovaná šablona Hello má mnoho hodnot pevně a pravděpodobně ne tolik parametrů by obvykle definujete. Tento přístup je užitečné, když jste změnili skupiny prostředků hello po nasazení. Tato šablona obvykle vyžaduje úpravy, než je možné ji použít.

Toto téma ukazuje obou přístupů prostřednictvím portálu hello.

## <a name="deploy-resources"></a>Nasazení prostředků
Začněme nasazením tooAzure prostředky, které můžete použít pro export jako šablona. Pokud už máte skupinu prostředků v rámci vašeho předplatného, které chcete tooexport tooa šablony, můžete tuto část přeskočit. Hello zbývající část tohoto článku předpokládá, že jste nasadili hello webovou aplikaci a SQL database řešení uvedené v této části. Pokud používáte jiné řešení, prostředí může být jen málo liší, ale hello hello kroky tooexport šablony jsou stejné. 

1. V hello [portál Azure](https://portal.azure.com), vyberte **nový**.
   
      ![výběr možnosti Nový](./media/resource-manager-export-template/new.png)
2. Vyhledejte **web app + SQL** a vyberte z dostupných možností hello.
   
      ![vyhledání řešení Webová aplikace a SQL](./media/resource-manager-export-template/webapp-sql.png)

3. Vyberte **Vytvořit**.

      ![výběr možnosti Vytvořit](./media/resource-manager-export-template/create.png)

4. Zadejte hodnoty hello požadované hello webovou aplikaci a databázi SQL. Vyberte **Vytvořit**.

      ![zadání hodnot webu a SQL](./media/resource-manager-export-template/provide-web-values.png)

Hello nasazení může trvat několik minut. Po dokončení hello nasazení obsahuje vaše předplatné hello řešení.

## <a name="view-template-from-deployment-history"></a>Zobrazení šablony z historie nasazení
1. Přejděte toohello okně skupiny prostředků pro nové skupiny prostředků. Všimněte si, že hello okno zobrazí hello výsledek posledního nasazení hello. Vyberte tento odkaz.
   
      ![okno skupiny prostředků](./media/resource-manager-export-template/select-deployment.png)
2. Zobrazí historii nasazení pro skupinu hello. Ve vašem případě hello okno pravděpodobně uvádí jenom jedno nasazení. Vyberte ho.
   
     ![poslední nasazení](./media/resource-manager-export-template/select-history.png)
3. Hello zobrazuje souhrn hello nasazení. Hello souhrn obsahuje stav hello hello nasazení a jeho operací a hello hodnoty, které jste zadali pro parametry. toosee hello šablonu, kterou jste použili pro hello nasazení, vyberte **zobrazit šablonu**.
   
     ![zobrazení souhrnu nasazení](./media/resource-manager-export-template/view-template.png)
4. Resource Manager načte následující sedm soubory pro vás hello:
   
   1. **Šablona** -hello šablonu, která definuje hello infrastrukturu pro vaše řešení. Při vytváření účtu úložiště hello prostřednictvím portálu hello Resource Manager používá šablony toodeploy ho a tuto šablonu uložil pro budoucí použití.
   2. **Parametry** -soubor s parametry, můžete použít toopass hodnoty během nasazení. Obsahuje hello hodnoty, které jste zadali při prvním nasazení hello. Při opětovném nasazování šablony hello můžete tyto hodnoty.
   3. **Rozhraní příkazového řádku** -Azure rozhraní příkazového řádku (CLI) souboru skriptu, které můžete použít toodeploy hello šablony.
   3. **Rozhraní příkazového řádku 2.0** -Azure rozhraní příkazového řádku (CLI) souboru skriptu, které můžete použít toodeploy hello šablony.
   4. **Prostředí PowerShell** -souboru skriptu prostředí Azure PowerShell, které můžete použít toodeploy hello šablony.
   5. **Rozhraní .NET** -třídy A rozhraní .NET, které můžete použít toodeploy hello šablony.
   6. **Ruby** -A Ruby třídy, které můžete použít toodeploy hello šablony.
      
      Hello soubory jsou k dispozici prostřednictvím odkazů v okně hello. Ve výchozím nastavení zobrazí okno hello hello šablony.
      
       ![zobrazení šablony](./media/resource-manager-export-template/see-template.png)
      
Tato šablona je skutečný šablona hello používá toocreate vaší webové aplikace a SQL database. Všimněte si, že obsahuje parametry, které umožňují tooprovide různé hodnoty během nasazení. toolearn Další informace o struktuře hello šablon, najdete v části [šablon pro tvorbu Azure Resource Manageru](resource-group-authoring-templates.md).

## <a name="export-hello-template-from-resource-group"></a>Exportovat šablonu hello ze skupiny prostředků
Pokud máte ručně změnit vaše prostředky nebo přidat prostředky do více nasazení, načítání šablonu z historie nasazení hello neodráží hello aktuální stav skupiny prostředků hello. Tato část uvádí, jak hello tooexport šablonu, která odráží aktuální stav skupiny prostředků hello. 

> [!NOTE]
> Nejde exportovat šablonu pro skupinu prostředků, která má víc než 200 prostředků.
> 
> 

1. Šablona hello tooview pro skupinu prostředků, vyberte **skriptu pro automatizaci**.
   
      ![export skupiny prostředků](./media/resource-manager-export-template/select-automation.png)
   
     Správce prostředků vyhodnocuje hello prostředky ve skupině prostředků hello a generuje šablonu pro tyto prostředky. Ne všechny typy prostředků podporují funkce exportu šablony hello. Může se zobrazit chyba oznamující, že došlo k potížím s hello export. Zjistíte, jak toohandle ty problémy v hello [opravte problémy při exportu](#fix-export-issues) části.
2. Znovu naleznete v tématu hello šest souborů, které můžete použít tooredeploy hello řešení. Tato šablona hello čas je však jen málo liší. Všimněte si, že hello vygenerované šablony obsahuje méně parametrů, než hello šablony v předchozí části. Navíc mnoho hodnot hello (např. umístění a hodnoty SKU) jsou pevně zakódovaná v této šabloně místo přijetí hodnotu parametru. Před opětovným použitím této šablony, můžete chtít tooedit hello šablony toomake lepší použití parametrů. 
   
3. Máte několik možností pro pokračování toowork s touto šablonou. Můžete stáhnout hello šablonu a pracovat na něm místně JSON editor. Nebo můžete uložit hello šablona tooyour knihovny a na něm pracovat prostřednictvím portálu hello.
   
     Pokud umíte pomocí editoru JSON jako [VS Code](https://code.visualstudio.com/) nebo [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), možná budete chtít stažení šablony hello místně a pomocí tohoto editoru. toowork místně, vyberte **Stáhnout**.
   
      ![stažení šablony](./media/resource-manager-export-template/download-template.png)
   
     Pokud nemáte nastavení v editoru JSON, možná budete chtít úpravy šablony hello prostřednictvím portálu hello. Hello zbývající část tohoto tématu se předpokládá, že jste uložili hello šablona tooyour knihovny portálu hello. Však provedete hello stejná syntaxe změny šablony toohello zda lokálně pomocí JSON editor nebo přes portál hello. Vyberte toowork prostřednictvím portálu hello **přidat toolibrary**.
   
      ![Přidat toolibrary](./media/resource-manager-export-template/add-to-library.png)
   
     Při přidávání knihovnu toohello šablony, zadejte název a popis šablony hello. Potom vyberte **Uložit**.
   
     ![nastavení hodnot šablony](./media/resource-manager-export-template/save-library-template.png)
4. Vyberte tooview šablonu uložit v knihovně, **další služby**, typ **šablony** toofilter výsledky, vyberte **šablony**.
   
      ![vyhledání šablon](./media/resource-manager-export-template/find-templates.png)
5. Vyberte šablonu hello hello názvem, který jste uložili.
   
      ![výběr šablony](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-hello-template"></a>Přizpůsobení šablony hello
Hello exportovat šablonu funguje dobře, že pokud chcete, aby toocreate hello stejné webové aplikaci a SQL database pro všechna nasazení. Resource Manager ale nabízí možnosti pro ještě flexibilnější nasazení šablon. Tento článek ukazuje, jak tooadd parametry pro hello databáze jméno a heslo správce. Tento stejný tooadd přístupu můžete použít pro ostatní hodnoty v šabloně hello větší flexibilitu.

1. toocustomize hello šablony, vyberte **upravit**.
   
     ![zobrazení šablony](./media/resource-manager-export-template/select-edit.png)
2. Vyberte šablonu hello.
   
     ![Úprava šablony](./media/resource-manager-export-template/select-added-template.png)
3. toobe možné toopass hello hodnoty, můžete chtít toospecify během nasazení, přidejte následující dva parametry toohello hello **parametry** oddílu v šabloně hello:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. toouse hello nové parametry, nahraďte hello SQL server definice v hello **prostředky** části. Všimněte si, že **administratorLogin** a **administratorLoginPassword** jsou teď hodnoty parametrů.

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

6. Vyberte **OK** po dokončení úprav hello šablony.
7. Vyberte **Uložit** toosave hello změny toohello šablony.
   
     ![uložení šablony](./media/resource-manager-export-template/save-template.png)
8. tooredeploy hello aktualizované šablony, vyberte **nasadit**.
   
     ![nasazení šablony](./media/resource-manager-export-template/redeploy-template.png)
9. Zadejte hodnoty parametrů a vyberte prostředek skupiny toodeploy hello prostředků pro.


## <a name="fix-export-issues"></a>Oprava problémů s exportem
Ne všechny typy prostředků podporují funkce exportu šablony hello. tooresolve-li tento problém, ručně přidejte chybějící prostředky, které hello zpět do šablony. Hello chybová zpráva obsahuje hello typů prostředků, které nelze exportovat. Vyhledejte tyto typy prostředků v [referenčních informacích k šablonám](/azure/templates/). Například toomanually přidat bránu virtuální sítě najdete v tématu [odkaz na šablonu Microsoft.Network/virtualNetworkGateways](/azure/templates/microsoft.network/virtualnetworkgateways).

> [!NOTE]
> K problémům s exportem může dojít pouze při exportu ze skupiny prostředků, ne z historie nasazení. Pokud vaše poslední nasazení přesně představuje hello aktuální stav skupiny prostředků hello, je třeba exportovat šablonu hello z historie nasazení hello místo skupiny prostředků hello. Exportujte pouze ze skupiny prostředků po provedení změny toohello skupiny prostředků nejsou definovány v jediné šabloně.
> 
> 

## <a name="next-steps"></a>Další kroky
Jste se naučili, jak tooexport šablonu z prostředků, které jste vytvořili v portálu hello.

* Šablonu můžete nasadit pomocí těchto možností: [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) nebo [REST API](resource-group-template-deploy-rest.md).
* jak zjistit, tooexport šablonu prostřednictvím Powershellu, toosee [použití Azure Powershellu s Azure Resource Manager](powershell-azure-resource-manager.md).
* jak tooexport šablonu prostřednictvím rozhraní příkazového řádku Azure, najdete v části toosee [hello použití Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru](xplat-cli-azure-resource-manager.md).

