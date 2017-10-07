---
title: "aaaTroubleshoot rolí, které nesplní toostart | Microsoft Docs"
description: "Tady jsou některé běžné příčiny, proč roli cloudové služby může selhat toostart. K dispozici jsou taky řešení toothese problémy."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e2fbecb08a10984add79dfc74e73de6869bb314f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-toostart"></a>Řešení potíží s cloudové služby rolí, které nesplní toostart
Tady jsou některé běžné problémy a řešení související tooAzure cloudové služby rolí, které nesplní toostart.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Chybějící knihovny DLL nebo závislosti
Reagovat role a role, které jsou prosté mezi **Probíhá inicializace**, **zaneprázdněn**, a **zastavení** stavy může být způsobeno chybějícím knihovny DLL nebo sestavení.

Příznaky chybějící knihovny DLL nebo sestavení může být:

* Role instance je prosté prostřednictvím **Probíhá inicializace**, **zaneprázdněn**, a **zastavení** stavy.
* Role instance přesunula příliš**připraven** , ale pokud přejdete tooyour webové aplikace, se nezobrazí stránka hello.

Existuje několik doporučených metod pro příčin těchto problémů.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Chybí knihovna DLL problémů ve webové roli diagnostiky
Když přejdete tooa webu, který je nasazen ve webové roli a hello prohlížeč zobrazí podobné toohello následující chyba serveru, může to znamenat, že chybí knihovna DLL.

