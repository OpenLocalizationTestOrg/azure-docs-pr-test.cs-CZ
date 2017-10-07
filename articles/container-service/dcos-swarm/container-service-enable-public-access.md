---
title: aplikace kontejneru aaaEnable access tooAzure DC/OS | Microsoft Docs
description: "Jak tooenable veřejný přístup ke kontejnerům tooDC nebo operačního systému v Azure Container Service."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 1ba251ba5a176a6a5e1c7831655164e380a62b27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-public-access-tooan-azure-container-service-application"></a>Povolit aplikaci Azure Container Service tooan veřejný přístup
Každý kontejner DC/OS v hello ACS [veřejného agenta fondu](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) je automaticky zveřejněné toohello Internetu. Ve výchozím nastavení, porty **80**, **443**, **8080** jsou otevřené, a jsou dostupné všechny (veřejné) kontejneru naslouchá na těchto portech. Tento článek ukazuje, jak tooopen více porty pro vaše aplikace v Azure Container Service.

## <a name="open-a-port-portal"></a>Otevřete port (portál)
Nejdřív potřebujeme tooopen hello port, který má být.

1. Přihlaste se toohello portálu.
2. Skupina prostředků hello najít, který jste nasadili hello Azure Container Service na.
3. Vyberte pro vyrovnávání zatížení agenta hello (která má název podobné příliš**XXXX--agent-lb XXXX**).
   
    ![Nástroj pro vyrovnávání zatížení Azure container service](./media/container-service-enable-public-access/agent-load-balancer.png)
4. Klikněte na tlačítko **sondy** a potom **přidat**.
   
    ![Nástroj pro vyrovnávání zatížení Azure container service sondy](./media/container-service-enable-public-access/add-probe.png)
5. Vyplňte formulář hello kontroly a klikněte na tlačítko **OK**.
   
   | Pole | Popis |
   | --- | --- |
   | Name (Název) |Popisný název hello kontroly. |
   | Port |port Hello tootest kontejneru hello. |
   | Cesta |(Pokud v režimu HTTP) hello tooprobe cesta relativní webu. Protokol HTTPS není podporována. |
   | Interval |Hello mezi testu pokusů, v sekundách. |
   | Prahová hodnota špatného stavu |Počet po sobě jdoucích testu pokusy o zadání před zvažování hello kontejneru není v pořádku. |
6. Zpět na hello vlastnosti služby vyrovnání zatížení hello agenta, klikněte na tlačítko **pravidla Vyrovnávání zatížení** a potom **přidat**.
   
    ![Pravidla nástroje pro vyrovnávání zatížení Azure container service](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Vyplňte formulář pro vyrovnávání zatížení hello a klikněte na tlačítko **OK**.
   
   | Pole | Popis |
   | --- | --- |
   | Name (Název) |Popisný název služby Vyrovnávání zatížení hello. |
   | Port |veřejný port příchozí Hello. |
   | Back-endový port |Hello interní veřejný port provozu tooroute kontejneru hello. |
   | Fond back-end |Hello kontejnery v tomto fondu budou hello cíl pro tuto službu Vyrovnávání zatížení. |
   | Test |Pokud cíl v hello Hello kontroly používané toodetermine **fond back-end** je v pořádku. |
   | Trvalost relace |Určuje způsob zpracování přenosů od klienta pro hello trvání relace hello.<br><br>**Žádný**: po sobě jdoucí požadavky ze stejného klienta může zpracovávat všechny kontejneru hello.<br>**Klient IP**: po sobě jdoucí požadavky ze hello stejnou IP adresu klienta, jsou zpracovávány hello stejnému kontejneru.<br>**Klient IP a protokol**: po sobě jdoucí požadavky ze stejné kombinaci IP adresy a protokol klienta jsou zpracovávány hello hello stejnému kontejneru. |
   | Časový limit nečinnosti |(Pouze TCP) V minutách, hello tookeep čas klienta TCP nebo HTTP otevřete bez spoléhání na *udržování* zprávy. |

## <a name="add-a-security-rule-portal"></a>Přidat pravidlo zabezpečení (portál)
V dalším kroku potřebujeme tooadd pravidlo zabezpečení, který směruje provoz z našich otevřen port přes bránu firewall hello.

1. Přihlaste se toohello portálu.
2. Skupina prostředků hello najít, který jste nasadili hello Azure Container Service na.
3. Vyberte hello **veřejné** agenta skupinu zabezpečení sítě (která má název podobné příliš**XXXX-agent veřejný nsg-XXXX**).
   
    ![Skupina zabezpečení sítě Azure container service](./media/container-service-enable-public-access/agent-nsg.png)
4. Vyberte **příchozí pravidla zabezpečení** a potom **přidat**.
   
    ![Pravidla skupiny zabezpečení sítě Azure container service](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Vyplňte tooallow pravidlo brány firewall hello veřejný port a klikněte na tlačítko **OK**.
   
   | Pole | Popis |
   | --- | --- |
   | Name (Název) |Popisný název pravidla brány firewall hello. |
   | Priorita |Pořadí priority pro pravidlo hello. Hello nižší hello číslo hello vyšší hello prioritou. |
   | Zdroj |Omezte hello příchozí IP adresa rozsahu toobe povolené nebo zakázané tímto pravidlem. Použití **žádné** toonot zadejte omezení. |
   | Služba |Vyberte sadu předdefinovaných služeb, ke kterému se toto pravidlo zabezpečení. Jinak použijte **vlastní** toocreate vlastní. |
   | Protocol (Protokol) |Omezit přenos na základě **TCP** nebo **UDP**. Použití **žádné** toonot zadejte omezení. |
   | Rozsah portů |Když **služby** je **vlastní**, určuje hello rozsah portů, která toto pravidlo vztahuje. Můžete používat jediný port, jako třeba **80**, nebo jako rozsah **1024-1 500**. |
   | Akce |Povolí nebo zakážou provoz, která splňuje kritéria hello. |

## <a name="next-steps"></a>Další kroky
Další informace o hello rozdíl mezi [veřejné a privátní agentů DC/OS](container-service-dcos-agents.md).

Přečtěte si další informace o [Správa kontejnerů Váš DC/OS](container-service-mesos-marathon-ui.md).

