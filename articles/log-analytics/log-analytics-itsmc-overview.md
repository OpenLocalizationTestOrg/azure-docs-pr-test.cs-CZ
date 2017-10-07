---
title: aaaIT konektoru Management Service v OMS | Microsoft Docs
description: "Použít hello IT Service Management Connector toocentrally monitorování a správě hello ITSM pracovních položek v OMS a rychle vyřešit všechny problémy."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 33ed5d432591b836eb41ba982c66c96f22879444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Centrálně spravovat ITSM pracovních položek pomocí IT Service Management Connector (Preview)

![Symbol konektoru služby správy IT](./media/log-analytics-itsmc/itsmc-symbol.png)

Můžete použít hello IT služby správy konektoru (ITSMC) v nástroji Sledování toocentrally OMS analýzy protokolů a spravovat pracovní položky v rámci ITSM produktů nebo služeb.

Hello IT Service Management Connector se integruje s analýzy protokolů OMS existující IT služby správy (ITSM) produkty a služby.  řešení Hello má obousměrnou integraci s ITSM produktů nebo služeb, kde poskytuje hello OMS uživatele možnost toocreate incidenty, výstrahy nebo události v ITSM řešení. Hello konektor importuje také data, jako jsou incidenty a žádostí o změny z ITSM řešení do analýzy protokolů OMS.

Pomocí konektoru služby správy IT můžete:

  - Centrálně monitorovat a spravovat pracovní položky pro ITSM produkty nebo služby používané ve vaší organizaci.
  - Vytvoření ITSM pracovních položek (například výstrahy, události, incident) v ITSM z OMS výstrah a prostřednictvím protokolu vyhledávání.
  - Čtení incidenty a žádosti o změnu z řešení ITSM a korelovat s daty relevantní protokolu v pracovní prostor analýzy protokolů.
  - Najde všechny neočekávanou a neobvyklé události a vyřešit, ještě před hello koncoví uživatelé volání a sestav je toohello technickou podporu.
  - Importovat data pracovních položek do analýzy protokolů a vytváření sestav výkonnosti klíče pro Klíčový ukazatel výkonu.  Pomocí těchto sestav, můžete identifikovat, hodnocení a fungovat na některé důležité skutečnosti, jako je například posouzením malwaru.
  - Zobrazit kurátorované řídicí panely pro podrobnější přehled na incidenty, žádostí o změnu a ovlivněné systémy.
  - Řešení potíží s rychlejší pomocí souvztažnosti pomocí jiných řešení pro správu v pracovní prostor analýzy protokolů hello.


## <a name="prerequisites"></a>Požadavky

tooimport hello ITSM pracovních položek do OMS analýzy protokolů, hello řešení vyžaduje připojení mezi hello konektoru služby správy IT v hello OMS a IT SM hello produktům a službám, ze kterých importujete hello pracovní položky.


## <a name="configuration"></a>Konfigurace

Přidat hello IT Service Management Connector řešení tooyour OMS pracovní prostor, pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).

Konektoru Service Management IT dlaždici, jak vidíte v Galerii řešení hello:

