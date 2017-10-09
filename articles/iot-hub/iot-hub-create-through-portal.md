---
title: "aaaUse hello Azure portálu toocreate služby IoT Hub | Microsoft Docs"
description: "Toocreate, jak spravovat a odstranit Azure IoT hubs prostřednictvím hello portálu Azure. Obsahuje informace o cenových úrovní, škálování, zabezpečení a zasílání zpráv konfigurace."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: 383968c90ee7ef3bff85a6c90efbf5f0e8fbb208
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-portal"></a>Vytvoření služby IoT hub pomocí hello portálu Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Tento článek popisuje:

* Jak toofind hello služby IoT Hub v portálu Azure hello.
* Jak toocreate a spravovat centra IoT.

## <a name="where-toofind-hello-iot-hub-service"></a>Kde toofind hello služby IoT Hub

Hello služby IoT Hub naleznete v následujících umístění portálu hello hello:

* Zvolte **+ nový**, zvolte **Internet věcí**.
* V hello Marketplace, zvolte **Internet věcí**.

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

Můžete vytvořit služby IoT hub pomocí hello následující metody:

* Hello **+ nový** možnost otevře okno hello ukazuje následující snímek obrazovky hello. Hello kroky pro vytvoření služby IoT hub hello prostřednictvím této metody a prostřednictvím hello marketplace jsou identické.
* V hello Marketplace, zvolte **vytvořit** okno hello tooopen ukazuje následující snímek obrazovky hello.

Hello následující části popisují hello několik kroků toocreate služby IoT hub:

### <a name="choose-hello-name-of-hello-iot-hub"></a>Zvolte název hello hello centra IoT

toocreate služby IoT hub, musí název hello IoT hub. Tento název musí být jedinečný mezi všechny služby IoT hubs.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-hello-pricing-tier"></a>Vyberte cenovou úroveň hello

Můžete si vybrat z čtyři úrovně: **volné**, **standardní 1** a **standardní 2**, a **úrovně Standard S3**. úroveň free Hello umožňuje pouze 500 toobe zařízení připojené toohello IoT hub a až too8 000 zpráv za den.

**Standard S1**: použití hello S1 edition pro počítače s řešeními IoT s velký počet zařízení, že každý generovat malé množství dat. Jednotlivé jednotky hello S1 edition umožňuje až too400 000 zpráv za den ve všech připojených zařízeních.

**Standard S2**: použití hello S2 edition pro počítače s řešení IoT, ve kterých zařízení generovat velké objemy dat. Jednotlivé jednotky hello S2 edition umožňuje až too6 mil. zpráv za den mezi všech připojených zařízeních.

**Úrovně Standard S3**: použití hello S3 edition pro počítače s řešení IoT, které generují velké objemy dat. Jednotlivé jednotky hello S3 edition umožňuje až too300 mil. zpráv za den mezi všech připojených zařízeních.

![][4]

> [!NOTE]
> IoT Hub umožňuje jenom jednu bezplatnou rozbočovače za předplatné Azure.

### <a name="iot-hub-units"></a>Jednotky centra IoT hub

Hello počet zpráv povolených na jednotku za den závisí na vaše Centrum cenovou úroveň. Například pokud chcete hello IoT hub toosupport příchozího 700 000 zpráv, zvolte dvě jednotky vrstvy S1.

### <a name="device-toocloud-partitions-and-resource-group"></a>Oddíly toocloud zařízení a skupině prostředků

Můžete změnit hello počet oddílů pro Centrum IoT. Hello výchozí počet oddílů je 4, můžete vybrat jiné číslo z rozevíracího seznamu hello.

Není nutné tooexplicitly vytvořte skupinu prostředků prázdný. Když vytvoříte prostředek, můžete zvolit buď toocreate a nové nebo použít existující skupinu prostředků.

![][5]

### <a name="choose-subscription"></a>Zvolte předplatné

Azure IoT Hub automaticky seznamy hello předplatná Azure hello uživatelský účet je přiřazen. Můžete použít Centrum IoT hello předplatného Azure tooassociate hello k.

### <a name="choose-hello-location"></a>Vyberte umístění hello

možnosti umístění Hello poskytuje seznam hello oblastí, kde je k dispozici služby IoT Hub.

### <a name="create-hello-iot-hub"></a>Vytvoření centra IoT hello

Po dokončení všech předchozích kroků můžete vytvořit hello IoT hub. Klikněte na tlačítko **vytvořit** toostart hello toocreate proces back-end a nasadit hello IoT hub pro hello možnosti, které jste zvolili.

Může trvat několik minut toocreate hello IoT hub jako trvá dobu hello back-end nasazení toorun na serverech příslušné umístění hello.

## <a name="change-hello-settings-of-hello-iot-hub"></a>Změňte nastavení hello hello IoT hub

Po vytvoření z hello okno centra IoT, můžete změnit nastavení hello stávající služby IoT hub.

![][8]

**Sdílené zásady přístupu**: tyto zásady určují hello oprávnění pro zařízení a služeb tooconnect tooIoT rozbočovače. Tyto zásady můžete přístupu kliknutím **zásady sdíleného přístupu** pod **Obecné**. V tomto okně můžete upravit existující zásady nebo přidání nové zásady.

### <a name="create-a-policy"></a>Vytvořit zásadu

