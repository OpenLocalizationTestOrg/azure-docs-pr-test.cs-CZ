---
title: aaaHPC Pack clusteru pro aplikaci Excel a SOA | Microsoft Docs
description: "Začínáme s rozsáhlé úlohy aplikace Excel a SOA v clusteru HPC Pack v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: 55b4b2c25fe65d06b75025cc23c3c13b8b764238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Začít a spustit úlohy aplikace Excel a SOA v clusteru HPC Pack v Azure
Tento článek ukazuje, jak toodeploy Microsoft HPC Pack 2012 R2 clusteru na virtuálních počítačích Azure pomocí šablony Azure rychlý start, nebo volitelně skript nasazení Azure PowerShell. Hello clusteru používá virtuální počítač Azure Marketplace bitové kopie určené toorun Microsoft Excel nebo orientované na služby architektura (SOA) úlohy s HPC Pack. Můžete použít toorun hello clusteru HPC aplikace Excel a SOA služby z klientského počítače k místní. služby HPC pro Excel Hello zahrnují snižování zátěže sešitu aplikace Excel a uživatelem definované funkce aplikace Excel nebo UDF.

> [!IMPORTANT] 
> Tento článek vychází z funkce, šablony a skripty pro HPC Pack 2012 R2. Tento scénář není aktuálně podporován v prostředí HPC Pack 2016.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Na vysoké úrovni, hello následující diagram znázorňuje hello HPC Pack clusteru, že který vytvoříte.

![Cluster HPC s uzly, které jsou spuštěné úlohy aplikace Excel][scenario]

