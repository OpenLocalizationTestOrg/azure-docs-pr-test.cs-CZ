---
title: "aaaVMware řešení monitorování v Log Analytics | Microsoft Docs"
description: "Informace o jak hello řešení VMware monitorování vám mohou pomoci sledovat hostitelích ESXi a spravovat protokoly."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 16516639-cc1e-465c-a22f-022f3be297f1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.openlocfilehash: 959d5c2201fc5c7947f8b8870d055cdf9eea8e01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Řešení VMware monitorování (Preview) v analýzy protokolů

![VMware – symbol](./media/log-analytics-vmware/vmware-symbol.png)

Hello řešení VMware monitorování v analýzy protokolů je řešení, které vám pomůže vytvořit centralizované protokolování a monitorování přístup pro velké protokoly VMware. Tento článek popisuje, jak můžete řešení, zaznamenat a spravovat hostitele ESXi hello na jednom místě pomocí hello řešení. S řešením hello uvidíte podrobná data pro všechny hostitele ESXi na jednom místě. Se zobrazí počty nejvyšší událostí, stav a trendy hostitelů virtuálních počítačů a ESXi poskytované prostřednictvím protokolů hostitele ESXi hello. Řešení potíží s zobrazením a hledání centralizované protokoly hostitele ESXi. A můžete vytvářet výstrahy založené na protokolu vyhledávací dotazy.

řešení Hello používá nativní syslog funkce hello ESXi hostitele toopush data tooa cílový virtuální počítač, který má OMS Agent. Hello řešení však nepodporuje zapisovat soubory do protokolu syslog v rámci hello cílovém virtuálním počítači. Hello OMS agent otevře port 1514 a naslouchá toothis. Jakmile obdrží hello dat, hello OMS agent doručí hello data do OMS.

## <a name="installing-and-configuring-hello-solution"></a>Instalace a konfigurace řešení hello
Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

