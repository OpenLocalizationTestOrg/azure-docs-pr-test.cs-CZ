---
title: "tooOMS aaaConnect počítače pomocí hello OMS brány | Microsoft Docs"
description: "Připojení zařízení spravovaná OMS a nástroje Operations Manager monitorované počítače s hello OMS služba brány pro toosend data toohello OMS při nemají přístup k Internetu."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: magoedte
ms.openlocfilehash: 0cfa8f2fb66016e494f22c780e328be472b5fdee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-computers-without-internet-access-toooms-using-hello-oms-gateway"></a>Připojte počítače bez tooOMS přístup k Internetu pomocí hello OMS brány

Tento dokument popisuje, jak vaše OMS řízená a System Center Operations Manager monitorované počítače může odesílat data toohello OMS služby při nemají přístup k Internetu. Hello OMS brána, která dopředného proxy HTTP, který podporuje tunelování HTTP pomocí příkazu HTTP připojit hello můžete shromažďovat data a poslat toohello OMS služby jejich jménem.  

Hello OMS brána podporuje:

* Procesy služby Azure Automation Hybrid Runbook Worker  
* Počítače se systémem Windows s hello agenta Microsoft Monitoring Agent přímo připojené pracovním prostorem OMS tooan
* Počítače se systémem Linux s hello OMS agenta pro Linux přímo připojené pracovním prostorem OMS tooan  
* System Center Operations Manager 2012 SP1 s kumulativní aktualizací 7, Operations Manager 2012 R2 s UR3 nebo Operations Manager 2016 skupiny pro správu, integrované s OMS.  

Pokud vaše zásady zabezpečení IT neumožňují počítačů ve vaší síti tooconnect toohello Internetu, jako je například bod zařízení POS (POS) nebo serverech podporujících IT služeb, ale musíte tooconnect je tooOMS toomanage a monitorovat je, může být nakonfigurovány toocommunicate přímo s konfigurace tooreceive hello OMS brány a předávání dat jejich jménem.  Pokud jsou tyto počítače nakonfigurované s toodirectly agenta OMS hello připojte pracovním prostorem OMS tooan všechny počítače se místo toho komunikovat s hello OMS brány.  Brána Hello přenosu dat z agentů tooOMS hello přímo, nebudou analyzované žádné hello dat během přenosu.

Pokud skupinu správy nástroje Operations Manager je spojen s OMS, servery pro správu hello můžete být nakonfigurované tooconnect toohello informace o konfiguraci brány OMS tooreceive a poslat shromážděná data v závislosti na hello řešení, které jste povolili.  Agenti nástroje Operations Manager odesílat některá data, například výstrahy nástroje Operations Manager, konfigurace assessment, prostoru instancí a server pro správu kapacity datového toohello. Další velkých objemů dat, například protokoly služby IIS, výkonu a události zabezpečení jsou odesílány přímo toohello OMS brány.  Pokud máte jeden nebo více serverů brány nástroje Operations Manager nasazena v hraniční sítě nebo jiné toomonitor izolovanou síť nedůvěryhodné systémy, nemůže komunikovat s bránu OMS.  Servery nástroje Operations Manager brány lze pouze sestavy tooa serveru pro správu.  Pokud skupinu správy nástroje Operations Manager je nakonfigurované toocommunicate s hello OMS brány, hello informace o konfiguraci proxy serveru je automaticky distribuován tooevery počítač spravovaný agentem, který je nakonfigurovaný toocollect data pro analýzu protokolu i v případě, nastavení Hello je prázdný.    

tooprovide vysoká dostupnost pro přímé připojení nebo Operations Management skupin, které komunikují s OMS prostřednictvím hello brány, můžete použít tooredirect Vyrovnávání zatížení sítě a distribuovat hello přenosů mezi několik serverů brány.  Pokud jeden server brány přestane fungovat, je hello přenosy přesměrované tooanother uzel k dispozici.  

Doporučujeme nainstalovat agenta OMS hello hello počítače se systémem hello OMS brány softwaru toomonitor hello OMS brány a analyzovat data výkonu nebo události. Kromě toho hello agenta pomáhá hello OMS brány identifikovat hello koncovým bodům služby, které je nutné toocommunicate s.

Každý agent, musí mít bránu tooits připojení sítě tak, aby agenty lze automaticky převést data tooand z brány hello. Instalaci hello brány na řadiči domény se nedoporučuje.

