---
title: "Aplikační server Java aaaRun na virtuálním počítači Azure classic | Microsoft Docs"
description: "Tento kurz používá prostředky, které jsou vytvořené pomocí modelu nasazení classic hello a ukazuje, jak toocreate Windows virtuální počítač a nakonfigurujte ji toorun Apache Tomcat aplikačního serveru."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 2d9f586c9f628d3738522b320996b95b078d7454
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-java-application-server-on-a-virtual-machine-created-with-hello-classic-deployment-model"></a>Jak toorun aplikačního serveru Java na virtuálním počítači vytvořit pomocí modelu nasazení classic hello
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Toodeploy webapp s Java 8 a Tomcat šablony Resource Manageru najdete v části [zde](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).

S Azure můžete použít virtuální počítač tooprovide serveru možnosti. Jako příklad může být virtuální počítač běží na Azure nakonfigurovaný toohost aplikačního serveru Java, jako je například Apache Tomcat.

Po dokončení této příručce, bude mít pochopení toho, jak toocreate virtuální počítače spuštěné v Azure a nakonfigurujte ji toorun aplikační server Java. Bude informace a proveďte hello následující úlohy:

* Jak toocreate virtuální počítač, který má Java Development Kit (JDK) už nainstalovaná.
* Jak tooremotely přihlásit tooyour virtuálního počítače.
* Jak tooinstall aplikačního serveru Java – Apache Tomcat – na virtuálním počítači.
* Jak toocreate koncového bodu pro virtuální počítač.
* Jak tooopen port v hello brány firewall pro aplikační server.

Hello dokončit výsledky instalace v Tomcat, které jsou spuštěny na virtuálním počítači.

![Virtuální počítač se systémem Apache Tomcat][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a>toocreate virtuálního počítače
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).  
2. Klikněte na tlačítko **nový**, klikněte na tlačítko **výpočetní**, pak klikněte na tlačítko **zobrazit všechny** v hello **vybrané aplikace**.
3. Klikněte na tlačítko **JDK**, klikněte na tlačítko **JDK 8** v hello **JDK** podokně.  
   Virtuální počítač bitové kopie, které podporují **JDK 6** a **JDK 7** jsou k dispozici, pokud máte starší aplikace, které nejsou připravené toorun 8 JDK.
4. V podokně hello JDK 8 vyberte **Classic**, pak klikněte na tlačítko **vytvořit**.
5. V hello **Základy** okno:
   1. Zadejte název pro virtuální počítač hello.
   2. Zadejte název pro správce hello v hello **uživatelské jméno** pole. Mějte na paměti, tento název a přiřazené heslo, který následuje další pole hello hello. Musíte je při vzdálené přihlášení toohello virtuálního počítače.
   3. Zadejte heslo do hello **nové heslo** pole a znovu ho zadejte do hello **potvrzení hesla** pole. Je toto heslo pro účet správce hello.
   4. Vyberte odpovídající hello **předplatné**.
   5. Pro hello **skupiny prostředků**, klikněte na tlačítko **vytvořit nový** a zadejte název hello novou skupinu prostředků. Nebo klikněte na tlačítko **použít existující** a vyberte jednu z hello dostupných skupin zdrojů.
   6. Vyberte umístění, kde hello virtuální počítač nachází, jako například **jihu USA**.
6. Klikněte na **Další**.
7. V hello **velikost bitové kopie virtuálního počítače** vyberte **A1 standardní** nebo jiné příslušné bitové kopie.
8. Klikněte na **Vybrat**.

9. V hello **nastavení** okně klikněte na tlačítko **OK**. Můžete použít výchozí hodnoty hello poskytovaný platformou Azure.  
10. V hello **Souhrn** okně klikněte na tlačítko **OK**.

## <a name="tooremotely-sign-in-tooyour-virtual-machine"></a>tooremotely přihlášení tooyour virtuálního počítače
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **virtuálních počítačů (klasické)**. V případě potřeby klikněte na tlačítko **další služby** na hello levém dolním rohu pod kategorií služby hello. Hello **virtuálních počítačů (klasické)** položka je uvedena v hello **výpočetní** skupiny.
3. Klikněte na název hello hello virtuálního počítače, který chcete toosign v.
4. Po spuštění virtuálního počítače hello nabídky hello horní části podokna hello umožňuje připojení.
5. Klikněte na **Připojit**.
6. Odpověď výzvy toohello jako potřebné tooconnect toohello virtuální počítač. Obvykle můžete uložit nebo otevřete soubor RDP hello, který obsahuje podrobnosti připojení hello. Můžete mít toocopy hello adresy url: port jako hello poslední část hello první řádek souboru .rdp hello a vložte ji do vzdálené aplikace přihlášení.

## <a name="tooinstall-a-java-application-server-on-your-virtual-machine"></a>tooinstall aplikačního serveru Java na virtuálním počítači
Můžete zkopírovat virtuální počítač Java aplikace server tooyour, nebo můžete nainstalovat aplikační server Java prostřednictvím Instalační program.

Tento kurz používá jako hello Java aplikace server tooinstall Tomcat.

1. Když jsou přihlášení tooyour virtuální počítač, otevřete relaci prohlížeče příliš[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).
2. Dvakrát klikněte na odkaz hello **Instalační služby systému Windows 32-bit nebo 64-bit**. Když použijete tento postup, Tomcat nainstaluje jako služby systému Windows.
3. Po zobrazení výzvy vyberte toorun hello Instalační služby.
4. V rámci hello **Apache Tomcat instalace** průvodce, postupujte podle hello vyzve k zadání tooinstall Tomcat. Pro účely tohoto kurzu hello je v pořádku přijetí hello výchozího nastavení. Když se dostanete hello **hello dokončení Průvodce instalací Apache Tomcat** dialogové okno, můžete volitelně zkontrolovat **spustit Apache Tomcat** toohave Tomcat spustit nyní. Klikněte na tlačítko **Dokončit** toocomplete hello Tomcat procesu instalace.

