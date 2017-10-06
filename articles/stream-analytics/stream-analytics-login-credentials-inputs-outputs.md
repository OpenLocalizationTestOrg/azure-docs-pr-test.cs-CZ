---
title: "Stream Analytics: Otáčení protokolu pověření pro vstupy a výstupy | Microsoft Docs"
description: "Zjistěte, jak tooupdate hello přihlašovací údaje pro Stream Analytics vstupy a výstupy."
keywords: "přihlašovací údaje"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42ae83e1-cd33-49bb-a455-a39a7c151ea4
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: ac2374c539012b66ab390656c5750024e02f6bdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Otočit přihlašovací údaje pro vstupy a výstupy úlohy Stream Analytics
## <a name="abstract"></a>Abstraktní
Azure Stream Analytics dnes neumožňuje nahrazení hello přihlašovacích údajů na vstupu a výstupu, když je spuštěná úloha hello.

Zatímco Azure Stream Analytics podporuje obnovení úlohy z posledního výstupu, jsme chtěli tooshare hello celý proces pro minimalizaci hello prodleva mezi hello zastavení a spuštění úlohy hello a otáčení hello přihlašovací údaje.

## <a name="part-1---prepare-hello-new-set-of-credentials"></a>Část 1 – Příprava hello novou sadu přihlašovacích údajů:
Tato část se vztahuje toohello následující vstupy/výstupy:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

Pro ostatní vstupy/výstupy pokračujte část 2.

### <a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště/tabulky
1. Přejdete na portál pro správu Azure hello toohello úložiště rozšíření:  
   ![graphic1][graphic1]
2. Vyhledejte hello úložiště, které používá vaše úloha a přejděte do ní:  
   ![graphic2][graphic2]
3. Klikněte na příkaz Spravovat přístupové klíče hello:  
   ![graphic3][graphic3]
4. Mezi hello primární přístupový klíč a hello sekundární přístupový klíč **vyberte hello jeden není používá vaše úloha**.
5. Stiskněte tlačítko znovu vygenerovat:  
   ![graphic4][graphic4]
6. Zkopírujte klíč hello nově vygenerované:  
   ![graphic5][graphic5]
7. Pokračujte tooPart 2.

### <a name="event-hubs"></a>Služba Event hubs
1. Rozšíření Service Bus toohello přejdete na portál pro správu Azure hello:  
   ![graphic6][graphic6]
2. Vyhledejte hello Namespace Service Bus používá vaše úloha a přejděte do ní:  
   ![graphic7][graphic7]
3. Pokud vaše úlohy používá zásady sdíleného přístupu v hello Namespace Service Bus, přeskočit toostep 6  
4. Přejdete toohello Event Hubs karty:  
   ![graphic8][graphic8]
5. Vyhledejte hello centra událostí, které používá vaše úloha a přejděte do ní:  
   ![graphic9][graphic9]
6. Přejdete toohello karta konfigurace:  
   ![graphic10][graphic10]
7. V hello rozevíracího seznamu název zásady najděte hello sdílené zásady přístupu používá vaše úloha:  
   ![graphic11][graphic11]
8. Mezi hello primární klíč a sekundární klíč hello **vyberte hello jeden není používá vaše úloha**.  
9. Stiskněte tlačítko znovu vygenerovat:  
   ![graphic12][graphic12]
10. Zkopírujte klíč hello nově vygenerované:  
   ![graphic13][graphic13]
11. Pokračujte tooPart 2.  

### <a name="sql-database"></a>SQL Database
> [!NOTE]
> Poznámka: budete potřebovat tooconnect toohello služby databáze SQL. Přidáme tooshow jak toodo této konfigurace pomocí prostředí správy hello na hello portál pro správu Azure, ale můžete rozhodnout toouse některé klientské nástroje, jako je SQL Server Management Studio také.
>
> 

1. Rozšíření toohello databází SQL, přejdete na portál pro správu Azure hello:  
   ![graphic14][graphic14]
2. Vyhledejte hello databázi SQL používanou nástrojem úlohu a **klikněte na hello server** odkaz na hello stejný řádek:  
   ![graphic15][graphic15]
3. Klikněte na příkaz Spravovat hello:  
   ![graphic16][graphic16]
4. Hlavní databáze typu:  
   ![graphic17][graphic17]
5. Zadejte uživatelské jméno, heslo a klikněte na protokolu:  
   ![graphic18][graphic18]
6. Klikněte na nový dotaz:  
   ![graphic19][graphic19]
7. Typ v hello následující dotaz nahrazení < login_name > svým uživatelským jménem a nahrazení <enterStrongPasswordHere> pomocí nového hesla:  
   `CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8. Klikněte na tlačítko spustit:  
   ![graphic20][graphic20]
9. Vraťte toostep 2 a Tentokrát klikněte na databázi hello:  
   ![graphic21][graphic21]
10. Klikněte na příkaz Spravovat hello:  
   ![graphic22][graphic22]
11. Zadejte uživatelské jméno, heslo a klikněte na možnost přihlášení:  
   ![graphic23][graphic23]
12. Klikněte na nový dotaz:  
   ![graphic24][graphic24]
13. Zadejte následující dotaz nahrazení < uživatelské_jméno > s názvem, podle kterého chcete tooidentify hello toto přihlášení v kontextu hello této databáze (můžete zadat stejnou hodnotu, která jste zadali pro < login_name >, například hello) a nahraďte < login_name > nové uživatelské jméno:  
   `CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klikněte na tlačítko spustit:  
   ![graphic25][graphic25]
