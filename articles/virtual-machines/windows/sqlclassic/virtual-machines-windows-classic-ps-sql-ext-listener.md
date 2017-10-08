---
title: "aaaConfigure o externí naslouchací proces pro skupiny dostupnosti Always On | Microsoft Docs"
description: "Tento kurz nevystavíte slabé stránky zabezpečení vás provede kroky k vytvoření vždy na naslouchací proces skupiny dostupnosti v Azure, které je externě přístupné pomocí hello veřejná virtuální IP adresa hello související cloudové služby."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a2453032-94ab-4775-b976-c74d24716728
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: f8e2110bcc25d9cb7653675cb4ae5c8d717b6902
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Konfigurace o externí naslouchací proces pro skupiny dostupnosti Always On v Azure
> [!div class="op_single_selector"]
> * [Interní naslouchací proces](../classic/ps-sql-int-listener.md)
> * [Externí naslouchací proces](../classic/ps-sql-ext-listener.md)
> 
> 

Toto téma ukazuje, jak hello tooconfigure naslouchací proces pro vždy na skupiny dostupnosti, která je dostupné externě na Internetu. To je umožněno díky přidružení hello Cloudová služba **veřejná virtuální IP (VIP)** adresu hello naslouchací proces.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.

Vaší skupiny dostupnosti může obsahovat replik, které jsou pouze, místní, Azure nebo span místní a Azure pro hybridní konfigurace. Azure repliky mohou být uloženy v rámci hello stejné oblasti nebo nad několika oblastmi pomocí více virtuálních sítí (virtuální sítě). Následující postup Hello předpokládá, že máte již [nakonfigurovat skupinu dostupnosti](../classic/portal-sql-alwayson-availability-groups.md) ale nenakonfigurovali naslouchací proces.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Pokyny a omezení pro externí naslouchací procesy
Vezměte na vědomí následující pokyny o hello naslouchací proces skupiny dostupnosti v Azure při nasazení pomocí hello cloudové služby stydké VIP adresy hello:

* naslouchací proces skupiny dostupnosti Hello je podporován v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.
* klientská aplikace Hello se musí nacházet v jiné cloudové službě než hello ten, který obsahuje vaší skupiny dostupnosti virtuálních počítačů. Azure nepodporuje přímé odpovědi ze serveru s klientem a serverem v hello stejné cloudové služby.
* Ve výchozím nastavení hello kroky v tomto článku ukazují, jak tooconfigure jeden naslouchací proces toouse hello cloudové virtuální IP (VIP) Adresa služby. Ale je možné tooreserve a vytvořte více VIP adresy pro cloudové služby. To vám umožní toouse hello kroky tohoto článku toocreate více naslouchací procesy, které jsou všechny přidružené k jiné virtuální IP adresy. Informace o tom, jak toocreate více adres VIP, najdete v části [několika virtuálními IP adresami za cloudové služby](../../../load-balancer/load-balancer-multivip.md).
* Pokud vytváříte naslouchací proces pro hybridní prostředí, hello místní síť musí mít připojení k toohello veřejného Internetu v přidání toohello site-to-site VPN s hello virtuální síť Azure. V hello podsíti Azure, je dostupná pouze pro hello veřejnou IP adresu odpovídající cloud služby hello hello naslouchací proces skupiny dostupnosti.
* Není podporováno toocreate o externí naslouchací proces ve stejné cloudové služby, kde máte také naslouchací proces interní pomocí hello hello interní nástroj pro vyrovnávání zatížení (ILB).

