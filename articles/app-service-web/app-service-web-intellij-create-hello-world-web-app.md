---
title: "aaaCreate základní Azure webové aplikace ve IntelliJ | Microsoft Docs"
description: Tento kurz ukazuje, jak toouse hello Azure Toolkit pro IntelliJ toocreate Hello World webovou aplikaci pro Azure.
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4667497213cac3ddf754d164e614c809f338cce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a>Vytvoření základní Azure webové aplikace v IntelliJ
Tento kurz ukazuje, jak toocreate a nasazení základní tooAzure aplikace Hello, World jako webovou aplikaci pomocí hello [nástrojů Azure pro IntelliJ]. Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.

Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako toohello následující ilustrace, při zobrazení ve webovém prohlížeči:

![Ukázkové webové stránky][01]

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), v 1.8 nebo novější.
* IntelliJ IDEA Ultimate Edition. To si můžete stáhnout z <https://www.jetbrains.com/idea/download/index.html>.
* Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].
* Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Hello [nástrojů Azure pro IntelliJ]. Informace o instalaci hello nástrojů Azure najdete v tématu [hello instalace nástrojů Azure pro IntelliJ].

## <a name="toocreate-a-hello-world-application"></a>toocreate aplikace Hello World
Nejdříve začneme s vytvořením projektu Java.

1. Spusťte IntelliJ a klikněte na hello **soubor** nabídky, pak klikněte na tlačítko **nový**a potom klikněte na **projektu**.
   
    ![Soubor nový projekt][02]
2. V dialogové okno Nový projekt hello, vyberte **Java**, pak **webové aplikace**a potom klikněte na **nový** tooadd SDK projektu.
   
    ![Dialogové okno Nový projekt][03a]
   
3. V hello vyberte domovský adresář pro dialogové okno JDK, hello vyberte složku, kde je nainstalován vaší JDK a pak klikněte na tlačítko **OK**. Klikněte na tlačítko **Další** v toocontinue pole dialogové okno Nový projekt hello.
   
    ![Zadejte JDK domovský adresář][03b]
4. Pro účely tohoto kurzu, pojmenujte projekt hello **Java-Web-aplikace-na-Azure**a potom klikněte na **Dokončit**.
   
    ![Dialogové okno Nový projekt][04]
5. V rámci zobrazení IntelliJ na prohlížeči projektu rozbalte **Java-Web-aplikace-na-Azure**, pak rozbalte **webové**a potom dvakrát klikněte na **index.jsp**.
   
    ![Otevřete indexovou stránku][05c]
6. Když se soubor index.jsp otevře v IntelliJ, přidat zobrazení textu toodynamically **Hello, World!** v rámci existující hello `<body>` elementu. Aktualizovaný `<body>` obsah by měla vypadat přibližně hello následující ukázka:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. Uložte index.jsp.

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a>toodeploy tooan vaše aplikace Azure Web App kontejneru
Existuje několik způsobů, pomocí kterých můžete nasadit tooAzure webové aplikace Java. Tento kurz popisuje jedno z nejjednodušší hello: bude vaše aplikace nasazené tooan kontejneru Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba. Hello JDK a hello webový kontejner software budou poskytovat vám Azure, takže není bez nutnosti tooupload vlastní; je třeba je webové aplikace Java. V důsledku toho hello publikování pro vaše aplikace bude trvat sekund, ne minuty.

Než můžete publikovat aplikaci, musíte nejprve tooconfigure nastavení modulu. toodo tedy použijte hello následující kroky:

1. V prohlížeči projektu na IntelliJ, klikněte pravým tlačítkem na hello **Java-Web-aplikace-na-Azure** projektu. Jakmile se zobrazí hello kontextové nabídky, klikněte na tlačítko **otevřete nastavení modulu**.

    ![Otevřete nastavení modulu][05a]
2. Jakmile se zobrazí dialogové okno projektu struktura hello:

   a. Klikněte na tlačítko **artefakty** v seznamu hello **nastavení projektu**.
   b. Změna názvu artefaktů hello v hello **název** pole tak, aby neobsahuje prázdný znak nebo speciální znaky; to je nezbytné, protože název hello budou použity v hello identifikátor URI (Uniform Resource).
   c. Změna hello **typ** příliš**webové aplikace: archivu**.
   d. Klikněte na tlačítko **OK** dialogové okno tooclose hello strukturu projektu.

    ![Otevřete nastavení modulu][05b]

Pokud jste nakonfigurovali nastavení modulu, můžete publikovat tooAzure vaší aplikace pomocí hello následující kroky:

1. V prohlížeči projektu na IntelliJ, klikněte pravým tlačítkem na hello **Java-Web-aplikace-na-Azure** projektu. Jakmile se zobrazí hello kontextové nabídky, vyberte **Azure**a pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**
   
    ![Místní nabídka publikovat Azure][06]
2. Pokud nebyly již přihlášení k Azure z IntelliJ, bude výzvami toosign ke svému účtu Azure. (Pokud máte více účtů Azure, část hello výzvy při přihlášení hello v procesu se může zobrazit více než jednou, i když se objeví toobe hello stejné. V takovém případě pokračujte toofollow hello přihlašovací v pokynech.)
   
    ![Protokolů Azure v dialogovém okně][07]
