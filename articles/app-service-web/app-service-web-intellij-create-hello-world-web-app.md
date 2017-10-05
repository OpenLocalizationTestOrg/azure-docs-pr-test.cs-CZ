---
title: "Vytvoření základní Azure webové aplikace v IntelliJ | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak vytvořit Hello World webovou aplikaci pro Azure pomocí sady nástrojů Azure pro IntelliJ."
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
ms.openlocfilehash: 20b2c3d86f5bde9302647f345aa99b030d78613a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a>Vytvoření základní Azure webové aplikace v IntelliJ
Tento kurz ukazuje, jak vytvořit a nasadit základní aplikace Hello World do Azure jako webovou aplikaci pomocí [nástrojů Azure pro IntelliJ]. Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.

Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako na následujícím obrázku, při zobrazení ve webovém prohlížeči:

![Ukázkové webové stránky][01]

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), v 1.8 nebo novější.
* IntelliJ IDEA Ultimate Edition. To si můžete stáhnout z <https://www.jetbrains.com/idea/download/index.html>.
* Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].
* Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* [nástrojů Azure pro IntelliJ]. Informace o instalaci sady Azure najdete v tématu [instalace sady Toolkit Azure pro IntelliJ].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Hello World
Nejdříve začneme s vytvořením projektu Java.

1. Spusťte IntelliJ a klikněte na **soubor** nabídky, pak klikněte na tlačítko **nový**a potom klikněte na **projektu**.
   
    ![Soubor nový projekt][02]
2. V dialogovém okně Nový projekt vyberte **Java**, pak **webové aplikace**a potom klikněte na **nový** přidat projekt SDK.
   
    ![Dialogové okno Nový projekt][03a]
   
3. Vyberte domovský adresář pro dialogové okno JDK, vyberte složku, kde je nainstalován vaší JDK a pak klikněte na tlačítko **OK**. Klikněte na tlačítko **Další** v dialogovém okně Nový projekt pokračovat.
   
    ![Zadejte JDK domovský adresář][03b]
4. Pro účely tohoto kurzu, název projektu **Java-Web-aplikace-na-Azure**a potom klikněte na **Dokončit**.
   
    ![Dialogové okno Nový projekt][04]
5. V rámci zobrazení IntelliJ na prohlížeči projektu rozbalte **Java-Web-aplikace-na-Azure**, pak rozbalte **webové**a potom dvakrát klikněte na **index.jsp**.
   
    ![Otevřete indexovou stránku][05c]
6. Když se soubor index.jsp otevře v IntelliJ, přidejte v dynamicky zobrazený text **Hello, World!** do existujícího elementu `<body>`. Aktualizovaný `<body>` obsah by měl podobat následujícímu příkladu:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. Uložte index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Chcete-li nasadit aplikaci s kontejnerem Azure webové aplikace
Existuje několik způsobů, pomocí kterých můžete nasadit webovou aplikaci Java do Azure. Tento kurz popisuje jedno z nejjednodušší: vaše aplikace bude nasazena na kontejner Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba. Sadu JDK a webový kontejner software budou poskytovat vám Azure, takže není nutné nahrát vlastní; je třeba je webové aplikace Java. Proces publikování pro aplikaci v důsledku toho bude trvat sekund, ne minuty.

Než můžete publikovat aplikaci, musíte nejprve nakonfigurovat nastavení modulu. Chcete-li tak učinit, proveďte následující kroky:

1. V prohlížeči projektu na IntelliJ, klikněte pravým tlačítkem myši **Java-Web-aplikace-na-Azure** projektu. Jakmile se zobrazí místní nabídky, klikněte na tlačítko **otevřete nastavení modulu**.

    ![Otevřete nastavení modulu][05a]
2. Jakmile se zobrazí dialogové okno strukturu projektu:

   a. Klikněte na tlačítko **artefakty** v seznamu **nastavení projektu**.
   b. Změňte název artefaktů v **název** pole tak, aby neobsahuje prázdný znak nebo speciální znaky; to je nezbytné, protože název se použije v identifikátor URI (Uniform Resource).
   c. Změna **typ** k **webové aplikace: archivu**.
   d. Klikněte na tlačítko **OK** zavřete dialogové okno strukturu projektu.

    ![Otevřete nastavení modulu][05b]

Pokud jste nakonfigurovali nastavení modulu, můžete publikovat svoji aplikaci do Azure pomocí následujících kroků:

1. V prohlížeči projektu na IntelliJ, klikněte pravým tlačítkem myši **Java-Web-aplikace-na-Azure** projektu. Po zobrazení v místní nabídce vyberte **Azure**a pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**
   
    ![Místní nabídka publikovat Azure][06]
