---
title: uzly clusteru HPC Pack aaaAutoscale | Microsoft Docs
description: "Automaticky zvětšovat a zmenšovat hello počet výpočetních uzlů clusteru HPC Pack v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0bdf55625d337a2bbfe05677682d645a584798d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-grow-and-shrink-hello-hpc-pack-cluster-resources-in-azure-according-toohello-cluster-workload"></a>Automaticky zvětšovat a zmenšovat prostředky clusteru HPC Pack hello v Azure podle zatížení clusteru toohello
Pokud nasazujete Azure "shluků" uzly v clusteru HPC Pack nebo vytváření clusteru HPC Pack ve virtuálních počítačích Azure, můžete způsob, jak automaticky zvětšovat a zmenšovat hello prostředky clusteru jako uzly nebo jader podle hello zatížení v clusteru hello. Škálování prostředky clusteru hello tímto způsobem umožňuje toouse vašich prostředků Azure efektivněji a kontrolovat náklady.

Tento článek ukazuje dva způsoby, jak HPC Pack poskytuje tooautoscale výpočetní prostředky:

* Hello vlastnost clusteru HPC Pack **AutoGrowShrink**

* Hello **AzureAutoGrowShrink.ps1** skript prostředí PowerShell HPC

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Aktuálně lze pouze automaticky zvýšit nebo snížit HPC Pack výpočetní uzly, které je spuštěn operační systém Windows Server.


## <a name="set-hello-autogrowshrink-cluster-property"></a>Nastavte vlastnost clusteru AutoGrowShrink hello
### <a name="prerequisites"></a>Požadavky

