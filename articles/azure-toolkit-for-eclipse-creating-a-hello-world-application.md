---
title: "aaaCreate Hello World cloudové služby pro Azure v prostředí Eclipse"
description: "Zjistěte, jak hello toocreate jednoduché aplikace Hello, World pomocí nástrojů Azure pro prostředí Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 7262e705-59d6-43ce-b888-29a21c8e0cb7
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: dfb81374aaf78e933c0bf83a1dbd98023801491a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>V prostředí Eclipse vytvořte Hello World cloudové služby pro Azure.
Hello následující kroky ukazují, jak toocreate a nasazení základní tooAzure aplikaci JSP pomocí hello Azure Toolkit pro Eclipse. Příklad JSP je uvedený na jednoduchost, ale vysoce podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.

aplikace Hello bude vypadat podobně jako toohello následující:

![][ic600360]

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), v 1.7 nebo novější.
* Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, džínovinu nebo novější. To si můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Distribuce založené na jazyce Java webový server nebo aplikačního serveru, například Apache Tomcat, GlassFish, aplikační Server JBoss, Jetty nebo jádra Liberty IBM® WebSphere® aplikace serveru.
* Předplatné Azure, který můžete získat z <http://azure.microsoft.com/pricing/purchase-options/>.
* Hello nástrojů Azure pro prostředí Eclipse. Další informace najdete v tématu [hello instalace nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse].

## <a name="toocreate-a-hello-world-application"></a>toocreate aplikace Hello World
Nejdříve začneme s vytvořením projektu Java.

1. Spusťte Eclipse a v nabídce hello na **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**. (Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor**, **nový**, pak hello následující: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu...** , rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.)

1. Pro účely tohoto kurzu, pojmenujte projekt hello **MyHelloWorld**. (Ujistěte se, použijte tento název, vaše toobe soubor WAR s názvem MyHelloWorld očekávat následné kroky v tomto kurzu). Na obrazovce zobrazí podobné toohello následující:

   ![][ic589576]

1. Klikněte na **Dokončit**.

1. V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyHelloWorld**. Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).

1. V hello **nový soubor JSP** dialogové okno, název souboru hello **index.jsp**. Zachovat hello nadřazené složky jako **MyHelloWorld/WebContent**, jak ukazuje následující hello:

   ![][ic659262]

1. V hello **vybrat šablonu JSP** dialogové okno pro účely tohoto kurzu možnost **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.

1. Když hello soubor index.jsp otevře v prostředí Eclipse, přidejte v zobrazení textu toodynamically **Hello, World!** v rámci existující hello `<body>` elementu. Aktualizovaný `<body>` obsah se mají zobrazit jako hello následující:
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Uložte index.jsp.

## <a name="toodeploy-your-application-tooazure-hello-quick-and-simple-way"></a>toodeploy tooAzure aplikace, hello rychlý a jednoduchý způsob,
Jakmile máte připravené tootest Java webové aplikace, můžete použít následující zástupce tootry ho odhlašování přímo na hello Azure cloud hello.

1. V prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **MyHelloWorld**.

2. V panelu nástrojů Eclipse hello, klikněte na tlačítko hello **publikovat** rozevírací tlačítko a pak klikněte na tlačítko **publikovat jako cloudová služba Azure**

   ![][publishDropdownButton]

3. Pokud jsou publikování této aplikace tooAzure pro hello poprvé a jste dosud nevytvořili projektu nasazení Azure pro tuto aplikaci před, projektu nasazení Azure se vám vytvoří automaticky. Měli byste vidět hello následující dotaz, který také obsahuje seznam hello JDK balíčku i aplikace server, který bude automaticky nasazen toorun vaší aplikace.

   ![][ic789598]
   
   Tento přístup zástupce umožňuje rychlý a snadný způsob tootest aplikaci v Azure bez nutnosti tooconfigure konkrétní server nebo JDK, které se liší od výchozí hodnoty hello. Pokud budete spokojeni s hello výchozí hodnoty, můžete kliknout na **OK** toocontinue s hello následující kroky.
   Ale pokud chcete, aby toochange hello JDK nebo toouse serveru aplikace pro aplikaci, můžete to udělat později úpravou hello Azure nasazení projektu, který byl automaticky vytvořen pro vás, nebo můžete kliknout na **zrušit** teď a pro čtení Hello **nasazení Azure o projekty části** tohoto kurzu.

