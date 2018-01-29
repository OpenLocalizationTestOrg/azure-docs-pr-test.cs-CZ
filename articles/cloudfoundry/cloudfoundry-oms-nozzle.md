---
title: "Nasazení Azure Log Analytics trysek pro monitorování cloudu Foundry | Microsoft Docs"
description: "Podrobné pokyny pro nasazení cloudu Foundry loggregator trysek pro analýzy protokolů Azure. Ke sledování metrik Foundry cloudu systému stavu a výkonu použijte tryska."
services: virtual-machines-linux
documentationcenter: 
author: ningk
manager: timlt
editor: 
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.openlocfilehash: 0d13d39d2921c51c537534a5b000564a9df91880
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Nasazení Azure Log Analytics trysek pro monitorování systému cloudu Foundry

[Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/) je služba v Microsoft [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/) (OMS). Umožňuje shromažďovat a analyzovat data, která se generují z vašeho cloudu a místní prostředí.

Trysek analýzy protokolů (trysek) je součástí cloudu Foundry (CR), který předává metriky z [cloudu Foundry loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) firehose k analýze protokolů. S trysek můžete shromažďovat, zobrazit a analyzovat vaše CR systému stavu a metrik výkonu, napříč více nasazení.

V tomto dokumentu zjistěte, jak nasadit tryska do prostředí CR a poté přístup k datům z konzoly protokolů analýzy OMS.

## <a name="prerequisites"></a>Požadavky

Následující kroky jsou požadavky pro nasazení trysek.

### <a name="1-deploy-a-cf-or-pivotal-cloud-foundry-environment-in-azure"></a>1. Nasazení CR nebo hrají Foundry cloudové prostředí v Azure

Tryska můžete použít s otevřeným zdrojem CR nasazení nebo nasazení hrají cloudu Foundry (PCF).

* [Nasazení cloudu Foundry v Azure](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)