2. Pokud nebyly již přihlášení k Azure z IntelliJ, zobrazí se výzva k přihlásit k účtu Azure. (Pokud máte více účtů Azure, některé výzvy při přihlašování v procesu se může zobrazit více než jednou, i když se objeví být stejné. V takovém případě postupujte dál podle pokynů přihlášení.)
   
    ![Protokolů Azure v dialogovém okně][07]
3. Po úspěšné registraci ke svému účtu Azure **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů. (Pokud existuje jsou uvedena v seznamu více předplatných a chcete pracovat pouze konkrétní podmnožinu z nich, vám může volitelně zrušte zaškrtnutí políčka odběry, které nechcete použít.) Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.
   
    ![Správa odběrů][08]
4. Když **nasadit do Azure Web App kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznam prázdný.
   
    ![Kontejnery aplikací][09]
5. Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete publikovat aplikaci pro nový kontejner, použijte následující postup. Jinak vyberte existujícího webového kontejneru aplikace a přejděte ke kroku 6 níže.
   
   1. Klikněte na**+**
      
       ![Přidat kontejner aplikace][10]
   2. **Nový kontejner webové aplikace** dialogové okno se zobrazí, který se použije pro následujících několika krocích.
      
       ![Nový kontejner aplikace][11a]
   3. Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat popisek DNS listu hostitelské adresy URL pro webovou aplikaci v Azure. Všimněte si, že název musí být k dispozici a v souladu s požadavky na pojmenování webové aplikace Azure.
   4. V **webový kontejner** rozevírací nabídky vyberte příslušný software pro vaši aplikaci.
      
       V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9. Poslední distribuční vybrané softwaru budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.
   5. V **předplatné** rozevírací nabídky vyberte předplatné, kterou chcete použít pro toto nasazení.
   6. V **skupiny prostředků** rozevírací nabídky vyberte skupinu prostředků, pro který chcete přidružit webové aplikace. (Skupiny prostředků azure umožňují seskupit související prostředky tak, aby například, můžete je odstranit společně.)
      
       Můžete vybrat existující skupinu prostředků (pokud nějaké máte) a přeskočit na krok g níže, nebo pomocí následujících kroků můžete vytvořit novou skupinu prostředků:
      
      * Vyberte  **&lt; &lt; vytvořit novou skupinu prostředků &gt; &gt;**  v **skupiny prostředků** rozevírací nabídce.
      * **Novou skupinu prostředků** zobrazí se dialogové okno:
        
          ![Novou skupinu prostředků][12]
      * V **název** textovému poli, zadejte název nové skupiny prostředků.
      * V **oblast** rozevírací nabídky vyberte odpovídající Azure datového centra umístění skupiny prostředků.
      * Klikněte na **OK**.
   7. **Plán služby App Service** rozevírací nabídce uvádí plány služby aplikace, které jsou spojeny se skupinami prostředků, které jste vybrali. (Plán služby App Service určuje informace o umístění vaší webové aplikace, cenové úrovně a velikost výpočetní instance. Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)
      
       Můžete vyberte existující plán služby App Service (pokud nějaké máte) a přeskočit na krok h níže, nebo použijte následující postup k vytvoření nový plán služby App Service:
      
      * Vyberte  **&lt; &lt; vytvořit nový plán služby App Service &gt; &gt;**  v **plán služby App Service** rozevírací nabídce.
      * **Nový plán služby App Service** zobrazí se dialogové okno:
        
          ![Nový plán aplikační služby][13]
      * V **název** textovému poli, zadejte název pro váš nový plán služby App Service.
      * V **umístění** rozevírací nabídky vyberte odpovídající Azure datového centra umístění pro plán.
      * V **cenová úroveň** rozevírací nabídky vyberte příslušné ceny pro plán. Při testování můžete **volné**.
      * V **velikost Instance** rozevírací nabídky, vyberte příslušnou instanci velikost plánu. Při testování můžete **malé**.
      * Klikněte na **OK**.
   8. (Volitelné) Ve výchozím nastavení poslední distribuce Java 8 budou automaticky nasazeny jako vaše prostředí Java Virtual Machine Azure do kontejneru vaší webové aplikace. Můžete ale vybrat jinou verzi a distribuci systém JVM. Chcete-li tak učinit, proveďte následující kroky:
      
      * Klikněte **JDK** ve **nový kontejner webové aplikace** dialogové okno.
      * Můžete zvolit jednu z následujících možností:
        
        * Nasadit výchozí JDK, které nabízí Azure
        * Nasazení 3. stran JDK z rozevíracího seznamu další JDKs, které jsou k dispozici v Azure
        * Nasazení vlastní JDK, které musí být zabalené jako soubor ZIP a buď veřejně dostupné nebo ve vašem účtu úložiště Azure
        
        ![Novou kartu JDK kontejneru aplikace][11b]
   9. Po dokončení všech výše uvedených kroků, dialogové okno Nový kontejner webové aplikace by měl vypadat následovně:
      
       ![Nový kontejner aplikace][14]
   10. Klikněte na tlačítko **OK** vytvoření kontejneru vaší nové webové aplikace.
       
        Počkejte několik sekund seznam kontejnery webové aplikace nutné je aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu.
