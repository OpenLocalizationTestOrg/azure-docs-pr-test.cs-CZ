---
title: "Vytvořte základní Azure webovou aplikaci pomocí Eclipse | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak používat Azure nástrojů pro Eclipse k vytvoření Hello World webové aplikace pro Azure."
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
ms.openlocfilehash: 683d6828546995a4dc60d8cac0669f356501c723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a>Vytvořte základní Azure webovou aplikaci pomocí Eclipse
Tento kurz ukazuje, jak vytvořit a nasadit základní aplikace Hello World do Azure jako webovou aplikaci pomocí [nástrojů Azure pro Eclipse]. Základní příklad JSP je uvedený na jednoduchost, ale podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.

Po dokončení tohoto kurzu, vaše aplikace bude vypadat podobně jako na následujícím obrázku, při zobrazení ve webovém prohlížeči:

![Verze Preview Hello World aplikace][01]

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), v 1.8 nebo novější.
* Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, vzhled Luna nebo novější. To si můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Distribuční založené na jazyce Java webový server nebo aplikačního serveru jako například [Apache Tomcat] nebo [Jetty].
* Předplatné Azure, který můžete získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* [nástrojů Azure pro Eclipse]. Informace o instalaci sady Azure najdete v tématu [instalaci sady nástrojů Azure pro Eclipse].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Hello World
Nejdříve začneme s vytvořením projektu Java.

1. Spusťte Eclipse a v nabídce klikněte na položku **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**. (Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor** a **nový**, postupujte takto: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu...** , rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.)
2. Pro účely tohoto kurzu, název projektu **MyWebApp**. Na obrazovce se zobrazí podobná této:
   
    ![Vytvoření nového dynamického webového projektu][02]
3. Klikněte na **Dokončit**.
4. V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyWebApp**. Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).
5. V **nový soubor JSP** dialogové okno, název souboru **index.jsp**, nadřazený adresář ponechte na jako **MyWebApp/WebContent**a potom klikněte na **Další**.
6. V **vybrat šablonu JSP** dialogové okno, pro účely tohoto kurzu možnost **nový soubor JSP (html)**a potom klikněte na **Dokončit**.
7. Když se soubor index.jsp otevře v prostředí Eclipse, přidejte text dynamicky zobrazíte **Hello, World!** do existujícího elementu `<body>`. Aktualizovaný `<body>` obsah by měl podobat následujícímu příkladu:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. Uložte index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Chcete-li nasadit aplikaci s kontejnerem Azure webové aplikace
Existuje několik způsobů, pomocí kterých můžete nasadit webovou aplikaci Java do Azure. Tento kurz popisuje jedno z nejjednodušší: vaše aplikace bude nasazena na kontejner Azure Web App – žádné speciální projektu typu ani další nástroje jsou potřeba. Sadu JDK a webový kontejner software budou poskytovat vám Azure, takže není nutné nahrát vlastní; je třeba je webové aplikace Java. Proces publikování pro aplikaci v důsledku toho bude trvat sekund, ne minuty.

1. V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyWebApp**.
2. V místní nabídce vyberte **Azure**, pak klikněte na tlačítko **publikovat jako webové aplikace Azure...**
   
    ![Publikování v Azure Web App][03]
   
    Alternativně projektu webové aplikace v prohlížeči projektu vybráno, kliknutím **publikovat** rozevírací tlačítka na panelu nástrojů a vyberte **publikovat jako webové aplikace Azure** zde:
   
    ![Publikování v Azure Web App][14]
3. Pokud nebyly již přihlášení k Azure z prostředí Eclipse, zobrazí se výzva k přihlásit k účtu Azure:
   
    ![Azure přihlašovací dialogové okno][04]
   
    Pokud máte více účtů Azure, některé výzvy při přihlašování v procesu se může zobrazit více než jednou, i když se objeví být stejné. V takovém případě pokračujte po přihlášení pokyny.
4. Po úspěšné registraci ke svému účtu Azure **Spravovat odběry** dialogové okno se zobrazí seznam odběry, které jsou spojeny pomocí svých přihlašovacích údajů. Pokud jsou uvedena v seznamu více předplatných a chcete pracovat pouze konkrétní podmnožinu je, případně může zrušit zaškrtnutí ty, které chcete použít. Pokud jste vybrali vašich předplatných, klikněte na tlačítko **Zavřít**.
   
    ![Spravovat předplatná, dialogové okno][05]
5. Když **nasadit do Azure Web App kontejneru** se zobrazí dialogové okno, zobrazí všechny kontejnery webové aplikace, které jste předtím vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznam prázdný.
   
    ![Nasazení do dialogového okna kontejneru Azure webové aplikace][06]