4. V hello **publikování tooAzure** dialogové okno:

   1. Pokud nejsou žádné odběry tooselect v hello **předplatné** seznam ještě postupujte podle těchto kroků tooimport informace o vašem předplatném:
      1. Klikněte na tlačítko **Import ze souboru nastavení publikování**.
      2. V hello **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **stáhnout soubor nastavení publikování**. Pokud ještě nejste přihlášeni k účtu Azure, bude výzvami toolog v. Potom budete vyzváni k souboru s nastavením publikování toosave Azure. Uložte tooyour místního počítače.
      3. Stále v hello **importovat informace o předplatném** dialogové okno, klikněte na tlačítko hello **Procházet** tlačítko, vyberte hello uložený místně v předchozím kroku hello souboru s nastavením publikování a pak klikněte na tlačítko  **Otevřete**. Na obrazovce by měl vypadat podobně jako toohello následující:![][ic644267]
      4. Klikněte na **OK**.
   2. Pro **předplatné**, vyberte předplatné hello, kterou chcete použít pro vaše nasazení.
   3. Pro **účet úložiště**, vyberte účet úložiště hello, mají toouse, nebo klikněte na tlačítko **nový** toocreate nový účet úložiště.
   4. Pro **název služby**, vyberte hello cloudovou službu, mají toouse, nebo klikněte na tlačítko **nový** toocreate novou cloudovou službu.
   5. Pro **cíl OS**vyberte hello verze hello operačního systému, kterou chcete toouse pro vaše nasazení.
   6. Pro **cílové prostředí**pro účely tohoto kurzu vyberte **pracovní**. (Pokud jste připravené toodeploy tooyour pracoviště, změníte tím příliš**produkční**.)
   7. Volitelné: Ujistěte se, že **přepsat předchozí nasazení** je zaškrtnuté políčko, pokud chcete, aby vaše nové nasazení tooautomatically přepsat hello předchozí nasazení. Když povolíte tuto možnost, není prostředí "409 – konflikt" problémy při publikování toohello stejné umístění.
      Všimněte si, že hello **publikování tooAzure** dialogové okno obsahuje oddíl pro **vzdáleného přístupu**. Ve výchozím nastavení vzdálený přístup není povolen a nebude jsme ji povolit pro tento příklad. tooenable vzdáleného přístupu, zadejte uživatelské jméno a heslo toouse při vzdálené přihlášení. Další informace o roli vzdálený přístup, najdete v části [povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse][Enabling Remote Access for Azure Deployments in Eclipse].
      Vaše **publikování tooAzure** zobrazí se dialogové okno podobné toohello následující:![][ic719488]

5. Klikněte na tlačítko **publikovat** toopublish toohello pracovní prostředí.

   Po kliknutí na výzvami tooperform úplné sestavení, **Ano**. To může trvat několik minut, než první sestavení hello.
   **Protokol činnosti Azure** se zobrazí v části zobrazení vašeho prostředí Eclipse na kartách.
   ![][ic719489]Vám může použít tento protokol, stejně jako hello **konzoly** zobrazit, toosee hello průběh nasazení. Alternativou je toolog v toohello [portálu pro správu Azure][Azure Management Portal]a použijte hello **cloudové služby** části toomonitor hello stavu.

6. Po úspěšném nasazení vaše nasazení, hello **protokol činnosti Azure** se zobrazí stav **publikováno**. Klikněte na tlačítko **publikováno**, jak je znázorněno v následujícím hello bitové kopie a prohlížeči se otevře instance vašeho nasazení.

   ![][ic719490]

Protože šlo o nasazení tooa, pracovní prostředí, bude mít název DNS hello hello formuláře http://&lt;*guid*&gt;. cloudapp.net a adresa URL hello bude obsahovat název DNS hello plus přípona pro vaši aplikaci. Například http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (hello **MyHelloWorld** část je malá a velká písmena.) Můžete také zjistit hello DNS název Pokud kliknete na název nasazení hello v hello portálu pro správu platformy Azure (v rámci cloudové služby část hello hello portálu pro správu).

I když byl tohoto návodu pro nasazení toohello, pracovní prostředí, nasazení tooproduction následuje hello stejný postup, s výjimkou v rámci hello **publikování tooAzure** dialogovém okně, vyberte **produkční** místo **pracovní** pro hello **cílové prostředí**. Nasazení tooproduction výsledkem adresu URL na základě názvu DNS hello podle vaší volby, namísto identifikátor GUID jako použité pro přípravu.

> [!WARNING]
> V tomto okamžiku jste nasadili cloudu toohello aplikaci Azure. Ale než budete pokračovat, uvědomte si, že nasazené aplikace, i když není spuštěný, bude pokračovat, tooaccrue fakturovatelný čas pro vaše předplatné. Proto je velmi důležité, odstranit nežádoucí nasazení z vašeho předplatného Azure.
> 
> 

## <a name="about-azure-deployment-projects"></a>O projektech nasazení Azure
V pořadí toodeploy jeden nebo více tooAzure aplikace Java, je potřeba projektu nasazení aplikace Azure. Ho hraje roli hello hello "balíček", který vaše aplikace potřebují toobe zabalené do v pořadí toobe publikovat na platformě Azure.