* Přidat hello monitorování VMware řešení tooyour pracovním prostorem OMS pomocí procesu hello popsané v [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Podporované hostitelé VMware ESXi
vSphere 5.5 hostitele ESXi a 6.0

#### <a name="prepare-a-linux-server"></a>Připravte Linux server
Vytvořte všechna data syslog tooreceive Linux operačního systému virtuálního počítače z hostitele ESXi hello. Hello [agenta OMS Linux](log-analytics-linux-agents.md) je hello bod kolekce pro všechna data syslog hostitele ESXi. Můžete vytvořit více ESXi hostitelů tooforward protokoly tooa jeden server Linux, stejně jako hello následující ukázka.  

   ![tok procesu Syslog](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Konfigurace sběru syslog
1. Nastavte syslog předávání VSphere. Podrobné informace toohelp nastavení předávání syslog, najdete v části [konfigurace syslog na ESXi 5.x a 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Přejděte příliš**konfigurace hostitele ESXi** > **softwaru** > **Upřesnit nastavení** > **Syslog**.
   ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  
2. V hello *Syslog.global.logHost* pole, přidejte vaše číslo pro port serveru a hello Linux *1514*. Například `tcp://hostname:1514` nebo`tcp://123.456.789.101:1514`
3. Otevřete bránu firewall hostitele ESXi hello pro syslog. **Konfigurace hostitele ESXi** > **softwaru** > **profil zabezpečení** > **brány Firewall** a otevřete  **Vlastnosti**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  
4. Zkontrolujte hello vSphere konzoly tooverify, který tento syslog je správně nastavený. Potvrďte na hello tento port hostitele ESXI **1514** je nakonfigurovaný.
5. Stáhněte a nainstalujte hello OMS agenta pro Linux na hello Linux server. Další informace najdete v tématu hello [dokumentace pro OMS agenta pro Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).
6. Po instalaci hello OMS agenta pro Linux přejděte toohello /etc/opt/microsoft/omsagent/sysconf/omsagent.d a kopírování hello vmware_esxi.conf soubor toohello /etc/opt/microsoft/omsagent/conf/omsagent.d adresář a hello skupiny patří hello změn a oprávnění souboru hello. Například:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
7. Restartujte hello OMS agenta pro Linux spuštěním `sudo /opt/microsoft/omsagent/bin/service_control restart`.
8. Testovací hello připojení mezi serverem Linux hello a hostitele ESXi hello pomocí hello `nc` příkazu na hostiteli ESXi hello. Například:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection too123.456.789.101 1514 port [tcp/*] succeeded!
    ```

9. V hello portálu OMS, provádět vyhledávání protokolu pro `Type=VMware_CL`. Když OMS shromažďuje hello syslog data, zachová formátu syslog hello. Hello portálu některých konkrétních polí zaznamenání, jako například *Hostname* a *název_procesu*.  

    ![type](./media/log-analytics-vmware/type.png)  

    Pokud výsledky hledání protokolu zobrazení jsou podobné toohello obrázku výše, nastavíte jste řídicího řešení panelu monitorování VMware OMS hello toouse.  

## <a name="vmware-data-collection-details"></a>Podrobnosti kolekce dat VMware
Hello řešení VMware monitorování shromažďuje různé metriky a protokolu údaje o výkonu z hostitele ESXi pomocí hello OMS agentů pro Linux, které jste povolili.

Hello následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují.

| Platforma | OMS agenta pro Linux | Agenta nástroje SCOM | Azure Storage | SCOM vyžaduje? | Data agenta SCOM odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |každé 3 minuty |

Hello následující tabulka obsahuje příklady datová pole shromažďují hello řešení VMware monitorování:

| Název pole | description |
| --- | --- |
| Device_s |Zařízení úložiště VMware |
| ESXIFailure_s |selhání typy |
| EventTime_t |čas výskytu události |
| HostName_s |Název hostitele ESXi |
| Operation_s |Vytvoření virtuálního počítače nebo odstranění virtuálního počítače |
| ProcessName_s |Název události |
| ResourceId_s |název hostitele VMware hello |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |Stav VMware SCSI |
| SyslogMessage_s |Syslog data |
| UserName_s |uživatel, který vytvořené nebo odstraněné virtuálních počítačů |
| VMName_s |název virtuálního počítače |
| Počítač |hostitelský počítač |
| TimeGenerated |čas hello data byla vygenerována. |
| DataCenter_s |Datové centrum VMware |
| StorageLatency_s |úložiště latence (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Přehled řešení VMware monitorování
Hello VMware dlaždice se objeví v hello portálu OMS. Poskytuje podrobný pohled případných selhání. Když kliknete na dlaždici hello, přejděte do zobrazení řídicího panelu.

![Dlaždice](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-hello-dashboard-view"></a>Přejděte hello zobrazení řídicího panelu
V hello **VMware** zobrazení řídicího panelu okna jsou uspořádané podle:

* Počet selhání stav
* Počty nejvyšší hostitele událostí
* Počty nejvyšší událostí
* Aktivity virtuálního počítače
* Události hostitele ESXi disku

![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Klikněte na libovolné okno tooopen podokna Hledat analýzy protokolů, které jsou uvedeny podrobné informace specifické pro okno hello.

Zde můžete upravit hello vyhledávání dotazu toomodify pro určitý objekt. Kurz týkající se základy hello OMS hledání, podívejte se na hello [kurzu vyhledávání protokolu OMS.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Najít události hostitele ESXi
Jednom hostiteli ESXi generuje více protokolů, na základě jejich procesů. Hello řešení VMware monitorování je centralizuje a shrnuje počty událostí hello. Toto zobrazení centralizované vám pomůže pochopit hostitele ESXi, v němž má k velkému počtu událostí a událostech, které se vyskytují nejčastěji ve vašem prostředí.

![Události](./media/log-analytics-vmware/events.png)

Můžete zobrazit další podrobnosti Další kliknutím na hostiteli ESXi nebo typ události.

Když kliknete na název hostitele ESXi, zobrazí se informace z tohoto hostitele ESXi. Pokud chcete výsledky toonarrow hello typu události, přidejte `“ProcessName_s=EVENT TYPE”` v dotazu hledání. Můžete vybrat **název_procesu** v hello vyhledávací filtr. Který zúží hello informace pro vás.

![Přejít k podrobnostem](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Najít vysoké aktivity virtuálních počítačů
Virtuální počítač můžete vytvořit a na každém hostiteli ESXi odstraněn. Je to užitečné pro správce tooidentify, kolik virtuálních počítačů hostiteli ESXi vytvoří. Který naopak, pomáhá toounderstand výkonu a plánování kapacity. Udržování přehledu o události aktivity virtuálního počítače je důležitá při správě prostředí.

![Přejít k podrobnostem](./media/log-analytics-vmware/vmactivities1.png)

Pokud chcete hostiteli ESXi další toosee data vytvoření virtuálního počítače, klikněte na název hostitele ESXi.

![Přejít k podrobnostem](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Běžné dotazy vyhledávání
Hello řešení zahrnuje další užitečné dotazy, které vám může pomoci spravovat hostitele ESXi, jako je prostor úložiště s vysokou latencí úložiště a selhání cestu.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Dotazy](./media/log-analytics-vmware/queries.png)


#### <a name="save-queries"></a>Uložení dotazů
Ukládání Vyhledávací dotazy je standardní funkce v OMS a vám pomůže uchovávat žádné dotazy, které jste najít užitečné. Jakmile vytvoříte dotaz, který pro vás užitečné, uložte kliknutím hello **Oblíbené**. Uložený dotaz umožňuje snadno jej znovu použít z hello [vlastní řídicí panel](log-analytics-dashboards.md) stránku, kde můžete vytvořit vlastní řídicí panely.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Vytvářet výstrahy z dotazů
Po vytvoření můžete své dotazy, můžete chtít toouse hello dotazy tooalert konkrétní události. V tématu [výstrahy v analýzy protokolů](log-analytics-alerts.md) informace o toocreate výstrahy. Příkladem výstrahy dotazy a další příklady dotazů, najdete v části hello [VMware monitorování pomocí analýzy protokolů OMS](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) příspěvku na blogu.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy
### <a name="what-do-i-need-toodo-on-hello-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Co potřebuji toodo na hostiteli ESXi hello nastavení? Jaký dopad bude mít na moje aktuální prostředí?
řešení Hello používá mechanismus hello nativní Syslog hostitele ESXi předávání. Nepotřebujete žádný další software společnosti Microsoft hello hostitele ESXi toocapture hello záznamy. Měl by mít stávajícího prostředí tooyour nízkou dopad. Však nutné předávání tooset syslog, což je funkce ESXI.

### <a name="do-i-need-toorestart-my-esxi-host"></a>Je nutné toorestart Moje hostitele ESXi?
Ne. Tento proces nevyžaduje restartování. V některých případech vSphere správně neaktualizuje hello syslog. V takovém případě protokolu na hostiteli ESXi toohello a znovu načtěte hello syslog. Nemáte znovu, toorestart hello hostitele, takže tento proces není rušivé tooyour prostředí.

### <a name="can-i-increase-or-decrease-hello-volume-of-log-data-sent-toooms"></a>Můžete zvýšit nebo snížit hello svazku protokolu data odeslaná tooOMS?
Ano, můžete. Hello úroveň protokolu hostitele ESXi nastavení můžete použít v vSphere. Protokol kolekce je založena na hello *informace* úroveň. Ano, pokud chcete tooaudit vytvoření virtuálního počítače nebo odstranění, je třeba tookeep hello *informace* úrovni na Hostd. Další informace najdete v tématu hello [VMware znalostní báze](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-toooms-my-log-setting-is-set-tooinfo"></a>Proč není Hostd poskytuje tooOMS dat? Moje protokolu je nastavení tooinfo.
Došlo chyb hostitele ESXi pro časové razítko hello syslog. Další informace najdete v tématu hello [VMware znalostní báze](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Po použití alternativní řešení hello Hostd by měla fungovat normálně.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-tooa-single-vm-with-omsagent"></a>Může mít několik ESXi hostitele předávání dat tooa syslog jeden virtuální počítač s omsagent?
Ano. Můžete mít více ESXi hostitele předávání tooa jeden virtuální počítač s omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Proč nevidím dat odesílaných do OMS?
Může být z několika důvodů:

* hostitele ESXi Hello není správně vkládání dat toohello virtuálních počítačů spuštěných omsagent. tootest, proveďte následující kroky hello:

  1. tooconfirm, přihlaste se na hostiteli ESXi toohello pomocí ssh a spusťte následující příkaz hello:`nc -z ipaddressofVM 1514`

      Pokud neproběhne úspěšně, nastavení vSphere v hello Upřesnit konfiguraci jsou pravděpodobně není opravit. V tématu [konfigurace sběru syslog](#configure-syslog-collection) informace o tom, jak tooset až hello ESXi hostitele pro předávání syslog.
  2. Pokud připojení port syslog je úspěšné, ale stále se nezobrazí žádná data, potom ho znovu načtěte hello syslog na hostiteli ESXi hello pomocí ssh toorun hello následující příkaz:` esxcli system syslog reload`
* Hello virtuálního počítače s agentem OMS není správně nastaven. tootest, proveďte následující kroky hello:

  1. OMS přijímá toohello port 1514 a zapisuje data do OMS. tooverify, že se jedná o otevřený, spusťte následující příkaz hello:`netstat -a | grep 1514`
  2. Měli byste vidět port `1514/tcp` otevřete. Pokud ho použít nechcete, ověřte, zda že tento omsagent hello je správně nainstalován. Pokud se informace o portu hello nezobrazí, pak hello syslog port není otevřen na hello virtuálních počítačů.

     1. Ověřte, že hello je spuštěn OMS Agent pomocí `ps -ef | grep oms`. Pokud není spuštěná, spusťte proces hello spuštěním příkazu hello` sudo /opt/microsoft/omsagent/bin/service_control start`
     2. Otevřete hello `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` souboru.

         Ověřte, zda hello správné uživatele a skupiny nastavení je platný, podobně jako:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

         Pokud soubor hello neexistuje nebo hello uživatele a skupiny nastavení je nesprávný, podniknout kroky podle [Příprava serveru Linux](#prepare-a-linux-server).

## <a name="next-steps"></a>Další kroky
* Použití [protokolu hledání](log-analytics-log-searches.md) v analýzy protokolů tooview podrobná data pro hostitele VMware.
* [Vytvořit vlastní řídicí panely](log-analytics-dashboards.md) zobrazující data hostitele VMware.
* [Vytvářet výstrahy](log-analytics-alerts.md) při výskytu určité události hostitele VMware.
