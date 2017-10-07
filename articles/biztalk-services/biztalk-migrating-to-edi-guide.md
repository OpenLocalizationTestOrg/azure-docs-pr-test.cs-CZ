---
title: "BizTalk Server EDI řešení tooBizTalk aaaMigrating technické příručce služby | Microsoft Docs"
description: Migrace EDI tooMABS; Microsoft Azure BizTalk Services
services: biztalk-services
documentationcenter: na
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 61c179fa-3f37-495b-8016-dee7474fd3a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 34cca3c939a6a7845a860ead6858287000d03ee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-biztalk-server-edi-solutions-toobiztalk-services-technical-guide"></a>Migrace služeb tooBizTalk řešení BizTalk serveru EDI: technické příručce

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Autor: Tim Wieman a Nitin Mehrotra

Recenzenti: Karthik Bharthy

Vytvořené pomocí: verze služby Microsoft Azure BizTalk Services – únor 2014.

## <a name="introduction"></a>Úvod
Elektronické Data Interchange (EDI) je jedním ze současných převažujících znamená hello, podle kterého podnikům vyměňovat data elektronicky, také označovaných jako Business-to-Business nebo B2B transakce. BizTalk Server má obsahoval pro podporu EDI přes deset od vydání hello počáteční BizTalk serveru. Službou BizTalk pokračuje Microsoft hello podpora EDI řešení na platformě Microsoft Azure hello. Transakce B2B jsou většinou externí tooan organizace, a proto je snazší tooimplement Pokud již byl implementován na cloudové platformy. Tuto možnost prostřednictvím služby BizTalk Services nabízí Microsoft Azure.

Když někteří zákazníci podívejte se na službu BizTalk Services jako platforma "greenfield" nové řešení EDI, mít mnoho zákazníků aktuální řešení BizTalk serveru EDI, že chtějí toomigrate tooAzure. Protože EDI služby BizTalk je navrženou na základě hello stejný klíč entity jako architektura BizTalk serveru EDI (obchodních partnerů, entity, smluv), je možné toomigrate BizTalk serveru EDI artefakty tooBizTalk služeb.