* Klikněte na tlačítko **přidat** tooopen okno. Zadejte název nové zásady hello a hello oprávnění, která má tooassociate s těmito zásadami, jak je znázorněno v následující hello obrázek:

    Jsou k dispozici několik oprávnění, které může být spojeno s tyto sdílené zásady. Hello **registru číst** a **registru zápisu** zásady udělit ke čtení a registru identit toohello práva k zápisu. Výběrem možnosti zápisu hello automaticky zvolí možnost čtení hello.

    Hello **služba připojit** zásad uděluje oprávnění tooaccess koncové body služby, jako **přijímat zařízení cloud**. Hello **zařízení připojit** zásad uděluje oprávnění pro odesílání a přijímání zpráv pomocí koncových bodů hello straně zařízení IoT Hub.

* Klikněte na tlačítko **vytvořit** tooadd tomto nově vytvořený zásad toohello existujícího seznamu.

![][10]

## <a name="endpoints"></a>Koncové body

Klikněte na tlačítko **koncové body** toodisplay seznam koncových bodů pro hello IoT hub, který chcete upravit. Existují dva typy koncových bodů: koncové body, které jsou součástí hello IoT hub a koncové body přidejte toohello IoT hub po jeho vytvoření.

![][11]

### <a name="built-in-endpoints"></a>Předdefinované koncové body

Existují dva předdefinované koncové body: **cloudu zpětné vazby toodevice** a **události**.

* **Cloud zpětné vazby toodevice** nastavení: Toto nastavení má dva subsettings: **cloudu tooDevice TTL** (time-to-live) a **dobu uchování** (v hodinách) pro zprávy hello. Při první vytvoření služby IoT hub, obě tato nastavení mají hello výchozí hodnotu 1 hodina. tooadjust tato nastavení použít posuvníky hello nebo zadat hodnoty hello.
* **Události** nastavení: Toto nastavení má několik subsettings, z nichž některé jsou jen pro čtení. Hello následující seznam popisuje tato nastavení:

  * **Oddíly**: výchozí hodnota je nastavena, když je vytvořen hello IoT hub. Hello počet oddílů pomocí tohoto nastavení můžete změnit.

  * **Název kompatibilní s centrem událostí a koncového bodu**: Po vytvoření služby IoT hub hello centra událostí se může potřebovat přistupovat toounder určitých okolností interně vytvoří. Nelze přizpůsobit název a koncový bod hodnoty hello kompatibilní s centrem událostí, ale můžete je zkopírovat kliknutím **kopie**.

  * **Doba uchování**: ve výchozím nastavení tooone den, ale můžete změnit pomocí rozevíracího seznamu hello. Tato hodnota je ve dnech pro nastavení zařízení cloud hello.

  * **Skupiny příjemců**: skupiny příjemců povolit více zpráv tooread čtečky nezávisle hello IoT hub. Každé centrum IoT je vytvořen s výchozí skupina příjemců. Můžete ale přidat nebo odstranit příjemce skupiny tooyour IoT hubs pomocí tohoto nastavení.

  > [!NOTE]
  > Výchozí skupina příjemců Hello nelze upravit ani odstranit.

### <a name="custom-endpoints"></a>Vlastní koncové body

Ve službě IoT hub pomocí portálu hello můžete přidat vlastní koncové body. Z hello **koncové body** okně klikněte na tlačítko **přidat** v hello nejvyšší tooopen hello **přidání koncového bodu** okno. Zadejte hello požadované informace a potom klikněte na tlačítko **OK**. Svůj vlastní koncový bod je nyní obsažena v hello hlavní **koncové body** okno.

![][13]

Další informace o vlastní koncové body v [odkaz – koncové body centra IoT][lnk-devguide-endpoints].

## <a name="routes"></a>Trasy

Klikněte na tlačítko **trasy** toomanage jak IoT Hub odešle zprávu vaše zprávy typu zařízení cloud.

![][14]

Kliknutím můžete přidat trasy tooyour IoT hub **přidat** hello horní části hello **trasy*** okně zadáním hello požadované informace a kliknutím na tlačítko **OK**. Vaše trasy, pak je uvedena ve hello hlavní **trasy** okno. Trasa můžete upravit kliknutím na hello seznam tras. tooenable trasu, klikněte na něj v seznamu hello tras a nastavit hello **povoleno** přepnutí příliš**vypnout**. Změna hello toosave, klikněte na tlačítko **OK** v hello dolní části okna hello.

![][15]

## <a name="pricing-and-scale"></a>Ceny a škálování

Hello ceny z stávající služby IoT hub lze změnit pomocí hello **cenová** nastavení s hello následující výjimky:

* V aktuální implementace hello, nelze změnit centrum IoT se volné SKU tooone vrstev z hello placené SKU, nebo naopak.
* Může existovat pouze jedna úroveň free IoT hub v hello předplatného Azure.

![][12]

Můžete přesunout z vyšší úroveň toolower jenom v případě, že hello počet zpráv odeslaných daný den překročení hello kvóty pro nižší úroveň hello. Například pokud hello počet zpráv za den překračuje 400,000, pak hello vrstva pro hello IoT hub lze změnit. Ale pokud změníte úroveň S1 toohello pak hello IoT hub je omezen pro daný den.

## <a name="delete-hello-iot-hub"></a>Odstranit centrum IoT hello

Můžete procházet toohello IoT hub chcete toodelete kliknutím **Procházet**, a pak vyberete hello toodelete odpovídající rozbočovače. toodelete hello služby IoT hub, klikněte na tlačítko hello **odstranit** tlačítko pod hello název centra IoT.

## <a name="next-steps"></a>Další kroky

Použijte tyto odkazy toolearn informace o správě Azure IoT Hub:

* [Hromadné spravovat zařízení IoT][lnk-bulk]
* [Metriky služby IoT Hub][lnk-metrics]
* [Monitorování operací][lnk-monitor]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]
* [Simulaci zařízení s hranou IoT][lnk-iotedge]
* [Zabezpečení řešení IoT z hello pozadí][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
