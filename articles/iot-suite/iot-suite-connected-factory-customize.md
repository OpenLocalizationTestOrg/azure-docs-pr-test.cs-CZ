---
title: "objekt pro vytváření připojení aaaCustomize Azure IoT Suite | Microsoft Docs"
description: "Popis jak toocustomize hello chování hello připojen objekt pro vytváření předkonfigurovaného řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 53f2fef7a76b5d8e6ad023945a7812dc7fabd12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="customize-how-hello-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Přizpůsobit způsob hello připojen objekt pro vytváření řešení zobrazí data z vašich serverů OPC UA

## <a name="introduction"></a>Úvod

Hello připojené vytváření řešení agreguje a zobrazí data z hello OPC UA servery připojené toohello řešení. Můžete procházet a odesílat příkazy toohello OPC UA servery ve vašem řešení. Další informace o OPC UA najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).

Agregovaná data v řešení hello příkladem hello efektivitu celkové zařízení (OEE) a klíčových ukazatelů výkonu (KPI), můžete zobrazit v řídicím panelu hello na úrovních stanice, řádku a objektu pro vytváření hello. Hello následující snímek obrazovky ukazuje hello hodnoty OEE a klíčový ukazatel výkonu pro hello **sestavení** na stanici **produkční řádku 1**, v hello **Mnichov** factory:

![Příklad hodnoty OEE a klíčových ukazatelů výkonu v řešení hello][img-oee-kpi]

umožňuje řešení Hello tooview vám podrobné informace z konkrétní datové položky z hello OPC UA servery, nazývané *stanice*. Hello následující snímek obrazovky ukazuje pozemků hello počet vyrobenou položek z konkrétní stanice:

![Pozemků počet vyrobenou položek][img-manufactured-items]

Pokud kliknete na jednu z hello grafy, můžete prozkoumat hello dat další pomocí časové řady přehledy (TSI):

![Prozkoumejte data pomocí statistiky časové řady][img-tsi]

Tento článek popisuje:

- Jak hello dat se provádí k dispozici toohello různých zobrazení v řešení hello.
- Jak můžete přizpůsobit hello způsob hello řešení zobrazí hello data.

## <a name="data-sources"></a>Zdroje dat

Hello připojené vytváření řešení zobrazí data z hello OPC UA servery připojené toohello řešení. Hello výchozí instalace zahrnuje několik OPC UA servery se systémem simulace objekt pro vytváření. Můžete přidat vlastní servery OPC UA, [připojení přes bránu] [ lnk-connect-cf] tooyour řešení.

Můžete procházet, připojený server OPC UA může odesílat tooyour řešení na řídicím panelu hello hello datové položky:

1. Přejděte toohello **vyberte server OPC UA** zobrazení:

    ![Přejděte toohello vyberte zobrazení serverů OPC UA][img-select-server]

1. Vyberte server a klikněte na tlačítko **Connect**. Klikněte na tlačítko **pokračovat** Jakmile se zobrazí upozornění zabezpečení hello.

    > [!NOTE]
    > Toto upozornění pouze vyskytuje jednou pro každý server a vytvoří vztah důvěryhodnosti mezi řídicí panel řešení hello a hello server.

1. Můžete nyní procházet hello datových položek, které hello server může odeslat toohello řešení. Položky, které jsou odesílány toohello řešení mají zelená značka zaškrtnutí:

    ![Publikované položky][img-published]

1. Pokud jste *správce* v hello řešení, můžete zvolit toopublish toomake položka dat je k dispozici v hello připojen objekt pro vytváření řešení. Jako správce můžete také změnit hodnotu hello datové položky a volání metody v hello OPC UA serveru.

## <a name="map-hello-data"></a>Mapování dat hello

Hello připojený objekt pro vytváření řešení mapy a agreguje hello položek publikovaných dat z hello OPC UA serveru toohello různých zobrazení v řešení hello. Hello připojené vytváření řešení nasadí tooyour účet Azure při zřizování řešení hello. Soubor JSON v hello Visual Studio připojené objekt pro vytváření řešení ukládá tyto informace o mapování. Můžete zobrazit a upravit tento konfigurační soubor JSON v hello připojené vytváření řešení sady Visual Studio. Můžete znovu nasadit řešení hello po provedení změny.

Můžete použít soubor konfigurace hello k:

- Upravte existující simulované objekty Factory hello, řádky výroby a stanice.
- Mapování dat z reálného OPC UA serverů připojení toohello řešení.

tooclone kopii hello připojen objekt pro vytváření řešení sady Visual Studio, hello použijte následující příkaz git:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

soubor Hello **ContosoTopologyDescription.json** definuje hello položky toohello zobrazení v řídicím panelu řešení připojených factory hello mapování z hello OPC UA dat serveru. Tento konfigurační soubor můžete najít v hello **Contoso\Topology** složky v hello **WebApp** projekt v hello řešení sady Visual Studio.

Hello obsah souboru JSON hello jsou uspořádána jako hierarchie objekt pro vytváření, řádek výrobní a uzly stanice. Tato hierarchie definuje hello navigační hierarchii hello připojené factory řídicím panelu. Hodnoty v každém uzlu hierarchie hello určují hello informace zobrazené na řídicím panelu hello. Například soubor JSON hello obsahuje následující hodnoty pro hello Mnichov factory hello:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

