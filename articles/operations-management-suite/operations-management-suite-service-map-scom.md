---
title: "Mapa integrace s nástrojem System Center Operations Manager aaaService | Microsoft Docs"
description: "Mapa služeb je do řešení služby Operations Management Suite, který automaticky zjišťuje součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami. Tento článek popisuje pomocí mapy služeb tooautomatically vytváření diagramů distribuované aplikace v nástroji Operations Manager."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: e8614a5a-9cf8-4c81-8931-896d358ad2cb
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: bwren;dairwin
ms.openlocfilehash: cff9cce2559448ec3a5fd14087b867f314716560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-map-integration-with-system-center-operations-manager"></a>Integrace mapy služeb s nástrojem System Center Operations Manager
  > [!NOTE]
  > Tato funkce je ve verzi public preview.
  > 
  
Mapy Operations Management Suite služby automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapuje hello komunikace mezi službami. Mapa služeb umožňuje tooview způsob hello servery, jak si myslíte z nich, jako vzájemně propojena systémy, které doručují důležité služby. Mapy služeb zobrazí připojení mezi servery, procesy a porty mezi žádné připojení TCP architektura žádnou konfiguraci vyžaduje kromě hello instalace agenta. Další informace najdete v tématu hello [mapy služeb dokumentaci](operations-management-suite-service-map.md).

Díky této integraci mezi mapy služeb a System Center Operations Manager můžete automaticky vytvořit diagramy distribuované aplikace v nástroji Operations Manager, které jsou založeny na hello map dynamické závislostí v mapy služeb.

