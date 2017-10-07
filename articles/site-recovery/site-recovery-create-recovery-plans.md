---
title: "aaaCreate plány obnovení pro převzetí služeb při selhání a obnovení v Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak toocreate a přizpůsobení plánů obnovení v Azure Site Recovery toofail přes a obnovení virtuálních počítačů a fyzických serverů"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 09ca7719e92460b283947fdbe752e8654e5b9cab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-recovery-plans"></a>Vytvořit plány obnovení


Tento článek obsahuje informace o vytváření a přizpůsobení plánů obnovení v [Azure Site Recovery](site-recovery-overview.md).

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Vytvořte hello toodo plány obnovení je následující:

* Definujte skupiny počítačů, které převzetí služeb při selhání společně a pak spusťte společně.
* Modelu závislosti mezi počítači, když seskupíte společně do obnovení naplánujte skupiny. Například toofail přes a zprovoznit konkrétní aplikaci, všechny hello virtuálních počítačů pro tuto aplikaci do hello seskupit stejné skupiny plánu obnovení.
* Spusťte převzetí služeb při selhání. Můžete spustit test plánované, nebo neplánované převzetí služeb při selhání pro plán obnovení.


## <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení

1. Klikněte na tlačítko **plány obnovení** > **vytvoření plánu obnovení**.
   Zadejte název pro plán obnovení hello a zdroj a cíl. Hello zdrojové umístění musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení.

    - Pro replikaci tooVMM VMM, vyberte **typ zdroje** > **VMM**a hello zdrojové a cílové servery VMM. Klikněte na tlačítko **technologie Hyper-V** toosee cloudy, které jsou chráněné.
    - VMM tooAzure, vyberte **typ zdroje** > **VMM**.  Vyberte hello zdrojový server VMM, a **Azure** jako cíl hello.
    - TooAzure replikace technologie Hyper-V (bez VMM), vyberte **typ zdroje** > **technologie Hyper-V lokality**. Vyberte hello lokality jako zdroj hello a **Azure** jako cíl hello.
    - Pro virtuální počítače VMware nebo fyzický místní server tooAzure, vyberte konfigurační server jako zdroj hello a **Azure** jako cíl hello.
    - Pro plán obnovení Azure tooAzure vyberte jako cíl hello k oblasti Azure jako zdroj hello a sekundární oblasti Azure. sekundární oblasti Azure Hello jsou že tyto toowhich virtuální počítače jsou chráněné.
2. V **vybrat virtuální počítače**, vyberte hello virtuální počítače (nebo replikační skupiny), chcete tooadd toohello výchozí skupinu (skupiny 1) v plánu obnovení hello.

## <a name="customize-and-extend-recovery-plans"></a>Přizpůsobit a rozšířit plánů obnovení

Můžete přizpůsobit a rozšířit plány obnovení:

