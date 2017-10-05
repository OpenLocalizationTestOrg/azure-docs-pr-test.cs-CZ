---
title: "Přesunutí nové předplatné nebo prostředek skupiny prostředků Azure | Microsoft Docs"
description: "Azure Resource Manager využívat k přesunu prostředků do nové skupiny prostředků nebo předplatného."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: tomfitz
ms.openlocfilehash: e138f80e808968ab4bf5c11cfd5fd46fe4a1bcce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Přesunutím prostředků do nové skupiny prostředků nebo předplatného
Toto téma ukazuje, jak k přesunu prostředků do nového předplatného nebo novou skupinu prostředků ve stejném předplatném. Můžete na portálu, prostředí PowerShell, rozhraní příkazového řádku Azure nebo rozhraní REST API pro přesun prostředků. Operace přesunutí v tomto tématu jsou vám k dispozici bez jakékoli pomoci z podpory Azure.

Při přesouvání prostředků, skupině zdrojové a cílové skupiny jsou zamčené během operace. Zápis a operace odstranění jsou zablokované na skupiny prostředků, až po dokončení přesunu. Tato Zámek znamená nelze přidat, aktualizovat nebo odstranit prostředky ve skupině prostředků, ale neznamená, že zmrazené prostředky. Pokud přesunete SQL Server a jeho databáze do nové skupiny prostředků, například aplikace, která používá databázi vyskytne nedojde k žádnému výpadku. Můžete dál číst a zapisovat do databáze.

Nelze změnit umístění prostředku. Přesunutí prostředku přesouvá pouze ji do nové skupiny prostředků. Nové skupiny prostředků může mít jiné umístění, ale které nelze změnit umístění prostředku.

