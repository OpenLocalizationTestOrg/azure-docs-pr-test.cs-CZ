---
title: verze aaaCanary s Vamp na clusteru Azure DC/OS | Microsoft Docs
description: "Jak toouse Vamp toocanary verze služby a použít filtrování v clusteru Azure Container Service DC/OS inteligentní provozu."
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: e7b8658a161a7cddcf718e3e1c12a889a330d3d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>Lesknice verze mikroslužeb s Vamp v clusteru Azure Container Service DC/OS

V tomto návodu budeme nastavit Vamp v Azure Container Service se cluster DC/OS. Jsme lesknice verze hello Vamp ukázku služby "Sávy" a potom vyřešte nekompatibility hello služby s Firefox použitím filtrování inteligentního přenosu. 

> [!TIP] 
> V tomto návodu Vamp běží na clusteru DC/OS, ale můžete také použít Vamp s Kubernetes jako hello orchestrator.
>

## <a name="about-canary-releases-and-vamp"></a>O Kanárských uvolní a Vamp


[Lesknice uvolnění](https://martinfowler.com/bliki/CanaryRelease.html) je přijat inovativní organizacím, jako Netflix, Facebook či Spotify strategie inteligentní nasazení. Je postup, který dává smysl, protože snižuje problémy, představuje bezpečnostní sítě a zvyšuje inovace. Proto proč všech společností, které nepoužívají ho? Rozšíření kanálu tooinclude CI/CD lesknice strategie přidá složitost a vyžaduje devops rozsáhlé znalosti a zkušenosti. Předtím, než se i Začínáme je dostatek tooblock menší firem a podniků agentem. 

[Vamp](http://vamp.io/) je tooease systém open source určené tento přechod a převeďte lesknice uvolnit scheduler kontejneru tooyour upřednostňovaný funkce. Funkce lesknice na vamp překročí pohledu na základě procenta. Provoz můžete filtrovat a rozdělit na širokou škálu podmínky, například tootarget konkrétní uživatele, rozsahy IP adres nebo zařízení. Vamp sleduje a analyzuje metrik výkonu, povolení pro automatizaci na základě reálného dat. Můžete nastavit automatické vrácení zpět na chyby, nebo škálování variant jednotlivých služeb na základě zatížení nebo latence.

## <a name="set-up-azure-container-service-with-dcos"></a>Nastavení Azure Container Service s DC/OS



1. [Nasadit cluster DC/OS](container-service-deployment.md) se jeden z nich a dva agenti výchozí velikost. 

2. [Vytvoření tunelu SSH](../container-service-connect.md) clusteru DC/OS toohello tooconnect. Tento článek předpokládá, vytvoření tunelu toohello clusteru na místního portu 80.


## <a name="set-up-vamp"></a>Nastavit Vamp

Teď, když máte spuštěného clusteru DC/OS, můžete nainstalovat Vamp z hello uživatelského rozhraní DC/OS (http://localhost:80). 

![Uživatelské rozhraní DC/OS](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Instalace probíhá ve dvou fázích:

1. **Nasazení Elasticsearch**.

2. Potom **nasazení Vamp** nainstalováním hello Vamp DC/OS universe balíčku.

### <a name="deploy-elasticsearch"></a>Nasazení Elasticsearch

Vamp vyžaduje Elasticsearch metriky shromažďování a agregace. Můžete použít hello [imagí Dockeru magneticio](https://hub.docker.com/r/magneticio/elastic/) toodeploy kompatibilní Vamp Elasticsearch zásobníku.

1. Hello uživatelského rozhraní DC/OS, přejděte v příliš**služby** a klikněte na tlačítko **nasadit službu**.

2. Vyberte **režim JSON** z hello **nasazení nové služby** automaticky otevírané okno.

  ![Vyberte režim JSON](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Vložte následující JSON hello. Tato konfigurace používá hello kontejner s 1 GB paměti RAM a kontrola základní stavu na portu Elasticsearch hello.
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. Klikněte na tlačítko **nasazení**.

  DC/OS nasadí kontejner Elasticsearch hello. Průběh můžete sledovat na hello **služby** stránky.  

  ![nasazení e? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Nasazení Vamp

Jakmile Elasticsearch sestavy jako **systémem**, můžete přidat balíček hello Vamp Universe DC/OS. 

1. Přejděte příliš**Universe** a vyhledejte **vamp**. 
  ![Vamp na DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Klikněte na tlačítko **nainstalovat** další toohello vamp balíčku a vyberte **rozšířený instalace**.

3. Posuňte se dolů a zadejte následující elasticsearch – adresa url hello: `http://elasticsearch.marathon.mesos:9200`. 

  ![Zadejte adresu URL Elasticsearch](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Klikněte na tlačítko **zkontrolovat a nainstalovat**, pak klikněte na tlačítko **nainstalovat** toostart hello nasazení.  

  DC/OS nasadí všechny požadované součásti Vamp. Průběh můžete sledovat na hello **služby** stránky.
  
  ![Nasazení Vamp jako balíček universe](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. Po dokončení nasazení se můžete dostat hello Vamp uživatelského rozhraní:

  ![Služba vamp na DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp uživatelského rozhraní](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>Nasazení vaší první službě

Nyní tento Vamp je spuštěná, nasazení služby z plán, podle kterého. 

Ve své nejjednodušší podobě [Vamp plán, podle kterého](http://vamp.io/documentation/using-vamp/blueprints/) popisuje hello koncových bodů (brány), clustery a toodeploy služby. Vamp používá clustery toogroup různé varianty hello stejné služby do logických skupin pro lesknice uvolněním nebo A / B testování.  

Tento scénář používá ukázkovou aplikaci monolitický názvem [ **Sávy**](https://github.com/magneticio/sava), který je ve verzi 1.0. monolitu Hello je součástí Docker kontejneru, který je v úložiště Docker Hub v rámci magneticio/sava:1.0.0. aplikace Hello za normálních okolností běží na portu 8080, ale chcete ho v tomto případě portu 9050 tooexpose. Nasazení aplikace hello prostřednictvím Vamp používá jednoduchý plán, podle kterého.

1. Přejděte příliš**nasazení**.

2. Klikněte na tlačítko **Přidat**.

3. Vložte následující hello plán, podle kterého YAML. Tento plán, podle kterého obsahuje jeden cluster s variant pouze jedna služba, která nám změnit později:

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster toocreate
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. Klikněte na **Uložit**. Vamp zahájí hello nasazení.

nasazení Hello je uvedená na hello **nasazení** stránky. Klikněte na tlačítko hello nasazení toomonitor jeho stav.

![Uživatelské rozhraní – nasazení Sávy vamp](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![Služba Sávy v Vamp uživatelského rozhraní](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

Vytváří dvě brány, které jsou uvedeny na hello **brány** stránky:

* Dobrý den tooaccess stabilní koncový bod službou (port 9050) 
* spravované Vamp interní brány (Další informace o této brány se později). 

![Vamp uživatelské rozhraní – Sávy brány](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

službu Sávy Hello má nyní nasadit, ale protože tooforward provoz tooit není známo ještě hello nástroj pro vyrovnávání zatížení Azure ho nelze přístup externě. tooaccess hello služba hello aktualizace konfigurace sítí Azure.


## <a name="update-hello-azure-network-configuration"></a>Aktualizace konfigurace hello síť Azure

Služba Sávy hello vamp nasazené na uzlech agenta DC/OS hello, vystavení stabilní koncový bod na port 9050. tooaccess hello služby z clusteru DC/OS mimo hello, proveďte následující změny konfigurace Azure sítě toohello ve vašem nasazení clusteru hello: 

1. **Konfigurace hello nástroj pro vyrovnávání zatížení Azure** pro agenty hello (hello prostředek s názvem **orchestrátoru DC/OS agenta lb xxxx**) s test stavu a přenosem tooforward pravidlo na portu 9050 toohello Sávy instancí. 

2. **Skupina zabezpečení sítě hello aktualizace** pro veřejné agenty hello (hello prostředek s názvem **XXXX-agent veřejný nsg-XXXX**) tooallow přenosy na portu 9050.

Pro podrobné kroky toocomplete hello tyto úlohy pomocí portálu Azure najdete [povolit aplikaci Azure Container Service tooan veřejný přístup](container-service-enable-public-access.md). Zadejte port 9050 pro všechna nastavení portu.


Po všechno, co byla vytvořena, přejděte toohello **přehled** okno služby Vyrovnávání zatížení agenta DC/OS hello (hello prostředek s názvem **orchestrátoru DC/OS agenta lb xxxx**). Najde hello **veřejnou IP adresu**a použití hello adresu tooaccess Sávy na portu 9050.

![Portál Azure – get veřejnou IP adresu](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![Sávy](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Spustit lesknice verze

Předpokládejme, že máte novou verzi této aplikace, které chcete toocanary verze do produkčního prostředí. Ji kontejnerizované jako magneticio/sava:1.1.0 a jsou připravené toogo. Vamp umožňuje snadno přidat nové služby toohello běží nasazení. Tyto služby "sloučené" jsou nasadit souběžně s hello stávající služby v clusteru hello a přiřadili váhu % 0. Žádný provoz je služba směrované tooa nově sloučit, dokud upravit distribuce přenosů hello. jezdec váhy Hello v hello Vamp uživatelského rozhraní poskytuje úplnou kontrolu nad hello distribuce, aby vám umožnil přírůstkové úpravy (lesknice vydání) nebo rychlých vrácení zpět.

### <a name="merge-a-new-service-variant"></a>Sloučení nové služby variant

toomerge hello nové Sávy 1.1 služby s hello běží nasazení:

1. V hello Vamp uživatelského rozhraní, klikněte na **plány vyfotografovat**.

2. Klikněte na tlačítko **přidat** a vložte následující hello plán, podle kterého YAML: Tento plán, podle kterého popisuje nové toodeploy typu variant (Sávy: 1.1.0) služby v rámci stávajícího clusteru hello (sava_cluster).

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster tooupdate
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint tooupdate
  ```
  
3. Klikněte na **Uložit**. Hello plán, podle kterého je uložena a uvedené na hello **plány vyfotografovat** stránky.

4. Nabídka Akce otevřete hello na plán, podle kterého Sávy: 1.1 hello a klikněte na **sloučí se**.

  ![Vamp uživatelské rozhraní – plány](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Vyberte hello **Sávy** nasazení a klikněte na tlačítko **sloučení**.

  ![Vamp uživatelského rozhraní – sloučení plán, podle kterého toodeployment](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp nasadí hello nové Sávy: 1.1.0 služby typu variant popsané v hello plán, podle kterého spolu s Sávy: 1.0.0 v hello **sava_cluster** z hello běží nasazení. 

![Vamp uživatelské rozhraní – aktualizované Sávy nasazení](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

Hello **Sávy/sava_cluster/webport** brány (koncový bod clusteru hello) je rovněž aktualizováno, přidání trasy toohello nově nasazené Sávy: 1.1.0. V tomto okamžiku žádný provoz se směruje zde (hello **VÁHY** nastavena too0 %).

![Vamp uživatelské rozhraní – clusteru brány](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Lesknice verze

V obou verzích Sávy nasazené v hello stejný cluster, upravte hello distribuce přenosů mezi nimi přesunutím hello **VÁHY** posuvníku.

1. Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) další příliš**VÁHY**.

2. Nastavit too50%/50% distribuce váhy hello a klikněte na tlačítko **Uložit**.

  ![Vamp uživatelské rozhraní – brána váhy posuvníku](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Přejděte zpět tooyour prohlížeče a aktualizujte stránku hello Sávy několik vícekrát. Hello Sávy nyní aplikaci přepíná mezi Sávy: 1.0 stránky a stránky Sávy: 1.1.

  ![střídání sava1.0 a sava1.1 služby](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Tato alternace stránku hello nejvhodnější hello "Incognito" nebo "Anonymní" režim prohlížeče z důvodu hello ukládání do mezipaměti statické prostředky.
  >

### <a name="filter-traffic"></a>Filtrování provozu

Po nasazení Předpokládejme, že jste zjišťuje nekompatibilitu v Sávy: 1.1.0, že příčiny zobrazit problémy v prohlížečích Firefox. Můžete nastavit Vamp toofilter příchozí provoz a přímé, že všichni uživatelé Firefox zpět toohello známé stabilní Sávy: 1.0.0. Tento filtr okamžitě vyřeší hello přerušení pro uživatele, Firefox, zatímco ostatní uživatelé stále tooenjoy hello výhod hello vylepšené Sávy: 1.1.0.

Vamp používá **podmínky** toofilter provoz mezi trasy v bránu. Provoz je nejdřív filtrovaný a přesměruje podle toohello podmínky použijí tooeach trasy. Veškerý zbývající provoz je distribuován podle nastavení váhy toohello brány.

Můžete vytvořit podmínku toofilter všichni uživatelé Firefox a směrovat je starý Sávy: 1.0.0 toohello:

1. Na hello Sávy/sava_cluster/webport **brány** klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd **PODMÍNKU** sava/sava_cluster/sava:1.0.0/webport toohello trasy. 

2. Zadejte podmínky hello **uživatelský agent == Firefox** a klikněte na tlačítko ![Vamp uživatelské rozhraní – Uložit](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

  Vamp přidá hello podmínky se výchozí sílu % 0. filtrování provozu toostart, musíte tooadjust hello podmínku sílu.

3. Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **SÍLU** použít toohello podmínku.
 
4. Sada hello **SÍLU** too100 % a klikněte na tlačítko ![Vamp uživatelské rozhraní – Uložit](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.

  Vamp teď odešle veškerý provoz odpovídající toosava:1.0.0 hello podmínku (všichni uživatelé Firefox).

  ![Vamp uživatelského rozhraní – použít podmínku toogateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Nakonec upravte hello brány váhy toosend všechny zbývající provoz (všechny uživatele bez Firefox) toohello nové Sávy: 1.1.0. Klikněte na tlačítko ![Vamp uživatelské rozhraní – upravit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) další příliš**VÁHY** a nastavte distribuce váhy hello tak, aby 100 % sava/sava_cluster/sava:1.1.0/webport směrovanou toohello trasy.

  Veškerý provoz nejsou filtrovány podle hello podmínka je nyní směrovanou toohello nové Sávy: 1.1.0.

6. Filtr hello toosee v akci, otevřete dva různé prohlížeče (jeden Firefox a jeden jiný prohlížeč) a přístup k službě Sávy hello z obou. Všechny požadavky Firefox odešlou toosava:1.0.0, zatímco všech dalších prohlížečích jsou řízené toosava:1.1.0.

  ![Vamp uživatelské rozhraní – filtrovat provoz](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Shrnutí

Tento článek byl rychlý úvod tooVamp na cluster DC/OS. Pro začátek jste získali Vamp a spuštěná na vaše Azure Container Service DC/OS clusteru, nasazení služby s Vamp plán, podle kterého a přístup na adrese koncový bod vystavený hello (brány).

Můžeme také dotýkal na některé výkonné funkce Vamp: sloučení nové služby variant toohello běží nasazení a jeho zavedení postupně, a filtrování provozu tooresolve známé nekompatibilita.


## <a name="next-steps"></a>Další kroky

* Další informace o správě Vamp akce prostřednictvím hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).

* Skripty pro automatizaci Vamp v Node.js sestavit a spustit je jako [Vamp pracovních](http://vamp.io/documentation/tutorials/create-a-workflow/).

* Další informace najdete v [VAMP kurzy](http://vamp.io/documentation/tutorials/overview/).

