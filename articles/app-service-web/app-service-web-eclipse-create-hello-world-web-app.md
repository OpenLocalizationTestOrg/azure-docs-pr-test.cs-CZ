---
title: "základní Azure webovou aplikaci pomocí aaaCreate Eclipse | Microsoft Docs"
description: Tento kurz ukazuje, jak toouse hello Azure Toolkit pro Eclipse toocreate Hello World webovou aplikaci pro Azure.
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: b2f42e0e7a5b98760ec02fab2fc38f9f07b1156b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a>Vytvořte základní Azure webovou aplikaci pomocí Eclipse
Tento kurz ukazuje, jak toocreate a nasazení základní tooAzure aplikace Hello, World jako webovou aplikaci pomocí hello [nástrojů Azure pro Eclipse]. Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.

Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako toohello následující ilustrace, při zobrazení ve webovém prohlížeči:

![Verze Preview Hello World aplikace][01]

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), v 1.8 nebo novější.
* Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, vzhled Luna nebo novější. To si můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].
* Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Hello [nástrojů Azure pro Eclipse]. Informace o instalaci hello nástrojů Azure najdete v tématu [hello instalace nástrojů Azure pro Eclipse].

## <a name="toocreate-a-hello-world-application"></a>toocreate aplikace Hello World
Nejdříve začneme s vytvořením projektu Java.

1. Spusťte Eclipse a v nabídce hello na **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**. (Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor** a **nový**, pak hello následující: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu...** , rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.)
2. Pro účely tohoto kurzu, pojmenujte projekt hello **MyWebApp**. Na obrazovce zobrazí podobné toohello následující:
   
    ![Vytvoření nového dynamického webového projektu][02]
3. Klikněte na **Dokončit**.
4. V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyWebApp**. Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).
5. V hello **nový soubor JSP** dialogové okno, název souboru hello **index.jsp**, zachovat hello nadřazené složky jako **MyWebApp/WebContent**a pak klikněte na tlačítko **Další**.
6. V hello **vybrat šablonu JSP** dialogové okno, pro účely tohoto kurzu možnost **nový soubor JSP (html)**a potom klikněte na **Dokončit**.
7. Když se soubor index.jsp otevře v prostředí Eclipse, přidejte v zobrazení textu toodynamically **Hello, World!** v rámci existující hello `<body>` elementu. Aktualizovaný `<body>` obsah by měla vypadat přibližně hello následující ukázka:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. Uložte index.jsp.

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a>toodeploy tooan vaše aplikace Azure Web App kontejneru
Existuje několik způsobů, pomocí kterých můžete nasadit tooAzure webové aplikace Java. Tento kurz popisuje jedno z nejjednodušší hello: bude vaše aplikace nasazené tooan kontejneru Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba. Hello JDK a hello webový kontejner software budou poskytovat vám Azure, takže není bez nutnosti tooupload vlastní; je třeba je webové aplikace Java. V důsledku toho hello publikování pro vaše aplikace bude trvat sekund, ne minuty.

1. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyWebApp**.
2. V místní nabídce hello vyberte **Azure**, pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**
   
    ![Publikování v Azure Web App][03]
   
    Můžete taky v hello prohlížeči projektu vybráno projektu webové aplikace, můžete kliknout na hello **publikovat** rozevírací tlačítka na panelu nástrojů hello a vyberte **publikovat jako webové aplikace Azure** zde:
   
    ![Publikování v Azure Web App][14]
3. Pokud nebyly již přihlášení k Azure z prostředí Eclipse, bude výzvami toosign ke svému účtu Azure:
   
    ![Azure přihlašovací dialogové okno][04]
   
    Pokud máte několik účtů Azure, přihlaste se některé výzvy hello během hello proces se může zobrazit více než jednou, i když se objeví toobe hello stejné. V takovém případě pokračujte, následující hello přihlášení pokyny.
4. Po úspěšné registraci ke svému účtu Azure, hello **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů. Pokud existuje více odběry v seznamu a chcete toowork se pouze konkrétní podmnožinu je, případně může zrušit zaškrtnutí hello ty, které potřebujete toouse. Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.
   
    ![Spravovat předplatná, dialogové okno][05]