6. Pokud jste dosud nevytvořili kontejner Azure webové aplikace před, nebo pokud chcete publikovat aplikaci pro nový kontejner, použijte následující postup. Jinak vyberte existujícího webového kontejneru aplikace a přejděte ke kroku 7 níže.
   
   1. Klikněte na tlačítko **nové...**
      
       ![Nasazení do dialogového okna kontejneru Azure webové aplikace][15]
   2. **Nový kontejner webové aplikace** zobrazí se dialogové okno:
      
       ![Dialogové okno Nový kontejner webové aplikace][07a]
   3. Zadejte **popisek DNS** pro kontejner vaší webové aplikace; to budou formovat popisek DNS listu hostitelské adresy URL pro webovou aplikaci v Azure. (Všimněte si, že název musí být k dispozici a v souladu s požadavky na pojmenování webové aplikace Azure.)
   4. V **webový kontejner** rozevírací nabídky vyberte příslušný software pro vaši aplikaci.
      
       V současné době můžete z Tomcat 8, Tomcat 7 nebo Jetty 9. Poslední distribuční vybrané softwaru budou poskytovat Azure a bude spuštěna v posledních distribučním JDK 8 vytvořené Oracle a poskytovaný platformou Azure.
   5. V **předplatné** rozevírací nabídky vyberte předplatné, kterou chcete použít pro toto nasazení.
   6. V **skupiny prostředků** rozevírací nabídky vyberte skupinu prostředků, pro který chcete přidružit webové aplikace. (Skupiny prostředků azure umožňují seskupit související prostředky tak, aby například, můžete je odstranit společně.)
      
       Můžete vyberte existující skupinu prostředků (pokud nějaké máte) a přeskočit na krok g níže nebo použít tyto kroky k vytvoření nové skupiny prostředků:
      
      * Klikněte na tlačítko **nové...**
      * **Novou skupinu prostředků** zobrazí se dialogové okno:
        
          ![Dialogové okno nové skupiny prostředků][08]
      * V **název** textovému poli, zadejte název nové skupiny prostředků.
      * V **oblast** rozevírací nabídky vyberte odpovídající Azure datového centra umístění skupiny prostředků.
      * Volitelné: Ve výchozím nastavení, poslední distribuce Java 8 bude nasazeno Azure automaticky do vaší webové aplikace kontejneru jako vaše prostředí Java Virtual Machine. Můžete však zadejte jinou verzi a distribuci systém JVM, pokud to vyžaduje vaší webové aplikace. Chcete-li zadat sadu JDK pro vaši webovou aplikaci, klikněte na tlačítko **JDK** a vyberte jednu z následujících možností:
        
        * **Nasadit výchozí JDK nabízené službou Azure Web Apps**: Tato možnost nasadí poslední distribuce Java 8.
        * **Nasazení 3. stran JDK dostupné v Azure**: Tato možnost vám umožňuje vybrat ze seznamu JDKs, které jsou poskytovány Microsoft Azure.
        * **Nasazení vlastní JDK z tohoto umístění stahování**: Tato možnost umožňuje zadat vlastní JDK distribuce, které musí být jako soubor ZIP zabalené a nahrané do umístění veřejně dostupné stahování nebo účet úložiště Azure, pro které máte přístup.
          
          ![Dialogové okno Nový kontejner webové aplikace][07b]
   7. Klikněte na **OK**.
   8. **Plán služby App Service** rozevírací nabídce uvádí plány služby aplikace, které jsou spojeny se skupinami prostředků, které jste vybrali. (Informace o umístění vaší webové aplikace, cenové úrovně a velikost instance výpočetní zadejte plány služby app Service. Jednoho plánu služby App Service můžete použít pro více webových aplikací, které je důvod, proč je zachován odděleně od konkrétní nasazení webové aplikace.)
      
       Můžete vyberte existující plán služby App Service (pokud nějaké máte) a přeskočit na krok h níže nebo použít tyto kroky k vytvoření nový plán služby App Service:
      
      * Klikněte na tlačítko **nové...**
      * **Nový plán služby App Service** zobrazí se dialogové okno:
        
          ![Nový plán služby App Service dialogové okno][09]
      * V **název** textovému poli, zadejte název pro váš nový plán služby App Service.
      * V **umístění** rozevírací nabídky vyberte odpovídající Azure datového centra umístění pro plán.
      * V **cenová úroveň** rozevírací nabídky vyberte příslušné ceny pro plán. Při testování můžete **volné**.
      * V **velikost Instance** rozevírací nabídky, vyberte příslušnou instanci velikost plánu. Při testování můžete **malé**.
   9. Po dokončení všech výše uvedených kroků, dialogové okno Nový kontejner webové aplikace by měl vypadat následovně:
      
       ![Dialogové okno Nový kontejner webové aplikace][10]
   10. Klikněte na tlačítko **OK** vytvoření kontejneru vaší nové webové aplikace.
       
        Počkejte několik sekund seznam kontejnery webové aplikace nutné je aktualizovat a nově vytvořený webový kontejner aplikace měla by být vybrána nyní v seznamu.