> [!NOTE]
> Tento článek popisuje postup přesunutí prostředků v Azure existující účet nabízení. Pokud skutečně chcete změnit účtu Azure nabídky (například upgrade z průběžné platby předem věnovat) přitom dál pracovat se stávajícími prostředky, najdete v článku [vašeho předplatného Azure přepnout na jinou nabídku](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Kontrolní seznam před přesunutím prostředků
Před přesunutím prostředku je nutné provést několik důležitých kroků. Ověřením těchto podmínek se můžete vyhnout chybám.

1. Zdrojové a cílové odběry, musí existovat v rámci stejné [klienta Azure Active Directory](../active-directory/active-directory-howto-tenant.md). Chcete-li zkontrolovat, že oba odběry obsahují stejné ID klienta, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.

  Pro Azure PowerShell použijte:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  Azure CLI 2.0 použijte:

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  Pokud klient ID pro zdrojové a cílové předplatné nejsou stejné, pokuste se změnit adresář pro předplatné. Však tato možnost je dostupná jenom pro správce služby, který je přihlášený pomocí účtu Microsoft (není účet organizace). Aby se změna adresáři, přihlaste se k [portálu classic](https://manage.windowsazure.com/)a vyberte **nastavení**a vyberte předplatné. Pokud **upravit adresář** ikonu je k dispozici, vyberte ji chcete změnit přidružené Azure Active Directory.

  ![Upravit adresář](./media/resource-group-move-resources/edit-directory.png)

  Pokud tuto ikonu není k dispozici, obraťte se na podporu pro přesun prostředků do nového klienta.

2. Služba musí umožňovat operaci přesouvání prostředků. Toto téma uvádí služby, které Povolit přesunutí prostředků a služby, které nepovolíte přesunutí prostředků.
3. Cílové předplatné musí být registrováno pro poskytovatele přesouvaného prostředku. Pokud ne, se zobrazí chybová zpráva s informacemi, které **předplatné není zaregistrované pro typ prostředku**. K problému může dojít, pokud přesouváte prostředek do nového předplatného, ale toto předplatné nebylo pro příslušný typ prostředku nikdy použito. Další informace o stavu registrace a registraci poskytovatelů prostředků najdete v tématu [Typy prostředků a jejich poskytovatelé](resource-manager-supported-services.md).

## <a name="when-to-call-support"></a>Při volání podpory
Většina prostředkům prostřednictvím operace samoobslužné služby zobrazí v tomto tématu se můžete přesunout. Pomocí operace samoobslužné služby, které se:

* Přesouvání prostředků Resource Manageru
* Přesunout klasické prostředky podle požadavků [omezení nasazení classic](#classic-deployment-limitations).

Až budete potřebovat, obraťte se na podporu:

* Přesuňte vašich prostředků na nový účet Azure (a klienta Azure Active Directory).
* Přesunout klasické prostředky ale dochází k potížím s omezeními.

## <a name="services-that-enable-move"></a>Služby, které umožňují přesunout
Prozatím se služeb, které Přesun na novou skupinu prostředků a předplatného jsou:

* API Management
* Aplikace služby App Service (webové aplikace) – viz [omezení služby App Service](#app-service-limitations)
* Application Insights
* Automation
* Batch
* Mapy Bing
* CDN
* Cloudové služby - viz [omezení nasazení Classic](#classic-deployment-limitations)
* Kognitivní služby
* Content Moderator
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Azure Cosmos DB
* Event Hubs
* Clustery HDInsight - najdete v části [omezení HDInsight](#hdinsight-limitations)
* Centra IoT
* Key Vault
* Nástroje pro vyrovnávání zatížení
* Logic Apps
* Machine Learning
* Media Services
* Mobile Engagement
* Notification Hubs
* Operational Insights
* Správa operací
* Power BI
* Redis Cache
* Scheduler
* Search
* Správa serveru
* Service Bus
* Service Fabric
* Úložiště
* Najdete v části úložiště (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)
* Stream Analytics - Stream Analytics úlohy nelze přesunout, při spuštění ve stavu.
* Databáze SQL server – databáze a server se musí nacházet ve stejné skupině prostředků. Když přesouváte systému SQL server, budou přesunuty také všechny její databáze.
* Traffic Manager
* Virtuální počítače
* Virtuální počítače s certifikátem uložené v Key Vault – přesunout do nového prostředku skupiny v rámci stejného předplatného je povolena, ale přesun křížové předplatného není povolená.
* Virtuální počítače (klasické) - najdete v části [omezení nasazení Classic](#classic-deployment-limitations)
* Škálovací sady virtuálních počítačů
* Virtuální sítě - aktuálně, peered virtuální sítě nelze přesunout, dokud partnerský vztah virtuální sítě je zakázané. Jakmile zakázaná, virtuální sítě možné úspěšně přesunout a lze je povolit partnerského vztahu virtuální sítě. Kromě toho virtuální sítě nelze přesunout do jiného předplatného, pokud virtuální síť obsahuje všechny podsítě s odkazy na zdroje navigace. Například podsíť virtuální sítě nemá navigační odkaz na prostředek, pokud prostředek redis Microsoft.Cache je nasazený do dané podsítě.
* VPN Gateway


## <a name="services-that-do-not-enable-move"></a>Služby, které nepovolujte přesunutí
Služby, které aktuálně nepovolujte přesunutí prostředku jsou:

* Služba AD Domain Services
* Hybridní AD Health Service
* Application Gateway
* S virtuálními počítači s disky spravovat sady dostupnosti
* BizTalk Services
* Container Service
* ExpressRoute
* DevTest Labs – přesunout do nové skupiny prostředků v rámci stejného předplatného je povoleno, ale přesunutí křížové předplatného není povolená.
* Dynamics LCS
* Bitové kopie vytvořené z spravované disků
* Managed Disks
* Spravované aplikace
* Trezor služeb zotavení – také proveďte není přesunout prostředky výpočty, síť a úložiště přidružený k trezoru služeb zotavení, najdete v části [služeb zotavení omezení](#recovery-services-limitations).
* Zabezpečení
* Snímky vytvořené z spravované disky
* Správce zařízení StorSimple
* Virtuální počítače s spravované disky
* Najdete v části virtuální sítě (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)
* Virtuální počítače vytvořené z Marketplace prostředků – nelze přesunout ve předplatných. Prostředků musí být v aktuálním předplatném zrušit a znovu nasadit v rámci nového předplatného

## <a name="app-service-limitations"></a>Omezení služby App Service
Při práci s aplikacemi App Service, nemůžete přesunout pouze plán služby App Service. Chcete-li přesunout aplikací App Service, možnosti jsou:

* Přesuňte plán služby App Service a všechny ostatní prostředky služby App Service v příslušné skupině prostředků do nové skupiny prostředků, který ještě nemá prostředky služby App Service. Tento požadavek znamená, že je nutné přesunout i prostředky služby App Service, které nejsou přidružené plán služby App Service.
* Přesunutí aplikace do jiné skupině prostředků, ale ponechat všechny plány služby App Service v původní skupinu prostředků.

Plán služby App Service se nemusí být umístěné ve stejné skupině prostředků jako aplikace pro aplikaci správné fungování.

Například, pokud obsahuje vaší skupiny prostředků:

* **webové a** který je přidružen **plánu a**
* **Web-b** který je přidružen **plán b**

Možnosti jsou:

* Přesunout **webové a**, **plán a**, **web-b**, a **plán b**
* Přesunout **webové a** a **web-b**
* Přesunout **webové a**
* Přesunout **web-b**

Všechny ostatní kombinace zahrnovat ponechat za typ prostředku, který nemůže být ponecháno za při přesunu plán služby App Service (libovolný typ prostředku služby App Service).

Pokud vaše webová aplikace se nachází v jiné skupině prostředků než jeho plán služby App Service, ale chcete přesunout na novou skupinu prostředků, je nutné provést přesun ve dvou krocích. Například:

* **webové a** se nachází v **skupinu webových**
* **plán a** se nachází v **plán skupiny**
* Chcete, aby **webové a** a **plán a** být umístěné ve **kombinaci skupiny**

Aby bylo možné tento přesun, proveďte dvě samostatné přesunutí operace v tomto pořadí:

1. Přesunout **webové a** k **plán skupiny**
2. Přesunout **webové a** a **plán a** k **kombinaci skupiny**.

Certifikát služby aplikace můžete přesunout do nové skupiny prostředků nebo předplatného bez problémů. Pokud vaše webová aplikace obsahuje certifikát SSL, který jste zakoupili externě a nahrané do aplikace, můžete před přesunutím webové aplikace musíte odstranit certifikát. Například můžete provést následující kroky:

1. Odstranění se nahraný certifikát z webové aplikace
2. Přesunout webové aplikace
3. Nahrajte certifikát do webové aplikace

## <a name="recovery-services-limitations"></a>Omezení služby obnovení
Přesunutí není povolen pro úložiště, sítě, nebo výpočetní prostředky, které jsou používána k nastavení zotavení po havárii s Azure Site Recovery.

Předpokládejme například, jste nastavili replikaci počítačů na místě na účet úložiště (Storage1) a chcete chráněného počítače přijít po převzetí služeb při selhání do Azure jako virtuální počítač (VM1) připojených k virtuální síti (Network1). Některé z těchto prostředků Azure - Storage1, VM1 a Network1 - nelze přesunout skupiny prostředků v rámci stejného předplatného nebo pro odběry.

## <a name="hdinsight-limitations"></a>Omezení HDInsight

Clustery HDInsight se můžete přesunout do nové předplatné nebo skupinu prostředků. Však nelze přesouvat mezi odběry síťových prostředků propojené ke clusteru HDInsight (například virtuální sítě, síťové karty nebo nástroj pro vyrovnávání zatížení). Kromě toho nelze přesunout do nové skupiny prostředků síťový adaptér, který je připojen k virtuálnímu počítači pro cluster.

Při přesunu clusteru HDInsight na nové předplatné, nejprve přesunete jiné prostředky (například účet úložiště). HDInsight cluster, pak přesuňte samostatně.

## <a name="classic-deployment-limitations"></a>Omezení nasazení Classic
Možnosti pro přesun prostředků nasazené pomocí klasického modelu se liší v závislosti na tom, jestli jsou přesun prostředků v rámci předplatného nebo do nového předplatného.

### <a name="same-subscription"></a>Stejného předplatného.
Při přesouvání prostředků z jedné skupiny prostředků do jiné skupiny prostředků v rámci stejného předplatného, platí následující omezení:

* Nelze přesunout virtuální sítě (klasické).
* Virtuální počítače (klasické) je třeba přesunout s cloudovou službou.
* Cloudové služby se dají přesunout pouze při přesunutí zahrnuje všechny virtuální počítače.
* Současně lze přesunout jenom jeden cloudové služby.
* Současně lze přesouvat pouze jeden účet úložiště (klasické).
* Účet úložiště (klasické) nelze přesunout do stejné operace virtuálního počítače nebo cloudové služby.

K přesunutí klasické prostředky na novou skupinu prostředků v rámci stejného předplatného, použijte standardní Přesunutí operací prostřednictvím [portál](#use-portal), [prostředí Azure PowerShell](#use-powershell), [rozhraní příkazového řádku Azure](#use-azure-cli), nebo [rozhraní REST API](#use-rest-api). Můžete použít stejné operace jako použijte pro přesun prostředků Resource Manager.

### <a name="new-subscription"></a>Nové předplatné
Při přesunu prostředků do nového předplatného, platí následující omezení:

* Všechny klasické prostředky v předplatném je třeba přesunout v rámci jedné operace.
* Cílové předplatné nesmí obsahovat všechny klasické prostředky.
* Přesunutí může požadovat pouze přes samostatné REST API pro classic přesune. Standardní příkazy přesunutí Resource Manager nefungují při přesunu klasické prostředky na nové předplatné.

Chcete-li přesunout klasické prostředky do nového předplatného, použijte operace REST, které jsou specifické pro klasické prostředky. Abyste mohli použít REST, proveďte následující kroky:

1. Zkontrolujte, pokud zdrojové předplatné mohl účastnit přesunu mezi předplatnými. Použijte následující operace:

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     V textu požadavku patří:

  ```json
  {
    "role": "source"
  }
  ```

     Odpověď pro operace ověření je v následujícím formátu:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Zkontrolujte, pokud cílového odběru mohl účastnit přesunu mezi předplatnými. Použijte následující operace:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     V textu požadavku patří:

  ```json
  {
    "role": "target"
  }
  ```

     Odpověď je ve stejném formátu jako zdroj ověření předplatného.
3. Pokud oba odběry projít ověřením, přesune všechny klasické prostředky z jedno předplatné do jiného předplatného s následující operace:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    V textu požadavku patří:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

Operaci může běžet několik minut.

## <a name="use-portal"></a>Použít portál
Chcete-li přesunout prostředky, vyberte skupinu prostředků obsahující tyto prostředky a pak vyberte **přesunout** tlačítko.

![Přesunutí prostředků](./media/resource-group-move-resources/select-move.png)

Vyberte, jestli přecházíte prostředky na novou skupinu prostředků nebo nové předplatné.

Vyberte zdroje, které chcete přesunout a cílové skupiny prostředků. Na vědomí, že budete muset aktualizovat skripty pro tyto prostředky a vyberte **OK**. Pokud jste v předchozím kroku vybrali na ikonu pro úpravu předplatné, musíte také vybrat cílového odběru.

![Vyberte cíl](./media/resource-group-move-resources/select-destination.png)

V **oznámení**, uvidíte, že je spuštěna operaci přesunutí.

![Zobrazit stav přesunutí](./media/resource-group-move-resources/show-status.png)

Pokud ho dokončit, zobrazí se zpráva výsledku.

![Zobrazit přesunout výsledků](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Použití prostředí PowerShell
Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte `Move-AzureRmResource` příkaz.

V prvním příkladu ukazuje, jak přesunout jeden prostředek do nové skupiny prostředků.

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

Druhý příklad ukazuje, jak přesunout více prostředků do nové skupiny prostředků.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Pokud chcete přesunout do nového předplatného, obsahovat hodnotu pro `DestinationSubscriptionId` parametr.

Zobrazí se výzva k potvrzení, že chcete přesunout zadané prostředky.

```powershell
Confirm
Are you sure you want to move these resources to the resource group
'/subscriptions/{guid}/resourceGroups/newRG' the resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a>Použití Azure CLI 2.0
Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte `az resource move` příkaz. Zadejte ID prostředků pro přesun prostředků. Můžete získat ID prostředků pomocí následujícího příkazu:

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

Následující příklad ukazuje, jak přesunout do nové skupiny prostředků účtu úložiště. V `--ids` parametr, zadejte místo oddělený seznam ID pro přesun prostředků.

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

Chcete-li přesunout do nového předplatného, zadat `--destination-subscription-id` parametr.

## <a name="use-azure-cli-10"></a>Použití Azure CLI 1.0
Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, použijte `azure resource move` příkaz. Zadejte ID prostředků pro přesun prostředků. Můžete získat ID prostředků pomocí následujícího příkazu:

```azurecli
azure resource list -g sourceGroup --json
```

Která vrací v následujícím formátu:

```azurecli
[
  {
    "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
    "name": "storagedemo",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "southcentralus",
    "tags": {},
    "kind": "Storage",
    "sku": {
      "name": "Standard_RAGRS",
      "tier": "Standard"
    }
  }
]
```

Následující příklad ukazuje, jak přesunout do nové skupiny prostředků účtu úložiště. V `-i` parametr zadejte čárkami oddělený seznam ID pro přesun prostředků.

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

Zobrazí se výzva k potvrzení, že chcete přesunout zadaný prostředek.

## <a name="use-rest-api"></a>Použití rozhraní REST API
Chcete-li existující prostředky přesunout do jiné skupiny prostředků nebo předplatného, spusťte:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

V těle žádosti je zadat cílová skupina prostředků a prostředky, které chcete přesunout. Další informace o operaci REST přesunutí najdete v tématu [přesunout prostředky](https://msdn.microsoft.com/library/azure/mt218710.aspx).

## <a name="next-steps"></a>Další kroky
* Další informace o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](powershell-azure-resource-manager.md).
* Další informace o rozhraní příkazového řádku Azure pro správu předplatného najdete v tématu [pomocí rozhraní příkazového řádku Azure s Resource Managerem](xplat-cli-azure-resource-manager.md).
* Další informace o portálu funkce pro správu předplatného najdete v tématu [pomocí portálu Azure ke správě prostředků](resource-group-portal.md).
* Další informace o použití logického organizace pro vaše prostředky najdete v tématu [použití značek k uspořádání prostředků](resource-group-using-tags.md).
