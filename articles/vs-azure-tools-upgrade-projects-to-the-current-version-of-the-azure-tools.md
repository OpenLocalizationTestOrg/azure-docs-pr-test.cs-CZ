---
title: "aaaHow tooupgrade projekty toohello aktuální verzi hello nástroje Azure | Microsoft Docs"
description: "Zjistěte, jak tooupgrade Azure projektu v sadě Visual Studio toohello aktuální verzi hello nástroje Azure"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1d64070a-078d-468a-87f4-e6715de6475f
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: c89ba43af0f2fd9db46ce0c90f0da3d70dc1510b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupgrade-projects-toohello-current-version-of-hello-azure-tools-for-visual-studio"></a>Jak tooupgrade projekty toohello aktuální verzi hello nástroje Azure pro sadu Visual Studio
## <a name="overview"></a>Přehled
Po instalaci hello aktuální verzi nástroje Azure hello (nebo předchozí verze, která je novější než 1.6), všechny projekty, které byly vytvořeny pomocí nástroje Azure verzi před 1.6 (Listopad 2011) budou automaticky aktualizovány při jejich otevření. Pokud jste vytvořili projektů pomocí hello 1.6 (Listopad 2011) verzi těchto nástrojů a stále máte tuto verzi nainstalovat, můžete otevřít ve starší verzi hello tyto projekty a rozhodnout později zda tooupgrade je.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Jak projektu změní, když ho upgradovat
Pokud je automaticky upgradován na projekt nebo můžete určit, že chcete tooupgrade, projekt je upravena toowork s aktuálními verzemi určité sestavení a některé vlastnosti jsou také změnit, protože tato část popisuje. Pokud váš projekt vyžaduje další změny toobe kompatibilní s novější verzí hello hello nástrojů, musíte tyto změny provést ručně.

* soubor web.config Hello webové role a hello souboru app.config u rolí pracovního procesu jsou aktualizované tooreference hello novější verzi Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.
* Hello Microsoft.WindowsAzure.StorageClient.dll, Microsoft.WindowsAzure.Diagnostics.dll a Microsoft.WindowsAzure.ServiceRuntime.dll sestavení jsou upgradovaný toohello nové verze.
* Profily publikování, které byly uloženy v souboru projektu Azure hello (.ccproj) jsou samostatný soubor přesunutý tooa s .azurePubXml hello rozšíření, v hello **publikovat** podadresáři.
* Profil publikování se některé vlastnosti v hello jsou aktualizované toosupport nové a změněné funkce. **AllowUpgrade** je nahrazena **DeploymentReplacementMethod** protože nasazené cloudovou službu můžete aktualizovat současně nebo postupně.
* Hello vlastnost **UseIISExpressByDefault** bude přidána a nastavte toofalse tak, aby hello webový server, který se používá pro ladění nedojde ke změně automaticky z Internetové informační služby (IIS) tooIIS Express. Služba IIS Express je hello výchozí webový server pro projekty, které jsou vytvořeny pomocí novější verze nástrojů hello hello.
* Pokud je ukládání do mezipaměti Azure hostovaná v jednom nebo více rolí vašeho projektu, se některé vlastnosti v konfiguraci služby hello (soubor .cscfg) a definice služby (soubor .csdef) změnit při upgradu na projekt. Pokud projekt hello používá balíčku NuGet pro ukládání do mezipaměti Azure hello, projekt hello je upgradovaná toohello nejnovější verzi balíčku hello. Měli byste otevřete soubor web.config hello a ověřit, že tato konfigurace klienta hello byla zachována správně během procesu upgradu hello. Pokud jste přidali hello odkazy tooAzure ukládání do mezipaměti klienta sestavení bez použití balíčku NuGet hello, nebude aktualizovat tyto sestavení; je nutné ručně aktualizovat tyto odkazy toohello nové verze.

> [!IMPORTANT]
> Pro F # projekty je nutné ručně aktualizovat odkazy na sestavení tooAzure, tak, aby se odkazovat hello novější verze tato sestavení.
> 
> 

### <a name="how-tooupgrade-an-azure-project-toohello-current-release"></a>Jak tooupgrade Azure projektu toohello aktuální verzi
1. Nainstalujte hello aktuální verzi hello nástroje Azure do hello instalace sady Visual Studio má toouse pro hello upgrade projektu a pak otevřete hello projektu, které chcete tooupgrade. Pokud projekt hello byla vytvořena pomocí nástroje Azure verzi před 1.6 (Listopad 2011), projekt hello je automaticky upgradovaný toohello aktuální verze. Pokud projekt hello byl vytvořen s hello Listopad 2011 verze a verze je stále nainstalován, hello projekt se otevře v této verzi.
2. V Průzkumníku řešení otevřete hello místní nabídce uzlu projektu hello, zvolte **vlastnosti**a potom zvolte hello **aplikace** kartě hello zobrazeném dialogu.
   
    Hello **aplikace** kartě se zobrazují hello verze nástrojů, který je spojen s projektem hello. Pokud se zobrazí hello aktuální verzi nástroje Azure, hello projekt již byla upgradována. Pokud jste nainstalovali novější verzi nástroje hello, než jaké hello kartě se zobrazují, **Upgrade** se zobrazí tlačítko.
3. Zvolte hello **Upgrade** tlačítko tooupgrade projektu toohello aktuální verze nástroje hello.
4. Sestavení projektu hello a pak vyřešte všechny chyby, které jsou výsledkem změny rozhraní API. Informace o způsobu toomodify kódu pro hello novou verzi, naleznete v dokumentaci k hello hello konkrétní rozhraní API.