5. Když hello **nasazení tooAzure webové aplikace kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude hello seznam prázdný.
   
    ![Nasazení webové aplikace kontejneru tooAzure – dialogové okno][06]
6. Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete toopublish vaší aplikace tooa nový kontejner, použijte následující kroky hello. Jinak vyberte existujícího webového kontejneru aplikace a přeskočit toostep 7 níže.
   
   1. Klikněte na tlačítko **nové...**
      
       ![Nasazení webové aplikace kontejneru tooAzure – dialogové okno][15]
   2. Hello **nový kontejner webové aplikace** zobrazí se dialogové okno:
      
       ![Dialogové okno Nový kontejner webové aplikace][07a]
   3. Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat hello listu DNS popisek hello hostitelské adresy URL pro webovou aplikaci v Azure. (Všimněte si, že hello název musí být k dispozici a v souladu s požadavky pojmenování webové aplikace tooAzure.)
   4. V hello **webový kontejner** rozevírací nabídky, vyberte hello příslušný software pro vaši aplikaci.
      
       V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9. Poslední distribuce softwaru hello vybrané budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.
   5. V hello **předplatné** rozevírací nabídky, vyberte hello předplatné chcete toouse pro toto nasazení.
   6. V hello **skupiny prostředků** rozevírací nabídky vyberte hello skupinu prostředků pro který chcete tooassociate vaší webové aplikace. (Skupiny prostředků azure povolit, že jste toogroup související prostředky společně, aby, například, můžete je odstranit společně.)
      
       Můžete vybrat existující skupinu prostředků (pokud nějaké máte) a přeskočit toostep g níže nebo hello použijte následující tyto kroky toocreate novou skupinu prostředků:
      
      * Klikněte na tlačítko **nové...**
      * Hello **novou skupinu prostředků** zobrazí se dialogové okno:
        
          ![Dialogové okno nové skupiny prostředků][08]
      * V hello hello **název** textovému poli, zadejte název nové skupiny prostředků.
      * V hello hello **oblast** rozevírací nabídky vyberte hello odpovídající Azure datového centra umístění skupiny prostředků.
      * Volitelné: Ve výchozím nastavení, poslední distribuce Java 8 bude nasazeno Azure automaticky tooyour webové aplikace kontejneru jako vaše prostředí Java Virtual Machine. Můžete však zadejte jinou verzi a distribuci hello prostředí Java Virtual Machine, pokud to vyžaduje vaší webové aplikace. toospecify hello JDK pro webovou aplikaci, klikněte na tlačítko hello **JDK** a vyberte jednu z hello následující možnosti:
        
        * **Nasadit výchozí hello JDK nabízené službou Azure Web Apps**: Tato možnost nasadí poslední distribuce Java 8.
        * **Nasazení 3. stran JDK dostupné v Azure**: Tato možnost vám umožňuje toochoose ze seznamu hello JDKs, které jsou poskytovány Microsoft Azure.
        * **Nasazení vlastní JDK z tohoto umístění stahování**: Tato možnost vám umožňuje toospecify vlastní JDK distribuci, která musí být zabalené jako soubor ZIP a nahrát tooeither umístění veřejně dostupné stahování nebo úložiště Azure účet, pro kterou je máte přístup.
          
          ![Dialogové okno Nový kontejner webové aplikace][07b]
   7. Klikněte na **OK**.
   8. Hello **plán služby App Service** rozevírací nabídce uvádí plány hello aplikace služby, které jsou přidruženy hello skupinu prostředků, kterou jste vybrali. (Plán služby app Service zadejte informace, jako je hello umístění webové aplikace hello cenová úroveň a velikost instance výpočetní hello. Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)
      
       Můžete vybrat existující plán služby App Service (pokud nějaké máte) a přeskočit toostep h níže, nebo použijte hello následující toocreate tyto kroky nový plán služby App Service:
      
      * Klikněte na tlačítko **nové...**
      * Hello **nový plán služby App Service** zobrazí se dialogové okno:
        
          ![Nový plán služby App Service dialogové okno][09]
      * V hello hello **název** textovému poli, zadejte název pro váš nový plán služby App Service.
      * V hello hello **umístění** rozevírací nabídky vyberte hello odpovídající Azure datového centra umístění pro plán hello.
      * V hello hello **cenová úroveň** rozevírací nabídky vyberte hello příslušné ceny pro plán hello. Při testování můžete **volné**.
      * V hello hello **velikost Instance** rozevírací nabídky, vyberte hello velikost odpovídající instance hello plánu. Při testování můžete **malé**.
   9. Po dokončení všech hello výše uvedených kroků, by měl vypadat dialogové okno Nový kontejner webové aplikace hello hello následující obrázek:
      
       ![Dialogové okno Nový kontejner webové aplikace][10]
   10. Klikněte na tlačítko **OK** toocomplete hello vytvoření kontejneru vaší nové webové aplikace.
       
        Počkejte několik sekund hello seznam toobe kontejnery webové aplikace hello aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu hello.