15. Nový uživatel by měl nyní poskytovat s hello stejné role a oprávnění měl původního uživatele.
16. Pokračujte tooPart 2.

## <a name="part-2-stopping-hello-stream-analytics-job"></a>Část 2: Zastavení hello úlohy Stream Analytics
1. Přejdete na portál pro správu Azure hello toohello Stream Analytics rozšíření:  
   ![graphic26][graphic26]
2. Najděte úlohu a přejděte do ní:  
   ![graphic27][graphic27]
3. Přejděte toohello vstupy kartě nebo hello výstupů založené na tom, jestli jsou otáčení hello pověření na vstup nebo výstup.  
   ![graphic28][graphic28]
4. Klikněte na příkaz Stop hello a potvrďte, že hello úloha byla zastavena:  
   ![graphic29][graphic29] počkejte toostop úlohy hello.
5. Vyhledejte hello vstupu a výstupu chcete toorotate pověření na a přejděte do ní:  
   ![graphic30][graphic30]
6. Pokračujte tooPart 3.

## <a name="part-3-editing-hello-credentials-on-hello-stream-analytics-job"></a>Část 3: Úprava hello přihlašovací údaje u hello úlohy Stream Analytics
### <a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště/tabulky
1. Najít pole hello klíč účtu úložiště a vložte do něj nově vygenerovaný klíč:  
   ![graphic31][graphic31]
2. Klikněte na příkaz Uložit hello a potvrďte uložení změn:  
   ![graphic32][graphic32]
3. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že je úspěšně byla úspěšná.
4. Pokračujte tooPart 4.

### <a name="event-hubs"></a>Služba Event hubs
1. Najít pole hello klíč události rozbočovače zásady a vložit vaše nově vygenerovaný klíč do ní:  
   ![graphic33][graphic33]
2. Klikněte na příkaz Uložit hello a potvrďte uložení změn:  
   ![graphic34][graphic34]
3. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.
4. Pokračujte tooPart 4.

### <a name="power-bi"></a>Power BI
1. Klikněte na tlačítko Obnovit autorizace hello:  

   ![graphic35][graphic35]
2. Zobrazí se následující potvrzení hello:  

   ![graphic36][graphic36]
3. Klikněte na příkaz Uložit hello a potvrďte uložení změn:  
   ![graphic37][graphic37]
4. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že úspěšně úspěšně prošel.
5. Pokračujte tooPart 4.

### <a name="sql-database"></a>SQL Database
1. Najde hello pole uživatelské jméno a heslo a vkládání do nich vaše nově vytvořenou sadu přihlašovacích údajů:  
   ![graphic38][graphic38]
2. Klikněte na příkaz Uložit hello a potvrďte uložení změn:  
   ![graphic39][graphic39]
3. Test připojení se automaticky spustí, když uložíte změny, ujistěte se, že má úspěšně úspěšně.  
4. Pokračujte tooPart 4.

## <a name="part-4-starting-your-job-from-last-stopped-time"></a>Část 4: Od poslední zastaven úlohu
1. Opustit hello vstupu a výstupu:  
   ![graphic40][graphic40]
2. Klikněte na příkaz spuštění hello:  
   ![graphic41][graphic41]
3. Vyberte hello naposledy zastaveno a klikněte na tlačítko OK:  
   ![graphic42][graphic42]
4. Pokračujte tooPart 5.  

## <a name="part-5-removing-hello-old-set-of-credentials"></a>Část 5: Odebrání hello původní sadu přihlašovacích údajů
Tato část se vztahuje toohello následující vstupy/výstupy:

* Blob Storage
* Event Hubs
* SQL Database
* Table Storage

### <a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště/tabulky
Opakujte část 1 pro hello přístupový klíč, který byl dříve používán vaše úloha toorenew hello teď nepoužívá přístupový klíč.

### <a name="event-hubs"></a>Služba Event hubs
Opakujte část 1 pro hello klíč, který byl dříve používán vaše úloha toorenew hello teď nepoužívá klíč.

### <a name="sql-database"></a>SQL Database
1. Přejděte zpět okno dotazu toohello z část 1 kroku 7 a zadejte následující dotaz, nahraďte < previous_login_name > hello uživatelské jméno, který byl dříve používán vaše úloha hello:  
   `DROP LOGIN <previous_login_name>`  
2. Klikněte na tlačítko spustit:  
   ![graphic43][graphic43]  

Měli byste obdržet hello následující potvrzení: 

    Command(s) completed successfully.

## <a name="get-help"></a>Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png

