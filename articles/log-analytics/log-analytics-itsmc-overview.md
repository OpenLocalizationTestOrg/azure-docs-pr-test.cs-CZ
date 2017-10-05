---
title: "IT služby konektoru Management v OMS | Microsoft Docs"
description: "Pomocí konektoru služby správy IT centrálně monitorovat a spravovat ITSM pracovních položek v OMS a rychle vyřešit všechny problémy."
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
ms.openlocfilehash: 54974ef06efdae69ddbfa12b1ba9278b48b113d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Centrálně spravovat ITSM pracovních položek pomocí IT Service Management Connector (Preview)

![Symbol konektoru služby správy IT](./media/log-analytics-itsmc/itsmc-symbol.png)

Konektor pro správu služby IT (ITSMC) v OMS analýzy protokolů můžete použít centrálně monitorovat a spravovat pracovní položky v rámci ITSM produktů nebo služeb.

Konektor služby správy IT se integruje s analýzy protokolů OMS existující IT služby správy (ITSM) produkty a služby.  Řešení obsahuje obousměrnou integraci s ITSM produktů nebo služeb, přičemž poskytuje uživatelům OMS možnost pro vytvoření incidentů, výstrahy nebo události v ITSM řešení. Konektor importuje také data, jako jsou incidenty a žádostí o změny z ITSM řešení do analýzy protokolů OMS.

Pomocí konektoru služby správy IT můžete:

  - Centrálně monitorovat a spravovat pracovní položky pro ITSM produkty nebo služby používané ve vaší organizaci.
  - Vytvoření ITSM pracovních položek (například výstrahy, události, incident) v ITSM z OMS výstrah a prostřednictvím protokolu vyhledávání.
  - Čtení incidenty a žádosti o změnu z řešení ITSM a korelovat s daty relevantní protokolu v pracovní prostor analýzy protokolů.
  - Najde všechny neočekávanou a neobvyklé události a vyřešit, ještě před koncovým uživatelům volání a nahlásit oddělení technické podpory.
  - Importovat data pracovních položek do analýzy protokolů a vytváření sestav výkonnosti klíče pro Klíčový ukazatel výkonu.  Pomocí těchto sestav, můžete identifikovat, hodnocení a fungovat na některé důležité skutečnosti, jako je například posouzením malwaru.
  - Zobrazit kurátorované řídicí panely pro podrobnější přehled na incidenty, žádostí o změnu a ovlivněné systémy.
  - Řešení potíží s rychlejší pomocí souvztažnosti pomocí jiných řešení pro správu v pracovním prostoru analýzy protokolů.


## <a name="prerequisites"></a>Požadavky

Pokud chcete importovat ITSM pracovních položek do OMS analýzy protokolů, řešení vyžaduje připojení mezi konektor správy služeb IT v OMS a na IT SM produktům a službám, ze kterého můžete importovat pracovní položky.


## <a name="configuration"></a>Konfigurace

Přidání konektoru služby správy IT řešení do prostoru pracovní OMS, pomocí procesu popsaného v tématu [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md).

Konektoru Service Management IT dlaždici, jak vidíte v Galerii řešení:

