---
title: "V prostředí Eclipse vytvořte Hello World cloudové služby pro Azure."
description: "Zjistěte, jak vytvořit jednoduchou aplikaci Hello World pomocí nástrojů Azure pro Eclipse."
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
ms.openlocfilehash: 9b31f0faeb6ee7b5e7b8fe3a1f2827133d6188e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>V prostředí Eclipse vytvořte Hello World cloudové služby pro Azure.
Následující kroky ukazují, jak vytvořit a nasadit základní aplikaci JSP do Azure pomocí sady nástrojů pro Azure pro Eclipse. Příklad JSP je uvedený na jednoduchost, ale vysoce podobným způsobem může být vhodné pro Java servlet, co se týče nasazení Azure.

Aplikace bude vypadat nějak takto:

![][ic600360]

## <a name="prerequisites"></a>Požadavky
* Java Developer Kit (JDK), v 1.7 nebo novější.
* Integrované vývojové prostředí Eclipse pro vývojáře v jazyce Java EE, džínovinu nebo novější. To si můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Distribuce založené na jazyce Java webový server nebo aplikačního serveru, například Apache Tomcat, GlassFish, aplikační Server JBoss, Jetty nebo jádra Liberty IBM® WebSphere® aplikace serveru.
* Předplatné Azure, který můžete získat z <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure nástrojů pro Eclipse. Další informace najdete v tématu [instalaci sady nástrojů Azure pro Eclipse][Installing the Azure Toolkit for Eclipse].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Hello World
Nejdříve začneme s vytvořením projektu Java.

1. Spusťte Eclipse a v nabídce klikněte na položku **soubor**, klikněte na tlačítko **nový**a potom klikněte na **Dynamic Web Project**. (Pokud nevidíte **Dynamic Web Project** uvedené jako dostupné projekt po kliknutí na **soubor**, **nový**, postupujte takto: klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**, klikněte na tlačítko **projektu...** , rozbalte položku **webové**, klikněte na tlačítko **Dynamic Web Project**a klikněte na tlačítko **Další**.)

1. Pro účely tohoto kurzu, název projektu **MyHelloWorld**. (Ujistěte se, použijte tento název, váš soubor WAR s názvem MyHelloWorld očekávat následné kroky v tomto kurzu). Na obrazovce se zobrazí podobná této:

   ![][ic589576]

1. Klikněte na **Dokončit**.

1. V rámci zobrazení Eclipse na prohlížeči projektu rozbalte **MyHelloWorld**. Klikněte pravým tlačítkem na **WebContent**, pak na **New** (Nový) a nakonec na **JSP File** (Soubor JSP).

1. V **nový soubor JSP** dialogové okno, název souboru **index.jsp**. Nadřazený adresář ponechte na jako **MyHelloWorld/WebContent**, jak je znázorněno v následujícím:

   ![][ic659262]

1. V **vybrat šablonu JSP** dialogové okno pro účely tohoto kurzu možnost **nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.

1. Když se soubor index.jsp otevře v prostředí Eclipse, přidejte text dynamicky zobrazíte **Hello, World!** do existujícího elementu `<body>`. Aktualizovaný `<body>` obsah se mají zobrazit jako následující:
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Uložte index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Pro nasazení aplikace do Azure, rychlý a jednoduchý způsob
Jakmile máte webové aplikace Java test připraven, můžete následující klávesovou zkratku vyzkoušejte ji přímo na cloudu Azure.

1. V prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **MyHelloWorld**.

2. Na panelu nástrojů Eclipse klikněte **publikovat** rozevírací tlačítko a pak klikněte na tlačítko **publikovat jako cloudová služba Azure**

   ![][publishDropdownButton]

3. Pokud jsou publikování této aplikace do Azure poprvé a jste dosud nevytvořili projektu nasazení Azure pro tuto aplikaci před, projektu nasazení Azure se vám vytvoří automaticky. Měli byste vidět následující řádek, který také obsahuje seznam balíčků JDK a aplikační server, který se automaticky nasadí do spusťte aplikaci.

   ![][ic789598]
   
   Tento přístup zástupce umožňuje rychlý a snadný způsob, jak otestovat aplikaci v Azure, aniž by bylo nutné konfigurovat konkrétní server nebo JDK, které se liší od výchozí hodnoty. Pokud budete spokojeni s výchozí hodnoty, můžete kliknout na **OK** Chcete-li pokračovat pomocí následujících kroků.
   Ale pokud chcete změnit sadu JDK nebo aplikačního serveru chcete použít pro aplikace, můžete to udělat později úpravou projekt nasazení Azure, který byl automaticky vytvořen pro vás, nebo můžete kliknout na **zrušit** teď a čtení  **O nasazení Azure projekty** tohoto kurzu.

