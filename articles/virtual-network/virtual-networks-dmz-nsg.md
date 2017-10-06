---
title: "aaaAzure příklad DMZ – vytvoření jednoduché DMZ pomocí skupin Nsg | Microsoft Docs"
description: "Sestavení DMZ se skupinami zabezpečení sítě (NSG)"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 11c5c6026da30fbc9c5e585f5c16e2d411d6fd80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="example-1--build-a-simple-dmz-using-nsgs-with-an-azure-resource-manager-template"></a>Příklad 1 – Vytvoření jednoduché DMZ pomocí skupin Nsg pomocí šablony Azure Resource Manager
[Vrátí toohello stránku osvědčené postupy zabezpečení hranic][HOME]

> [!div class="op_single_selector"]
> * [Šablona Resource Manageru](virtual-networks-dmz-nsg.md)
> * [Classic – prostředí PowerShell](virtual-networks-dmz-nsg-asm.md)
> 
>

Tento příklad vytvoří primitivní hraniční sítě se čtyřmi servery Windows a skupiny zabezpečení sítě. Tento příklad popisuje každý hello odpovídající šablonu části tooprovide podrobnější vysvětlení jednotlivých kroků. Je zde také na provoz scénář části tooprovide podrobný podrobný rozbor toho, jak se provoz pokračuje prostřednictvím hello vrstev obrany ve hello DMZ. Nakonec v části odkazy hello je hello úplnou šablonu kódu a pokyny toobuild tento tootest prostředí a experimentů pomocí různé scénáře. 

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] 

![Příchozí DMZ s NSG][1]

## <a name="environment-description"></a>Popis prostředí
V tomto příkladu obsahuje předplatné hello následující prostředky:

* Jedna skupina prostředků
* Virtuální síť se dvěma podsítěmi; "FrontEnd" a "Back-end"
* Skupinu zabezpečení sítě, která je použitá tooboth podsítě
* Windows Server, který představuje server webových aplikací ("IIS01")
* Dva windows serverů, které představují servery back-end aplikace ("AppVM01", "AppVM02")
* Windows server, který představuje server DNS ("DNS01")
* Veřejné IP adresy přidružené k serveru webové aplikace hello

V části odkazy hello je šablony Azure Resource Manageru tooan odkaz, který sestaví hello prostředí popsané v tomto příkladu. Vytváření hello virtuálních počítačů a virtuálních sítí, i když provádí hello příklad šablony, nejsou popsané podrobně v tomto dokumentu. 

**toobuild toto prostředí** (podrobné pokyny naleznete v části odkazy hello tohoto dokumentu);

1. Nasazení šablony Azure Resource Manageru v hello: [šablon Azure rychlý start][Template]
2. Nainstalujte hello ukázkovou aplikaci v: [ukázkový skript aplikace][SampleApp]

>[!NOTE]
>tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole." První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru. Případně může být veřejnou IP adresu ke každému serveru síťový adaptér pro snazší RDP přidružena.
> 
>

Hello následující části obsahují podrobný popis hello skupinu zabezpečení sítě a jak funguje v tomto příkladu pomocí s návodem klíče řádků hello šablony Azure Resource Manageru.

## <a name="network-security-groups-nsg"></a>Skupiny zabezpečení sítě (NSG)
V tomto příkladu je skupinu NSG vytvořené a pak načtená šesti pravidla. 

>[!TIP]
>Obecně platí musí nejprve vytvořit konkrétní pravidel "Povolit" a pak poslední hello více obecná pravidla "Deny". Hello prioritou stanoví, která pravidla se vyhodnocují jako první. Jakmile provoz nenajde tooapply tooa konkrétní pravidlo, jsou vyhodnotit žádná další pravidla. Pravidla NSG můžete použít buď v hello příchozí nebo odchozí směr (z hlediska hello hello podsítě).
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

Je výchozí odchozí pravidlo, které umožňuje provozu toohello Internetu. V tomto příkladu jsme se umožňuje odchozí provoz a úprava není žádná odchozí pravidla. tooapply tootraffic zásad zabezpečení v obou směrech, uživatel definovaná směrování je povinný a je prozkoumali "Příklad 3" na hello [stránku osvědčené postupy zabezpečení hranic][HOME].

Každé pravidlo je podrobněji popsána následujícím způsobem:

1. Prostředek skupinu zabezpečení sítě musí být instancí toohold hello pravidla:

    ```JSON
    "resources": [
      {
        "apiVersion": "2015-05-01-preview",
        "type": "Microsoft.Network/networkSecurityGroups",
        "name": "[variables('NSGName')]",
        "location": "[resourceGroup().location]",
        "properties": { }
      }
    ]
    ``` 

