---
title: aaaCreate Service Fabric cluster v Azure | Microsoft Docs
description: "Zjistěte, jak toocreate Windows nebo Linux Service Fabric cluster v Azure pomocí prostředí PowerShell."
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
ms.openlocfilehash: e697e2a2e99f67cb02422efdb368c664c9fd5a97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-secure-cluster-on-azure-using-powershell"></a>Vytvoření clusteru s podporou zabezpečení v Azure pomocí prostředí PowerShell
Tento kurz ukazuje, jak toocreate Service Fabric clusteru (Windows nebo Linux) spuštěná v Azure. Jakmile budete hotovi, máte cluster se systémem hello cloudu, který můžete nasadit aplikace do.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření zabezpečení clusteru Service Fabric v Azure pomocí prostředí PowerShell
> * Zabezpečení clusteru hello společně s certifikátem X.509
> * Připojte toohello cluster pomocí prostředí PowerShell
> * Odebrat cluster

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu:
- Pokud nemáte předplatné Azure, vytvořte [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Nainstalujte hello [modul Service Fabric SDK a prostředí PowerShell](service-fabric-get-started.md)
- Nainstalujte hello [prostředí Azure Powershell verze modulu 4.1 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)

Hello následující postup vytvoří cluster Service Fabric náhled jeden uzel (jeden virtuální počítač). Hello clusteru je zabezpečená službou certifikát podepsaný svým držitelem, který získá vytvořen společně s hello clusteru a pak umístit do trezoru klíčů. Clustery s jedním uzlem nelze škálovat nad rámec jednoho virtuálního počítače a clustery preview nemůže být toonewer upgradovaná verze.

toocalculate náklady způsobené spuštění clusteru Service Fabric v Azure pomocí hello [cenové kalkulačce pro Azure](https://azure.microsoft.com/pricing/calculator/).
Další informace o vytváření clusterů Service Fabric najdete v tématu [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="create-hello-cluster-using-azure-powershell"></a>Vytvoření clusteru hello pomocí Azure PowerShell
1. Stáhněte si místní kopii šablony Azure Resource Manageru hello a soubor parametrů hello z hello [šablony Azure Resource Manageru pro Service Fabric](https://aka.ms/securepreviewonelineclustertemplate) úložiště GitHub.  *azuredeploy.JSON* je hello šablony Azure Resource Manageru, která definuje cluster Service Fabric. *azuredeploy.Parameters.JSON* je soubor parametrů pro vás nasazení clusteru toocustomize hello.

2. Přizpůsobení hello následující parametry v hello *azuredeploy.parameters.json* soubor parametrů:

   | Parametr       | Popis | Navrhovaná hodnota |
   | --------------- | ----------- | --------------- |
   | clusterLocation | Hello oblast Azure toowhich toodeploy hello clusteru. | *například westeurope, eastasia, eastus* |
   | Název clusteru     | Název clusteru hello chcete toocreate. | *například honzuv sfpreviewcluster* |
   | adminUserName   | Hello účet místního správce na hello clusteru virtuálních počítačů. | *Všechny platné uživatelské jméno systému Windows Server* |
   | adminPassword   | Heslo účtu místního správce hello na hello clusteru virtuálních počítačů. | *Všechny platné heslo systému Windows Server* |
   | clusterCodeVersion | toorun verze Service Fabric Hello. (255.255.X.255 jsou verze preview). | **255.255.5718.255** |
   | vmInstanceCount | Hello počet virtuálních počítačů v clusteru (může být 1 nebo 3-99). | **1** | *Pro cluster s podporou preview zadejte jenom jeden virtuální počítač* |

3. Otevřete konzolu prostředí PowerShell, tooAzure přihlášení a vyberte hello odběr, který chcete v clusteru toodeploy hello:

   ```powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription -SubscriptionId <subscription-id>
   ```
4. Vytváření a šifrování heslo pro toobe certifikát hello používá Service Fabric.

   ```powershell
   $pwd = "<your password>" | ConvertTo-SecureString -AsPlainText -Force
   ```
5. Vytvoření clusteru hello a svůj certifikát spuštěním hello následující příkaz:

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
   >Hello `-CertificateSubjectName` parametr by měl zarovnané s hello clusterName parametr zadaný v souboru parametrů hello, stejně jako hello domény svázané toohello oblast Azure jste zvolili, jako například: `clustername.eastus.cloudapp.azure.com`.

Po dokončení konfigurace hello výstupy informace o clusteru hello vytvoří v Azure. Také zkopíruje hello clusteru certifikát toohello - CertificateOutputFolder adresáře na hello cesta, kterou jste zadali pro tento parametr. Je nutné tento certifikát tooaccess Service Fabric Explorer a zobrazení stavu hello clusteru.

Poznamenejte si adresu URL hello pro váš cluster, který může být podobné toohello následující adresu URL: *https://mycluster.westeurope.cloudapp.azure.com:19080*

## <a name="modify-hello-certificate--access-service-fabric-explorer"></a>Upravit certifikát pro hello & přístup Service Fabric Exploreru ##

1. Dvakrát klikněte na hello certifikát tooopen hello Průvodce importem certifikátu.

2. Použít výchozí nastavení, ale ujistěte se, zda text hello toocheck **označit tento klíč jako exportovatelný.** Zaškrtněte políčko v hello **ochranu privátního klíče** krok. Visual Studio musí certifikát hello tooexport při konfiguraci ověřování clusteru infrastruktury Azure kontejneru registru tooService později v tomto kurzu.

3. Service Fabric Explorer nyní lze otevřít v prohlížeči. toodo tedy přejděte toohello **ManagementEndpoint** adresu URL pro váš cluster pomocí webového prohlížeče a vyberte hello certifikát, který byl uložen ve vašem počítači.

>[!NOTE]
>Při otevírání Service Fabric Explorer, zobrazí chyba certifikátu, jak používáte certifikát podepsaný svým držitelem. V Edgi máte tooclick *podrobnosti* a pak hello *přejděte na webové stránce toohello* odkaz. V prohlížeči Chrome, máte tooclick *Upřesnit* a pak hello *pokračovat* odkaz.

>[!NOTE]
>Pokud hello vytváření clusteru selže, může vždy znovu hello příkaz, který aktualizuje již nasazené prostředky hello. Pokud certifikát byl vytvořen jako součást nasazení hello se nezdařilo, vygeneruje se nový. Vytvoření clusteru tootroubleshoot, najdete v části [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).

## <a name="connect-toohello-secure-cluster"></a>Připojte toohello zabezpečené cluster
Připojte toohello clusteru pomocí modulu Service Fabric prostředí PowerShell hello nainstalované s hello Service Fabric SDK.  Nejprve nainstalujte hello certifikát do úložiště osobních (My) hello hello aktuální uživatel ve vašem počítači.  Spusťte následující příkaz prostředí PowerShell hello:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\mysfcluster20170531104310.pfx `
        -Password $certpwd
```

Nyní je připraven tooconnect tooyour zabezpečení clusteru.

Hello **Service Fabric** modulu PowerShell poskytuje mnoho rutiny pro správu clusterů Service Fabric, aplikace a služby.  Použití hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) rutiny tooconnect toohello zabezpečení clusteru. Hello kryptografický otisk certifikátu a podrobnosti koncový bod připojení se nacházejí v hello výstup z předchozího kroku.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Zkontrolujte, zda jste připojeni a hello clusteru je v pořádku, pomocí hello [Get-ServiceFabricClusterHealth](/powershell/module/servicefabric/get-servicefabricclusterhealth) rutiny.

```powershell
Get-ServiceFabricClusterHealth
```

## <a name="clean-up-resources"></a>Vyčištění prostředků

Cluster se skládá z jiných prostředků Azure kromě toohello prostředek clusteru, sám sebe. Hello nejjednodušší způsob, jak toodelete hello clusteru a všechny prostředky hello, které budou využívat je skupina prostředků toodelete hello.

Přihlaste se tooAzure a vybrat ID předplatného hello, pro který chcete tooremove hello clusteru.  Můžete najít svoje ID předplatného přihlášením toohello [portál Azure](http://portal.azure.com). Odstranit skupinu prostředků hello a všechny prostředky clusteru hello pomocí hello [rutinu Remove-AzureRMResourceGroup](/en-us/powershell/module/azurerm.resources/remove-azurermresourcegroup).

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
> * Zabezpečení clusteru hello společně s certifikátem X.509
> * Připojte toohello cluster pomocí prostředí PowerShell
> * Odebrat cluster

V dalším kroku zálohy toohello následující kurz toolearn jak toodeploy stávající aplikace.
> [!div class="nextstepaction"]
> [Nasazení aplikace .NET existující pomocí Docker Compose](service-fabric-host-app-in-a-container.md)