![Chyba serveru v aplikaci '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostikovat problémy vypnutím vlastní chyby
Nakonfigurováním hello web.config pro režim tooOff vlastní chybové tooset hello hello webové role a opětovného nasazení služby hello lze zobrazit podrobnější informace o chybě.

tooview více dokončení chyby bez pomocí vzdálené plochy:

1. Otevřete hello řešení v sadě Microsoft Visual Studio.
2. V hello **Průzkumníku**, vyhledejte soubor web.config hello a otevřete jej.
3. V souboru web.config hello najděte část system.web hello a přidejte následující řádek hello:

    ```xml
    <customErrors mode="Off" />
    ```
4. Uložte soubor hello.
5. Znovu zabalte a znovu nasaďte hello služby.

Jakmile hello služba je znovu nasazena, zobrazí se chybová zpráva s názvem hello hello chybí sestavení nebo DLL.

## <a name="diagnose-issues-by-viewing-hello-error-remotely"></a>Vyřešení problémů se zobrazením hello chyba vzdáleně
Můžete používat roli hello tooaccess Vzdálená plocha a vzdáleně zobrazit podrobnější informace o chybě. Použijte následující postup tooview hello chyby pomocí vzdálené plochy hello:

1. Zkontrolujte, zda je nainstalovaný Azure SDK 1.3 nebo novější.
2. Během nasazování hello hello řešení pomocí sady Visual Studio, vyberte příliš "... Konfigurace vzdálené plochy připojení". Další informace o konfiguraci připojení ke vzdálené ploše hello najdete v tématu [pomocí vzdálené plochy s rolemi Azure](../vs-azure-tools-remote-desktop-roles.md).
3. V hello portálu Microsoft Azure classic, jakmile hello instance zobrazuje stav **připraven**, klikněte na jednu z instancí rolí hello.
4. Klikněte na tlačítko hello **připojit** ikonu v hello **vzdáleného přístupu** oblasti hello pásu karet.
5. Přihlaste se toohello virtuálního počítače pomocí hello přihlašovacích údajů, které se zadaly během konfigurace vzdálené plochy hello.
6. Otevřete okno příkazového řádku.
7. Zadejte `IPconfig`.
8. Poznamenejte si hodnotu hello adresu IPV4.
9. Otevřete Internet Explorer.
10. Zadejte adresu hello a název hello hello webové aplikace. Například, `http://<IPV4 Address>/default.aspx`.

Navigace toohello webu nyní vrátí podrobnější chybové zprávy:

* Chyba serveru v aplikaci '/'.
* Popis: Při provádění hello hello aktuální webové žádosti došlo k neošetřené výjimce. Přečtěte si hello trasování zásobníku pro další informace o chybě hello a kde vytvořena v kódu hello.
* Podrobnosti o výjimce: System.IO.FIleNotFoundException: Nelze načíst soubor nebo sestavení ' Microsoft.WindowsAzure.StorageClient, verze = 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35, nebo jeden z jeho závislých. Hello systém nemůže najít zadaný soubor hello.

Například:

![Chyba explicitní serveru v aplikaci '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-hello-compute-emulator"></a>Vyřešení problémů pomocí emulátoru služby výpočty hello
Můžete použít hello Microsoft Azure výpočetní emulátor toodiagnose a řešení problémů chybějících závislostí a chyby v souboru web.config.

Nejlepších výsledků dosáhnete v pomocí této metody diagnostiky měli byste použít počítač nebo virtuální počítač, který má čistou instalaci systému Windows. toobest simulovat hello prostředí Azure, použijte Windows Server 2008 R2 x64.

1. Nainstalujte hello samostatnou verzi hello [Azure SDK](https://azure.microsoft.com/downloads/).
2. Na vývojovém počítači hello sestavení projektu hello cloudové služby.
3. V Průzkumníku Windows přejděte toohello bin\debug složky hello projekt cloudové služby.
4. Zkopírujte složku .csx hello a počítač toohello souboru .cscfg je, že používáte toodebug hello problémy.
5. Na hello čištění počítače, otevřete okno příkazového řádku Azure SDK a zadejte `csrun.exe /devstore:start`.
6. Hello příkazového řádku, zadejte `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.
7. Když se spustí hello role, zobrazí se podrobné informace o chybě v aplikaci Internet Explorer. Můžete také použít standardní Windows nástroje pro řešení potíží toofurther diagnostikovat problém hello.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostikovat problémy s použitím technologie IntelliTrace
Pro pracovní a webové role, které používají rozhraní .NET Framework 4, můžete použít [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), která je dostupná v aplikaci Microsoft Visual Studio Enterprise.

Postupujte podle těchto kroků toodeploy hello služby s použitím technologie IntelliTrace povoleno:

1. Zkontrolujte, že je nainstalovaná sada Azure SDK 1.3 nebo novější.
2. Nasaďte řešení hello pomocí sady Visual Studio. Během nasazení, zkontrolujte hello **povolit IntelliTrace pro role rozhraní .NET 4** zaškrtávací políčko.
3. Jakmile se spustí hello instance, otevřete hello **Průzkumníka serveru**.
4. Rozbalte hello **Azure\\cloudové služby** uzlu a najděte hello nasazení.
5. Rozbalte hello nasazení, dokud neuvidíte hello instancí rolí. Klikněte pravým tlačítkem myši na jednu z instancí hello.
6. Zvolte **protokoly IntelliTrace zobrazení**. Hello **IntelliTrace Souhrn** se otevře.
7. Vyhledejte hello výjimky část hello souhrnu. Pokud jsou výjimky, bude hello části s názvem bez přípony **Data výjimky**.
8. Rozbalte hello **Data výjimky** a vyhledejte **System.IO.FileNotFoundException** podobné toohello následující chyby:

![Data výjimky, chybí soubor nebo sestavení](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adresa chybějící knihovny DLL a sestavení
tooaddress chybí chyby knihoven DLL a sestavení, postupujte takto:

1. Otevřete hello řešení v sadě Visual Studio.
2. V **Průzkumníku řešení**, otevřete hello **odkazy** složky.
3. Klikněte na určené v hello chyby sestavení hello.
4. V hello **vlastnosti** podokně vyhledejte **místní kopie vlastnosti** a nastavte hodnotu hello příliš**True**.
5. Znovu nasaďte hello cloudové služby.

Jakmile si ověříte, že byly odstraněny všechny chyby, můžete nasadit hello služby bez kontroly, zda text hello **povolit IntelliTrace pro role rozhraní .NET 4** zaškrtávací políčko.

## <a name="next-steps"></a>Další kroky
Zobrazení [řešení potíží s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pro cloudové služby.

v tématu jak problémy tootroubleshoot cloudové služby role pomocí Azure PaaS počítače diagnostická data toolearn [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
