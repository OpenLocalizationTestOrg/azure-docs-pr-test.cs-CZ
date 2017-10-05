---
title: "Vytvořit cluster Service Fabric v Azure | Microsoft Docs"
description: "Zjistěte, jak vytvořit cluster Windows nebo Linux Service Fabric v Azure pomocí prostředí PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi
ms.openlocfilehash: f62249417552b9cf840bfac3b94c27f63bd7064e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a>Vytvoření clusteru s podporou zabezpečení v Azure pomocí prostředí PowerShell
V tomto kurzu se dozvíte, jak vytvořit cluster Service Fabric (Windows nebo Linux) běží v Azure. Jakmile budete hotovi, máte cluster se systémem, kterou můžete nasadit aplikace do cloudu.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí prostředí PowerShell
> * Zabezpečení clusteru společně s certifikátem X.509
> * Připojení ke clusteru pomocí prostředí PowerShell
> * Odebrat cluster

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte [modul Service Fabric SDK a prostředí PowerShell](service-fabric-get-started.md)
- Nainstalujte [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Následující postup vytvoří cluster Service Fabric náhled jeden uzel (jeden virtuální počítač). Cluster je zabezpečená službou certifikát podepsaný svým držitelem, který získá vytvořen společně s clusteru a pak umístit do trezoru klíčů. Clustery s jedním uzlem nelze škálovat nad rámec jednoho virtuálního počítače a clustery preview nelze upgradovat na novější verze.

Chcete-li vypočítat náklady způsobené spuštění clusteru Service Fabric v Azure pomocí [cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/).
Další informace o vytváření clusterů Service Fabric najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="create-the-cluster-using-azure-powershell"></a>Vytvoření clusteru pomocí Azure PowerShell
1. Stáhněte si místní kopii šablony Azure Resource Manageru a soubor parametrů z [šablony Azure Resource Manageru pro Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) úložiště GitHub.  *azuredeploy.JSON* je šablonu Azure Resource Manager, která definuje cluster Service Fabric. *azuredeploy.Parameters.JSON* je soubor parametrů můžete přizpůsobit nasazení clusteru.

2. Přizpůsobení následující parametry v *azuredeploy.parameters.json* soubor parametrů:

   | Parametr       | Popis | Navrhovaná hodnota |
   | --------------- | ----------- | --------------- |
   | clusterLocation | Oblast Azure do které chcete nasadit cluster. | *například westeurope, eastasia, eastus* |
   | Název clusteru     | Název clusteru, který chcete vytvořit. | *například honzuv sfpreviewcluster* |
   | adminUserName   | Účet místního správce na virtuální počítače clusteru. | *Všechny platné uživatelské jméno systému Windows Server* |
   | adminPassword   | Heslo účtu místního správce na virtuální počítače clusteru. | *Všechny platné heslo systému Windows Server* |
   | clusterCodeVersion | Verze Service Fabric ke spuštění. (255.255.X.255 jsou verze preview). | **255.255.5718.255** |
   | vmInstanceCount | Počet virtuálních počítačů v clusteru (může být 1 nebo 3-99). | **1** | *Pro cluster s podporou preview zadejte jenom jeden virtuální počítač* |

3. Otevřete konzolu prostředí PowerShell, přihlášení k Azure a vyberte předplatné, které chcete nasadit v clusteru:

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. Vytváření a šifrování heslo pro certifikát, který chcete používat Service Fabric.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. Vytvoření clusteru a svůj certifikát spuštěním následujícího příkazu:

   ```powershell
      New-AzureRmServiceFabricCluster
          -TemplateFile C:\Users\me\Desktop\azuredeploy.json `
          -ParameterFile C:\Users\me\Desktop\azuredeploy.parameters.json `
          -CertificateOutputFolder C:\Users\me\Desktop\ `
          -CertificatePassword $pwd `
          -CertificateSubjectName "mycluster.westeurope.cloudapp.azure.com" `
          -ResourceGroupName myclusterRG
   ```

   >[!NOTE]
   >`-CertificateSubjectName` Parametr by měl zarovnané s clusterName parametr zadaný v souboru parametrů, stejně jako domény vázaný na oblast Azure, které jste zvolili, jako například: `clustername.eastus.cloudapp.azure.com`.

Po dokončení konfigurace výstupy informace o clusteru vytvoří v Azure. Také zkopíruje certifikát clusteru k adresáři - CertificateOutputFolder na cestu, kterou jste zadali pro tento parametr. Je nutné tento certifikát pro přístup k Service Fabric Explorer a zobrazení stavu služby clusteru.

Poznamenejte si adresu URL pro váš cluster, který může být podobná následující adresu URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*

## <a name="modify-the-certificate--access-service-fabric-explorer"></a>Změnit certifikát & přístup Service Fabric Exploreru ##

1. Dvakrát klikněte na certifikát, který chcete otevřít Průvodce importem certifikátu.

2. Použít výchozí nastavení, ale nezapomeňte zaškrtnout **označit tento klíč jako exportovatelný.** Zaškrtněte políčko v **ochranu privátního klíče** krok. Visual Studio je potřeba exportovat certifikát při konfiguraci registru kontejner Azure k ověřování Service Fabric Cluster později v tomto kurzu.

3. Service Fabric Explorer nyní lze otevřít v prohlížeči. Chcete-li to provést, přejděte na **ManagementEndpoint** adresu URL pro váš cluster pomocí webového prohlížeče a vyberte certifikát, který byl uložen ve vašem počítači.

>[!NOTE]
>Při otevírání Service Fabric Explorer, zobrazí chyba certifikátu, jak používáte certifikát podepsaný svým držitelem. V Edge, budete muset kliknout na tlačítko *podrobnosti* a potom *přejděte na webovou stránku* odkaz. V prohlížeči Chrome, budete muset kliknout na tlačítko *Upřesnit* a potom *pokračovat* odkaz.

>[!NOTE]
>Pokud vytvoření clusteru selže, je vždy znovu spustit příkaz, který aktualizuje již nasazené prostředky. Pokud certifikát byl vytvořen jako součást neúspěšné nasazení, vygeneruje se nový. K vyřešení vytváření clusteru najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="connect-to-the-secure-cluster"></a>Připojte se ke zabezpečení clusteru
Připojte ke clusteru pomocí Service Fabric prostředí PowerShell modul nainstalovaný pomocí Service Fabric SDK.  Nejprve nainstalujte certifikát do úložiště osobních (My) aktuálního uživatele ve vašem počítači.  Spusťte následující příkaz prostředí PowerShell:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Nyní jste připraveni připojit se ke svému zabezpečenému clusteru.

**Service Fabric** modulu PowerShell poskytuje mnoho rutiny pro správu clusterů Service Fabric, aplikace a služby.  Použití [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) pro připojení k zabezpečení clusteru. Podrobnosti koncový bod připojení a kryptografický otisk certifikátu se nacházejí ve výstupu z předchozího kroku.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Zkontrolujte, zda jste připojeni, a je v pořádku clusteru pomocí [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) rutiny.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Cluster se kromě vlastního prostředku clusteru skládá z dalších prostředků Azure. Nejjednodušší způsob, jak odstranit cluster a všechny prostředky, které využívá, je odstranit příslušnou skupinu prostředků.

Přihlaste se k Azure a vybrat ID předplatného, pro který chcete odebrat cluster.  Můžete najít svoje ID předplatného přihlášením k [portál Azure](http://portal.azure.com). Odstranit skupinu prostředků a všechny prostředky clusteru pomocí [rutinu Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId "Subcription ID"

$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvořit cluster Service Fabric v Azure
> * Zabezpečení clusteru společně s certifikátem X.509
> * Připojení ke clusteru pomocí prostředí PowerShell
> * Odebrat cluster

V dalším kroku přechodu na následující kurzu se dozvíte, jak nasadit existující aplikaci.
> [!div class="nextstepaction"]
> [Nasazení aplikace .NET existující pomocí Docker Compose](service-fabric-host-app-in-a-container.md)
