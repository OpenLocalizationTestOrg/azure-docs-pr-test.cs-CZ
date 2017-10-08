---
title: "sestavení aaaCommand řádku pro Azure. | Microsoft Docs"
description: "Sestavení příkazového řádku pro Azure."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.openlocfilehash: 295b61ba162dd4373ee3f56cc1462decb3e16762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="building-azure-projects-from-hello-command-line"></a>Sestavení projektů Azure z příkazového řádku hello
Pomocí hello Microsoft Build Engine (MSBuild), můžete vytvořit produkty v sestavení testovacích prostředích, kde není nainstalovaná sada Visual Studio. MSBuild používá formátu XML pro soubory projektu, které je rozšiřitelný a plně podporované společností Microsoft. Pomocí formátu souboru MSBuild hello, můžete popisují, co položky musí být vytvořené pro jednu nebo více platforem a konfigurace.

Můžete také spouštět nástroje MSBuild v příkazovém řádku hello a toto téma popisuje tento přístup. Nastavením vlastnosti na příkazovém řádku hello můžete vytvořit konkrétní konfigurace projektu. Podobně můžete také definovat hello cíle, které MSBuild sestavení. Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [Reference k příkazovému řádku MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>Parametry nástroje MSBuild
Nejjednodušší způsob, jak toocreate Hello balíček je toorun MSBuild s hello `/t:Publish` možnost. Ve výchozím nastavení, tento příkaz vytvoří adresář vztah toohello kořenové složky pro projekt hello, jako například `<ProjectDirectory>\bin\Configuration\app.publish\`. Při sestavování projektu Azure jsou generovány dva soubory: hello samotný soubor balíčku a hello doprovodné konfigurační soubor:

* Soubor balíčku (`project.cspkg`)
* Konfigurační soubor (`ServiceConfiguration.TargetProfile.cscfg`)

Každý projekt, Azure ve výchozím nastavení, obsahuje jeden soubor konfigurace služby pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním). Můžete ale přidat nebo odebrat soubory konfigurace služby podle potřeby. Když vytvoříte balíček Visual Studia, zobrazí se výzva které soubor konfigurace služby tooinclude spolu s hello balíčku. Když vytvoříte balíček pomocí nástroje MSBuild, hello místního souboru konfigurace služby je zahrnuté ve výchozím nastavení. soubor tooinclude různé konfigurace služby, sada hello `TargetProfile` vlastnost hello příkaz MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Pokud chcete, aby toouse alternativní adresář pro hello uložené balíček a konfiguračních souborů, cesta hello sady pomocí hello `/p:PublishDir=Directory\` možnost, včetně hello koncové oddělovače zpětné lomítko.

## <a name="next-steps"></a>Další kroky
Jakmile hello balíček je vytvořen, můžete ho nasadit tooAzure. Kurz, který ukazuje, jak tooautomate, která zpracovávají, najdete v části [nastavené průběžné doručování pro cloudové služby v Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).

