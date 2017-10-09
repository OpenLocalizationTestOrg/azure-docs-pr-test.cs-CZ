---
title: "hello aaaUsing Visual Studio Průvodci publikováním aplikace Azure | Microsoft Docs"
description: "Zjistěte, jak tooconfigure hello různá nastavení v hello Visual Studio Průvodci publikováním aplikace Azure"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7d8f1ac9-e439-47e0-a183-0642c4ea1920
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3f0b580d0054cc246372597660d8ae317d9b8686
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-visual-studio-publish-azure-application-wizard"></a>Pomocí hello Visual Studio Průvodci publikováním aplikace Azure
Až budete vyvíjet webové aplikace v sadě Visual Studio, můžete publikovat tuto aplikaci tooan cloudové služby Azure pomocí hello **publikování aplikaci Azure** průvodce. 

> [!NOTE]
> Toto téma se věnuje nasazení toocloud služeb, není tooweb lokalit. Informace o nasazení tooweb lokality najdete v tématu [jak tooDeploy webovou stránku Azure](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).
> 
> 

## <a name="accessing-hello-publish-azure-application-wizard"></a>Přístup k Průvodci publikování aplikaci Azure hello

Můžete přistupovat Průvodce publikování aplikaci Azure hello dvěma způsoby v závislosti na typu hello projektu sady Visual Studio, které máte.

**Pokud máte projekt Azure cloud service:**

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **publikovat**.

**Pokud máte projekt webové aplikace, který není povolen pro Azure:**

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **převést** > **převést tooAzure projekt cloudové služby**. 

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt Azure hello nově vytvořený a hello místní nabídce vyberte **publikovat**.

## <a name="sign-in-page"></a>Přihlašovací stránka

![Přihlašovací stránka](./media/vs-azure-tools-publish-azure-application-wizard/sign-in.png)

**Účet** – vyberte účet, nebo vyberte **přidat účet** v hello účet rozevíracího seznamu.

**Zvolte předplatné** – zvolte hello toouse předplatné pro vaše nasazení.
   
## <a name="settings-page---common-settings-tab"></a>Nastavení stránky – karta společné nastavení   

![Obecná nastavení](./media/vs-azure-tools-publish-azure-application-wizard/settings-common-settings.png)

**Cloudová služba** -pomocí rozevíracího seznamu hello buď vyberte existující cloudové služby, nebo vybrat možnost  **&lt;vytvořit nový >**a vytvořit cloudovou službu. Hello datového centra se zobrazí v závorkách u každé cloudové služby. Doporučuje se, že umístění center hello dat pro hello Cloudová služba hello stejné jako hello data center umístění účtu úložiště hello (Upřesnit nastavení).  

**Prostředí** -vyberte buď **produkční** nebo **pracovní**. Zvolte hello pracovní prostředí, pokud chcete, aby toodeploy vaší aplikace v testovacím prostředí. 

**Konfigurace sestavení** -vyberte buď **ladění** nebo **verze**.

**Konfigurace služby** -vyberte buď **cloudu** nebo **místní**.
   
**Povolení vzdálené plochy pro všechny role** -zaškrtněte tuto možnost, pokud chcete mít tooremotely toobe připojit toohello služby. Tato možnost slouží především pro řešení potíží. Zaškrtnutím tohoto políčka se text hello **konfigurace vzdálené plochy** zobrazí se dialogové okno. Zvolte hello **nastavení** odkaz toochange hello konfigurace.
   