7. Jste nyní připraven toocomplete hello počátečního nasazení tooAzure vaší webové aplikace:
   
    ![Nasazení webové aplikace kontejneru tooAzure – dialogové okno][11]
   
    Klikněte na tlačítko **OK** toodeploy vaše toohello aplikace Java vybraný kontejner webové aplikace.
   
    Ve výchozím nastavení budou aplikace nasazeny jako podadresáři hello aplikačního serveru. Pokud chcete ho nasadit jako kořenová aplikace hello toobe, zkontrolujte hello **nasazení tooroot** políčko před kliknutím na tlačítko **OK**.
8. V dalším kroku byste měli vidět hello **protokol činnosti Azure** zobrazení, která bude znamenat hello stav nasazení vaší webové aplikace.
   
    ![Protokol činnosti Azure][12]
   
    Hello proces nasazení vaší webové aplikace tooAzure měli jenom pár sekund trvat toocomplete. Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v hello **stav** sloupce. Po kliknutí na odkaz hello, bude vám trvat domovskou stránku tooyour nasazené webové aplikace.

## <a name="updating-your-web-app"></a>Aktualizace webové aplikace
Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Můžete aktualizovat hello nasazení stávající webovou aplikaci Java.
* Můžete publikovat další aplikaci toohello Java stejnému kontejneru webové aplikace.

V obou případech hello proces je stejný jako a trvá jenom pár sekund:

1. V prohlížeči projektu Eclipse hello klikněte pravým tlačítkem na aplikaci Java hello mají tooupdate nebo přidat tooan existující webové aplikace kontejneru.
2. Jakmile se zobrazí hello kontextové nabídky, vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**
3. Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci. Vyberte jeden má toopublish nebo znovu publikovat vaše Java aplikace tooand klikněte na tlačítko hello **OK**.

Hello později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a bude moct tooverify aktualizované aplikace ve webovém prohlížeči.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Spuštění, zastavení nebo restartování existující webovou aplikaci
toostart nebo zastavit existující webové aplikace Azure kontejneru (včetně všech aplikací v jazyce Java hello nasazené v ní), můžete použít hello **Azure Explorer** zobrazení.

Pokud hello **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **okno** nabídky v prostředí Eclipse klikněte **zobrazit zobrazení**, pak **jiné...** , pak **Azure**a potom klikněte na **Azure Explorer**. Pokud je uživatel není přihlášený dříve, vás vyzve toodo tak.

Když hello **Azure Explorer** se zobrazí, postupujte podle těchto kroků toostart použít nebo zastavit vaší webové aplikace: 

1. Rozbalte hello **Azure** uzlu.
2. Rozbalte hello **webové aplikace** uzlu. 
3. Klikněte pravým tlačítkem na hello požadované webové aplikace.
4. Jakmile se zobrazí hello kontextové nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**. Upozorňujeme, že jsou možnosti nabídky hello kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.
   
    ![Zastavení stávající webovou aplikaci][13]

## <a name="next-steps"></a>Další kroky
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:

* [nástrojů Azure pro Eclipse]
  * [hello instalace nástrojů Azure pro Eclipse]
  * *Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse (v tomto článku)*
  * [Co je nového v hello nástrojů Azure pro Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Instalace hello Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]
  * [Co je nového v hello nástrojů Azure pro IntelliJ]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].

Další informace o vytváření webové aplikace Azure najdete v tématu hello [webových aplikací – přehled].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[hello instalace nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalace hello Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[webových aplikací – přehled]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