![dlaždice konektoru](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Po úspěšném přidání, zobrazí se konektoru Management Service IT v rámci **OMS** > **nastavení** > **připojené zdroje.**

![ITSMC připojení](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Ve výchozím nastavení konektor služby správy IT aktualizuje data připojení jednou za každých 24 hodin. Aktualizovat připojení k data okamžitě pro úpravy nebo aktualizace šablony, které provedete, klepněte na tlačítko Aktualizovat se zobrazí vedle připojení.

 ![Aktualizace ITSMC](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Sady Management Pack
Toto řešení nevyžaduje žádné sady management Pack.

## <a name="connected-sources"></a>Připojené zdroje

Podporuje následující ITSM produkty nebo služby konektoru IT Service Management:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Použití řešení

Jakmile se připojíte konektor OMS IT službu správy s ITSM službou, analýzy protokolů služby spustí, shromažďování dat z připojených ITSM produkty nebo služby.

> [!NOTE]
> - Dat importovaných pomocí konektoru služby správy IT řešení se zobrazí v analýzy protokolů jako události s názvem **ServiceDesk_CL**.
- Událost obsahuje pole s názvem **ServiceDeskWorkItemType_s**. která může převzít jeho hodnotu jako incident, nebo v závislosti na data pracovních položek, které jsou obsažené v žádosti o změnu **ServiceDesk_CL** událostí.

## <a name="input-data"></a>Vstupní data
Pracovní položky, které jsou importovány z ITSM produkty nebo služby.

Tyto informace jsou uvedeny příklady data shromážděná konektorem správy služeb IT:

> [!NOTE]
> V závislosti na typu pracovní položky naimportovat do analýzy protokolů **ServiceDesk_CL** obsahuje následující pole:

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
| AssignedTo_s | Přiřazené  |
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
| AssignedTo_s | Přiřazené  |
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

IT Service Management Connector aktuálně podporuje integraci s řešením mapy služeb.

Mapa služeb automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapuje komunikace mezi službami. Umožňuje zobrazit vaše servery co možná z nich – jako vzájemně propojena systémy, které doručují důležité služby. Mapy služeb zobrazí připojení mezi servery, procesy, a vyžaduje porty mezi žádné připojení TCP architektura žádnou konfiguraci, jiné než instalace agenta. Další informace: [mapy služeb](../operations-management-suite/operations-management-suite-service-map.md).

Díky této integraci můžete zobrazit položky služby podpory, které jsou vytvořené v ITSM řešení, jak je znázorněno v následujícím příkladu:

![Integrované řešení ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Vytváření pracovních položek ITSM OMS výstrahy

Pro OMS výstrahy můžete vytvořit přidružených pracovních položek v připojených ITSM zdroje.  Chcete-li to provést, použijte následující postup:

1. Z **hledání protokolů** okně Spustit protokolu vyhledávací dotaz. Chcete-li zobrazit data. Výsledky dotazu jsou zdroj pro pracovní položky.
2. V **hledání protokolů**, klikněte na tlačítko **výstrahy** otevřete **přidat pravidlo výstrahy** stránky.

    ![Obrazovka analýzy protokolů](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Na **přidat pravidlo výstrahy** okno, zadejte požadované podrobnosti pro **název**, **závažnost**, **vyhledávací dotaz**, a **výstrah kritéria** (časová okna/metrika měří období).
4. Vyberte **Ano** pro **ITSM akce**.
5. Vyberte ITSM připojení z **vyberte připojení** seznamu.
6. Zadejte podrobnosti podle potřeby.
7. Vytvořit samostatný pracovní položku pro každé položky protokolu této výstrahy, vyberte **vytvořit jednotlivé pracovní položky pro každý záznam protokolu** zaškrtávací políčko.

    Nebo

    nechte toto políčko Zrušit vytvořit pouze jeden pracovní položku pro libovolný počet záznamů protokolu v rámci této výstrahy.

7. Klikněte na **Uložit**.

Bude vytvořena výstraha OMS ve **výstrahy**. Pracovní položky odpovídající ITSM připojení vytvářejí, když je splněna podmínka zadaný výstrahy.

## <a name="create-itsm-work-items-from-oms-logs"></a>Vytváření pracovních položek ITSM z protokolů OMS

Pracovní položky můžete vytvořit v připojených zdrojů ITSM pomocí vyhledávání protokolu OMS. Chcete-li to provést, použijte následující postup:

1. Z **hledání protokolů**hledání požadovaných dat, vyberte podrobností a klikněte na tlačítko **vytvořit pracovní položka**.

    **Vytvořit ITSM pracovní položka** zobrazí se okno:

    ![Obrazovka analýzy protokolů](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Přidejte následující podrobnosti:

  - **Název pracovní položky**: název pro pracovní položku.
  - **Pracovní položky Popis**: popis novou pracovní položku.
  - **Vliv na počítače**: název počítače, kde tato data protokolu byla nalezena.
  - **Vyberte připojení**: ITSM připojení, ve kterém chcete vytvořit tuto pracovní položku.
  - **Pracovní položka**: typ pracovní položky.

3. Chcete-li použít existující šablonu pracovní položky pro incidentu, klikněte na tlačítko **Ano** pod **generování pracovní položka založený na šabloně** a potom klikněte na **vytvořit**.

    Nebo:

    Klikněte na tlačítko **ne** Pokud chcete zadat vlastní hodnoty.

4. Zadejte odpovídající hodnoty v **typu Kontakt**, **dopad**, **naléhavost**, **kategorie**, a **dílčí kategorie** textová pole a pak klikněte na tlačítko **vytvořit**.

Pracovní položka bude vytvořen v ITSM, které lze také prohlížet v OMS.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Řešení potíží s ITSM připojení v OMS
1.  Pokud připojení selže z připojeného zdroje uživatelského rozhraní a můžete získat **Chyba při ukládání připojení** zprávy, postupujte takto:
 - V případě připojení ServiceNow, Cherwell a Provance Ujistěte se, že jste správně zadali uživatelské jméno a heslo a klient ID nebo klient tajný klíč pro jednotlivá připojení. Pokud chyba přetrvává, zkontrolujte, pokud máte dostatečná oprávnění v rámci odpovídající ITSM produktu pro připojení.
 - V případě portálu Service Manager, zajistěte, aby webová aplikace je úspěšně nasazen a hybridní připojení se vytvoří. Ověřte připojení se úspěšně naváže na místní počítač portálu Service Manager, najdete na adresu URL webové aplikace podle popisu v dokumentaci k provádění [hybridní připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Pokud není získávání synchronizovat data z ServiceNow v OMS, zajistěte, aby ServiceNow instance není pozastaveno. K tomu může dojít chvíli v instance ServiceNow vývojářů, při nečinnosti. Jinak ohlaste daný problém.
3.  Pokud při získávání vyvolání výstrahy od OMS, ale pracovní položky nejsou získávání vytvořené v ITSM produktu nebo konfigurace položek nezobrazují vytvořen nebo propojené pracovní položky nebo nějaké obecné informace o, postupujte takto:
 -  Řešení pro správu konektoru služby IT na portálu OMS může použít k získání shrnutí připojení nebo pracovní položky nebo počítače atd. Klikněte na tlačítko chybová zpráva v okně Stav, přejděte na **hledání protokolů** a zobrazit připojení, který má chyba pomocí podrobnosti v chybové zprávě.
 - přímo můžete zobrazit informace týkající se nebo chyby v **hledání protokolů** stránky pomocí *typ = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Řešení potíží s nasazení aplikace webového portálu Service Manager
1.  V případě jakékoli potíže při nasazení webové aplikace zkontrolujte, zda že máte dostatečná oprávnění v rámci předplatného uvedených vytvořit nebo nasadit prostředky.
2.  Pokud **objektu odkaz není nastaven na instanci objektu** chybová zpráva se zobrazí při spuštění [skriptu](log-analytics-itsmc-service-manager-script.md) zkontrolujte, zda jste zadali platné hodnoty v části **konfigurace uživatele** oddíl.
3.  Pokud se vám nepodaří vytvoření oboru názvů service bus relay, ujistěte se, že zprostředkovatel požadovaný prostředek je zaregistrován v rámci předplatného. Pokud není zaregistrovaný, ručně vytvořte ho z portálu Azure. Můžete také vytvořit ji při [vytvoření hybridního připojení](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) z portálu Azure.


## <a name="contact-us"></a>Kontaktujte nás

Pro dotazy nebo zpětnou vazbu na správu konektoru služby IT, kontaktujte nás na adrese [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Další kroky
[Přidání ITSM produktů nebo služeb do konektoru služby správy IT](log-analytics-itsmc-connections.md).