Hello název, popis a umístění se zobrazují v tomto zobrazení v řídicím panelu hello:

![Mnichov data v řídicím panelu hello][img-munich]

Každý objekt pro vytváření, řádek výrobní a stanice mít vlastnost bitové kopie. Tyto soubory JPEG můžete najít v hello **Content\img** složky v hello **WebApp** projektu. Tyto soubory bitové kopie se zobrazí v hello připojené vytváření řídicího panelu.

Každé stanici zahrnuje několik podrobné vlastnosti, které definují hello mapování z hello OPC UA datových položek. Tyto vlastnosti jsou popsány v následující části hello:

### <a name="opcuri"></a>OpcUri

Hello **OpcUri** hodnota je hello OPC UA Application URI, který jedinečně identifikuje hello OPC UA serveru. Například hello **OpcUri** hodnota hello sestavení stanice na řádku výrobní 1 v Mnichově vypadá jako následující hello: **urn: scada2194:ua:munich:productionline0:assemblystation**.

Hello identifikátory URI hello připojené OPC UA serverů můžete zobrazit v řídicí panel řešení hello:

![Zobrazit OPC UA server identifikátory URI][img-server-uris]

### <a name="simulation"></a>Simulace

informace v hello Hello **simulace** konkrétní toohello OPC UA simulace, který je spuštěn v hello OPC UA servery, které jsou zřízené ve výchozím nastavení je uzel. Pro skutečné serveru OPC UA se nepoužívá.

### <a name="kpi1-and-kpi2"></a>Kpi1 a Kpi2

Tyto uzly popisují, jak data z hello stanice přispívá toohello dvě hodnoty klíčového ukazatele výkonu v řídicím panelu hello. Ve výchozím nasazení jsou tyto hodnoty klíčového ukazatele výkonu jednotky za hodinu a kWh za hodinu. řešení Hello vypočítá vales klíčového ukazatele výkonu na úrovni hello stanice a agreguje je na řádku výrobní hello a objektu pro vytváření.

Každý ukazatel KPI má minimální, maximální a cílovou hodnotu. Každá hodnota klíčového ukazatele výkonu můžete také definovat akce výstrah pro hello připojen objekt pro vytváření řešení tooperform. Hello následující fragment kódu ukazuje hello Definice klíčového ukazatele výkonu pro hello sestavení stanice na řádku výrobní 1 v Mnichově:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

Hello následující snímek obrazovky ukazuje hello klíčového ukazatele výkonu data v řídicím panelu hello.

![Informace o klíčových ukazatelů výkonu v řídicím panelu hello][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

Hello **OpcNodes** uzly identifikovat hello položky publikovaných dat z hello OPC UA server a zadat jak tooprocess tato data.

Hello **NodeId** hodnota identifikuje hello konkrétní OPC UA NodeID z hello OPC UA serveru. Hello první uzel v hello sestavení stanice pro produkční řádku 1 v Mnichově má hodnotu **ns = 2; i = 385**. A **NodeId** hodnota určuje tooread položky hello data z hello OPC UA serveru a hello **symbolickou** poskytuje uživatelské jméno toouse hello řídicím panelu pro tato data.

Dalších hodnot přidružených každý uzel jsou shrnuty v následující tabulce hello:

| Hodnota | Popis |
| ----- | ----------- |
| Důležitost  | hodnoty klíčového ukazatele výkonu a OEE Hello tato data přispívá k. |
| Operační kód     | Jak agregovaná hello data. |
| Jednotky      | Hello jednotky toouse hello řídicím panelu.  |
| Viditelné    | Jestli toodisplay, tato hodnota v řídicím panelu hello. Některé hodnoty se používá ve výpočtech ale nezobrazí.  |
| Maximální počet    | Hello maximální hodnota, která aktivuje výstrahu hello řídicím panelu. |
| MaximumAlertActions | Tootake akce v odpovědi tooan výstrahy. Například odešlete příkaz tooa stanice. |
| ConstValue | Konstantní hodnota používá výpočtu. |

## <a name="deploy-hello-changes"></a>Nasadit hello změny

Když dokončíte provádění změny toohello **ContosoTopologyDescription.json** souboru, je nutné znovu nasadit hello připojen objekt pro vytváření řešení tooyour účet Azure.

Hello **azure-iot připojené factory** zahrnuje úložiště **build.ps1** skript prostředí PowerShell můžete použít toorebuild a nasadit řešení hello.

## <a name="next-steps"></a>Další kroky

Další informace o vytváření předkonfigurovaného řešení hello připojené pomocí čtení hello následující články:

* [Průvodce předkonfigurovaným řešením propojené továrny][lnk-rm-walkthrough]
* [Nasazení brány pro připojené vytváření][lnk-connect-cf]
* [Oprávnění na webu azureiotsuite.com hello][lnk-permissions]
* [Propojená továrna – nejčastější dotazy](iot-suite-faq-cf.md)
* [NEJČASTĚJŠÍ DOTAZY][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md