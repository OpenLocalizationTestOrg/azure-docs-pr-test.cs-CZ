---
title: "aaaAzure příklad DMZ – vytvoření jednoduché DMZ pomocí skupin Nsg | Microsoft Docs"
description: "Sestavení DMZ se skupinami zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: f8622b1d-c07d-4ea6-b41c-4ae98d998fff
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 32a40a8dc7539c4c7293988e6c36e5e32ef11045
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-classic-powershell"></a>Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg classic PowerShell
[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]

> [!div class="op_single_selector"]
> * [Šablona Resource Manageru](virtual-networks-dmz-nsg.md)
> * [Classic – prostředí PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Tento příklad vytvoří primitivní hraniční sítě se čtyřmi servery Windows a skupiny zabezpečení sítě. Tento příklad popisuje každý hello relevantní prostředí PowerShell příkazy tooprovide podrobnější vysvětlení jednotlivých kroků. K dispozici je také provozu scénář části tooprovide na podrobné podrobné jak provoz pokračuje prostřednictvím hello vrstev obrany ve hello hraniční sítě. Nakonec v části odkazy hello je kompletní kód hello a instrukce toobuild tento tootest prostředí a experimentů pomocí různé scénáře. 

![Příchozí DMZ s NSG][1]

## <a name="environment-description"></a>Popis prostředí
V tomto příkladu obsahuje předplatné hello následující prostředky:

* Dva cloudové služby: "FrontEnd001" a "BackEnd001"
* Virtuální síť, "CorpNetwork" se dvěma podsítěmi; "FrontEnd" a "Back-end"
* Skupinu zabezpečení sítě, která je použitá tooboth podsítě
* Windows Server, který představuje server webových aplikací ("IIS01")
* Dva windows serverů, které představují servery back-end aplikace ("AppVM01", "AppVM02")
* Windows server, který představuje server DNS ("DNS01")

V části odkazy hello je skript prostředí PowerShell, který sestaví většinu hello prostředí popsané v tomto příkladu. Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádějí hello ukázkový skript, nejsou popsané podrobně v tomto dokumentu. 

toobuild hello prostředí;

1. Uložit hello sítě konfigurační soubor xml v oddíle odkazy hello (aktualizovat název, umístění a IP adresy toomatch hello zadané scénář)
2. Aktualizace hello uživatelské proměnné ve hello skriptu toomatch hello prostředí hello skriptu je toobe spouštění (odběry, názvy služeb atd.)
3. Spuštění skriptu hello v prostředí PowerShell

>[!Note]
>oblast Hello označený ve hello skript prostředí PowerShell musí odpovídat hello oblast označený ve hello sítě konfigurační soubor xml.
>
>

Jakmile hello skript spustí úspěšně další volitelné kroky mohou být přijata, v části hello odkazy jsou dva skripty tooset hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě.

Hello následující části obsahují podrobný popis skupiny zabezpečení sítě a jak fungují v tomto příkladu pomocí klíče řádky skriptu prostředí PowerShell hello s návodem.

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení sítě (NSG)
V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla. 

> [!TIP]
> Obecně platí musí nejprve vytvořit konkrétní pravidel "Povolit" a pak poslední hello více obecná pravidla "Deny". Hello prioritou stanoví, která pravidla se vyhodnocují jako první. Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla. Pravidla NSG můžete použít buď v hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).
> 
> 

Deklarativně jsou pro příchozí provoz sestavuje hello následující pravidla:

1. Interní DNS provoz (port 53) je povolený
2. Provoz protokolu RDP (portu 3389) z Internetu tooany hello virtuálních počítačů je povoleno.
3. Je povolen přenos HTTP (port 80) ze serveru tooweb Internet hello (IIS01)
4. Jakýkoli přenos (všechny porty) z IIS01 tooAppVM1 je povolen.
5. Jakýkoli přenos (všechny porty) z Internetu toohello hello celý virtuální síť (obě podsítě) byl odepřen.
6. Jakýkoli přenos (všechny porty) z hello front-endu podsíť toohello back-end podsítě byl odepřen.

