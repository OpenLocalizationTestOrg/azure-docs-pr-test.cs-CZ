---
title: "problémy Brána pro správu dat aaaTroubleshoot | Microsoft Docs"
description: "Poskytuje tipy tootroubleshoot problémy související tooData Brána pro správu."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 85dacc8a1e8d574d6e7d5b556c995cebdc148fde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a>Řešení potíží při použití Brány pro správu dat
Tento článek obsahuje informace o řešení potíží s použitím brány pro správu dat.

> [!NOTE]
> V tématu hello [Brána pro správu dat](data-factory-data-management-gateway.md) článku podrobné informace o bráně hello. V tématu hello [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článku návod přesouvání dat od tooMicrosoft databáze systému SQL Server místní úložiště objektů Blob v Azure pomocí hello brány.
>
>

## <a name="failed-tooinstall-or-register-gateway"></a>Tooinstall nebo zaregistrovat bránu
### <a name="1-problem"></a>1. Problém
Zobrazí tato chybová zpráva při instalaci a registraci bránu, konkrétně při stahování hello brány instalační soubor.

`Unable tooconnect toohello remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a>Příčina
Hello počítače, na který se pokoušíte tooinstall hello brány se nezdařilo toodownload hello nejnovější brány soubor instalace z centra Stažení hello kvůli tooa problém sítě.

#### <a name="resolution"></a>Řešení
Zkontrolujte toosee nastavení serveru proxy vaší brány firewall, zda text hello nastavení blokovat hello síťové připojení mezi počítači toohello hello [centra stahování softwaru společnosti](https://download.microsoft.com/)a aktualizovat nastavení hello odpovídajícím způsobem.

Případně si můžete stáhnout instalační soubor hello hello nejnovější bránu z hello [centra stahování softwaru společnosti](https://www.microsoft.com/download/details.aspx?id=39717) na jiných počítačích, kteří mohou přistupovat k stažení softwaru společnosti Microsoft hello. Můžete pak kopie hello instalační soubor toohello brány hostitelského počítače a potom ho spusťte ručně brána hello tooinstall a aktualizace.

### <a name="2-problem"></a>2. Problém
Zobrazí tato chyba, pokud se pokoušíte se tooinstall bránu kliknutím **nainstalovat přímo na tomto počítači** v hello portálu Azure.

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a>Příčina
Brána je už nainstalovaná na počítači hello.

#### <a name="resolution"></a>Řešení
Odinstalujte hello existující bránu na počítači hello a klikněte na tlačítko hello **nainstalovat přímo na tomto počítači** propojení znovu.

### <a name="3-problem"></a>3. Problém
Tato chyba může zobrazit při registraci novou bránu.

`Error: hello gateway has encountered an error during registration.`

#### <a name="cause"></a>Příčina
Může se tato zpráva pro jednu z následujících důvodů hello:

* Formát Hello hello klíč brány je neplatný.
* klíč brány Hello byla zrušena platnost.
* klíč brány Hello má byl znovu vygenerovat z portálu hello.  

#### <a name="resolution"></a>Řešení
Ověřte, zda používáte hello správné brány klíč z portálu hello. V případě potřeby znovu vygenerovat klíč a použít hello klíče tooregister hello brány.

### <a name="4-problem"></a>4. Problém
Může se zobrazit následující chybová zpráva, pokud se registrace brány hello.

`Error: hello content or format of hello gateway key "{gatewayKey}" is invalid, please go tooazure portal toocreate one new gateway or regenerate hello gateway key.`



![Obsah nebo formát klíče je neplatný](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a>Příčina
Hello obsah nebo formátu hello vstupní brána klíče je nesprávné. Jedním z důvodů hello může být, že jste zkopírovali pouze část hello klíč z portálu hello nebo používáte neplatný klíč.

#### <a name="resolution"></a>Řešení
Generovat klíč brány hello portálu a použít hello kopie tlačítko toocopy hello celý klíč. U této brány hello tooregister okno, vložte jej.

### <a name="5-problem"></a>5. Problém
Může se zobrazit následující chybová zpráva, pokud se registrace brány hello.

`Error: hello gateway key is invalid or empty. Specify a valid gateway key from hello portal.`

![Klíč brány je neplatný nebo prázdný](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a>Příčina
byl znovu vygenerovat klíč brány Hello nebo hello brány je Odstraněná hello portálu Azure. Může také dojít, pokud instalační program hello Brána pro správu dat není nejnovější.

#### <a name="resolution"></a>Řešení
Zkontrolujte, pokud instalační program hello Brána pro správu dat je hello nejnovější verzi, můžete najít nejnovější verzi hello na hello Microsoft [centra stahování softwaru společnosti](https://go.microsoft.com/fwlink/p/?LinkId=271260).

Pokud instalační program je aktuální / nejnovější a brány stále existuje na portálu, generuje se nový klíč brány hello hello portál Azure a použít hello kopie tlačítko toocopy hello celý klíč a pak ji vložit u této brány hello tooregister okno. Jinak znovu vytvořte hello brány a začněte znovu.

### <a name="6-problem"></a>6. Problém
Může se zobrazit následující chybová zpráva, pokud se registrace brány hello.

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with hello status “Gateway key is invalid”`

![Klíč brány je neplatný nebo prázdný](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a>Příčina
K této chybě může dojít, protože brána hello byl odstraněn nebo byla znovu vygenerovat klíč hello přidružené brány.

#### <a name="resolution"></a>Řešení
Pokud byl odstraněn hello brány, znovu vytvořit bránu hello z hello portálu, klikněte na **zaregistrovat**, zkopírujte hello klíč z portálu hello, vložte ho a zkuste tooregister hello brány.

Pokud brána hello stále existuje, ale byla znovu vygeneroval svůj klíč, použijte hello nového klíče tooregister hello brány. Pokud nemáte hello klíč, znovu vygenerujte klíč hello znovu z portálu hello.

### <a name="7-problem"></a>7. Problém
Pokud se registrace brány, bude pravděpodobně třeba tooenter cestu a heslo pro certifikát.

![Zadejte certifikát](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a>Příčina
na jiných počítačích než byl zaregistrován Hello brány. Během hello počáteční registrace brána byl přidružit k bráně hello šifrovací certifikát. Hello certifikátu můžete buď samoobslužné generovat bránou hello ani poskytované hello uživatele.  Tento certifikát je pověření použité tooencrypt hello data Store (propojené služby).  

![Export certifikátu](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

Při obnovení hello brány na jiný hostitelský počítač, zobrazí Průvodce registrací hello dotaz pro tento certifikát toodecrypt pověření dříve šifrovaných s tímto certifikátem.  Bez tohoto certifikátu hello přihlašovací údaje nelze dešifrovat hello nové brány a další kopie aktivity spuštěních přidružené k této nové brány se nezdaří.  

#### <a name="resolution"></a>Řešení
Pokud certifikát přihlašovacích údajů hello byly exportovány z původní počítač brány hello pomocí hello **exportovat** na hello tlačítko **nastavení** kartě v správy Správce konfigurace brány dat, použijte hello Tady certifikát.

Při obnovení bránu nedá Přeskočit tuto fázi. Pokud hello certifikát chybí, třeba toodelete hello brány z portálu hello a znovu vytvořit novou bránu.  Kromě toho aktualizujte všechny propojené služby, které jsou brány související toohello nutnosti opětovného zadávání svých přihlašovacích údajů.

### <a name="8-problem"></a>8. Problém
Může se zobrazit následující chybová zpráva hello.

`Error: hello remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a>Příčina
Tato chyba se stane, když vaše brána je v prostředí, které vyžaduje HTTP proxy tooaccess prostředků z Internetu, nebo se změnilo heslo pro ověřování serveru serveru proxy, ale není příslušným způsobem aktualizuje v bránu.

#### <a name="resolution"></a>Řešení
Postupujte podle pokynů hello v hello [důležité informace o Proxy serveru](#proxy-server-considerations) části tohoto článku a konfigurace nastavení proxy serveru s Data Management Gateway Configuration Manager.

## <a name="gateway-is-online-with-limited-functionality"></a>Je brána online s omezenou funkčností
### <a name="1-problem"></a>1. Problém
Zobrazí stav hello hello brány jako online s omezenou funkčností.

#### <a name="cause"></a>Příčina
Stav hello hello brány zobrazí jako online s omezenou funkčností pro jednu z následujících důvodů hello:

* Brána se nemůže připojit toocloud služby přes Azure Service Bus.
* Cloudové služby se nemůže připojit toogateway přes službu Service Bus.

Když je brána hello online s omezenou funkčností, nemusí být možné toouse hello Průvodce kopírováním služby Data Factory toocreate datových kanálů pro kopírování dat tooor z místní datová úložiště. Jako alternativní řešení můžete použít Editor služby Data Factory hello portálu, Visual Studio nebo Azure PowerShell.

#### <a name="resolution"></a>Řešení
Řešení tohoto problému (online s omezenou funkčností) je založen na tom, jestli nelze hello brány připojit toohello cloudové služby nebo hello jiným způsobem. Hello následující oddíly poskytují tato řešení.

### <a name="2-problem"></a>2. Problém
Zobrazí následující chyba hello.

`Error: Gateway cannot connect toocloud service through service bus`

![Brána se nemůže připojit toocloud služby](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a>Příčina
Brána se nemůže připojit toohello Cloudová služba přes službu Service Bus.

#### <a name="resolution"></a>Řešení
Postupujte podle těchto kroků tooget hello brány zpátky do online režimu:

1. Povolí odchozí pravidla v počítači brány hello a podniková brána firewall hello IP adresu. Můžete najít IP adresy z protokolu událostí systému Windows hello (ID == 401): Pokus o byla provedena tooaccess soketu způsobem, jeho přístupovými oprávněními XX je zakázané. XX. XX. XX:9350.
* Konfigurace nastavení proxy serveru na hello brány. V tématu hello [důležité informace o Proxy serveru](#proxy-server-considerations) podrobnosti.
* Povolte Odchozí porty v obou hello brány Windows Firewall v počítači brány hello a podniková brána firewall hello 9350-9354 a 5671. V tématu hello [porty a brány firewall](#ports-and-firewall) podrobnosti. Tento krok je volitelný, ale doporučujeme ho důvodů výkonu.

### <a name="3-problem"></a>3. Problém
Zobrazí následující chyba hello.

`Error: Cloud service cannot connect toogateway through service bus.`

#### <a name="cause"></a>Příčina
Přechodná chyba v připojení k síti.

#### <a name="resolution"></a>Řešení
Postupujte podle těchto kroků tooget hello brány zpátky do online režimu:

1. Počkejte několik minut, hello připojení se automaticky obnoví při hello chyba byla odstraněna.
* Pokud hello chyba přetrvává, restartujte službu Brána hello.

## <a name="failed-tooauthor-linked-service"></a>Neúspěšné tooauthor propojené služby
### <a name="problem"></a>Problém
Tato chyba se můžete setkat, když akci toouse správce přihlašovacích údajů hello portálu tooinput pověření pro nové propojené služby, nebo aktualizovat přihlašovací údaje pro existující propojenou službu.

`Error: hello data store '<Server>/<Database>' cannot be reached. Check connection settings for hello data source.`

Když se tato chyba, stránka nastavení hello nástroje Data Management Gateway Configuration Manager může vypadat například hello následující snímek obrazovky.

![Databázi nelze získat přístup.](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a>Příčina
certifikát SSL Hello může byla v počítači brány hello ztraceny. počítač brány Hello nelze načíst hello certifikát aktuálně používaný pro šifrování SSL. Můžete si také prohlédnout chybové hlášení v protokolu událostí hello, který je podobný toohello následující zprávou.

 `Unable tooget hello gateway settings from cloud service. Check hello gateway key and hello network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a>Řešení
Postupujte podle těchto kroků toosolve hello problému:

1. Spusťte Správce konfigurace brány pro správu dat.
2. Přepínač toohello **nastavení** kartě.  
3. Klikněte na tlačítko hello **změnu** certifikát SSL hello toochange tlačítko.

   ![Tlačítko změnit certifikát](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. Vyberte nový certifikát jako certifikát SSL hello. Můžete použít libovolný certifikát SSL, který je generován můžete nebo některé z organizací.

   ![Zadejte certifikát](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a>Aktivita kopírování se nezdaří
### <a name="problem"></a>Problém
Možná jste si všimli hello následující "UserErrorFailedToConnectToSqlserver" selhání po nastavení kanál hello portálu.

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect tooSQL Server`

#### <a name="cause"></a>Příčina
K tomu může dojít z různých důvodů a zmírnění se liší podle toho.

#### <a name="resolution"></a>Řešení
Povolte odchozí připojení TCP přes port TCP 1433 nebo na hello na straně klienta Brána pro správu dat před připojením tooan SQL database.

Pokud hello cílová databáze je databáze Azure SQL, zkontrolujte nastavení systému SQL Server brány firewall pro Azure také.

Najdete v následující části tootest hello připojení toohello místní úložiště dat hello.

## <a name="data-store-connection-or-driver-related-errors"></a>Připojení nebo chyby související s ovladačů úložiště dat
Pokud se zobrazí data uložit připojení nebo chyby související s ovladačem, proveďte hello následující kroky:

1. Spusťte Správce konfigurace brány pro správu dat na počítač brány hello.
2. Přepínač toohello **diagnostiky** kartě.
3. V **Test připojení**, přidejte hodnoty pro skupinu hello brány.
4. Klikněte na tlačítko **Test** toosee, pokud připojíte toohello místní zdroj dat z počítače brány hello pomocí hello informace o připojení a přihlašovací údaje. Jestliže hello test připojení stále po instalaci ovladače, restartování hello brány toopick až hello nejnovější změny pro ni.

![Test připojení na kartě diagnostiky](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a>Protokoly Gateway
### <a name="send-gateway-logs-toomicrosoft"></a>Odeslat protokoly tooMicrosoft brány
Při kontaktování Microsoft Support tooget nápovědy vyřešit problémy brány, můžete být požádáni tooshare protokolů brány. Verze hello hello brány můžete sdílet protokoly požadované gateway s dvěma kliknutí na tlačítko v Data Management Gateway Configuration Manager.    

1. Přepínač toohello **diagnostiky** kartě v Data Management Gateway Configuration Manager.

    ![Karta diagnostiku brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. Klikněte na tlačítko **odeslat protokoly** toosee hello následující dialogové okno.

    ![Data Management Gateway odeslat protokoly](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. (Volitelné) Klikněte na tlačítko **zobrazit protokoly** tooreview protokoly v prohlížeči událostí hello.
4. (Volitelné) Klikněte na tlačítko **o ochraně osobních údajů** tooreview Microsoft webové služby Zásady ochrany osobních údajů.
5. Jakmile budete spokojeni s jsou o tooupload, klikněte na tlačítko **odeslat protokoly** tooactually odeslat protokoly hello ze hello posledních sedmi dnech tooMicrosoft pro řešení potíží. Měli byste vidět hello stav operace odesílání protokolů hello, jak ukazuje následující snímek obrazovky hello.

    ![Data Management Gateway odeslat protokoly stavu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. Po dokončení operace hello zobrazí dialogové okno, jak ukazuje následující snímek obrazovky hello.

    ![Data Management Gateway odeslat protokoly stavu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. Uložit hello **ID sestavy** a sdílet je s Microsoft Support. ID sestavy Hello je použité toolocate hello brány protokoly, které jste nahráli k řešení potíží.  ID sestavy Hello je také uloženy v prohlížeči událostí hello.  Můžete najít prohlížením hello ID události "25" a zkontrolujte hello datum a čas.

    ![Data Management Gateway odeslat protokoly ID sestavy](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a>Archiv brány protokoly na hostitelském počítači brány
Je několik scénářů, kdy máte problémy s brány a protokoly gateway nemohou sdílet přímo:

* Ručně nainstalovat hello brány a zaregistrovat bránu hello.
* Pokusíte tooregister hello brány pomocí obnoveného klíče v Data Management Gateway Configuration Manager.
* Zkuste toosend protokoly a nemůže být připojen hostitelská služba brány pro hello.

Pro tyto scénáře můžete uložit protokoly gateway jako soubor zip a sdílet ho při kontaktujte podporu společnosti Microsoft. Například pokud obdržíte chybu při registraci brány hello jako ukazuje následující snímek obrazovky hello.   

![Chyba registrace brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

Klikněte na tlačítko hello **archivu protokoly gateway** propojit tooarchive a uložte protokoly a potom sdílet soubor zip hello s podporu společnosti Microsoft.

![Protokoly archivu brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a>Najít protokoly gateway
Podrobné brány protokolu informace naleznete v protokolech událostí systému Windows hello.

1. Spustit systém Windows **Prohlížeč událostí**.
2. Vyhledání protokolů v hello **protokoly aplikací a služeb** > **Brána pro správu dat** složky.

 Pokud se řešení potíží s problémy související s brány, vyhledejte úroveň události chyb v prohlížeči událostí hello.

![Brána pro správu dat protokoly v prohlížeči událostí](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