2. první pravidlo Hello v tomto příkladu umožňuje přenosů mezi všechny server DNS toohello interní sítě v podsíti hello back-end. pravidlo Hello má některé důležité parametry:
  * "destinationAddressPrefix" - pravidla můžete použít zvláštní typ předpona adresy názvem "Výchozí značka", tyto značky jsou identifikátory poskytované systémem, které umožňují snadný způsob tooaddress vyšší kategorie předpon adres. Toto pravidlo používá hello výchozí značka "Internet" toosignify žádné adresy mimo hello virtuální sítě. Ostatní předponu značky jsou virtuální síť a AzureLoadBalancer.
  * "Směr" označuje, že ve směru toku provozu účinné toto pravidlo. Směr Hello je z hlediska hello hello podsíť nebo virtuálního počítače (v závislosti na tom, kde je tato skupina NSG vázán). Proto pokud je směr "Příchozí" a provoz vstupující hello podsíť, hello pravidlo vztahuje a odchozího provozu z podsítě hello ovlivněn tímto pravidlem.
  * "Priority" Nastaví hello pořadí, ve kterém je přenosový tok vyhodnocena. Hello nižší hello číslo hello vyšší hello prioritou. Když se pravidlo vztahuje tooa konkrétní přenosový tok, žádná další pravidla se zpracovávají. Proto pokud pravidlo s prioritou 1 umožňuje provoz a pravidlo s prioritou 2 odmítne provozu a obě pravidla použít tootraffic pak provoz hello bude mít možnost tooflow (vzhledem k tomu, že pravidlo 1 měl vyšší prioritu trvalo účinek a žádná další pravidla byly použity).
  * "Přístup" označuje, že toto pravidlo je-li blokované ("Deny") nebo povolených ("Povolit").

    ```JSON
    "properties": {
    "securityRules": [
      {
        "name": "enable_dns_rule",
        "properties": {
          "description": "Enable Internal DNS",
          "protocol": "*",
          "sourcePortRange": "*",
          "destinationPortRange": "53",
          "sourceAddressPrefix": "VirtualNetwork",
          "destinationAddressPrefix": "10.0.2.4",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
    ```

3. Toto pravidlo umožňuje tooflow provoz protokolu RDP z hello internet toohello portu RDP na libovolném serveru v hello vázaný podsítě. 

    ```JSON
    {
      "name": "enable_rdp_rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 110,
        "direction": "Inbound"
      }
    },
    ```

4. Toto pravidlo umožňuje příchozí internetové přenosy toohit hello webový server. Toto pravidlo nedojde ke změně chování směrování hello. Hello pravidlo umožňuje pouze provoz určený pro IIS01 toopass. Takže pokud provoz z Internetu hello měl hello webový server jako svůj cíl toto pravidlo by se povolit a zastavit zpracování další pravidla. (V hello pravidla s důležitostí 140 všechny ostatní příchozí internetový provoz blokováno). Pokud máte pouze zpracování přenos HTTP, může být toto pravidlo další s omezeným přístupem tooonly povolit cílový Port 80.

    ```JSON
    {
      "name": "enable_web_rule",
      "properties": {
        "description": "Enable Internet too[variables('VM01Name')]",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "10.0.1.5",
        "access": "Allow",
        "priority": 120,
        "direction": "Inbound"
        }
      },
    ```

5. Toto pravidlo umožňuje toopass provoz ze serveru IIS01 hello toohello AppVM01 serveru, novější pravidlo blokuje všechny ostatní přenosy tooBackend front-endu. tooimprove, který toto pravidlo, pokud hello port se ví, že má být přidána. Například pokud server služby IIS hello je stiskne pouze SQL Server na AppVM01, rozsah cílových portů hello by mělo být změněno z "*" (Any) too1433 (hello port služby SQL) umožňuje menší prostor pro příchozí útok na AppVM01, by měla webová aplikace hello někdy dojít k ohrožení.

    ```JSON
    {
      "name": "enable_app_rule",
      "properties": {
        "description": "Enable [variables('VM01Name')] too[variables('VM02Name')]",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "10.0.1.5",
        "destinationAddressPrefix": "10.0.2.5",
        "access": "Allow",
        "priority": 130,
        "direction": "Inbound"
      }
    },
     ```