## <a name="determine-hello-accessibility-of-hello-listener"></a>Určení hello usnadnění hello naslouchacího procesu
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Tento článek se zaměřuje na tvorbu naslouchací proces, který používá **Vyrovnávání zatížení externí**. Pokud chcete naslouchací proces, který je tooyour privátní virtuální sítě, naleznete v části hello verze tohoto článku, který obsahuje postup pro nastavení [naslouchací proces s ILB](../classic/ps-sql-int-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Vytvoření koncové body s vyrovnáváním zatížení virtuálních počítačů s přímý server návratový
Vyrovnávání zatížení externí používá hello virtuálním hello veřejná virtuální IP adresa hello cloudové služby, který je hostitelem virtuálních počítačů. Proto není nutné toocreate nebo nakonfigurujte hello nástroj pro vyrovnávání zatížení v tomto případě.

Koncový bod Vyrovnávání zatížení sítě je nutné vytvořit pro každý virtuální počítač Azure repliky hostování. Pokud máte repliky v několika oblastech, každou repliku pro danou oblast musí být ve stejné cloudové služby v hello hello stejnou virtuální síť. Vytvoření skupiny dostupnosti replik, které jsou rozmístěny v několika oblastmi Azure vyžaduje konfiguraci více virtuálních sítí. Další informace o konfiguraci křížové připojení virtuální sítě najdete v tématu [tooVNet nakonfigurovat virtuální síť připojení](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. V hello portálu Azure přejděte tooeach virtuálního počítače, který je hostitelem repliky a zobrazení podrobností hello.
2. Klikněte na tlačítko hello **koncové body** kartu pro každý hello virtuálních počítačů.
3. Ověřte, že hello **název** a **veřejný Port** koncového bodu hello naslouchací proces má toouse není již používán. V příkladu hello níže je "MyEndpoint" hello název a hello port je "1433".
4. Na místního klienta, stáhněte a nainstalujte [nejnovější modul prostředí PowerShell hello](https://azure.microsoft.com/downloads/).
5. Spusťte **prostředí Azure PowerShell**. Nové prostředí PowerShell, které se relace je otevřena s hello Azure pro správu moduly zavedené.
6. Spustit **Get-AzurePublishSettingsFile**. Tato rutina přesměruje tooa prohlížeče toodownload k publikování nastavení souboru tooa místnímu adresáři. Budete vyzváni k zadání pověření přihlašování pro vaše předplatné Azure.
7. Spustit hello **Import AzurePublishSettingsFile** příkaz s hello cestu hello publikovat nastavení souboru, který jste stáhli:
   
        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>
   
    Po publikování hello importu souboru nastavení, můžete spravovat vaše předplatné Azure v relaci prostředí PowerShell hello.
    
1. Zkopírujte níže uvedený skript prostředí PowerShell hello do textového editoru a nastavte toosuit hello hodnoty proměnných prostředí (výchozí hodnoty byly zadány pro některé parametry). Všimněte si, že pokud vaší skupiny dostupnosti zahrnuje oblasti Azure, je nutné spustit skript hello jednou v každé datové centrum pro hello Cloudová služba a uzly, které jsou umístěny ve stejné datové centrum.
   
        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
   
        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

2. Jakmile jednou nastavíte hello proměnné, kopie hello skript z hello textového editoru do vašeho prostředí Azure PowerShell relace toorun ho. Pokud stále zobrazuje hello řádku >>, zadejte znovu zadejte, zda skript hello toomake spuštění.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Ověřte, zda KB2854082 nainstalována v případě potřeby
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a>Otevřete porty brány firewall hello uzly skupiny dostupnosti
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a>Vytvořte naslouchací proces skupiny dostupnosti hello

Vytvořte naslouchací proces skupiny dostupnosti hello ve dvou krocích. Nejprve vytvořte prostředek clusteru hello klientský přístup k bodu a nakonfigurovat závislosti. Druhý nakonfigurujte prostředky clusteru hello pomocí prostředí PowerShell.

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a>Vytvořit hello klientský přístupový bod a nakonfigurovat závislosti clusteru hello
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a>Konfigurace prostředků clusteru hello v prostředí PowerShell
1. Pro externí zátěže, je třeba získat hello veřejná virtuální IP adresa hello Cloudová služba, která obsahuje repliky. Přihlaste se k hello portálu Azure. Přejděte toohello Cloudová služba, která obsahuje vaší skupiny dostupnosti virtuálních počítačů. Otevřete hello **řídicí panel** zobrazení.
2. Poznamenejte si adresu hello v části **veřejná virtuální IP (VIP) Adresa**. Pokud vaše řešení zahrnuje virtuální sítě, můžete tento krok opakujte pro každou cloudovou službu, která obsahuje virtuální počítač, který je hostitelem repliky.
3. Na jednom ze hello virtuální počítače do textového editoru, zkopírujte hello níže uvedený skript prostředí PowerShell a nastavení proměnných hello toohello hodnoty, které jste si předtím poznamenali.
   
        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service
   
        Import-Module FailoverClusters
   
        # If you are using Windows Server 2012 or higher, use hello Get-Cluster Resource command. If you are using Windows Server 2008 R2, use hello cluster res command. Both commands are commented out. Choose hello one applicable tooyour environment and remove hello # at hello beginning of hello line tooconvert hello comment tooan executable line of code.
   
        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255
4. Jednou jste nastavili hello proměnné, otevřete okno se zvýšenými oprávněními prostředí Windows PowerShell, potom zkopírujte skript hello z hello textovém editoru a vložit do vašeho prostředí Azure PowerShell relace toorun. Pokud stále zobrazuje hello řádku >>, zadejte znovu zadejte, zda skript hello toomake spuštění.
5. Tento postup opakujte pro každý virtuální počítač. Tento skript nakonfiguruje hello IP adresu prostředku s IP adresou hello hello cloudové služby a nastaví další parametry, jako je port testu hello. Při hello prostředků IP adresa je uvést do režimu online, můžete pak odpověď toohello dotazování na port testu hello z v tomto kurzu vytvořili koncový bod Vyrovnávání zatížení sítě hello.

## <a name="bring-hello-listener-online"></a>Přepněte naslouchací proces hello online
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Položky následnou akci
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-vnet"></a>Naslouchací proces skupiny dostupnosti testu hello (uvnitř hello stejné virtuální sítě)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-hello-availability-group-listener-over-hello-internet"></a>Naslouchací proces skupiny dostupnosti testu hello (přes hello internet)
V pořadí naslouchací proces tooaccess hello z mimo hello virtuální sítě, je třeba použít vyrovnávání zatížení externí nebo veřejné (popsaný v tomto tématu) místo ILB, který je dostupný jenom z v rámci hello stejnou virtuální síť. V připojovacím řetězci hello zadejte název hello cloudové služby. Například pokud jste měli cloudové služby s názvem hello *mycloudservice*, hello sqlcmd příkaz vypadat takto:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Na rozdíl od hello předchozí příklad, musí ověřování SQL použít, protože volající hello nelze použít ověřování systému windows přes hello Internetu. Další informace najdete v tématu [vždy na skupiny dostupnosti ve virtuálním počítači Azure: případech připojení klienta](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Při použití ověřování SQL, ujistěte se, že vytvoříte hello stejné přihlášení na obou replikách. Další informace o odstraňování potíží s přihlášení se skupinami dostupnosti najdete v tématu [jak toomap přihlášení nebo použijte obsažen SQL uživatele tooconnect tooother replik databáze a databáze tooavailability map](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Pokud hello Always On repliky jsou v různých podsítích, musí klienti zadat **MultisubnetFailover = True** hello připojovacího řetězce. Výsledkem tooreplicas pokusy o paralelní připojení v různých podsítích hello. Všimněte si, že tento scénář zahrnuje nasazení mezi oblastmi vždy na skupiny dostupnosti.

## <a name="next-steps"></a>Další kroky
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]