3. Po úspěšné registraci ke svému účtu Azure, hello **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů. (Pokud existuje více odběry v seznamu a chcete toowork se pouze konkrétní podmnožinu z nich, vám může volitelně zrušte zaškrtnutí políčka hello odběry nechcete, aby toouse.) Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.
   
    ![Správa odběrů][08]
4. Když hello **nasazení tooAzure webové aplikace kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude hello seznam prázdný.
   
    ![Kontejnery aplikací][09]
5. Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete toopublish vaší aplikace tooa nový kontejner, použijte následující kroky hello. Jinak vyberte existujícího webového kontejneru aplikace a přeskočit toostep 6 níže.
   
   1. Klikněte na**+**
      
       ![Přidat kontejner aplikace][10]
   2. Hello **nový kontejner webové aplikace** dialogové okno se zobrazí, který bude použit pro hello vedle několik kroků.
      
       ![Nový kontejner aplikace][11a]
   3. Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat hello listu DNS popisek hello hostitelské adresy URL pro webovou aplikaci v Azure. Všimněte si, že hello název musí být k dispozici a v souladu s požadavky pojmenování tooAzure webové aplikace.
   4. V hello **webový kontejner** rozevírací nabídky, vyberte hello příslušný software pro vaši aplikaci.
      
       V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9. Poslední distribuce softwaru hello vybrané budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.
   5. V hello **předplatné** rozevírací nabídky, vyberte hello předplatné chcete toouse pro toto nasazení.
   6. V hello **skupiny prostředků** rozevírací nabídky vyberte hello skupinu prostředků pro který chcete tooassociate vaší webové aplikace. (Skupiny prostředků azure povolit, že jste toogroup související prostředky společně, aby, například, můžete je odstranit společně.)
      
       Můžete vybrat existující skupinu prostředků (pokud nějaké máte) a přeskočit toostep g níže, nebo použijte hello následující kroky toocreate novou skupinu prostředků:
      
      * Vyberte  **&lt; &lt; vytvořit novou skupinu prostředků &gt; &gt;**  v hello **skupiny prostředků** rozevírací nabídce.
      * Hello **novou skupinu prostředků** zobrazí se dialogové okno:
        
          ![Novou skupinu prostředků][12]
      * V hello hello **název** textovému poli, zadejte název nové skupiny prostředků.
      * V hello hello **oblast** rozevírací nabídky vyberte hello odpovídající Azure datového centra umístění skupiny prostředků.
      * Klikněte na **OK**.
   7. Hello **plán služby App Service** rozevírací nabídce uvádí plány hello aplikace služby, které jsou přidruženy hello skupinu prostředků, kterou jste vybrali. (Plán služby App Service určuje informace, jako je hello umístění webové aplikace hello cenová úroveň a velikost instance výpočetní hello. Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)
      
       Můžete vybrat existující plán služby App Service (pokud nějaké máte) a přeskočit toostep h níže, nebo použijte hello následující kroky toocreate nový plán služby App Service:
      
      * Vyberte  **&lt; &lt; vytvořit nový plán služby App Service &gt; &gt;**  v hello **plán služby App Service** rozevírací nabídce.
      * Hello **nový plán služby App Service** zobrazí se dialogové okno:
        
          ![Nový plán aplikační služby][13]
      * V hello hello **název** textovému poli, zadejte název pro váš nový plán služby App Service.
      * V hello hello **umístění** rozevírací nabídky vyberte hello odpovídající Azure datového centra umístění pro plán hello.
      * V hello hello **cenová úroveň** rozevírací nabídky vyberte hello příslušné ceny pro plán hello. Při testování můžete **volné**.
      * V hello hello **velikost Instance** rozevírací nabídky, vyberte hello velikost odpovídající instance hello plánu. Při testování můžete **malé**.
      * Klikněte na **OK**.
   8. (Volitelné) Ve výchozím nastavení poslední distribuce Java 8 bude automaticky nasazeno jako vaše JVM Azure tooyour webové aplikace kontejneru. Ale můžete vybrat jinou verzi a distribuci hello JVM. toodo tedy použijte hello následující kroky:
      
      * Klikněte na tlačítko hello **JDK** ve hello **nový kontejner webové aplikace** dialogové okno.
      * Můžete zvolit jednu z hello následující možnosti:
        
        * Nasadit výchozí hello JDK, které nabízí Azure
        * Nasazení 3. stran JDK z rozevíracího seznamu další JDKs, které jsou k dispozici v Azure
        * Nasazení vlastní JDK, které musí být zabalené jako soubor ZIP a buď veřejně dostupné nebo ve vašem účtu úložiště Azure
        
        ![Novou kartu JDK kontejneru aplikace][11b]
   9. Po dokončení všech hello výše uvedených kroků, by měl vypadat dialogové okno Nový kontejner webové aplikace hello hello následující obrázek:
      
       ![Nový kontejner aplikace][14]
   10. Klikněte na tlačítko **OK** toocomplete hello vytvoření kontejneru vaší nové webové aplikace.
       
        Počkejte několik sekund hello seznam toobe kontejnery webové aplikace hello aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu hello.
