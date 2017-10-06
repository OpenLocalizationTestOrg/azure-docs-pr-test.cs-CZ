---
title: "aaaMove prostředky Azure toonew předplatné nebo prostředek skupiny | Microsoft Docs"
description: "Pomocí Azure Resource Manager toomove prostředky tooa novou skupinu prostředků nebo předplatného."
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
ms.openlocfilehash: 09d35f0afbbcdc0c66779f98a982d878f0807497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="move-resources-toonew-resource-group-or-subscription"></a>Přesunout skupiny prostředků toonew prostředků nebo předplatného
Toto téma ukazuje, jak toomove prostředky tooeither nové předplatné nebo nový prostředek skupiny v hello stejného předplatného. Můžete použít hello portálu, prostředí PowerShell, rozhraní příkazového řádku Azure nebo hello prostředků toomove REST API. operace přesunutí Hello v tomto tématu jsou k dispozici tooyou bez jakékoli požádat o pomoc podporu Azure.

Při přesouvání prostředků, zdrojová skupina hello i hello cílové skupiny jsou zamčené během operace hello. Zápis a operace odstranění jsou zablokované o skupinách prostředků hello až po dokončení přesunu hello. Tato Zámek znamená nelze přidat, aktualizovat nebo odstranit prostředky v hello skupiny prostředků, ale neznamená, že zmrazené hello prostředky. Například pokud přesunete systému SQL Server a její databáze tooa novou skupinu prostředků, aplikace, která používá databázi hello vyskytne nedojde k žádnému výpadku. Stále můžete číst a zapisovat toohello databáze.

Nelze změnit umístění hello hello prostředku. Přesunutí prostředku pouze přesune ho tooa novou skupinu prostředků. Hello novou skupinu prostředků může mít jiné umístění, ale který nezmění umístění hello hello prostředku.