7. Nyní jste připraveni k dokončení počáteční nasazení vaší webové aplikace do Azure:
   
    ![Nasazení do dialogového okna kontejneru Azure webové aplikace][11]
   
    Klikněte na tlačítko **OK** k nasazení aplikace v jazyce Java do vybraného kontejneru webové aplikace.
   
    Ve výchozím nastavení budou aplikace nasazeny jako podadresáři aplikačního serveru. Pokud chcete, aby ho nasadit jako kořenová aplikace, podívejte se **nasadit do kořenové** políčko před kliknutím na tlačítko **OK**.
8. V dalším kroku byste měli vidět **protokol činnosti Azure** zobrazení, které se označují stav nasazení vaší webové aplikace.
   
    ![Protokol činnosti Azure][12]
   
    Proces nasazení webové aplikace do Azure by měl trvat jenom pár sekund. Pokud vaše aplikace připravené, zobrazí se odkaz s názvem **publikováno** v **stav** sloupce. Po kliknutí na odkaz, jeho přejdete na domovskou stránku vaší nasazené webové aplikace.

## <a name="updating-your-web-app"></a>Aktualizace webové aplikace
Aktualizace stávající spuštění webové aplikace Azure je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Můžete aktualizovat nasazení stávající webovou aplikaci Java.
* Můžete publikovat aplikaci Java další ke stejnému kontejneru webové aplikace.

V obou případech proces je stejný jako a trvá jenom pár sekund:

1. V prohlížeči projektu Eclipse klikněte pravým tlačítkem na aplikaci Java, které chcete aktualizovat nebo přidat do existujícího webového kontejneru aplikace.
2. Po zobrazení v místní nabídce vyberte **Azure** a potom **publikovat jako webové aplikace Azure...**
3. Vzhledem k tomu, že jste již přihlášení dříve, zobrazí se seznam kontejnerů existující webovou aplikaci. Vyberte ten, který chcete publikovat nebo znovu publikovat aplikace Java a klikněte na tlačítko **OK**.

Později, několik sekund **protokol činnosti Azure** zobrazení se zobrazí aktualizovaná nasazení jako **publikováno** a vy nebudete moci ověřit aktualizované aplikace ve webovém prohlížeči.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Spuštění, zastavení nebo restartování existující webovou aplikaci
Spuštění nebo zastavení existující webové aplikace Azure kontejneru (včetně všech nasazených aplikací Java v ní), můžete použít **Azure Explorer** zobrazení.

Pokud **Průzkumník Azure** zobrazení ještě není otevřené, který můžete otevřít kliknutím na tlačítko pak **okno** nabídky v prostředí Eclipse klikněte **zobrazit zobrazení**, pak **jiné...** , pak **Azure**a potom klikněte na **Azure Explorer**. Pokud jste nepřihlásili dříve, vyzve vás k tomu.

Když **Azure Explorer** zobrazení, pomocí následujícího postupu spuštění nebo zastavení webové aplikace: 

1. Rozbalte **Azure** uzlu.
2. Rozbalte **webové aplikace** uzlu. 
3. Klikněte pravým tlačítkem na požadovanou webovou aplikaci.
4. Jakmile se zobrazí místní nabídky, klikněte na tlačítko **spustit**, **Zastavit**, nebo **restartujte**. Všimněte si, že možnosti nabídky kontextově, tak můžete zastavit funkční webovou aplikaci nebo pouze spustí webovou aplikaci, která není spuštěna.
   
    ![Zastavení stávající webovou aplikaci][13]

## <a name="next-steps"></a>Další kroky
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [nástrojů Azure pro Eclipse]
  * [instalaci sady nástrojů Azure pro Eclipse]
  * *Vytvoření webové aplikace Hello World služby Azure v prostředí Eclipse (v tomto článku)*
  * [Novinky v sadě Azure Toolkit pro Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]
  * [Novinky v sadě Azure Toolkit pro IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

Další informace o vytváření webové aplikace Azure najdete v tématu [webových aplikací – přehled].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[instalaci sady nástrojů Azure pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Novinky v sadě Azure Toolkit pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java]: https://azure.microsoft.com/develop/java/
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