4. V **publikovat do Azure** dialogové okno:

   1. Pokud nejsou žádná předplatná a vybrat v **předplatné** seznam ještě použijte následující postup importovat informace o vašem předplatném:
      1. Klikněte na tlačítko **Import ze souboru nastavení publikování**.
      2. V **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **stáhnout soubor nastavení publikování**. Pokud ještě nejste přihlášeni k účtu Azure, budete vyzváni k přihlášení. Potom budete vyzváni k uložení Azure soubor nastavení publikování. Uložte ho do místního počítače.
      3. Pořád ještě v **importovat informace o předplatném** dialogové okno, klikněte na tlačítko **Procházet** tlačítko, vyberte soubor nastavení publikování, který jste uložili místně v předchozím kroku a pak klikněte na tlačítko **otevřete**. Na obrazovce by měl vypadat takto:![][ic644267]
      4. Klikněte na **OK**.
   2. Pro **předplatné**, vyberte předplatné, kterou chcete použít pro vaše nasazení.
   3. Pro **účet úložiště**, vyberte účet úložiště, který chcete použít, nebo klikněte na tlačítko **nový** k vytvoření nového účtu úložiště.
   4. Pro **název služby**, vyberte cloudovou službu, kterou chcete použít, nebo klikněte na tlačítko **nový** vytvořit novou cloudovou službu.
   5. Pro **cíl OS**, vyberte verzi operačního systému, který chcete použít pro vaše nasazení.
   6. Pro **cílové prostředí**pro účely tohoto kurzu vyberte **pracovní**. (Až budete připraveni k nasazení do produkční lokality, budete k změnit **produkční**.)
   7. Volitelné: Ujistěte se, že **přepsat předchozí nasazení** je zaškrtnuté políčko, pokud chcete, aby vaše nové nasazení k automatickému přepsání předchozí nasazení. Když povolíte tuto možnost, bude se při publikování do stejného umístění, není prostředí "409 – konflikt" problémy.
      Všimněte si, že **publikovat do Azure** dialogové okno obsahuje oddíl pro **vzdáleného přístupu**. Ve výchozím nastavení vzdálený přístup není povolen a nebude jsme ji povolit pro tento příklad. Postup povolení vzdáleného přístupu, by zadejte uživatelské jméno a heslo pro použití při vzdálené přihlášení. Další informace o roli vzdálený přístup, najdete v části [povolení vzdáleného přístupu pro Azure nasazení v prostředí Eclipse][Enabling Remote Access for Azure Deployments in Eclipse].
      Vaše **publikovat do Azure** zobrazí se dialogové okno podobné následujícím:![][ic719488]

5. Klikněte na tlačítko **publikovat** publikovat do pracovního prostředí.

   Po zobrazení výzvy k provedení úplné sestavení, klikněte na tlačítko **Ano**. To může trvat několik minut, než první sestavení.
   **Protokol činnosti Azure** se zobrazí v části zobrazení vašeho prostředí Eclipse na kartách.
   ![][ic719489]Tento protokol, můžete použít společně s **konzoly** zobrazení, abyste viděli průběh nasazení. Alternativou je do protokolu [portálu pro správu Azure][Azure Management Portal]a použít **cloudové služby** části k monitorování stavu.

6. Po úspěšném nasazení vaše nasazení, **protokol činnosti Azure** se zobrazí stav **publikováno**. Klikněte na tlačítko **publikováno**, jak je znázorněno na následujícím obrázku, a prohlížeči se otevře instance vašeho nasazení.

   ![][ic719490]

Protože to byla nasazení pro pracovní prostředí, bude mít název DNS ve tvaru http://&lt;*guid*&gt;. cloudapp.net a bude adresa URL obsahovat název DNS a přípony pro vaši aplikaci. Například http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. ( **MyHelloWorld** část je malá a velká písmena.) Taky uvidíte název DNS Pokud kliknete na název nasazení v portálu pro správu platformy Azure (v části cloudové služby na portálu pro správu).

I když byl tohoto návodu pro nasazení, který je pracovní prostředí, nasazení do produkčního prostředí postupuje stejně, s výjimkou v rámci **publikovat do Azure** dialogovém okně, vyberte **produkční** místo **Pracovní** pro **cílové prostředí**. Nasazení do produkčního prostředí výsledkem adresu URL na základě názvu DNS podle vaší volby, namísto identifikátor GUID jako použité pro přípravu.

> [!WARNING]
> V tomto okamžiku jste nasadili aplikaci Azure do cloudu. Nicméně než budete pokračovat, uvědomte si, že nasazené aplikace, i když není spuštěna, budou dál nabíhat fakturovatelný čas pro vaše předplatné. Proto je velmi důležité, odstranit nežádoucí nasazení z vašeho předplatného Azure.
> 
> 