6. Toto pravidlo odmítne provoz z hello internetové tooany servery v síti hello. S hello pravidla s důležitostí 110 a 120 hello efekt je tooallow pouze příchozí internetové přenosy toohello brány firewall a porty protokolu RDP na serverech a blokuje nic jiného. Toto pravidlo je "jistotu" pravidlo tooblock všechny neočekávané toky.

    ```JSON
    {
      "name": "deny_internet_rule",
      "properties": {
        "description": "Isolate hello [variables('VNetName')] VNet from hello Internet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Deny",
        "priority": 140,
        "direction": "Inbound"
      }
    },
     ```

7. poslední pravidlo Hello odmítne provoz z hello front-endu podsíť toohello back-end podsítě. Vzhledem k tomu, že toto pravidlo je pouze příchozí pravidlo, zpětné provoz je povolený (z back-end toohello hello front-endu).

    ```JSON
    {
      "name": "deny_frontend_rule",
      "properties": {
        "description": "Isolate hello [variables('Subnet1Name')] subnet from hello [variables('Subnet2Name')] subnet",
        "protocol": "*",
        "sourcePortRange": "*",
        "destinationPortRange": "*",
        "sourceAddressPrefix": "[variables('Subnet1Prefix')]",
        "destinationAddressPrefix": "[variables('Subnet2Prefix')]",
        "access": "Deny",
        "priority": 150,
        "direction": "Inbound"
      }
    }
    ```

## <a name="traffic-scenarios"></a>Provoz scénáře
#### <a name="allowed-internet-tooweb-server"></a>(*Povolené*) internetového tooweb serveru
1. Internetu uživatel požádá o stránku HTTP z hello veřejnou IP adresu hello seskupování přidruženého hello IIS01 seskupování
2. Hello veřejnou IP adresu předá toohello provoz virtuální sítě směrem IIS01 (hello webový server)
3. Podsíť frontend zahájí zpracování příchozí pravidlo:
  1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
  2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
  3. Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla
4. Provoz dotkne interní IP adresu hello webového serveru IIS01 (10.0.1.5)
5. IIS01 naslouchá pro webový provoz, získá tento požadavek a spustí zpracování požadavku hello
6. IIS01 hello systému SQL Server na AppVM01 vyzve k zadání informací
7. Žádná odchozí pravidla na podsíť Frontend provoz je povolený.
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
12. server služby IIS Hello obdrží odpověď hello SQL a dokončí hello odpovědi HTTP a odešle toohello žadatel
13. Vzhledem k tomu, že neexistují žádná odchozí pravidla na podsíť Frontend hello, hello odpovědi je povolen a hello Internet uživatel obdrží webovou stránku hello požadovaný.

#### <a name="allowed-rdp-tooiis-server"></a>(*Povolené*) serveru tooIIS RDP
1. Správce serveru na Internetu požadavky tooIIS01 relace protokolu RDP na hello veřejnou IP adresu hello seskupování přidruženého hello IIS01 seskupování (Tato veřejná IP adresa naleznete prostřednictvím hello portálu nebo prostředí PowerShell)
2. Hello veřejnou IP adresu předá toohello provoz virtuální sítě směrem IIS01 (hello webový server)
3. Podsíť frontend zahájí zpracování příchozí pravidlo:
  1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
  2. Použít NSG pravidlo 2 (RDP), Probíhá zpracování povolených, zastavení pravidla
4. Žádná odchozí pravidla použít výchozí pravidla a návratový provoz je povolený
5. Je povoleno relaci protokolu RDP.
6. IIS01 vyzve k zadání hello uživatelské jméno a heslo

>[!NOTE]
>tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole." První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.
>
>

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

#### <a name="denied-rdp-toobackend"></a>(*Byl odepřen*) toobackend protokolu RDP
1. Uživatel s Internetu pokusí tooRDP tooserver AppVM01
2. Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server
3. Pokud z nějakého důvodu byla povolená veřejnou IP adresu, ale by pravidla NSG 2 (RDP) povolit tento provoz

>[!NOTE]
>tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole." První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.
>
>

#### <a name="denied-web-toobackend-server"></a>(*Byl odepřen*) toobackend webu
1. Uživatel s Internetu pokusí tooaccess souboru na AppVM01
2. Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server
3. Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, by tento provoz blokovat pravidla NSG 5 (Internet tooVNet)

#### <a name="denied-web-dns-look-up-on-dns-server"></a>(*Byl odepřen*) hledání DNS pro Web na serveru DNS
1. Uživatel s Internetu pokusí toolook až interní záznam DNS na DNS01
2. Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server
3. Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, pravidla NSG 5 (tooVNet Internetu) by blokovat tento provoz (Poznámka: aby pravidlo 1 (DNS) nebude použít, protože hello požadavky zdrojové adresy je hello internet a pravidla 1 uplatňuje se pouze toohello virtuální místní síť jako zdroj hello)