Kromě hello informace o aplikacích, projektu nasazení Azure také obsahuje informace o dalších klíčové komponenty vašeho nasazení co je nejdůležitější: hello vaší webové aplikace v aplikaci server kontejneru toorun a hello Java runtime toorun ho na. Azure podporuje velké výběr Java moduly runtime a Java aplikační servery, které můžete vybrat z.

Přestože použít zde je příklad hello je výrazně jednodušší pro vzdělávací účely, projektu nasazení Azure může také obsahovat další důležité konfigurační informace, které vám umožní toocreate téměř libovolně komplexní, škálovatelné a vysoce dostupný, vícevrstvé cloudové služby s vašimi aplikacemi. Můžete povolit **spřažení relace (dále jen "trvalé relace")**, **rychlé ukládání do mezipaměti**, **snižování zátěže protokolu SSL**, **port brány firewall nebo směrování**, **vzdáleného přístupu**a počet jiných výkonné možnosti.

Pokud jste dokončili hello předchozí části tohoto kurzu ("toodeploy tooAzure aplikace, hello rychlý a jednoduchý způsob"), se nyní zobrazí nový projekt nasazení Azure v hello Project Exploreru vygenerovány pro vás automaticky a s názvem " **MyHelloWorld_onAzure**".

Může také začnete tento kurz nejprve vytvoříte projekt prázdné nasazení Azure sami a následným přidáním tooit vaší aplikace. Je delší proces, ale poskytuje další kontrolu nad hello počáteční konfigurace ze začátku hello.

toocreate nový projekt nasazení Azure od začátku, klikněte na tlačítko hello **nový projekt nasazení Azure** tlačítko ![][ic710876].

Bez ohledu na to, zda funguje s již existující projekt nasazení Azure, nebo vytvořit od začátku, jsou možné toochange jeho nastavení konfigurace a součásti, například hello JDK nebo hello aplikační server, stejně snadno kdykoli.

toochange hello JDK, nebo hello aplikační server nebo seznam aplikací hello v existujícího projektu nasazení Azure:

1. Rozbalte uzel projektu hello (například **MyHelloWorld_onAzure**) v prohlížeči projektu hello

2. Klikněte pravým tlačítkem na **WorkerRole1**

3. Rozbalte hello **Azure** podnabídky v kontextové nabídce hello

4. Klikněte na tlačítko **konfigurace serveru**

Bez ohledu na to zda jste spustili tyto kroky konfigurace serveru tak, že upravíte existující projekt nasazení Azure, jako v příkladu nahoře, nebo vytvořením nového od začátku, zobrazí se hello stejný typ dialogová okna, což vám tooconfigure JDK, serverů a aplikací komponenty. toolearn více jak toochange hello nastavení v těchto dialogová okna, například toochange hello JDK, hello aplikační server a přidat nebo odebrat aplikace v nasazení, najdete v části hello [vlastnosti konfigurace serveru] [ Server configuration properties] článku.

## <a name="windows-only-toodeploy-your-application-toohello-compute-emulator"></a>Pouze v systému Windows: toodeploy emulátor služby výpočty toohello vaší aplikace

> [!NOTE]
> Hello Azure emulátor je k dispozici pouze v systému Windows. Tuto část přeskočte, pokud používáte operační systém než Windows.
> 
> 

Pokud jste vytvořili nový projekt nasazení Azure hello postupu popsaného dříve, tj. implicitně publikování tooAzure vaší aplikace hello JDK a aplikační servery jsou nakonfigurované pro hello cloud, ale ne pro místní emulace. tooprepare svůj projekt pro testování v emulátoru místního hello, postupujte takto:

1. V prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **MyHelloWorld_onAzure**.

2. Klikněte pravým tlačítkem na **WorkerRole1**.

3. Rozbalte hello **Azure** podnabídky v kontextové nabídce hello.

4. Klikněte na tlačítko **konfigurace serveru**.

5. Na hello **JDK** kartě, zkontrolujte, zda text hello toolkit předem nakonfigurován výchozí místní JDK za vás. Pokud ne, nebo pokud chcete toochange hello předpokládá, že výchozí hodnoty, ujistěte se, že hello **použití hello JDK z tuto cestu k souboru pro místní testování** je zaškrtnuté políčko a hello JDK umístění instalace, které chcete toouse je zadán. Pokud chcete, aby toochange, klikněte na tlačítko hello **Procházet** tlačítko a pomocí řízení hello procházet, vyberte umístění adresáře hello hello JDK toouse.

6. Klikněte na tlačítko hello **Server** kartě.

