---
title: "aaaMonitoring prostředky v Operations Management Suite zabezpečení a Audit řešení | Microsoft Docs"
description: "Tento dokument pomáhá toouse OMS zabezpečení a Audit možnosti toomonitor vašich prostředků a identifikovat problémy se zabezpečením."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 932b946ae1ffa3b979c02f419702d42d46abf7ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Sledování prostředků v Operations Management Suite zabezpečení a Audit řešení
Tento dokument vám pomůže používat OMS zabezpečení a Audit možnosti toomonitor vaše prostředky a identifikovat problémy se zabezpečením.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft Operations Management Suite (OMS) je řešení pro správu IT, která pomáhá spravovat a chránit místní a cloudové infrastruktury založené na službě cloud společnosti Microsoft. Další informace o OMS, přečtěte si článek hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Sledování prostředků
Vždy, když je možné, budete chtít incidenty zabezpečení tooprevent nedocházelo v první místní hello. Je však možné tooprevent všechny incidenty zabezpečení. Když dojít bezpečnostního incidentu, budete potřebovat tooensure, že je minimalizován jeho dopad.  Existují tři kritické doporučení, které se dají použít toominimize hello číslo a hello dopad incidentů, zabezpečení:

* Pravidelně vyhodnoťte ohrožení zabezpečení ve vašem prostředí.
* Pravidelně zkontrolujte všechny počítačových systémů a tooensure síťové zařízení, která mají všechny hello nainstalovány nejnovější opravy.
* Pravidelně zkontrolujte všechny protokoly a mechanismy protokolování, včetně protokolů událostí operačního systému, konkrétní protokoly aplikací a protokoly systému zjišťování neoprávněných vniknutí.

OMS zabezpečení a Audit řešení umožňuje IT tooactively monitorovat všechny prostředky, které může pomoci minimalizovat dopad hello bezpečnostní incidenty v oblasti. OMS zabezpečení a Audit má zabezpečení domény, které lze použít ke sledování prostředků. Hello zabezpečení domény nabízí rychlý přístup tooa možnosti, pro monitorování hello zabezpečení následujících domén se budeme v další podrobnosti:

* posouzením malwaru
* Posouzení aktualizací
* Identita a přístup

