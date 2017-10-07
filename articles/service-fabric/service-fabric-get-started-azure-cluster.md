---
title: "aaaSet do clusteru služby Azure Service Fabric | Microsoft Docs"
description: "Rychlé zprovoznění – vytvoření clusteru Service Fabric s Windows nebo Linuxem v Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 13c60e293d19d607bb41ee4859706508c219a833
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Vytvoření vašeho prvního clusteru Service Fabric v Azure
[Cluster Service Fabric](service-fabric-deploy-anywhere.md) je síťově propojená sada virtuálních nebo fyzických počítačů, ve které se nasazují a spravují mikroslužby. Tento rychlý start vám pomůže toocreate pěti uzly clusteru, se systémem Windows nebo Linux, prostřednictvím hello [prostředí Azure PowerShell](https://msdn.microsoft.com/library/dn135248) nebo [portál Azure](http://portal.azure.com) za několik minut.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.


## <a name="use-hello-azure-portal"></a>Hello použití portálu Azure

Přihlášení toohello portál Azure na [http://portal.azure.com](http://portal.azure.com).

### <a name="create-hello-cluster"></a>Vytvoření clusteru hello

1. Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.
2. Vyberte **výpočetní** z hello **nový** okna a potom vyberte **Service Fabric Cluster** z hello **výpočetní** okno.
3. Vyplňte hello Service Fabric **Základy** formuláře. Pro **operačního systému**vyberte hello verzi systému Windows nebo Linux, které chcete hello toorun uzly clusteru. Hello uživatelské jméno a heslo zadané v tomto poli je použité toolog toohello virtuálním počítači. V části **Skupina prostředků** vytvořte novou. Skupina prostředků je logický kontejner, ve kterém se vytváří a hromadně spravují prostředky Azure. Jakmile budete hotovi, klikněte na **OK**.

    ![Výstup po instalaci clusteru][cluster-setup-basics]

4. Vyplňte hello **konfigurace clusteru** formuláře.  Jako **Počet typů uzlu** zadejte 1.

5. Vyberte **typ uzlu 1 (primární)** a vyplňte hello **konfiguraci typu uzlu** formuláře.  Zadejte název typu uzlu a nastavte hello [odolnost vrstvy](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) příliš "Bronzovým."  Vyberte velikost virtuálního počítače.

    Typy uzlů definovat hello velikost virtuálního počítače, počet virtuálních počítačů, vlastní koncové body, a další nastavení pro hello virtuálních počítačů daného typu. Každý typ uzlu definována je nastavený jako sada škálování samostatný virtuální počítač, který je použité toodeploy a spravovaných virtuálních počítačích jako sada. U každého typu uzlu je možné nezávisle vertikálně navýšit nebo snížit kapacitu. Mají různé sady otevřených portů a můžou mít různé metriky kapacity.  Hello první nebo primární, typ uzlu Určuje, kde jsou hostované Service Fabric systémových služeb a musí mít pět nebo více virtuálních počítačů.

    Důležitým krokem každého produkčního nasazení je [plánování kapacity](service-fabric-cluster-capacity.md).  V tomto rychlém zprovoznění ale nespouštíme aplikace, takže jako velikost virtuálního počítače vyberte *DS1_v2 Standard*.  Vyberte "Stříbrná" pro hello [úroveň spolehlivosti](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) a počáteční škálovací sady virtuálních počítačů kapacitu 5.  

    Vlastní koncové body otevře porty nástroji pro vyrovnávání zatížení Azure hello tak, aby můžete propojit s aplikací, které běží na clusteru hello.  Zadejte "80, 8172" tooopen až porty 80 a 8172.

    Neprovádět kontrolu hello **konfigurovat upřesňující nastavení** pole, která se používá k přizpůsobení koncových bodů správy TCP nebo HTTP, rozsahy portů aplikace, [omezení umístění](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints), a [kapacity vlastnosti](service-fabric-cluster-resource-manager-metrics.md).    

    Vyberte **OK**.

6. V hello **konfigurace clusteru** formuláři, nastavte **diagnostiky** příliš**na**.  Pro tento rychlý start, není nutné tooenter všechny [nastavení prostředků infrastruktury](service-fabric-cluster-fabric-settings.md) vlastnosti.  V **verze Fabric**, vyberte **automatické** upgrade režimu tak, aby Microsoft automaticky aktualizuje hello verze kódu fabric hello systémem hello clusteru.  Nastavit režim hello příliš**ruční** příliš Chcete-li[zvolte na podporovanou verzi](service-fabric-cluster-upgrade.md) tooupgrade k. 

    ![Konfigurace typu uzlu][node-type-config]

    Vyberte **OK**.

7. Vyplňte hello **zabezpečení** formuláře.  Pro toto rychlé zprovoznění vyberte **Nezabezpečené**.  Vysoce je doporučeno toocreate zabezpečení clusteru pro úlohy v produkčním prostředí, ale vzhledem k tomu, že každý, kdo anonymně připojení tooan nezabezpečená clusteru a provádět operace správy.  

    Certifikáty se používají v Service Fabric tooprovide ověřování a šifrování toosecure různé aspekty cluster a jeho aplikace. Další informace o použití certifikátů ve službě Service Fabric najdete v tématu věnovaném [scénářům zabezpečení clusteru Service Fabric](service-fabric-cluster-security.md).  tooenable ověřování uživatelů pomocí služby Azure Active Directory nebo tooset se certifikáty pro zabezpečení aplikací [vytvoření clusteru z šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md).

    Vyberte **OK**.

8. Hello Zkontrolujte souhrn.  Pokud chcete toodownload šablony Resource Manageru z nastavení hello jste zadali, vyberte **stáhnout šablonu a parametry**.  Vyberte **vytvořit** toocreate hello clusteru.

    Zobrazí se průběh vytváření hello v oznámeních hello. (Klikněte na ikonu zvonku"hello" v blízkosti hello stavového řádku v hello pravé horní části obrazovky.) Pokud jste klikli na **Pin tooStartboard** při vytváření clusteru hello, uvidíte **nasazení Cluster Service Fabric** připnutý toohello **spustit** panelu.

### <a name="view-cluster-status"></a>Zobrazení stavu clusteru
Po vytvoření clusteru si můžete prohlédnout cluster v hello **přehled** okna portálu hello. Zobrazí podrobnosti o hello clusteru na řídicím panelu hello, včetně veřejný koncový bod clusteru hello a odkaz tooService Fabric Exploreru.

![Stav clusteru][cluster-status]

### <a name="visualize-hello-cluster-using-service-fabric-explorer"></a>Vizualizace hello clusteru pomocí Service Fabric Exploreru
[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je nástroj vhodný pro vizualizaci clusteru a správu aplikací.  Service Fabric Explorer je služba, která běží v clusteru hello.  Přístup pomocí webového prohlížeče kliknutím hello **Service Fabric Explorer** odkaz hello clusteru **přehled** stránku hello portálu.  Můžete také zadat adresu hello přímo do prohlížeče hello: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)

řídicí panel clusteru Hello obsahuje přehled clusteru, včetně shrnutí stavu uzlu a aplikace. zobrazení uzlu Hello zobrazuje fyzické rozložení hello hello clusteru. Pro daný uzel můžete zjistit, které aplikace mají v uzlu nasazený kód.

![Service Fabric Explorer][service-fabric-explorer]

### <a name="connect-toohello-cluster-using-powershell"></a>Připojte toohello cluster pomocí prostředí PowerShell
Ověřte, zda je že daný cluster hello běží se připojíte pomocí prostředí PowerShell.  Hello ServiceFabric je nainstalován modul PowerShell s hello [Service Fabric SDK](service-fabric-get-started.md).  Hello [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) rutina vytvoří cluster toohello připojení.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
V tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md) pro další příklady připojování tooa clusteru. Po připojování toohello clusteru pomocí hello [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) rutiny toodisplay seznam uzlů v clusteru a stavové informace pro každý uzel hello. Položka **HealthState** by měla mít pro každý uzel hodnotu *OK*.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-hello-cluster"></a>Odebrat hello cluster
Cluster Service Fabric se skládá z jiných prostředků Azure kromě toohello prostředek clusteru, sám sebe. Takže toocompletely odstranit cluster Service Fabric je také nutné toodelete všechny prostředky, které se provádí z hello. Hello nejjednodušší způsob, jak toodelete hello clusteru a všechny prostředky hello, které budou využívat je skupina prostředků toodelete hello. Pro jiné způsoby toodelete cluster nebo toodelete některé (ale ne všechny) hello prostředky ve skupině prostředků najdete v části [odstranění clusteru](service-fabric-cluster-delete.md)

Odstranění skupiny prostředků v hello portálu Azure:
1. Přejděte cluster Service Fabric toohello chcete toodelete.
2. Klikněte na tlačítko hello **skupiny prostředků** názvem na stránce essentials hello clusteru.
3. V hello **Essentials skupiny prostředků** klikněte na tlačítko **odstranit skupinu prostředků** a postupujte podle pokynů hello na této stránce toocomplete hello odstranění skupiny prostředků hello.
    ![Odstraňte skupinu prostředků hello][cluster-delete]


## <a name="use-azure-powershell-toodeploy-a-secure-cluster"></a>Použití Azure Powershell toodeploy zabezpečení clusteru
1. Stáhnout hello [prostředí Azure Powershell verze modulu 4.0 nebo vyšší](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) na váš počítač.

2. Otevřete okno prostředí Windows PowerShell, hello spusťte následující příkaz. 
    
    ```powershell

    Get-Command -Module AzureRM.ServiceFabric 
    ```

    Měli byste vidět následující toohello podobné výstupu.

    ![ps-list][ps-list]

3. Přihlášení tooAzure a vyberte hello předplatné toowhich chcete toocreate hello clusteru

    ```powershell

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionId "Subcription ID" 
    ```

4. Spuštění hello následující příkaz toonow vytvoření clusteru s podporou zabezpečení. Nevynechali toocustomize hello parametry. 

    ```powershell
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $RDPpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 
    $RDPuser="vmadmin"
    $RGname="mycluster" # this is also hello name of your cluster
    $clusterloc="SouthCentralUS"
    $subname="$RGname.$clusterloc.cloudapp.azure.com"
    $certfolder="c:\mycertificates\"
    $clustersize=1 # can take values 1, 3-99

    New-AzureRmServiceFabricCluster -ResourceGroupName $RGname -Location $clusterloc -ClusterSize $clustersize -VmUserName $RDPuser -VmPassword $RDPpwd -CertificateSubjectName $subname -CertificatePassword $certpwd -CertificateOutputFolder $certfolder
    ```

    příkaz Hello může trvat od toocomplete minut too30 10 minut, na konci hello ho, měli byste obdržet následující toohello podobné výstupu. výstup Hello má informace o certifikátu hello, hello KeyVault, kde byl odeslán, a hello místní složku, kam je zkopírovali hello certifikátu. 

    ![ps-out][ps-out]

5. Zkopírujte celou výstupu hello a uložit tooa textového souboru, jako je třeba toorefer tooit. Poznamenejte si následující informace z výstupu hello hello. 

    - **CertificateSavedLocalPath** : c:\mycertificates\mycluster20170504141137.pfx
    - **CertificateThumbprint** : C4C1E541AD512B8065280292A8BA6079C3F26F10
    - **ManagementEndpoint** : https://mycluster.southcentralus.cloudapp.azure.com:19080
    - **ClientConnectionEndpointPort** : 19000

### <a name="install-hello-certificate-on-your-local-machine"></a>Nainstalujte certifikát hello na místním počítači
  
tooconnect toohello clusteru, musíte tooinstall hello certifikát do úložiště osobních (My) hello hello aktuálního uživatele. 

Spustit hello následující prostředí PowerShell

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\hello name of hello cert.pfx `
        -Password (ConvertTo-SecureString -String certpwd -AsPlainText -Force)
```

Nyní je připraven tooconnect tooyour zabezpečení clusteru.

### <a name="connect-tooa-secure-cluster"></a>Připojte tooa zabezpečené cluster 

Spustit hello následující příkaz prostředí PowerShell tooconnect tooa zabezpečení clusteru. Podrobnosti o Hello certifikátu musí odpovídat certifikát, který byl použité tooset až hello clusteru. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
          -StoreLocation CurrentUser -StoreName My
```


Následující příklad ukazuje hello Hello dokončil parametry: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Spusťte následující příkaz toocheck, že jste připojeni hello a hello clusteru je v pořádku.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="publish-your-apps-tooyour-cluster-from-visual-studio"></a>Publikování aplikace tooyour clusteru ze sady Visual Studio

Teď, když jste nastavili Azure clusteru, můžete publikovat aplikace tooit ze sady Visual Studio pomocí následující hello [publikovat tooan clusteru](service-fabric-publish-app-remote-cluster.md) dokumentu. 

### <a name="remove-hello-cluster"></a>Odebrat hello cluster
Cluster se skládá z jiných prostředků Azure kromě toohello prostředek clusteru, sám sebe. Hello nejjednodušší způsob, jak toodelete hello clusteru a všechny prostředky hello, které budou využívat je skupina prostředků toodelete hello. 

```powershell

Remove-AzureRmResourceGroup -Name $RGname -Force

```

## <a name="next-steps"></a>Další kroky
Teď, když jste nastavili vývojový cluster, zkuste následující hello:
* [Vytvoření clusteru s podporou zabezpečení hello portálu](service-fabric-cluster-creation-via-portal.md)
* [Vytvoření clusteru ze šablony](service-fabric-cluster-creation-via-arm.md) 
* [Nasazení aplikací s použitím prostředí PowerShell](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
[ps-list]: ./media/service-fabric-get-started-azure-cluster/pslist.PNG
[ps-out]: ./media/service-fabric-get-started-azure-cluster/psout.PNG
