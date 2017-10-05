---
title: "Brána pro správu dat pro objekt pro vytváření dat | Microsoft Docs"
description: "Nastaví bránu dat pro přesun dat mezi místními a cloudem. Přesunutí dat pomocí brány pro správu dat v Azure Data Factory."
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
ms.openlocfilehash: 9e40eba285aeb1cce6b77311d1b69a6b96967a0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="data-management-gateway"></a>Brána správy dat
Brána pro správu dat je klientský agent, který je nutné nainstalovat v prostředí místní ke kopírování dat mezi úložišti dat cloudu a místně. Místní data podporovaných službou Data Factory úložiště jsou uvedeny v [podporované zdroje dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) části.

Doplňuje podle návodu v tomto článku [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku. V tomto návodu vytvoříte kanál, který používá bránu pro přesun dat z databáze SQL serveru místně do objektu blob Azure. Tento článek obsahuje podrobné podrobné informace o Brána pro správu dat. 

Brána pro správu dat můžete škálovat tím, že přidružíte více místní počítače s bránou. Je možné škálovat až zvýšením počtu úloh přesun dat, které můžou běžet souběžně na uzlu. Tato funkce je také k dispozici pro logické brány se jeden uzel. V tématu [škálování Brána pro správu dat v Azure Data Factory](data-factory-data-management-gateway-high-availability-scalability.md) článku.

> [!NOTE]
> Brána v současné době podporuje pouze aktivity kopírování a aktivity uložené procedury v datové továrně. Není možné používat pro přístup k místním zdrojům dat brány z vlastních aktivit.      

## <a name="overview"></a>Přehled
### <a name="capabilities-of-data-management-gateway"></a>Možnosti brány pro správu dat
Brána pro správu dat poskytuje následující možnosti:

* Model místní zdroje dat a cloudové zdroje dat v rámci stejné služby data factory a přesouvat data.
* Máte jedno podokno skla pro monitorování a správu s přehledem stav brány. na stránce služby Data Factory.
* Spravovat přístup k místním zdrojům dat bezpečně.
  * Žádné změny do podnikové brány firewall. Brána je pouze odchozí připojení k Internetu otevřete založené na protokolu HTTP.
  * Šifrování přihlašovacích údajů pro vaše místní data úložiště pomocí certifikátu.
* Přesun dat efektivně – data se přenáší paralelně, odolné vůči přerušované sítě problémy s automatickým logika opakovaných pokusů.

### <a name="command-flow-and-data-flow"></a>Příkaz a tok dat
Pokud použijete aktivitu kopírování ke kopírování dat mezi místními a cloudovými, aktivita používá bránu k přenosu dat z místního zdroje dat do cloudu a naopak.

Zde je souhrn kroků pro kopírování pomocí brána dat a podrobný datový tok pro: ![tok dat pomocí brány.](./media/data-factory-data-management-gateway/data-flow-using-gateway.png)

1. Data developer vytvoří bránu pro Azure Data Factory pomocí [portál Azure](https://portal.azure.com) nebo [rutiny prostředí PowerShell](https://msdn.microsoft.com/library/dn820234.aspx).
2. Data developer vytvoří propojené služby pro místnímu úložišti dat tak, že zadáte brány. Jako součást nastavení propojené služby developer dat používá aplikace nastavení pověření k určení typů ověřování a přihlašovací údaje.  Dialogové okno nastavení pověření aplikace komunikuje s úložištěm dat k testování připojení a bránu a uložte přihlašovací údaje.
3. Brána šifruje přihlašovací údaje s certifikát přidružený k bráně (poskytl vývojáře data), před uložením přihlašovací údaje v cloudu.
4. Služba data Factory komunikuje s bránou pro plánování a správu úloh přes řídicí kanál, který používá sdílené služby Azure service bus fronty. Když úlohu kopírování aktivity musí být spuštěna., Data Factory zařadí do fronty požadavek spolu s přihlašovací údaje. Brána se spustí úlohu po dotazování fronty.
5. Brána dešifruje přihlašovací údaje s stejný certifikát a pak připojí k místnímu úložišti dat typu správné ověření a přihlašovací údaje se.
6. Brána zkopíruje data z místního úložiště do cloudového úložiště, nebo naopak v závislosti na konfiguraci aktivitě kopírování v datovém kanálu. Pro tento krok brány komunikuje přímo se služby cloudového úložiště, jako je například úložiště objektů Blob Azure přes zabezpečený kanál (HTTPS).

### <a name="considerations-for-using-gateway"></a>Předpoklady pro použití brány
* Jedna instance brány pro správu dat můžete použít pro více zdrojů dat na místě. Ale **jedna instance brány je vázaný na pouze jeden objekt pro vytváření dat Azure** a nemohou být sdíleny s jinou služby data factory.
* Můžete mít **pouze jedna instance brány pro správu dat** nainstalované na jednom počítači. Předpokládejme, že, máte dvě datové továrny, které potřebují přístup k místním zdrojům dat, budete muset nainstalovat brány ve dvou počítačích na místě. Jinými slovy brány je vázaný na konkrétní data factory
* **Brány nemusí být ve stejném počítači jako zdroj dat**. S brány blíž ke zdroji dat však zkracuje čas pro bránu pro připojení ke zdroji dat. Doporučujeme nainstalovat bránu na počítači, který se liší od ten, který je hostitelem zdroje dat na místě. Pokud zdroj brány a data jsou na různé počítače, brána není pokouší o prostředky se zdrojem dat.
* Můžete mít **více bran na různé počítače připojení ke zdroji dat stejnou místní**. Například můžete mít dvě brány obsluhující dvě datové továrny ale stejný místní zdroj dat není zaregistrována datové továrny.
* Pokud je již nainstalovaná na vašem počítači slouží **Power BI** scénář, nainstalovat **samostatnou bránu pro Azure Data Factory** na jiném počítači.
* Brány musí použít i v případě, že používáte **ExpressRoute**.
* Váš zdroj dat považovat za zdroj dat na místě, (který je za bránou firewall) i při použití **ExpressRoute**. Použijte bránu k navázání připojení mezi službou a zdroj dat.
* Je nutné **použití brány** i v případě, že je úložiště dat v cloudu **virtuálního počítače Azure IaaS**.

## <a name="installation"></a>Instalace
### <a name="prerequisites"></a>Požadavky
* U podporovaných **operačního systému** jsou verze Windows 7, Windows 8 nebo 8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Instalace brány pro správu dat na řadiči domény není aktuálně podporována.
* Rozhraní .NET framework 4.5.1 nebo novější je vyžadován. Pokud instalujete brány na počítače s Windows 7, nainstalujte rozhraní .NET Framework 4.5 nebo novější. V tématu [požadavky na systém rozhraní .NET Framework](https://msdn.microsoft.com/library/8z6watww.aspx) podrobnosti.
* Doporučené **konfigurace** pro bránu počítač je alespoň 2 GHz, 4 jádra, 8 GB paměti RAM a 80-GB místa na disku.
* Pokud hostitelský počítač přejde do režimu spánku, brána neodpovídá na požadavky na data. Proto konfigurovat odpovídající **schéma napájení** v počítači před instalací brány. Pokud je počítač nakonfigurovaný do režimu spánku, vyzve k instalaci brány zprávu.
* Musíte být správce na počítači k instalaci a konfiguraci brány pro správu dat úspěšně. Můžete přidat další uživatelům **Brána pro správu dat uživatele** místní skupiny Windows. Členové této skupiny jsou schopné používat **Správce konfigurace brány pro správu dat** nástroj pro konfiguraci brány.

Jako běh aktivit kopie dojít na konkrétní frekvenci, využití prostředků (procesor, paměť) na počítači také používá se stejný vzor s ve špičce a doby nečinnosti. Využití prostředků závisí také výraznou na množství dat přesouvá. Více úloh kopie jsou v průběhu, uvidíte zvedla během špiček využití prostředků.

### <a name="installation-options"></a>Možnosti instalace
Brána pro správu dat lze nainstalovat následujícím způsobem:

* Stažením balíčku MSI Instalační program z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717).  Soubor MSI můžete také použít k upgradu existující brány pro správu dat na nejnovější verzi, se všemi možnými nastavení zachovaná.
* Kliknutím na **stáhnout a nainstalovat bránu dat** odkazu na RUČNÍ instalace nebo **nainstalovat přímo na tomto počítači** pod Expresní instalace. V tématu [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny k používání Expresní instalace najdete v článku. Manuální krok přejdete na webu Stažení softwaru.  Pokyny ke stažení a instalaci brány ze služby Stažení softwaru jsou v další části.

### <a name="installation-best-practices"></a>Osvědčené postupy instalace:
1. Schéma napájení nakonfigurujte na hostitelském počítači pro bránu, aby počítač nepřejde do režimu spánku. Pokud hostitelský počítač přejde do režimu spánku, brána neodpovídá na požadavky na data.
2. Zálohujte certifikát přidružený k bráně.

### <a name="install-the-gateway-from-download-center"></a>Bránu nainstalovat ze služby Stažení softwaru
1. Přejděte na [stránky pro stažení Brána pro správu dat](https://www.microsoft.com/download/details.aspx?id=39717).
2. Klikněte na tlačítko **Stáhnout**, vyberte příslušnou verzi (**32-bit** vs. **64-bit**) a klikněte na tlačítko **Další**.
3. Spustit **MSI** přímo nebo uložte ho na pevný disk a spustit.
4. Na **úvodní** vyberte **jazyk** klikněte na tlačítko **Další**.
5. **Přijměte** licenční smlouvu a klikněte na tlačítko **Další**.
6. Vyberte **složky** instalace brány a klikněte na tlačítko **Další**.
7. Na **připravené k instalaci** klikněte na tlačítko **nainstalovat**.
8. Klikněte na tlačítko **Dokončit** k dokončení instalace.
9. Získáte klíč z portálu Azure. Najdete v části Další podrobné pokyny.
10. Na **registrace brány** stránka **Správce konfigurace brány pro správu dat** spuštěná na počítači, proveďte následující kroky:
    1. Vložte klíč v textu.
    2. Volitelně klikněte na **Show klíč brány** zobrazíte text klíče.
    3. Klikněte na tlačítko **zaregistrovat**.

### <a name="register-gateway-using-key"></a>Zaregistrujte bránu pomocí klíče
#### <a name="if-you-havent-already-created-a-logical-gateway-in-the-portal"></a>Pokud jste ještě nevytvořili logické brány na portálu
Vytvoření brány na portálu a získání klíče z **konfigurace** stránky, postupujte podle kroků z návod v [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článku.    

#### <a name="if-you-have-already-created-the-logical-gateway-in-the-portal"></a>Pokud jste již vytvořili logické brány na portálu
1. Na portálu Azure, přejděte na **Data Factory** a klikněte na tlačítko **propojené služby** dlaždici.

    ![Stránka objektu pro vytváření dat](media/data-factory-data-management-gateway/data-factory-blade.png)
2. V **propojené služby** vyberte logické **brány** vytvořený na portálu.

    ![logické brány](media/data-factory-data-management-gateway/data-factory-select-gateway.png)  
3. V **bránu Data Gateway** klikněte na tlačítko **stáhnout a nainstalovat bránu dat**.

    ![Stáhněte si odkaz na portálu](media/data-factory-data-management-gateway/download-and-install-link-on-portal.png)   
4. V **konfigurace** klikněte na tlačítko **znovu vytvořte klíč**. Klepněte na tlačítko Ano upozornění po přečtení ji pečlivě.

    ![Znovu vytvořte klíč](media/data-factory-data-management-gateway/recreate-key-button.png)
5. Klikněte na tlačítko Kopírovat vedle klíč. Klíč je zkopírován do schránky.

    ![Zkopírovat klíč](media/data-factory-data-management-gateway/copy-gateway-key.png)

### <a name="system-tray-icons-notifications"></a>Systémové ikony oznamovací oblasti nebo oznámení
Následující obrázek ukazuje některé ikony oznamovací oblasti, které vidíte.

![systémové ikony oznamovací oblasti](./media/data-factory-data-management-gateway/gateway-tray-icons.png)

Pokud přesuňte ukazatel na systému panelu ikonu nebo oznámení, můžete zobrazit podrobnosti o stavu operace aktualizace pro bránu v automaticky otevíraném okně.

### <a name="ports-and-firewall"></a>Porty a brány firewall
Existují dvě brány firewall, je potřeba zvážit: **podniková brána firewall** systémem směrovači centrální organizace, a **brány Windows firewall** nakonfigurovaný jako démon na místním počítači, kde je brána nainstalovat.  

![Brány firewall](./media/data-factory-data-management-gateway/firewalls2.png)

Na úrovni podniková brána firewall je nutné nakonfigurovat následující domény a odchozí porty:

| Názvy domén | Porty | Popis |
| --- | --- | --- |
| *. servicebus.windows.net |443, 80 |Používá ke komunikaci s back-end služba pro přesun dat |
| *. core.windows.net |443 |Použít pro kopírování připravený pomocí objektů Blob v Azure (Pokud je nakonfigurováno)|
| *. frontend.clouddatahub.net |443 |Používá ke komunikaci s back-end služba pro přesun dat |


Na úrovni brány firewall systému Windows jsou obvykle povolené tyto Odchozí porty. Pokud není, můžete nakonfigurovat domén a porty odpovídajícím způsobem v počítači brány.

> [!NOTE]
> 1. Na základě vaší zdroje / jímky, možná budete muset povolených dalších domén a odchozí porty v bráně firewall podnikové a Windows.
> 2. Pro některé databáze cloudu (například: [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-configure-firewall-settings), [Azure Data Lake](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-secure-data#set-ip-address-range-for-data-access)atd), budete muset seznam povolených adres IP adresu počítače brány na jejich konfiguraci brány firewall.
>
>


#### <a name="copy-data-from-a-source-data-store-to-a-sink-data-store"></a>Kopírování dat ze zdrojového úložiště dat do úložiště dat jímka
Zkontrolujte, zda jsou pravidla brány firewall na podnikové brány firewall, brána Windows firewall v počítači s bránou povolené správně a data ukládat sám sebe. Tato pravidla povolíte bránu pro připojení k obou zdroj a jímka úspěšně. Povolení pravidel pro každou úložiště dat, který je zahrnut v operaci kopírování.

Například pro kopírování z **úložišti místní data jímky Azure SQL Database nebo jímky Azure SQL Data Warehouse**, proveďte následující kroky:

* Povolit odchozí **TCP** komunikaci na portu **1433** pro bránu Windows firewall a podniková brána firewall.
* Konfigurace nastavení brány firewall serveru Azure SQL na IP adresu počítače brány přidat do seznamu povolených IP adres.

> [!NOTE]
> Pokud brána firewall nedovoluje odchozí port 1433, brána nemá přístup k Azure SQL přímo. V takovém případě můžete použít [připravený kopie](https://docs.microsoft.com/azure/data-factory/data-factory-copy-activity-performance#staged-copy) do SQL Azure Database nebo datového skladu SQL Azure. V tomto scénáři by pro přesun dat vyžadují jenom protokolu HTTPS (port 443).
>
>


### <a name="proxy-server-considerations"></a>Důležité informace o proxy serveru
Pokud vaše podnikové síťové prostředí používá proxy server pro přístup k Internetu, nakonfigurujte Brána pro správu dat použít příslušná nastavení proxy serveru. Během fáze počáteční registrace můžete nastavit proxy server.

![Sada proxy během registrace](media/data-factory-data-management-gateway/SetProxyDuringRegistration.png)

Brána používá proxy server pro připojení ke cloudové službě. Klikněte na tlačítko **změnu** propojení během počáteční instalace. Zobrazí **nastavení proxy serveru** dialogové okno.

![Sada proxy serveru pomocí Správce konfigurace](media/data-factory-data-management-gateway/SetProxySettings.png)

Existují tři možnosti konfigurace:

* **Nepoužívejte proxy**: Brána pro připojení ke cloudovým službám explicitně nepoužívá jakýkoli proxy server.
* **Použít proxy server systému**: Brána používá proxy server, který je nastavení nakonfigurované v diahost.exe.config a diawp.exe.config.  Pokud žádný proxy server je nakonfigurován v diahost.exe.config a diawp.exe.config, brána připojí ke cloudové službě přímo bez proxy serveru.
* **Používat vlastní proxy server**: Konfigurace nastavení pro bránu, místo použití konfigurace v diahost.exe.config a diawp.exe.config proxy protokolu HTTP.  Adresa a Port jsou povinné.  Uživatelské jméno a heslo jsou volitelné v závislosti na nastavení vašeho proxy ověřování.  Všechna nastavení jsou šifrován certifikát přihlašovacích údajů brány a jsou uložené místně na hostitelském počítači brány.

Brána pro správu dat hostitelská služba restartuje automaticky po uložení aktualizované nastavení proxy serveru.

Po Brány byl úspěšně registrován, pokud chcete zobrazit nebo aktualizovat nastavení proxy serveru, použijte Správce konfigurace brány pro správu dat.

1. Spusťte **Správce konfigurace brány pro správu dat**.
2. Přepnout **nastavení** kartě.
3. Klikněte na tlačítko **změnu** na odkaz v **server Proxy protokolu HTTP** části spustíte **nastavení proxy serveru HTTP** dialogové okno.  
4. Po kliknutí **Další** tlačítko se zobrazí dialog s upozorněním žádostí o oprávnění k uložení nastavení proxy serveru a restartujte hostitelskou službu brány.

Můžete zobrazit a aktualizovat server proxy protokolu HTTP pomocí nástroje Configuration Manager.

![Sada proxy serveru pomocí Správce konfigurace](media/data-factory-data-management-gateway/SetProxyConfigManager.png)

> [!NOTE]
> Pokud jste nastavili proxy server pomocí ověřování NTLM, kompatibilní se hostitelská služba brány pro účet domény. Pokud změníte heslo pro účet domény později, musíte aktualizovat nastavení konfigurace pro službu a restartujte ji odpovídajícím způsobem. Kvůli tomuto požadavku, doporučujeme používat vyhrazený doménový účet pro přístup k proxy serveru, který nevyžaduje často aktualizovat heslo.
>
>

### <a name="configure-proxy-server-settings"></a>Nakonfigurujte nastavení serveru proxy
Pokud vyberete **používat proxy server systému** nastavení pro proxy server HTTP, brána používá nastavení proxy v diahost.exe.config a diawp.exe.config.  Pokud žádný proxy server je zadané v diahost.exe.config a diawp.exe.config, brána připojí ke cloudové službě přímo bez proxy serveru. Následující postup obsahuje pokyny pro aktualizaci souboru diahost.exe.config.  

1. V Průzkumníku souborů vytvořte bezpečné kopii C:\Program Files\Microsoft Data správy Gateway\2.0\Shared\diahost.exe.config zálohování původní soubor.
2. Spusťte Notepad.exe spuštění jako správce a otevřete textový soubor "C:\Program Files\Microsoft Data správy Gateway\2.0\Shared\diahost.exe.config. Najít výchozí značka pro system.net, jak je znázorněno v následujícím kódu:

         <system.net>
             <defaultProxy useDefaultCredentials="true" />
         </system.net>    

   Poté můžete přidat podrobnosti o proxy serveru, jak je znázorněno v následujícím příkladu:

         <system.net>
               <defaultProxy enabled="true">
                     <proxy bypassonlocal="true" proxyaddress="http://proxy.domain.org:8888/" />
               </defaultProxy>
         </system.net>

   Další vlastnosti jsou uvnitř značky proxy povoleno zadat požadované nastavení, jako je scriptLocation. Odkazovat na [proxy – Element (nastavení sítě)](https://msdn.microsoft.com/library/sa91de1e.aspx) na syntaxi.

         <proxy autoDetect="true|false|unspecified" bypassonlocal="true|false|unspecified" proxyaddress="uriString" scriptLocation="uriString" usesystemdefault="true|false|unspecified "/>
3. Konfigurační soubor uložte do původního umístění a potom restartujte službu hostitel brány správy dat, která vybere změny. Restartování služby: pomocí apletu služby v Ovládacích panelech nebo z **Správce konfigurace brány pro správu dat** > klikněte na tlačítko **zastavit službu** tlačítko a pak klikněte na **Start Služba**. Pokud se služba nespustí, je pravděpodobné, že byl přidán nesprávnou syntaxi – značka XML do konfiguračního souboru aplikace, která byla upravena.

> [!IMPORTANT]
> Nezapomeňte aktualizovat **i** diahost.exe.config a diawp.exe.config.  


Kromě těchto bodů musíte také zkontrolujte, zda že je v seznamu povolených IP adres vaší společnosti Microsoft Azure. Seznam platných Microsoft Azure IP adres si můžete stáhnout z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="possible-symptoms-for-firewall-and-proxy-server-related-issues"></a>Možné příznaky brány firewall a proxy server s problémy
Pokud narazíte na chyby, které jsou podobné těm, které jsou následující, je pravděpodobně z důvodu nesprávné konfigurace brány firewall nebo proxy serveru, která blokuje brány v připojení k objektu pro vytváření dat ke svému ověření. Podívejte se na předchozí část, aby brána firewall a proxy serveru jsou správně nakonfigurovány.

1. Při pokusu o registraci brány se zobrazí následující chyba: "se nepodařilo zaregistrovat klíč brány. Než se pokusíte znovu zaregistrujte klíč brány, potvrďte, že je brána pro správu dat v připojeném stavu a je spuštěna hostitelská služba brány správy dat."
2. Když otevřete nástroje Configuration Manager, uvidíte stav jako "Odpojeno" nebo "Připojení". Při zobrazení protokoly událostí systému Windows, v části "Prohlížeč událostí" > "Aplikace a služby Logs" > "Brána pro správu dat", se zobrazí chybové zprávy, jako je například k následující chybě:`Unable to connect to the remote server`
   `A component of Data Management Gateway has become unresponsive and restarts automatically. Component name: Gateway.`

### <a name="open-port-8050-for-credential-encryption"></a>Otevřete port 8050 pro šifrování přihlašovacích údajů.
**Nastavení pověření** aplikace používá portu pro příchozí spojení **8050** k předávání přes přihlašovací údaje k bráně při nastavování služby místní propojené na portálu Azure. Během instalace brány ve výchozím nastavení, instalaci brány nástroje ho otevře na počítači s bránou.

Pokud používáte bránu firewall jiného výrobce, můžete ručně otevřete port 8050. Pokud narazíte na problém brány firewall během instalace brány, můžete zkusit pomocí následujícího příkazu se nainstalovat bránu bez konfiguraci brány firewall.

    msiexec /q /i DataManagementGateway.msi NOFIREWALL=1

Pokud se rozhodnete, že není otevřete port 8050 na počítači s bránou, použijte mechanismy pro než pomocí **nastavení pověření** aplikace nakonfigurovat přihlašovací údaje k úložišti dat. Například můžete použít [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny prostředí PowerShell. V tématu [nastavení přihlašovacích údajů a zabezpečení](#set-credentials-and-securityy) oddíl na tom, jak data ukládat přihlašovací údaje lze nastavit.

## <a name="update"></a>Aktualizace
Ve výchozím nastavení se Brána pro správu dat automaticky aktualizuje až bude dostupná novější verze brány. Brána není aktualizován, dokud všechny naplánované úlohy se provádějí. Žádné další úlohy jsou zpracovávány bránou, dokud se nedokončí operaci aktualizace. Pokud se aktualizace nezdaří, brány je vrácena zpět na původní verzi.

Zobrazí čas naplánované aktualizace na těchto místech:

* Stránka Vlastnosti brány na portálu Azure.
* Domovská stránka nástroje správu Správce konfigurace brány dat
* Zpráva oznámení panelu systému.

Kartě Domů nástroje správu Správce konfigurace brány dat zobrazí plán aktualizací a čas posledního bráně byl nainstalován aktualizovány.

![Aktualizace plánu](media/data-factory-data-management-gateway/UpdateSection.png)

Můžete instalovat aktualizace hned, nebo počkat pro bránu, aby se automaticky aktualizovaly v naplánovaném čase. Například následující obrázek ukazuje oznámení zobrazí v aplikaci Správce konfigurace brány společně s tlačítko aktualizace, které lze klepnout a nainstalujte ji okamžitě.

![Aktualizace v DMG Configuration Manager](./media/data-factory-data-management-gateway/gateway-auto-update-config-manager.png)

Oznámení na hlavním panelu systému vypadat, jak je znázorněno na následujícím obrázku:

![Zpráva panelu systému](./media/data-factory-data-management-gateway/gateway-auto-update-tray-message.png)

Zobrazí stav operace aktualizace (ručně nebo automaticky) na hlavním panelu. Při příštím spuštění Správce konfigurace brány, zobrazí se zpráva na panelu oznámení, že brány se aktualizovala spolu s odkazem na [co je nového tématu](data-factory-gateway-release-notes.md).

### <a name="to-disableenable-auto-update-feature"></a>Zapnout/vypnout automatickou aktualizaci funkce
Je možné zapnout/vypnout funkci Automatické aktualizace provedením následujících kroků:

[Pro jeden uzel brány]
1. Spusťte prostředí Windows PowerShell v počítači s bránou.
2. Přejděte do složky C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript.
3. Spusťte následující příkaz, který zapnout automatickou aktualizaci funkci vypnout (zakázat).   

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off
    ```
4. Chcete-li zpátky na:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on  
    ```
[[Pro více uzly vysoce dostupnou a škálovatelnou brány (preview)](data-factory-data-management-gateway-high-availability-scalability.md)]
1. Spusťte prostředí Windows PowerShell v počítači s bránou.
2. Přejděte do složky C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript.
3. Spusťte následující příkaz, který zapnout automatickou aktualizaci funkci vypnout (zakázat).   

    Pro bránu s funkci vysoké dostupnosti (preview) se vyžaduje navíc AuthKey param.
    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -off -AuthKey <your auth key>
    ```
4. Chcete-li zpátky na:

    ```PowerShell
    .\GatewayAutoUpdateToggle.ps1  -on -AuthKey <your auth key> 


## Configuration Manager
Once you install the gateway, you can launch Data Management Gateway Configuration Manager in one of the following ways:

1. In the **Search** window, type **Data Management Gateway** to access this utility.
2. Run the executable **ConfigManager.exe** in the folder: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**

### Home page
The Home page allows you to do the following actions:

* View status of the gateway (connected to the cloud service etc.).
* **Register** using a key from the portal.
* **Stop** and start the **Data Management Gateway Host service** on the gateway machine.
* **Schedule updates** at a specific time of the days.
* View the date when the gateway was **last updated**.

### Settings page
The Settings page allows you to do the following actions:

* View, change, and export **certificate** used by the gateway. This certificate is used to encrypt data source credentials.
* Change **HTTPS port** for the endpoint. The gateway opens a port for setting the data source credentials.
* **Status** of the endpoint
* View **SSL certificate** is used for SSL communication between portal and the gateway to set credentials for data sources.  

### Diagnostics page
The Diagnostics page allows you to do the following actions:

* Enable verbose **logging**, view logs in event viewer, and send logs to Microsoft if there was a failure.
* **Test connection** to a data source.  

### Help page
The Help page displays the following information:  

* Brief description of the gateway
* Version number
* Links to online help, privacy statement, and license agreement.  

## Monitor gateway in the portal
In the Azure portal, you can view near-real time snapshot of resource utilization (CPU, memory, network(in/out), etc.) on a gateway machine.  

1. In Azure portal, navigate to the home page for your data factory, and click **Linked services** tile. 

    ![Data factory home page](./media/data-factory-data-management-gateway/monitor-data-factory-home-page.png) 
2. Select the **gateway** in the **Linked services** page.

    ![Linked services page](./media/data-factory-data-management-gateway/monitor-linked-services-blade.png)
3. In the **Gateway** page, you can see the memory and CPU usage of the gateway.

    ![CPU and memory usage of gateway](./media/data-factory-data-management-gateway/gateway-simple-monitoring.png) 
4. Enable **Advanced settings** to see more details such as network usage.
    
    ![Advanced monitoring of gateway](./media/data-factory-data-management-gateway/gateway-advanced-monitoring.png)

The following table provides descriptions of columns in the **Gateway Nodes** list:  

Monitoring Property | Description
:------------------ | :---------- 
Name | Name of the logical gateway and nodes associated with the gateway. Node is an on-premises Windows machine that has the gateway installed on it. For information on having more than one node (up to four nodes) in a single logical gateway, see [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md).    
Status | Status of the logical gateway and the gateway nodes. Example: Online/Offline/Limited/etc. For information about these statuses, See [Gateway status](#gateway-status) section. 
Version | Shows the version of the logical gateway and each gateway node. The version of the logical gateway is determined based on version of majority of nodes in the group. If there are nodes with different versions in the logical gateway setup, only the nodes with the same version number as the logical gateway function properly. Others are in the limited mode and need to be manually updated (only in case auto-update fails). 
Available memory | Available memory on a gateway node. This value is a near real-time snapshot. 
CPU utilization | CPU utilization of a gateway node. This value is a near real-time snapshot. 
Networking (In/Out) | Network utilization of a gateway node. This value is a near real-time snapshot. 
Concurrent Jobs (Running/ Limit) | Number of jobs or tasks running on each node. This value is a near real-time snapshot. Limit signifies the maximum concurrent jobs for each node. This value is defined based on the machine size. You can increase the limit to scale up concurrent job execution in advanced scenarios, where CPU/memory/network is under-utilized, but activities are timing out. This capability is also available with a single-node gateway (even when the scalability and availability feature is not enabled).  
Role | There are two types of roles in a multi-node gateway – Dispatcher and worker. All nodes are workers, which means they can all be used to execute jobs. There is only one dispatcher node, which is used to pull tasks/jobs from cloud services and dispatch them to different worker nodes (including itself).

In this page, you see some settings that make more sense when there are two or more nodes (scale out scenario) in the gateway. See [Data Management Gateway - high availability and scalability](data-factory-data-management-gateway-high-availability-scalability.md) for details about setting up a multi-node gateway.

### Gateway status
The following table provides possible statuses of a **gateway node**: 

Status  | Comments/Scenarios
:------- | :------------------
Online | Node connected to Data Factory service.
Offline | Node is offline.
Upgrading | The node is being auto-updated.
Limited | Due to Connectivity issue. May be due to HTTP port 8050 issue, service bus connectivity issue, or credential sync issue. 
Inactive | Node is in a configuration different from the configuration of other majority nodes.<br/><br/> A node can be inactive when it cannot connect to other nodes. 


The following table provides possible statuses of a **logical gateway**. The gateway status depends on statuses of the gateway nodes. 

Status | Comments
:----- | :-------
Needs Registration | No node is yet registered to this logical gateway
Online | Gateway Nodes are online
Offline | No node in online status.
Limited | Not all nodes in this gateway are in healthy state. This status is a warning that some node might be down! <br/><br/>Could be due to credential sync issue on dispatcher/worker node. 

## Scale up gateway
You can configure the number of **concurrent data movement jobs** that can run on a node to scale up the capability of moving data between on-premises and cloud data stores. 

When the available memory and CPU are not utilized well, but the idle capacity is 0, you should scale up by increasing the number of concurrent jobs that can run on a node. You may also want to scale up when activities are timing out because the gateway is overloaded. In the advanced settings of a gateway node, you can increase the maximum capacity for a node. 
  

## Troubleshooting gateway issues
See [Troubleshooting gateway issues](data-factory-troubleshoot-gateway-issues.md) article for information/tips for troubleshooting issues with using the data management gateway.  

## Move gateway from one machine to another
This section provides steps for moving gateway client from one machine to another machine.

1. In the portal, navigate to the **Data Factory home page**, and click the **Linked Services** tile.

    ![Data Gateways Link](./media/data-factory-data-management-gateway/DataGatewaysLink.png)
2. Select your gateway in the **DATA GATEWAYS** section of the **Linked Services** page.

    ![Linked Services page with gateway selected](./media/data-factory-data-management-gateway/LinkedServiceBladeWithGateway.png)
3. In the **Data gateway** page, click **Download and install data gateway**.

    ![Download gateway link](./media/data-factory-data-management-gateway/DownloadGatewayLink.png)
4. In the **Configure** page, click **Download and install data gateway**, and follow instructions to install the data gateway on the machine.

    ![Configure page](./media/data-factory-data-management-gateway/ConfigureBlade.png)
5. Keep the **Microsoft Data Management Gateway Configuration Manager** open.

    ![Configuration Manager](./media/data-factory-data-management-gateway/ConfigurationManager.png)    
6. In the **Configure** page in the portal, click **Recreate key** on the command bar, and click **Yes** for the warning message. Click **copy button** next to key text that copies the key to the clipboard. The gateway on the old machine stops functioning as soon you recreate the key.  

    ![Recreate key](./media/data-factory-data-management-gateway/RecreateKey.png)
7. Paste the **key** into text box in the **Register Gateway** page of the **Data Management Gateway Configuration Manager** on your machine. (optional) Click **Show gateway key** check box to see the key text.

    ![Copy key and Register](./media/data-factory-data-management-gateway/CopyKeyAndRegister.png)
8. Click **Register** to register the gateway with the cloud service.
9. On the **Settings** tab, click **Change** to select the same certificate that was used with the old gateway, enter the **password**, and click **Finish**.

   ![Specify Certificate](./media/data-factory-data-management-gateway/SpecifyCertificate.png)

   You can export a certificate from the old gateway by doing the following steps: launch Data Management Gateway Configuration Manager on the old machine, switch to the **Certificate** tab, click **Export** button and follow the instructions.
10. After successful registration of the gateway, you should see the **Registration** set to **Registered** and **Status** set to **Started** on the Home page of the Gateway Configuration Manager.

## Encrypting credentials
To encrypt credentials in the Data Factory Editor, do the following steps:

1. Launch web browser on the **gateway machine**, navigate to [Azure portal](http://portal.azure.com). Search for your data factory if needed, open data factory in the **DATA FACTORY** page and then click **Author & Deploy** to launch Data Factory Editor.   
2. Click an existing **linked service** in the tree view to see its JSON definition or create a linked service that requires a data management gateway (for example: SQL Server or Oracle).
3. In the JSON editor, for the **gatewayName** property, enter the name of the gateway.
4. Enter server name for the **Data Source** property in the **connectionString**.
5. Enter database name for the **Initial Catalog** property in the **connectionString**.    
6. Click **Encrypt** button on the command bar that launches the click-once **Credential Manager** application. You should see the **Setting Credentials** dialog box.

    ![Setting credentials dialog](./media/data-factory-data-management-gateway/setting-credentials-dialog.png)
7. In the **Setting Credentials** dialog box, do the following steps:
   1. Select **authentication** that you want the Data Factory service to use to connect to the database.
   2. Enter name of the user who has access to the database for the **USERNAME** setting.
   3. Enter password for the user for the **PASSWORD** setting.  
   4. Click **OK** to encrypt credentials and close the dialog box.
8. You should see a **encryptedCredential** property in the **connectionString** now.

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
Pokud máte přístup k portálu z počítače, který se liší od počítače brány, musí se ujistěte, že aplikace Správce přihlašovacích údajů můžete připojit k počítači s bránou. Pokud aplikace nebudou moci připojit počítače brány, neumožňuje vám umožní nastavit přihlašovací údaje pro zdroj dat a otestovat připojení ke zdroji dat.  

Při použití **nastavení pověření** aplikace portálu šifruje přihlašovací údaje s certifikát uvedený v **certifikát** kartě **Správce konfigurace brány**  na počítači s bránou.

Pokud hledáte způsob založené na rozhraní API pro šifrování přihlašovacích údajů, můžete použít [New-AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) rutiny prostředí PowerShell k šifrování přihlašovacích údajů. Rutina používá certifikát tuto bránu Pokud chcete použít k šifrování přihlašovacích údajů. Přidat zašifrované přihlašovací údaje k **EncryptedCredential** element **connectionString** v kódu JSON. Použití formátu JSON s [New-AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) rutiny nebo v editoru služby Data Factory.

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

Neexistuje jeden další postup pro nastavení přihlašovacích údajů pomocí editoru služby Data Factory. Pokud vytvoříte službu SQL Server propojené pomocí editoru a zadejte přihlašovací údaje ve formátu prostého textu, přihlašovací údaje jsou šifrované pomocí certifikátu, který vlastní službu Data Factory. Nepoužívá certifikát této brány je nakonfigurovaný na použití. Tento přístup může být v některých případech trochu rychlejší, je to méně bezpečné. Proto doporučujeme, postupujte podle tohoto přístupu pouze pro účely vývoje/testování.

## <a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell
Tato část popisuje, jak vytvořit a registrovat bránu pomocí rutin prostředí Azure PowerShell.

1. Spusťte **prostředí Azure PowerShell** v režimu správce.
2. Přihlaste se k účtu Azure tak, že spustíte následující příkaz a zadání přihlašovacích údajů Azure.

    ```PowerShell
    Login-AzureRmAccount
    ```
3. Použití **New-AzureRmDataFactoryGateway** rutiny pro vytvoření logické brány následujícím způsobem:

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

1. V prostředí Azure PowerShell přejděte do složky: **C:\Program Files\Microsoft Data správy Gateway\2.0\PowerShellScript\**. Spustit **RegisterGateway.ps1** přidružený k místní proměnné **$Key** jak je znázorněno v následujícím příkazu. Tento skript zaregistruje Klientský agent nainstalovaný na počítači s logické brány, kterou vytvoříte dříve.

    ```PowerShell
    PS C:\> .\RegisterGateway.ps1 $MyDMG.Key
    ```
    ```
    Agent registration is successful!
    ```
    Pomocí parametru IsRegisterOnRemoteMachine můžete registrovat bránu ve vzdáleném počítači. Příklad:

    ```PowerShell
    .\RegisterGateway.ps1 $MyDMG.Key -IsRegisterOnRemoteMachine true
    ```
2. Můžete použít **Get-AzureRmDataFactoryGateway** rutiny získat seznam bran v datové továrně. Když **stav** ukazuje **online**, znamená to vaše brána je připravený k použití.

    ```PowerShell        
    Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF
    ```
Můžete odebrat brány pomocí **odebrat AzureRmDataFactoryGateway** brány pomocí rutiny a aktualizace popis **Set-AzureRmDataFactoryGateway** rutiny. Syntaxe a další podrobnosti o těchto rutinách najdete v tématu referenční Data Factory.  

### <a name="list-gateways-using-powershell"></a>Seznam brány pomocí prostředí PowerShell

```PowerShell
Get-AzureRmDataFactoryGateway -DataFactoryName jasoncopyusingstoredprocedure -ResourceGroupName ADF_ResourceGroup
```

### <a name="remove-gateway-using-powershell"></a>Odebrat bránu pomocí prostředí PowerShell

```PowerShell
Remove-AzureRmDataFactoryGateway -Name JasonHDMG_byPSRemote -ResourceGroupName ADF_ResourceGroup -DataFactoryName jasoncopyusingstoredprocedure -Force
```


## <a name="next-steps"></a>Další kroky
* V tématu [přesun dat mezi místní a cloudové úložiště dat](data-factory-move-data-between-onprem-and-cloud.md) článku. V tomto návodu vytvoříte kanál, který používá bránu pro přesun dat z databáze SQL serveru místně do objektu blob Azure.  