> [!NOTE]
> Přehled všechny tyto možnosti najdete v tématu [Začínáme s Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Monitorování ochrany systému
V obrany ve hloubka přístup, je důležité pro hello každou úroveň ochrany celkový stav zabezpečení asset. Počítače s zjištěné hrozby a počítače s nedostatečnou ochranou se zobrazují v hello posouzením malwaru dlaždici v části zabezpečení domény. Pomocí informací hello na hello posouzením malwaru, můžete identifikovat servery toohello ochrany tooapply plán, které je třeba ji. tooaccess tuto možnost hello postupujte podle kroků dole:

1. V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **zabezpečení a Audit** dlaždici.
   
    ![Zabezpečení a Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. V hello **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **antimalwarových Assessment** pod **zabezpečení domény**. Hello **antimalwarových Assessment** řídicího panelu se zobrazí, jak je uvedeno níže:

![posouzením malwaru](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Můžete použít hello **posouzením malwaru** hello tooidentify řídicí panel následující problémy se zabezpečením:

* **Aktivní hrozeb**: počítače, které byly ohrožené a mít aktivní hrozeb v systému hello.
* **Opravené hrozeb**: počítače, které byly ohrožení zabezpečení, ale hello hrozeb provedla nápravu.
* **Zastaralý podpis**: počítače, které mají povoleno, ale podpis hello ochranu proti malwaru je zastaralý.
* **Žádná ochrana v reálném čase**: počítače, které nemají nainstalovaný Antimalwarový program.

### <a name="monitoring-updates"></a>monitorování aktualizací
Nejlepším postupem zabezpečení je použití hello nejnovější aktualizace zabezpečení a by měla být zahrnuta do strategie správy aktualizace. Služba Microsoft Monitoring Agent (HealthService.exe) čte informace o aktualizaci z monitorovaných počítačů a pak odešle tato aktualizované informace toohello OMS služba v cloudu hello ke zpracování. Hello službu Microsoft Monitoring Agent je nakonfigurovaný jako automatické služby a měla by být v cílovém počítači hello vždy spuštěna.

![monitorování aktualizací](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logika je použité toohello aktualizace dat a hello Cloudová služba zaznamenává hello data. Pokud se nenajdou chybějící aktualizace, zobrazí se na hello **aktualizace** řídicího panelu. Můžete použít hello **aktualizace** toowork řídicí panel s chybějící aktualizací a vytvořte plán tooapply je toohello servery, které je potřebují. Postupujte podle kroků hello tooaccess hello **aktualizace** řídicí panel:

1. V hello **Microsoft Operations Management Suite** klikněte na hlavním řídicím **zabezpečení a Audit** dlaždici.
2. V hello **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **vyhodnocení aktualizací** pod **zabezpečení domény**. Aktualizace Hello řídicího panelu se zobrazí, jak je uvedeno níže:

![Vyhodnocení aktualizací](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

V tomto řídicím panelu můžete provést aktualizaci assessment toounderstand hello aktuální stav počítače a nejdůležitější hrozeb hello adresu. Pomocí hello **důležité aktualizace nebo aktualizace zabezpečení** dlaždice, správci IT bude moct tooaccess podrobné informace o hello aktualizace, které chybí, jak je uvedeno níže:

![Výsledky vyhledávání](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Tato sestava zahrnují důležité informace, které lze použít tooidentify hello typ tohoto systému je snadno napadnutelný, ohrožení, které obsahuje články znalostní báze Microsoft KB hello přidružené hello aktualizace zabezpečení a hello Bulletin MS, který obsahuje další podrobnosti o hello ohrožení zabezpečení.

### <a name="monitoring-identity-and-access"></a>Sledování přístupu a identit
S uživatelů, kteří pracují odkudkoli a pomocí různých zařízení a přístupu k obrovské množství cloudové a místní aplikace je nutné ochrany svých přihlašovacích údajů. Útoky krádeže přihlašovacích údajů jsou ty, ve kterých útočník původně získá přístup tooa regulární uživatele tooaccess pověření systému v rámci sítě hello. Kolikrát tento počáteční útok je pouze způsob tooget přístup toohello síť, hello konečným cílem je toodiscover oprávnění účty. 

Útočníci zůstanou v síti hello pomocí volně dostupných nástrojů tooextract pověření z relací hello dalších účtů přihlášen. V závislosti na konfiguraci systému hello můžete extrahovat tyto přihlašovací údaje v podobě hello hodnoty hash, lístků nebo hesel, i ve formátu prostého textu.  

> [!NOTE]
> počítače, které jsou přímo zveřejněné toohello Internetu se zobrazí mnoho se nezdařilo pokusí tento toologin zkuste pomocí všechny druh dobře známé uživatelských jmen (například správce). Ve většině případů je OK pokud hello dobře známé uživatelských jmen se nepoužívají, a pokud je dostatečně silné heslo hello.
> 
> 

Ho je možné tooidentify tyto případné útočníky před jejich ohrožení účet oprávnění. Můžete využít **OMS zabezpečení a Audit řešení** toomonitor identit a přístupu. Postupujte podle kroků hello tooaccess hello **identit a přístupu** řídicí panel:

1. V hello **Microsoft Operations Management Suite** zabezpečení a Audit, klikněte na hlavním řídicím panelu dlaždici.
2. V hello **zabezpečení a Audit** řídicí panel, klikněte na tlačítko **identit a přístupu** pod **zabezpečení domény**. Hello **identit a přístupu** řídicího panelu se zobrazí, jak je uvedeno níže:

![Identita a přístup](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Jako součást strategie regulární monitorování je nutné zahrnout monitorování identity. Správce IT by měl vypadat, pokud je konkrétní platné uživatelské jméno, který má počtu pokusů o zadání. To může znamenat buď útočník, který získali hello skutečné uživatelské jméno a zkuste vynutit toobrute nebo automatické nástroj, který používá pevně zakódovaný heslo, jehož platnost vypršela.

Tento řídicí panel povolit IT tooquickly identifikovat potenciální hrozby související tooidentity a přístup toocompany na prostředky. Je konkrétní důležité tooalso identifikovat potenciální trendy, například v dlaždici hello přihlášení v čase, se zobrazí po dobu kolikrát byl proveden pokus o přihlášení se nezdařilo. V takovém případě hello počítače **souborovém serveru** přijatých 35 pokusů o přihlášení. Další informace o tomto počítači, můžete prozkoumat kliknutím na. 

![Další informace](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Hello sestava vygenerována pro tento počítač přináší cenné informace o tomto vzoru. K tomu, že hello **účet** sloupec poskytuje hello uživatelský účet, který byl použité tootry tooaccess hello systému hello **TIMEGENERATED** sloupec poskytuje hello časový interval, ve které hello byl proveden pokus o a Hello **LOGONTYPENAME** sloupec poskytuje hello umístění, kde bylo provedeno tento pokus. Pokud tyto pokusy byly prováděny místně hello systému programem, hello **proces** sloupec by zobrazuje název procesu hello. Ve scénářích, kde se pokus o přihlášení hello pochází z programu máte k dispozici název procesu hello a teď můžete provádět další šetření v cílovém systému hello.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili jak toomonitor toouse OMS zabezpečení a Audit řešení vašich prostředků. Další informace o zabezpečení OMS toolearn najdete hello následující články:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Začínáme se službou Operations Management Suite zabezpečení a Audit řešení](oms-security-getting-started.md)
* [Monitorování a výstrahy tooSecurity odpovídá v Operations Management Suite zabezpečení a Audit řešení](oms-security-responding-alerts.md)