## <a name="prerequisites"></a>Požadavky
* Skupinu správy nástroje Operations Manager, který je Správa u sady serverů.
* Prostoru Operations Management Suite s hello povolené žádné řešení mapy služeb.
* Sada serverů (alespoň jeden), které spravuje nástroj Operations Manager a odesílání dat tooService mapy. Servery se systémy Windows a Linux jsou podporovány.
* Objekt služby s přístup toohello předplatné Azure, který je přidružený k pracovnímu prostoru služby Operations Management Suite hello. Další informace, přejděte příliš[vytvoření instančního objektu](#creating-a-service-principal).

## <a name="install-hello-service-map-management-pack"></a>Instalovat sadu management pack hello mapy služeb
Povolíte hello integrace mezi nástrojem Operations Manager a Service Map importováním hello Microsoft.SystemCenter.ServiceMap komplet sady management pack (Microsoft.SystemCenter.ServiceMap.mpb). Hello sady obsahuje hello následující sady management Pack:
* Zobrazení aplikací služby Microsoft mapy
* Mapa služeb Microsoft System Center interní
* Přepsání mapy služby Microsoft System Center
* Mapa služeb Microsoft System Center

## <a name="configure-hello-service-map-integration"></a>Konfigurace integrace mapy služeb hello
Po instalaci hello mapy služeb management pack, nový uzel, **mapy služeb**, se zobrazí v části **Operations Management Suite** v hello **správy** podokně. 

tooconfigure integrace mapy služeb hello následující:

1. Průvodce konfigurací hello tooopen, v hello **přehled mapy služby** podokně klikněte na tlačítko **přidání prostoru**.  

    ![Přehled služby mapy podokno](media/oms-service-map/scom-configuration.png)

2. V hello **konfigurace připojení** okno, zadejte název klienta hello nebo ID, ID aplikace (také označované jako uživatelské jméno hello nebo clientID) a heslo hello instanční objekt a potom klikněte na **Další**. Další informace, přejděte příliš[vytvoření instančního objektu](#creating-a-service-principal).

    ![okno Konfigurace připojení Hello](media/oms-service-map/scom-config-spn.png)

3. V hello **výběr předplatného** okně vyberte hello předplatné, skupinu prostředků Azure (hello, která obsahuje pracovní prostor služby Operations Management Suite hello) a pracovní prostor služby Operations Management Suite a pak klikněte na **Další**.

    ![Hello pracovní prostor služby Operations Manager konfigurace](media/oms-service-map/scom-config-workspace.png)

4. V hello **výběr skupiny počítače** okně vyberte skupiny, které služba mapy počítače chcete toosync tooOperations Manager. Klikněte na tlačítko **skupiny počítačů přidat nebo odebrat**, vyberte skupiny ze seznamu hello **dostupných skupin počítačů**a klikněte na tlačítko **přidat**.  Po dokončení výběru skupiny klikněte na **Ok** toofinish.
    
    ![Hello skupiny počítače konfigurace Operations Manager](media/oms-service-map/scom-config-machine-groups.png)
    
5. V hello **výběr serveru** okně nakonfigurovat hello skupin serverů mapy služby se hello servery, které chcete toosync mezi nástrojem Operations Manager a mapy služeb. Klikněte na tlačítko **přidat nebo odebrat servery**.   
    
    Pro hello integrace toobuild diagramu distribuované aplikace pro server musí být hello server:

    * Spravován nástrojem Operations Manager
    * Spravuje mapy služeb
    * Uvedené v hello skupin serverů služby mapy

    ![Hello skupina konfigurace nástroje Operations Manager](media/oms-service-map/scom-config-group.png)

6. Volitelné: Vyberte hello serveru pro správu prostředků fondu toocommunicate s Operations Management Suite a pak klikněte na tlačítko **přidání prostoru**.

    ![Hello fond zdrojů konfigurace Operations Manager](media/oms-service-map/scom-config-pool.png)

    Ho může trvat minutu tooconfigure a zaregistrujte hello prostoru služby Operations Management Suite. Po dokončení své konfigurace, Operations Manager inicializuje hello první mapy služeb synchronizace ze služby Operations Management Suite.

    ![Hello fond zdrojů konfigurace Operations Manager](media/oms-service-map/scom-config-success.png)


## <a name="monitor-service-map"></a>Monitorování mapy služeb
Až se pracovní prostor služby Operations Management Suite hello připojí, novou složku, mapy služeb, se zobrazí v hello **monitorování** podokně konzoly nástroje Operations Manager hello.

![Monitorování nástroje Operations Manager podokně Hello](media/oms-service-map/scom-monitoring.png)

Hello mapy služeb složka obsahuje čtyři uzly:
* **Aktivní výstrahy**: uvádí všechny aktivní výstrahy hello o hello komunikaci mezi nástrojem Operations Manager a mapy služeb.  Všimněte si, že tyto výstrahy nejsou výstrah Operations Management Suite se synchronizoval tooOperations správce. 

* **Servery**: seznamy hello monitorované servery, které jsou nakonfigurované toosync z mapy služeb.

    ![Podokno Hello servery monitorování nástroje Operations Manager](media/oms-service-map/scom-monitoring-servers.png)

* **Počítač zobrazení závislostí skupiny**: uvádí všechny skupiny počítačů, které jsou synchronizované z mapy služeb. Můžete kliknout na všechny skupiny tooview jeho diagramu distribuované aplikace.

    ![diagram distribuované aplikace Hello nástroje Operations Manager](media/oms-service-map/scom-group-dad.png)

* **Zobrazení závislostí server**: uvádí všechny servery, které jsou synchronizované z mapy služeb. Můžete kliknout na libovolný server tooview jeho diagramu distribuované aplikace.

    ![diagram distribuované aplikace Hello nástroje Operations Manager](media/oms-service-map/scom-dad.png)

## <a name="edit-or-delete-hello-workspace"></a>Upravit nebo odstranit pracovní prostor hello
Můžete upravit nebo odstranit pracovní prostor hello nakonfigurované prostřednictvím hello **přehled mapy služby** podokně (**správy** podokně > **Operations Management Suite**  >  **Služby mapy**). Teď můžete konfigurovat jenom jeden pracovní prostor služby Operations Management Suite.

![Hello podokně pracovní prostor upravit služby Operations Manager](media/oms-service-map/scom-edit-workspace.png)

## <a name="configure-rules-and-overrides"></a>Konfigurace pravidla a přepsání
Pravidlo, _Microsoft.SystemCenter.ServiceMapImport.Rule_, je vytvořen tooperiodically načítání informací z mapy služeb. časování toochange synchronizace, můžete nakonfigurovat přepsání pravidel hello (**vytváření** podokně > **pravidla** > **Microsoft.SystemCenter.ServiceMapImport.Rule**).

![okno vlastností Operations Manager přepíše Hello](media/oms-service-map/scom-overrides.png)

* **Povolit**: Povolit nebo zakázat automatické aktualizace. 
* **IntervalMinutes**: resetování hello čas mezi aktualizace. Výchozí interval Hello je jedna hodina. Pokud chcete server mapy toosync častěji, můžete změnit hodnotu hello.
* **TimeoutSeconds**: resetování hello délku doby, než vyprší časový limit žádosti hello. 
* **TimeWindowMinutes**: resetování hello časový interval pro dotazování na data. Výchozí hodnota je 60 minut okno. Hello povolená pomocí mapy služeb maximální hodnota je 60 minut.

## <a name="known-issues-and-limitations"></a>Známé problémy a omezení

Hello aktuálního návrhu uvede hello následující problémy a omezení:
* Budete moct připojit jen tooa jeden pracovní prostor služby Operations Management Suite.
* I když můžete přidat servery toohello skupin serverů mapy služby ručně prostřednictvím hello **vytváření** podokně hello mapy pro tyto servery není synchronizovaná okamžitě.  Bude se synchronizovat ze služby mapy při příštím synchronizačním cyklu hello.
* Pokud provedete změny toohello distribuované aplikace diagramy vytvořené hello sady management pack, tyto změny budou přepsány pravděpodobně na příští synchronizaci hello pomocí mapy služeb.

## <a name="create-a-service-principal"></a>Vytvoření instančního objektu
Oficiální Azure dokumentaci o vytvoření objektu služby najdete v tématu:
* [Vytvoření objektu služby pomocí prostředí PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Vytvořit objekt služby pomocí rozhraní příkazového řádku Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)
* [Vytvořit objekt služby s použitím hello portálu Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)

### <a name="feedback"></a>Váš názor
Máte k dispozici žádné zpětná vazba nám o mapy služeb nebo této dokumentace? Navštivte naše [User Voice stránky](https://feedback.azure.com/forums/267889-log-analytics/category/184492-service-map), kde můžete navrhnout funkce nebo hlasovat o existující návrhy.