Pomocí těchto pravidel vázané tooeach podsíti, pokud požadavek HTTP byl příchozí z hello Internet toohello webový server, obě pravidla 3 (Povolit) a 5 (Odepřít) by použít, ale vzhledem k tomu, že pravidlo 3 má vyšší prioritu jenom by použít a pravidlo 5 nebude možné uplatnit. Proto hello požadavku HTTP bude mít možnost toohello webový server. Pokud tento stejný provoz pokoušel tooreach hello DNS01 server, pravidlo 5 (Odepřít) bude, že hello první tooapply hello přenosů dat a nebude možné toopass toohello serveru. Pravidlo 6 (Odepřít) blokuje podsíť Frontend hello z rozhovoru toohello back-end podsítě (s výjimkou povolené přenosy v pravidlech 1 a 4), této sady pravidel chrání síť back-end hello v případě, že by útočník ohrožení hello webovou aplikaci na hello front-endu, útočník hello mít omezený přístup toohello back-end "chráněné" sítě (jenom tooresources zveřejněné na serveru AppVM01 hello).

Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu. V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla. toolock dolů provoz v obou směrech uživatele definovaná směrování je povinný a je prozkoumali "Příklad 3" na hello [stránku osvědčené postupy zabezpečení hranic][HOME].

Každé pravidlo je podrobněji popsána takto (**Poznámka**: libovolnou položku v následujícím seznamu počínaje znak dolaru hello (například: $NSGName) je uživatelem definované proměnné ze skriptu hello v hello odkaz části tohoto dokumentu):

1. Skupina zabezpečení sítě musí být nejprve sestaven toohold hello pravidla:

    ```PowerShell
    New-AzureNetworkSecurityGroup -Name $NSGName `
        -Location $DeploymentLocation `
        -Label "Security group for $VNetName subnets in $DeploymentLocation"
    ```

2. první pravidlo Hello v tomto příkladu umožňuje přenosů mezi všechny server DNS toohello interní sítě v podsíti hello back-end. pravidlo Hello má některé důležité parametry:
   
   * "Typ" označuje, že ve směru toku provozu účinné toto pravidlo. Směr Hello je z hlediska hello hello podsíť nebo virtuálního počítače (v závislosti na tom, kde je tato skupina NSG vázán). Proto pokud je typ "Příchozí" a provoz vstupující hello podsíť, hello pravidlo vztahuje a odchozího provozu z podsítě hello ovlivněn tímto pravidlem.
   * "Priority" Nastaví hello pořadí, ve kterém je přenosový tok vyhodnocena. Hello nižší hello číslo hello vyšší hello prioritou. Když se pravidlo vztahuje tooa konkrétní přenosový tok, žádná další pravidla se zpracovávají. Proto pokud pravidlo s prioritou 1 umožňuje provoz a pravidlo s prioritou 2 odmítne provozu a obě pravidla použít tootraffic pak provoz hello bude mít možnost tooflow (vzhledem k tomu, že pravidlo 1 měl vyšší prioritu trvalo účinek a žádná další pravidla byly použity).
   * "Action" označuje, že pokud toto pravidlo přenosů blokovat nebo povolit.

    ```PowerShell    
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
        -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[4] `
        -DestinationPortRange '53' `
        -Protocol *
    ```

