---
title: "aaaDebug v protokolech operace a služby Stream Analytics | Microsoft Docs"
description: "Jak toouse Stream Analytics protokoly operací"
keywords: "protokoly služby"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: d3dd27706ccc879a724e1894b33d47021d972f31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Ladění pomocí služby a operace protokoly úlohy Stream Analytics
Všechny služby Azure napájení provozní protokolování zpráv toousers toorecord podrobnosti související s toomanagement operace. V Azure Stream Analytics tyto informace slouží pro účely, například zobrazení stavu úlohy, průběh úlohy a selhání zprávy tootrack hello průběh úlohy v čase od toooutput tooprocessing spuštění ladění.

## <a name="find-operation-logs-in-hello-azure-management-portal"></a>Najít operaci protokoly na portálu pro správu Azure hello
Protokoly operací je přístupná dvěma způsoby:  

* Řídicí panel úlohy Stream Analytics hello  
* Správa služeb v portálu Azure Classic hello  

## <a name="dashboard-of-hello-stream-analytics-job"></a>Řídicí panel úlohy Stream Analytics hello
Odkaz toohello, odpovídající protokoly úlohy Stream Analytics se zobrazí na kartě úlohy hello řídicího panelu. Pokud kliknete na tento odkaz, nastaví hello filtry tak, že se zobrazí nejnovější protokoly pro tuto konkrétní úlohu.

  ![Vyberte protokoly služby správy](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Služby správy
toomanually přejděte protokoly operací toohello Stream Analytics a dalších služeb v portálu Azure Classic hello:

1. Klikněte na **Management Services** v hello [portálu Azure Classic](https://manage.windowsazure.com).
2. Vyberte **Stream Analytics** pro **typ** a název hello hello úlohy pro **název služby**.  
   
   ![Vyberte služby Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-hello-azure-portal"></a>Najít protokoly auditu v hello portálu Azure
Klikněte na tlačítko toofind operační protokoly pro úlohu služby Stream Analytics v hello portál Azure, **Procházet** a pak vyberte **protokoly auditu**.

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Otevře se okno zobrazení událostí z hello posledních 7 dnů pro všechny prostředky ve vašem předplatném.  Můžete filtrovat události toosee zadejte typ nebo časového rámce kliknutím hello **filtru** příkaz.

  ![Portálu Azure vyberte Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Získat podrobnosti protokolu
Můžete filtrovat podle časové rozmezí a stav tooview hello protokoly pro úlohu.

V portálu pro správu Azure hello, klikněte na hello **podrobnosti** tlačítko hello dolní části okna tooview hello další podrobnosti o zvolenou událost. 

  ![Vyberte podrobnosti](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Portál Azure, klikněte na položku toosee protokolu v hello hello podrobné události v ho.

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Odtud můžete otevřít hello **podrobností** tak, že kliknete na hello událostí.

  ![Portálu Azure vyberte podrobnosti](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Ladění neúspěšnou úlohu
Na portálu pro správu Azure hello klikněte na ikonu hledání hello a zadejte "se nezdařilo". To dává výsledku všechny protokoly s chybami. 

  ![Ladění neúspěšnou úlohu](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

V hello portálu Azure, můžete filtrovat podle úrovně zpráva tooview **kritický** události.

  ![Portál Azure ladění](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Můžete vybrat některého hello selhání a klikněte na hello **podrobnosti** pro další informace o chybě hello.  Některé chybové zprávy také poskytují informace o tom, jak toomitigate hello problém. 

  ![Podrobnosti o operaci](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

V případě, že potřebujete toocontact [podporu](https://azure.microsoft.com/support/options/) nebo zadejte informace o toohello team prostřednictvím hello [fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), je potřeba počítat s hello provozní údaje, konkrétně hello **ID korelace**. 

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