Tento dokument popisuje některé rozdíly hello spojené s migraci artefaktů tooBizTalk EDI serveru BizTalk Services. Tento dokument předpokládá praktické znalosti zpracování EDI BizTalk serveru a Trading Partner smlouvy. Další informace o EDI BizTalk serveru najdete v tématu [Trading Partner Management pomocí BizTalk Server](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-toobiztalk-services"></a>Která verze BizTalk serveru EDI artefaktů můžete migrovat tooBizTalk služby?
BizTalk Server EDI modulu Hello výraznému vylepšení pro BizTalk Server 2010, při Změna modelu tooinclude partnerů, profilů a smlouvy. BizTalk Services používá hello stejný model tooorganize hello obchodních partnerů a hello firmy divizemi v rámci ty obchodních partnerů. V důsledku toho EDI migraci artefaktů z BizTalk Server 2010 a novější verze tooBizTalk služby, je mnohem víc splněny následující proces. toomigrate EDI artefakty související s předchozí verzí tooBizTalk Server 2010, musíte nejdřív upgradovat tooBizTalk Server 2010 a pak proveďte znovu migraci artefaktů EDI tooBizTalk služby.

## <a name="scenariosmessage-flow"></a>Tok scénáře/zpráv
Jako BizTalk Server EDI zpracování ve službě BizTalk Services vytvořené kolem řešení Trading Partner Management (TPM). Hello TPM řešení má hello následující klíčové komponenty:

* Obchodních partnerů, které představují organizaci ve B2B transakce.
* Profily, které představují divizemi v rámci obchodního partnera.
* Trading partner smlouvy (nebo smluv), které představují hello firmy smlouvu mezi dva partneři nebo profily.

Hello následující obrázek znázorňuje hello podobnosti a také rozdíly mezi řešení BizTalk serveru EDI a řešení BizTalk Services EDI:

![][EDImessageflow]

Hello hlavní rozdíly a podobnosti mezi tok EDI řešení BizTalk serveru, a jsou služby BizTalk Services:

* Stejně jako BizTalk Server používá tooreceive kanálu EDIReceive zprávu EDI a EDISend kanálu toosend zprávu EDI, BizTalk Services používá EDI přijímat tooreceive most a odeslat EDI most toosend out EDI zprávy. BizTalk serveru hello kanálů jsou přidružené smlouvu pomocí odesílat nebo přijímat porty. Ve službě BizTalk Services hello smlouvu, samotné označuje hello odesílat nebo přijímat most.
* BizTalk serveru po hello EDIReceive kanálu procesy hello EDI zprávu, zpráva hello je dumpingových tooa databáze systému SQL Server. kanál EdiSend Hello pak převezme uvítací zprávu z databáze serveru SQL Server hello, procesy a odešle ji na toohello obchodního partnera.
  
    Ve službě BizTalk Services po hello EDI zobrazí zpráva most procesy hello EDI, směruje hello zpráva tooan externího procesu. Hello externího procesu může být spuštěn v Microsoft Azure nebo místně. Hello externího procesu by měl směrovat toohello zpráva hello EDI odeslat most; Most odesílání Hello ze své podstaty nestahuje uvítací zprávu. Po zpracování zprávy hello směruje hello EDI odeslat most hello zpráva toohello obchodního partnera.

Služba BizTalk Services nabízí snadno použít konfiguraci prostředí tooquickly vytvořit a nasadit smlouvy B2B mezi obchodních partnerů bez konfigurace žádné Microsoft Azure Compute instance (role Web nebo Worker), všechny databáze SQL Azure Microsoft nebo žádné Účty úložiště Microsoft Azure. Složitější scénáře bude vyžadovat příkazů v pracovních postupech nebo další zpracování služby "okrajů hello" obchodování partnerskou smlouvu, který je před nebo po zpracování most Trading Partner smlouvy EDI. Podrobně hello následující pořadí událostí dojde během zprávu EDI zpracování ve službě BizTalk Services.

1. Zprávu EDI byl přijat z obchodního partnera, Fabrikam.  Pro příjem zpráv EDI z obchodními partnery, BizTalk Services podporuje protokoly s transportní například FTP, SFTP, AS2 a HTTP/S.
2. Hello trading partner smlouvy škálování na straně zpracování provede zpětný překlad formát tooXML zprávy EDI hello.  Je možné směrovat koncové body hello rozložit EDI zprávy (ve formátu XML) tooService Bus jako koncový bod předávání přes Service Bus, téma sběrnice, frontou Service Bus nebo mostu BizTalk Services.
3. Hello bylo zpráv XML může pak přijímat z hello koncového bodu pro další vlastní zpracování.  Tyto koncové body nebylo možno zpracovat komponentu místní nebo Microsoft Azure Compute instance toofurther proces hello zprávu ve službě Windows Workflow (WF) nebo Windows Communication Foundation (WCF), např.
4. Hello "straně odesílání zpracování" hello smlouvy s obchodním partnerem pak sestaví uvítací zprávu XML do formátu EDI a odešle ji tootrading partner společnosti Contoso.  BizTalk Services pro odesílání zpráv EDI tootrading partnery, podporuje stejné protokoly jako používaných pro příjem zpráv EDI hello.

Další tento dokument obsahuje koncepční pokyny k migraci některých hello různých BizTalk serveru EDI artefakty tooBizTalk služeb.

## <a name="sendreceive-ports-tootrading-partners"></a>Porty pro odesílání a přijímání tooTrading partnery
BizTalk serveru nastavíte přijímat umístění a Receive porty tooreceive EDI/XML zprávy z obchodními partnery a nastavíte odeslání porty toosend EDI/XML zprávy tootrading partnera. Potom vytížit tyto porty tooa obchodování smlouvu pro partnery pomocí konzoly pro správu serveru BizTalk hello. Ve službě BizTalk Services, kterou budete dostávat zprávy z obchodními partnery a kde odeslat, že zprávy tootrading partneři jsou nakonfigurovány jako součást hello obchodování smlouvu pro partnery samostatně (jako součást nastavení přenosu) v umístění hello hello portál služby BizTalk .  Proto není nutné skutečně hello koncept "odesílání portů" a "přijímat umístění", samo o sobě, ve službě BizTalk Services. Další informace najdete v tématu [vytváření smlouvy](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Kanály (mostů)
Kanály v EDI BizTalk serveru, jsou entit zpracování zpráv, které může také obsahovat vlastní logiky pro zpracování specifické možnosti, podle potřeby aplikace hello. Služby BizTalk Services by hello ekvivalentní EDI most. Ale ve službě BizTalk Services zatím mostů EDI hello "zavřeny".  To znamená nelze přidat vlastní most EDI tooan vlastní aktivity. Všechny vlastní zpracování je třeba provést mimo hello EDI most ve vaší aplikaci, před nebo po uvítací zprávu zadá hello most nakonfigurovaný jako součást hello obchodování partnerskou smlouvu. Mosty EAI mít hello možnost toodo vlastní zpracování. Pokud chcete vlastní zpracování, můžete mosty EAI, před nebo po uvítací zprávu zpracovává most EDI hello. Další informace najdete v tématu [jak tooInclude vlastní kód na mostů](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Můžete vložit k publikování a přihlášení k odběru toku vlastního kódu a/nebo pomocí než hello obchodování partnerskou smlouvu obdrží uvítací zprávu nebo po hello smlouvy zpracovává uvítací zprávu a směruje je koncový bod Service Bus tooa pro zprávy fronty a témata sběrnice služby.

V tématu **scénáře/zprávy toku** v tomto tématu vzorce toku zpráv hello.

## <a name="agreements"></a>Smlouvy
Pokud jste obeznámeni s hello BizTalk Server 2010 Trading Partner smlouvy použity pro zpracování EDI, vyhledejte BizTalk Services smluv s obchodními partnery velmi dobře. Většina hello smlouvy jsou hello stejné nastavení a použití hello stejné terminologie. V některých případech hello smlouvy nastavení jsou mnohem jednodušší porovnání toohello stejné nastavení BizTalk serveru. Microsoft Azure BizTalk Services podporuje X12, EDIFACT a AS2 přenosu.

Služba Microsoft Azure BizTalk Services také poskytuje **migrace dat TPM** nástroj toomigrate obchodními partnery a smluv na BizTalk serveru Trading Partner modulu tooBizTalk portál služeb. Nástroj pro migraci dat TPM Hello je k dispozici jako součást balíčku nástrojů, který si můžete stáhnout z hello [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). balíček Hello také obsahuje soubor readme, který obsahuje pokyny, jak nástroj hello toouse a základní informace o řešení potíží hello nástroj.

## <a name="schemas"></a>Schémata
Služba BizTalk Services nabízí schémat EDI, které se dají použít v řešení BizTalk Services.  Kromě toho schémat BizTalk Server EDI lze také službou BizTalk Services protože hello kořenový uzel hello EDI schématu je stejná napříč BizTalk serveru, jakož i služba BizTalk Services. Proto bude se moct toodirectly proveďte vaší schémat EDI BizTalk serveru a použijte je v hello EDI řešení, které vyvíjíte pomocí služby BizTalk Services. Můžete také stáhnout hello schémata z hello [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Mapy (transformací)
Mapy BizTalk serveru, se nazývají transformací ve službě BizTalk Services. Migrace mapy služby BizTalk Server tooBizTalk, služby může být jeden z hello složitější úlohy tooachieve (v závislosti na složitosti mapy). Hello mapování nástroj používaný pro služby BizTalk se liší od hello BizTalk mapper. I když hello mapper vypadá většinou stejný Dobrý den, základní formát mapy hello se liší. Hello functoids (nazývá **mapy Operations** ve službě BizTalk Services) k dispozici toohello uživatelé se také liší.  V důsledku toho nelze použít přímo BizTalk mapy ve službě BizTalk Services. Ne všechny functoids hello k dispozici v BizTalk Server jsou také k dispozici jako mapy operace ve službě BizTalk Services.

### <a name="new-transform-operations"></a>Nové operace transformace.
Když se může zdát výrazně lišit od hello mapper BizTalk Server hello seznam operací na mapu transformace, které jsou k dispozici, transformuje služby BizTalk mít nové způsoby provádění hello stejné úlohy. Například transformuje služby BizTalk mít **operace výpisu** k dispozici. Nebyl k dispozici v hello BizTalk mapper.  Hello **operace výpisu** umožňují toocreate a provozovat na "Seznam", kde seznam je sada položek (také označované jako "řádky") a kde každá položka může mít více členů (také označované jako "sloupce").  Můžete řadit hello seznamu, vyberte položky na základě podmínky, atd.

Další příklad nové funkce v transformuje služby BizTalk se hello **smyčky Operations**.  Je obtížné toocreate vnořené smyčky v hello mapper BizTalk Server.  Proto hello smyčky mapy operace jsou přidány pro hello transformuje služby BizTalk.

Ještě dalším příkladem je hello **If-potom Else** operace mapy výrazu.  Provádění operace if potom else bylo možné v hello BizTalk mapper, ale je vyžadován více functoids tooaccomplish zdánlivě jednoduchou úlohou.

### <a name="migrating-biztalk-server-maps"></a>Migrace serveru BizTalk mapy
Microsoft Azure BizTalk Services poskytuje že nástroj toomigrate BizTalk Server mapuje tooBizTalk služby transformací. Hello **BTMMigrationTool** je k dispozici jako součást hello **nástroje** balíček součástí hello [stažení sady SDK služby BizTalk](http://go.microsoft.com/fwlink/p/?LinkId=235057). Další informace o nástroji hello najdete v tématu [převést tooa mapy BizTalk transformace služby BizTalk](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Můžete také zobrazit na ukázku podle Sandro Pereira, BizTalk MVP, na tom, jak příliš[migrovat BizTalk Server mapy tooBizTalk služby transformací](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orchestrations
Pokud potřebujete toomigrate BizTalk serveru orchestration zpracování tooMicrosoft Azure, hello orchestrations potřebovat toobe přepsaná, protože Microsoft Azure nepodporuje spuštěné orchestrations BizTalk Server.  Může přepisování funkce orchestration hello ve službě Windows Workflow Foundation 4.0 (WF4).  To může být dokončení přepisování není aktuálně žádná migrace z tooWF4 orchestrations BizTalk Server. Zde jsou některé prostředky pro pracovní postup systému Windows:

* [*Jak toointegrate pracovního postupu WCF služby s fronty služby Service Bus a témat* ](https://msdn.microsoft.com/library/azure/hh709041.aspx) podle Paolo Salvatori. 
* [*Vytváření aplikací s Windows Workflow Foundation a Azure* relace](http://go.microsoft.com/fwlink/p/?LinkId=237314) z konference hello sestavení 2011.
* [*Centrum vývojářů pro Windows Workflow Foundation* ](http://go.microsoft.com/fwlink/p/?LinkId=237315) na webu MSDN.
* [*Dokumentaci k Windows Workflow Foundation 4 (WF4)* ](https://msdn.microsoft.com/library/dd489441.aspx) na webu MSDN.

## <a name="other-considerations"></a>Další důležité informace
Toto jsou několik aspekty, které musíte provést při používání služby BizTalk Services.

### <a name="fallback-agreements"></a>Záložní smlouvy
BizTalk Server EDI zpracování má hello konceptu "Záložního smlouvy".  BizTalk Services nepodporuje **není** , pokud mají koncept záložního smlouvy.  Dokumentace tématech BizTalk [hello Role smlouvy při zpracování EDI](http://go.microsoft.com/fwlink/p/?LinkId=237317) a [záložního smlouvy vlastnosti nebo konfigurace globální](https://msdn.microsoft.com/library/bb245981.aspx) informace o používání záložního smluv v BizTalk Server.

### <a name="routing-toomultiple-destinations"></a>Směrování toomultiple cíle
BizTalk Services mostů v jejím aktuálním stavu nepodporuje toomultiple cílů směrování zpráv pomocí publikovat-přihlášení k odběru modelu. Místo toho může směrovat zprávy ze služby BizTalk Services most tooa tématu Service Bus, to může být více předplatných tooreceive uvítací zprávu na více než jeden koncový bod.

## <a name="conclusion"></a>Závěr
Microsoft Azure BizTalk Services je aktualizován v pravidelných milníky tooadd další funkce a možnosti. Při každé aktualizaci Těšíme toofacilitate funkce toosupporting vyšší vytváření řešení pro kompletní pomocí služby BizTalk Services a další technologie Azure.

## <a name="see-also"></a>Viz také
[Vývoj aplikací Enterprise s Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