> [!NOTE]
> Tento článek popisuje, jak účet nabízení toomove prostředky v rámci existující Azure. Pokud chcete ve skutečnosti toochange účtu Azure nabídky (například upgrade z průběžnými platbami toopre platím) přitom dál toowork se stávajícími prostředky, najdete v [přepínač vaši nabídku tooanother předplatné](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Kontrolní seznam před přesunutím prostředků
Existují některé důležité kroky tooperform před přesunutím prostředku. Ověřením těchto podmínek se můžete vyhnout chybám.

1. Hello zdrojové a cílové odběry, musí existovat v rámci hello stejné [klienta Azure Active Directory](../active-directory/active-directory-howto-tenant.md). toocheck oba odběry mají hello stejné ID klienta, použijte prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure.

  Pro Azure PowerShell použijte:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  Azure CLI 2.0 použijte:

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  Pokud ID klienta hello nejsou hello zdrojové a cílové předplatné hello stejné, můžete se pokusit toochange hello adresáře pro předplatné hello. Tato možnost je však pouze k dispozici tooService správci, kteří jsou podepsané pomocí účtu Microsoft (není účet organizace). Změna hello adresáře, přihlášení toohello tooattempt [portálu classic](https://manage.windowsazure.com/)a vyberte **nastavení**a vyberte předplatné hello. Pokud hello **upravit adresář** ikonu je k dispozici, vyberte ji toochange hello přidružené Azure Active Directory.

  ![Upravit adresář](./media/resource-group-move-resources/edit-directory.png)

  Pokud tuto ikonu není k dispozici, obraťte se na podporu toomove hello prostředky tooa nového klienta.

2. Hello služby musíte povolit hello možnost toomove prostředky. Toto téma uvádí služby, které Povolit přesunutí prostředků a služby, které nepovolíte přesunutí prostředků.
3. Hello cílového odběru musí být zaregistrován pro poskytovatele prostředků hello přesouvání prostředku hello. Pokud ne, se zobrazí chyba s oznámením, že hello **předplatné není zaregistrované pro typ prostředku**. Tento problém může dojít při přesouvání prostředků tooa nové předplatné, ale toto předplatné dosud nebyl použit s tohoto typu prostředku. toolearn způsobu toocheck hello stav registrace a registraci zprostředkovatele prostředků najdete v části [zprostředkovatelé prostředků a typy](resource-manager-supported-services.md).

## <a name="when-toocall-support"></a>Když toocall podporovat
Většina prostředkům prostřednictvím operace hello samoobslužné služby zobrazí v tomto tématu se můžete přesunout. Použijte na operací hello samoobslužné služby:

* Přesouvání prostředků Resource Manageru
* Přesunout klasické prostředky podle toohello [omezení nasazení classic](#classic-deployment-limitations).

Až budete potřebovat, obraťte se na podporu:

* Přesunete prostředky tooa nový účet Azure (a klienta Azure Active Directory).
* Přesunout klasické prostředky, ale máte potíže se omezeních hello.

## <a name="services-that-enable-move"></a>Služby, které umožňují přesunout
Prozatím se hello služeb, které umožňují přesunutí tooboth novou skupinu prostředků a předplatného jsou:

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
* Databáze systému SQL server - hello databázi a server se musí nacházet v hello stejnou skupinu prostředků. Když přesouváte systému SQL server, budou přesunuty také všechny její databáze.
* Traffic Manager
* Virtuální počítače
* Virtuální počítače s certifikát uložený v Key Vault - přesun toonew prostředků skupiny v rámci stejného předplatného je povoleno, ale přesun křížové předplatného není povolená.
* Virtuální počítače (klasické) - najdete v části [omezení nasazení Classic](#classic-deployment-limitations)
* Škálovací sady virtuálních počítačů
* Virtuální sítě - aktuálně, peered virtuální sítě nelze přesunout, dokud partnerský vztah virtuální sítě je zakázané. Jakmile zakázaná, hello virtuální sítě možné úspěšně přesunout a lze je povolit partnerský vztah hello virtuální sítě. Kromě toho virtuální sítě nesmí být přesunutý tooa jiného předplatného, pokud hello virtuální síť obsahuje všechny podsítě s odkazy na zdroje navigace. Například podsíť virtuální sítě nemá navigační odkaz na prostředek, pokud prostředek redis Microsoft.Cache je nasazený do dané podsítě.
* VPN Gateway


## <a name="services-that-do-not-enable-move"></a>Služby, které nepovolujte přesunutí
Hello služby, které aktuálně nepovolujte přesunutí prostředku jsou:

* Služba AD Domain Services
* Hybridní AD Health Service
* Application Gateway
* S virtuálními počítači s disky spravovat sady dostupnosti
* BizTalk Services
* Container Service
* ExpressRoute
* DevTest Labs – přesun toonew prostředků skupiny v rámci stejného předplatného je povoleno, ale mezi přesun předplatného není povolená.
* Dynamics LCS
* Bitové kopie vytvořené z spravované disků
* Managed Disks
* Spravované aplikace
* Trezor služeb zotavení – také provést není přesunutí hello výpočty, síť a úložiště prostředky přidružené služeb zotavení hello trezoru, najdete v části [služeb zotavení omezení](#recovery-services-limitations).
* Zabezpečení
* Snímky vytvořené z spravované disky
* Správce zařízení StorSimple
* Virtuální počítače s spravované disky
* Najdete v části virtuální sítě (klasické) - [omezení nasazení Classic](#classic-deployment-limitations)
* Virtuální počítače vytvořené z Marketplace prostředků – nelze přesunout ve předplatných. Prostředek musí toobe zrušit v aktuálním předplatném hello a znovu nasadit v nové předplatné hello

## <a name="app-service-limitations"></a>Omezení služby App Service
Při práci s aplikacemi App Service, nemůžete přesunout pouze plán služby App Service. aplikace služby App Service toomove, možnosti jsou:

* V této prostředků skupiny tooa novou skupinu prostředků, prostředků služby App Service již nemá přesuňte hello plán služby App Service a všechny ostatní prostředky služby App Service. Tento požadavek, že znamená, je nutné přesunout i hello prostředky služby App Service, nejsou přidružené hello plán služby App Service.
* Přesunout hello aplikace tooa jiné skupině prostředků, ale ponechat všechny plány služby App Service ve skupině prostředků původní hello.

Hello plán nemusí tooreside v App Service hello stejné skupině prostředků jako hello aplikace pro toofunction aplikace hello správně.

Například, pokud obsahuje vaší skupiny prostředků:

* **webové a** který je přidružen **plánu a**
* **Web-b** který je přidružen **plán b**

Možnosti jsou:

* Přesunout **webové a**, **plán a**, **web-b**, a **plán b**
* Přesunout **webové a** a **web-b**
* Přesunout **webové a**
* Přesunout **web-b**

Všechny ostatní kombinace zahrnovat ponechat za typ prostředku, který nemůže být ponecháno za při přesunu plán služby App Service (libovolný typ prostředku služby App Service).

Pokud vaše webová aplikace se nachází v jiné skupině prostředků než jeho plán služby App Service, ale chcete toomove novou skupinu prostředků obou tooa, je nutné provést přesun hello ve dvou krocích. Například:

* **webové a** se nachází v **skupinu webových**
* **plán a** se nachází v **plán skupiny**
* Chcete, aby **webové a** a **plán a** tooreside v **kombinaci skupiny**

tooaccomplish to přesunout, proveďte dvě samostatné přesunutí operace v hello následující pořadí:

1. Přesunout hello **webové a** příliš**plán skupiny**
2. Přesunout **webové a** a **plán a** příliš**kombinaci skupiny**.

Můžete přesunout certifikát služby aplikace tooa novou skupinu prostředků nebo předplatného bez problémů. Ale pokud vaše webová aplikace obsahuje certifikát SSL zakoupili externě a nahrát aplikaci toohello, musíte odstranit certifikát hello před přesunutí hello webové aplikace. Můžete například provést hello následující kroky:

1. Odstranit hello nahrát certifikát z hello webové aplikace
2. Přesunutí hello webové aplikace
3. Nahrání hello certifikát toohello webové aplikace

## <a name="recovery-services-limitations"></a>Omezení služby obnovení
Přesunutí není povoleno pro úložiště, sítě, nebo výpočetní prostředky používat tooset až zotavení po havárii s Azure Site Recovery.

Například předpokládejme, že jste nastavili replikaci účtu místního počítače tooa úložiště (Storage1) a chcete hello chráněný počítač toocome až po převzetí služeb při selhání tooAzure jako virtuální počítač (VM1) připojen tooa virtuální sítě (Network1). Nelze přesunout některé z těchto prostředků Azure - Storage1, VM1, a Network1 - napříč prostředků skupiny v rámci hello stejné předplatné nebo ve předplatných.

## <a name="hdinsight-limitations"></a>Omezení HDInsight

Můžete přesunout HDInsight clustery tooa nové předplatné nebo skupinu prostředků. Však nelze přesouvat mezi odběry hello sítě clusteru HDInsight toohello propojené prostředky (například hello virtuální sítě, síťové karty nebo nástroj pro vyrovnávání zatížení). Kromě toho nelze přesunout tooa novou skupinu prostředků síťovou kartu, která je připojená tooa virtuálního počítače pro hello cluster.

Při přesunu tooa clusteru HDInsight pro nové předplatné, nejprve přesunete jiné prostředky (například účet úložiště hello). Pak přesuňte clusteru HDInsight hello samostatně.

## <a name="classic-deployment-limitations"></a>Omezení nasazení Classic
Hello možnosti pro přesun prostředků nasazené pomocí klasického modelu hello se liší v závislosti na tom, jestli se přesun prostředků hello v rámci předplatného nebo tooa nové předplatné.

### <a name="same-subscription"></a>Stejného předplatného.
Při přesunu prostředků z jedné skupiny tooanother prostředků skupiny prostředků v rámci hello použít stejného předplatného, hello následující omezení:

* Nelze přesunout virtuální sítě (klasické).
* Virtuální počítače (klasické) je třeba přesunout s cloudovou službou hello.
* Cloudové služby se dají přesunout pouze při přesunutí hello zahrnuje všechny virtuální počítače.
* Současně lze přesunout jenom jeden cloudové služby.
* Současně lze přesouvat pouze jeden účet úložiště (klasické).
* Účet úložiště (klasické) nelze přesunout v hello stejné operace virtuálního počítače nebo cloudové služby.

toomove klasické prostředky tooa novou skupinu prostředků v hello stejného předplatného, použijte standardní Přesunutí operací hello prostřednictvím hello [portál](#use-portal), [prostředí Azure PowerShell](#use-powershell), [rozhraní příkazového řádku Azure](#use-azure-cli), nebo [rozhraní REST API](#use-rest-api). Používáte hello stejné operace jako použijte pro přesun prostředků Resource Manager.

### <a name="new-subscription"></a>Nové předplatné
Při přesunu prostředků tooa nové předplatné, platí následující omezení hello:

* Všechny klasické prostředky v předplatném hello je třeba přesunout v hello stejné operace.
* cílové předplatné Hello nesmí obsahovat všechny klasické prostředky.
* Přesunutí Hello mohou být požadována pouze přes samostatné REST API pro přesun classic. Hello standardní Resource Manager přesunutí příkazy nefungují při přesunu klasické prostředky tooa nové předplatné.

toomove klasické prostředky tooa nové předplatné, použijte hello REST operace, které jsou specifické tooclassic prostředky. toouse REST, proveďte následující kroky hello:

1. Kontrola, pokud hello zdrojové předplatné mohl účastnit mezi předplatnými přesunout. Použijte hello následující operace:

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     V textu žádosti hello patří:

  ```json
  {
    "role": "source"
  }
  ```

     Hello odpověď pro operace ověření hello je ve formátu hello:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Kontrola, pokud hello cílového odběru mohou být součástí mezi předplatnými přesunout. Použijte hello následující operace:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     V textu žádosti hello patří:

  ```json
  {
    "role": "target"
  }
  ```

     odpověď Hello je v hello stejný formát jako hello zdrojové předplatné ověření.
3. Pokud oba odběry projít ověřením, přesune všechny klasické prostředky z předplatného tooanother jedno předplatné s hello následující operace:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    V textu žádosti hello patří:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

operace Hello může běžet několik minut.

## <a name="use-portal"></a>Použít portál
toomove prostředky, vyberte skupinu prostředků hello obsahující tyto prostředky a potom vyberte hello **přesunout** tlačítko.

![Přesunutí prostředků](./media/resource-group-move-resources/select-move.png)

Vyberte, zda jsou přesunutí hello prostředky tooa novou skupinu prostředků nebo nové předplatné.

Vyberte toomove hello prostředků a skupina prostředků cílového hello. Na vědomí, že potřebujete tooupdate skripty pro tyto prostředky a vyberte **OK**. Pokud předplatné ikonu pro úpravu hello jste vybrali v předchozím kroku hello, je nutné vybrat také hello cílového předplatného.

![Vyberte cíl](./media/resource-group-move-resources/select-destination.png)

V **oznámení**, uvidíte, že hello přesunout probíhá operace.

![Zobrazit stav přesunutí](./media/resource-group-move-resources/show-status.png)

Pokud ho dokončit, zobrazí se zpráva výsledku hello.

![Zobrazit přesunout výsledků](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Použití prostředí PowerShell
toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello `Move-AzureRmResource` příkaz.

Hello první příklad ukazuje způsob toomove jeden prostředek tooa novou skupinu prostředků.

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

Hello sekundu příklad ukazuje, jak toomove více prostředků tooa novou skupinu prostředků.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

toomove tooa nové předplatné, vložte hodnotu pro hello `DestinationSubscriptionId` parametr.

Jste vyzváni, které chcete toomove hello tooconfirm zadané prostředky.

```powershell
Confirm
Are you sure you want toomove these resources toohello resource group
'/subscriptions/{guid}/resourceGroups/newRG' hello resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a>Použití Azure CLI 2.0
toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello `az resource move` příkaz. Zadejte hello prostředků ID toomove prostředky hello. ID prostředku můžete získat pomocí hello následující příkaz:

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

Hello následující příklad ukazuje, jak toomove úložiště účet tooa novou skupinu prostředků. V hello `--ids` parametr, poskytovat seznam toomove ID prostředku hello oddělených mezerami.

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

toomove tooa nové předplatné, zadejte hello `--destination-subscription-id` parametr.

## <a name="use-azure-cli-10"></a>Použití Azure CLI 1.0
toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello `azure resource move` příkaz. Zadejte hello prostředků ID toomove prostředky hello. ID prostředku můžete získat pomocí hello následující příkaz:

```azurecli
azure resource list -g sourceGroup --json
```

Která vrací hello následující formát:

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

Hello následující příklad ukazuje, jak toomove úložiště účet tooa novou skupinu prostředků. V hello `-i` parametr, poskytovat seznam toomove ID prostředku hello oddělených čárkami.

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

Jste vyzváni, které chcete toomove hello tooconfirm zadaný prostředek.

## <a name="use-rest-api"></a>Použití rozhraní REST API
toomove existující prostředky tooanother skupinu prostředků nebo předplatného, spusťte:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

V textu žádosti hello zadejte hello cílová skupina prostředků a prostředky toomove hello. Další informace o operaci REST přesunutí hello najdete v tématu [přesunout prostředky](https://msdn.microsoft.com/library/azure/mt218710.aspx).

## <a name="next-steps"></a>Další kroky
* toolearn o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](powershell-azure-resource-manager.md).
* toolearn o rozhraní příkazového řádku Azure pro správu předplatného, najdete v části [hello pomocí rozhraní příkazového řádku Azure s Resource Managerem](xplat-cli-azure-resource-manager.md).
* toolearn o funkcích portálu pro správu předplatného, najdete v části [pomocí prostředků Azure portálu toomanage hello](resource-group-portal.md).
* toolearn o použití tooyour prostředky logické organizaci, najdete v části [pomocí značky tooorganize vaše prostředky](resource-group-using-tags.md).
