---
title: "aaaAzure Automation DSC přehled | Microsoft Docs"
description: "Přehled o DSC Azure Automation požadovaného stavu konfigurace (), podmínky a známé problémy"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "prostředí PowerShell dsc, konfigurace požadovaného stavu prostředí powershell dsc azure"
ms.assetid: fd40cb68-c1a6-48c3-bba2-710b607d1555
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 5b8e5104c7b5bed848c015ac26a8b7d1f5b24de9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-dsc-overview"></a>Přehled služby Azure Automation DSC

Azure Automation DSC je služba Azure, který vám umožní toowrite, spravovat a kompilovat PowerShell požadovaného stavu konfigurace (DSC) [konfigurace](https://msdn.microsoft.com/powershell/dsc/configurations), import [prostředky DSC](https://msdn.microsoft.com/powershell/dsc/resources)a přiřaďte Konfigurace uzlů tootarget, všechny v cloudu hello.

## <a name="why-use-azure-automation-dsc"></a>Proč používat Azure Automation DSC.

Azure Automation DSC poskytuje několik výhod oproti použití DSC mimo Azure.

### <a name="built-in-pull-server"></a>Předdefinované načítacího serveru

Poskytuje služby Azure Automation [server DSC za](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) tak, aby cílové uzly automaticky zobrazí konfigurace, odpovídat toohello požadovaného stavu a zpětně hlásit dodržování předpisů.
Hello předdefinované načítacího serveru ve službě Azure Automation eliminuje nutnost tooset hello nahoru a udržovat vlastní server vyžádané replikace.
Služby Azure Automation, můžete vybrat virtuální nebo fyzická systému Windows nebo Linux počítačů, v hello cloudu nebo místně.

### <a name="management-of-all-your-dsc-artifacts"></a>Správa všech artefaktů DSC

Azure Automation DSC přináší hello stejné vrstva správy příliš[konfigurace požadovaného stavu prostředí PowerShell](https://msdn.microsoft.com/powershell/dsc/overview) jako Azure Automation nabízí skriptů prostředí PowerShell.

Od hello portálu Azure, nebo prostředí PowerShell můžete spravovat všechny vaše DSC konfigurace, prostředky a cílové uzly.

![Snímek obrazovky okna hello Azure Automation.](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importovat data pro generování sestav do analýzy protokolů

Uzly, které budou spravovat pomocí Azure Automation DSC odeslat podrobný stav dat toohello předdefinované vyžádání obsahu serveru sestav.
Tento pracovní prostor analýzy protokolů Microsoft Operations Management Suite (OMS) tooyour dat můžete nakonfigurovat toosend Azure Automation DSC.
jak zjistit, toosend DSC stav dat tooyour pracovní prostor analýzy protokolů, toolearn [dál Azure Automation DSC generování sestav dat tooOMS analýzy protokolů](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Úvodní video

Raději se díváte tooreading? Podíváme se na následující videa z květen 2015, když se poprvé oznámeno Azure Automation DSC hello.

>[!NOTE]
>Koncepty hello a životního cyklu, které jsou popsané v tomto videu jsou sice správné, Azure Automation DSC má posunula hodně dopředu a vzhledem k tomu, že záznamu tohoto videa.
>Nyní je obecně k dispozici, má mnohem rozsáhlejší uživatelské rozhraní v hello portál Azure a podporuje mnoho další funkce.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Další kroky

* toolearn způsobu tooonboard uzly toobe spravované pomocí Azure Automation DSC, najdete v [registrace počítačů pro správu Azure Automation DSC.](automation-dsc-onboarding.md)
* tooget spuštění pomocí Azure Automation DSC, najdete v části [Začínáme s Azure Automation DSC.](automation-dsc-getting-started.md)
* toolearn o kompilaci konfigurace DSC, aby je lze přiřadit tootarget uzlů, najdete v části [kompilování konfigurace v Azure Automation DSC.](automation-dsc-compile.md)
* Referenční informace o rutinách prostředí PowerShell pro Azure Automation DSC, najdete v části [rutiny Azure Automation DSC.](/powershell/module/azurerm.automation/#automation)
* Informace o cenách naleznete v tématu [ceny Azure Automation DSC.](https://azure.microsoft.com/pricing/details/automation/)
* toosee příklad použití Azure Automation DSC v kanálu průběžné nasazování, najdete v části [tooIaaS průběžné nasazování virtuálních počítačů pomocí Azure Automation DSC a Chocolatey](automation-dsc-cd-chocolatey.md)