#### <a name="denied-sql-access-on-hello-web-server"></a>(*Byl odepřen*) SQL přístup na webový server hello
1. Uživatel s Internetu vyžaduje SQL data z IIS01
2. Vzhledem k tomu, že nejsou žádné veřejné IP adresy přidružené k této servery síťový adaptér, tato komunikace by nikdy zadejte hello virtuální sítě a nebude kontaktovat hello server
3. Pokud z nějakého důvodu bylo povolené veřejnou IP adresu, podsíť Frontend hello zahájí zpracování příchozí pravidlo:
  1. Netýká NSG pravidlo 1 (DNS), přesunete toonext pravidlo
  2. Pravidla NSG 2 (RDP) netýká, přesuňte toonext pravidlo
  3. Použít NSG pravidla 3 (tooIIS01 Internetu), Probíhá zpracování povolených, zastavení pravidla
4. Provoz dotkne interní IP adresu hello IIS01 (10.0.1.5)
5. IIS01 nenaslouchá na portu 1433, takže žádný požadavek na toohello odpovědi

## <a name="conclusion"></a>Závěr
V tomto příkladu je relativně jednoduché a splněny následující způsob izolace hello back-end podsíť z příchozí přenosy.

Další příklady a přehled hranice zabezpečení sítě najdete [sem][HOME].

## <a name="references"></a>Odkazy
### <a name="azure-resource-manager-template"></a>Šablona Azure Resource Manageru
Tento příklad používá šablonu Azure Resource Manager předdefinované v úložišti GitHub spravován společností Microsoft a otevřete toohello komunity. Tuto šablonu můžete nasazovat přímo z Githubu, nebo stáhli a upravili toofit vašim potřebám. 

Hlavní šablona Hello je v souboru hello s názvem "azuredeploy.json." Tato šablona jde odeslat prostřednictvím prostředí PowerShell nebo rozhraní příkazového řádku (s soubor přidružené "azuredeploy.parameters.json" hello) toodeploy této šablony. Najít hello nejjednodušší způsob je toouse hello tlačítko "Nasadit tooAzure" na stránce README.md hello v Githubu.

toodeploy hello šablonu, která vytvoří tento příklad z Githubu a hello portál Azure, postupujte takto:

1. V prohlížeči přejděte toohello [šablony][Template]
2. Klikněte na tlačítko "Nasadit tooAzure" hello (nebo hello "Vizualizovat" tlačítko toosee grafické reprezentace této šablony)
3. Zadejte v okně parametry hello hello účet úložiště, uživatelské jméno a heslo a potom klikněte na tlačítko **OK**
5. Vytvořte skupinu prostředků pro toto nasazení (můžete použít existující šablonu, ale I doporučujeme novou nejlepších výsledků dosáhnete)
6. V případě potřeby změňte hello předplatném a umístění nastavení pro virtuální síť.
7. Klikněte na tlačítko **přečíst si právní podmínky**, přečtěte si podmínky hello a klikněte na tlačítko **nákupu** tooagree.
8. Klikněte na tlačítko **vytvořit** toobegin hello nasazení této šablony.
9. Po nasazení hello skončí úspěšně, přejděte toohello, který vytvořili skupinu prostředků pro toto nasazení prostředky hello toosee nakonfigurované uvnitř.

>[!NOTE]
>Tato šablona umožňuje RDP toohello IIS01 pouze server (hello Najít veřejnou IP adresu pro IIS01 na hello portálu). tooRDP tooany back-end serverů v této instanci serveru IIS hello slouží jako "jump pole." První server služby IIS toohello RDP a potom z hello RDP serveru IIS toohello back-end serveru.
>
>

tooremove tato nasazení, hello odstranit skupinu prostředků a všechny podřízené prostředky budou také odstraněny.

#### <a name="sample-application-scripts"></a>Ukázkové skripty aplikace
Po úspěšném spuštění hello šablony můžete nastavit hello webového serveru a aplikačního serveru s tooallow jednoduché webové aplikace testování s touto konfigurací hraniční sítě. tooinstall ukázkové aplikace pro toto a další příklady hraniční sítě, jednu bylo zadáno v hello následující odkaz: [ukázkový skript aplikace][SampleApp]

## <a name="next-steps"></a>Další kroky

* Tento příklad nasazení
* Vytvoření ukázkové aplikace hello
* Testování různé přenosové toky prostřednictvím této hraniční sítě

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-arm/example1design.png "Příchozí DMZ s NSG"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[Template]: https://github.com/Azure/azure-quickstart-templates/tree/master/301-dmz-nsg
[SampleApp]: ./virtual-networks-sample-app.md