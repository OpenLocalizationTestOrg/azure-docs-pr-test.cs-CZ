---
title: "výstrahy v reálném čase CDN aaaAzure | Microsoft Docs"
description: "V reálném čase výstrah v Microsoft Azure CDN. Výstrahy v reálném čase poskytují oznámení o výkonu hello hello koncových bodů ve vašem profilu CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 269c90437088bbc41bf899a8c02749e8e6f3006c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>V reálném čase výstrah v Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Přehled
Tento dokument popisuje, v reálném čase výstrah v Microsoft Azure CDN. Tato funkce poskytuje v reálném čase oznámení o výkonu hello hello koncových bodů ve vašem profilu CDN.  Můžete nastavit e-mailu nebo HTTP upozornění na základě:

* Šířka pásma
* Stavové kódy
* Stavy mezipaměti
* Připojení

## <a name="creating-a-real-time-alert"></a>Vytváření v reálném čase výstrahy
1. V hello [portálu Azure](https://portal.azure.com), procházet tooyour profil CDN.
   
    ![Okno profil CDN](./media/cdn-real-time-alerts/cdn-profile-blade.png)
2. Z hello okno profil CDN, klikněte na tlačítko hello **spravovat** tlačítko.
   
    ![Tlačítko Spravovat okno profil CDN](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    Otevře portál pro správu Hello CDN.
3. Hover přes hello **Analytics** kartu a potom hover přes hello **statistiky v reálném čase** plovoucím panelem.  Klikněte na **v reálném čase výstrahy**.
   
    ![Portál pro správu CDN](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    Zobrazí se Hello seznam stávajících výstrahy konfigurací (pokud existuje).
4. Klikněte na tlačítko hello **přidat výstrahy** tlačítko.
   
    ![Výstrahy tlačítko Přidat](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Formulář pro vytvoření nového upozornění se zobrazí.
   
    ![Formulář nové výstrahy](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Pokud chcete tuto výstrahu toobe active když kliknete na tlačítko **Uložit**, zkontrolujte hello **upozornění povolené** zaškrtávací políčko.
6. Zadejte popisný název pro upozornění v hello **název** pole.
7. V hello **typ média** rozevíracího seznamu vyberte **velkého objektu HTTP**.
   
    ![Typ média HTTP velký objekt vybrán](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > Je nutné vybrat **velkého objektu HTTP** jako hello **typ média**.  Hello jiné možnosti nejsou používány nástrojem **Azure CDN společnosti Verizon**.  Selhání tooselect **velkého objektu HTTP** způsobí, že upozornění aktivována toonever.
   > 
   > 
8. Vytvoření **výraz** toomonitor výběrem **metrika**, **operátor**, a **aktivovat hodnotu**.
   
   * Pro **metrika**, vyberte typ podmínky, které chcete monitorovaných hello.  **MB/s šířky pásma** je hello množství využití šířky pásma v MB za sekundu.  **Celkový počet připojení** je hello počet souběžných připojení tooour HTTP servery edge.  Definice hello různé stavy, které jsou mezipaměti a stavových kódů, najdete v části [Azure CDN mezipaměti stavové kódy](https://msdn.microsoft.com/library/mt759237.aspx) a [stavové kódy HTTP CDN Azure](https://msdn.microsoft.com/library/mt759238.aspx)
   * **Operátor** je hello matematický operátor, který stanoví hello vztah mezi metrika hello a hodnota hello aktivační události.
   * **Aktivovat hodnota** je hello prahovou hodnotu, která musí být splněny, než bude odesláno oznámení.
     
     V následujícím příkladu se text hello hello výraz, který vytvořil jsem označuje, že nastavit jako toobe upozorněni, když je větší než 25 hello počet stavový kód 404.
     
     ![V reálném čase výstrahy ukázka výrazu](./media/cdn-real-time-alerts/cdn-expression.png)
9. Pro **Interval**, zadejte, jak často chcete hello výrazu vyhodnoceného.
10. V hello **upozornit na** rozevíracího seznamu vyberte, pokud chcete toobe upozorněni, když hello je výraz pravdivý.
    
    * **Podmínka spuštění** označuje, že bude být oznámení odesláno, pokud hello zadána, je nejprve zjistil podmínku.
    * **Podmínka End** označuje, že oznámení odešle při hello zadaná podmínka je již nebudou zjištěna. Toto oznámení se spustí pouze po zjištěna naše síť systému pro monitorování této hello zadaná podmínka došlo k chybě.
    * **Průběžné** označuje bude odesláno upozornění pokaždé, když hello systém monitorování sítě zjistí hello zadaná podmínka. Mějte na paměti tuto síť hello monitorování systému bude pouze kontrola jednou intervalu pro hello zadaná podmínka.
    * **Podmínka počáteční a koncové** označuje, že hello prvním hello zadanou podmínku je a znovu při hello podmínka je už bude odesláno upozornění.
11. Pokud chcete tooreceive oznámení e-mailem, zkontrolujte hello **oznámení e-mailem** zaškrtávací políčko.  
    
    ![Upozornit e-mailu formuláři](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    V hello **k** pole, zadejte je místo, kam chcete oznámení odeslaných hello e-mailovou adresu. Pro **subjektu** a **textu**, můžete ponechat výchozí hello nebo může přizpůsobit uvítací zprávu pomocí hello **k dispozici klíčová slova** seznamu toodynamically vložení dat výstrah při je odeslána zpráva Hello.
    
    > [!NOTE]
    > Hello e-mailové oznámení můžete otestovat klepnutím hello **testovací oznámení** tlačítko, ale pouze po uložení konfigurace výstrah hello.
    > 
    > 
12. Pokud chcete oznámení toobe odeslány tooa webového serveru, zkontrolujte hello **oznámení pomocí HTTP Post** zaškrtávací políčko.
    
    ![Upozornit pomocí HTTP Post formuláře](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    V hello **Url** pole, zadejte adresu URL hello je místo, kam chcete zprávu hello HTTP odeslány. V hello **hlavičky** textovému poli, zadejte hello HTTP hlavičky toobe odeslaných v požadavku hello.  Pro **textu** může přizpůsobit uvítací zprávu pomocí hello **k dispozici klíčová slova** seznamu toodynamically vložení dat výstrah při odeslání zprávy hello.  **Hlavičky** a **textu** výchozí tooan XML datové části podobné toohello následujícím příkladu.
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > Hello HTTP Post oznámení můžete otestovat klepnutím hello **testovací oznámení** tlačítko, ale pouze po uložení konfigurace výstrah hello.
    > 
    > 
13. Klikněte na tlačítko hello **Uložit** tlačítko toosave konfiguraci výstrah.  Pokud je zaškrtnuto **upozornění povolené** v kroku 5, upozornění je teď aktivní.

## <a name="next-steps"></a>Další kroky
* Analýza [statistiky v reálném čase v Azure CDN](cdn-real-time-stats.md)
* Dig hlubší s [Rozšířené sestavy HTTP](cdn-advanced-http-reports.md)
* Analýza [vzorce používání](cdn-analyze-usage-patterns.md)