7. V hello **místní server cesta** textového pole v dolní části hello hello dialogového okna, zadejte cestu hello místně nainstalován server, který odpovídá typu hello a hlavní číslo verze serveru hello vybrané v horní části hello hello dialogového okna, v části Hello **nasadit server tohoto typu** zaškrtávací políčko. Pokud chcete toouse na jiný typ nebo hlavní verzi hello aplikační server, změňte výběr hello pod toto zaškrtávací políčko nejdřív.

8. Klikněte na **OK**.

9. V panelu nástrojů Eclipse hello, klikněte na tlačítko hello **spustit v emulátoru Azure** tlačítko ![][ic710879]. Pokud hello **spustit v emulátoru Azure** není dostupné tlačítko, ujistěte se, že **MyHelloWorld_onAzure** je vybrán v prohlížeči projektu Eclipse společnosti a zkontrolujte, zda je Eclipse Project Exploreru jako hello aktuální okno. Tím prvním spuštění úplné sestavení projektu a spusťte svoji webovou aplikaci Java v emulátoru služby výpočty hello. (Všimněte si, že v závislosti na vlastnosti výkonu počítače, hello první sestavení může trvat mezi několik sekund tooa několik minut, ale následné sestavení získají rychlejší.) Po dokončení hello první krok sestavení, zobrazí se výzva nástrojem Řízení uživatelských účtů (UAC) tooallow toomake tento příkaz změní tooyour počítače. Klikněte na **Ano**.

> [!IMPORTANT]
> Pokud se nezobrazí výzva, zkontrolujte, hello nástroje Řízení uživatelských účtů hello hlavního panelu Windows hello ikony nástroje Řízení uživatelských účtů a klikněte na první. Někdy hello výzva nástroje Řízení uživatelských účtů nezobrazuje jako nejhornější okna, ale je viditelná pouze jako na ikonu hlavního panelu.
> 
> 

1. Zkontrolujte výstup hello hello výpočetní emulátor uživatelského rozhraní toodetermine, pokud jsou nějaké problémy s projektem. V závislosti na hello obsah vašeho nasazení může trvat několik minut, než se vaše aplikace toobe plně spustila v emulátoru služby výpočty hello.

2. Spusťte prohlížeč a použijte adresu URL hello `http://localhost:8080/MyHelloWorld` jako adresa hello (hello `MyHelloWorld` část adresy URL hello je malá a velká písmena). Měli byste vidět MyHelloWorld aplikace (hello výstup index.jsp), podobně jako toohello následující bitové kopie:

   ![][ic589579]

Po připravené toostop aplikace spuštěné v emulátoru služby výpočty hello, v panelu nástrojů Eclipse hello, klikněte na hello **resetovat emulátoru Azure** tlačítko ![][ic710880].

## <a name="toodelete-your-deployment"></a>toodelete nasazení
toodelete nasazení v rámci hello nástrojů Azure pro prostředí Eclipse, zajistěte, aby **MyHelloWorld_onAzure** je vybrali v prohlížeči projektu na Eclipse, ujistěte se, hello prohlížeči projektu Eclipse má hello aktuální okno zaměřit a pak klikněte na Hello **zrušit publikování** tlačítko ![][ic710883], v panelu nástrojů Eclipse hello. (Můžete to udělat hello stejné operace kliknutím pravým tlačítkem na **MyHelloWorld_onAzure** v prohlížeči projektu na Eclipse, kliknutím na tlačítko **Azure** a pak levým na **Undeploy z cloudu Azure**.) Bude se zobrazovat hello **zrušit publikování projektu Azure** dialogové okno.

![][ic719491]

Vyberte hello předplatného a cloud službu, která obsahuje vaše nasazení, vyberte hello nasazení má toodelete a pak klikněte na **zrušit publikování**.

(K alternativní toousing hello toolkit toodelete hello nasazení je toouse hello **cloudové služby** části hello Azure Management Portal: přejděte tooyour nasazení, vyberte ho a pak klikněte na tlačítko hello **odstranit** tlačítko. To bude zastavte a odstraňte hello nasazení. Pokud chcete pouze toostop hello nasazení a k jeho odstranění, klikněte na tlačítko hello **Zastavit** tlačítko místo hello **odstranit** tlačítko jak ale zmiňujeme výše, pokud neodstraníte hello nasazení, fakturovatelný poplatky budou Pokračujte tooaccrue pro vaše nasazení i v případě, že je zastaven).

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace hello nástrojů Azure pro Eclipse][Installing hello Azure Toolkit for Eclipse] 

[Co je nového v hello nástrojů Azure pro Eclipse][What's New in hello Azure Toolkit for Eclipse]

Další informace o používání Azure v jazyce Java, najdete v tématu hello [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->