Hello následující diagram znázorňuje tok dat z přímého agenty tooOMS pomocí serveru brány hello.  Agenti musí mít jejich shodu konfigurace proxy hello stejný port hello OMS brána je nakonfigurovaná toocommunicate s tooOMS.  

![agent přímé komunikaci s OMS diagram](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Hello následující diagram znázorňuje tok dat ze tooOMS skupiny správy nástroje Operations Manager.   

![Operations Manager komunikace s OMS diagram](./media/log-analytics-oms-gateway/oms-omsgateway-opsmgrconnect.png)

## <a name="prerequisites"></a>Požadavky

Při určování hello toorun na počítači brány OMS, musí mít tento počítač hello následující:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* Rozhraní .net framework 4.5
* Minimálně 4 jádra procesoru a 8 GB paměti

### <a name="language-availability"></a>Jazyk dostupnosti

Hello OMS brány je k dispozici v hello následující jazyky:

- Čínština (zjednodušená)
- Čínština (tradiční)
- čeština
- holandština
- Angličtina
- francouzština
- němčina
- maďarština
- italština
- japonština
- korejština
- polština
- Portugalština (Brazílie)
- Portugalština (Portugalsko)
- ruština
- Španělština (mezinárodní)

## <a name="download-hello-oms-gateway"></a>Stáhnout hello OMS brány

Existují tři způsoby tooget hello nejnovější verzi hello OMS brány instalační soubor.

1. Stáhnout z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Stáhněte si z portálu OMS hello.  Po přihlášení pracovním prostorem OMS tooyour přejděte příliš**nastavení** > **připojené zdroje** > **servery Windows** a klikněte na tlačítko **Stáhnout brány OMS**.

3. Stáhnout z hello [portál Azure](https://portal.azure.com).  Po přihlášení:  

   1. Procházet hello seznam služeb a pak vyberte **analýzy protokolů**.  
   2. Vyberte pracovní prostor.
   3. V okně prostoru v části **Obecné**, klikněte na tlačítko **rychlý Start**.
   4. V části **vyberte pracovní prostor datový zdroj tooconnect toohello**, klikněte na tlačítko **počítače**.
   5. V hello **přímé agenta** okně klikněte na tlačítko **stáhnout brány OMS**.<br><br> ![Stáhněte si OMS brány](./media/log-analytics-oms-gateway/download-gateway.png)


## <a name="install-hello-oms-gateway"></a>Nainstalujte hello OMS brány

tooinstall bránu, proveďte následující kroky hello.  Pokud jste nainstalovali předchozí verze, dříve se označovaly jako *Log Analytics předávání*, bude upgradována toothis verze.  

1. Z hello cílovou složku, klikněte dvakrát na **OMS Gateway.msi**.
2. Na hello **úvodní** klikněte na tlačítko **Další**.<br><br> ![Průvodce instalací brány](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br>
3. Na hello **licenční smlouvy** vyberte **hello podmínkami hello licenční smlouvy souhlasím** tooagree toohello smlouvy EULA a pak klikněte na **Další**.
4. Na hello **Port a proxy adres** stránky:
   1. Typ hello TCP port číslo toobe použít pro bránu hello. Instalační program nakonfiguruje příchozí pravidlo s tímto číslem portu v bráně Windows firewall.  Hello výchozí hodnota je 8080.
      platný rozsah Hello hello číslo portu je 1 – 65 535. Pokud hello vstup do tohoto rozsahu nespadá, zobrazí se chybová zpráva.
   2. Volitelně Pokud hello serveru, kde hello brána je nainstalovaná potřebám toocommunicate prostřednictvím proxy serveru, zadejte adresu proxy serveru hello, kde musí tooconnect hello brána. Například, `http://myorgname.corp.contoso.com:80`.  Bude-li prázdné, brány hello pokusí tooconnect toohello Internet přímo.  Pokud proxy server vyžaduje ověřování, zadejte uživatelské jméno a heslo.<br><br> ![Konfigurace proxy serveru brány Průvodce](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. Klikněte na **Další**.
5. Pokud nemáte povolenu službu Microsoft Update, zobrazí se stránka Microsoft Update hello kde si můžete vybrat tooenable ho. Proveďte výběr a potom klikněte na **Další**. Jinak pokračujte dalším krokem toohello.
6. Na hello **cílovou složku** stránky, nechte hello výchozí brána Files\OMS C:\Program nebo typ hello umístění složky kam chcete bránu tooinstall a pak klikněte na tlačítko **Další**.
7. Na hello **připraven tooinstall** klikněte na tlačítko **nainstalovat**. Řízení uživatelských účtů, může se objevit žádajícího tooinstall oprávnění. Pokud ano, klikněte na tlačítko **Ano**.
8. Po dokončení instalace klikněte na tlačítko **Dokončit**. Můžete ověřit, že služba hello používá tak, že otevřete modul snap-in services.msc hello a ověřte, že **OMS brány** se zobrazí v hello seznam služeb a jeho stav se **systémem**.<br><br> ![Services – OMS brány](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Konfigurace služby Vyrovnávání zatížení sítě
Můžete nakonfigurovat hello brány pro vysokou dostupnost, pomocí služby Vyrovnávání zatížení sítě (NLB) pomocí Microsoft Network Load Balancing (NLB) nebo nástroje pro vyrovnávání zatížení založené na hardwaru.  Hello nástroj pro vyrovnávání zatížení spravuje přenosy přesměrováním hello požadovaný připojení z hello OMS agentů nebo serverů pro správu nástroje Operations Manager mezi jeho uzlů. Pokud jeden server brány ocitne mimo provoz, provoz hello získá přesměrovaného tooother uzlů.

toolearn jak toodesign a nasazení clusteru služby Vyrovnávání zatížení sítě systému Windows Server 2016, najdete v části [Vyrovnávání zatížení sítě](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Hello následující kroky popisují, jak tooconfigure síť Microsoft clusteru vyrovnávání zatížení.  

1.  Přihlaste se na server Windows hello, který je členem clusteru programu NLB hello k účtu správce.  
2.  Ve Správci serveru otevřete Správce vyrovnávání zatížení sítě, klikněte na tlačítko **nástroje**a potom klikněte na **Správce vyrovnávání zatížení sítě**.
3. Klikněte pravým tlačítkem na IP adresu clusteru hello tooconnect serveru služby Brána OMS s hello Microsoft Monitoring Agent nainstalován a pak klikněte na **přidat hostitele tooCluster**.<br><br> ![Správce vyrovnávání zatížení sítě – přidání tooCluster hostitele](./media/log-analytics-oms-gateway/nlb02.png)<br>
4. Zadejte IP adresu hello hello serveru brány, které chcete tooconnect.<br><br> ![Správce vyrovnávání zatížení sítě – přidání hostitele tooCluster: připojení](./media/log-analytics-oms-gateway/nlb03.png)

## <a name="configure-oms-agent-and-operations-manager-management-group"></a>Konfigurace agenta OMS a skupiny pro správu nástroje Operations Manager
Hello následující část obsahuje kroky na tom, jak tooconfigure přímo připojené OMS agentů, skupinu pro správu nástroje Operations Manager nebo procesy Azure Automation Hybrid Runbook Worker s hello OMS brány toocommunicate s OMS.  

toounderstand požadavky a kroky na tom, jak tooinstall hello agenta OMS na počítačích s Windows přímým připojením tooOMS, najdete v části [tooOMS počítače připojit Windows](log-analytics-windows-agents.md) nebo Linux počítačů najdete v tématu [počítače se systémem Linux připojení tooOMS](log-analytics-linux-agents.md).

### <a name="configuring-hello-oms-agent-and-operations-manager-toouse-hello-oms-gateway-as-a-proxy-server"></a>Konfigurace nástroje Operations Manager toouse hello OMS brány a hello OMS agent jako proxy server

### <a name="configure-standalone-oms-agent"></a>Konfigurace agenta OMS samostatné
V tématu [nakonfigurovat nastavení proxy a firewall pomocí hello agenta Microsoft Monitoring Agent](log-analytics-proxy-firewall.md) informace o konfiguraci toouse agenta proxy server, který v tomto případě je hello brány.  Pokud jste nasadili několik serverů brány za službou Vyrovnávání zatížení sítě, je konfigurace proxy serveru agenta OMS hello hello virtuální IP adresy hello Vyrovnávání zatížení sítě:<br><br> ![Microsoft Monitoring Agent vlastnosti – nastavení proxy serveru](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager---all-agents-use-hello-same-proxy-server"></a>Konfigurace nástroje Operations Manager – všechny agenty použijte hello stejný server proxy
Nakonfigurujete server brány hello tooadd Operations Manager.  Hello nástroje Operations Manager konfigurace proxy serveru je automaticky použijí tooall agenti reporting tooOperations správce, i v případě nastavení hello je prázdný.

toouse hello brány toosupport nástroje Operations Manager, musíte mít:

* Microsoft Monitoring Agent (verze agenta – **8.0.10900.0** a novější) na serveru brány hello nainstalovaný a nakonfigurovaný pro hello OMS pracovních prostorů, se kterými chcete toocommunicate.
* Hello brány musí mít připojení k Internetu, nebo připojené tooa proxy server, který nemá.

> [!NOTE]
> Pokud nezadáte hodnotu hello brány, jsou prázdné hodnoty nabídnutých tooall agenty.


1. Otevřete hello Konzola nástroje Operations Manager a v části **Operations Management Suite**, klikněte na tlačítko **připojení** a pak klikněte na **nakonfigurovat Proxy Server**.<br><br> ![Nástroje Operations Manager – nakonfigurujte Proxy Server](./media/log-analytics-oms-gateway/scom01.png)<br>
2. Vyberte **použít proxy server tooaccess hello Operations Management Suite** a pak zadejte hello IP adresu serveru brány OMS hello nebo virtuální IP adresy hello Vyrovnávání zatížení sítě. Ujistěte se, že začínáte s hello `http://` předponu.<br><br> ![Nástroj Operations Manager – adresu proxy serveru.](./media/log-analytics-oms-gateway/scom02.png)<br>
3. Klikněte na **Dokončit**. Pracovní prostor OMS připojené tooyour je váš server nástroje Operations Manager.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Konfigurace nástroje Operations Manager – konkrétní agenty použít proxy server
Pro velká nebo složitá prostředí můžete pouze konkrétní servery (nebo skupiny) toouse hello server brány OMS.  Pro tyto servery nelze aktualizovat hello agenta přímo jako tato hodnota se přepíše hello globální hodnota hello pro skupinu pro správu nástroje Operations Manager.  Místo toho musíte toooverride hello pravidlo používá toopush tyto hodnoty.

> [!NOTE]
> Tento stejný postup konfigurace může být použit tooallow hello použití více serverů brány OMS ve vašem prostředí.  Může například vyžadovat konkrétní toobe servery brány OMS zadaný na základě podle oblasti.

1. Otevřete hello Konzola nástroje Operations Manager a vyberte hello **vytváření** pracovního prostoru.  
2. V pracovním prostoru vytváření obsahu hello, vyberte **pravidla** a klikněte na tlačítko hello **oboru** tlačítka na panelu nástrojů nástroje Operations Manager hello. Pokud toto tlačítko není k dispozici, zkontrolujte, zda máte vybrán objekt, a nikoliv složka, v podokně monitorování hello toomake. Hello **obor objektů sady Management Pack** dialogové okno zobrazí seznam běžných cílové třídy, skupiny nebo objekty.
3. Typ **služba Health Service** v hello **vyhledejte** pole a vyberte ji ze seznamu hello.  Klikněte na **OK**.  
4. Vyhledejte pravidlo hello **pravidlo nastavení proxy serveru Advisor** a hello nástrojů kontroly Operations console, klikněte na **přepsání** a potom příliš**přepsání hello Rule\For konkrétní objekt třídy: stavu Služba** a hello seznamu vyberte určitý objekt.  Volitelně můžete vytvořit vlastní skupiny obsahující objekt služby stavu hello hello serverů chcete tooapply tato přepsání tooand pak použít hello přepsání toothat skupiny.
5. V hello **vlastnosti přepsání** dialogovém okně klikněte na tooplace zaškrtnutí hello **přepsat** toohello další sloupec **WebProxyAddress** parametr.  V hello **hodnotu přepsání** pole, zadejte adresu URL hello hello OMS brány serveru zajistit začínající hello `http://` předponu.
   >[!NOTE]
   > Jak je již spravován automaticky přepsáním obsažené v hello Microsoft System Center Advisor zabezpečené referenční Override sady management pack cílení hello monitorování skupiny serveru serveru Microsoft System Center Advisor nepotřebujete tooenable hello pravidlo.
   >
6. Buď vyberte sadu management pack z hello **vyberte cílovou sadu management pack** seznamu nebo vytvořte novou sadu management pack kliknutím **nový**.
7. Po dokončení změny klikněte na tlačítko **OK**.

### <a name="configure-for-automation-hybrid-workers"></a>Konfigurace pro automatizaci hybridní pracovní procesy
Pokud máte automatizace procesů Hybrid Runbook Worker ve vašem prostředí, poskytují hello kroků ruční, dočasné řešení tooconfigure hello brány toosupport je.

Hello následující kroky je nutné tooknow hello oblast Azure, ve kterém se nachází hello účet Automation. toolocate hello umístění:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Vyberte služby Azure Automation hello.
3. Vyberte příslušný účet Azure Automation hello.
4. Zobrazit jeho oblasti pod **umístění**.<br><br> ![Portál Azure – umístění účtu Automation.](./media/log-analytics-oms-gateway/location.png)  

Použijte následující adresu URL hello tooidentify tabulky pro každé umístění hello:

**Úloha adresy URL služby dat za běhu**

| **location** | **ADRESA URL** |
| --- | --- |
| Střed USA – sever |ncus-jobruntimedata produkčnímu su1.azure-automation.net |
| Západní Evropa |we-jobruntimedata-prod-su1.azure-automation.net |
| Střed USA – jih |scus-jobruntimedata-prod-su1.azure-automation.net |
| Východní USA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Střední Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Severní Evropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Jihovýchodní Asie |sea-jobruntimedata-prod-su1.azure-automation.net |
| Střed Indie |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japonsko |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Austrálie |ase-jobruntimedata-prod-su1.azure-automation.net |

**Adresy URL služby agenta**

| **location** | **ADRESA URL** |
| --- | --- |
| Střed USA – sever |ncus-agentservice produkčnímu 1.azure-automation.net |
| Západní Evropa |budeme agentservice produkčnímu 1.azure-automation.net |
| Střed USA – jih |scus-agentservice produkčnímu 1.azure-automation.net |
| Východní USA 2 |eus2-agentservice produkčnímu 1.azure-automation.net |
| Střední Kanada |kopie – agentservice produkčnímu 1.azure-automation.net |
| Severní Evropa |Ne – agentservice produkčnímu 1.azure-automation.net |
| Jihovýchodní Asie |SEA-agentservice produkčnímu 1.azure-automation.net |
| Střed Indie |CID-agentservice produkčnímu 1.azure-automation.net |
| Japonsko |JPE-agentservice produkčnímu 1.azure-automation.net |
| Austrálie |App Service Environment agentservice produkčnímu 1.azure-automation.net |

Pokud váš počítač je registrován jako hybridní pracovní proces Runbooku automaticky pro opravy pomocí řešení pro správu aktualizací hello, postupujte takto:

1. Přidáte hello úlohy běhová Data služby adresy URL toohello povolené hostitele seznam na hello OMS brány. Příklad: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Restartujte službu brány OMS hello pomocí hello následující rutiny prostředí PowerShell:`Restart-Service OMSGatewayService`

Pokud je počítač v zahrnuté tooAzure automatizace pomocí rutiny registrace hello hybridní pracovní proces Runbooku, postupujte takto:

1. Přidáte seznam povolené hostitele toohello registrace hello agenta služby adres URL na hello OMS brány. Příklad: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Přidáte hello úlohy běhová Data služby adresy URL toohello povolené hostitele seznam na hello OMS brány. Příklad: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Restartujte službu brány OMS hello.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Užitečné rutiny prostředí PowerShell
Rutiny můžete dokončit úkoly, které jsou potřebné tooupdate hello OMS brány nastavení konfigurace. Předtím, než je použijete, nezapomeňte:

1. Nainstalujte hello OMS brány (MSI).
2. Otevřete okno konzoly prostředí PowerShell.
3. modul hello tooimport, zadejte tento příkaz:`Import-Module OMSGateway`
4. Pokud v předchozím kroku hello nedošlo k žádné chybě, hello modul byl úspěšně importován a jde použít rutiny hello. Typ`Get-Module OMSGateway`
5. Když provedete změny pomocí rutin hello, ujistěte se, restartování služby brány hello.

Modul hello nebyl importován, pokud dojde k chybě v kroku 3. Hello chybě může dojít, když prostředí PowerShell je nelze toofind hello modul. Najdete ho v instalační cestě hello brány: *C:\Program Files\Microsoft OMS Gateway\PowerShell*.

| **Rutiny** | **Parametry** | **Popis** | **Příklad** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Klíč |Získá hello konfigurace služby hello |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Klíč (povinné) <br> Hodnota |Hello změny konfigurace služby hello |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Získá adresu hello předávání přes proxy (nadřazený) |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adresa<br> Uživatelské jméno<br> Heslo |Nastaví hello adresy (a přihlašovací údaje) předávání přes proxy (nadřazený) |1. Nastavení proxy serveru pro předávání a přihlašovací údaje:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Nastavte předávání přes proxy server, který nevyžaduje ověření:`Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Nastavení proxy serveru předávání zrušte hello:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Získá hello hostitele aktuálně povolené (pouze místně nakonfigurované povolené hostitele, hello nezahrnuje automaticky stažené povolené hostitele) |`Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedHost` |Hostitel (povinné) |Přidá hello hostitele toohello seznamu povolených aplikací |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Hostitel (povinné) |Odebere hostitele hello hello seznamu povolených aplikací |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Předmět (povinné) |Přidá hello klientský certifikát subjektu toohello seznamu povolených aplikací |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Předmět (povinné) |Odebere předmětu certifikátu klienta hello hello seznamu povolených aplikací |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Získá hello aktuálně povolená klienta předměty certifikátu (pouze místně hello nakonfigurované povolené témata, nezahrnuje povolené témata automaticky stažené) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Řešení potíží
toocollect události zapsané podle hello brány, budete potřebovat tooalso nainstalován agent OMS hello.<br><br> ![Prohlížeč událostí – v protokolu brány OMS](./media/log-analytics-oms-gateway/event-viewer.png)

**ID událostí brány OMS a popisy**

Hello následující tabulka uvádí hello ID událostí a popisy pro OMS brány protokolu událostí.

| **ID** | **Popis** |
| --- | --- |
| 400 |Chyby aplikace, která nemá specifické ID |
| 401 |Chybná konfigurace. Příklad: listenPort = "text" místo celé číslo |
| 402 |Výjimka při analýze TLS handshake zprávy |
| 403 |Došlo k chybě sítě. Příklad: Nelze se připojit tootarget server |
| 100 |Obecné informace |
| 101 |Služba byla spuštěna |
| 102 |Služba byla zastavena |
| 103 |Příkaz připojení protokolu HTTP přijatých od klienta |
| 104 |Není připojení protokolu HTTP příkaz |
| 105 |Cílový server se nenachází v seznamu povolených nebo hello cílový port není zabezpečený port (443) <br> <br> Ujistěte se, že agent MMA hello na serveru brány a agenty hello komunikaci s hello brány jsou připojené toohello stejný pracovní prostor analýzy protokolů. |
| 105 |Chyba TcpConnection – neplatný klientský certifikát: CN = brány <br><br> Zajistěte, aby: <br>    <br> &#149; Používáte bránu s číslem verze 1.0.395.0 nebo vyšší. <br> &#149; Hello agenta MMA na serveru brány a agenty hello komunikaci s hello brány jsou připojené toohello stejný pracovní prostor analýzy protokolů. |
| 106 |Z jakéhokoli důvodu, kterou je relace TLS hello podezřelé a odmítnutých |
| 107 |relace protokolu TLS Hello byla ověřena. |

**Toocollect čítače výkonu**

Hello následující tabulka uvádí hello čítačů výkonu, které jsou k dispozici pro hello OMS brány. Můžete přidat čítače hello pomocí sledování výkonu.

| **Název** | **Popis** |
| --- | --- |
| Připojení klienta brány/aktivní OMS |Počet připojení active klientské sítě (TCP) |
| Počet brány nebo chyb OMS |Počet chyb |
| OMS brány nebo připojení klienta |Počet připojených klientů |
| Počet brány nebo odmítání OMS |Počet zamítnutí z důvodu chyby ověření tooany TLS |

![Čítače výkonu OMS brány](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Získejte pomoc
Když jste toohello přihlášeného portálu Azure, můžete vytvořit žádost o pomoc s hello OMS brány nebo jiné služby Azure nebo součástí služby.
Klikněte na symbol otazník hello v hello pravém horním rohu portálu hello toorequest pomoc a potom klikněte na **nová žádost o podporu**. Dokončete hello nový formulář žádosti o podporu.

![Nová žádost o podporu](./media/log-analytics-oms-gateway/support.png)

Můžete také ponechat zpětnou vazbu o OMS nebo analýzy protokolů v hello [fóru pro zpětnou vazbu Microsoft Azure](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Další kroky
* [Přidat zdroje dat](log-analytics-data-sources.md) toocollect data z hello připojené zdroje v pracovním prostoru OMS a ukládá je v úložišti OMS hello.