![dlaždice konektoru](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Po úspěšném přidání, uvidíte hello správu konektoru služby IT v rámci **OMS** > **nastavení** > **připojené zdroje.**

![ITSMC připojení](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Ve výchozím nastavení hello IT Service Management Connector aktualizuje data hello připojení jednou za každých 24 hodin. toorefresh data vaše připojení okamžitě pro všechny úpravy nebo šablonu aktualizace, které provedete, klikněte na tlačítko hello aktualizace tlačítko zobrazí další tooyour připojení.

 ![Aktualizace ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Sady Management Pack
Toto řešení nevyžaduje žádné sady management Pack.

## <a name="connected-sources"></a>Připojené zdroje

Hello následující ITSM produkty nebo služby podporuje hello konektoru služby správy IT:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-hello-solution"></a>Pomocí řešení hello

Jakmile se připojíte hello OMS IT Service Management Connector s ITSM službou, hello analýzy protokolů služby spustí, shromažďování dat hello z hello připojené ITSM produkty nebo služby.

> [!NOTE]
> - Dat importovaných pomocí konektoru služby správy IT řešení se zobrazí v analýzy protokolů jako události s názvem **ServiceDesk_CL**.
- Událost obsahuje pole s názvem **ServiceDeskWorkItemType_s**. které trvat jeho hodnotu jako incident nebo žádost o změnu, v závislosti na hello pracovních položek data obsažená v hello **ServiceDesk_CL** událostí.

## <a name="input-data"></a>Vstupní data
Pracovní položky importovány z hello ITSM produktů nebo služeb.

Hello následující informace uvádí příklady data shromážděná konektorem hello správy služeb IT:

> [!NOTE]
> V závislosti na hello typ pracovní položky naimportovat do analýzy protokolů **ServiceDesk_CL** obsahuje hello následující pole:

**Pracovní položka:** **incidenty**  
ServiceDeskWorkItemType_s = "Incidentem"

**Pole**

- ServiceDeskConnectionName
- ID služby podpory
- Stav
- Naléhavosti
- Dopad
- Priorita
- Eskalaci
- Autor
- Vyřešil
- Uzavřené
- Zdroj
- Přiřazené
- Kategorie
- Název
- Popis
- Datum vytvoření
- Datum uzavření
- Datum vyřešení
- Datum poslední změny
- Počítač


**Pracovní položka:** **žádosti o změnu**

ServiceDeskWorkItemType_s = "ChangeRequest"

**Pole**
- ServiceDeskConnectionName
- ID služby podpory
- Autor
- Uzavřené
- Zdroj
- Přiřazené
- Název
- Typ
- Kategorie
- Stav
- Eskalaci
- Konflikt stavu
- Naléhavosti
- Priorita
- Riziko
- Dopad
- Přiřazené
- Datum vytvoření
- Datum uzavření
- Datum poslední změny
- Požadovaná data
- Plánované počáteční datum
- Naplánované koncové datum
- Pracovní počáteční datum
- Pracovní koncové datum
- Popis
- Počítač

## <a name="output-data-for-a-servicenow-incident"></a>Výstupní data pro ServiceNow incident

| Pole OMS | ITSM pole |
|:--- |:--- |
| ServiceDeskId_s| Číslo |
| IncidentState_s | Stav |
| Urgency_s |Naléhavosti |
| Impact_s |Dopad|
| Priority_s | Priorita |
| CreatedBy_s | Otevřít |
| ResolvedBy_s | Vyřešil|
| ClosedBy_s  | Uzavřené |
| Source_s| Obraťte se na typ |
| AssignedTo_s | Přiřazené příliš |
| Category_s | Kategorie |
| Title_s|  Krátký popis |
| Description_s|  Poznámky |
| CreatedDate_t|  Otevřít |
| ClosedDate_t| uzavřený|
| ResolvedDate_t|Vyřešil|
| Počítač  | položky konfigurace |

## <a name="output-data-for-a-servicenow-change-request"></a>Výstupní data pro ServiceNow žádost o změnu

| Pole OMS | ITSM pole |
|:--- |:--- |
| ServiceDeskId_s| Číslo |
| CreatedBy_s | Požadoval |
| ClosedBy_s | Uzavřené |
| AssignedTo_s | Přiřazené příliš |
| Title_s|  Krátký popis |
| Type_s|  Typ |
| Category_s|  Catgory |
| CRState_s|  Stav|
| Urgency_s|  Naléhavosti |
| Priority_s| Priorita|
| Risk_s| Riziko|
| Impact_s| Dopad|
| RequestedDate_t  | Požadovaná data |
| ClosedDate_t | Datum uzavření |
| PlannedStartDate_t  |     Plánované počáteční datum |
| PlannedEndDate_t  |   Plánované koncové datum |
| WorkStartDate_t  | Skutečné počáteční datum |
| WorkEndDate_t | Skutečné koncové datum|
| Description_s | Popis |
| Počítač  | Položky konfigurace |

**Ukázka obrazovky analýzy protokolů pro ITSM data:**

![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>Konektoru Service Management IT – integrace s jinými řešeními OMS

IT Service Management Connector aktuálně podporuje integraci se službou hello řešení mapy služeb.

Mapa služeb automaticky vyhledá hello součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami. Umožňuje vám tooview vaše servery, jako jste je – představit jako vzájemně propojena systémy, které poskytování důležitých služeb. Mapy služeb zobrazí připojení mezi servery, procesy, a vyžaduje porty mezi žádné připojení TCP architektura žádnou konfiguraci, jiné než instalace agenta. Další informace: [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md).

Díky této integraci můžete zobrazit hello služby podpory položky vytvořené v hello ITSM řešení, jak je znázorněno v hello následující ukázka:

![Integrované řešení ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Vytváření pracovních položek ITSM OMS výstrahy

Pro hello OMS výstrahy můžete vytvořit přidružených pracovních položek v hello připojené ITSM zdroje.  toodo hello tento, použijte následující postup:

1. Z **hledání protokolů** okně tooview dat protokolu vyhledávací dotaz. Výsledky dotazu jsou hello zdroj pro pracovní položky.
2. V **hledání protokolů**, klikněte na tlačítko **výstrahy** tooopen hello **přidat pravidlo výstrahy** stránky.

    ![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Na hello **přidat pravidlo výstrahy** okno, zadejte hello požadované podrobnosti **název**, **závažnost**, **vyhledávací dotaz**, a  **Výstrahy kritéria** (časová okna/metrika měří období).
4. Vyberte **Ano** pro **ITSM akce**.
5. Vyberte připojení ITSM z hello **vyberte připojení** seznamu.
6. Zadejte podrobnosti hello podle potřeby.
7. toocreate samostatné pracovní položku pro každou položku protokolu této výstrahy, vyberte hello **vytvořit jednotlivé pracovní položky pro každý záznam protokolu** zaškrtávací políčko.

    Nebo

    nechte toto políčko nezaškrtnuté toocreate jenom jeden pracovní položku pro libovolný počet záznamů protokolu v rámci této výstrahy.

7. Klikněte na **Uložit**.

bude vytvořena výstraha OMS Hello ve **výstrahy**. Hello pracovním odpovídající ITSM připojení položky vytvářejí, když je splněna podmínka hello zadaný výstrahy.

## <a name="create-itsm-work-items-from-oms-logs"></a>Vytváření pracovních položek ITSM z protokolů OMS

Pracovní položky můžete vytvořit v zdroje ITSM hello připojené pomocí vyhledávání protokolu OMS. toodo hello tento, použijte následující postup:

1. Z **hledání protokolů**, vyhledávání hello požadované dat, vyberte hello podrobností a klikněte na **vytvořit pracovní položka**.

    Hello **vytvořit ITSM pracovní položka** zobrazí se okno:

    ![Obrazovka analýzy protokolů](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Přidejte hello následující podrobnosti:

  - **Název pracovní položky**: název hello pracovní položku.
  - **Pracovní položky Popis**: popis hello novou pracovní položku.
  - **Vliv na počítače**: název hello počítače, kde tato data protokolu byla nalezena.
  - **Vyberte připojení**: ITSM připojení, ve kterém má toocreate tuto pracovní položku.
  - **Pracovní položka**: typ pracovní položky.

3. toouse existující šablony pracovní položky pro incidentu, klikněte na tlačítko **Ano** pod **generování pracovní položka založený na šabloně hello** a potom klikněte na **vytvořit**.

    Nebo:

    Klikněte na tlačítko **ne** Pokud chcete tooprovide svoje vlastní hodnoty.

4. Zadejte odpovídající hodnoty hello v hello **typu Kontakt**, **dopad**, **naléhavost**, **kategorie**, a **dílčí kategorie**  textová pole a pak klikněte na tlačítko **vytvořit**.

Hello pracovní položka bude vytvořen v hello ITSM, které lze také prohlížet v OMS.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Řešení potíží s ITSM připojení v OMS
1.  Pokud připojení selže z připojeného zdroje uživatelského rozhraní a získat hello **Chyba při ukládání připojení** zprávy, hello následující:
 - V případě připojení ServiceNow, Cherwell a Provance si zajistěte hello správně zadané uživatelské jméno a heslo a klient ID nebo sdílený tajný klíč klienta pro každé připojení hello. Pokud hello chyba přetrvává, zkontrolujte, pokud máte dostatečná oprávnění v hello odpovídající ITSM produktu toomake hello připojení.
 - V případě portálu Service Manager, zajistěte, aby hello webové aplikace je úspěšně nasazen a hybridní připojení se vytvoří. hello místní portálu Service Manager počítač se úspěšně naváže připojení hello tooverify, navštivte adresa URL webové aplikace hello podle popisu v dokumentaci hello provedení hello [hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Pokud není získávání synchronizovat data z ServiceNow v OMS, zajistěte, že instance ServiceNow hello není pozastaveno. To může nějakou dobu zopakovat dojít v hello instance ServiceNow vývojářů, při nečinnosti. #Else problém hello sestavy.
3.  Pokud při získávání vyvolání výstrahy od OMS, ale nejsou získávání pracovní položky vytvořené v produktu ITSM nebo položek konfigurace nejsou získávání toowork vytvoření propojené položky nebo pro všechny obecné informace, hello následující:
 -  Řešení pro správu konektoru služby IT na portálu OMS může být použité tooget souhrn připojení nebo pracovní položky nebo počítače atd. Klikněte na tlačítko hello chybová zpráva v okně hello stav, přejděte příliš**hledání protokolů** a zobrazit hello připojení, které obsahuje chybu hello pomocí příkazu podrobnosti hello v hello chybová zpráva.
 - Zobrazí informace týkající se/chyby hello se přímo v hello **hledání protokolů** stránky pomocí *typ = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Řešení potíží s nasazení aplikace webového portálu Service Manager
1.  V případě jakékoli potíže při nasazení webové aplikace, zajistěte, abyste měli dostatečná oprávnění v rámci předplatného hello uvedených toocreate nebo nasazení prostředků.
2.  Pokud **odkaz na objekt není nastaven tooinstance objektu** chybová zpráva se zobrazí při spuštění hello [skriptu](log-analytics-itsmc-service-manager-script.md) zkontrolujte, zda jste zadali platné hodnoty v části **konfigurace uživatele**části.
3.  Pokud tak předávání názvů toocreate sběrnice, ujistěte se, že hello vyžaduje poskytovatele prostředků je zaregistrován v rámci předplatného hello. Pokud není zaregistrovaný, ručně vytvořte ho z hello portálu Azure. Můžete také vytvořit ji při [vytváření hello hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) z hello portálu Azure.


## <a name="contact-us"></a>Kontaktujte nás

Pro dotazy nebo zpětnou vazbu na hello konektoru správy IT služby, kontaktujte nás na adrese [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Další kroky
[Přidat tooIT ITSM produkty nebo služby Service Management Connector](log-analytics-itsmc-connections.md).