## <a name="about-azure-deployment-projects"></a>O projektech nasazení Azure
Abyste mohli nasadit jeden nebo více aplikací v jazyce Java do Azure, je potřeba projektu nasazení aplikace Azure. Ho hraje roli "balíček", který vaše aplikace musí být uzavřen do, aby bylo možné publikovat na platformě Azure.

Kromě informace o aplikacích, projektu nasazení Azure také obsahuje informace o dalších klíčové komponenty vašeho nasazení co je nejdůležitější: kontejneru aplikace server ke spouštění vaší webové aplikace v a Java runtime spuštění na. Azure podporuje velké výběr Java moduly runtime a Java aplikační servery, které můžete vybrat z.

Přestože použít zde je příklad je zjednodušený výrazně pro vzdělávací účely, projektu nasazení Azure může také obsahovat další důležité konfigurační informace, které vám umožní vytvořit téměř libovolně komplexní, škálovatelné a vysoce dostupný, vícevrstvé cloudové služby s vašimi aplikacemi. Můžete povolit **spřažení relace (dále jen "trvalé relace")**, **rychlé ukládání do mezipaměti**, **snižování zátěže protokolu SSL**, **port brány firewall nebo směrování**, **vzdáleného přístupu**a počet jiných výkonné možnosti.

Pokud po dokončení předchozí části tohoto kurzu ("pro nasazení aplikace do Azure, rychlý a jednoduchý způsob"), se nyní zobrazí nový projekt nasazení Azure v prohlížeči projektu vygenerovány pro vás automaticky a s názvem " **MyHelloWorld_onAzure**".

Může také začnete tento kurz nejprve vytvoříte projekt prázdné nasazení Azure sami a následným přidáním vaší aplikace do ní. Je delší proces, ale poskytuje další kontrolu nad počáteční konfigurace od začátku.

Chcete-li vytvořit nový projekt nasazení Azure od začátku, klikněte na tlačítko **nový projekt nasazení Azure** tlačítko ![][ic710876].

Bez ohledu na to, zda funguje s již existující projekt nasazení Azure, nebo vytvořit od začátku, budete moci změnit jeho nastavení konfigurace a součásti, například sadu JDK nebo aplikačního serveru, stejně snadno kdykoli.

Chcete-li změnit sadu JDK, nebo je aplikační server nebo seznamu aplikací v existujícího projektu nasazení Azure:

1. Rozbalte uzel projektu (například **MyHelloWorld_onAzure**) v prohlížeči projektu

2. Klikněte pravým tlačítkem na **WorkerRole1**

3. Rozbalte **Azure** podnabídky v místní nabídce

4. Klikněte na tlačítko **konfigurace serveru**

Bez ohledu na tom, jestli jste spustili tyto kroky konfigurace serveru tak, že upravíte existující projekt nasazení Azure, jako v příkladu nahoře, nebo vytvořením nové od začátku, zobrazí se stejný typ lze konfigurovat JDK, serveru a aplikace dialogová okna komponenty. Další informace o tom, jak změnit nastavení v těchto dialogová okna, například když chcete změnit sadu JDK, aplikační server a přidat nebo odebrat aplikace v nasazení, najdete v článku [vlastnosti konfigurace serveru] [ Server configuration properties] článek.

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Pouze v systému Windows: nasazení aplikace na emulátoru služby výpočty v

> [!NOTE]
> Azure emulátor je k dispozici pouze v systému Windows. Tuto část přeskočte, pokud používáte operační systém než Windows.
> 
> 

Pokud jste vytvořili nový projekt nasazení Azure kroků popsaných výše, tj. implicitně publikováním aplikace Azure, JDK a aplikace konfiguraci pro cloud, ale ne pro místní emulace. Příprava projektu pro testování v místní emulátoru, postupujte takto:

1. V prohlížeči projektu Eclipse společnosti, klikněte na tlačítko **MyHelloWorld_onAzure**.

2. Klikněte pravým tlačítkem na **WorkerRole1**.

3. Rozbalte **Azure** podnabídky v místní nabídce.

4. Klikněte na tlačítko **konfigurace serveru**.

5. Na **JDK** kartě, zkontrolujte, zda sada nástrojů předem nakonfigurován výchozí místní JDK za vás. Pokud ne, nebo pokud chcete změnit předpokládané výchozí hodnoty, zkontrolujte, zda **použít sadu JDK z tuto cestu k souboru pro místní testování** je zaškrtnuté políčko a je zadané umístění instalace JDK, kterou chcete použít. Pokud chcete změnit, klikněte **Procházet** tlačítko a pomocí ovládacího prvku procházet, vyberte umístění adresáře JDK používat.