**Povolit nasazení webu pro všechny webové role** -zkontrolujte tuto možnost, tooenable nasazení webu pro službu hello. Je nutné vybrat hello **povolení vzdálené plochy u všech rolí** možnost toouse tuto funkci. Další informace najdete v tématu [ [publikování cloudové služby Azure pomocí sady Visual Studio](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). 

## <a name="settings-page---advanced-settings-tab"></a>Nastavení stránky – karta Upřesnit nastavení

![Upřesnit nastavení](./media/vs-azure-tools-publish-azure-application-wizard/settings-advanced-settings.png)

**Označení nasazení** -přijměte výchozí název hello, nebo zadejte název vašeho výběru. tooappend hello datum toohello označení nasazení, ponechejte hello zaškrtnuté políčko. 
   
**Účet úložiště** – vyberte hello toouse účet úložiště pro toto nasazení **&lt;vytvořit nový > toocreate účet úložiště. Hello datového centra se zobrazí v závorkách pro každý účet úložiště. Doporučuje se, že umístění center hello dat pro hello účet úložiště hello stejné jako umístění center hello dat pro hello cloudové služby (obecná nastavení).  
   
Hello účtu úložiště Azure ukládá hello balíček pro nasazení aplikace hello. Po nasazení aplikace hello hello balíčku je odebraný z účtu úložiště hello.

**Odstranění nasazení na selhání** – vyberte tuto možnost toohave hello nasazení odstranit, pokud došlo k chybám při publikování. To mělo nezaškrtnuté, pokud chcete toomaintain konstantní virtuální IP adresy pro cloudové služby.

**Aktualizace nasazení** – tuto možnost vyberte, pokud chcete, aby toodeploy pouze aktualizované součásti. Tento typ nasazení může být rychlejší než úplné nasazení. To je třeba kontrolovat, pokud chcete toomaintain konstantní virtuální IP adresy pro cloudové služby. 

**Nasazení aktualizace - nastavení** -se používá toto dialogové okno toofurther určit způsob hello role toobe aktualizovat. Pokud se rozhodnete **přírůstkové aktualizace**, každou instanci vaší aplikace je aktualizovaný jeden po druhém, takže aplikace hello je vždy k dispozici. Pokud se rozhodnete **Souběžná aktualizace**, jsou aktualizovány všechny instance aplikace na hello stejnou dobu. Souběžné aktualizace je rychlejší, ale vaše služba nemusí být k dispozici během procesu aktualizace hello. 

![Nastavení nasazení](./media/vs-azure-tools-publish-azure-application-wizard/deployment-settings.png)

**Povolit IntelliTrace** – určete, zda mají tooenable IntelliTrace. S použitím technologie IntelliTrace můžete protokolovat rozsáhlé ladicí informace pro instanci role při spuštění v Azure. Pokud potřebujete toofind hello příčinu problému, můžete použít toostep protokoly IntelliTrace hello prostřednictvím kódu ze sady Visual Studio jako kdyby byly spuštěny v Azure. Další informace o používání IntelliTrace najdete v tématu [ladění publikovaný Azure cloud service sadou Visual Studio a IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md). 

**Povolit profilace** – určete, zda mají tooenable profilace výkonu. Visual Studio profiler Hello umožňuje tooget hloubkovou analýzu hello výpočetní aspektů jak cloudové služby běží. Další informace o použití sady Visual Studio profiler hello najdete v tématu [testování výkonu hello cloudové služby Azure](./vs-azure-tools-performance-profiling-cloud-services.md).

**Povolení vzdáleného ladicího programu u všech rolí** – určete, zda mají tooenable vzdálené ladění. Další informace o ladění cloudové služby pomocí sady Visual Studio najdete v tématu [ladění Azure cloudové služby nebo virtuálního počítače v sadě Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md).

## <a name="diagnostics-settings-page"></a>Stránka nastavení diagnostiky

![Nastavení diagnostiky](./media/vs-azure-tools-publish-azure-application-wizard/diagnostic-settings.png)

Diagnostika umožňuje tootroubleshoot cloudové služby Azure (nebo virtuální počítač Azure). Informace o diagnostiky najdete v tématu [konfigurace diagnostiky pro Azure Cloud Services a virtuální počítače](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). Informace o Application Insights najdete v tématu [co je Application Insights?](./application-insights/app-insights-overview.md).

## <a name="summary-page"></a>Stránka souhrnu

![Souhrn](./media/vs-azure-tools-publish-azure-application-wizard/summary.png)

**Cíl profil** – můžete zvolit toocreate profil publikování hello nastavení, které jste zvolili. Například může vytvořit jeden profil pro testovací prostředí a druhý k produkci. toosave tento profil, vyberte hello **Uložit** ikonu. Hello Průvodce vytvoří profil hello a uloží ho do projektu Visual Studia hello. Název profilu toomodify hello, otevřete hello **cíle profil** seznamu a potom vyberte **< spravovat... >**.
   
   > [!NOTE]
   > profil publikování Hello se zobrazí v Průzkumníku řešení v sadě Visual Studio a nastavení profilu hello se zapisují tooa souboru s příponou .azurePubxml. Nastavení se ukládají jako atributy značek XML.
   > 
   > 

## <a name="publishing-your-application"></a>Publikování aplikace

Jakmile nakonfigurujete všechny hello nastavení pro váš projekt nasazení, vyberte **publikovat** dolnímu hello hello dialogového okna. Můžete sledovat stav procesu hello v hello **výstup** oken v sadě Visual Studio.

## <a name="next-steps"></a>Další kroky
- [Migrace a publikovat webovou aplikaci tooan cloudové služby Azure ze sady Visual Studio](./vs-azure-tools-migrate-publish-web-app-to-cloud-service.md)
- [Zjistěte, jak toouse Visual Studio toopublish Azure cloudové služby](./vs-azure-tools-publishing-a-cloud-service.md)
- [Ladění publikovaný Azure cloud service sadou Visual Studio a IntelliTrace](./vs-azure-tools-intellitrace-debug-published-cloud-services.md)
- [Testování výkonu hello cloudové služby Azure](./vs-azure-tools-performance-profiling-cloud-services.md)
- [Konfigurace diagnostiky pro cloudové služby Azure a virtuálních počítačů](./vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md). 
- [Co je Application Insights?](./application-insights/app-insights-overview.md)