* **HPC Pack 2012 R2 Update 2 nebo novější clusteru** -hello hlavního uzlu clusteru může být nasazena místně nebo ve virtuálním počítači Azure. V tématu [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget začít s hlavnímu uzlu místní a uzly Azure "shluků". V tématu hello [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) tooquickly nasazení clusteru HPC Pack ve virtuálních počítačích Azure.

* **Pro cluster s hlavního uzlu v Azure (modelu nasazení Resource Manager)** – od HPC Pack 2016, ověřování pomocí certifikátu v aplikaci Azure Active Directory se používá pro automaticky rostoucí a zmenšení clusteru virtuálních počítačů nasadit pomocí Azure Resource Manager. Nakonfigurujte certifikát takto:

  1. Po nasazení clusteru připojte pomocí vzdálené plochy tooone hlavního uzlu.

  2. Nahrát hlavního uzlu tooeach hello certifikát (ve formátu PFX pomocí soukromého klíče) a nainstalujte tooCert:\LocalMachine\My a Cert: \LocalMachine\Root.

  3. Otevřete prostředí Azure PowerShell jako správce a spusťte následující příkazy v jednom hlavního uzlu hello:

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    Pokud váš účet je ve více než jednoho klienta Azure Active Directory nebo předplatné Azure, můžete spustit následující hello příkaz tooselect hello správné klienta a předplatné:
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    Spusťte následující příkaz tooview hello hello aktuálně vybrané klienta a předplatné:
    
    ```powershell
        Get-AzureRMContext
    ```

  4. Spusťte následující skript hello

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    kde

    **DisplayName** -zobrazovaný název aplikace Azure aktivní. Pokud aplikace hello neexistuje, vytvoří se v Azure Active Directory.

    **Domovská stránka** -hello domovskou stránku hello aplikace. Můžete nakonfigurovat fiktivní adresu URL, jako v předchozím příkladu hello.

    **IdentifierUri** -identifikátor aplikace hello. Můžete nakonfigurovat fiktivní adresu URL, jako v předchozím příkladu hello.

    **CertificateThumbprint** -kryptografický otisk certifikátu hello jste nainstalovali hello hlavního uzlu v kroku 1.

    **TenantId** -ID služby Azure Active Directory klienta. Hello ID klienta můžete získat z portálu Azure Active Directory hello **vlastnosti** stránky.

    Další podrobnosti o **ConfigARMAutoGrowShrinkCert.ps1**spusťte `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.


* **Pro cluster s hlavního uzlu v Azure (modelu nasazení classic)** – Pokud používáte hello HPC Pack IaaS nasazení skriptu toocreate hello cluster v modelu nasazení classic hello, povolit hello **AutoGrowShrink** clusteru Vlastnost nastavením možnosti AutoGrowShrink hello v konfiguračním souboru na hello clusteru. Podrobnosti najdete v dokumentaci k hello doplňujícími hello [stahování skriptu](https://www.microsoft.com/download/details.aspx?id=44949).

    Můžete taky povolit hello **AutoGrowShrink** vlastnost clusteru poté, co nasadíte hello clusteru prostředí HPC PowerShell pomocí příkazů popsané v následující části hello. tooprepare pro tento první dokončení hello následující kroky:

  1. Nakonfigurujte certifikát pro správu Azure hello hlavního uzlu a v hello předplatného Azure. Pro testovací nasazení můžete použít výchozí Microsoft HPC Azure hello podepsaný svým držitelem se certifikát, který nainstaluje sady HPC Pack hello hlavního uzlu a pak nahrajte tento certifikát tooyour předplatného Azure. Možnosti a kroky najdete v tématu hello [pokyny v knihovně TechNet](https://technet.microsoft.com/library/gg481759.aspx).

  2. Spustit **regedit** hello hlavního uzlu, přejděte tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo a přidejte hodnotu řetězce. Nastavte název hodnoty hello příliš "Kryptografický otisk" a hodnota dat toohello kryptografický otisk certifikátu hello v kroku 1.

### <a name="hpc-powershell-commands-tooset-hello-autogrowshrink-property"></a>Prostředí HPC PowerShell příkazy tooset hello AutoGrowShrink vlastnost
Následují ukázkové prostředí HPC PowerShell příkazy tooset **AutoGrowShrink** a tootune své chování s další parametry. V tématu [AutoGrowShrink parametry](#AutoGrowShrink-parameters) dále v tomto článku hello úplný seznam nastavení.

toorun tyto příkazy, spusťte prostředí HPC PowerShell hello hlavního uzlu clusteru jako správce.

**hello tooenable AutoGrowShrink vlastnost**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

**hello toodisable AutoGrowShrink vlastnost**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

**toochange hello růst interval v minutách**

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

**toochange hello zmenšit interval v minutách**

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

**tooview hello aktuální konfiguraci AutoGrowShrink**

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

**Uzel skupin tooexclude z AutoGrowShrink**

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> Tento parametr je podporované počínaje HPC Pack 2016
>

### <a name="autogrowshrink-parameters"></a>Parametry AutoGrowShrink
Hello následují AutoGrowShrink parametry, které lze upravit pomocí hello **Set-HpcClusterProperty** příkaz.

* **EnableGrowShrink** – přepínač tooenable nebo zakázat hello **AutoGrowShrink** vlastnost.
* **ParamSweepTasksPerCore** -počet čištění parametrů úlohy toogrow jednoho jádra. Výchozí hodnota Hello je toogrow jednoho jádra daná úloha.

  > [!NOTE]
  > Změny HPC Pack QFE KB3134307 **ParamSweepTasksPerCore** příliš**TasksPerResourceUnit**. Ho je založený na typu prostředku hello úlohy a může být uzlu, soketu nebo jádra.
  >
  >
* **GrowThreshold** -prahovou hodnotu automatického zvětšování tootrigger zařazených do fronty úloh. Hello výchozí hodnota je 1, což znamená, že pokud neexistují 1 nebo více úloh v hello zařazených do fronty stavu, automaticky rozšiřovat uzly.
* **GrowInterval** – Interval v minutách tootrigger automatického zvětšování. Výchozí interval Hello je 5 minut.
* **ShrinkInterval** – Interval v minutách tootrigger automatické zmenšit. Výchozí interval Hello je 5 minut. |
* **ShrinkIdleTimes** -počet průběžné kontroly tooshrink tooindicate hello uzlů, které nejsou používána. Hello výchozí hodnota je 3 x. Například, pokud hello **ShrinkInterval** je 5 minut, HPC Pack zkontroluje, zda text hello uzel je nečinnosti každých 5 minut. Pokud hello uzly jsou ve stavu nečinnosti hello po 3 průběžné kontroluje (15 minut), HPC Pack zmenší tento uzel.
* **ExtraNodesGrowRatio** -další procento toogrow uzlů pro úlohy Message Passing Interface (MPI). Hello výchozí hodnota je 1, což znamená zvětšování HPC Pack uzlů pro úlohy MPI 1 %.
* **GrowByMin** – přepínač tooindicate, jestli zásady automatické zvětšování hello podle hello minimální prostředky potřebné pro úlohy hello. Hello výchozí hodnota je false, což znamená, že HPC Pack zvětšování uzlů pro úlohy podle hello maximální prostředky potřebné pro úlohy hello.
* **SoaJobGrowThreshold** -prahovou hodnotu příchozí SOA požadavky tootrigger hello automatické zvětšení procesu. Hello výchozí hodnota je 50000.

  > [!NOTE]
  > Tento parametr je podporované počínaje HPC Pack 2012 R2 Update 3.
  >
  >
* **SoaRequestsPerCore** -počet příchozích SOA požadavků toogrow jednoho jádra. Hello výchozí hodnota je 20000.

  > [!NOTE]
  > Tento parametr je podporované počínaje HPC Pack 2012 R2 Update 3.
  >
* **ExcludeNodeGroups** – uzel skupiny automaticky zvýšit nebo snížit zadány uzly v hello.
  
  > [!NOTE]
  > Tento parametr je podporované počínaje HPC Pack 2016.
  >

### <a name="mpi-example"></a>Příklad MPI
Ve výchozím nastavení HPC Pack zvětšování 1 % navíc uzlů pro úlohy MPI (**ExtraNodesGrowRatio** nastavena too1). Hello důvodem je, že MPI může vyžadovat více uzlů a hello úlohu lze spustit pouze když jsou všechny uzly připravené. Při spuštění uzlů Azure příležitostně jeden uzel může být nutné další čas toostart než jiné způsobuje ostatní uzly toobe nečinnosti při čekání na tento uzel tooget připraven. Způsobeného nárůstem další uzly, HPC Pack snižuje dobu čekání na tento prostředek a potenciálně šetří náklady. Procento hello tooincrease navíc uzlů pro úlohy MPI (například too10 %), spusťte příkaz podobný

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Příklad SOA
Ve výchozím nastavení **SoaJobGrowThreshold** nastavena too50000 a **SoaRequestsPerCore** too200000 nastavena. Pokud odešlete jeden úlohy architektury SOA s 70000 požadavky, je ve frontě úloh a 70000 jsou příchozí požadavky. V takovém případě HPC Pack zvětšování 1 jádro hello zařazených do fronty úloh a pro příchozí požadavky a zvětšování (70000-50000) nebo základní 20000 = 1, takže v celkem zvětšování 2 jádra pro tuto úlohu SOA.

## <a name="run-hello-azureautogrowshrinkps1-script"></a>Spusťte skript AzureAutoGrowShrink.ps1 hello
### <a name="prerequisites"></a>Požadavky

* **HPC Pack 2012 R2 Update 1 nebo novější clusteru** – hello **AzureAutoGrowShrink.ps1** skript je nainstalován do složky bin hello % CCP_HOME %. Hello hlavního uzlu clusteru může být nasazena místně nebo ve virtuálním počítači Azure. V tématu [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget začít s hlavnímu uzlu místní a uzly Azure "shluků". V tématu hello [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) tooquickly nasazení clusteru HPC Pack ve virtuálních počítačích Azure, nebo použijte [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).
* **Prostředí Azure PowerShell 1.4.0** -hello skriptu aktuálně závisí na konkrétní verzi prostředí Azure PowerShell.
* **Pro cluster s Azure burst uzly** – spusťte skript hello na klientském počítači, kde je nainstalován HPC Pack nebo hello hlavního uzlu. Pokud běží na klientském počítači, zajistěte, abyste nastavili hello proměnné $env: CCP_SCHEDULER toopoint toohello hlavního uzlu. uzly Azure "shluků" Hello, musí být přidaný toohello clusteru, ale může být v hello stavu nasazení není.
* **Pro cluster s podporou nasazené ve virtuálních počítačích Azure (modelu nasazení Resource Manager)** -pro cluster s podporou virtuálních počítačů Azure nasazené v modelu nasazení Resource Manager hello hello skriptu podporuje dvě metody pro ověřování Azure: Přihlaste tooyour účet Azure skript hello toorun pokaždé, když (spuštěním `Login-AzureRmAccount`, nebo nakonfigurovat hlavní tooauthenticate služby pomocí certifikátu. HPC Pack poskytuje hello skriptu **ConfigARMAutoGrowShrinkCert.ps** toocreate hlavní název služby pomocí certifikátu. Hello skript vytvoří aplikaci služby Azure Active Directory (Azure AD) a hlavní název služby a přiřadí hello Přispěvatel role toohello instanční objekt. toorun hello skriptu, otevřete prostředí Azure PowerShell jako správce a spusťte následující příkazy hello:

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    Další podrobnosti o **ConfigARMAutoGrowShrinkCert.ps1**spusťte `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,

* **Pro cluster s podporou nasazené ve virtuálních počítačích Azure (modelu nasazení classic)** – spusťte skript hello hello hlavního uzlu virtuálního počítače, protože závisí na hello **Start-HpcIaaSNode.ps1** a **Stop-HpcIaaSNode.ps1**skripty, které jsou nainstalovány existuje. Tyto skripty navíc vyžadují certifikát pro správu Azure nebo soubor nastavení publikování (viz [spravovat výpočetní uzly v prostředí HPC Pack clusteru v Azure](hpcpack-cluster-node-manage.md)). Zajistěte, aby všechny hello výpočetní uzel virtuální počítače, musíte již byly přidány toohello clusteru. Mohou být v zastaveném stavu hello.



### <a name="syntax"></a>Syntaxe
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a>Parametry
* **NodeTemplates** -názvů hello uzel šablony toodefine hello obor pro hello uzly toogrow a zmenšit. Není-li zadána (hello výchozí hodnota je @()), všechny uzly v hello **AzureNodes** uzlu skupiny jsou v oboru při **NodeType** má hodnotu AzureNodes a všech uzlech v hello **ComputeNodes** uzlu skupiny jsou v oboru při **NodeType** má hodnotu ComputeNodes.
* **JobTemplates** -názvy hello úlohy šablony toodefine hello obor pro uzly toogrow hello.
* **Typ uzlu** – hello typ uzlu toogrow a zmenšit. Podporované hodnoty jsou:

  * **AzureNodes** – u uzlů Azure PaaS (shluků) v místní nebo v clusteru Azure IaaS.
  * **ComputeNodes** – pro výpočetní uzel virtuální počítače pouze v clusteru služby Azure IaaS.

* **NumOfQueuedJobsPerNodeToGrow** -počet úloh zařazených do fronty vyžadovaných toogrow jeden uzel.
* **NumOfQueuedJobsToGrowThreshold** -hello prahová hodnota počtu úloh zařazených do fronty toostart hello růst procesu.
* **NumOfActiveQueuedTasksPerNodeToGrow** -hello počet aktivních úloh zařazených do fronty vyžadovaných toogrow jeden uzel. Pokud **NumOfQueuedJobsPerNodeToGrow** je zadán s hodnotou větší než 0, tento parametr je ignorován.
* **NumOfActiveQueuedTasksToGrowThreshold** -hello prahová hodnota počtu aktivních úloh zařazených do fronty toostart hello růst procesu.
* **NumOfInitialNodesToGrow** – hello počáteční minimální počet uzlů toogrow, pokud jsou všechny uzly hello v oboru **není nasazena** nebo **zastaveno (Deallocated)**.
* **GrowCheckIntervalMins** -hello interval v minutách mezi kontroluje toogrow.
* **ShrinkCheckIntervalMins** -hello interval v minutách mezi kontroluje tooshrink.
* **ShrinkCheckIdleTimes** -hello počtu kontrol průběžné zmenšení (oddělených **ShrinkCheckIntervalMins**) jsou uzly hello tooindicate nečinnosti.
* **UseLastConfigurations** -hello předchozích konfigurací, které jsou uloženy v souboru argument hello.
* **ArgFile**– hello název hello argument soubor použitý toosave a aktualizovat hello konfigurace toorun hello skriptu.
* **LogFilePrefix** -hello předpona názvu souboru protokolu hello. Můžete zadat cestu. Ve výchozím nastavení hello protokol je zapsaný toohello aktuální pracovní adresář.

### <a name="example-1"></a>Příklad 1
Hello následující příklad konfiguruje hello Azure burst uzly nasazené pomocí výchozí šablony AzureNode toogrow a zmenšit automaticky. Pokud jsou všechny uzly původně v hello **není nasazena** stavu, jsou spuštěny aspoň 3 uzly. Pokud hello počet úloh zařazených do fronty překračuje 8, hello skript spustí uzlů, dokud se jejich počet překračuje hello poměr zařazených do fronty úloh **NumOfQueuedJobsPerNodeToGrow**. Pokud se uzel najde toobe nečinnosti 3 po sobě jdoucích nečinnosti časů, je zastavena.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Příklad 2
Hello následující příklad konfiguruje hello Azure výpočetní uzel virtuální počítače nasazené pomocí výchozí šablony ComputeNode toogrow hello a zmenšit automaticky.
úlohy Hello nakonfigurovat pomocí šablony úlohy výchozí hello definuje rozsah hello zatížení v clusteru hello. Pokud jsou všechny uzly hello původně zastavena, nejméně 5 uzly jsou spuštěny. Pokud hello počet aktivních úloh zařazených do fronty překročí 15, hello skript spustí uzlů, dokud se jejich počet překračuje hello poměr aktivních úloh zařazených do fronty příliš**NumOfActiveQueuedTasksPerNodeToGrow**. Pokud se uzel najde toobe nečinnosti v 10krát po sobě jdoucích nečinnosti, je zastavena.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
