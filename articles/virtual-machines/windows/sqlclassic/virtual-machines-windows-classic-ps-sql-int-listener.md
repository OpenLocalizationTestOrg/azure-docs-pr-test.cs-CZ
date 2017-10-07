---
title: "aaaConfigure naslouchací proces ILB pro Always On skupiny dostupnosti v Azure | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a vytvoří Always On naslouchací proces skupiny dostupnosti v Azure, která používá interní nástroj."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 2ce9b64fea491c945b58f7641e41fd39d90b078a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Konfigurace naslouchací proces ILB pro skupiny dostupnosti Always On v Azure
> [!div class="op_single_selector"]
> * [Interní naslouchací proces](../classic/ps-sql-int-listener.md)
> * [Externí naslouchací proces](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Přehled

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá hello pomocí modelu nasazení classic hello. Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello.

najdete v části tooconfigure naslouchací proces pro skupiny dostupnosti Always On v modelu Resource Manager hello [konfigurace pro vyrovnávání zatížení pro skupinu dostupnosti Always On v Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

Skupině dostupnosti může obsahovat repliky pouze místní nebo jenom Azure, které jsou, nebo které span místní a Azure pro hybridní konfigurace. Azure repliky mohou být uloženy v rámci hello stejné oblasti nebo v několika oblastech, které používají více virtuálních sítí. Hello postupy v tomto článku předpokládá, že máte již [nakonfigurovat skupinu dostupnosti](../classic/portal-sql-alwayson-availability-groups.md) , ale zatím nenakonfigurovali naslouchací proces.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Pokyny a omezení pro interní naslouchací procesy
použití Hello k interní pro vyrovnávání zatížení (ILB) s naslouchací proces skupiny dostupnosti v Azure je subjektu toohello následující pokyny:

* naslouchací proces skupiny dostupnosti Hello je podporován v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.
* Pouze jeden naslouchací proces skupiny dostupnosti interní je podporována pro jednotlivých cloudových služeb, protože je hello naslouchací proces nakonfigurovat toohello ILB a je pouze jeden ILB pro jednotlivých cloudových služeb. Je však možné toocreate více externí naslouchací procesy. Další informace najdete v tématu [konfigurace o externí naslouchací proces pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-ext-listener.md).

## <a name="determine-hello-accessibility-of-hello-listener"></a>Určení hello usnadnění hello naslouchacího procesu
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Tento článek se zaměřuje na tvorbu naslouchací proces, který používá ILB. Pokud potřebujete naslouchací proces veřejných nebo externí, naleznete v části hello verze tohoto článku, který popisuje nastavení nahoru [externí naslouchací proces](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Vytvoření koncové body s vyrovnáváním zatížení virtuálních počítačů s přímý server návratový
Nejprve vytvoříte ILB spuštěním skriptu hello později v této části.

Vytvořte koncový bod Vyrovnávání zatížení sítě pro každý virtuální počítač, který je hostitelem Azure repliky. Pokud máte repliky v několika oblastech, každou repliku pro danou oblast musí být ve stejné cloudové služby v hello hello stejné virtuální síti Azure. Vytváření replik skupin, které jsou rozmístěny v několika oblastmi Azure dostupnosti vyžaduje konfiguraci více virtuálních sítí. Další informace o konfiguraci křížové připojení k virtuální síti, najdete v části [nakonfigurovat virtuální síť připojení k síti toovirtual](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. V hello portálu Azure přejděte tooeach virtuálního počítače, který je hostitelem repliky tooview hello podrobnosti.

2. Klikněte na tlačítko hello **koncové body** kartu pro každý virtuální počítač.

3. Ověřte, že hello **název** a **veřejný Port** koncového bodu hello naslouchací proces, který chcete toouse již nejsou používány. V příkladu hello v této části, je název hello *MyEndpoint*, a hello port je *1433*.

4. Na místního klienta, stáhněte a nainstalujte nejnovější hello [modulu PowerShell](https://azure.microsoft.com/downloads/).

5. Spusťte prostředí Azure PowerShell.  
    Otevře novou relaci prostředí PowerShell s hello Azure pro správu moduly zavedené.

6. Spusťte `Get-AzurePublishSettingsFile`. Tato rutina přesměruje tooa prohlížeče toodownload k publikování nastavení souboru tooa místnímu adresáři. Pro vaše předplatné Azure, může být vyzvání k zadání pověření přihlášení.

7. Spusťte následující hello `Import-AzurePublishSettingsFile` příkaz s hello cestu hello publikovat nastavení souboru, který jste stáhli:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Po publikování hello importu souboru nastavení, můžete spravovat vaše předplatné Azure v relaci prostředí PowerShell hello.

8. Pro *ILB*, přiřadit statickou IP adresu. Zkontrolujte aktuální konfiguraci virtuální sítě hello spuštěním hello následující příkaz:

        (Get-AzureVNetConfig).XMLConfiguration
9. Poznámka: hello *podsíť* název pro podsíť hello, který obsahuje hello virtuálních počítačů, které jsou hostiteli hello repliky. Tento název se používá v parametru hello $SubnetName ve skriptu hello.

10. Poznámka: hello *VirtualNetworkSite* název a text hello, od *AddressPrefix* hello podsítě, která obsahuje hello virtuálních počítačů, které jsou hostiteli hello repliky. Hledat dostupnou adresu IP pomocí předání obě hodnoty toohello `Test-AzureStaticVNetIP` příkazu a kontrolou hello *AvailableAddresses*. Například, pokud hello virtuální sítě se jmenuje *MyVNet* a má rozsah adres podsítě, která se spouští v *172.16.0.128*, hello následující příkaz, by seznam dostupných adres:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Vyberte jednu z dostupných adres hello a použít ho v parametru hello $ILBStaticIP hello skriptu v dalším kroku hello.

12. Zkopírujte hello následující prostředí PowerShell skriptu tooa textovém editoru a nastavte toosuit hello hodnoty proměnných prostředí. Výchozí hodnoty jsou uvedeny některé parametry.  

    Existující nasazení, které používají skupiny vztahů nelze přidat ILB. Další informace o požadavcích na ILB najdete v tématu [přehled nástroje pro vyrovnávání zatížení pro vnitřní](../../../load-balancer/load-balancer-internal-overview.md).

    Navíc pokud vaší skupiny dostupnosti zahrnuje oblasti Azure, je nutné spustit skript hello jednou v každé datové centrum pro hello Cloudová služba a uzly, které jsou umístěny ve stejné datové centrum.

        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that hello replicas use in hello virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for hello ILB in hello subnet
        $ILBName = "AGListenerLB" # customize hello ILB name or use this default value

        # Create hello ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. Po nastavení proměnné hello kopie hello skript z hello textového editoru tooyour prostředí PowerShell relace toorun ho. Pokud stále zobrazuje hello řádku  **>>** , stiskněte klávesu Enter znovu toomake zda hello skript spuštěn.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Ověřte, zda KB2854082 nainstalována v případě potřeby
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a>Otevřete porty brány firewall hello uzly skupiny dostupnosti
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a>Vytvořte naslouchací proces skupiny dostupnosti hello

Vytvořte naslouchací proces skupiny dostupnosti hello ve dvou krocích. Nejprve vytvořte prostředek clusteru hello klientský přístup k bodu a nakonfigurovat závislosti. Druhý nakonfigurujte prostředky clusteru hello v prostředí PowerShell.

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a>Vytvořit hello klientský přístupový bod a nakonfigurovat závislosti clusteru hello
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a>Konfigurace prostředků clusteru hello v prostředí PowerShell
1. Pro ILB je nutné použít IP adresu hello hello ILB, který jste vytvořili. tooobtain této IP adres v prostředí PowerShell, použijte hello následující skript:

        # Define variables
        $ServiceName="<MyServiceName>" # hello name of hello cloud service that contains hello AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Na jednom ze hello virtuální počítače zkopírujte hello skript prostředí PowerShell pro váš operační systém tooa textovém editoru a nastavte hello proměnné toohello hodnoty, které jste si předtím poznamenali.

    Pro Windows Server 2012 nebo novější použijte hello následující skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    Pro Windows Server 2008 R2 použijte hello následující skript:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Až budete mít sadu hello proměnné, otevřete okno prostředí Windows PowerShell se zvýšenými oprávněními, vložte hello skript z hello textového editoru do vaší toorun relace prostředí PowerShell ji. Pokud stále zobrazuje hello řádku  **>>** , stiskněte klávesu Enter znovu toomake jistotu, že hello skript spuštěn.

4. Opakujte hello předcházející kroky pro každý virtuální počítač.  
    Tento skript nakonfiguruje prostředek hello IP adresy s IP adresou hello hello cloudové služby a nastaví dalších parametrů, jako je port testu hello. Pokud prostředek hello IP adresy je uvést do režimu online, může reagovat toohello dotazování na port testu hello z hello Vyrovnávání zatížení sítě koncový bod, který jste vytvořili dříve.

## <a name="bring-hello-listener-online"></a>Přepněte naslouchací proces hello online
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Položky následnou akci
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-virtual-network"></a>Naslouchací proces skupiny dostupnosti testu hello (uvnitř hello stejné virtuální sítě)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Další kroky
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