3. Toto pravidlo umožňuje tooflow provoz protokolu RDP z hello internet toohello portu RDP na libovolném serveru v hello vázaný podsítě. Toto pravidlo používá dva speciální typy předpon adres; "VIRTUAL_NETWORK" a "INTERNET". Tyto značky se snadný způsob tooaddress vyšší kategorie předpon adres.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" `
         -Type Inbound -Priority 110 -Action Allow `
         -SourceAddressPrefix INTERNET -SourcePortRange '*' `
         -DestinationAddressPrefix VIRTUAL_NETWORK `
         -DestinationPortRange '3389' `
         -Protocol *
    ```

4. Toto pravidlo umožňuje příchozí internetové přenosy toohit hello webový server. Toto pravidlo nedojde ke změně chování směrování hello. Hello pravidlo umožňuje pouze provoz určený pro IIS01 toopass. Takže pokud provoz z Internetu hello měl hello webový server jako svůj cíl toto pravidlo by se povolit a zastavit zpracování další pravidla. (V hello pravidla s důležitostí 140 všechny ostatní příchozí internetový provoz blokováno). Pokud máte pouze zpracování přenos HTTP, může být toto pravidlo další s omezeným přístupem tooonly povolit cílový Port 80.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
         Set-AzureNetworkSecurityRule -Name "Enable Internet too$VMName[0]" `
         -Type Inbound -Priority 120 -Action Allow `
         -SourceAddressPrefix Internet -SourcePortRange '*' `
         -DestinationAddressPrefix $VMIP[0] `
         -DestinationPortRange '*' `
         -Protocol *
    ```

5. Toto pravidlo umožňuje toopass provoz ze serveru IIS01 hello toohello AppVM01 serveru, novější pravidlo blokuje všechny ostatní přenosy tooBackend front-endu. tooimprove, který toto pravidlo, pokud hello port se ví, že má být přidána. Například pokud server služby IIS hello je stiskne pouze SQL Server na AppVM01, rozsah cílových portů hello by mělo být změněno z "*" (Any) too1433 (hello port služby SQL) umožňuje menší prostor pro příchozí útok na AppVM01, by měla webová aplikace hello někdy dojít k ohrožení.

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] too$VMName[2]" `
        -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
    ```

6. Toto pravidlo odmítne provoz z hello internetové tooany servery v síti hello. S hello pravidla s důležitostí 110 a 120 hello efekt je tooallow pouze příchozí internetové přenosy toohello brány firewall a porty protokolu RDP na serverech a blokuje nic jiného. Toto pravidlo je "jistotu" pravidlo tooblock všechny neočekávané toky.
    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $VNetName VNet from hello Internet" `
        -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *
    ```
7. poslední pravidlo Hello odmítne provoz z hello front-endu podsíť toohello back-end podsítě. Vzhledem k tomu, že toto pravidlo je pouze příchozí pravidlo, zpětné provoz je povolený (z back-end toohello hello front-endu).

    ```PowerShell
    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule `
        -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" `
        -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix `
        -DestinationPortRange '*' `
        -Protocol * 
    ```

## <a name="traffic-scenarios"></a>Provoz scénáře
#### <a name="allowed-internet-tooweb-server"></a>(*Povolené*) internetového tooweb serveru
1. Internetu uživatel požádá o stránku HTTP z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)
2. Cloudové služby předává přenos prostřednictvím otevřených koncových bodů na portu 80 směrem IIS01 (hello webový server)
3. Podsíť frontend zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla
4. Provoz dotkne interní IP adresu hello webového serveru IIS01 (10.0.1.5)
5. IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello
6. IIS01 hello systému SQL Server na AppVM01 vyzve k zadání informací
7. Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti front-endu, provoz je povolený
8. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Skupina NSG pravidla 3 (Internet tooFirewall) netýká, přesuňte toonext pravidlo
   4. Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla
9. AppVM01 přijímá hello dotazu SQL a odpoví
10. Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti hello back-end, je povoleno hello odpovědi
11. Podsíť frontend zahájí zpracování příchozí pravidlo:
    1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
    2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.
12. server služby IIS Hello obdrží odpověď hello SQL a dokončí hello odpovědi HTTP a odešle toohello žadatele
13. Vzhledem k tomu, že neexistují žádná odchozí pravidla na podsíť Frontend hello hello odpovědi je povolen a hello internet uživatel obdrží webovou stránku hello požadovaný.

#### <a name="allowed-rdp-toobackend"></a>(*Povolené*) toobackend protokolu RDP
1. Správce serveru na Internetu požadavky tooAppVM01 relace protokolu RDP na BackEnd001.CloudApp.Net:xxxxx kde xxxxx je číslo portu hello náhodně přiřadí tooAppVM01 protokolu RDP (portu přiřazené hello naleznete na hello portál Azure nebo pomocí prostředí PowerShell)
2. Back-end podsíť zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla
3. Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený
4. Je povoleno relaci protokolu RDP.
5. AppVM01 vyzve k zadání hello uživatelské jméno a heslo

#### <a name="allowed-web-server-dns-look-up-on-dns-server"></a>(*Povolené*) hledání DNS webového serveru na serveru DNS
1. Webový Server, IIS01, požadavky datového kanálu v www.data.gov, ale potřebuje tooresolve hello adresu.
2. Hello konfiguraci sítě pro virtuální síť seznamy hello DNS01 (10.0.2.4 v podsíti hello back-end) jako primární server DNS hello IIS01 odešle tooDNS01 požadavek DNS hello
3. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
4. Back-end podsíť zahájí zpracování příchozí pravidlo:
   * Použít NSG pravidlo 1 (DNS), Probíhá zpracování povolených, zastavení pravidla
5. DNS server obdrží požadavek hello
6. DNS server nemá hello adresu do mezipaměti a požádá kořenový server DNS na hello Internetu
7. Žádná odchozí pravidla na back-end podsítě provoz je povolený.
8. Server DNS pro Internet odpoví, vzhledem k tomu, že tuto relaci bylo zahájeno interně, je povoleno hello odpovědi
9. DNS server ukládá do mezipaměti odpovědi hello a odpoví back tooIIS01 toohello úvodního požadavku
10. Žádná odchozí pravidla na back-end podsítě provoz je povolený.
11. Podsíť frontend zahájí zpracování příchozí pravidlo:
    1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
    2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený
12. IIS01 obdrží odpověď hello z DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Povolené*) přístup k souboru webového serveru na AppVM01
1. IIS01 požádá o soubor na AppVM01
2. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
3. podsíť back-end Hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Skupina NSG pravidla 3 (Internet tooIIS01) netýká, přesuňte toonext pravidlo
   4. Použít NSG pravidla 4 (IIS01 tooAppVM01), Probíhá zpracování povolených, zastavení pravidla
4. AppVM01 obdrží požadavek na hello a odpoví souboru (za předpokladu, že je autorizovaný přístup)
5. Vzhledem k tomu, že neexistují žádná odchozí pravidla v podsíti hello back-end, je povoleno hello odpovědi
6. Podsíť frontend zahájí zpracování příchozí pravidlo:
   1. Neexistuje žádné pravidlo NSG, která se použije tooInbound provozu z podsítě hello back-end podsíť toohello front-endu, tak pravidla NSG hello nepoužijí
   2. Hello výchozí systému pravidlo umožňuje provoz mezi podsítěmi umožňuje tento provoz, provoz hello je povolený.
7. server služby IIS Hello obdrží soubor hello

#### <a name="denied-web-toobackend-server"></a>(*Byl odepřen*) toobackend webu
1. Uživatel s Internetu pokusí tooaccess souboru na AppVM01 prostřednictvím hello BackEnd001.CloudApp.Net služby
2. Vzhledem k tomu, že nejsou otevřené pro sdílené složky koncové body, tato komunikace by předat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, by tento provoz blokovat pravidla NSG 5 (Internet tooVNet)

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Byl odepřen*) hledání DNS pro Web na serveru DNS
1. Uživatel s Internetu pokusí toolook až interní DNS záznam na DNS01 prostřednictvím hello BackEnd001.CloudApp.Net služby
2. Vzhledem k tomu, že nejsou otevřené pro DNS koncové body, tato komunikace by předat hello cloudové služby a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byly otevřené hello koncových bodů, pravidla NSG 5 (tooVNet Internetu) by blokovat tento provoz (Poznámka: Tento pravidlo 1 (DNS) nebude použít dvou důvodů, první zdrojové adresy hello je hello internet, toto pravidlo se vztahuje pouze na toohello virtuální místní síť jako hello také zdroje Toto pravidlo je pravidlo povolení, takže by nikdy zakážou provoz)

#### <a name="denied-web-toosql-access-through-firewall"></a>(*Byl odepřen*) webové tooSQL přístup přes bránu firewall
1. Uživatel s Internetu vyžaduje SQL data z FrontEnd001.CloudApp.Net (Internet čelí cloudové služby)
2. Vzhledem k tomu, že jsou pro SQL otevřené žádné koncové body, tento provoz by předat hello cloudové služby a nebude dosáhnout hello brány firewall
3. Pokud z nějakého důvodu byly otevřené koncových bodů, podsíť Frontend hello zahájí zpracování příchozí pravidlo:
   1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
   2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
   3. Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla
4. Provoz dotkne interní IP adresu hello IIS01 (10.0.1.5)
5. IIS01 nenaslouchá na portu 1433, takže žádný požadavek na toohello odpovědi

## <a name="conclusion"></a>Závěr
V tomto příkladu je relativně jednoduché a splněny následující způsob izolace hello back-end podsíť z příchozí přenosy.

Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].

## <a name="references"></a>Odkazy
### <a name="main-script-and-network-config"></a>Hlavní skript a síťové konfigurace
Uložte hello úplné skript v souboru skriptu prostředí PowerShell. Uložit hello konfiguraci sítě do souboru s názvem "NetworkConf1.xml."
Umožňuje změnit proměnné uživatelem definované hello jako hello potřebné a spusťte skript.

#### <a name="full-script"></a>Celý skript
Tento skript bude na základě proměnných hello uživatelem definované;

1. Připojit tooan předplatného Azure
2. vytvořit účet úložiště
3. Vytvořit virtuální síť a dvě podsítě, jak jsou definovány v souboru konfigurace sítě hello
4. Sestavení windows čtyři virtuální počítače serveru
5. Konfigurace, včetně NSG:
   * Vytvořit skupinu NSG
   * Sestavování s pravidly
   * Vazba hello NSG toohello příslušné podsítě

Tento skript prostředí PowerShell je vhodné spustit místně na Internetu připojený počítač nebo server.

> [!IMPORTANT]
> Když tento skript se spustí, může být upozornění nebo ostatní informační zprávy, které pop v prostředí PowerShell. Pouze chybové zprávy červeně jsou příčinou problém.
> 
>

```PowerShell
<# 
 .SYNOPSIS
  Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)

 .DESCRIPTION
  This script will build out a sample DMZ setup containing:
   - A default storage account for VM disks
   - Two new cloud services
   - Two Subnets (FrontEnd and BackEnd subnets)
   - One server on hello FrontEnd Subnet
   - Three Servers on hello BackEnd Subnet
   - Network Security Groups tooallow/deny traffic patterns as declared

  Before running script, ensure hello network configuration file is created in
  hello directory referenced by $NetworkConfigFile variable (or update the
  variable tooreflect hello path and file name of hello config file being used).

 .Notes
  Security requirements are different for each use case and can be addressed in a
  myriad of ways. Please be sure that any sensitive data or applications are behind
  hello appropriate layer(s) of protection. This script serves as an example of some
  of hello techniques that can be used, but should not be used for all scenarios. You
  are responsible tooassess your security needs and hello appropriate protections
  needed, and then effectively implement those protections.

  FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
   IIS01      - 10.0.1.5

  BackEnd Service (BackEnd subnet 10.0.2.0/24)
   DNS01      - 10.0.2.4
   AppVM01    - 10.0.2.5
   AppVM02    - 10.0.2.6

