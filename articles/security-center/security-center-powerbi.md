---
title: "přehledy aaaGet z Azure Security Center dat s Power BI | Microsoft Docs"
description: "Hello balíček obsahu Azure Security Center Power BI umožňuje snadno toofind výstrah zabezpečení, doporučení, prostředků vystavených útoku a trendů, založené na datovou sadu, která byla vytvořena pro účely generování sestav."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 0ded6bc7-52e8-43b4-8940-0bee137526e3
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: yurid
ms.openlocfilehash: af68aef626034fe03d793c36b515a3f26619e5f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Přehledné znázornění dat z Azure Security Center v řešení Power BI
Hello [řídicí panel Power BI](http://aka.ms/azure-security-center-power-bi) pro Azure Security Center umožňuje toovisualize, analyzovat a filtrovat doporučení a výstrahy zabezpečení odkudkoli, včetně vašeho mobilního zařízení. Použijte hello Power BI řídicí panel tooreveal trendy a vzorce útoků – Zobrazit výstrahy zabezpečení podle prostředku nebo zdrojové IP adresy a nevyřešená rizika zabezpečení podle prostředku nebo stáří.

Taky můžete doporučení a výstrahy zabezpečení ze služby Security Center zajímavě zkombinovat s jinými daty, třeba pomocí dat z [protokolů auditu Azure](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) a [auditů Azure SQL Database](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Také obsahují řídicí panely Power BI a můžete také exportovat tento tooExcel data pro snadné vytváření sestav na hello stavu zabezpečení cloudových prostředků.

## <a name="using-azure-security-center-dashboard-tooaccess-power-bi"></a>Pomocí tooaccess řídicí panel Azure Security Center Power BI
Můžete také použít sestavy Power BI tooaccess řídicí panel Azure Security Center hello. Postupujte podle kroků tooperform hello tento úkol:

1. V hello **Azure Security Center** řídicí panel, klikněte na tlačítko **Power BI** tlačítko.

    ![Připojit tooAzure Security Center pomocí Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-1-newUI-2017.png)
2. Hello **Power BI** otevře okno na pravé straně hello, jak je znázorněno v hello následující obrazovka:

    ![Připojit tooAzure Security Center pomocí Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new11-2017.png)
3. Pokud vytváříte řídicí panel Power BI hello pro hello poprvé, můžete zvolit jednu z následujících hello možností v hello **prozkoumat v Power BI** okno:

   * **Přehledný řídicí panel zabezpečení**: tuto možnost zvolte, pokud chcete, aby toocreate řídicí panel, který obsahuje stav zabezpečení, vláken a detekce. Tuto možnost obvykle používají osoby s rolí DevOps, které zodpovídají za analýzu stavu ochrany a zjištěných výstrah v rámci předplatných.
   * **Řídicí panel správy zásad**: tuto možnost zvolte, pokud chcete, aby tooexplore správy a vynucování zásad.  Tuto zásadu obvykle používají osoby z centrálního oddělení IT, které se zaměřují na řízení. Použitím tento řídicí panel toogain viditelnost a přehledy o dodržování zásad zabezpečení ve své organizaci.
   * Pokud již máte řídicí panel Power BI, klikněte na tlačítko **přejděte tooyour aktuální řídicí panel Power BI**.
4. Pro účely tohoto příkladu klikněte na možnost **Řídicí panel přehledů o zabezpečení**. Pokud je to hello poprvé, vytváříte řídicí panel Power BI pro Security Center se zobrazí výzva balíček obsahu tooinstall hello. Klikněte na tlačítko **získat** tlačítka na hello **balíčky obsahu Power BI** okno, jak je znázorněno v hello následující obrazovka:

    ![Přehledný řídicí panel zabezpečení Azure Security Center](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)
5. Hello **připojit přehledy o zabezpečení tooAzure Security Center** zobrazit okno. Ujistěte se, zda text hello **ověřování** je metoda **oAuth2** jak je uvedeno níže a klikněte na tlačítko **přihlášení** tlačítko.

    ![Authentication](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)
6. Můžete být vyzváni tooauthenticate znovu pomocí svých přihlašovacích údajů Azure. Po ověření se vytvoří řídicí panel. Po vytvoření hello řídicího panelu zobrazí sestava s podobnou strukturou hello, jak je znázorněno v následující obrazovce hello:

    ![Řídicí panel Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)

> [!NOTE]
> Aktualizace hello sestavy je místní tootake naplánované na každý den. V případě, že dojde k selhání na tato aktualizace, přečtěte si [potenciální problémy s hello Azure Security Center Power BI aktualizací](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), další informace o tom, tootroubleshoot.
>
>

Zde můžete zobrazit hello počet výstrah zabezpečení a doporučení, a také hello počet virtuálních počítačů, databází Azure SQL a síťových prostředků sledovaných pomocí služby Azure Security Center.

Odkaz tooAzure Security Center přesměruje toohello portálu Azure. Hello grafy usnadňují snadno toovisualize informace o doporučeních zabezpečení a výstrahách, včetně:

* Resource Security State (Stav zabezpečení prostředků)
* Pending Recommendations (Nevyřízená doporučení)
* Doporučení pro virtuální počítače
* Alerts over Time (Výstrahy v průběhu času)
* Attacked Resources (Napadené prostředky)
* Attacked IPs (Napadené IP adresy)

Za každým grafem najdete další statistiky. Vyberte dlaždici toosee Další informace. Například hello **stav zabezpečení prostředků** dlaždice obsahuje další podrobnosti o nevyřízených doporučeních podle prostředků jak je znázorněno v hello následující obrazovka:

![Doporučení](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Pokud kliknete na kterýkoli řádek v tomto grafu, hello ostatní uživatelé toogray out a soustředit se jenom na hello, kterou jste vybrali. tooreturn toohello řídicí panel, klikněte na tlačítko **Azure Security Center** pod hello **řídicí panely** možnost v levém podokně hello části této stránky.

> [!NOTE]
> Pokud chcete toocustomize sestavy přidáním další pole nebo změnit stávající vizuální prvky, můžete upravit sestavu hello. Další informace najdete v článku [Interakce se sestavou v zobrazení pro úpravy v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/).
>
>

Hello **výstrahy v průběhu času napadení prostředky** a **IP adresy útočník** dlaždice bude mít podobný výstup hello, když kliknete na ně. K tomu dojde, protože hello sestava shromáždí informace týkající se všech těchto tří proměnných a nazve je **napadené prostředky** jak je znázorněno v hello následující obrazovka:

![Napadené prostředky](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

V tomto okamžiku můžete také uložit kopii sestavy, vytisknout nebo publikování na webu hello pomocí možnosti hello hello **souboru** nabídky.

![Nabídka Soubor](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Prozkoumání dat z Azure Security Center pomocí služeb Power BI
Připojit toohello [Power BI Content Pack Services](https://msit.powerbi.com/groups/me/getdata/services) v Power BI a provedení hello následující kroky:

1. V hello **balíček obsahu pro Power BI** okně uvidíte dvě možnosti, jak je uvedeno níže.

    ![Balíček obsahu pro Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

   > [!NOTE]
   > Pokud již proveden hello první část tohoto článku uvidíte jenom jedna možnost, která je Azure Security Center zásad správy.
   >
   >
2. Hello účel tohoto příkladu, klikněte na tlačítko **získat** v hello **Azure Security Center zásady správy** dlaždici.
3. V hello **připojit tooAzure Správa zásad zabezpečení Center** okna, ujistěte se, že tooselect **oAuth2** pod **metodu ověřování** rozevíracím seznamu, jak je uvedeno níže a klikněte na tlačítko **Přihlášení** tlačítko.

    ![Okno správy zásad](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)
4. Bude přesměrované tooan ověřovací stránku, kde byste měli zadat přihlašovací údaje hello, že používáte tooconnect tooAzure Security Center. Po dokončení procesu ověřování hello Power BI se spustí import data toobuild sestavy. Během této doby se můžete setkat s následující zprávou v pravém rohu prohlížeče hello hello:

    ![Připojit tooAzure Security Center pomocí Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

   > [!NOTE]
   > Pokud hello řídicí panel se vytváří pro hello poprvé může trvat déle než obvykle, hlavně v situacích, kdy máte víc předplatných.
   >
   >
5. Po dokončení procesu hello řídicího panelu Azure Security Center Power BI načte hello **Správa zásad** sestavy podobné toohello níže:

    ![Řídicí panel správy zásad](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak toouse Power BI v Azure Security Center. toolearn Další informace o službě Azure Security Center, najdete v části hello následující:

* [Průvodce Azure Security Center plánováním a provozem](security-center-planning-and-operations-guide.md) – Další informace jak tooplan přijetí Azure Security Center.
* [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Další informace jak tooconfigure nastavení zabezpečení v Azure Security Center
* [Správa a zda odpovídá toosecurity výstrahy v Azure Security Center](security-center-managing-and-responding-alerts.md) – Další informace jak toomanage a reakce toosecurity výstrahy
* [Nejčastější dotazy k Azure Security Center](security-center-faq.md) – přečtěte si nejčastější dotazy o použití služby hello
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů
