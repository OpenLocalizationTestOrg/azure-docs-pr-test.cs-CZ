---
title: "aaaRun spuštění úlohy v Azure Cloud Services | Microsoft Docs"
description: "Spuštění úlohy pomůže připravit vaše prostředí cloudové služby pro vaši aplikaci. To se naučíte, jak spuštění úloh pracovní a toomake je"
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 3391a5d7434164f59972b8e497e5c34e33409543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-and-run-startup-tasks-for-a-cloud-service"></a>Jak tooconfigure a spusťte spuštění úlohy pro cloudové služby
Spuštění úlohy tooperform operace můžete před zahájením roli. Operace může být, že chcete tooperform zahrnovat instalaci komponenty, registraci komponenty modelu COM, nastavení klíče registru nebo dlouhotrvající proces.

> [!NOTE]
> Spuštění úlohy nejsou použitelné tooVirtual počítače, jenom tooCloud webové služby a rolí pracovního procesu.
> 
> 

## <a name="how-startup-tasks-work"></a>Jak fungují spuštění úlohy
Spuštění úlohy se akcí, které před zahájením role a jsou definovány v hello [ServiceDefinition.csdef] souboru pomocí hello [úloh] v rámci hello [spuštění]elementu. Často spuštění úlohy jsou dávkové soubory, ale lze je také konzolové aplikace nebo dávkové soubory, které se spouštějí skripty prostředí PowerShell.

Proměnné prostředí předání informací do úloha spuštění a místní úložiště může být použité toopass informace o spuštění úloh. Například proměnná prostředí můžete určit program tooa cesta hello chcete tooinstall a toolocal úložiště, které lze poté číst později vaše role lze zapisovat soubory.

Spuštění úkolu můžete informace a chyby toohello adresář zadaný hello protokolu **TEMP** proměnné prostředí. Během spuštění úlohy hello hello **TEMP** proměnnou prostředí přeloží toohello *C:\\prostředky\\temp\\[identifikátor guid]. [[ Rolename]\\RoleTemp* directory při spuštění v cloudu hello.

Spuštění úlohy lze také spustit několikrát mezi jednotlivými restartováními. Například hello spuštění úlohy se spustí pokaždé, když recykluje hello role a role recykluje nemusí vždy zahrnovat restartování. Spuštění úlohy budou zasílány způsobem, který umožňuje jejich toorun bez problémů.

Spuštění úlohy musí končit **errorlevel** (nebo ukončovací kód) nula pro toocomplete procesu spuštění hello. Pokud úloha spuštění končí nenulovou **errorlevel**, hello role se nespustí.

## <a name="role-startup-order"></a>Pořadí spouštění role
Hello následující seznam obsahuje hello role spuštění procedury při spuštění v Azure:

1. Hello instance je označena jako **počáteční** a nepřijímá provoz.
2. Všechny úlohy spuštění jsou spouštěny podle tootheir **taskType** atribut.
   
   * Hello **jednoduché** synchronně, provedení úloh po jednom.
   * Hello **pozadí** a **popředí** úloh se úloha spustila asynchronně, paralelní toohello spuštění.  
     
     > [!WARNING]
     > IIS nemusí být plně nakonfigurované během hello spuštění úloh fáze procesu spouštění hello, tak roli specifických dat nemusí být k dispozici. Spuštění úlohy, které vyžadují specifickou rolí dat využít [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).
     > 
     > 
3. spuštění Hello role Hostitelský proces a hello stránka je vytvořena ve službě IIS.
4. Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) metoda je volána.
5. Hello instance je označena jako **připraven** a provoz se směruje toohello instance.
6. Hello [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metoda je volána.

## <a name="example-of-a-startup-task"></a>Příklad spuštění úlohy
Spuštění úlohy jsou definovány v hello [ServiceDefinition.csdef] souboru ve hello **úloh** elementu. Hello **commandLine** atribut určuje název hello a parametry hello spuštění dávky souboru nebo konzole příkazu hello **executionContext** atribut určuje úroveň oprávnění hello hello spuštění Úloha a hello **taskType** atribut určuje, jak bude proveden hello úloh.

V tomto příkladu se proměnná prostředí **MyVersionNumber**, se vytvoří pro hello spuštění úloh a nastavte hodnotu toohello "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

V následujícím příkladu hello, hello **Startup.cmd** dávkový soubor zapíše řádku hello "hello aktuální verze je 1.0.0.0" toohello StartupLog.txt soubor v adresáři hello určené proměnnou prostředí TEMP hello. Hello `EXIT /B 0` řádku zajišťuje končí tuto úlohu spuštění hello **errorlevel** nula.

```cmd
ECHO hello current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> V sadě Visual Studio, hello **zkopírujte tooOutput Directory** vlastnost pro váš dávkový soubor spuštění musí být nastavená příliš**kopie vždy** toobe se, že je váš dávkový soubor spuštění správně nasadili projekt tooyour v Azure (**approot\\bin** pro webové role a **approot** pro role pracovního procesu).
> 
> 

## <a name="description-of-task-attributes"></a>Popis úloh atributů
Hello následující text popisuje atributy hello hello **úloh** element v hello [ServiceDefinition.csdef] souboru:

**commandLine** -určuje hello příkazový řádek úkolu spuštění hello:

* Hello příkaz s parametry volitelné příkazového řádku, které začne úloha spuštění hello.
* Často jde hello název dávkového souboru CMD nebo BAT.
* Úloha Hello je relativní toohello AppRoot\\složky Koš služby pro nasazení hello. Při určování a hello cesta k souboru hello úlohy nejsou rozbalit proměnné prostředí. Pokud rozšíření prostředí je potřeba, můžete vytvořit malé .cmd skript, který volá spuštění úkolu.
* Může být konzolovou aplikaci nebo dávkového souboru, který začíná [skript prostředí PowerShell](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -určuje úroveň oprávnění hello hello spuštění úlohy. úroveň oprávnění Hello můžete omezené nebo se zvýšenými oprávněními:

* **omezená**  
  Úloha spuštění Hello spustí pomocí hello stejné oprávnění jako hello role. Když hello **executionContext** atribut pro hello [Runtime] element je také **omezené**, jsou použita uživatelská oprávnění.
* **zvýšené**  
  Úloha spuštění Hello spustí s oprávněními správce. To umožňuje spuštění úlohy tooinstall programy, udělat změny konfigurace služby IIS, provedení změn v registru a další úkoly úrovni správce bez zvýšení úrovně oprávnění hello hello role sám sebe.  

> [!NOTE]
> Hello úroveň oprávnění při spuštění úlohy nemusí toobe hello stejné jako hello role sám sebe.
> 
> 

**taskType** -určuje hello způsob spuštění úlohy se spustí.

* **jednoduché**  
  Úlohy jsou spouštěny synchronně, jeden po druhém, v pořadí hello zadaný v hello [ServiceDefinition.csdef] souboru. Pokud jeden **jednoduché** končí úloha spuštění **errorlevel** nulové hodnoty, hello vedle **jednoduché** spuštění úlohy je spuštěn. Pokud neexistují žádné další **jednoduché** spuštění úloh tooexecute, pak bude spuštěn hello role sám sebe.   
  
  > [!NOTE]
  > Pokud hello **jednoduché** úloh končí nenulovou **errorlevel**, hello instance se zablokuje. Následné **jednoduché** spuštění úlohy a role hello, samostatně, se nespustí.
  > 
  > 
  
    tooensure, který končí váš dávkový soubor **errorlevel** nulové hodnoty, spusťte příkaz hello `EXIT /B 0` na konci hello dávkové zpracování souboru.
* **pozadí**  
  Úlohy se spustí asynchronně, paralelně s hello spuštění role hello.
* **popředí**  
  Úlohy se spustí asynchronně, paralelně s hello spuštění role hello. Hello spočívá hlavní rozdíl mezi **popředí** a **pozadí** úloh je, že **popředí** úloh brání hello role z recyklace nebo vypnutí, dokud úloha hello byl ukončen. Hello **pozadí** úlohy nemají toto omezení.

## <a name="environment-variables"></a>Proměnné prostředí
Proměnné prostředí jsou způsob toopass informace tooa spuštění úlohy. Například můžete umístit objekt hello cesta tooa blob, který obsahuje tooinstall program nebo čísla portů, který bude používat vaše role nebo funkce toocontrol nastavení spuštění úkolu.

Existují dva typy proměnných prostředí pro spuštění úlohy; statické proměnné prostředí, proměnné prostředí založené na členy hello [ RoleEnvironment] třídy. Obě jsou v hello [prostředí] části hello [ServiceDefinition.csdef] soubor a obě použití hello [proměnné] elementu a **název** atribut.

Hello používá proměnné prostředí statickým **hodnotu** atribut hello [proměnné] elementu. výše uvedený příklad Hello vytvoří proměnnou prostředí hello **MyVersionNumber** jehož statickou hodnotu "**1.0.0.0**". Dalším příkladem může být toocreate **StagingOrProduction** proměnné prostředí, kterou můžete nastavit ručně toovalues z "**pracovní**"nebo"**produkční**" tooperform různé spuštění akce na základě hello hodnoty hello **StagingOrProduction** proměnné prostředí.

Proměnné prostředí založené na členy hello RoleEnvironment třída nepoužívejte hello **hodnotu** atribut hello [proměnné] elementu. Místo toho hello [RoleInstanceValue] podřízený element s hello odpovídající **XPath** hodnota atributu, jsou použité toocreate proměnné prostředí založené na konkrétní členem hello [ RoleEnvironment] třídy. Hodnoty pro hello **XPath** atribut tooaccess různé [ RoleEnvironment] hodnoty jsou uvedeny [zde](cloud-services-role-config-xpath.md).

Například toocreate proměnná prostředí, která je "**true**" při hello je spuštěn v emulátoru služby výpočty hello, a "**false**" při spuštění v cloudu hello, použijte následující hello [proměnné] a [RoleInstanceValue] prvky:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create hello environment variable that informs hello startup task whether it is running
                in hello Compute Emulator or in hello cloud. "%ComputeEmulatorRunning%"=="true" when
                running in hello Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in hello cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Další kroky
Zjistěte, jak tooperform některé [běžné úlohy spuštění](cloud-services-startup-tasks-common.md) s Cloudovou službou.

[Balíček](cloud-services-model-and-package.md) cloudové služby.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[úloh]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[spuštění]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[prostředí]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[proměnné]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[ RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