#>

# Fixed Variables
    $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password toobe used for all VMs"
    $VMName = @()
    $ServiceName = @()
    $VMFamily = @()
    $img = @()
    $size = @()
    $SubnetName = @()
    $VMIP = @()

# User-Defined Global Variables
  # These should be changes tooreflect your subscription and services
  # Invalid options will fail in hello validation section

  # Subscription Access Details
    $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

  # VM Account, Location, and Storage Details
    $LocalAdmin = "theAdmin"
    $DeploymentLocation = "Central US"
    $StorageAccountName = "vmstore02"

  # Service Details
    $FrontEndService = "FrontEnd001"
    $BackEndService = "BackEnd001"

  # Network Details
    $VNetName = "CorpNetwork"
    $FESubnet = "FrontEnd"
    $FEPrefix = "10.0.1.0/24"
    $BESubnet = "BackEnd"
    $BEPrefix = "10.0.2.0/24"
    $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"

  # VM Base Disk Image Details
    $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}

  # NSG Details
    $NSGName = "MyVNetSG"

# User-Defined VM Specific Configuration
    # Note: tooensure proper NSG Rule creation later in this script:
    #       - hello Web Server must be VM 0
    #       - hello AppVM1 Server must be VM 1
    #       - hello DNS server must be VM 3
    #
    #       Otherwise hello NSG rules in hello last section of this
    #       script will need toobe changed toomatch hello modified
    #       VM array numbers ($i) so hello NSG Rule IP addresses
    #       are aligned toohello associated VM IP addresses.

    # VM 0 - hello Web Server
      $VMName += "IIS01"
      $ServiceName += $FrontEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $FESubnet
      $VMIP += "10.0.1.5"

    # VM 1 - hello First Application Server
      $VMName += "AppVM01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.5"

    # VM 2 - hello Second Application Server
      $VMName += "AppVM02"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.6"

    # VM 3 - hello DNS Server
      $VMName += "DNS01"
      $ServiceName += $BackEndService
      $VMFamily += "Windows"
      $img += $SrvImg
      $size += "Standard_D3"
      $SubnetName += $BESubnet
      $VMIP += "10.0.2.4"

