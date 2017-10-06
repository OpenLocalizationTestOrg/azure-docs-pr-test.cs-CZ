---
title: "Poznámky k aaaRelease služby Azure BizTalk Services | Microsoft Docs"
description: "Seznamy hello známé problémy pro služby Azure BizTalk Services"
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: ea53d6c40ed58badf4141453dc77d28dcfc6407f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-azure-biztalk-services"></a>Poznámky k verzi pro služby Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Poznámky k verzi Hello služby hello Microsoft Azure BizTalk Services obsahují hello známé problémy v této verzi.

## <a name="whats-new-in-hello-november-update-of-biztalk-services"></a>Co je nového v aktualizaci listopadu hello služby BizTalk Services
* Šifrování v klidovém stavu lze povolit v hello portál služby BizTalk. V tématu [povolit šifrování v klidovém stavu portálu služby BizTalk](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Aktualizace historie
### <a name="october-update"></a>Říjen aktualizace
* Účty organizace nejsou podporované:  
  * **Scénář**: jste zaregistrovali nasazení služby BizTalk pomocí účtu Microsoft (jako je user@live.com). V tomto scénáři hello pouze Account Microsoft můžou uživatelé spravovat služby BizTalk pomocí portálu služby BizTalk Services hello. Nelze použít účet organizace.  
  * **Scénář**: jste zaregistrovali nasazení služby BizTalk pomocí organizačního účtu v Azure Active Directory (například user@fabrikam.com nebo user@contoso.com). V tomto scénáři pouze uživatelů Azure Active Directory v rámci hello, které může spravovat stejné organizaci hello služby BizTalk pomocí portálu služby BizTalk Services hello. Nelze použít účet Microsoft.  
* Při vytváření služby BizTalk v hello portál Azure classic, se automaticky registruje v hello portál služby BizTalk.
  * **Scénář**: Přihlaste hello portál Azure classic, vytvoření služby BizTalk a pak vyberte **spravovat** pro hello velmi poprvé. Když se otevře portál služby BizTalk Services hello, hello služba BizTalk automaticky registruje a je připravený pro vaše nasazení.  
    V tématu [registrace a aktualizace v nasazení služby BizTalk hello portál služeb BizTalk](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Srpen 14 aktualizace
* Smlouvy a most oddělení – obchodní partnery smlouvy a mostů jsou nyní odpojené v hello portál služby BizTalk. Teď vytvořte smlouvy a mostů samostatně a v modulu runtime mostů vyřešit tooan smlouvy založené na hodnotách hello ve zprávě EDI hello. V tématu [vytvoření smluv ve službě Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689908.aspx), [vytvořit mostu EDI pomocí portálu služby BizTalk](https://msdn.microsoft.com/library/azure/dn793986.aspx), [vytvořit mostu AS2 pomocí portálu služby BizTalk](https://msdn.microsoft.com/library/azure/dn793993.aspx), a [jak mostů vyřešit smlouvy za běhu?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Hello možnost toocreate šablony pro smlouvy je přerušeno.  
* Pro hello straně odesílání smlouvu nyní můžete určit jiný oddělovač sady pro každé schéma. Tato konfigurace je zadaný v nastavení protokolu pro smlouvu straně odeslání. Další informace najdete v tématu [vytvořit na X12 smluv ve službě Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) a [vytvořili smlouvu EDIFACT v Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Dva nové entity jsou rovněž přidány toohello TPM OM rozhraní API pro hello stejnému účelu. V tématu [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) a [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standardní XSD konstrukce, včetně odvozené typy jsou nyní podporovány. V tématu [použijte standardní XSD vytvoří ve vašem mapy](https://msdn.microsoft.com/library/azure/dn793987.aspx) a [použití odvozené typy v mapování scénáře a příklady](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 podporuje nových algoritmů povinná kontrola úrovně Důvěryhodnosti pro podepisování zpráv a nový šifrovací algoritmus. V tématu [vytvořit ve službě Azure BizTalk Services smlouvu AS2](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Vědět problémy

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Problémy s připojením k po aktualizaci portálu služby BizTalk Services
  Pokud máte hello otevřete portál služby BizTalk při upgradu služby BizTalk Services tooroll v změní toohello služby, může mít potíže připojení s hello portál služby BizTalk.  
  Jako alternativní řešení může restartujte prohlížeč hello, odstraňte hello mezipaměti prohlížeče nebo spustit portál hello v privátním režimu.  

### <a name="visual-studio-ide-cannot-locate-hello-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE nelze najít artefakt hello, pokud kliknete na tlačítko chybě nebo upozornění v projektu služby BizTalk Services
Nainstalujte Visual Studio 2012 Update 3 RC 1 hello toofix hello problém.  

### <a name="custom-binding-project-reference"></a>Odkaz na projekt vlastní vazby
Vezměte v úvahu hello následující situace s projektem v řešení sady Visual Studio BizTalk Services:  

* V hello stejné řešení sady Visual Studio, je projektu služby BizTalk Services a projekt vlastní vazby. Hello projektu služby BizTalk má soubor projektu odkaz toothis vlastní vazby.
* Hello projektu služby BizTalk má odkaz tooa vlastní vazby/chování knihovny DLL.

Úspěšně jste 'Sestavení' hello řešení v sadě Visual Studio. Pak 'Znovu sestavte' nebo Vyčistit řešení hello. Poté když znovu sestavit nebo vyčistit znovu hello následující, dojde k chybě:  
  Soubor nelze toocopy <Path tooDLL> too"bin\Debug\FileName.dll". Hello proces nemá přístup hello souboru 'bin\Debug\FileName.dll', protože je stále používán jiným procesem.  

#### <a name="workaround"></a>Alternativní řešení
* Pokud [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) je nainstalovaná, máte hello následující dvě možnosti:
  
  * Restartujte Visual Studio, nebo
  * Restartujte hello řešení. Potom proveďte pouze sestavení na hello řešení.  
* Pokud [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) je nainstalovaná není, otevřete Správce úloh, klikněte na kartu hello procesy, klikněte na tlačítko hello MSBuild.exe proces a pak klikněte na tlačítko Ukončit proces hello.  

### <a name="routing-toobasichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Směrování tooBasicHttpRelay koncových bodů není podporováno z: mostů a portál služby BizTalk Pokud povýšené netisknutelné znaky jako hlavičky protokolu HTTP
Pokud používáte netisknutelné znaky jako součást propagovaných vlastností pro zprávy, nelze tyto zprávy směrované toorelay cíle, které hello BasicHttpRelay vazbu používají. Navíc hello povýší vlastnosti, které jsou k dispozici jako součást sledování jsou kódování URL pro objekty BLOB a zrušení kódovaného pro cílů.  

### <a name="mdn-is-sent-asynchronously-even-if-hello-send-asynchronous-mdn-option-is-unchecked"></a>MDN je odeslán asynchronně, i když hello odeslání není zaškrtnuta možnost asynchronní MDN
Vezměte v úvahu tento scénář – Pokud vyberete hello **odesílat asynchronní MDN** zaškrtávací políčko a zadejte adresu URL toosend hello asynchronní MDN na a potom zrušte zaškrtnutí políčka hello **odesílat asynchronní MDN** políčko znovu hello MDN je stále Odeslané toohello zadaná adresa URL, přestože hello možnost toosend asynchronní MDNs není vybraná.  
Jako alternativní řešení, musíte vymazat hello před zaškrtnutí políčka hello zadat adresu URL **odesílat asynchronní MDN** zaškrtávací políčko a potom nasadíte hello AS2 smlouvy.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-toobe-sent-toohello-suspend-endpoint"></a>Prázdné znaky následující po platný výměnu příčina toohello toobe odeslané prázdná zpráva pozastavit koncový bod
Pokud jsou prázdné znaky nad rámec IEA segment, hello disassembler považován za konec aktuální výměnu a zjistí hello další sadu prázdné znaky jako další zprávy. Vzhledem k tomu, že to není platný výměnu, můžete všimnout, že jednu úspěšné zprávu je toohello trasy cílové jednu prázdnou zprávu posílat a hello pozastavit koncový bod.  

### <a name="tracking-in-biztalk-services-portal"></a>Sledování na portálu služby BizTalk
Zpracování zpráv toohello EDI a všechny korelace zaznamenání sledování událostí. V případě selhání zprávu mimo hello fáze protokol sledování se zobrazí jako úspěšně dokončený. V této situaci, najdete v části toohello protokolu v části hello **podrobnosti** sloupec v **sledování** podrobnosti chyby.
Hello X12 příjem a odesílání nastavení ([vytvořit na X12 smluv ve službě Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)) poskytují informace o hello fáze protokolu.  

### <a name="update-agreement"></a>Smlouva pro aktualizaci
Hello portál služby BizTalk můžete toomodify hello kvalifikátor Identity smlouvu konfigurována. Výsledkem může být inconsistence vlastnosti. Například je smlouvu pomocí ZZ:1234567 a ZZ:7654321 hello kvalifikátor. V nastavení profilu hello portál služby BizTalk můžete změnit ZZ:1234567 toobe 01:ChangedValue. Otevřete hello smlouvy a zobrazí se místo ZZ:1234567 01:ChangedValue.
Aktualizovat toomodify hello kvalifikátor svou identitu, odstraňte hello smlouvy, **identity** v hello partnera profil a pak znovu vytvořte hello smlouvy.  

> AZURE. UPOZORNĚNÍ toto chování ovlivňuje X12 a AS2.  
> 
> 

### <a name="as2-attachments"></a>AS2 příloh
Příloh pro AS2 zprávy nejsou podporovány v odeslat nebo přijmout. Konkrétně přílohy jsou bez upozornění ignorovat a tělo zprávy hello zpracovávány jako regulární zprávu AS2.  

### <a name="resources-remembering-path"></a>Zdroje: Zapamatování cestu
Při přidávání **prostředky**, hello dialogového okna nemusí pamatovat cestu použil tooadd hello prostředku. tooremember hello dřív používal cestu, zkuste přidat hello portál služeb BizTalk webové stránky příliš**Důvěryhodné servery** v aplikaci Internet Explorer.  

### <a name="if-you-rename-hello-entity-name-of-a-bridge-and-close-hello-project-without-saving-changes-opening-hello-entity-again-results-in-an-error"></a>Pokud přejmenujete název entity hello most a zavřít hello projekt bez uložení změn, znovu otevřít hello entity výsledkem chyba
Představte si třeba situaci v hello následující pořadí:  

* Přidat most (například most XML One-Way) tooa projektu služby BizTalk  
* Přejmenujte hello most zadáním hodnoty pro hello vlastnost název Entity. To přejmenuje soubor přidružené .bridgeconfig hello s hello název, který jste zadali.  
* Zavřete soubor .bcs hello (ukončením hello karta v sadě Visual Studio) bez uložení změn hello.  
* Hello .bcs soubor otevřít znovu hello Průzkumníku řešení.  
  Si všimnete, že při hello přidružené .bridgeconfig soubor má hello nový název, který jste zadali, název entity hello hello návrhové ploše je stále hello starým názvem. Pokud se pokusíte tooopen hello konfigurace mostu dvojitým kliknutím na soubor hello most součást, získáte hello následující chybě:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist`tooavoid spuštěna v tomto scénáři, ujistěte se, že po přejmenování hello entity v projektu služby BizTalk se uložit změny.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Sestavení projektu služby BizTalk se úspěšně, i když artefakt byl vyloučen z projektu sady Visual Studio
Vezměte v úvahu scénář, kde přidat artefaktů (například soubor XSD) tooa projektu služby BizTalk, zahrnout tento artefaktů hello most konfigurace (například zadáním ji jako typ zprávy požadavku) a vyloučíte je z projektu sady Visual Studio hello. V takovém případě sestavení projektu hello nebude poskytnout všechny chyby, dokud je k dispozici na disku hello hello hello odstranit artefaktů stejné umístění, ze kterých je zahrnutý v projektu sady Visual Studio hello.
  
### <a name="hello-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-hello-bridges"></a>Hello projektu služby BizTalk nekontroluje dostupnost schématu při konfiguraci mostů hello
V projektu služby BizTalk Pokud schéma, které se přidá toohello projektu importuje jiné schéma, hello projektu služby BizTalk nekontroluje, zda text hello importované schéma je přidat toohello projektu. Pokud se pokusíte toobuild tohoto projektu, se nezobrazí žádné chyby sestavení.
  
### <a name="hello-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>uvítací zprávu odpovědi pro požadavek-odpověď mostu XML je vždy charset UTF-8
Pro tuto verzi hello charset hello zprávy s odpovědí z požadavku a odpovědi mostu XML je vždycky nastavený tooUTF-8.
  
### <a name="user-defined-datatypes"></a>Uživatelem definované datové typy
Hello sady BizTalk Adapter Pack adaptéry v rámci funkce hello služba BizTalk Adapter Service můžete využít uživatelem definované datové typy pro operace adaptéru.
Při použití uživatelem definované datové typy, zkopírujte soubory (.dll) toodrive:\Program Files\Microsoft hello Service\BAServiceRuntime\bin\ adaptér BizTalk nebo toohello globální mezipaměti sestavení (GAC) na serveru hello hostitelem služby pro služby BizTalk Adapter Service hello. Hello následující chyba, jinak může dojít v klientovi hello:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">hello UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing hello assembly containing hello UDT definition in hello Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Je doporučené toouse GACUtil.exe tooinstall soubor do hello globální mezipaměti sestavení. GACUtil.exe dokumenty jak toouse tento nástroj a hello možnosti příkazového řádku Visual Studio.  
> 
> 

### <a name="restarting-hello-biztalk-adapter-service-web-site"></a>Restartování hello webu služby BizTalk adaptéru
Instalaci hello **běh služby BizTalk adaptér*** vytvoří hello **služba BizTalk Adapter Service** web služby IIS, který obsahuje hello **BAService** aplikace. **BAService** aplikace interně používá předávací vazby tooextend hello reach místní služby koncový bod toohello cloudu. Pouze v případě, že hello místní služba spustí, bude pro služby hostované na místních počítačích registrovaná hello odpovídající předávání přes koncový bod na hello Service Bus.  

Je-li zastavit a spustit aplikaci, není dodržení hello konfiguraci pro automatické spuštění aplikace. Takže když **BAService** je zastavena, vždy je nutné restartovat hello **služba BizTalk Adapter Service** místo webový server. Spuštění nebo zastavení hello **BAService** aplikace.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>Speciální znaky se nesmí používat pro adresu a entity názvy součástí LOB
Pro adresu a entity názvy součástí LOB byste neměli používat speciální znaky. Pokud tak učiníte, zobrazí se chyba při nasazování hello projektu služby BizTalk. Pro některé znaků jako '%', hello webu služby BizTalk Adapter Service může přejít do stavu ukončeno a bude mít toomanually spusťte ji.

### <a name="test-map-with-get-context-property"></a>Mapa test s vlastností kontextu Get
Pokud obsahuje transformace **získat vlastností kontextu** operace mapy, **testovací mapy** se nezdaří. Jako dočasné řešení, nahraďte hello **získat vlastností kontextu** mapy operaci s operací řetězení mapy řetězec obsahující fiktivní data. Tato akce naplnit hello cílové schéma a povolit, že byste otestovat další funkce transformace.

### <a name="test-map-property-does-not-display"></a>Vlastnost mapy testovací nezobrazí.
Hello **testovací mapy** vlastnosti nezobrazovat v sadě Visual Studio. Tato situace může nastat, pokud hello **vlastnosti** okno a hello **Průzkumníku řešení** okno nejsou ukotveno současně. tooresolve se ukotvení hello **vlastnosti** a hello **Průzkumníku řešení** systému windows.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Je zobrazeno šedě rozevíracího seznamu přeformátujte data a času
Při přidání mapy operace přeformátujte DateTime toohello návrh prostor a konfiguraci, hello formátu rozevíracího seznamu může být neaktivní. K tomu může dojít, pokud počítače hello zobrazení **Střední – 125 %** nebo **větší – 150 %**. tooresolve, nastavte zobrazení hello příliš**menší – 100 % (výchozí)** pomocí následujících kroků hello:  

1. Otevřete hello **ovládací panely** a klikněte na tlačítko **vzhled a přizpůsobení**.
2. Klikněte na tlačítko **zobrazení**.
3. Klikněte na tlačítko **menší – 100 % (výchozí)** a klikněte na tlačítko **použít**.

Hello **formát** rozevíracího seznamu by měl nyní fungovat podle očekávání.

### <a name="duplicate-agreements-in-hello-biztalk-services-portal"></a>Duplicitní smluv v hello portál služby BizTalk
Vezměte v úvahu hello následující scénář:

1. Vytvořte smlouvu pomocí rozhraní API OM hello Trading Partner Management.
2. Otevřete smlouvu hello v hello portál služeb BizTalk na dvou různých kartách.
3. Nasaďte hello smlouvy z obou hello karty.
4. V důsledku toho obě hello získat nasazení smluv výsledkem duplicitní položky v hello portál služby BizTalk

**Alternativní řešení**. Otevřete některého duplicitní dohod hello v hello portál služby BizTalk a zrušit nasazení.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-hello-artifact-store"></a>Mosty nepoužívejte aktualizovaný certifikát i po aktualizaci se certifikát v úložišti artefaktů hello
Vezměte v úvahu hello následující scénáře:  

**Scénář 1: Použití certifikátů na základě kryptografického otisku pro přenos zpráv z koncového bodu služby tooa most zabezpečení**  
Vezměte v úvahu scénář, kde používat certifikáty založené na kryptografický otisk ve vašem projektu služby BizTalk. Aktualizujete hello certifikátu v hello portál služeb BizTalk s hello stejný název ale jiný kryptografický otisk, ale neaktualizují hello projektu služby BizTalk odpovídajícím způsobem. V takovém scénáři může hello most pokračovat tooprocess hello zprávy, protože starší data certifikátu hello nemusí být ještě v mezipaměti kanál hello. Potom se nezdaří zpracování zprávy.  

**Alternativní řešení**: aktualizovat certifikát hello v hello projektu služby BizTalk a znovu nasaďte projekt hello.  

**Scénář 2: Použití certifikátů tooidentify chování na základě názvu pro přenos zpráv z koncového bodu služby tooa most zabezpečení**

Představte si třeba situaci, kdy používáte chování na základě názvu tooidentify certifikáty ve vašem projektu služby BizTalk. Aktualizovat certifikát hello v hello portál služeb BizTalk však neaktualizují hello projektu služby BizTalk odpovídajícím způsobem. V takovém scénáři může hello most pokračovat tooprocess hello zprávy, protože starší data certifikátu hello nemusí být ještě v mezipaměti kanál hello. Potom se nezdaří zpracování zprávy.  

**Alternativní řešení**: aktualizovat certifikát hello v hello projektu služby BizTalk a znovu nasaďte projekt hello.  

### <a name="bridges-continue-tooprocess-messages-even-when-hello-sql-database-is-offline"></a>Mosty pokračovat tooprocess zprávy i v případě, že databáze SQL hello je offline
BizTalk Services mostů Hello pokračovat tooprocess zprávy nějakou dobu, i v případě, že hello Microsoft Azure SQL Database (která ukládá hello systémem informace, jako jsou nasazené artefaktů a kanálů), je offline. Je to proto, že služba BizTalk Services používá artefakty hello do mezipaměti a konfigurace mostu.
Pokud nechcete, aby hello mostů tooprocess všechny zprávy při hello databáze SQL je offline, můžete použít toostop rutiny prostředí PowerShell služby BizTalk hello nebo pozastavit hello služby BizTalk. V tématu [Ukázka správy služby Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=329019) operacím toomanage rutiny prostředí Windows PowerShell hello.  

### <a name="reading-hello-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Čtení hello XML zprávy v rámci komponenty vlastního kódu most je i znak navíc BOM
Vezměte v úvahu scénář, kde chcete tooread zprávu XML v rámci vlastního kódu most. Pokud používáte hello System.Text.Encoding.UTF8.GetString(bytes) rozhraní API .NET je jeden znak navíc BOM součástí výstup hello na začátku hello uvítací zprávu. Pokud nechcete, aby hello tooinclude výstup hello tedy znak BOM navíc, musíte použít ```System.IO.StreamReader().ReadToEnd()```.

### <a name="sending-messages-tooa-bridge-using-wcf-does-not-scale"></a>Odesílání zpráv pomocí WCF most tooa se nedá použít.
Zprávy odeslané tooa most pomocí WCF se nedá použít. Místo toho byste měli používat HttpWebRequest – Pokud chcete, aby škálovatelné klienta.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-toogeneral-availability-ga"></a>UPGRADE: Informace o chybě zprostředkovatele tokenu po provedení upgradu z verze Preview služby BizTalk tooGeneral dostupnosti (GA)
S active dávky je EDI nebo smlouvy AS2. Když hello služba BizTalk je upgradovali z verze Preview tooGA, může dojít k hello následující:

* Chyba: Poskytovatel tokenu hello byl nelze tooprovide tokenu zabezpečení. Zprostředkovatel tokenu vrátil zprávu: hello vzdálený název nelze rozpoznat.
* Úkoly služby batch, se zruší.

**Alternativní řešení**: po hello služba BizTalk je aktualizovaný tooGeneral dostupnosti (GA), znovu nasaďte hello smlouvy.  

### <a name="upgrade-toolbox-shows-hello-old-bridge-icons-after-upgrading-hello-biztalk-services-sdk"></a>UPGRADE: Sada nástrojů zobrazí hello staré most ikony po upgradu hello sady SDK služby BizTalk
Po upgradu na dřívější verzi hello BizTalk Services SDK, která měla původní ikony představující mostů hello, pokračuje hello nástrojů tooshow hello staré ikony pro mostů hello. Ale pokud přidáte mostu tooBizTalk služby projektu plochu návrháře, hello prostor ukazuje hello ikonu nový.  

**Alternativní řešení**. Tento problém můžete vyřešit odstraněním hello .tbd soubory v <system drive>: \Users\<uživatele > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-tooga-might-show-an-error-indicating-that-hello-edi-capability-is-not-available"></a>UPGRADE: Aktualizace BizTalk portálu z verze Preview tooGA může zobrazit chyba oznamující, že tento hello EDI funkce není k dispozici
Pokud jste přihlášeni do hello portál služeb BizTalk hello BizTalk Services je upgradu z verze Preview tooGA, může získat hello na portálu hello následující chybě:  

Tato funkce není k dispozici jako součást této edici služby Microsoft Azure BizTalk Services. toouse tyto možnosti přepnout tooan příslušnou verzi.  

**Řešení**: při odhlášení z portálu hello, hello zavřete a otevřete prohlížeč a přihlaste se k portálu hello.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-tooga"></a>UPGRADE: Nová data sledování nezobrazuje upgradovaný tooGA po BizTalk Services
Předpokládejme situaci, kdy máte XML most nasazené v předplatném Preview služby BizTalk. Odesílání zpráv toohello most a hello odpovídající sledování dat je k dispozici na hello portál služby BizTalk. Nyní, pokud bity runtime hello portál služby BizTalk a BizTalk Services jsou upgradovaný tooGA a odeslat zprávu toohello, stejný koncový bod most předtím, hello data sledování nezobrazuje pro zprávy odeslané po upgradu.  

### <a name="pipelines-versus-bridges"></a>Kanály versus mostů
V tomto dokumentu se hello termín 'kanálů' a 'mostů' používají zcela zaměnitelným významem. I v podstatě znamená hello stejné věcí, který je nasazen na služby BizTalk Services jednotku zpracování zprávy.  

### <a name="concepts"></a>Koncepty
[BizTalk Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   