* [Nasazení Foundry hrají cloudu v Azure](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. Nainstalujte nástroje příkazového řádku pro nasazení trysek CR

Tryska běží jako aplikace v prostředí CR. Je nutné CR rozhraní příkazového řádku k nasazení aplikace.

Tryska musí také přístupová oprávnění k loggregator firehose a Kontroleru cloudu. Chcete-li vytvořit a konfigurovat uživatele, musíte službu uživatelský účet a ověřování (UAA).

* [Nainstalujte cloudu Foundry rozhraní příkazového řádku](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

* [Instalace klienta na cloudu Foundry UAA příkazového řádku](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md)

Před nastavením klienta UAA příkazového řádku, zkontrolujte, zda je nainstalován Rubygems.

### <a name="3-create-an-oms-workspace-in-azure"></a>3. Vytvořit pracovní prostor OMS služby v Azure

Pracovní prostor OMS můžete vytvořit ručně nebo pomocí šablony. Po dokončení nasazení trysek načíst předem nakonfigurovaná zobrazení OMS a výstrahy.

Ruční vytvoření pracovního prostoru:

1. Na portálu Azure v seznamu služeb Hledat v Azure Marketplace a pak vyberte analýzy protokolů.
2. Vyberte **vytvořit**a potom vyberte možnosti pro následující položky:

   * **Pracovní prostor OMS**: Zadejte název pracovního prostoru.
   * **Předplatné**: Pokud máte více předplatných, vyberte ten, který je stejný jako CR nasazení.
   * **Skupina prostředků**: můžete vytvořit novou skupinu prostředků, nebo použijte stejný s nasazením CR.
   * **Umístění**: Zadejte umístění.
   * **Cenová úroveň**: vyberte **OK** k dokončení.

Další informace najdete v tématu [začít pracovat s analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

Alternativně můžete vytvořit pracovní prostor OMS šablonou OMS. Pomocí této metody šablony načte předem nakonfigurovaná zobrazení OMS a výstrahy automaticky. Další informace najdete v tématu [analýzy protokolů Azure OMS řešení pro Cloud Foundry](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-cloudfoundry-solution).

## <a name="deploy-the-nozzle"></a>Nasazení tryska

Existuje několik různých způsobů, jak nasadit tryska: jako dlaždici PCF nebo jako CR aplikace.

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>Nasazení tryska jako dlaždici PCF Ops Manager

Pokud jste nasadili PCF pomocí nástroje Operations Manager, postupujte podle kroků pro [instalace a konfigurace tryska pro PCF](http://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html). Tryska je nainstalován jako dlaždici s nástrojem Operations Manager.

### <a name="deploy-the-nozzle-as-a-cf-application"></a>Nasaďte tryska jako aplikaci CR

Pokud nepoužíváte PCF Ops Manager, nasazení se tryska jako aplikace. Následující části popisují tohoto procesu.

#### <a name="sign-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>Přihlaste se k nasazení CR jako správce pomocí rozhraní příkazového řádku CR

Spusťte následující příkaz:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

"SYSTEM_DOMAIN" je název domény CR. Můžete ji načíst tak, že "SYSTEM_DOMAIN" v souboru manifestu nasazení CR. 

"CF_User" je název CR správce. Hledání v části "scim", hledá název a "cf_admin_password" v souboru manifestu nasazení CR můžete načíst jméno a heslo.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>Vytvořte CR uživatele a udělit požadovaná oprávnění

Spusťte následující příkazy:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

"SYSTEM_DOMAIN" je název domény CR. Můžete ji načíst tak, že "SYSTEM_DOMAIN" v souboru manifestu nasazení CR.

#### <a name="download-the-latest-log-analytics-nozzle-release"></a>Stáhněte si nejnovější verzi trysek analýzy protokolů

Spusťte následující příkaz:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables"></a>Proměnné prostředí sady

Nyní můžete nastavit proměnné prostředí v souboru manifest.yml v aktuálním adresáři. Následující obrázek znázorňuje manifest aplikace pro tryska. Nahraďte hodnoty konkrétní informace pracovní prostor OMS.

```
OMS_WORKSPACE             : OMS workspace ID: open OMS portal from your OMS workspace, select Settings, and select connected sources.
OMS_KEY                   : OMS key: open OMS portal from your OMS workspace, select Settings, and select connected sources.
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to OMS Log Analytics. The default is 10 seconds.
OMS_BATCH_TIME            : Interval for posting a batch to OMS Log Analytics. The default is 10 seconds.
OMS_MAX_MSG_NUM_PER_BATCH : The maximum number of messages in a batch to OMS Log Analytics. The default is 1000.
API_ADDR                  : The API URL of the CF environment. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
DOPPLER_ADDR              : Loggregator's traffic controller URL. For more information, see the preceding section, "Sign in to your CF deployment as an admin through CF CLI."
FIREHOSE_USER             : CF user you created in the preceding section, "Create a CF user and grant required privileges." This user has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma-separated list. Valid event types are METRIC, LOG, and HTTP.
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the traffic controller.
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments.
IDLE_TIMEOUT              : The Keep Alive duration for the firehose consumer. The default is 60 seconds.
LOG_LEVEL                 : The logging level of the Nozzle. Valid levels are DEBUG, INFO, and ERROR.
LOG_EVENT_COUNT           : If true, the total count of events that the Nozzle has received and sent are logged to OMS Log Analytics as CounterEvents.
LOG_EVENT_COUNT_INTERVAL  : The time interval of the logging event count to OMS Log Analytics. The default is 60 seconds.
```

### <a name="push-the-application-from-your-development-computer"></a>Push aplikace z vývojovém počítači

Ujistěte se, že jste ve složce oms-log-analytics-firehose trysek. Spusťte následující příkaz:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>Ověření instalace trysek

### <a name="from-apps-manager-for-pcf"></a>Od správce aplikace (pro PCF)

1. Přihlaste se k nástroji Operations Manager a ujistěte se, že se zobrazí na dlaždici na řídicím panelu instalace.
2. Přihlaste se k aplikacím Manager, ujistěte se, že na místo, které jste vytvořili pro tryska je uveden v sestavě využití a potvrďte, že stav je normální.

### <a name="from-your-development-computer"></a>Z vývojovém počítači

V okně CR rozhraní příkazového řádku zadejte:
```
cf apps
```
Zkontrolujte, zda že je spuštěna aplikace trysek OMS.

## <a name="view-the-data-in-the-oms-portal"></a>Zobrazení dat na portálu OMS

### <a name="1-import-the-oms-view"></a>1. Import OMS zobrazení

Z portálu OMS, přejděte do **Návrhář zobrazení** > **Import** > **Procházet**a vyberte jeden ze souborů omsview. Vyberte například *cloudu Foundry.omsview*a uložte zobrazení. Nyní se zobrazí na dlaždici na OMS **přehled** stránky. Vyberte Zobrazit vizualizovaných metriky.

Můžete přizpůsobit tato zobrazení nebo vytvořte nové zobrazení prostřednictvím **Návrhář zobrazení**.

*"Cloudu Foundry.omsview"* je verze preview zobrazit šablonu cloudu Foundry OMS. Toto je plně nakonfigurované výchozí šablony. Pokud máte návrhy nebo zpětnou vazbou o šablony, pošlete je [vydání části](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues).

### <a name="2-create-alert-rules"></a>2. Vytváření pravidel upozornění

Můžete [vytvoření výstrahy](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts)a podle potřeby přizpůsobit dotazy a prahové hodnoty. Následující doporučujeme výstrahy:

| Vyhledávací dotaz.                                                                  | Generují výstrahu založenou na | Popis                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Typ = CF_ValueMetric_CL Origin_s = bbs Name_s = "Domain.cf aplikací"                   | Počet výsledků < 1   | **BBS. Aplikace Domain.cf** Určuje, zda je aktuální domény CR aplikace. To znamená, že požadavky aplikace CR z cloudu řadiče jsou synchronizovány do bbs. LRPsDesired (AIs potřeby Diego) pro provedení. Nebyla přijata žádná data znamená, že aplikace CR domény není aktuální v zadaný časový interval. |
| Typ = CF_ValueMetric_CL Origin_s = rep Name_s = UnhealthyCell Value_d > 1            | Počet výsledků > 0   | Pro Diego buněk 0 znamená v pořádku a 1 znamená není v pořádku. Nastavte výstrahy, pokud zjištění více není v pořádku Diego buněk v zadaný časový interval. |
| Typ = CF_ValueMetric_CL Origin_s = "bosh předávání hm" Name_s="system.healthy" Value_d = 0 | Počet výsledků > 0 | 1 znamená systém je v pořádku a 0 znamená, že systém není v pořádku. |
| Typ = CF_ValueMetric_CL Origin_s = route_emitter Name_s = ConsulDownMode Value_d > 0 | Počet výsledků > 0   | Konzul pravidelně vysílá jeho stav. 0 znamená, systém je v pořádku a 1 znamená, že vysílače trasy zjistí, že konzul dolů. |
| Typ = CF_CounterEvent_CL Origin_s Delta_d DopplerServer (Name_s="TruncatingBuffer.DroppedMessages" nebo Name_s="doppler.shedEnvelopes") = > 0 | Počet výsledků > 0 | Rozdílů je počet zpráv Doppler záměrně zrušených kvůli nárokům na pozadí. |
| Typ = CF_LogMessage_CL SourceType_s = LGR MessageType_s = ERR                      | Počet výsledků > 0   | Vysílá Loggregator **LGR** případné problémy s procesem protokolování. Příkladem takových problémů je při výstup zprávu protokolu je příliš vysoká. |
| Typ = CF_ValueMetric_CL Name_s = slowConsumerAlert                               | Počet výsledků > 0   | Když se tryska obdrží upozornění na pomalé příjemce od loggregator, odešle **slowConsumerAlert** ValueMetric k OMS. |
| Typ = CF_CounterEvent_CL Job_s = trysek Name_s = Ztracené události Delta_d > 0              | Počet výsledků > 0   | Pokud počet rozdílů ke ztrátě událostí dosáhne prahové hodnoty, znamená to, že se tryska může jít o problém s. |

## <a name="scale"></a>Měřítko

Je možné škálovat trysek a loggregator.

### <a name="scale-the-nozzle"></a>Škálování tryska

Měli byste začít s aspoň dvě instance tryska. Firehose rozděluje zatížení mezi všechny instance tryska.
Pokud chcete mít jistotu, tryska můžete udržovat tempo s přenosy dat z firehose, nastavte **slowConsumerAlert** výstrahy (uvedené v předchozí části "Vytvoření pravidla výstrah"). Po upozornění můžete podle pokynů [pokyny pro pomalé trysek](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz) k určení, zda je nutné škálování.
Škálování tryska, zvýšit čísla instance nebo prostředky paměti nebo volného pro tryska pomocí aplikace správce nebo rozhraní příkazového řádku CR.

### <a name="scale-the-loggregator"></a>Škálování loggregator

Odešle Loggregator **LGR** zprávy protokolu případné problémy s procesem protokolování. Můžete monitorovat výstrahu, kterou chcete určit, zda loggregator musí možné škálovat.
Škálování loggregator, buď zvětšete velikost Doppler vyrovnávací paměti, nebo přidejte další Doppler server instance v manifestu CR. Další informace najdete v tématu [pokyny pro škálování loggregator](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Aktualizace

Pokud chcete aktualizovat tryska novější verzí, stáhněte si novou verzi trysek, postupujte podle kroků v předchozí části "Nasazení Tryskou" a push aplikaci znovu.

### <a name="remove-the-nozzle-from-ops-manager"></a>Odebrat tryska z nástroje Operations Manager

1. Přihlaste se k nástroji Operations Manager.
2. Vyhledejte **Microsoft Azure Log Analytics trysek pro PCF** dlaždici.
3. Vyberte ikonu uvolňování paměti a potvrďte odstranění.

### <a name="remove-the-nozzle-from-your-development-computer"></a>Odebrání tryska vývojovém počítači

V okně CR rozhraní příkazového řádku zadejte:
```
cf delete <App Name> -r
```

Pokud odeberete tryska, data na portálu OMS automaticky se neodebere. Jeho platnost vyprší podle vašeho nastavení uchování analýzy protokolů OMS.

## <a name="support-and-feedback"></a>Podpora a zpětná vazba

Azure Log Analytics trysek je otevřena z domácích zdrojů. Odesílat dotazy a zpětnou vazbu k [Githubu části](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues). Chcete-li otevřít žádost o podporu Azure, vyberte "Virtuální počítač spuštěn Foundry cloudu" jako kategorie služby. 

## <a name="next-step"></a>Další krok

Kromě metriky cloudu Foundry zahrnutých v tryska můžete agenta OMS a získáte přehled o provozních dat úroveň virtuálního počítače (například Syslog, výkon, výstrahy, inventáře). Jako Bosh rozšíření pro virtuální počítače CR je nainstalován OMS agent.

Podrobnosti najdete v tématu [agenta OMS nasadit na vaše nasazení cloudu Foundry](https://github.com/Azure/oms-agent-for-linux-boshrelease).