6. Nyní jste připravené toocomplete hello počátečního nasazení vaší webové aplikace tooAzure; Klikněte na tlačítko **OK** toodeploy vaše toohello aplikace Java vybraný kontejner webové aplikace. Ve výchozím nastavení budou aplikace nasazeny jako podadresáři hello aplikačního serveru. Pokud chcete ho nasadit jako kořenová aplikace hello toobe, zkontrolujte hello **nasazení tooroot** políčko před kliknutím na tlačítko **OK**.
   
    ![Nasazení tooAzure][15]
7. V dalším kroku byste měli vidět hello **protokol činnosti Azure** zobrazení, která bude znamenat hello stav nasazení vaší webové aplikace.
   
    ![Ukazatel průběhu][16]
   
    Hello proces nasazení vaší webové aplikace tooAzure měli jenom pár sekund trvat toocomplete. Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v hello **stav** sloupce. Po kliknutí na odkaz hello, to bude trvat domovskou stránku tooyour nasazené webové aplikace, nebo můžete použít hello kroky v následující části toobrowse tooyour webové aplikace hello.

## <a name="browsing-tooyour-web-app-on-azure"></a>Procházení tooyour webové aplikace v Azure
toobrowse tooyour webové aplikace v Azure, můžete použít hello **Azure Explorer** zobrazení.

Pokud hello **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **zobrazení** nabídky v IntelliJ, pak klikněte na **nástroj Windows**a pak klikněte na tlačítko  **Průzkumník služby**. Pokud je uživatel není přihlášený dříve, vás vyzve toodo tak.

Když hello **Azure Explorer** se zobrazí, použití postupujte podle těchto kroků toobrowse tooyour webové aplikace: 

1. Rozbalte hello **Azure** uzlu.
2. Rozbalte hello **webové aplikace** uzlu. 
3. Klikněte pravým tlačítkem na hello požadované webové aplikace.
4. Jakmile se zobrazí hello kontextové nabídky, klikněte na tlačítko **otevřít v prohlížeči**.
   
    ![Procházet webové aplikace][17]

## <a name="updating-your-web-app"></a>Aktualizace webové aplikace
Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Můžete aktualizovat hello nasazení stávající webovou aplikaci Java.
* Můžete publikovat další aplikaci toohello Java stejnému kontejneru webové aplikace.

V obou případech hello proces je stejný jako a trvá jenom pár sekund:

1. V prohlížeči projektu IntelliJ hello klikněte pravým tlačítkem na aplikaci Java hello mají tooupdate nebo přidat tooan existující webové aplikace kontejneru.
2. Jakmile se zobrazí hello kontextové nabídky, vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**
3. Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci. Vyberte jeden má toopublish nebo znovu publikovat vaše Java aplikace tooand klikněte na tlačítko hello **OK**.

Hello později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a bude moct tooverify aktualizované aplikace ve webovém prohlížeči.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Spuštění, zastavení nebo restartování existující webovou aplikaci
toostart nebo zastavit existující webové aplikace Azure kontejneru (včetně všech aplikací v jazyce Java hello nasazené v ní), můžete použít hello **Azure Explorer** zobrazení.

Pokud hello **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **zobrazení** nabídky v IntelliJ, pak klikněte na **nástroj Windows**a pak klikněte na tlačítko  **Průzkumník služby**. Pokud je uživatel není přihlášený dříve, vás vyzve toodo tak.

Když hello **Azure Explorer** se zobrazí, postupujte podle těchto kroků toostart použít nebo zastavit vaší webové aplikace: 

1. Rozbalte hello **Azure** uzlu.
2. Rozbalte hello **webové aplikace** uzlu. 
3. Klikněte pravým tlačítkem na hello požadované webové aplikace.
4. Jakmile se zobrazí hello kontextové nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**. Upozorňujeme, že jsou možnosti nabídky hello kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.
   
    ![Zastavení webové aplikace][18]

## <a name="next-steps"></a>Další kroky
Další informace o hello sadách Azure pro integrovaného vývojového prostředí Java najdete v tématu hello následující odkazy:

* [Azure nástrojů pro Eclipse]
  * [Instalace hello nástrojů Azure pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
  * [Co je nového v hello nástrojů Azure pro Eclipse]
* [nástrojů Azure pro IntelliJ]
  * [hello instalace nástrojů Azure pro IntelliJ]
  * *Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ (v tomto článku)*
  * [Co je nového v hello nástrojů Azure pro IntelliJ]

<a name="see-also"></a>

## <a name="see-also"></a>Viz také
Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java].

Další informace o vytváření webové aplikace Azure najdete v tématu hello [webových aplikací – přehled].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse.md
[nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalace hello nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[hello instalace nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co je nového v hello nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co je nového v hello nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Azure střediska pro vývojáře Java]: https://azure.microsoft.com/develop/java/
[webových aplikací – přehled]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
