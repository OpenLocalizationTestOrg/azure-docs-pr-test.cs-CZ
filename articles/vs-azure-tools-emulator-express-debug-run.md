---
title: "Expresní emulátor toorun aaaUsing a ladění Azure cloudové služby v místním počítači | Microsoft Docs"
description: "Pomocí emulátoru Express toorun a ladění cloudové služby v místním počítači"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 73108f98-a552-4817-b7a1-551367b71906
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/06/2017
ms.author: kraigb
ms.openlocfilehash: d89a0fc2dce42b4a2d2f93f9c4ff0482af924ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-emulator-express-toorun-and-debug-an-azure-cloud-service-on-a-local-machine"></a>Pomocí emulátoru Express toorun a ladění Azure cloudové služby v místním počítači
Pomocí emulátoru Express, můžete si otestovat a ladění cloudové služby bez spuštění sady Visual Studio jako správce. Můžete nastavit toouse nastavení vašeho projektu buď Express emulátoru nebo hello úplný emulátor, v závislosti na požadavcích hello cloudové služby. Další informace o úplný emulátor hello najdete v tématu [spustit aplikaci Azure v hello výpočetní emulátor](storage/common/storage-use-emulator.md).

## <a name="using-emulator-express-in-visual-studio"></a>Pomocí expresní emulátor služby v sadě Visual Studio
Při vytváření projektu Azure v Azure SDK 2.3 nebo novější, expresní emulátor automaticky použita. Pro existující projekty, které byly vytvořeny z předchozích verzí hello Azure SDK použijte následující kroky tooselect expresní emulátor hello:

1. Vytvořit nebo otevřít projekt Azure cloud service v sadě Visual Studio.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **vlastnosti**.

1. Na stránkách vlastností projekty hello vyberte hello **webové** kartě.

    ![Vlastnosti projektu Azure cloud service](./media/vs-azure-tools-emulator-express-debug-run/web-properties.png)

1. V části **místní vývojový Server**, vyberte **možnost použít službu IIS Express**.

1. V části **emulátoru**, vyberte **použít expresní emulátor**.
   
1. toolaunch hello emulátoru Express, spusťte následující příkaz na příkazovém řádku hello: 

    ```
    csrun.exe /useemulatorexpress
    ```

## <a name="emulator-express-limitations"></a>Omezení expresní emulátor
omezení expresní emulátor známé Hello následující problémy: 

- Expresní emulátor není kompatibilní s webového serveru IIS.
- Cloudové služby může obsahovat víc rolí, ale každá role je omezená tooone instance.
- Nelze získat přístup čísla portů pod 1 000. Pokud používáte zprostředkovatele ověřování, který obvykle využívá port pod 1 000, bude pravděpodobně nutné toochange toto číslo portu tooa hodnotu, která je vyšší než 1 000.
- Jakákoli omezení, které se vztahují toohello výpočetní emulátor Azure platí také tooEmulator Express. Například nemůže mít více než 50 instancí role na nasazení. Další informace o hello výpočetní emulátor Azure najdete v tématu [spustit aplikaci Azure v hello výpočetní emulátor](http://go.microsoft.com/fwlink/p/?LinkId=623050).

## <a name="next-steps"></a>Další kroky
[Ladění cloudové služby Azure](https://msdn.microsoft.com/library/azure/ee405479.aspx)