- **Přidat nové skupiny**– přidejte výchozí skupiny toohello plánu skupiny (až tooseven) další obnovení a poté přidejte další počítače nebo replikační skupiny, skupiny plánu obnovení toothose. Skupiny jsou číslované ve hello pořadí, ve kterém je přidat. Virtuální počítač nebo skupinu replikace můžete zahrnuty pouze do jedné obnovení plán skupiny.
- **Přidání ručně prováděné akce**– můžete přidat ručně prováděné akce, které spustit před nebo po skupiny pro plán obnovení. Po spuštění plánu obnovení hello zastaví v hello okamžiku, kdy jste vložili hello manuální akce. Dialogové okno zobrazí výzvu toospecify, že je dokončená hello manuální akce.
- **Skript přidáte**– můžete přidat skripty, které spustit před nebo po skupiny pro plán obnovení. Při přidání skript přidá novou sadu akcí pro skupinu hello. Například se vytvoří sadu předběžné kroky 1. skupina s názvem hello: 1. skupina: předběžné kroky. Objeví se všechny předběžné kroky v této sadě. Pokud máte server VMM nasazený, můžete přidat pouze skript v primární lokalitě hello.
- **Přidat runbooky Azure**– můžete rozšířit plány obnovení s runbooky služby Azure. Například tooautomate úlohy nebo toocreate krokování obnovení. [Další informace](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Přidat skripty

Můžete použít skripty prostředí PowerShell v plánu obnovení.

 - Zkontrolujte, jestli skripty používají bloků try-catch tak, aby hello výjimky jsou zpracovávány řádně.
    - Pokud dojde k výjimce ve skriptu hello, přestane pracovat a hello úloh zobrazí jako neúspěšná.
    - Pokud dojde k chybě, všechny zbývající část skriptu hello nefunguje.
    - Pokud dojde k chybě, když spustíte neplánované převzetí služeb při selhání, plán obnovení hello pokračuje.
    - Pokud dojde k chybě při spuštění plánovaného převzetí služeb při selhání, zastaví se plán obnovení hello. Je třeba toofix hello skriptu, zkontrolujte, zda pracuje podle očekávání a potom spusťte hello plán obnovení znovu.
- Hello Write-Host příkaz nefunguje v skript plánu obnovení a hello skript selže. toocreate výstup, vytvořte skript proxy, který pak spustí hlavního skriptu. Ujistěte se, že všechny je výstup pomocí hello >> příkaz.
  * skript Hello časového limitu, pokud není vrátit do 600 sekund.
  * Pokud nic se zapíše na tooSTDERR, skript hello je klasifikován jako neúspěšná. Tyto informace se zobrazí v hello podrobnosti provádění skriptu.

Pokud používáte VMM ve vašem nasazení:

* V kontextu hello hello účet služby VMM spustit skripty v plánu obnovení. Ujistěte se, že tento účet má oprávnění ke čtení pro hello vzdálené sdílené složce, na které hello je uložený skript. Otestujte hello toorun skript v hello úroveň oprávnění účtu služby VMM.
* Rutin služby VMM se dodávají v modulu Windows PowerShell. modul Hello je nainstalován při instalaci konzole VMM hello. Je možné načíst do vašeho skriptu pomocí hello následující příkaz ve skriptu hello:
   - Import-Module-Name virtualmachinemanager. [Další informace](https://technet.microsoft.com/library/hh875013.aspx).
* Zajistěte, že abyste měli aspoň jeden server knihovny ve vašem nasazení VMM. Ve výchozím nastavení hello cesta ke sdílené složce knihovny pro VMM server nachází místně na serveru VMM, názvem složky hello MSCVMMLibrary hello.
    * Pokud vaše cesta ke sdílené složce knihovny je vzdálený (nebo místní, ale není sdílený s MSCVMMLibrary), nakonfigurujte sdílení hello takto (pomocí \\libserver2.contoso.com\share\ jako příklad):
      * Otevřete hello Editor registru a přejděte příliš**HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure lokality Recovery\Registration**.
      * Upravit hodnotu hello **ScriptLibraryPath** a umístěte jej jako \\libserver2.contoso.com\share\. Zadejte text hello úplný plně kvalifikovaný název domény. Zadejte umístění sdílené složky toohello oprávnění.
      * Ujistěte se, abyste otestovali hello skriptu pomocí uživatelského účtu, který má hello stejný účet služby oprávnění jako hello VMM. Tato kontrola ověřuje, že samostatné otestované skripty spustit hello stejný způsobem, jak se v rámci plánů obnovení. Na serveru VMM hello nastavte hello provádění zásad toobypass následujícím způsobem:
        * Otevřete konzolu prostředí Windows PowerShell 64-bit hello použitím zvýšených oprávnění.
        * Typ: **nepoužívat Set-executionpolicy**. [Další informace](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="add-a-script-or-manual-action-tooa-plan"></a>Přidat skript nebo manuální akce tooa plán

Můžete přidat skupiny pro plán obnovení toohello výchozí skript po přidání virtuálních počítačů nebo tooit skupiny replikace a hello plán vytvořil.

1. Otevřete hello plánu obnovení.
2. Klikněte na položku v hello **krok** seznamu a pak klikněte na tlačítko **skriptu** nebo **ruční akce**.
3. Zadejte, zda toowant tooadd hello skriptu nebo akce před nebo po hello vybranou položku. Použití hello **nahoru** a **přesunout dolů** tlačítka, pozice hello toomove hello skriptu nahoru nebo dolů.
4. Pokud chcete přidat skript VMM, vyberte **převzetí služeb při selhání skriptu tooVMM**. V **cestu ke skriptu**, typ hello relativní cestu toohello sdílené složky. V následujícím příkladu VMM hello zadáte cestu hello: **\RPScripts\RPScript.PS1**.
5. Pokud přidáte Azure automation spustit adresáře, zadejte účet Azure Automation hello, ve které hello sada runbook je skript nachází a vyberte možnost hello odpovídající Azure sady runbook.
6. Proveďte převzetí služeb při selhání plánu obnovení hello, že skript hello toomake funguje podle očekávání.


### <a name="add-a-vmm-script"></a>Přidejte skript VMM

Pokud máte VMM zdrojovou lokalitu, můžete vytvořit skript na serveru VMM hello a její zahrnutí do plánu obnovení.

1. Vytvořte novou složku ve sdílené složce knihovny hello. Například \<VMMServerName > \MSSCVMMLibrary\RPScripts. Umístit na hello zdrojové a cílové servery VMM.
2. Vytvořit skript hello (například RPScript) a zkontrolujte, že funguje podle očekávání.
3. Skript hello umístit hello umístění \<VMMServerName > \MSSCVMMLibrary na serverech VMM zdrojové a cílové hello.


## <a name="next-steps"></a>Další kroky

[Další informace](site-recovery-failover.md) o spuštění převzetí služeb při selhání.
