---
title: "aaaConfigure hello skupiny dostupnosti Always On na virtuální počítač Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento kurz používá prostředky, které byly vytvořené pomocí modelu nasazení classic hello. Použijte PowerShell toocreate skupiny dostupnosti Always On v Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d4a27e203b2ff299adebec2b010c03422459b3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-always-on-availability-group-on-an-azure-vm-with-powershell"></a>Konfigurace skupiny dostupnosti Always On hello na virtuální počítač Azure pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [Klasické: uživatelského rozhraní](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasické: prostředí PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Než začnete, vezměte v úvahu, že je nyní možné dokončit tuto úlohu v modelu Azure resource manager. Doporučujeme, abyste model nástroje Správce prostředků Azure pro nová nasazení. V tématu [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> Doporučujeme vám, že většina nových nasazení používala model Resource Manager hello. Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení classic hello.

Virtuální počítače Azure (VM) může pomoct náklady na správci toolower hello databáze systému SQL Server vysokou dostupnost. Tento kurz ukazuje, jak tooimplement dostupnosti skupiny pomocí SQL serveru Always On klient server v prostředí Azure. Na konci hello hello kurzu vaše řešení SQL serveru Always On v Azure budou tvořeny hello následující prvky:

* Virtuální síť, která obsahuje více podsítí, včetně front-end a back-end podsítě.
* Řadič domény s doménou služby Active Directory.
* Dva SQL serveru virtuálních počítačů, které jsou nasazené toohello back-end podsíť a toohello připojené k doméně služby Active Directory.
* Tři uzly clusteru převzetí služeb při selhání systému Windows s modelem kvora Většina uzlů hello.
* Skupina dostupnosti databáze dostupnosti s dvěma replik se synchronním potvrzováním.

Tento scénář je vhodný pro jeho jednoduchost v Azure, ne pro jeho nákladová efektivnost nebo dalších faktorů. Například můžete minimalizovat hello počet virtuálních počítačů pro toosave dvě repliky dostupnosti skupiny na výpočetní hodiny v Azure pomocí řadiče domény hello jako určující sdílená složka souboru hello kvora v clusteru s podporou převzetí služeb při selhání dvěma uzly. Tato metoda snižuje počet hello virtuálních počítačů pomocí jedné z hello výše konfigurace.

Tento kurz je určen, že tooshow hello kroky, které jsou požadované tooset až hello popsané výše, řešení bez vypracování na hello podrobnosti o jednotlivých kroků. Proto místo Pokud kroků konfigurace hello grafickým uživatelským rozhraním, používá prostředí PowerShell skriptování tootake můžete rychle přes každý krok. Tento kurz předpokládá hello následující:

* Už máte účet Azure s předplatným hello virtuálního počítače.
* Jste nainstalovali hello [rutin prostředí Azure PowerShell](/powershell/azure/overview).
* Už máte plnou Principy Always On skupin dostupnosti pro místní řešení. Další informace najdete v tématu [Always On skupiny dostupnosti (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-tooyour-azure-subscription-and-create-hello-virtual-network"></a>Připojení tooyour předplatného Azure a vytvořit virtuální síť hello
1. V okně prostředí PowerShell v místním počítači importovat hello Azure modulu, stáhněte hello počítač tooyour soubor nastavení publikování a připojit vaše tooyour relace prostředí PowerShell předplatného Azure importováním hello stáhnout nastavení publikování.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Hello **Get-AzurePublishSettingsFile** příkaz automaticky vygeneruje certifikát pro správu s Azure a stáhne je tooyour počítače. Prohlížeč se automaticky otevře a vy budete výzvami tooenter přihlašovacích údajů účtu Microsoft hello vašeho předplatného Azure. Hello Stáhnout **.publishsettings** soubor obsahuje všechny informace hello, je nutné toomanage vašeho předplatného Azure. Po uložení tento soubor tooa místní adresář, ho importovat pomocí hello **Import AzurePublishSettingsFile** příkaz.

   > [!NOTE]
   > soubor .publishsettings Hello obsahuje vaše pověření (nekódovaných), které jsou používané tooadminister vaše předplatná Azure a služby. Hello osvědčený postup zabezpečení pro tento soubor je toostore ho dočasně mimo adresáře zdroje (například ve složce Libraries\Documents hello) a poté jej odstraňte po dokončení importu hello. Uživatel se zlými úmysly, který získá přístup k souboru .publishsettings toohello můžete upravit, vytvoření a odstranění služeb Azure.

2. Definujte řady proměnných, které použijete toocreate vaší cloudové infrastruktury IT.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Věnovat pozornost toohello následující tooensure, který bude později úspěšné příkazech:

   * Proměnné **$storageAccountName** a **$dcServiceName** musí být jedinečný, protože jsou použít tooidentify účet cloudového úložiště a cloudovými servery však v uvedeném pořadí, v hello Internetu.
   * Hello názvy, které zadáte pro proměnné **$affinityGroupName** a **$virtualNetworkName** konfigurované v dokumentu konfigurace virtuální sítě hello, které budete používat později.
   * **$sqlImageName** určuje hello aktualizovat název hello image virtuálního počítače, který obsahuje SQL Server 2012 Service Pack 1 Enterprise Edition.
   * Pro jednoduchost **Contoso! 000** je hello stejné heslo, které se používá napříč celou kurzu hello.

3. Vytvořte skupinu vztahů.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Vytvoření virtuální sítě pomocí importu konfiguračního souboru.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Hello konfigurační soubor obsahuje hello následující dokument XML. Stručně řečeno, určuje virtuální sítě s názvem **ContosoNET** ve skupině vztahů hello názvem **ContosoAG**. Má hello adresní prostor **10.10.0.0/16** a má dvě podsítě, **10.10.1.0/24** a **10.10.2.0/24**, které jsou v uvedeném pořadí hello podsítě front a zpět podsíť. Hello front podsíť je, kde můžete umístit klientské aplikace, třeba Microsoft SharePoint. je back podsíť Hello budete umístění virtuálních počítačů hello SQL serveru. Pokud změníte hello **$affinityGroupName** a **$virtualNetworkName** proměnné starší, musíte změnit taky hello odpovídající názvy níže.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. Vytvořte účet úložiště, který je spojen s hello skupinu vztahů, že jste vytvořili a nastavte ji jako hello aktuální účet úložiště v rámci vašeho předplatného.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Vytvořte server řadiče domény hello sady hello nové cloudové služby a dostupnost.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Tyto příkazy vytvoření kanálu hello následující věci:

   * **Nové AzureVMConfig** vytvoří konfigurace virtuálního počítače.
   * **Přidat AzureProvisioningConfig** poskytuje hello konfigurační parametry samostatný server systému Windows.
   * **Přidat AzureDataDisk** přidá hello datový disk, které budete používat pro ukládání dat služby Active Directory s hello tooNone sadu možnost ukládání do mezipaměti.
   * **Nový-AzureVM** vytvoří novou cloudovou službu a vytvoří hello nového virtuálního počítače Azure v hello novou cloudovou službu.

7. Počkejte hello nový virtuální počítač toobe plně zřízený a stáhnout hello souboru vzdálené plochy tooyour pracovní adresář. Vzhledem k tomu hello nový virtuální počítač Azure používá tooprovision dlouhou dobu, hello `while` smyčky pokračuje toopoll hello nový virtuální počítač, dokud je připravený k použití.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Nyní je úspěšně zřízen Hello server řadiče domény. Dále nakonfigurujete hello domény služby Active Directory na tomto serveru řadiče domény. Nechte okno prostředí PowerShell hello otevřené v místním počítači. Použijete jej znovu novější toocreate hello dva virtuální počítače SQL serveru.

## <a name="configure-hello-domain-controller"></a>Konfigurace řadiče domény hello
1. Připojte server řadiče domény toohello spuštěním souboru vzdálené plochy hello. Uživatelské jméno AzureAdmin a heslo správce počítače hello **Contoso! 000**, který jste zadali při vytvoření hello nového virtuálního počítače.
2. Otevřete okno prostředí PowerShell v režimu správce.
3. Spusťte následující hello **DCPROMO. EXE** tooset příkaz až hello **corp.contoso.com** domény s hello datové adresáře na disku M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Po dokončení příkazu hello hello virtuální počítač se automaticky restartuje.

4. Znovu připojte server řadiče domény toohello spuštěním souboru vzdálené plochy hello. Tentokrát, přihlaste se jako **CORP\Administrator**.
5. Otevřete okno prostředí PowerShell v režimu správce a importujte modul Active Directory PowerShell hello pomocí hello následující příkaz:

        Import-Module ActiveDirectory

6. Spusťte následující příkazy tooadd tři uživatelé toohello domény hello.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** je použité tooconfigure všechno související toohello instance služby SQL Server, hello převzetí služeb při selhání clusteru a skupinu dostupnosti hello. **CORP\SQLSvc1** a **CORP\SQLSvc2** jsou použity jako účty služby SQL Server hello pro hello dva virtuální počítače serveru SQL.
7. Hello další, spusťte následující příkazy toogive **CORP\Install** hello oprávnění toocreate počítačových objektů v doméně hello.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Hello výše zadaný identifikátor GUID je hello identifikátor GUID pro typ objektu počítače hello. Hello **CORP\Install** účet potřebuje hello **čtení všech vlastností** a **vytvářet objekty počítačů** oprávnění toocreate hello Active přímé objekty hello převzetí služeb při selhání cluster. Hello **čtení všech vlastností** oprávnění je již přidělena tooCORP\Install ve výchozím nastavení, takže není nutné toogrant jej explicitně. Další informace o oprávnění, které jsou potřeba toocreate hello převzetí služeb při selhání clusteru, najdete v části [převzetí služeb při selhání clusteru podrobný průvodce: Konfigurace účty ve službě Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Teď, když jste dokončili konfigurace služby Active Directory a hello uživatelské objekty, vytvoříte dva virtuální počítače serveru SQL a připojovat je toothis domény.

## <a name="create-hello-sql-server-vms"></a>Vytvoření virtuálních počítačů hello SQL serveru
1. Pokračujte v okně prostředí PowerShell hello toouse, které je otevřený v místním počítači. Definujte hello následující další proměnné:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Hello IP adresu **10.10.0.4** je obvykle přiřazena toohello první virtuální počítač, který vytvoříte v hello **10.10.0.0/16** podsíť virtuální sítě Azure. By měl ověřit, zda je hello adresu serveru řadiče domény tak, že spustíte **IPCONFIG**.
2. S názvem virtuální počítač v clusteru převzetí služeb při selhání hello spuštění hello po vytvoření kanálu příkazy toocreate hello nejprve **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Vezměte na vědomí následující hello týkající se nahoře na příkaz hello:

   * **Nové AzureVMConfig** vytvoří konfigurace virtuálního počítače se název sady dostupnosti požadované hello. Hello dalších virtuálních počítačů bude vytvořena s hello stejný název sady dostupnosti tak, aby se v připojené k toohello stejné skupině dostupnosti.
   * **Přidat AzureProvisioningConfig** spojení hello domény služby Active Directory toohello virtuálního počítače, který jste vytvořili.
   * **Set-AzureSubnet** místech hello virtuálního počítače v podsíti back hello.
   * **Nový-AzureVM** vytvoří novou cloudovou službu a vytvoří hello nového virtuálního počítače Azure v hello novou cloudovou službu. Hello **DnsSettings** parametr určuje tento server DNS hello pro hello servery v hello nové cloudové služby má hello IP adresu **10.10.0.4**. Toto je IP adresa hello hello server řadiče domény. Tento parametr je potřeba tooenable hello nové virtuální počítače v doméně služby Active Directory hello cloudové služby toojoin toohello úspěšně. Bez tohoto parametru je nutné ručně nastavit hello nastavením IPv4 na serveru řadiče domény hello toouse virtuálních počítačů jako primární server DNS hello po hello virtuálního počítače je zřízený a pak připojit k doméně služby Active Directory toohello hello virtuálních počítačů.
3. Po vytvoření kanálu spuštění hello příkazy hello toocreate virtuálním počítačům systému SQL Server, s názvem **ContosoSQL1** a **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Vezměte na vědomí následující hello týkající se příkazy hello výše:

   * **Nové AzureVMConfig** hello používá stejný název sady dostupnosti jako server řadiče domény hello a hello používá SQL Server 2012 Service Pack 1 Enterprise Edition obrázek v galerii virtuálních počítačů hello. Nastaví taky hello operačního systému disku tooread ukládání do mezipaměti pouze (bez ukládání do mezipaměti). Doporučujeme vám, že migrujete hello databáze soubory tooa samostatné datový disk připojit toohello virtuální počítač a konfigurovat žádné čtení nebo ukládání do mezipaměti. Hello skoro stejné je však, že tooremove ukládání do mezipaměti na disku operačního systému hello vzhledem k tomu, že nemůžete odebrat mezipaměti pro čtení na disku operačního systému hello.
   * **Přidat AzureProvisioningConfig** spojení hello domény služby Active Directory toohello virtuálního počítače, který jste vytvořili.
   * **Set-AzureSubnet** místech hello virtuálního počítače v podsíti back hello.
   * **Přidat AzureEndpoint** přidá přístup koncových bodů, aby tyto instance služby SQL Server na hello Internet přístup klientské aplikace. Jiné porty jsou uvedeny tooContosoSQL1 a ContosoSQL2.
   * **Nový-AzureVM** vytvoří hello nový virtuální počítač SQL Server v hello stejné cloudové služby jako ContosoQuorum. Virtuální počítače hello musí být ve stejné cloudové služby pokud je chcete toobe v hello hello stejné skupině dostupnosti.
4. Počkejte, než pro každý virtuální počítač toobe plně zřízený a pro každý virtuální počítač toodownload jeho souboru vzdálené plochy tooyour pracovní adresář. Hello `for` smyček procházení hello tři nové virtuální počítače a spouští příkazy hello uvnitř hello nejvyšší úrovně složené závorky pro každý z nich.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until hello VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    Hello virtuálních počítačů serveru SQL je nyní opatřen a spuštěná, ale instalované se systémem SQL Server s výchozími možnostmi.

## <a name="initialize-hello-failover-cluster-vms"></a>Inicializace hello převzetí služeb při selhání clusteru virtuálních počítačů
V této části je nutné hello toomodify tři se servery, které budete používat v clusteru převzetí služeb při selhání hello a instalace systému SQL Server hello. Zejména:

* Všechny servery: budete potřebovat tooinstall hello **Clustering převzetí služeb při selhání** funkce.
* Všechny servery: budete potřebovat tooadd **CORP\Install** jako počítač hello **správce**.
* ContosoSQL1 a ContosoSQL2 pouze: budete potřebovat tooadd **CORP\Install** jako **sysadmin** role v hello výchozí databáze.
* ContosoSQL1 a ContosoSQL2 pouze: budete potřebovat tooadd **NT AUTHORITY\System** jako Přihlaste se pomocí hello následující oprávnění:

  * Příkaz ALTER žádnou skupinu dostupnosti
  * Připojení SQL
  * Zobrazení stavu serveru
* ContosoSQL1 a ContosoSQL2 pouze: hello **TCP** na hello virtuální počítač SQL Server je již povolen protokol. Stále však tooopen hello brány firewall pro vzdálený přístup systému SQL Server.

Nyní jste toostart připraven. Počínaje **ContosoQuorum**, postupujte podle následujících kroků hello:

1. Připojit příliš**ContosoQuorum** spuštěním hello soubory vzdálené plochy. Použijte uživatelské jméno správce počítače hello **AzureAdmin** a heslo **Contoso! 000**, který jste zadali při vytváření virtuálních počítačů hello.
2. Ověřte, zda text hello počítače mít byla úspěšně připojeni příliš**corp.contoso.com**.
3. Počkejte toofinish instalace systému SQL Server hello systémem hello automatizované úlohy inicializace než budete pokračovat.
4. Otevřete okno prostředí PowerShell v režimu správce.
5. Nainstalujte funkci Clustering převzetí služeb při selhání Windows hello.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Přidat **CORP\Install** jako místní správce.

        net localgroup administrators "CORP\Install" /Add
7. Odhlásit z ContosoQuorum. Jste hotovi s tímto serverem teď.

        logoff.exe

V dalším kroku inicializovat **ContosoSQL1** a **ContosoSQL2**. Postupujte podle hello kroky, které jsou stejné pro oba virtuální počítače serveru SQL.

1. Připojte virtuální počítače serveru SQL toohello dva spuštěním hello soubory vzdálené plochy. Použijte uživatelské jméno správce počítače hello **AzureAdmin** a heslo **Contoso! 000**, který jste zadali při vytváření virtuálních počítačů hello.
2. Ověřte, zda text hello počítače mít byla úspěšně připojeni příliš**corp.contoso.com**.
3. Počkejte toofinish instalace systému SQL Server hello systémem hello automatizované úlohy inicializace než budete pokračovat.
4. Otevřete okno prostředí PowerShell v režimu správce.
5. Nainstalujte funkci Clustering převzetí služeb při selhání Windows hello.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Přidat **CORP\Install** jako místní správce.

        net localgroup administrators "CORP\Install" /Add
7. Importujte hello poskytovatele prostředí PowerShell pro Server SQL.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Přidat **CORP\Install** jako role sysadmin hello hello výchozí instanci SQL serveru.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. Přidat **NT AUTHORITY\System** jako u přihlášení s oprávněními hello tři popsané výše.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. Otevřete hello brány firewall pro vzdálený přístup systému SQL Server.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Odhlásit z oba virtuální počítače.

         logoff.exe

Nakonec jste skupiny dostupnosti připravené tooconfigure hello. Budete používat hello poskytovatele prostředí PowerShell pro Server SQL tooperform všechny hello pracovat na **ContosoSQL1**.

## <a name="configure-hello-availability-group"></a>Konfigurace skupiny dostupnosti hello
1. Připojit příliš**ContosoSQL1** znovu spuštěním hello soubory vzdálené plochy. Místo přihlášení pomocí účtu počítače hello, přihlaste se pomocí **CORP\Install**.
2. Otevřete okno prostředí PowerShell v režimu správce.
3. Definujte hello následující proměnné:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. Importujte hello poskytovatele prostředí PowerShell pro Server SQL.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. Změňte účet služby SQL Server hello ContosoSQL1 tooCORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. Změňte účet služby SQL Server hello ContosoSQL2 tooCORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. Stáhněte si **CreateAzureFailoverCluster.ps1** z [vytvořit Cluster převzetí služeb při selhání pro skupiny dostupnosti Always On ve virtuálním počítači Azure](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello místní pracovní adresář. Tento skript toohelp vytvoření clusteru s podporou převzetí služeb při selhání funkční budete používat. Důležité informace o clusteringu Windows převzetí služeb při selhání jak komunikuje s hello Azure sítě najdete v tématu [vysoké dostupnosti a zotavení po havárii pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Změňte tooyour pracovní adresář a vytvoření clusteru převzetí služeb při selhání hello pomocí skriptu hello stáhli.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. Povolte Always On skupiny dostupnosti pro instance systému SQL Server hello výchozí na **ContosoSQL1** a **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. Vytvořte adresář zálohy a udělit oprávnění účtům služby SQL Server hello. Tuto databázi dostupnosti hello tooprepare directory budete používat v sekundární replice hello.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Vytvořit databázi na **ContosoSQL1** názvem **MyDB1**, provést úplnou zálohu a zálohu protokolu a obnoví je na **ContosoSQL2** s hello **WITH NORECOVERY** možnost.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. Vytvoření hello dostupnosti skupiny koncových bodů na hello virtuálním počítačům systému SQL Server a nastavte u koncových bodů hello hello příslušná oprávnění.

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct1]" -ServerInstance $server2
13. Vytvořte hello replik dostupnosti.

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. Nakonec vytvořte hello skupiny dostupnosti a skupinu dostupnosti toohello sekundární repliky hello spojení.

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a>Další kroky
Jste teď úspěšně implementovali SQL serveru Always On tak, že vytvoříte skupinu dostupnosti v Azure. tooconfigure naslouchací proces pro tuto skupinu dostupnosti, najdete v části [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).

Další informace o používání systému SQL Server v Azure najdete v tématu [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