6. Klikněte **Server** kartě.

7. V **místní server cesta** textového pole v dolní části dialogové okno, zadejte cestu k serveru místně nainstalován, který odpovídá typu a hlavní číslo verze serveru vybrané v horní části dialogové okno, v části  **Nasazení serveru tohoto typu** zaškrtávací políčko. Pokud chcete použít jiný typ nebo hlavní verzi aplikačního serveru, nejprve změňte výběr pod toto zaškrtávací políčko.

8. Klikněte na **OK**.

9. Na panelu nástrojů Eclipse klikněte **spustit v emulátoru Azure** tlačítko ![][ic710879]. Pokud **spustit v emulátoru Azure** není dostupné tlačítko, ujistěte se, že **MyHelloWorld_onAzure** je vybrán v prohlížeči projektu Eclipse společnosti a zkontrolujte, zda je Eclipse Project Exploreru jako aktuální okno. Nejprve se spustit úplné sestavení projektu a spusťte svoji webovou aplikaci Java v emulátoru služby výpočty v. (Všimněte si, že v závislosti na vlastnosti výkonu počítače, první sestavení může trvat od několika sekund až několik minut, ale následné sestavení získají rychlejší.) Po dokončení první krok sestavení, vás vyzve ve Windows řízení uživatelských účtů (UAC) umožňuje tento příkaz k provedení změny v počítači. Klikněte na **Ano**.

> [!IMPORTANT]
> Pokud se nezobrazí výzva nástroje Řízení uživatelských účtů, zkontrolujte na hlavním panelu Windows ikony nástroje Řízení uživatelských účtů a klikněte na první. Někdy UAC řádku nezobrazuje jako nejhornější okna, ale je viditelná pouze jako na ikonu hlavního panelu.
> 
> 

1. Prohlédněte si výstup emulátoru služby výpočty v Uživatelském určí, jestli jsou všechny problémy s projektem. V závislosti na jeho obsah vašeho nasazení může trvat několik minut pro vaši aplikaci v emulátoru služby výpočty v plně spustit.

2. Spusťte prohlížeč a použijte adresu URL `http://localhost:8080/MyHelloWorld` jako adresa ( `MyHelloWorld` část adresy URL je malá a velká písmena). Měli byste vidět aplikaci MyHelloWorld (výstup index.jsp), podobně jako na následujícím obrázku:

   ![][ic589579]

Až budete připraveni k zastavení aplikace spuštěná v emulátoru služby výpočty v, na panelu nástrojů Eclipse klikněte **resetovat emulátoru Azure** tlačítko ![][ic710880].

## <a name="to-delete-your-deployment"></a>Chcete-li odstranit nasazení
Pokud chcete odstranit nasazení v rámci sady nástrojů Azure pro prostředí Eclipse, ujistěte se, že **MyHelloWorld_onAzure** je vybrali v prohlížeči projektu na Eclipse, ujistěte se, prohlížeči projektu Eclipse má aktuální okno zaměřit a potom klikněte  **Zrušit publikování** tlačítko ![][ic710883], na panelu nástrojů Eclipse. (Můžete to udělat kliknutím pravým tlačítkem na stejné operace **MyHelloWorld_onAzure** v prohlížeči projektu na Eclipse, kliknutím na **Azure** a potom kliknutím na **Undeploy z cloudu Azure**.) Bude se zobrazovat **zrušit publikování projektu Azure** dialogové okno.

![][ic719491]

Vyberte předplatné a cloudovou službu, která obsahuje vaše nasazení, vyberte nasazení, které chcete odstranit a potom klikněte na **zrušit publikování**.

(Alternativu k použití sady nástrojů odstranění nasazení se má používat **cloudové služby** části portálu pro správu Azure: přejděte na vaše nasazení, vyberte ho a klikněte **odstranit** tlačítko. To zastaví a pak odstraníte nasazení. Pokud chcete pouze zastavit nasazení a k jeho odstranění, klikněte na tlačítko **Zastavit** tlačítko místo **odstranit** tlačítko, ale jako uvedených výše, pokud neodstraníte nasazení, budou fakturovatelný poplatky dál nárůst pro vaše nasazení i v případě, že je zastaven).

## <a name="see-also"></a>Viz také
[Azure nástrojů pro Eclipse][Azure Toolkit for Eclipse]

[Instalace Azure Toolkit pro Eclipse][Installing the Azure Toolkit for Eclipse] 

[Co je nového v sadě Azure nástrojů pro Eclipse][What's New in the Azure Toolkit for Eclipse]

Další informace o používání Azure s Java najdete v tématu [Azure střediska pro vývojáře Java][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

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
