---
title: "dostupnost aaaHigh s Brána pro správu dat v Azure Data Factory | Microsoft Docs"
description: "Tento článek vysvětluje, jak můžete škálovat Brána pro správu dat tak, že přidáte další uzly a škálování až zvýšením počtu souběžných úloh, které můžou běžet na uzlu."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: abnarain
ms.openlocfilehash: 925f63728e23596bca2655636f6535b509fce0b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway---high-availability-and-scalability-preview"></a>Brána pro správu dat – vysokou dostupnost a škálovatelnost (Preview)
Tento článek vám pomůže nakonfigurovat vysokou dostupnost a škálovatelnost řešení s Brána pro správu dat.    

> [!NOTE]
> Tento článek předpokládá, že jste již obeznámeni s základy používání služby Brána pro správu dat. Pokud si nejste, přečtěte si téma [Brána pro správu dat](data-factory-data-management-gateway.md).

>**Tato funkce preview je oficiálně podporované na 2.12.xxxx.x verze brány pro správu dat a vyšší**. Zkontrolujte, zda používáte verzi 2.12.xxxx.x nebo vyšší. Stáhněte nejnovější verzi brány pro správu dat hello [zde](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="overview"></a>Přehled
Můžete přidružit brány správy dat, které jsou nainstalovány ve více počítačích na místě s jednou bránou logické z portálu hello. Tyto počítače se označují jako **uzly**. Může mít příliš**čtyři uzly** přidružený logické brány. Hello s více uzly (místní počítače s nainstalovanou bránu) pro logické brány výhody:  

- Zlepšení výkonu přesun dat mezi místními a cloudovými datová úložiště.  
- Pokud jeden z uzlů hello ocitne mimo provoz z nějakého důvodu, jsou stále k dispozici pro přesun dat hello dalších uzlů. 
- Pokud jeden z uzlů hello potřebovat toobe do režimu offline kvůli údržbě, jsou stále k dispozici pro přesun dat hello dalších uzlů.

Můžete také nakonfigurovat hello počet **úlohy Přesun souběžných datového** , můžete spustit na uzlu tooscale až hello funkce přesouvání dat mezi místními a cloudovými datová úložiště. 

Pomocí hello portálu Azure, můžete monitorovat stav hello tyto uzly, který vám pomůže zjistit, zda tooadd nebo odebrání uzlu z hello logické brány. 

## <a name="architecture"></a>Architektura 
Hello následující obrázek poskytuje přehled architektury hello škálovatelnosti a dostupnosti funkce hello Brána pro správu dat: 

![Brána pro správu dat – vysoké dostupnosti a škálovatelnosti](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-high-availability-and-scalability.png)

A **logické brány** je hello brány přidat objekt pro vytváření dat tooa v hello portálu Azure. Dříve může přidružíte jenom jeden počítač Windows místní brána pro správu dat nainstalované s logické brány. To místního počítače brány se nazývá uzel. Nyní můžete přiřadit až příliš**čtyři fyzické uzly** s logické brány. Je volána logické brány s několika uzly **několika uzly brány**.  

Všechny tyto uzly jsou **active**. Všechny může zpracovat data toomove úloh přesun dat mezi místními a cloudovými datová úložiště. Jeden z uzlů hello fungují jako dispečera a pracovního procesu. Jiné uzly v hello skupiny jsou uzlů pracovního procesu. A **dispečera** uzel vrátí data přesun úloh a úloh z hello cloudové služby a odešle je tooworker uzly (včetně samotného). A **pracovní** uzlu provede data toomove úloh přesun dat mezi místními a cloudovými datová úložiště. Jsou všechny uzly pracovních procesů. Jenom jeden uzel může být odesílání a pracovního procesu.    

Obvykle můžete začít s jedním uzlem a **škálovat** tooadd další uzly jako hello existující uzly jsou zahlcen zatížení přesun dat hello. Můžete také **škálovat** hello funkce přesun dat v uzlu brána zvýšením hello počet souběžných úloh, které jsou povoleny toorun na uzlu hello. Tato funkce je také dostupná s jedním uzlem bránou (i když není povolená funkce škálovatelnost a dostupnost hello). 

Bránu s více uzly udržuje přihlašovací údaje k úložišti dat hello synchronizace pro všechny uzly. Pokud nastane problém s připojením mezi uzly, může být synchronizován hello přihlašovací údaje. Když nastavíte přihlašovací údaje pro úložiště dat místně, který používá bránu, uloží pověření na hello dispečera nebo pracovního uzlu. Hello dispečera uzlu synchronizace s ostatními uzly pracovního procesu. Tento proces se označuje jako **pověření synchronizace**. může být hello komunikační kanál mezi uzly **šifrované** pomocí veřejného certifikátu SSL/TLS. 

## <a name="set-up-a-multi-node-gateway"></a>Nastaví bránu několika uzly
V této části se předpokládá, že jste prošli hello následující dva články nebo neznáte koncepty v těchto článcích: 

- [Brána pro správu dat](data-factory-data-management-gateway.md) -poskytuje podrobný přehled o hello brány.
- [Přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) – obsahuje návod podrobné pokyny pro bránu pomocí jednoho uzlu.  

> [!NOTE]
> Před instalací Brána pro správu dat na systému Windows na místním počítači, najdete v části předpoklady uvedené v [hello hlavní článek](data-factory-data-management-gateway.md#prerequisites).

1. V hello [návod](data-factory-move-data-between-onprem-and-cloud.md#create-gateway), při vytvoření logické brány, povolit hello **vysokou dostupnost a škálovatelnost** funkce. 

    ![Brána pro správu dat - povolit vysokou dostupnost a škálovatelnost](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-enable-high-availability-scalability.png)
2. V hello **konfigurace** stránky, použijte buď **Expresní instalace** nebo **ruční instalace** odkaz tooinstall brány na prvním uzlu hello (se počítač Windows místní).

    ![Brána pro správu dat – express nebo ruční instalace](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-express-manual-setup.png)

    > [!NOTE]
    > Pokud použijete možnost Expresní instalace hello, komunikace mezi uzly hello provádí bez šifrování. Název uzlu Hello je stejný jako název počítače hello. Pokud komunikaci mezi uzly hello musí toobe šifrovat nebo chcete toospecify název uzlu podle vaší volby, použijte ruční instalaci. Názvy nelze upravit později.
3. Pokud se rozhodnete **Expresní instalace**
    1. Zobrazí hello po úspěšné instalaci brány hello následující zprávou:

        ![Brána pro správu dat – úspěch Expresní instalace](media/data-factory-data-management-gateway-high-availability-scalability/express-setup-success.png)
    2. Spusťte Správce konfigurace správy dat pro bránu hello podle [tyto pokyny](data-factory-data-management-gateway.md#configuration-manager). Zobrazí název hello brány, název uzlu, stav, atd.

        ![Brána pro správu dat – instalace proběhla úspěšně.](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)
4. Pokud se rozhodnete **ruční instalaci**:
    1. Stažení instalačního balíčku hello z hello Microsoft Download Center, spusťte ho tooinstall brány na váš počítač.
    2. Použití hello **ověřovací klíč** z hello **konfigurace** stránky tooregister hello brány.
    
        ![Brána pro správu dat – instalace proběhla úspěšně.](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-authentication-key.png)
    3. V hello **nový uzel brány** stránky, můžete zadat vlastní **název** toohello uzel brány. Ve výchozím nastavení je stejný jako název počítače hello název uzlu.    

        ![Brána pro správu dat – zadejte název](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-name.png)
    4. V další stránku hello, můžete zvolit, zda příliš**povolit šifrování pro komunikaci mezi uzly**. Klikněte na tlačítko **přeskočit** toodisable šifrování (výchozí).

        ![Brána pro správu dat - povolit šifrování](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-node-encryption.png)  
    
        > [!NOTE]
        > Změna režimu šifrování je podporována pouze když máte velké uzlu v hello logické brány. režim šifrování hello toochange, když brána používá více uzlů, hello následující kroky: Odstraňte všechny hello uzly s výjimkou jeden uzel, změňte režim šifrování hello a poté znovu přidejte hello uzly.
        > 
        > V tématu [požadavky na certifikát protokolu TLS/SSL](#tlsssl-certificate-requirements) bodu seznam požadavků pro používání certifikát protokolu TLS/SSL. 
    5. Po úspěšné instalaci hello brány, klikněte na tlačítko spustit nástroje Configuration Manager:
    
        ![Ruční instalaci - spuštění nástroje configuration manager](media/data-factory-data-management-gateway-high-availability-scalability/manual-setup-launch-configuration-manager.png)   
    6. Správce konfigurace brány pro správu dat se zobrazí v uzlu hello (místní počítač Windows), který se zobrazuje stav připojení, **název brány**, a **název uzlu**.  

        ![Brána pro správu dat – instalace proběhla úspěšně.](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-installation-success.png)

        > [!NOTE]
        > Pokud zřizujete hello brány na virtuální počítač Azure, můžete použít [této šablony Azure Resource Manageru na Githubu](https://github.com/xiaoyingLJ/vms-with-multiple-data-management-gateway). Tento skript vytvoří logické brány, nastaví virtuální počítače s nainstalovaným softwarem Brána pro správu dat a jejich registruje hello logické brány. 
6. Na portálu Azure, spusťte hello **brány** stránky: 
    1. Na hello data factory domovskou stránku hello portálu, klikněte na tlačítko **propojené služby**.
    
        ![Domovská stránka objektu pro vytváření dat](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-home-page.png)
    2. Vyberte hello **brány** toosee hello **brány** stránky:
    
        ![Domovská stránka objektu pro vytváření dat](media/data-factory-data-management-gateway-high-availability-scalability/linked-services-gateway.png)
    4. Zobrazí hello **brány** stránky:   

        ![Brána s jedním uzlem zobrazení](media/data-factory-data-management-gateway-high-availability-scalability/gateway-first-node-portal-view.png) 
7. Klikněte na tlačítko **přidat uzel** na hello nástrojů tooadd logické brány toohello uzlu. Pokud plánujete toouse Expresní instalace, proveďte tento krok z hello na místním počítači, který bude přidán jako brána toohello uzlu. 

    ![Brána pro správu dat – přidat uzel nabídky](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)
8. Kroky jsou podobné toosetting až hello prvního uzlu. Hello uživatelského rozhraní nástroje Configuration Manager umožňuje nastavit název uzlu hello, pokud si zvolíte možnost Ruční instalace hello: 

    ![Configuration Manager – instalace druhé brány](media/data-factory-data-management-gateway-high-availability-scalability/install-second-gateway.png)
9. Po úspěšné instalaci hello brány v uzlu hello hello nástroje Configuration Manager zobrazí následující obrazovka hello:  

    ![Configuration Manager – úspěšné instalaci druhé brány](media/data-factory-data-management-gateway-high-availability-scalability/second-gateway-installation-successful.png)
10. Pokud otevřete hello **brány** stránku hello portálu byste nyní vidět dva uzly brány: 

    ![Brána s dvěma uzly hello portálu](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)
11. toodelete uzlu brány, klikněte na tlačítko **odstranit uzlu** na panelu nástrojů hello, vyberte uzel hello toodelete a pak klikněte na **odstranit** hello panelu nástrojů. Tato akce odstraní vybraný uzel hello ze skupiny hello. Všimněte si, že tato akce neodinstaluje software brány správy dat hello z uzlu hello (místní počítač Windows). Použití **přidat nebo odebrat programy** v Ovládacích panelech na hello místní toouninstall hello brány. Když odinstalujete z hello uzlu brány, automaticky se odstraní hello portálu.   

## <a name="upgrade-an-existing-gateway"></a>Upgrade existující bránu
Můžete upgradovat existující brány toouse hello vysokou dostupnost a škálovatelnost funkce. Tato funkce funguje jenom s uzly, které mají hello Brána pro správu dat. verze > = 2.12.xxxx. Uvidíte hello verze brány pro správu dat nainstalovány na počítači, v hello **pomoci** hello Správce konfigurace brány pro správu dat na kartě. 

1. Aktualizace brány hello hello místní počítač toohello nejnovější verzi pomocí následujících stahování a spuštěním instalačního balíčku MSI z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). V tématu [instalace](data-factory-data-management-gateway.md#installation) podrobnosti.  
2. Přejděte toohello portálu Azure. Spusťte hello **stránka objektu pro vytváření dat** data Factory. Klikněte na tlačítko propojené služby dlaždice toolaunch hello **stránka propojené služby**. Vyberte hello brány toolaunch hello **brány stránky**. Klikněte na možnost a povolte **funkce ve verzi Preview** jak ukazuje následující obrázek hello: 

    ![Brána pro správu dat - povolit funkce ve verzi preview](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-existing-gateway-enable-high-availability.png)   
2. Jakmile je funkce preview hello povolena hello portálu, zavřete všechny stránky. Znovu otevřete hello **brány stránky** toosee hello nové preview uživatelské rozhraní (UI).
 
    ![Brána pro správu dat – úspěch funkce verze preview povolit](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview-success.png)

    ![Brána pro správu dat – náhled uživatelského rozhraní](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-preview.png)

    > [!NOTE]
    > Během upgradu hello název prvního uzlu hello je hello název počítače hello. 
3. Nyní přidejte do uzlu. V hello **brány** klikněte na tlačítko **přidat uzel**.  

    ![Brána pro správu dat – přidat uzel nabídky](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-add-node-menu.png)

    Postupujte podle pokynů z hello předchozí část tooset až hello uzlu. 

### <a name="installation-best-practices"></a>Osvědčené postupy instalace

- Schéma napájení nakonfigurujte na hostitelském počítači hello hello brány, aby hello počítač nepřejde do režimu spánku. Pokud hello hostitelský počítač přejde do režimu spánku, brána hello neodpoví toodata požadavky.
- Zálohujte hello certifikát přidružený k hello brány.
- Zkontrolujte, zda že jsou všechny uzly podobné konfigurace (doporučeno) ideální výkonu. 
- Přidejte aspoň dva uzly tooensure vysokou dostupnost.  

### <a name="tlsssl-certificate-requirements"></a>Požadavky na certifikát protokolu TLS/SSL
Tady jsou požadavky hello hello certifikátu TLS/SSL, který se používá k zabezpečení komunikace mezi uzly brány:

- Hello certifikát musí být veřejně důvěryhodné X509 v3 certifikátu.
- Všechny uzly brány musí důvěřovat tento certifikát. 
- Doporučujeme vám, že používáte certifikáty vydané veřejnou (třetích stran) certifikační autoritou (CA).
- Podporuje všechny klíče velikost podporovaná technologií Windows Server 2012 R2 pro certifikáty SSL.
- Nepodporuje certifikáty, které používají klíči CNG.
- Zástupný znak-certifikáty jsou podporovány. 


## <a name="monitor-a-multi-node-gateway"></a>Monitorování několika uzly brány
### <a name="multi-node-gateway-monitoring"></a>Monitorování několika uzly brány
Hello portálu Azure uvidíte skoro v reálném čase snímek využití prostředků (procesoru, paměti, network(in/out) atd.) na každém uzlu společně s stavy uzlů brány. 

![Brána pro správu dat – více monitorování uzlu](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring.png)

Můžete povolit **upřesňující nastavení** v hello **brány** stránka toosee advanced metriky jako **sítě**(vstup/výstup), **Role & pověření stav**, což je užitečné při ladění problémů s brány, a **souběžných úloh** (spuštění / omezit) který může být upravený / odpovídajícím způsobem změněné během optimalizace výkonu. Hello následující tabulka obsahuje popis sloupců v hello **uzly brány** seznamu:  

Vlastnost monitorování | Popis
:------------------ | :---------- 
Name (Název) | Název logické brány hello a uzly, které jsou přidružené k hello brány.  
Status | Stav hello logické brány a hello zprostředkovatelských uzlů zasílání zpráv. Příklad: Online nebo Offline nebo Limited/atd. Informace o těchto stavů najdete v tématu [stav brány](#gateway-status) části. 
Verze | Zobrazuje verzi hello hello logické brány a každý uzel brány. Hello verzi hello logické brány je určen na základě verze Většina uzlů ve skupině hello. Pokud existují uzly s různými verzemi v instalačním programu hello logické brány, hello pouze hello uzlů se stejným číslem verze funkce hello logické brány správně. Ostatní jsou v režimu omezené hello. je potřeba toobe ručně aktualizovat (pouze v případě automatické aktualizace nezdaří). 
Dostupná paměť | Dostupná paměť na uzel brány. Tato hodnota je snímku near v reálném čase. 
Využití procesoru | Využití procesoru uzlu brány. Tato hodnota je snímku near v reálném čase. 
Sítě (vstup/výstup) | Využití uzlu brány sítě. Tato hodnota je snímku near v reálném čase. 
Souběžné úlohy (spuštění / omezit) | Počet úlohy nebo úlohy na jednotlivých uzlech spuštěné. Tato hodnota je snímku near v reálném čase. Limit označuje hello maximální souběžných úloh pro každý uzel. Tato hodnota je definována v závislosti na velikosti počítače hello. Můžete zvýšit limit tooscale hello až provádění souběžné úlohy v pokročilých scénářích, kde procesoru nebo paměti / síť je v části využívat, ale aktivity jsou vypršení časového limitu. Tato funkce je také dostupná s jedním uzlem bránou (i když není povolená funkce škálovatelnost a dostupnost hello). Další informace najdete v tématu [škálování aspekty](#scale-considerations) části. 
Role | Existují dva typy rolí – dispečera a pracovního procesu. Všechny uzly jsou pracovníci, což znamená, že může být použité tooexecute úlohy. Je pouze jeden uzel dispečera, což je použité toopull úlohy nebo úlohy z cloudové služby a jejich odesílání toodifferent uzlů pracovního procesu (včetně samotného). 

![Brána pro správu dat – pokročilé monitorování více uzlu](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-advanced.png)

### <a name="gateway-status"></a>Stav brány.

Hello následující tabulka obsahuje možné stavy z **uzel brány**: 

Status  | Komentáře nebo scénáře
:------- | :------------------
Online | Uzel připojen tooData objekt pro vytváření služby.
V režimu offline | Uzel je offline.
Upgrade | Hello uzlu, která má být automaticky aktualizován.
Omezená | Z důvodu tooConnectivity problém. Může být z důvodu tooHTTP port 8050 problém, problém s připojením služby sběrnice nebo problémům synchronizace přihlašovacích údajů. 
Neaktivní | Uzel je v konfiguraci se liší od konfigurace hello jiných Většina uzlů.<br/><br/> Uzlem může být neaktivní, když se nemůže připojit tooother uzlů. 


Hello následující tabulka obsahuje možné stavy z **logické brány**. Stav brány Hello závisí na stavy, které jsou uzlů brány hello. 

Status | Komentáře
:----- | :-------
Nutná registrace | Žádný uzel dosud registrovaných toothis logické brány
Online | Brána uzly jsou online
V režimu offline | Žádný uzel ve stavu online.
Omezená | Ne všechny uzly v této brány jsou v dobrém stavu. Tento stav se upozornění, že některé uzel může být mimo provoz! <br/><br/>Může být kvůli problémům synchronizace toocredential na dispečera nebo pracovního uzlu. 

### <a name="pipeline-activities-monitoring"></a>Kanál / monitorování aktivity
Hello portál Azure poskytuje kanálu monitorování zkušenosti s granulární uzlu úrovně podrobností. Například uvádí, které aktivity byl spuštěn na uzel. Tato informace může být užitečné pro porozumění problémy s výkonem v konkrétním uzlu, například z důvodu omezení toonetwork. 

![Brána pro správu dat – sledování pro kanály více uzlů.](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-monitoring-pipelines.png)

![Brána pro správu dat – podrobnosti o kanálu](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-multi-node-pipeline-details.png)

## <a name="scale-considerations"></a>Důležité informace o škálování

### <a name="scale-out"></a>Horizontální navýšení kapacity
Když hello **zbývá málo dostupné paměti** a hello **využití procesoru je Vysoká**, přidání nového uzlu pomáhá s více instancemi hello zatížení mezi počítači. Pokud aktivity se nedaří kvůli uzlu se na více systémů tootime nebo brána je offline, je dobré-li přidat bránu toohello uzlu.
 
### <a name="scale-up"></a>Vertikální navýšení kapacity
Pokud nejsou dobře využité hello dostatek paměti a procesoru, ale nečinnosti kapacita hello je 0, by měl škálování až zvýšením hello počet souběžných úloh, které můžou běžet na uzlu. Můžete také tooscale až když aktivity jsou vypršení časového limitu, protože je přetížena hello brány. Jak ukazuje následující obrázek hello, můžete zvýšit maximální kapacita hello pro uzel. Doporučujeme, čímž zdvojnásobí toostart s.  

![Brána pro správu dat – aspekty škálování](media/data-factory-data-management-gateway-high-availability-scalability/data-factory-gateway-scale-considerations.png)


## <a name="known-issuesbreaking-changes"></a>Známé problémy nebo nejnovější změny

- V současné době může mít až toofour fyzické zprostředkovatelských uzlů zasílání zpráv pro jednu logickou bránu. Pokud potřebujete více než čtyři uzly z důvodů výkonu, odešlete e-mail příliš[DMGHelp@microsoft.com](mailto:DMGHelp@microsoft.com).
- Nemůžete se znovu zaregistrovat uzel brány s hello ověřovací klíč z jiné logické brány tooswitch z aktuální logické brány hello. registrace toore hello bránu odinstalovat z uzlu hello, přeinstalujte hello brány a zaregistrovat ji pomocí hello ověřovací klíč pro hello jiné logické brány. 
- Pokud server proxy protokolu HTTP je potřeba pro všechny uzly brány, hello proxy v diahost.exe.config a diawp.exe.config a zda mají všechny uzly hello stejné diahost.exe.config a diawip.exe.config toomake hello server manager. V tématu [konfigurace nastavení proxy serveru](data-factory-data-management-gateway.md#configure-proxy-server-settings) podrobnosti. 
- režim toochange šifrování pro komunikaci mezi uzly ve Správci konfigurace brány odstranit všechny uzly hello portálu hello kromě jednoho. Pak přidáte uzly zpět po změně režimu šifrování hello.
- Použijte oficiální certifikátu protokolu SSL, pokud se rozhodnete tooencrypt hello – uzly komunikační kanál. Certifikát podepsaný svým držitelem může způsobit problémy s připojením k jako hello, které nemusí být stejný certifikát důvěryhodný v seznamu certifikační autority na jiné počítače. 
- Nelze zaregistrovat logické brány tooa uzlu brány, při hello uzlu verze je nižší než verze hello logické brány. Odstranit všechny uzly hello logické brány z portálu, tak, aby se můžete zaregistrovat nižší verze node(downgrade) ho. Pokud odstraníte všechny uzly logické brány, ručně nainstalujte a zaregistrujte nové uzly toothat logické brány. Expresní instalace není v tomto případě nepodporuje.
- Expresní instalace tooinstall uzly tooan stávající logické brána, která je stále pomocí pověření cloudu se nedá použít. Můžete zkontrolovat, kde jsou uložené přihlašovací údaje hello z hello Správce konfigurace brány na kartě nastavení hello.
- Nelze použít Expresní instalace tooinstall uzly tooan existující logické brány, který má povolené šifrování mezi uzly. Nastavení hello šifrování režim vyžaduje ruční přidání certifikáty, je Expresní instalace žádné další možnost. 
- Pro kopírování souborů z místního prostředí, neměli byste používat \\localhost nebo C:\files už od localhost nebo místní jednotku možná není přístupný prostřednictvím všech uzlů. Místo toho použijte \\ServerName\files toospecify soubory umístění.


## <a name="rolling-back-from-hello-preview"></a>Vrácení zpět z verze preview hello 
tooroll zpět z verze preview hello, odstraňte všechny uzly, ale jeden uzel. Nezávisle na tom, které uzly odstranit, ale ujistěte se, že máte nejméně jeden uzel v hello logické brány. Odinstalovává se brána na počítači hello nebo pomocí hello portálu Azure, můžete odstranit uzlu. V portálu Azure, v hello hello **Data Factory** klikněte na tlačítko propojené služby toolaunch hello **propojené služby** stránky. Vyberte hello brány toolaunch hello **brány** stránky. Na stránce hello brány najdete v hello uzly přidružené brány hello. Hello stránce můžete odstranit uzel z hello brány.
 
Po odstranění, klikněte na tlačítko **funkce verze preview** v hello stejné stránky portálu Azure a zakázat funkce ve verzi preview hello. Resetujete bránu brány tooone uzlu GA (Obecné dostupnosti).


## <a name="next-steps"></a>Další kroky
Projděte si hello následující články:
- [Brána pro správu dat](data-factory-data-management-gateway.md) -poskytuje podrobný přehled o hello brány.
- [Přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) – obsahuje návod podrobné pokyny pro bránu pomocí jednoho uzlu. 