## <a name="prerequisites"></a>Požadavky
* **Klientský počítač** -je nutné klienta se systémem Windows počítače toosubmit ukázkové aplikace Excel a SOA úlohy toohello cluster. Musíte taky hello toorun počítače Windows Azure PowerShell skript nasazení clusteru (Pokud zvolíte tuto metodu nasazení).
* **Předplatné Azure** – Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) si během několika minut.
* **Kvóta jader** -může být nutné tooincrease hello kvóty jader, zvlášť pokud nasadíte několik uzlů clusteru s vícejádrovými velikosti virtuálních počítačů. Pokud používáte šablonu Azure rychlý start, kvóta jader hello ve službě Správce prostředků je za oblast Azure. V takovém případě můžete potřebovat tooincrease hello kvóty v určité oblasti. V tématu [limity předplatného Azure, kvóty a omezení](../../azure-subscription-service-limits.md). tooincrease kvótu, [otevřete žádosti o podporu online zákazníka](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) zdarma.
* **Licenci aplikace Microsoft Office** – Pokud nasadíte výpočetní uzly bitovou kopii virtuálního počítače Marketplace HPC Pack 2012 R2 pomocí aplikace Microsoft Excel, 30denní zkušební verzi Microsoft Excelu Professional Plus 2013 je nainstalována. Po hello zkušební období budete potřebovat tooprovide platný Microsoft Office licence tooactivate Excel toocontinue toorun úlohy. V tématu [aktivace v aplikaci Excel](#excel-activation) dále v tomto článku. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Krok 1. Nastavení clusteru služby HPC Pack v Azure
Ukážeme dvě možnosti tooset až clusteru HPC Pack 2012 R2 hello: první, použití šablony Azure rychlý start a hello portál Azure; a Zadruhé, pomocí skript nasazení Azure PowerShell.

### <a name="option-1-use-a-quickstart-template"></a>Možnost 1. Použít šabloně pro rychlý start
Použití služby Azure rychlý start tooquickly šablony nasazení clusteru HPC Pack v hello portálu Azure. Když otevřete šablonu hello hello portálu, zobrazí jednoduchého uživatelského rozhraní, kde zadáte hello nastavení pro váš cluster. Tady jsou kroky hello. 

> [!TIP]
> Pokud chcete, použijte [šablony Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) vytvářející cluster podobné speciálně pro zatížení aplikace Excel. kroky Hello mírně lišit od následující hello.
> 
> 

1. Navštivte hello [stránku šablony vytvořit Cluster prostředí HPC na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Pokud chcete, zkontrolujte informace o šabloně hello a hello zdrojového kódu.
2. Klikněte na tlačítko **nasazení tooAzure** toostart nasazení pomocí šablony hello v hello portálu Azure.
   
   ![Nasazení šablony tooAzure][github]
3. Hello portálu postupujte podle těchto kroků tooenter hello parametrů šablony clusteru HPC hello.
   
   a. Na hello **parametry** stránky zadejte nebo upravte hodnoty pro parametry šablony hello. (Klikněte na tlačítko hello další tooeach nastavení Ikona pro informace nápovědy.) Ukázkové hodnoty jsou uvedené v následující obrazovce hello. Tento příklad vytvoří cluster s názvem *hpc01* v hello *hpc.local* domény, který se skládá z hlavního uzlu a 2 výpočetních uzlů. Hello výpočetní uzly jsou vytvořeny z virtuálních počítačů HPC Pack obrázku, který obsahuje aplikace Microsoft Excel.
   
   ![Zadejte parametry][parameters-new-portal]
   
   > [!NOTE]
   > virtuální počítač je automaticky vytvořen z hello hlavního uzlu Hello [nejnovější image Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 na Windows Server 2012 R2. Aktuálně hello image založena na HPC Pack 2012 R2 Update 3.
   > 
   > Výpočetní uzel virtuální počítače jsou vytvořeny z nejnovější bitové kopie hello hello vybrané výpočetní uzel rodiny. Vyberte hello **ComputeNodeWithExcel** možnost hello nejnovější HPC Pack výpočetního uzlu bitové kopie obsahující zkušební verzi Microsoft Excelu Professional Plus 2013. toodeploy clusteru s podporou pro obecné SOA relací nebo snižování zátěže systému souborů UDF Excelu, zvolte hello **ComputeNode** možnost (bez nainstalované aplikace Excel).
   > 
   > 
   
   b. Zvolte předplatné hello.
   
   c. Vytvořte skupinu prostředků clusteru hello, jako například *hpc01RG*.
   
   d. Vyberte umístění pro skupinu prostředků hello, jako je například střed USA.
   
   e. Na hello **právní podmínky** stránky, přečtěte si podmínky hello. Pokud souhlasíte, klikněte na tlačítko **nákupu**. Potom klikněte na po skončení nastavení hello hodnoty pro šablonu hello **vytvořit**.
4. Po dokončení nasazení hello (obvykle trvá přibližně 30 minut), exportovat soubor certifikátu hello clusteru z hlavního uzlu clusteru hello. V pozdější fázi importujte tento veřejný certifikát hello ověřování klientského počítače tooprovide hello serverové pro zabezpečené vazby HTTP.
   
   a. V hello portálu Azure, přejděte toohello řídicí panel, vyberte hello hlavního uzlu a klikněte na tlačítko **Connect** hello horní části tooconnect stránku hello pomocí vzdálené plochy.
   
    <!-- ![Connect toohello head node][connect] -->
   
   b. Použijte standardní postupy v certifikátu hlavního uzlu hello tooexport správce certifikátů (nachází se v části Cert: \LocalMachine\My) bez hello soukromého klíče. V tomto příkladu exportovat *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Export certifikátu hello][cert]

### <a name="option-2-use-hello-hpc-pack-iaas-deployment-script"></a>Možnost 2. Použít skript nasazení IaaS HPC Pack hello
Hello skript nasazení HPC Pack IaaS poskytuje jiný způsob univerzální toodeploy clusteru služby HPC Pack. Vytvoří cluster v modelu nasazení classic hello, zatímco hello šablona používá hello modelu nasazení Azure Resource Manager. Skript hello je také kompatibilní s předplatným Azure globální hello nebo služby Azure China.

**Další požadavky**

* **Prostředí Azure PowerShell** - [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview) (verze 0.8.10 nebo novější) na klientském počítači.
* **Skript nasazení HPC Pack IaaS** – stáhněte a rozbalte hello nejnovější verzi hello skript z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Zkontrolujte verzi hello hello skriptu spuštěním `New-HPCIaaSCluster.ps1 –Version`. Tento článek je založen na verzi 4.5.0 nebo později hello skriptu.

**Vytvoření konfiguračního souboru hello**

 Hello skript nasazení HPC Pack IaaS používá jako vstup, který popisuje hello infrastruktura clusteru HPC hello konfigurační soubor XML. toodeploy cluster, který se skládá z hlavního uzlu a 18 výpočetních uzlů vytvořené z hello výpočetní uzel image, která obsahuje aplikaci Microsoft Excel, nahraďte hodnoty pro vaše prostředí do hello následující vzorový konfigurační soubor. Další informace o hello konfigurační soubor najdete v tématu hello Manual.rtf souboru ve složce hello skriptu a [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Poznámky k hello konfiguračního souboru**

* Hello **VMName** hlavního uzlu hello **musí** být hello stejné jako hello **ServiceName**, nebo úlohy architektury SOA selže toorun.
* Je nutné zadat **EnableWebPortal** tak, aby hello hlavního uzlu certifikátu je generována a exportovat.
* soubor Hello Určuje skript prostředí PowerShell po konfiguraci PostConfig.ps1, která běží na hello hlavního uzlu. Následující ukázkový skript Hello nakonfiguruje hello úložiště Azure připojovací řetězec, odebere hello výpočetní uzel roli z hlavního uzlu hello a přináší všechny uzly online při jejich nasazení. 

```
    # add hello HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set hello Azure storage connection string for hello cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove hello compute node role for head node toomake sure hello Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in hello deployment including hello head node and compute nodes, which should match hello number specified in hello XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Spusťte skript hello**

1. Otevřete konzolu prostředí PowerShell hello na klientském počítači hello jako správce.
2. Změnit složku toohello skriptu (E:\IaaSClusterScript v tomto příkladu).
   
   ```
   cd E:\IaaSClusterScript
   ```
3. toodeploy hello HPC Pack clusteru, spusťte následující příkaz hello. Tento příklad předpokládá, že hello konfigurační soubor se nachází v E:\HPCDemoConfig.xml.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

Hello HPC Pack nasazovací skript spustí nějakou dobu. Jeden skript hello věc je tooexport a stáhněte certifikát hello clusteru a uložit ho hello aktuální složku Dokumenty uživatele na klientském počítači hello. Hello skript generuje následující toohello podobné zprávy. V tomto kroku importujete hello certifikátu v úložišti hello příslušný certifikát.    

    You have enabled REST API or web portal on HPC Pack head node. Please import hello following certificate in hello Trusted Root Certification Authorities certificate store on hello computer where you are submitting job or accessing hello HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Krok 2. Snižování zátěže sešitů aplikace Excel a spusťte UDF z místního klienta
### <a name="excel-activation"></a>Aktivace aplikace Excel
Při použití hello image virtuálního počítače ComputeNodeWithExcel pro úlohy v produkčním prostředí, je nutné tooprovide platný Microsoft Office licenční klíče tooactivate aplikace Excel na výpočetních uzlech hello. Jinak hello zkušební verzi aplikace Excel vyprší po 30 dnů a systémem sešitů aplikace Excel se nezdaří s hello COMException (0x800AC472). 

Obnovení aplikace Excel můžete aktivačního období pro jiné 30 dnů od doby vyhodnocení: Přihlaste toohello hlavního uzlu a clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` na všechny aplikace Excel výpočetní uzly prostřednictvím Správce clusteru HPC. Po obnovení aktivačního období maximálně dvakrát. Potom je nutné zadat platný klíč licence Office.

Hello Office Professional Plus 2013 nainstalovaný na hello image virtuálního počítače je multilicenční edici s obecné svazku licenční klíč (kód GVLK). Aktivujte ji prostřednictvím služby správy klíčů (KMS) nebo aktivaci prostřednictvím služby (AD-BA) nebo klíč k vícenásobné aktivaci (MAK). 

    * toouse služby správy KLÍČŮ/AD-BA použít existující server služby správy KLÍČŮ nebo nastavte novou pomocí hello Microsoft Office 2013 svazku balíků. (Pokud chcete, nastavte server hello hello hlavního uzlu.) Potom aktivujte klíč hostitele služby správy KLÍČŮ hello prostřednictvím hello Internet nebo telefon. Potom clusrun `ospp.vbs` tooset hello serveru služby správy KLÍČŮ a portu a aktivovat Office na všechny hello Excel výpočetní uzly. 

    * toouse klíč k vícenásobné aktivaci, první clusrun `ospp.vbs` tooinput hello klíč a poté znovu aktivovat všechny výpočetní uzly hello Excel prostřednictvím hello Internet nebo telefon. 

> [!NOTE]
> Prodejní kódy product key pro Office Professional Plus 2013 nelze použít s touto bitovou kopií virtuálního počítače. Pokud máte platné klíče a instalační médium pro edice Office nebo aplikace Excel než tato edice Office Professional Plus 2013 svazku, můžete je používat místo. Nejprve odinstalujte tento multilicenční edici a nainstalujte hello edici, která máte. Hello přeinstalovat Excel výpočetním uzlu se dají zachytit jako vlastní toouse bitové kopie virtuálních počítačů v nasazení ve velkém měřítku.
> 
> 

### <a name="offload-excel-workbooks"></a>Snižování zátěže sešitů aplikace Excel
Postupujte podle těchto kroků toooffload sešitu aplikace Excel, tak, aby běžel v clusteru HPC Pack hello v Azure. toodo, musí být v aplikaci Excel 2010 nebo 2013 už nainstalovaná na klientském počítači hello.

1. Použijte jednu z možností hello v kroku 1 toodeploy clusteru služby HPC Pack s hello Excel výpočetní uzel image. Získáte hello clusteru soubor certifikátu (.cer) a cluster uživatelské jméno a heslo.
2. Na klientském počítači hello importujte certifikát hello clusteru pod Cert: \CurrentUser\Root.
3. Zkontrolujte, zda že je nainstalována aplikace Excel. Vytvořte soubor Excel.exe.config s hello následující obsah v hello stejné složce jako Excel.exe na klientském počítači hello. Tento krok zajistí, že tento hello doplněk HPC Pack 2012 R2 Excel COM úspěšně načten.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. Nastavení clusteru HPC Pack toohello toosubmit úlohy aplikace hello klienta. Jednou z možností je toodownload hello úplné [HPC Pack 2012 R2 Update 3 instalace](http://www.microsoft.com/download/details.aspx?id=49922) a instalace klienta na HPC Pack hello. Můžete taky stáhnout a nainstalovat hello [HPC Pack 2012 R2 Update 3 klienta nástroje](https://www.microsoft.com/download/details.aspx?id=49923) a hello odpovídající Visual C++ 2010 redistributable pro tento počítač ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).
5. V tomto příkladu používáme Ukázka sešitu aplikace Excel s názvem ConvertiblePricing_Complete.xlsb. Můžete ho stáhnout [zde](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Zkopírujte hello pracovní složky tooa sešitu aplikace Excel například D:\Excel\Run.
7. Otevřete sešit aplikace Excel hello. Na hello **vývoj** pásu karet, klikněte na tlačítko **COM Doplňky** a potvrďte, že hello doplňku HPC Pack Excel COM byla úspěšně zavedena.
   
   ![Add-in pro prostředí HPC Pack v aplikaci Excel][addin]
8. Makra VBA hello HPCControlMacros v aplikaci Excel upravit tak, že změníte hello komentář řádky, jak je znázorněno v následující skript hello. Nahraďte příslušnými hodnotami pro vaše prostředí.
   
   ![Makro aplikace Excel pro HPC Pack][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. Zkopírujte hello nahrávání adresáře tooan sešitu aplikace Excel například D:\Excel\Upload. Tento adresář je určen v konstanta HPC_DependsFiles hello v makro VBA hello.
10. toorun hello sešitu na clusteru hello v Azure, klikněte na tlačítko hello **clusteru** tlačítko na listu hello.

### <a name="run-excel-udfs"></a>Spusťte systém souborů UDF Excelu
toorun systém souborů UDF Excelu, postupujte podle předchozích kroků hello tooset 1 – 3 až hello klientský počítač. Pro systém souborů UDF Excelu nepotřebujete toohave hello Excel aplikace nainstalované na výpočetních uzlech. Tím, že při vytváření clusteru výpočetních uzlů, může zvolit bitovou kopii normální výpočetní uzel místo hello výpočetní uzel image pomocí aplikace Excel.

> [!NOTE]
> Existuje limit 34 znak v hello Excel 2010 a 2013 dialogové okno konektor clusteru. Můžete použít toto dialogové okno pole toospecify hello clusteru, který spouští hello UDF. Pokud je název clusteru úplné hello delší (například hpcexcelhn01.southeastasia.cloudapp.azure.com), v dialogovém okně hello nevejde. Hello alternativní řešení je tooset celého systému proměnná, jako třeba *CCP_IAASHN* s hodnotou hello název hello dlouho clusteru. Potom zadejte *CCP_IAASHN %* v dialogovém okně hello jako název hlavního uzlu clusteru hello. 
> 
> 

Po úspěšném nasazení clusteru hello, pokračujte hello následující kroky toorun integrované ukázka UDF Excelu. Vlastní systém souborů UDF Excelu, najdete v těchto [prostředky](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) toobuild hello XLL a nasadit je na hello IaaS clusteru.

1. Otevřete nový sešit aplikace Excel. Na hello **vývoj** pásu karet, klikněte na tlačítko **doplňky**. Klikněte v dialogovém okně hello, **Procházet**, přejděte toohello %CCP_HOME%Bin\XLL32 složky a vyberte hello ukázka ClusterUDF32.xll. Pokud hello ClusterUDF32 neexistuje na hello klientský počítač, zkopírujte jej ze složky %CCP_HOME%Bin\XLL32 hello hello hlavního uzlu.
   
   ![Vyberte hello UDF][udf]
2. Klikněte na tlačítko **soubor** > **možnosti** > **rozšířené**. V části **vzorce**, zkontrolujte **povolit výpočetním clusteru, na uživatelem definované funkce toorun XLL**. Pak klikněte na tlačítko **možnosti** a zadejte název clusteru úplné hello v **název hlavního uzlu clusteru**. (Jak je uvedeno dříve toto vstupní pole je omezená too34 znaků, tak, aby název dlouho clusteru nemusí vešly. Proměnnou celého systému může použít pro název dlouho clusteru.)
   
   ![Konfigurace hello UDF][options]
3. toorun hello výpočtu UDF v clusteru hello, klikněte na tlačítko hello buňky s hodnotou =XllGetComputerNameC() a stiskněte klávesu Enter. Funkce Hello jednoduše načte název hello hello výpočetním uzlu, na které hello UDF běží. Pro hello prvním spuštění dialogové okno přihlašovací údaje vyzve k zadání hello uživatelské jméno a heslo tooconnect toohello IaaS clusteru.
   
   ![Spustit UDF][run]
   
   Po mnoho toocalculate buněk se stisknutím Alt-Shift-Ctrl + F9 toorun hello výpočet na všechny buňky.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Krok 3. Spuštění úlohy architektury SOA z místního klienta
toorun obecné aplikace SOA v clusteru HPC Pack IaaS hello, nejprve použijte jednu z metod hello v kroku 1 toodeploy hello clusteru. Zadejte v tomto případě bitovou kopii obecné výpočetního uzlu, protože není nutné aplikaci Excel na výpočetních uzlech hello. Potom postupujte podle těchto kroků.

1. Po načtení certifikátu clusteru hello, importujte ho na klientském počítači hello pod Cert: \CurrentUser\Root.
2. Nainstalujte hello [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) a [HPC Pack 2012 R2 Update 3 klienta nástroje](https://www.microsoft.com/download/details.aspx?id=49923). Tyto nástroje umožňují toodevelop a spusťte SOA klientské aplikace.
3. Stáhnout hello HelloWorldR2 [ukázkový kód](https://www.microsoft.com/download/details.aspx?id=41633). Otevřete hello HelloWorldR2.sln v sadě Visual Studio 2010 nebo 2012. (Tato ukázka není momentálně kompatibilní s novější verzí sady Visual Studio.)
4. Nejprve sestavení projektu EchoService hello. Potom nasaďte hello služby toohello IaaS clusteru v hello stejným způsobem jako nasazení tooan místní cluster. Podrobné pokyny najdete v části hello Readme.doc v HelloWordR2. Upravit a vytvořit hello HellWorldR2 a další projekty, jak je popsáno v následující části toogenerate hello SOA klientské aplikace, které běží na clusteru služby Azure IaaS hello.

### <a name="use-http-binding-with-azure-storage-queue"></a>Vazba Http pomocí fronty Azure storage
Vazba Http toouse s frontu úložiště Azure provést několik změn toohello ukázkový kód.

* Název clusteru hello aktualizace.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* Volitelně můžete použít výchozí hello TransportScheme v SessionStartInfo nebo explicitně nastavit tooHttp.

```
    info.TransportScheme = TransportScheme.Http;
```

* Používejte výchozí vazbu pro hello BrokerClient.
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    Nebo nastavte explicitně pomocí hello basicHttpBinding.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* Volitelně můžete nastavit hello UseAzureQueue příznak tootrue v SessionStartInfo. Pokud není sada, nastaví se tootrue ve výchozím nastavení při hello název clusteru má přípony domény Azure a hello TransportScheme Http.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Použít vazbu Http bez fronty Azure storage
Vazba Http toouse bez fronty úložiště Azure, explicitně sadu hello UseAzureQueue příznak toofalse v hello SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>NetTcp vazbu používají
toouse NetTcp vazby, konfigurace hello je podobné tooconnecting tooan místní cluster. Je nutné tooopen několik koncových bodů hello hlavního uzlu virtuálního počítače. Pokud jste použili hello HPC Pack IaaS nasazení skriptu toocreate hello clusteru, například sada koncových bodů hello v hello portál Azure následujícím způsobem.

1. Zastavte hello virtuálních počítačů.
2. Přidat porty TCP hello 9090, 9087, 9091, 9094 pro hello relace, zprostředkovatel, respektive zprostředkovatel worker a datové služby
   
    ![Konfigurace koncových bodů][endpoint-new-portal]
3. Spusťte hello virtuálních počítačů.

Hello SOA klientská aplikace nevyžaduje žádné změny kromě změna hello head toohello IaaS clusteru úplný název.

## <a name="next-steps"></a>Další kroky
* V tématu [tyto prostředky](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) pro další informace o spuštění úlohy aplikace Excel pomocí sady HPC Pack.
* V tématu [Správa služeb SOA v Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) Další informace o nasazení a správě SOA services pomocí sady HPC Pack.

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
