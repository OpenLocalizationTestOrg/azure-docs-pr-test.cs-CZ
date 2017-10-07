---
title: "Průvodce aaaTroubleshooting pro Azure Stream Analytics | Microsoft Docs"
description: "Jak tootroubleshoot vaše úloha Stream Analytics"
keywords: "řešení potíží s Průvodce"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 4add48054e06a7d8eb617a08595c1be4555c59d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Průvodce řešením potíží pro Azure Stream Analytics

Řešení potíží Azure Stream Analytics se může zobrazit toobe komplexní úsilí na první pohled. Po dokončení práce s mnoha uživateli, jsme vytvořili hello Tento průvodce toohelp zjednodušit proces a odeberte hello již nebudete muset odhadovat o vstupy, výstupy, dotazů a funkce.

## <a name="troubleshoot-your-stream-analytics-job"></a>Řešení potíží s vaší úloze Stream Analytics

Chcete-li dosáhnout nejlepších výsledků při řešení potíží úlohu služby Stream Analytics použijte hello následující pokyny:

1.  Test připojení:
    - Ověřte připojení tooinputs a výstupů pomocí hello **Test připojení** tlačítko pro každý vstup a výstup.

2.  Zkontrolujte vstupní data:
    - je tooverify, že vstupní dat odesílaných do centra událostí, použijte [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) tooconnect tooAzure centra událostí (Pokud se používá Centrum událostí vstupu).  
    - Použití hello [ **ukázková Data** ](stream-analytics-sample-data-input.md) tlačítko pro každý vstupní a stáhněte si hello vstupní ukázková data.
    - Zkontrolujte hello ukázková data toounderstand hello tvar dat hello: hello schéma a hello [datové typy](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  Otestování dotazu:
    - Na hello **dotazu** pomocí hello **testování** tlačítko tootest hello dotazu a použijte hello stáhli ukázková data příliš[testovací hello dotazu](stream-analytics-test-query.md). Zkontrolujte případné chyby a pokus toocorrect je.
    - Pokud používáte [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), ověřte, že události hello časová razítka větší než hello [čas spuštění úlohy](stream-analytics-out-of-order-and-late-events.md).

4.  Ladění dotazu:
    - Znovu sestavte dotaz hello progresivně z jednoduchého příkazu select toomore komplexní agregace pomocí kroků. použít toobuild až hello dotazu logiku krok za krokem, [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) klauzule.
    - Použití [SELECT INTO](stream-analytics-select-into.md) toodebug kroky dotazu.

5.  Eliminovat běžné nástrahy, jako například:
    - A [ **kde** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) klauzule v dotazu hello odfiltrovat všechny události, brání generován žádný výstup.
    - Při použití funkce okno, počkejte na toosee doba trvání hello celé okno výstup z dotazu hello.
    - čas spuštění úlohy hello předchází Hello časové razítko pro události, a proto jsou vyřazována události.

6.  Použijte řazení událostí:
    - Pokud všechny hello předchozí kroky fungovala správně, přejděte toohello **nastavení** a vyberte [ **řazení událostí**](stream-analytics-out-of-order-and-late-events.md). Ověřte, že je tato zásada povolena pro co smysl ve vašem scénáři. zásady Hello *není* použijí, když používáte hello **Test** tlačítko tootest hello dotazu. Tento výsledek je jeden rozdíl mezi testování v prohlížeči a spuštění úlohy hello v produkčním prostředí.

7.  Ladění pomocí metriky:
    - Pokud žádný výstup získáte po hello očekávané trvání (založené na dotazu hello), zkuste následující hello:
        - Podívejte se na [ **monitorování metriky** ](stream-analytics-monitoring.md) na hello **monitorování** kartě. Protože jsou agregovány hello hodnot, jsou hello metriky zpožděn několik minut.
            - Pokud vstupní události > 0, hello úlohy je možné tooread vstupní data. Pokud události vstup není pak > 0:
                - toosee zda zdroj dat hello obsahuje platná data, zkontrolujte ho pomocí [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Tato kontrola platí, pokud úloha hello používá jako vstup centra událostí.
                - Toosee zkontrolujte, zda text hello formát serializace dat a data kódování jsou podle očekávání.
                - Pokud úloha hello používá centra událostí, zkontrolujte toosee, zda text hello zprávy hello je *Null*.
            - Pokud chyb při převodu dat > 0 a stoupající, může být true hello následující:
                - Úloha Hello nemusí být možné toode-serializovat hello události.
                - Hello schématu události nemusí odpovídat definované hello nebo očekávané schéma hello událostí v dotazu hello.
                - datové typy Hello některých polí hello v události hello nemusí odpovídat očekávání.
            - Pokud chyby za běhu > 0 znamená tuto úlohu hello může přijímat hello data ale generuje chyby při zpracování dotazu hello.
                - toofind hello chyby, přejděte toohello [protokoly auditu](../azure-resource-manager/resource-group-audit.md) a filtrujte *se nezdařilo* stavu.
            - Pokud InputEvents > 0 a OutputEvents = 0, znamená to, zda je splněna jedna z následujících hello:
                - Zpracování dotazu mělo za výsledek nulu výstupních událostí.
                - Události nebo jeho pole může být poškozený, což vede k nulové výstup po zpracování dotazu.
                - Úloha Hello byla toohello dat nelze toopush [výstupní jímku](stream-analytics-select-into.md) důvodů připojení nebo ověřování.
        - Ve všech hello dříve uvedených případech chyb, operace zprávy protokolu popisují další podrobnosti (včetně, co se děje), s výjimkou případů, kdy hello dotazu logiku odfiltrovat všechny události. Pokud hello zpracování více událostí generuje chyby, Stream Analytics protokoly hello první tři chybové zprávy z hello stejný typ v rámci tooOperations 10 minut protokolů. Potlačí další identické chyby se zprávou, který čte "Chyby se děje příliš rychle že tyto potlačeno."

8. Ladění pomocí auditu a diagnostické protokoly:
    - Použití [protokoly auditu](../azure-resource-manager/resource-group-audit.md)a filtr tooidentify a ladění chyb.
    - Použití [úlohy diagnostické protokoly](stream-analytics-job-diagnostic-logs.md) tooidentify a ladění chyb.

9. Zkontrolujte výstupy hello:
    - Pokud je stav úlohy hello *systémem*, v závislosti na hello dobu, po kterou je stanoveno v dotazu hello, zobrazí se výstup hello ve zdroji dat podřízený hello.
    - Pokud nevidíte výstupů, které budou konkrétní výstupní typ tooa, přesměruje je tooan výstupní typ, který je méně složitých, jako je například objektu Blob Azure. Pomocí Průzkumníka úložiště zkontrolujte, zda se zobrazí výstup hello toosee. Kromě toho ověřte, zda omezení omezení na výstup hello brání data přijímání.

10. Tok dat analysis pomocí úlohy diagram metriky:
    - tooanalyze data toku a identifikovat problémy, použijte hello [diagram úlohy s metriky](stream-analytics-job-diagram-with-metrics.md).

11. Otevřete případu podpory:
    - Navíc pokud selžou všechny ostatní postupy, otevřete případu podpory společnosti Microsoft pomocí hello ID předplatného, která obsahuje úlohu.

## <a name="get-help"></a>Podpora

Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Další kroky

* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
