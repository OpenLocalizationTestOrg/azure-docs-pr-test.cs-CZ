---
title: "aaaData Brána pro správu pro vytváření dat | Microsoft Docs"
description: "Nastavení dat brány toomove dat mezi místními a hello cloudu. Použijte Brána pro správu dat v Azure Data Factory toomove data."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: b9084537-2e1c-4e96-b5bc-0e2044388ffd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
ms.openlocfilehash: 6f523891a6230cbc7b407f46f4b02d91dfd2cf46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="data-management-gateway"></a>Brána správy dat
Brána pro správu dat Hello je klientský agent, který je nutné nainstalovat ve vašich datech místní prostředí toocopy mezi úložišti dat cloudu a místně. Hello místní data podporovaných službou Data Factory úložiště jsou uvedeny v hello [podporované zdroje dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) části.

Tento článek doplňuje hello návod v hello [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku. V Průvodci hello vytvoříte kanál, který používá hello brány toomove data z databáze tooan systému SQL Server místní objektů blob v Azure. Tento článek obsahuje podrobné podrobné informace o Brána pro správu dat hello. 

Brána pro správu dat můžete škálovat tím, že přidružíte více místní počítače s bránou hello. Je možné škálovat až zvýšením počtu úloh přesun dat, které můžou běžet souběžně na uzlu. Tato funkce je také k dispozici pro logické brány se jeden uzel. V tématu [škálování Brána pro správu dat v Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) článku.

> [!NOTE]
> Brána v současné době podporuje pouze aktivity kopírování hello a aktivity uložené procedury v datové továrně. Není možné toouse hello brány z vlastní aktivity tooaccess místní datové zdroje.      

## <a name="overview"></a>Přehled
### <a name="capabilities-of-data-management-gateway"></a>Možnosti brány pro správu dat
Brána pro správu dat poskytuje hello následující možnosti:

* Model místní zdroje dat a cloudovými zdroji dat v hello stejný objekt pro vytváření dat a přesouvat data.
* Máte jedno podokno skla pro monitorování a správu s přehledem stav brány ze stránky hello Data Factory.
* Spravujte přístup ke zdrojům dat tooon místní bezpečně.
  * Brány firewall toocorporate nevyžadují žádné změny. Brána umožňuje pouze odchozí připojení založené na protokolu HTTP tooopen Internetu.
  * Šifrování přihlašovacích údajů pro vaše místní data úložiště pomocí certifikátu.
* Přesun dat efektivně – data se přenáší v paralelní, odolné toointermittent sítě problémy s logika opakovaných pokusů automaticky.

### <a name="command-flow-and-data-flow"></a>Příkaz a tok dat
Při použití dat toocopy aktivity kopírování mezi místními a cloudovými hello aktivita používá brána tootransfer dat z toocloud zdroje dat na místě a naopak.

Zde je souhrn kroků pro kopírování pomocí brána dat a hello podrobný datový tok pro: ![tok dat pomocí brány.](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Data developer vytvoří bránu pro Azure Data Factory pomocí buď hello [portál Azure](https://portal.azure.com) nebo [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/dn820234.aspx).
2. Data developer vytvoří propojené služby pro místnímu úložišti dat zadáním hello brány. Jako součást nastavení hello propojená služba, používá data vývojáře hello nastavení pověření aplikace toospecify ověřování typy a přihlašovací údaje.  Dialogové okno aplikace komunikuje s daty hello Hello nastavení pověření ukládat tootest připojení a hello brány toosave přihlašovací údaje.
3. Brána šifruje pověření hello s hello certifikát přidružený k hello brány (poskytl vývojáře data), před uložením hello přihlašovací údaje v cloudu hello.
4. Služba data Factory komunikuje s hello brány pro plánování a správu úloh přes řídicí kanál, který používá sdílené služby Azure service bus fronty. Když úloha aktivity kopírování potřebuje toobe spuštěna., Data Factory zařadí do fronty požadavek hello spolu s přihlašovací údaje. Brána se spustí úlohu hello po dotazování hello fronty.
5. Brána Hello dešifruje hello pověření s hello stejné certifikát a pak připojí toohello místní datové úložiště s typem správné ověření a přihlašovací údaje.
6. Brána Hello zkopíruje data z místního úložiště tooa cloudové úložiště nebo naopak v závislosti na tom, jak je nakonfigurovaná hello aktivitu kopírování v datovém kanálu hello. Pro tento krok hello brány komunikuje přímo se služby cloudového úložiště, jako je například úložiště objektů Blob Azure přes zabezpečený kanál (HTTPS).

### <a name="considerations-for-using-gateway"></a>Předpoklady pro použití brány
* Jedna instance brány pro správu dat můžete použít pro více zdrojů dat na místě. Ale **jedna instance brány je vázané tooonly jeden Azure data factory** a nemohou být sdíleny s jinou služby data factory.
* Můžete mít **pouze jedna instance brány pro správu dat** nainstalované na jednom počítači. Předpokládejme, že, máte dvě datové továrny, které je třeba tooaccess místní datové zdroje, musíte tooinstall brány na dva místní počítače. Jinými slovy brána je vázané tooa konkrétní data factory
* Hello **brány není nutné toobe na stejný počítač jako zdroj dat hello hello**. Zdroj dat toohello blíže brány s však snižuje dobu hello pro zdroj dat toohello tooconnect brány hello. Doporučujeme nainstalovat hello bránu na počítači, který se liší od hello jednoho zdroje dat místního hostitele. Po hello brány a zdroj dat na různé počítače se pro prostředky se zdrojem dat není pokouší hello brány.
* Můžete mít **více bran na různé počítače připojení toohello stejný zdroj dat na místě**. Například můžete mít dvě brány obsluhující dvě datové továrny ale hello stejný místní zdroj dat je registrován s obou hello datové továrny.
* Pokud je již nainstalovaná na vašem počítači slouží **Power BI** scénář, nainstalovat **samostatnou bránu pro Azure Data Factory** na jiném počítači.
* Brány musí použít i v případě, že používáte **ExpressRoute**.
* Váš zdroj dat považovat za zdroj dat na místě, (který je za bránou firewall) i při použití **ExpressRoute**. Použijte hello brány tooestablish připojení mezi službou hello a zdroj dat hello.
* Je nutné **používat bránu hello** i v případě hello úložiště dat je v cloudu hello na **virtuálního počítače Azure IaaS**.

## <a name="installation"></a>Instalace
### <a name="prerequisites"></a>Požadavky
* Hello podporované **operačního systému** jsou verze Windows 7, Windows 8 nebo 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Instalace hello Brána pro správu dat na řadiči domény není aktuálně podporována.
* Rozhraní .NET framework 4.5.1 nebo novější je vyžadován. Pokud instalujete brány na počítače s Windows 7, nainstalujte rozhraní .NET Framework 4.5 nebo novější. V tématu [požadavky na systém rozhraní .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) podrobnosti.
* Hello doporučená **konfigurace** pro počítač brány hello je alespoň 2 GHz, 4 jádra, 8 GB paměti RAM a 80-GB místa na disku.
* Pokud hello hostitelský počítač přejde do režimu spánku, brána hello neodpoví toodata požadavky. Proto konfigurovat odpovídající **schéma napájení** na počítači hello před instalací hello brány. Pokud je počítač hello nakonfigurované toohibernate, vyzve instalace brány hello zprávu.
* Musíte mít oprávnění správce na počítači tooinstall hello a konfigurace brány pro správu dat hello úspěšně. Můžete přidat další uživatele toohello **Brána pro správu dat uživatele** místní skupiny Windows. Hello členy této skupiny jsou možné toouse hello **Správce konfigurace brány pro správu dat** nástroj tooconfigure hello brány.

Jak kopírovat běh aktivit dojít na konkrétní frekvenci, hello využití prostředků (procesor, paměť) na počítači hello také následuje hello stejný vzor s ve špičce a doby nečinnosti. Využití prostředků závisí také výraznou na hello množství dat přesouvá. Více úloh kopie jsou v průběhu, uvidíte zvedla během špiček využití prostředků.

### <a name="installation-options"></a>Možnosti instalace
Brána pro správu dat se dá nainstalovat ve hello následující způsoby:

* Stažením instalačního balíčku MSI z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  Hello MSI lze také použít tooupgrade existující data správy brány toohello nejnovější verzi, se všemi možnými nastavení zachovaná.
* Kliknutím na **stáhnout a nainstalovat bránu dat** odkazu na RUČNÍ instalace nebo **nainstalovat přímo na tomto počítači** pod Expresní instalace. V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny k používání Expresní instalace najdete v článku. manuální krok Hello přejdete toohello stažení softwaru.  v další části hello jsou Hello pokyny pro stahování a instalace hello brány ze služby Stažení softwaru.

### <a name="installation-best-practices"></a>Osvědčené postupy instalace:
1. Schéma napájení nakonfigurujte na hostitelském počítači hello hello brány, aby hello počítač nepřejde do režimu spánku. Pokud hello hostitelský počítač přejde do režimu spánku, brána hello neodpoví toodata požadavky.
2. Zálohujte hello certifikát přidružený k hello brány.

### <a name="install-hello-gateway-from-download-center"></a>Nainstalujte bránu hello ze služby Stažení softwaru
1. Přejděte příliš[stránky pro stažení Brána pro správu dat](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klikněte na tlačítko **Stáhnout**, vyberte hello příslušnou verzi (**32-bit** vs. **64-bit**) a klikněte na tlačítko **Další**.
3. Spustit hello **MSI** přímo nebo ho uložit tooyour pevný disk a spustit.
4. Na hello **úvodní** vyberte **jazyk** klikněte na tlačítko **Další**.
5. **Přijměte** hello licenční smlouvu a klikněte na tlačítko **Další**.
6. Vyberte **složky** tooinstall hello brány a klikněte na tlačítko **Další**.
7. Na hello **připraven tooinstall** klikněte na tlačítko **nainstalovat**.
8. Klikněte na tlačítko **Dokončit** toocomplete instalace.
9. Získáte klíč hello z hello portálu Azure. Viz další část hello podrobné pokyny.
10. Na hello **registrace brány** stránka **Správce konfigurace brány pro správu dat** spuštěná na počítači, hello následující kroky:
    1. Vložte klíč hello v textu hello.
    2. Volitelně klikněte na **Show klíč brány** text klíče toosee hello.
    3. Klikněte na tlačítko **zaregistrovat**.

### <a name="register-gateway-using-key"></a>Zaregistrujte bránu pomocí klíče
#### <a name="if-you-havent-already-created-a-logical-gateway-in-hello-portal"></a>Pokud jste ještě nevytvořili logické brány hello portálu
toocreate brány v klíči hello portál a získání hello z hello **konfigurace** stránky, postupujte podle kroků z návod v hello [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článku.    

#### <a name="if-you-have-already-created-hello-logical-gateway-in-hello-portal"></a>Pokud jste již vytvořili logické brány hello hello portálu
1. Na portálu Azure, přejděte toohello **Data Factory** a klikněte na tlačítko **propojené služby** dlaždici.

    ![Stránka objektu pro vytváření dat](media/data-factory-data-management-gateway/data-factory-blade.png)
2. V hello **propojené služby** stránky, vyberte hello logické **brány** jste vytvořili v portálu hello.

    ![logické brány](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. V hello **bránu Data Gateway** klikněte na tlačítko **stáhnout a nainstalovat bránu dat**.

    ![Stáhněte si odkaz hello portálu](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. V hello **konfigurace** klikněte na tlačítko **znovu vytvořte klíč**. Klikněte na tlačítko Ano na zprávu upozornění hello po přečtení ji pečlivě.

    ![Znovu vytvořte klíč](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Klikněte na tlačítko Kopírovat tlačítko Další toohello klíč. klíč Hello je zkopírovaný toohello schránky.

    ![Zkopírovat klíč](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Systémové ikony oznamovací oblasti nebo oznámení
Hello následující obrázek ukazuje některé hello ikony oznamovací oblasti, které vidíte.

![systémové ikony oznamovací oblasti](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Pokud přesuňte ukazatel na hello systému panelu ikonu nebo oznámení, můžete zobrazit podrobnosti o stavu hello hello operace aktualizace pro bránu v automaticky otevíraném okně.

### <a name="ports-and-firewall"></a>Porty a brány firewall
Existují dvě brány firewall, je nutné tooconsider: **podniková brána firewall** systémem směrovač centrální hello hello organizace a **brány Windows firewall** nakonfigurovaný jako démon na hello místního počítače, kde hello Brána je nainstalovaná.  

![Brány firewall](./media/data-factory-data-management-gateway/firewalls2.png)

Na úrovni podniková brána firewall, je nutné nakonfigurovat hello následující domény a odchozí porty:

| Názvy domén | Porty | Popis |
| --- | --- | --- |
| *. servicebus.windows.net |443, 80 |Používá ke komunikaci s back-end služba pro přesun dat |
| *. core.windows.net |443 |Použít pro kopírování připravený pomocí objektů Blob v Azure (Pokud je nakonfigurováno)|
| *. frontend.clouddatahub.net |443 |Používá ke komunikaci s back-end služba pro přesun dat |


Na úrovni brány firewall systému Windows jsou obvykle povolené tyto Odchozí porty. Pokud není, můžete nakonfigurovat hello domén a porty odpovídajícím způsobem v počítači brány.

> [!NOTE]
> 1. Na základě vaší zdroje / jímky, můžete mít další domény toowhitelist a odchozí porty ve vaší podnikové/brána Windows firewall.
> 2. Pro některé databáze cloudu (například: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)atd), musíte toowhitelist IP adresu počítače brány na jejich konfiguraci brány firewall.
>
>


#### <a name="copy-data-from-a-source-data-store-tooa-sink-data-store"></a>Kopírování dat z úložiště zdroje dat úložiště tooa jímku dat
Zajistěte, aby hello pravidla brány firewall jsou povolené správně na hello podnikový firewall, brány Windows firewall na počítači gateway hello a úložiště dat hello sám sebe. Tato pravidla povolíte hello brány tooconnect tooboth zdroj a jímka úspěšně. Povolení pravidel pro každou úložiště dat, který je zahrnut v operaci kopírování hello.

Například toocopy z **jímky místní data store tooan Azure SQL Database nebo jímky Azure SQL Data Warehouse**, hello následující kroky:

* Povolit odchozí **TCP** komunikaci na portu **1433** pro bránu Windows firewall a podniková brána firewall.
* Konfigurovat nastavení brány firewall hello Azure SQL server tooadd hello IP adresu hello brány počítač toohello seznam povolených IP adres.

> [!NOTE]
> Pokud brána firewall nedovoluje odchozí port 1433, brána nemá přístup k Azure SQL přímo. V takovém případě můžete použít [připravený kopie](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) tooSQL Azure databáze nebo datového skladu SQL Azure. V tomto scénáři by pro přesun dat hello vyžadují jenom protokolu HTTPS (port 443).
>
>


### <a name="proxy-server-considerations"></a>Důležité informace o proxy serveru
Pokud vaše podnikové síťové prostředí používá proxy server tooaccess hello Internetu, nakonfigurujte data management gateway toouse příslušná nastavení proxy serveru. Během fáze počáteční registrace hello můžete nastavit hello proxy.

![Sada proxy během registrace](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Brána používá hello proxy server tooconnect toohello cloudové služby. Klikněte na tlačítko **změnu** propojení během počáteční instalace. Zobrazí hello **nastavení proxy serveru** dialogové okno.

![Sada proxy serveru pomocí Správce konfigurace](media/data-factory-data-management-gateway/SetProxySettings.png)

Existují tři možnosti konfigurace:

* **Nepoužívejte proxy**: brány explicitně nepoužívá žádné proxy tooconnect toocloud služby.
* **Použít proxy server systému**: Brána používá proxy server hello nastavení nakonfigurované v diahost.exe.config a diawp.exe.config.  Pokud žádný proxy server je nakonfigurován v diahost.exe.config a diawp.exe.config, brána připojí toocloud služby přímo bez proxy serveru.
* **Používat vlastní proxy server**: Konfigurace hello HTTP proxy nastavení toouse pro bránu, místo použití konfigurace v diahost.exe.config a diawp.exe.config.  Adresa a Port jsou povinné.  Uživatelské jméno a heslo jsou volitelné v závislosti na nastavení vašeho proxy ověřování.  Všechna nastavení jsou šifrován hello certifikát přihlašovacích údajů brány hello a jsou uložené místně na hostitelském počítači brány hello.

Brána pro správu dat Hello hostitelská služba restartuje automaticky po uložení hello aktualizované nastavení proxy serveru.

Po Brány byl úspěšně registrován, chcete-li tooview nebo aktualizovat nastavení proxy serveru, použijte Správce konfigurace brány pro správu dat.

1. Spusťte **Správce konfigurace brány pro správu dat**.
2. Přepínač toohello **nastavení** kartě.
3. Klikněte na tlačítko **změnu** na odkaz v **server Proxy protokolu HTTP** části toolaunch hello **nastavení proxy serveru HTTP** dialogové okno.  
4. Po kliknutí na tlačítko hello **Další** tlačítko se zobrazí dialog s upozorněním žádostí pro nastavení oprávnění toosave hello proxy a restartujte hello hostitelská služba brány.

Můžete zobrazit a aktualizovat server proxy protokolu HTTP pomocí nástroje Configuration Manager.

![Sada proxy serveru pomocí Správce konfigurace](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Pokud jste nastavili proxy server pomocí ověřování NTLM, kompatibilní se hostitelská služba brány pro účet domény hello. Pokud změníte hello heslo pro účet domény hello později, mějte na paměti, nastavení konfigurace tooupdate hello služby a restartujte ji odpovídajícím způsobem. Z důvodu toothis požadavek doporučujeme používat vyhrazené domény účtu tooaccess hello proxy serveru, který nevyžaduje heslo hello tooupdate často.
>
>

### <a name="configure-proxy-server-settings"></a>Nakonfigurujte nastavení serveru proxy
Pokud vyberete **používat proxy server systému** nastavení pro proxy server hello HTTP, brána používá hello nastavení proxy v diahost.exe.config a diawp.exe.config.  Pokud žádný proxy server je zadané v diahost.exe.config a diawp.exe.config, brána připojí toocloud služby přímo bez proxy serveru. Hello následující postup obsahuje pokyny pro aktualizaci souboru diahost.exe.config hello.  

1. V Průzkumníku souborů tvoří bezpečné kopie C:\Program Files\Microsoft Data správy Gateway\2.0\Shared\diahost.exe.config tooback hello původní soubor.
2. Spusťte Notepad.exe spuštění jako správce a otevřete textový soubor "C:\Program Files\Microsoft Data správy Gateway\2.0\Shared\diahost.exe.config. Najít hello výchozí značka pro system.net, jak je znázorněno v následujícím kódu hello:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Poté můžete přidat podrobnosti o proxy serveru, jak ukazuje následující příklad hello:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   V rámci hello proxy značky toospecify hello požadované nastavení jako scriptLocation jsou povoleny další vlastnosti. Odkazovat příliš[proxy – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na syntaxi.

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Uložit hello konfigurační soubor do původního umístění hello a potom restartujete hello hostitele brány správy dat služby, která převezme hello změny. Služba hello toorestart: pomocí apletu služby v Ovládacích panelech hello nebo z hello **Správce konfigurace brány pro správu dat** > klikněte na tlačítko hello **zastavit službu** tlačítko a pak klikněte na hello **Spustit službu**. Pokud hello služba nespustí, je pravděpodobné, že byl přidán nesprávnou syntaxi – značka XML do konfiguračního souboru aplikace hello, která byla upravena.

> [!IMPORTANT]
> Nevynechali tooupdate **i** diahost.exe.config a diawp.exe.config.  


Kromě toho toothese bodů, musíte taky toomake se, že Microsoft Azure v seznamu povolených IP adres vaší společnosti. Hello seznam platných Microsoft Azure IP adres si můžete stáhnout z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Možné příznaky brány firewall a proxy server s problémy
Pokud narazíte na chyby následující těch, které jsou podobné toohello, je pravděpodobně z důvodu tooimproper konfiguraci brány firewall nebo proxy server hello serveru, která blokuje brány z připojování tooauthenticate tooData objekt pro vytváření, sám sebe. Naleznete tooensure tooprevious část vaší brány firewall a proxy serveru jsou správně nakonfigurovány.

1. Když zkusíte tooregister hello brány, zobrazí hello následující chybě: "klíč brány hello tooregister se nezdařilo. Než to zkusíte znovu klíč brány tooregister hello, potvrďte, že hello Brána pro správu dat je v připojeném stavu a hello hostitelská služba brány správy dat je spuštěno."
2. Když otevřete nástroje Configuration Manager, uvidíte stav jako "Odpojeno" nebo "Připojení". Při zobrazení protokoly událostí systému Windows, v části "Prohlížeč událostí" > "Aplikace a služby Logs" > "Brána pro správu dat", se zobrazí chybové zprávy, jako je například hello následující chybě:`Unable tooconnect toohello remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Otevřete port 8050 pro šifrování přihlašovacích údajů.
Hello **nastavení pověření** používá aplikace hello příchozí port **8050** toorelay pověření toohello brány při nastavování místní propojené služby v hello portálu Azure. Během instalace brány ve výchozím nastavení, instalaci brány hello ho otevře na počítači brány hello.

Pokud používáte bránu firewall jiného výrobce, můžete ručně otevřít hello port 8050. Pokud narazíte na problém brány firewall během instalace brány, můžete použít následující příkaz tooinstall hello brány bez konfigurace brány firewall hello hello.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Pokud si zvolíte tooopen není na počítači brány hello port hello 8050, použijte mechanismy pro než pomocí hello **nastavení pověření** aplikace tooconfigure data ukládat přihlašovací údaje. Například můžete použít [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny prostředí PowerShell. V tématu [nastavení přihlašovacích údajů a zabezpečení](#set-credentials-and-securityy) oddíl na tom, jak data ukládat přihlašovací údaje lze nastavit.

## <a name="update"></a>Aktualizace
Ve výchozím nastavení je brána pro správu dat aktualizována automaticky při je k dispozici novější verze brány hello. Hello brány se neaktualizuje, dokud všechny hello naplánované úlohy se provádějí. Žádné další úlohy jsou zpracovávány bránou hello, dokud se nedokončí operaci aktualizace hello. Pokud se hello aktualizace nezdaří, brány se vrátí zpět toohello předchozí verzi.

Zobrazí čas hello naplánované aktualizace v hello následujících místech:

* Stránka vlastností Hello brány v hello portálu Azure.
* Domovskou stránku hello Správce konfigurace brány pro správu dat
* Zpráva oznámení panelu systému.

Karta Home Hello hello Správce konfigurace brány pro správu dat zobrazí plán aktualizace hello a hello poslední čas hello brána byla nainstalována nebo aktualizovat.

![Aktualizace plánu](media/data-factory-data-management-gateway/UpdateSection.png)

Můžete nainstalovat aktualizaci hello hned nebo počkejte na toobe brány hello automaticky aktualizovat v hello naplánované době. Například hello následující obrázek ukazuje hello oznámení ukazuje hello Správce konfigurace brány společně s tooinstall můžete kliknout na tlačítko Aktualizovat hello ji okamžitě.

![Aktualizace v DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

uvítací zprávu oznámení na panelu systému hello vypadat, jak ukazuje následující obrázek hello:

![Zpráva panelu systému](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Zobrazí stav operace aktualizace (ručně nebo automaticky) na panelu systému hello hello. Při příštím spuštění Správce konfigurace brány, zobrazí zprávu na hello oznamovací panel této brány hello se aktualizovala spolu s odkazem příliš[co je nového tématu](data-factory-gateway-release-notes.md).

### <a name="toodisableenable-auto-update-feature"></a>Funkce toodisable nebo povolit automatickou aktualizaci
Můžete můžete zapnout/vypnout funkci Automatické aktualizace hello díky hello následující kroky:

[Pro jeden uzel brány]
1. Spusťte prostředí Windows PowerShell v počítači brány hello.
2. Přepnout složky C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript toohello.
3. Spuštění hello následující příkaz tooturn hello automatickou aktualizaci funkci vypnout (zakázat).   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. tooturn zpátky na:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
[[Pro více uzly vysoce dostupnou a škálovatelnou brány (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]
1. Spusťte prostředí Windows PowerShell v počítači brány hello.
2. Přepnout složky C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript toohello.
3. Spuštění hello následující příkaz tooturn hello automatickou aktualizaci funkci vypnout (zakázat).   

    Pro bránu s funkci vysoké dostupnosti (preview) se vyžaduje navíc AuthKey param.
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. tooturn zpátky na:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install hello gateway, you can launch Data Management Gateway Configuration Manager in one of hello following ways:

1. In hello **Search** window, type **Data Management Gateway** tooaccess this utility.
2. Run hello executable **ConfigManager.exe** in hello folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
hello Home page allows you toodo hello following actions:

* View status of hello gateway (connected toohello cloud service etc.).
* **Register** using a key from hello portal.
* **Stop** and start hello **Data Management Gateway Host service** on hello gateway machine.
* **Schedule updates** at a specific time of hello days.
* View hello date when hello gateway was **last updated**.

### Settings page
hello Settings page allows you toodo hello following actions:

* View, change, and export **certificate** used by hello gateway. This certificate is used tooencrypt data source credentials.
* Change **HTTPS port** for hello endpoint. hello gateway opens a port for setting hello data source credentials.
* **Status** of hello endpoint
* View **SSL certificate** is used for SSL communication between portal and hello gateway tooset credentials for data sources.  

### Diagnostics page
hello Diagnostics page allows you toodo hello following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs tooMicrosoft if there was a failure.
* **Test connection** tooa data source.  

### Help page
hello Help page displays hello following information:  

* Brief description of hello gateway
* Version number
* Links tooonline help, privacy statement, and license agreement.  

## Monitor gateway in hello portal
In hello Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate toohello home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select hello **gateway** in hello **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In hello **Gateway** page, you can see hello memory and CPU usage of hello gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** toosee more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

hello following table provides descriptions of columns in hello **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of hello logical gateway and nodes associated with hello gateway. Node is an on-premises Windows machine that has hello gateway installed on it. For information on having more than one node (up toofour nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of hello logical gateway and hello gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows hello version of hello logical gateway and each gateway node. hello version of hello logical gateway is determined based on version of majority of nodes in hello group. If there are nodes with different versions in hello logical gateway setup, only hello nodes with hello same version number as hello logical gateway function properly. Others are in hello limited mode and need toobe manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies hello maximum concurrent jobs for each node. This value is defined based on hello machine size. You can increase hello limit tooscale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when hello scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used tooexecute jobs. There is only one dispatcher node, which is used toopull tasks/jobs from cloud services and dispatch them toodifferent worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in hello gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
hello following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected tooData Factory service.
Offline | Node is offline.
Upgrading | hello node is being auto-updated.
Limited | Due tooConnectivity issue. May be due tooHTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from hello configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect tooother nodes. 


hello following table provides possible statuses of a **logical gateway**. hello gateway status depends on statuses of hello gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered toothis logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due toocredential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure hello number of **concurrent data movement jobs** that can run on a node tooscale up hello capability of moving data between on-premises and cloud data stores. 

When hello available memory and CPU are not utilized well, but hello idle capacity is 0, you should scale up by increasing hello number of concurrent jobs that can run on a node. You may also want tooscale up when activities are timing out because hello gateway is overloaded. In hello advanced settings of a gateway node, you can increase hello maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using hello data management gateway.  

## Move gateway from one machine tooanother
This section provides steps for moving gateway client from one machine tooanother machine.

1. In hello portal, navigate toohello **Data Factory home page**, and click hello **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in hello **DATA GATEWAYS** section of hello **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In hello **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In hello **Configure** page, click **Download and install data gateway**, and follow instructions tooinstall hello data gateway on hello machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep hello **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In hello **Configure** page in hello portal, click **Recreate key** on hello command bar, and click **Yes** for hello warning message. Click **copy button** next tookey text that copies hello key toohello clipboard. hello gateway on hello old machine stops functioning as soon you recreate hello key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste hello **key** into text box in hello **Register Gateway** page of hello **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box toosee hello key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** tooregister hello gateway with hello cloud service.
9. On hello **Settings** tab, click **Change** tooselect hello same certificate that was used with hello old gateway, enter hello **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from hello old gateway by doing hello following steps: launch Data Management Gateway Configuration Manager on hello old machine, switch toohello **Certificate** tab, click **Export** button and follow hello instructions.
10. After successful registration of hello gateway, you should see hello **Registration** set too**Registered** and **Status** set too**Started** on hello Home page of hello Gateway Configuration Manager.

## Encrypting credentials
tooencrypt credentials in hello Data Factory Editor, do hello following steps:

1. Launch web browser on hello **gateway machine**, navigate too[Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in hello **DATA FACTORY** page and then click **Author & Deploy** toolaunch Data Factory Editor.   
2. Click an existing **linked service** in hello tree view toosee its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In hello JSON editor, for hello **gatewayName** property, enter hello name of hello gateway.
4. Enter server name for hello **Data Source** property in hello **connectionString**.
5. Enter database name for hello **Initial Catalog** property in hello **connectionString**.    
6. Click **Encrypt** button on hello command bar that launches hello click-once **Credential Manager** application. You should see hello **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In hello **Setting Credentials** dialog box, do hello following steps:
   1. Select **authentication** that you want hello Data Factory service toouse tooconnect toohello database.
   2. Enter name of hello user who has access toohello database for hello **USERNAME** setting.
   3. Enter password for hello user for hello **PASSWORD** setting.  
   4. Click **OK** tooencrypt credentials and close hello dialog box.
8. You should see a **encryptedCredential** property in hello **connectionString** now.

    ```JSON
    {
        "name": "SqlServerLinkedService",
        "properties": {
            "type": "OnPremisesSqlServer",
            "description": "",
            "typeProperties": {
                "connectionString": "data source=myserver;initial catalog=mydatabase;Integrated Security=False;EncryptedCredential=eyJDb25uZWN0aW9uU3R",
                "gatewayName": "adftutorialgateway"
            }
        }
    }
    ```
Pokud máte přístup k portálu hello z počítače, který se liší od hello počítač brány, musí se ujistěte, že aplikace hello správce přihlašovacích údajů můžete připojit počítač brány toohello. Pokud aplikace hello nebudou moci připojit počítač brány hello, ho neumožňuje tooset přihlašovací údaje pro zdroj dat hello a zdroj dat toohello tootest připojení.  

Při použití hello **nastavení pověření** aplikace hello portál šifruje pověření hello s hello certifikát uvedený v hello **certifikát** kartě hello **brány Nástroj Configuration Manager** na počítači brány hello.

Pokud hledáte způsob založené na rozhraní API pro šifrování přihlašovacích údajů hello, můžete použít hello [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) pověření tooencrypt rutiny prostředí PowerShell. rutiny Hello používá hello certifikátu, že brána je nakonfigurovaná toouse tooencrypt hello pověření. Přidat zašifrované přihlašovací údaje toohello **EncryptedCredential** element hello **connectionString** v hello JSON. Použít hello JSON s hello [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) rutiny nebo v hello editoru služby Data Factory.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Neexistuje jeden další postup pro nastavení přihlašovacích údajů pomocí editoru služby Data Factory. Pokud vytvoříte propojené systému SQL Server service pomocí editoru hello a zadejte přihlašovací údaje ve formátu prostého textu, hello přihlašovací údaje jsou šifrované pomocí certifikát, který vlastní hello služba Data Factory. Hello certifikátu, že brána je nakonfigurovaná toouse nepoužívá. Tento přístup může být v některých případech trochu rychlejší, je to méně bezpečné. Proto doporučujeme, postupujte podle tohoto přístupu pouze pro účely vývoje/testování.

## <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell
Tato část popisuje, jak toocreate a zaregistrujte bránu pomocí rutin prostředí Azure PowerShell.

1. Spusťte **prostředí Azure PowerShell** v režimu správce.
2. Přihlášení tooyour účet Azure tak, že spustíte následující příkaz a zadání přihlašovacích údajů Azure hello.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Použití hello **New-AzureRmDataFactoryGateway** toocreate rutiny logické brány následujícím způsobem:

    ```PowerShell
    $MyDMG = New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>
    ```
    **Ukázkový příkaz a výstupní**:

    ```
    PS C:\> $MyDMG = New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

    Name              : MyGateway
    Description       : gateway for walkthrough
    Version           :
    Status            : NeedRegistration
    VersionStatus     : None
    CreateTime        : 9/28/2014 10:58:22
    RegisterTime      :
    LastConnectTime   :
    ExpiryTime        :
    ProvisioningState : Succeeded
    Key               : ADF#00000000-0000-4fb8-a867-947877aef6cb@fda06d87-f446-43b1-9485-78af26b8bab0@4707262b-dc25-4fe5-881c-c8a7c3c569fe@wu#nfU4aBlq/heRyYFZ2Xt/CD+7i73PEO521Sj2AFOCmiI
    ```

1. V prostředí Azure PowerShell přepínač toohello složky: **C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript\**. Spustit **RegisterGateway.ps1** přidružený hello místní proměnné **$Key** jak je znázorněno v hello následující příkaz. Tento skript zaregistruje hello Klientský agent nainstalovaný na počítači s hello logické brány, kterou vytvoříte dříve.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Pomocí parametru IsRegisterOnRemoteMachine hello můžete zaregistrovat hello brány ve vzdáleném počítači. Příklad:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Můžete použít hello **Get-AzureRmDataFactoryGateway** rutiny tooget hello seznam bran v datové továrně. Když hello **stav** ukazuje **online**, znamená to vaše brána je připraven toouse.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Můžete odebrat bránu pomocí hello **odebrat AzureRmDataFactoryGateway** popis rutiny a aktualizace pro bránu pomocí hello **Set-AzureRmDataFactoryGateway** rutiny. Syntaxe a další podrobnosti o těchto rutinách najdete v tématu referenční Data Factory.  

### <a name="list-gateways-using-powershell"></a>Seznam brány pomocí prostředí PowerShell

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Odebrat bránu pomocí prostředí PowerShell

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Další kroky
* V tématu [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku. V Průvodci hello vytvoříte kanál, který používá hello brány toomove data z databáze tooan systému SQL Server místní objektů blob v Azure.  
