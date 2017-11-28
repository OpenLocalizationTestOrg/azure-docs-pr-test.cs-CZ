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
# <a name="customize-how-hello-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a><span data-ttu-id="cb627-103">Přizpůsobit způsob hello připojen objekt pro vytváření řešení zobrazí data z vašich serverů OPC UA</span><span class="sxs-lookup"><span data-stu-id="cb627-103">Customize how hello connected factory solution displays data from your OPC UA servers</span></span>

## <a name="introduction"></a><span data-ttu-id="cb627-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="cb627-104">Introduction</span></span>

<span data-ttu-id="cb627-105">Hello připojené vytváření řešení agreguje a zobrazí data z hello OPC UA servery připojené toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-105">hello connected factory solution aggregates and displays data from hello OPC UA servers connected toohello solution.</span></span> <span data-ttu-id="cb627-106">Můžete procházet a odesílat příkazy toohello OPC UA servery ve vašem řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-106">You can browse and send commands toohello OPC UA servers in your solution.</span></span> <span data-ttu-id="cb627-107">Další informace o OPC UA najdete v tématu hello [připojené nejčastější dotazy týkající se vytváření](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="cb627-107">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

<span data-ttu-id="cb627-108">Agregovaná data v řešení hello příkladem hello efektivitu celkové zařízení (OEE) a klíčových ukazatelů výkonu (KPI), můžete zobrazit v řídicím panelu hello na úrovních stanice, řádku a objektu pro vytváření hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-108">Examples of aggregated data in hello solution include hello Overall Equipment Efficiency (OEE) and Key Performance Indicators (KPIs) that you can view in hello dashboard at hello factory, line, and station levels.</span></span> <span data-ttu-id="cb627-109">Hello následující snímek obrazovky ukazuje hello hodnoty OEE a klíčový ukazatel výkonu pro hello **sestavení** na stanici **produkční řádku 1**, v hello **Mnichov** factory:</span><span class="sxs-lookup"><span data-stu-id="cb627-109">hello following screenshot shows hello OEE and KPI values for hello **Assembly** station, on **Production line 1**, in hello **Munich** factory:</span></span>

![Příklad hodnoty OEE a klíčových ukazatelů výkonu v řešení hello][img-oee-kpi]

<span data-ttu-id="cb627-111">umožňuje řešení Hello tooview vám podrobné informace z konkrétní datové položky z hello OPC UA servery, nazývané *stanice*.</span><span class="sxs-lookup"><span data-stu-id="cb627-111">hello solution enables you tooview detailed information from specific data items from hello OPC UA servers, called *stations*.</span></span> <span data-ttu-id="cb627-112">Hello následující snímek obrazovky ukazuje pozemků hello počet vyrobenou položek z konkrétní stanice:</span><span class="sxs-lookup"><span data-stu-id="cb627-112">hello following screenshot shows plots of hello number of manufactured items from a specific station:</span></span>

![Pozemků počet vyrobenou položek][img-manufactured-items]

<span data-ttu-id="cb627-114">Pokud kliknete na jednu z hello grafy, můžete prozkoumat hello dat další pomocí časové řady přehledy (TSI):</span><span class="sxs-lookup"><span data-stu-id="cb627-114">If you click one of hello graphs, you can explore hello data further using Time Series Insights (TSI):</span></span>

![Prozkoumejte data pomocí statistiky časové řady][img-tsi]

<span data-ttu-id="cb627-116">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="cb627-116">This article describes:</span></span>

- <span data-ttu-id="cb627-117">Jak hello dat se provádí k dispozici toohello různých zobrazení v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-117">How hello data is made available toohello various views in hello solution.</span></span>
- <span data-ttu-id="cb627-118">Jak můžete přizpůsobit hello způsob hello řešení zobrazí hello data.</span><span class="sxs-lookup"><span data-stu-id="cb627-118">How you can customize hello way hello solution displays hello data.</span></span>

## <a name="data-sources"></a><span data-ttu-id="cb627-119">Zdroje dat</span><span class="sxs-lookup"><span data-stu-id="cb627-119">Data sources</span></span>

<span data-ttu-id="cb627-120">Hello připojené vytváření řešení zobrazí data z hello OPC UA servery připojené toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-120">hello connected factory solution displays data from hello OPC UA servers connected toohello solution.</span></span> <span data-ttu-id="cb627-121">Hello výchozí instalace zahrnuje několik OPC UA servery se systémem simulace objekt pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="cb627-121">hello default installation includes several OPC UA servers running a factory simulation.</span></span> <span data-ttu-id="cb627-122">Můžete přidat vlastní servery OPC UA, [připojení přes bránu] [ lnk-connect-cf] tooyour řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-122">You can add your own OPC UA servers that [connect through a gateway][lnk-connect-cf] tooyour solution.</span></span>

<span data-ttu-id="cb627-123">Můžete procházet, připojený server OPC UA může odesílat tooyour řešení na řídicím panelu hello hello datové položky:</span><span class="sxs-lookup"><span data-stu-id="cb627-123">You can browse hello data items that a connected OPC UA server can send tooyour solution in hello dashboard:</span></span>

1. <span data-ttu-id="cb627-124">Přejděte toohello **vyberte server OPC UA** zobrazení:</span><span class="sxs-lookup"><span data-stu-id="cb627-124">Navigate toohello **Select an OPC UA server** view:</span></span>

    ![Přejděte toohello vyberte zobrazení serverů OPC UA][img-select-server]

1. <span data-ttu-id="cb627-126">Vyberte server a klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="cb627-126">Select a server and click **Connect**.</span></span> <span data-ttu-id="cb627-127">Klikněte na tlačítko **pokračovat** Jakmile se zobrazí upozornění zabezpečení hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-127">Click **Proceed** when hello security warning appears.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cb627-128">Toto upozornění pouze vyskytuje jednou pro každý server a vytvoří vztah důvěryhodnosti mezi řídicí panel řešení hello a hello server.</span><span class="sxs-lookup"><span data-stu-id="cb627-128">This warning only appears once for each server and establishes a trust relationship between hello solution dashboard and hello server.</span></span>

1. <span data-ttu-id="cb627-129">Můžete nyní procházet hello datových položek, které hello server může odeslat toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-129">You can now browse hello data items that hello server can send toohello solution.</span></span> <span data-ttu-id="cb627-130">Položky, které jsou odesílány toohello řešení mají zelená značka zaškrtnutí:</span><span class="sxs-lookup"><span data-stu-id="cb627-130">Items that are being sent toohello solution have a green check mark:</span></span>

    ![Publikované položky][img-published]

1. <span data-ttu-id="cb627-132">Pokud jste *správce* v hello řešení, můžete zvolit toopublish toomake položka dat je k dispozici v hello připojen objekt pro vytváření řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-132">If you are an *Administrator* in hello solution, you can choose toopublish a data item toomake it available in hello connected factory solution.</span></span> <span data-ttu-id="cb627-133">Jako správce můžete také změnit hodnotu hello datové položky a volání metody v hello OPC UA serveru.</span><span class="sxs-lookup"><span data-stu-id="cb627-133">As an Administrator, you can also change hello value of data items and call methods in hello OPC UA server.</span></span>

## <a name="map-hello-data"></a><span data-ttu-id="cb627-134">Mapování dat hello</span><span class="sxs-lookup"><span data-stu-id="cb627-134">Map hello data</span></span>

<span data-ttu-id="cb627-135">Hello připojený objekt pro vytváření řešení mapy a agreguje hello položek publikovaných dat z hello OPC UA serveru toohello různých zobrazení v řešení hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-135">hello connected factory solution maps and aggregates hello published data items from hello OPC UA server toohello various views in hello solution.</span></span> <span data-ttu-id="cb627-136">Hello připojené vytváření řešení nasadí tooyour účet Azure při zřizování řešení hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-136">hello connected factory solution deploys tooyour Azure account when you provision hello solution.</span></span> <span data-ttu-id="cb627-137">Soubor JSON v hello Visual Studio připojené objekt pro vytváření řešení ukládá tyto informace o mapování.</span><span class="sxs-lookup"><span data-stu-id="cb627-137">A JSON file in hello Visual Studio connected factory solution stores this mapping information.</span></span> <span data-ttu-id="cb627-138">Můžete zobrazit a upravit tento konfigurační soubor JSON v hello připojené vytváření řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb627-138">You can view and modify this JSON configuration file in hello connected factory Visual Studio solution.</span></span> <span data-ttu-id="cb627-139">Můžete znovu nasadit řešení hello po provedení změny.</span><span class="sxs-lookup"><span data-stu-id="cb627-139">You can redeploy hello solution after you make a change.</span></span>

<span data-ttu-id="cb627-140">Můžete použít soubor konfigurace hello k:</span><span class="sxs-lookup"><span data-stu-id="cb627-140">You can use hello configuration file to:</span></span>

- <span data-ttu-id="cb627-141">Upravte existující simulované objekty Factory hello, řádky výroby a stanice.</span><span class="sxs-lookup"><span data-stu-id="cb627-141">Edit hello existing simulated factories, production lines, and stations.</span></span>
- <span data-ttu-id="cb627-142">Mapování dat z reálného OPC UA serverů připojení toohello řešení.</span><span class="sxs-lookup"><span data-stu-id="cb627-142">Map data from real OPC UA servers that you connect toohello solution.</span></span>

<span data-ttu-id="cb627-143">tooclone kopii hello připojen objekt pro vytváření řešení sady Visual Studio, hello použijte následující příkaz git:</span><span class="sxs-lookup"><span data-stu-id="cb627-143">tooclone a copy of hello connected factory Visual Studio solution, use hello following git command:</span></span>

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

<span data-ttu-id="cb627-144">soubor Hello **ContosoTopologyDescription.json** definuje hello položky toohello zobrazení v řídicím panelu řešení připojených factory hello mapování z hello OPC UA dat serveru.</span><span class="sxs-lookup"><span data-stu-id="cb627-144">hello file **ContosoTopologyDescription.json** defines hello mapping from hello OPC UA server data items toohello views in hello connected factory solution dashboard.</span></span> <span data-ttu-id="cb627-145">Tento konfigurační soubor můžete najít v hello **Contoso\Topology** složky v hello **WebApp** projekt v hello řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb627-145">You can find this configuration file in hello **Contoso\Topology** folder in hello **WebApp** project in hello Visual Studio solution.</span></span>

<span data-ttu-id="cb627-146">Hello obsah souboru JSON hello jsou uspořádána jako hierarchie objekt pro vytváření, řádek výrobní a uzly stanice.</span><span class="sxs-lookup"><span data-stu-id="cb627-146">hello content of hello JSON file is organized as a hierarchy of factory, production line, and station nodes.</span></span> <span data-ttu-id="cb627-147">Tato hierarchie definuje hello navigační hierarchii hello připojené factory řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="cb627-147">This hierarchy defines hello navigation hierarchy in hello connected factory dashboard.</span></span> <span data-ttu-id="cb627-148">Hodnoty v každém uzlu hierarchie hello určují hello informace zobrazené na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-148">Values at each node of hello hierarchy determine hello information displayed in hello dashboard.</span></span> <span data-ttu-id="cb627-149">Například soubor JSON hello obsahuje následující hodnoty pro hello Mnichov factory hello:</span><span class="sxs-lookup"><span data-stu-id="cb627-149">For example, hello JSON file contains hello following values for hello Munich factory:</span></span>

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

<span data-ttu-id="cb627-150">Hello název, popis a umístění se zobrazují v tomto zobrazení v řídicím panelu hello:</span><span class="sxs-lookup"><span data-stu-id="cb627-150">hello name, description, and location appear on this view in hello dashboard:</span></span>

![Mnichov data v řídicím panelu hello][img-munich]

<span data-ttu-id="cb627-152">Každý objekt pro vytváření, řádek výrobní a stanice mít vlastnost bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="cb627-152">Each factory, production line, and station have an image property.</span></span> <span data-ttu-id="cb627-153">Tyto soubory JPEG můžete najít v hello **Content\img** složky v hello **WebApp** projektu.</span><span class="sxs-lookup"><span data-stu-id="cb627-153">You can find these JPEG files in hello **Content\img** folder in hello **WebApp** project.</span></span> <span data-ttu-id="cb627-154">Tyto soubory bitové kopie se zobrazí v hello připojené vytváření řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="cb627-154">These image files display in hello connected factory dashboard.</span></span>

<span data-ttu-id="cb627-155">Každé stanici zahrnuje několik podrobné vlastnosti, které definují hello mapování z hello OPC UA datových položek.</span><span class="sxs-lookup"><span data-stu-id="cb627-155">Each station includes several detailed properties that define hello mapping from hello OPC UA data items.</span></span> <span data-ttu-id="cb627-156">Tyto vlastnosti jsou popsány v následující části hello:</span><span class="sxs-lookup"><span data-stu-id="cb627-156">These properties are described in hello following sections:</span></span>

### <a name="opcuri"></a><span data-ttu-id="cb627-157">OpcUri</span><span class="sxs-lookup"><span data-stu-id="cb627-157">OpcUri</span></span>

<span data-ttu-id="cb627-158">Hello **OpcUri** hodnota je hello OPC UA Application URI, který jedinečně identifikuje hello OPC UA serveru.</span><span class="sxs-lookup"><span data-stu-id="cb627-158">hello **OpcUri** value is hello OPC UA Application URI that uniquely identifies hello OPC UA server.</span></span> <span data-ttu-id="cb627-159">Například hello **OpcUri** hodnota hello sestavení stanice na řádku výrobní 1 v Mnichově vypadá jako následující hello: **urn: scada2194:ua:munich:productionline0:assemblystation**.</span><span class="sxs-lookup"><span data-stu-id="cb627-159">For example, hello **OpcUri** value for hello assembly station on production line 1 in Munich looks like hello following: **urn:scada2194:ua:munich:productionline0:assemblystation**.</span></span>

<span data-ttu-id="cb627-160">Hello identifikátory URI hello připojené OPC UA serverů můžete zobrazit v řídicí panel řešení hello:</span><span class="sxs-lookup"><span data-stu-id="cb627-160">You can view hello URIs of hello connected OPC UA servers in hello solution dashboard:</span></span>

![Zobrazit OPC UA server identifikátory URI][img-server-uris]

### <a name="simulation"></a><span data-ttu-id="cb627-162">Simulace</span><span class="sxs-lookup"><span data-stu-id="cb627-162">Simulation</span></span>

<span data-ttu-id="cb627-163">informace v hello Hello **simulace** konkrétní toohello OPC UA simulace, který je spuštěn v hello OPC UA servery, které jsou zřízené ve výchozím nastavení je uzel.</span><span class="sxs-lookup"><span data-stu-id="cb627-163">hello information in hello **Simulation** node is specific toohello OPC UA simulation that runs in hello OPC UA servers that are provisioned by default.</span></span> <span data-ttu-id="cb627-164">Pro skutečné serveru OPC UA se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="cb627-164">It is not used for a real OPC UA server.</span></span>

### <a name="kpi1-and-kpi2"></a><span data-ttu-id="cb627-165">Kpi1 a Kpi2</span><span class="sxs-lookup"><span data-stu-id="cb627-165">Kpi1 and Kpi2</span></span>

<span data-ttu-id="cb627-166">Tyto uzly popisují, jak data z hello stanice přispívá toohello dvě hodnoty klíčového ukazatele výkonu v řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-166">These nodes describe how data from hello station contributes toohello two KPI values in hello dashboard.</span></span> <span data-ttu-id="cb627-167">Ve výchozím nasazení jsou tyto hodnoty klíčového ukazatele výkonu jednotky za hodinu a kWh za hodinu.</span><span class="sxs-lookup"><span data-stu-id="cb627-167">In a default deployment, these KPI values are units per hour and kWh per hour.</span></span> <span data-ttu-id="cb627-168">řešení Hello vypočítá vales klíčového ukazatele výkonu na úrovni hello stanice a agreguje je na řádku výrobní hello a objektu pro vytváření.</span><span class="sxs-lookup"><span data-stu-id="cb627-168">hello solution calculates KPI vales at hello level of a station and aggregates them at hello production line and factory levels.</span></span>

<span data-ttu-id="cb627-169">Každý ukazatel KPI má minimální, maximální a cílovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="cb627-169">Each KPI has a minimum, maximum, and target value.</span></span> <span data-ttu-id="cb627-170">Každá hodnota klíčového ukazatele výkonu můžete také definovat akce výstrah pro hello připojen objekt pro vytváření řešení tooperform.</span><span class="sxs-lookup"><span data-stu-id="cb627-170">Each KPI value can also define alert actions for hello connected factory solution tooperform.</span></span> <span data-ttu-id="cb627-171">Hello následující fragment kódu ukazuje hello Definice klíčového ukazatele výkonu pro hello sestavení stanice na řádku výrobní 1 v Mnichově:</span><span class="sxs-lookup"><span data-stu-id="cb627-171">hello following snippet shows hello KPI definitions for hello assembly station on production line 1 in Munich:</span></span>

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

<span data-ttu-id="cb627-172">Hello následující snímek obrazovky ukazuje hello klíčového ukazatele výkonu data v řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-172">hello following screenshot shows hello KPI data in hello dashboard.</span></span>

![Informace o klíčových ukazatelů výkonu v řídicím panelu hello][lnk-kpi]

### <a name="opcnodes"></a><span data-ttu-id="cb627-174">OpcNodes</span><span class="sxs-lookup"><span data-stu-id="cb627-174">OpcNodes</span></span>

<span data-ttu-id="cb627-175">Hello **OpcNodes** uzly identifikovat hello položky publikovaných dat z hello OPC UA server a zadat jak tooprocess tato data.</span><span class="sxs-lookup"><span data-stu-id="cb627-175">hello **OpcNodes** nodes identify hello published data items from hello OPC UA server and specify how tooprocess that data.</span></span>

<span data-ttu-id="cb627-176">Hello **NodeId** hodnota identifikuje hello konkrétní OPC UA NodeID z hello OPC UA serveru.</span><span class="sxs-lookup"><span data-stu-id="cb627-176">hello **NodeId** value identifies hello specific OPC UA NodeID from hello OPC UA server.</span></span> <span data-ttu-id="cb627-177">Hello první uzel v hello sestavení stanice pro produkční řádku 1 v Mnichově má hodnotu **ns = 2; i = 385**.</span><span class="sxs-lookup"><span data-stu-id="cb627-177">hello first node in hello assembly station for production line 1 in Munich has a value **ns=2;i=385**.</span></span> <span data-ttu-id="cb627-178">A **NodeId** hodnota určuje tooread položky hello data z hello OPC UA serveru a hello **symbolickou** poskytuje uživatelské jméno toouse hello řídicím panelu pro tato data.</span><span class="sxs-lookup"><span data-stu-id="cb627-178">A **NodeId** value specifies hello data item tooread from hello OPC UA server, and hello **SymbolicName** provides a user-friendly name toouse in hello dashboard for that data.</span></span>

<span data-ttu-id="cb627-179">Dalších hodnot přidružených každý uzel jsou shrnuty v následující tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="cb627-179">Other values associated with each node are summarized in hello following table:</span></span>

| <span data-ttu-id="cb627-180">Hodnota</span><span class="sxs-lookup"><span data-stu-id="cb627-180">Value</span></span> | <span data-ttu-id="cb627-181">Popis</span><span class="sxs-lookup"><span data-stu-id="cb627-181">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="cb627-182">Důležitost</span><span class="sxs-lookup"><span data-stu-id="cb627-182">Relevance</span></span>  | <span data-ttu-id="cb627-183">hodnoty klíčového ukazatele výkonu a OEE Hello tato data přispívá k.</span><span class="sxs-lookup"><span data-stu-id="cb627-183">hello KPI and OEE values this data contributes to.</span></span> |
| <span data-ttu-id="cb627-184">Operační kód</span><span class="sxs-lookup"><span data-stu-id="cb627-184">OpCode</span></span>     | <span data-ttu-id="cb627-185">Jak agregovaná hello data.</span><span class="sxs-lookup"><span data-stu-id="cb627-185">How hello data is aggregated.</span></span> |
| <span data-ttu-id="cb627-186">Jednotky</span><span class="sxs-lookup"><span data-stu-id="cb627-186">Units</span></span>      | <span data-ttu-id="cb627-187">Hello jednotky toouse hello řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="cb627-187">hello units toouse in hello dashboard.</span></span>  |
| <span data-ttu-id="cb627-188">Viditelné</span><span class="sxs-lookup"><span data-stu-id="cb627-188">Visible</span></span>    | <span data-ttu-id="cb627-189">Jestli toodisplay, tato hodnota v řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-189">Whether toodisplay this value in hello dashboard.</span></span> <span data-ttu-id="cb627-190">Některé hodnoty se používá ve výpočtech ale nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="cb627-190">Some values are used in calculations but not displayed.</span></span>  |
| <span data-ttu-id="cb627-191">Maximální počet</span><span class="sxs-lookup"><span data-stu-id="cb627-191">Maximum</span></span>    | <span data-ttu-id="cb627-192">Hello maximální hodnota, která aktivuje výstrahu hello řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="cb627-192">hello maximum value that triggers an alert in hello dashboard.</span></span> |
| <span data-ttu-id="cb627-193">MaximumAlertActions</span><span class="sxs-lookup"><span data-stu-id="cb627-193">MaximumAlertActions</span></span> | <span data-ttu-id="cb627-194">Tootake akce v odpovědi tooan výstrahy.</span><span class="sxs-lookup"><span data-stu-id="cb627-194">An action tootake in response tooan alert.</span></span> <span data-ttu-id="cb627-195">Například odešlete příkaz tooa stanice.</span><span class="sxs-lookup"><span data-stu-id="cb627-195">For example, send a command tooa station.</span></span> |
| <span data-ttu-id="cb627-196">ConstValue</span><span class="sxs-lookup"><span data-stu-id="cb627-196">ConstValue</span></span> | <span data-ttu-id="cb627-197">Konstantní hodnota používá výpočtu.</span><span class="sxs-lookup"><span data-stu-id="cb627-197">A constant value used in a calculation.</span></span> |

## <a name="deploy-hello-changes"></a><span data-ttu-id="cb627-198">Nasadit hello změny</span><span class="sxs-lookup"><span data-stu-id="cb627-198">Deploy hello changes</span></span>

<span data-ttu-id="cb627-199">Když dokončíte provádění změny toohello **ContosoTopologyDescription.json** souboru, je nutné znovu nasadit hello připojen objekt pro vytváření řešení tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="cb627-199">When you have finished making changes toohello **ContosoTopologyDescription.json** file, you must redeploy hello connected factory solution tooyour Azure account.</span></span>

<span data-ttu-id="cb627-200">Hello **azure-iot připojené factory** zahrnuje úložiště **build.ps1** skript prostředí PowerShell můžete použít toorebuild a nasadit řešení hello.</span><span class="sxs-lookup"><span data-stu-id="cb627-200">hello **azure-iot-connected-factory** repository includes a **build.ps1** PowerShell script you can use toorebuild and deploy hello solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb627-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cb627-201">Next Steps</span></span>

<span data-ttu-id="cb627-202">Další informace o vytváření předkonfigurovaného řešení hello připojené pomocí čtení hello následující články:</span><span class="sxs-lookup"><span data-stu-id="cb627-202">Learn more about hello connected factory preconfigured solution by reading hello following articles:</span></span>

* <span data-ttu-id="cb627-203">[Průvodce předkonfigurovaným řešením propojené továrny][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="cb627-203">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="cb627-204">[Nasazení brány pro připojené vytváření][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="cb627-204">[Deploy a gateway for connected factory][lnk-connect-cf]</span></span>
* <span data-ttu-id="cb627-205">[Oprávnění na webu azureiotsuite.com hello][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="cb627-205">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="cb627-206">Propojená továrna – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="cb627-206">Connected factory FAQ</span></span>](iot-suite-faq-cf.md)
* <span data-ttu-id="cb627-207">[NEJČASTĚJŠÍ DOTAZY][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="cb627-207">[FAQ][lnk-faq]</span></span>


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