6. Nyní jste připraveni k dokončení počáteční nasazení vaší webové aplikace do Azure; Klikněte na tlačítko **OK** k nasazení aplikace v jazyce Java do vybraného kontejneru webové aplikace. Ve výchozím nastavení budou aplikace nasazeny jako podadresáři aplikačního serveru. Pokud chcete, aby ho nasadit jako kořenová aplikace, podívejte se **nasadit do kořenové** políčko před kliknutím na tlačítko **OK**.
   
    ![Nasazení do Azure][15]
7. V dalším kroku byste měli vidět **protokol činnosti Azure** zobrazení, které se označují stav nasazení vaší webové aplikace.
   
    ![Ukazatel průběhu][16]
   
    Proces nasazení webové aplikace do Azure by měl trvat jenom pár sekund. Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v **stav** sloupce. Po kliknutí na odkaz, jeho přejdete na domovskou stránku vaší nasazené webové aplikace, nebo můžete použít kroky v následující části a přejděte do vaší webové aplikace.

## <a name="browsing-to-your-web-app-on-azure"></a>Procházení do vaší webové aplikace v Azure
Chcete-li procházet do vaší webové aplikace v Azure, můžete použít **Azure Explorer** zobrazení.

Pokud **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **zobrazení** nabídky v IntelliJ, pak klikněte na **nástroj Windows**a potom klikněte na  **Průzkumník služby**. Pokud jste nepřihlásili dříve, vyzve vás k tomu.

Když **Azure Explorer** se zobrazí, použijte proveďte následující kroky a přejděte do vaší webové aplikace: 

1. Rozbalte **Azure** uzlu.
2. Rozbalte **webové aplikace** uzlu. 
3. Klikněte pravým tlačítkem na požadovanou webovou aplikaci.
4. Jakmile se zobrazí místní nabídky, klikněte na tlačítko **otevřít v prohlížeči**.
   
    ![Procházet webové aplikace][17]

## <a name="updating-your-web-app"></a>Aktualizace webové aplikace
Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Můžete aktualizovat nasazení stávající webovou aplikaci Java.
* Můžete publikovat aplikaci Java další ke stejnému kontejneru webové aplikace.

V obou případech proces je stejný jako a trvá jenom pár sekund:

1. V prohlížeči projektu IntelliJ klikněte pravým tlačítkem na aplikaci Java, které chcete aktualizovat nebo přidat do existujícího webového kontejneru aplikace.
2. Po zobrazení v místní nabídce vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**
3. Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci. Vyberte ten, který chcete publikovat nebo znovu publikovat aplikace Java a klikněte na tlačítko **OK**.

Později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a vy nebudete moci ověřit aktualizované aplikace ve webovém prohlížeči.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Spuštění, zastavení nebo restartování existující webovou aplikaci
Spuštění nebo zastavení existující webové aplikace Azure kontejneru (včetně všech nasazených aplikací Java v ní), můžete použít **Azure Explorer** zobrazení.

Pokud **Azure Explorer** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **zobrazení** nabídky v IntelliJ, pak klikněte na **nástroj Windows**a potom klikněte na  **Průzkumník služby**. Pokud jste nepřihlásili dříve, vyzve vás k tomu.

Když **Azure Explorer** zobrazení, pomocí následujícího postupu spuštění nebo zastavení webové aplikace: 

1. Rozbalte **Azure** uzlu.
2. Rozbalte **webové aplikace** uzlu. 
3. Klikněte pravým tlačítkem na požadovanou webovou aplikaci.
4. Jakmile se zobrazí místní nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**. Všimněte si, že možnosti nabídky kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.
   
    ![Zastavení webové aplikace][18]

## <a name="next-steps"></a>Další kroky
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [Azure nástrojů pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
  * [Novinky v sadě Azure Toolkit pro Eclipse]
* [nástrojů Azure pro IntelliJ]
  * [instalace sady Toolkit Azure pro IntelliJ]
  * *Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ (v tomto článku)*
  * [Novinky v sadě Azure Toolkit pro IntelliJ]

<a name="see-also"></a>

## <a name="see-also"></a>Viz také
Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

Další informace o vytváření webové aplikace Azure najdete v tématu [webových aplikací – přehled].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse.md
[nástrojů Azure pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[instalace sady Toolkit Azure pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
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