## <a name="toostart-tomcat"></a>toostart Tomcat

Tomcat můžete spustit ručně tak, že otevřete příkazový řádek na virtuální počítač a spuštěný příkaz hello **net&nbsp;spustit&nbsp;Tomcat8**.

Jakmile je spuštěna Tomcat, dostanete Tomcat zadáním adresy URL hello <adrese http://localhost: 8080> v prohlížeči hello virtuálního počítače.

toosee Tomcat spuštěna z externích počítačů, třeba toocreate koncový bod a otevřít port.

## <a name="toocreate-an-endpoint-for-your-virtual-machine"></a>toocreate koncového bodu pro virtuální počítač
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **virtuálních počítačů (klasické)**.
3. Klikněte na název hello hello virtuálního počítače, který je spuštěn aplikační server Java.
4. Klikněte na **Koncové body**.
5. Klikněte na tlačítko **Přidat**.
6. V hello **přidání koncového bodu** dialogové okno:
   1. Zadejte název pro koncový bod hello; například **HttpIn**.
   2. Vyberte **TCP** pro protokol hello.
   3. Zadejte **80** pro veřejný port hello.
   4. Zadejte **8080** pro privátní port hello.
   5. Vyberte **zakázané** pro hello plovoucí IP adresa.
   6. Ponechte hello seznam řízení přístupu podle je.
   7. Klikněte na tlačítko hello **OK** tlačítko dialogové okno hello tooclose a vytvořit koncový bod hello.

## <a name="tooopen-a-port-in-hello-firewall-for-your-virtual-machine"></a>tooopen port v bráně firewall hello pro virtuální počítač
1. Přihlaste se tooyour virtuálního počítače.
2. Klikněte na tlačítko **spuštění Windows**.
3. Klikněte na tlačítko **ovládací panely**.
4. Klikněte na tlačítko **systém a zabezpečení**, klikněte na tlačítko **brány Windows Firewall**a potom klikněte na **Upřesnit nastavení**.
5. Klikněte na tlačítko **příchozí pravidla**a potom klikněte na **nové pravidlo**.  
   ![Nové příchozí pravidlo][NewIBRule]
6. Pro hello **typ pravidla**, vyberte **Port**a potom klikněte na **Další**.  
   ![Nový port příchozí pravidlo][NewRulePort]
7. Na hello **protokol a porty** obrazovku, vyberte **TCP**, zadejte **8080** jako hello **konkrétní místního portu**a potom klikněte na **Další**.  
  ![Nové příchozí pravidlo][NewRuleProtocol]
8. Na hello **akce** obrazovku, vyberte **povolit připojení hello**a potom klikněte na **Další**.
   ![Nová akce příchozí pravidlo][NewRuleAction]
9. Na hello **profil** obrazovky, ujistěte se, že **domény**, **privátní**, a **veřejné** jsou vybrané a pak klikněte na tlačítko **další** .
   ![Nový profil příchozí pravidlo][NewRuleProfile]
10. Na hello **název** obrazovky, zadejte název pravidla hello, například **HttpIn** (název pravidla hello není název koncového bodu vyžaduje toomatch hello, ale) a potom klikněte na **Dokončit**.  
    ![Nový název příchozí pravidlo][NewRuleName]

V tomto okamžiku by měla být váš web Tomcat viditelný z externího prohlížeče. V okně prohlížeče hello adresu, zadejte adresu URL ve formátu hello  **http://*vaše\_DNS\_název*. cloudapp.net**, kde ***vaše\_DNS\_název*** je název DNS hello jste zadali při vytvoření hello virtuálního počítače.

## <a name="application-lifecycle-considerations"></a>Aspekty životního cyklu aplikací
* Můžete vytvořit vlastní webový archiv aplikace (WAR) a přidejte ho toohello **webapps** složky. Například vytvoření základní dynamického webového projektu Java služby stránky (JSP) a exportovat jako soubor WAR. Zkopírujte hello WAR toohello Apache Tomcat **webapps** složky na hello virtuálního počítače, spusťte v prohlížeči.
* Ve výchozím nastavení při instalaci hello Tomcat služby bude nastaveno toostart ručně. Můžete přepnout tak toostart automaticky pomocí modulu snap-in služby hello. Spusťte modul snap-in služby hello kliknutím **spustit Windows**, **nástroje pro správu**a potom **služby**. Klikněte dvakrát na hello **Apache Tomcat** služby a nastavte **typ spuštění** příliš**automatické**.

    ![Nastavení služby toostart automaticky][service_automatic_startup]

    Hello výhodou s Tomcat automatické spuštění je, že je spuštěn při hello virtuální počítač po restartu (například po instalaci aktualizace softwaru, které vyžadují restartování).

## <a name="next-steps"></a>Další kroky
Informace o jiných služeb (například Azure Storage, služby service bus a SQL Database), které může být vhodné najdete tooinclude s vaší aplikací v jazyce Java. Zobrazení informací o hello k dispozici v hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from hello "toocreate an ednpoint for your virtual mache" 3/17/2017,
     toouse hello new portal.
6. In hello **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In hello **New endpoint details** dialog box:
-->
