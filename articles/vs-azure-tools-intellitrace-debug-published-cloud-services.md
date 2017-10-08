---
title: "aaaDebugging publikované na Azure cloud service pomocí sady Visual Studio a IntelliTrace | Microsoft Docs"
description: "Zjistěte, jak služba toodebug cloudu s Visual Studio a IntelliTrace"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 60734a71d5499de72e1ca81a3ab0ab9f34bc303a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Ladění publikovaný Azure cloud service sadou Visual Studio a IntelliTrace
S použitím technologie IntelliTrace můžete protokolovat rozsáhlé ladicí informace pro instanci role při spuštění v Azure. Pokud potřebujete toofind hello příčinu problému, můžete použít toostep protokoly IntelliTrace hello prostřednictvím kódu ze sady Visual Studio jako kdyby byly spuštěny v Azure. Ve skutečnosti dat technologie IntelliTrace záznamy klíče kódu provádění a prostředí při aplikaci Azure běží jako cloudová služba v Azure a umožňuje vám přehráním hello zaznamenaná data ze sady Visual Studio. 

Můžete použít IntelliTrace, pokud máte Visual Studio Enterprise nainstalován a cíle vaší aplikace Azure rozhraní .NET Framework 4 nebo novější verze. IntelliTrace shromažďuje informace o Azure role. Hello virtuálních počítačů pro tyto role vždy spouštět 64bitové operační systémy.

Jako alternativu, můžete použít [vzdálené ladění](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach přímo Cloudová služba tooa, který běží v Azure.

> [!IMPORTANT]
> IntelliTrace je určený jenom pro účely ladění a by nemělo být použito pro produkční nasazení.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Konfigurace aplikací Azure pro IntelliTrace
tooenable IntelliTrace pro aplikaci Azure, musíte vytvořit a publikování aplikace hello z projektu Visual Studio Azure. Před publikováním tooAzure, je nutné nakonfigurovat IntelliTrace pro vaše aplikace Azure. Pokud publikujete aplikaci bez konfigurace IntelliTrace, je třeba toorepublish hello projektu. Další informace najdete v tématu [publikování Azure cloud services projektů pomocí sady Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Až budete připraveni toodeploy vaše aplikace Azure, ověřte, zda jsou vaše cíle sestavení projektu nastavena příliš**ladění**.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a hello místní nabídce vyberte **publikovat**.
   
1. V hello **publikování aplikaci Azure** dialogové okno, vyberte hello předplatného Azure a vyberte **Další**.

1. V hello **nastavení** stránky, vyberte hello **Upřesnit nastavení** kartě.

1. Zapnout hello **povolit IntelliTrace** možnost toocollect protokoly IntelliTrace pro vaši aplikaci, když je publikován v cloudu hello.
   
1. toocustomize hello základní IntelliTrace konfiguraci, vyberte **nastavení** další příliš**povolit IntelliTrace**.

    ![Odkaz nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. V hello **IntelliTrace nastavení** dialogové okno, můžete zadat, které události toolog, jestli toocollect volání informace, které procesy a moduly toocollect v protokolech a kolik místa tooallocate toohello záznam. Další informace o IntelliTrace najdete v tématu [ladění pomocí technologie IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![Nastavení IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

protokolu IntelliTrace Hello je cyklický soubor protokolu hello maximální velikost zadaná v nastavení IntelliTrace hello (hello výchozí velikost je 250 MB). Protokoly IntelliTrace jsou shromážděná tooa souboru v systému souborů hello hello virtuálního počítače. Pokud budete požadovat hello protokoly, snímek je prováděné v tomto bodě v čase a stahovat tooyour místního počítače.

Po hello cloudové služby Azure publikované tooAzure, můžete určit, pokud bylo povoleno IntelliTrace z hello Azure uzlu v **Průzkumníka serveru**, jak ukazuje následující obrázek hello:

![Průzkumník serveru - IntelliTrace povoleno](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>Stažení protokolů IntelliTrace pro instanci role
Pomocí sady Visual Studio, můžete stáhnout protokoly IntelliTrace pro instanci role pomocí následujících kroků:

1. V **Průzkumníka serveru**, rozbalte položku hello **cloudové služby** uzel a vyhledejte instanci role, jejichž protokoly, které chcete toodownload. 

1. Klikněte pravým tlačítkem na instanci role hello a hello s místní nabídce vyberte **zobrazit protokoly IntelliTrace**. 

    ![Zobrazit možnosti nabídky protokoly IntelliTrace](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. protokoly IntelliTrace Hello jsou tooa stažený soubor v adresáři v místním počítači. Pokaždé, když, abyste si vyžádali hello protokoly IntelliTrace, se vytvoří nový snímek. Během stahování hello protokoly, Visual Studio zobrazí průběh hello hello operace v hello **protokol činnosti Azure** okno. Jak ukazuje následující obrázek hello, můžete rozbalit položky na řádku hello pro operaci toosee hello podrobněji.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

V sadě Visual Studio můžete pokračovat toowork v době, kdy se stahuje protokoly IntelliTrace hello. Po dokončení stahování hello protokolu otevře v sadě Visual Studio.

> [!NOTE]
> Hello protokoly nástroje IntelliTrace může obsahovat výjimky dané platformy hello generuje a zpracovává později. Kód vnitřní framework generuje tyto výjimky jako součást normální spuštění role, takže může bezpečně ignorovat.
> 
> 

## <a name="next-steps"></a>Další kroky
- [Možnosti pro ladění cloudové služby Azure](vs-azure-tools-debugging-cloud-services-overview.md)
- [Publikování Azure cloud service pomocí sady Visual Studio](vs-azure-tools-publishing-a-cloud-service.md)