# ----------------------------- #
# No User-Defined Variables or  #
# Configuration past this point #
# ----------------------------- #    

  # Get your Azure accounts
    Add-AzureAccount
    Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
    Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop

  # Create Storage Account
    If (Test-AzureName -Storage -Name $StorageAccountName) { 
        Write-Host "Fatal Error: This storage account name is already in use, please pick a different name." -ForegroundColor Red
        Return}
    Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
          New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}

  # Update Subscription Pointer tooNew Storage Account
    Write-Host "Updating Subscription Pointer tooNew Storage Account" -ForegroundColor Cyan 
    Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop

# Validation
$FatalError = $false

If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
     Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
     $FatalError = $true}

If (Test-AzureName -Service -Name $FrontEndService) { 
    Write-Host "hello FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello FrontEndService service name is valid for use." -ForegroundColor Green}

If (Test-AzureName -Service -Name $BackEndService) { 
    Write-Host "hello BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello BackEndService service name is valid for use." -ForegroundColor Green}

If (-Not (Test-Path $NetworkConfigFile)) { 
    Write-Host 'hello network config file was not found, please update hello $NetworkConfigFile variable toopoint toohello network config xml file.' -ForegroundColor Yellow
    $FatalError = $true}
Else { Write-Host "hello network configuration file was found" -ForegroundColor Green
        If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
            Write-Host 'hello deployment location was not found in hello network config file, please check hello network config file tooensure hello $DeploymentLocation variable is correct and hello network config file matches.' -ForegroundColor Yellow
            $FatalError = $true}
        Else { Write-Host "hello deployment location was found in hello network config file." -ForegroundColor Green}}

If ($FatalError) {
    Write-Host "A fatal error has occurred, please see hello above messages for more information." -ForegroundColor Red
    Return}
Else { Write-Host "Validation passed, now building hello environment." -ForegroundColor Green}

# Create VNET
    Write-Host "Creating VNET" -ForegroundColor Cyan 
    Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop

# Create Services
    Write-Host "Creating Services" -ForegroundColor Cyan
    New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
    New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop

# Build VMs
    $i=0
    $VMName | Foreach {
        Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
        New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
            Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
            Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
            Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
            Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
            Remove-AzureEndpoint -Name "PowerShell" | `
            New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
            # Note: A Remote Desktop endpoint is automatically created when each VM is created.
        $i++
    }
    # Add HTTP Endpoint for IIS01
    Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM

# Configure NSG
    Write-Host "Configuring hello Network Security Group (NSG)" -ForegroundColor Cyan

  # Build hello NSG
    Write-Host "Building hello NSG" -ForegroundColor Cyan
    New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"

  # Add NSG Rules
    Write-Host "Writing rules into hello NSG" -ForegroundColor Cyan
    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
        -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP too$VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet too$($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
        -SourceAddressPrefix Internet -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) too$($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $VNetName VNet from hello Internet" -Type Inbound -Priority 140 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
        -Protocol *

    Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate hello $FESubnet subnet from hello $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
        -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
        -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
        -Protocol *

    # Assign hello NSG toohello Subnets
        Write-Host "Binding hello NSG tooboth subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName

# Optional Post-script Manual Configuration
  # Install Test Web App (Run Post-Build Script on hello IIS Server)
  # Install Backend resource (Run Post-Build Script on hello AppVM01)
  Write-Host
  Write-Host "Build Complete!" -ForegroundColor Green
  Write-Host
  Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
  Write-Host " - Install Test Web App (Run Post-Build Script on hello IIS Server)" -ForegroundColor Gray
  Write-Host " - Install Backend resource (Run Post-Build Script on hello AppVM01)" -ForegroundColor Gray
  Write-Host
```

#### <a name="network-config-file"></a>Soubor konfigurace sítě
Uložte tento soubor xml s aktualizované umístění a přidejte hello odkaz toothis soubor toohello $NetworkConfigFile proměnné v hello předcházející skriptu.

```XML
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns>
      <DnsServers>
        <DnsServer name="DNS01" IPAddress="10.0.2.4" />
        <DnsServer name="Level3" IPAddress="209.244.0.3" />
      </DnsServers>
    </Dns>
    <VirtualNetworkSites>
      <VirtualNetworkSite name="CorpNetwork" Location="Central US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="FrontEnd">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="BackEnd">
            <AddressPrefix>10.0.2.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
        <DnsServersRef>
          <DnsServerRef name="DNS01" />
          <DnsServerRef name="Level3" />
        </DnsServersRef>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

#### <a name="sample-application-scripts"></a>Ukázkové skripty aplikace
Pokud chcete tooinstall ukázková aplikace pro toto a další příklady DMZ jeden bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]

## <a name="next-steps"></a>Další kroky
* Aktualizace a uložte soubor XML
* Spusťte prostředí hello hello PowerShell skriptu toobuild
* Nainstalujte hello ukázkové aplikace
* Testování různé přenosové toky prostřednictvím této hraniční sítě

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Příchozí DMZ s NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

