---
title: "Kurz: DevOps s hello portálu Azure | Microsoft Docs"
description: "Další informace hello různých pracovních postupů DevOps v hello portálu Azure."
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: 4c32dbbd4e4b1c3809ef4b01e1496e350183ebde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-devops-with-hello-azure-portal"></a>Kurz: DevOps s hello portálu Azure
Hello platformy Azure je plná flexibilní DevOps pracovních postupů. V tomto kurzu zjistíte, jak tooleverage hello možnosti hello toodevelop portálu Azure, testování, nasazení, řešení potíží s, sledovat a spravovat aplikace spuštěné. Tento kurz se zaměřuje na hello následující:

1. Vytvoření webové aplikace a povolení průběžného nasazování
2. Vývoj a testování aplikace
3. Sledování a řešení potíží s aplikací
4. Úlohy správy obecné aplikace

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a>Vytvoření webové aplikace a povolení průběžného nasazování
Vytvoření webové aplikace s [Azure App Service](https://azure.microsoft.com/services/app-service/), které budete používat v hello zbytek tohoto kurzu. Na začátku povolíte průběžné nasazování z úložiště zdrojového kódu do našeho spuštěného prostředí Azure.

1. Přihlaste se k portálu Azure hello
2. Zvolte **App Services** &gt; **ikonu Přidat** a zadejte název, vyberte předplatné a vytvořte nové tooserve skupiny prostředků jako hello kontejneru služby hello.
   
   Skupiny prostředků můžete povolit toomanage různé aspekty řešení hello například k fakturaci, nasazení a sledování všech jako jednu skupinu prostřednictvím [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
   
   ![image1][image1]
3. Za malou chvíli se vytvoří vaše aplikační služba. Trvat několik minut tooexplore hello různé možnosti nabídky služby hello hello portálu.
   
   ![image2][image2]    
4. Klikněte na adresu URL hello. Všimněte si hello různé dostupné možnosti nástroje a úložiště. Můžete také použít hello jazyků a rozhraní zvoleného včetně .NET, Java a Ruby.
   
   ![image3][image3]    
5. Hello portál Azure umožňuje průběžné nasazování jednoduchý proces, který zahrnuje několik jednoduchých kroků. V hello portálu Azure zvolte nastavení z hello ikony pro hello app service, kterou jste právě vytvořili.
   
   ![image4][image4]
   
   V okně hello, které se otevře na hello správné posuňte toohello publikování části.
   
   ![image5][image5]
6. V dalším kroku nakonfigurujte některé nastavení tooenable průběžné nasazování pro aplikaci hello. Klikněte na Zdroj nasazení a potom klikněte na tlačítko Vybrat zdroj. Všimněte si hello různé možnosti, které máte pro úložiště zdroje.
   
   ![image6][image6]
7. V tomto příkladu vyberte GitHub. Volitelně vyberte hello úložiště podle vašeho výběru a nastavit přihlašovací údaje pro autorizaci hello.
   
   ![image7][image7]
8. Po povolení tooyour úložiště pak můžete projekt a chcete toodeploy firemní pobočky. Dole je několik smyšlených příkladů.
   
   ![image8][image8]
9. Když si vyberete projekt a větev, klikněte na OK. Měli byste začít toosee oznámení o nasazení.
   
   ![image9][image9]
10. Přejděte zpět tooGitHub toosee hello webhooku, který byl vytvořený toointegrate hello zdroj ovládacího prvku úložišti Azure. Hello portálu Azure umožňuje integraci s Githubu se jenom pár jednoduchých kroků.
    
    ![image10][image10]
11. průběžné nasazování toodemonstrate, rychle přidáte některé obsahu toohello úložiště. Jednoduchý příklad přidejte úložiště GitHub tooa ukázkový textový soubor. Jste volné toouse .NET, Ruby, Python nebo jiný typ aplikace se App Service. Myslíte, že volné tooadd s textovým souborem, ASP.NET MVC, Java nebo Ruby úložišti toohello aplikace podle vašeho výběru.
    
    ![image11][image11]
12. Po potvrzení změn tooyour úložiště, uvidíte nový zahájit nasazení v oblasti portálu oznámení hello. Pokud se nezobrazí rychle změny po potvrzení tooyour úložiště, klikněte na tlačítko synchronizovat.
    
    ![image12][image12]
13. V tomto okamžiku Pokud akci a načíst stránku hello hello aplikace služby, může se zobrazit Chyba 403. V tomto příkladu je vzhledem k tomu, že žádné nastavení typický výchozí dokument pro stránku hello jako soubor jako index.htm nebo default.html. Můžete to rychle napravit s hello tooling v hello portálu Azure.  V hello portálu Azure vyberte nastavení &gt; nastavení aplikace.
    
     ![image13][image13]
14. Otevře se okno s nastavením aplikace. Zadejte název hello hello stránky "SamplePage.html" a klikněte na Uložit. Trvat několik minut tooexplore hello další nastavení.
    
    ![image14][image14]
15. Volitelně můžete aktualizujte vaše tooensure adresu URL prohlížeče zobrazí hello očekává změny. V takovém případě není jednoduchý text teď naplnění stránku hello. Každý další změny toohello úložiště by způsobilo nové automatického nasazení.
    
    ![image15][image15]
    
    Povolení průběžné nasazování pomocí hello portál Azure je snadný prostředí. Můžete také vytvořit složitější kanálů verze a použít mnoho jinými technikami s existující zdrojového kódu a průběžnou integraci tooAzure toodeploy systémy, jako je například využití automatizované sestavení a systémy správy verzí.

## <a name="develop-and-test-an-app"></a>Vývoj a testování aplikace
V dalším kroku provedeme některé změny kódu toohello základní a rychle nasadit tyto změny. Bude také nastavit některé testování výkonu pro hello webové aplikace.

1. V hello portálu Azure App Services vybírat hello navigačním podokně a vyhledejte App Service.
   
   ![image16][image16]
2. Klikněte na Nástroje
   
   ![image17][image17]
3. Všimněte si hello vyvíjet kategorie v nabídce Nástroje. Existuje několik užitečné nástroje zde která nám umožňují toowork s aplikací bez opuštění hello portálu Azure. Klikněte na Konzolu.
   
   ![image18][image18]
4. V okně konzoly hello můžete vydávat za provozu příkazy pro vaši aplikaci. Typ hello dir příkaz a stiskněte klávesu enter. Všimněte si, že příkazy, které potřebují zvýšená oprávnění, nebudou fungovat.
   
   ![image19][image19]
5. Přesunout zpět toohello vývoj kategorie a zvolte Visual Studio Online. Poznámka: Visual Studio Online je teď jmenuje Visual Studio Team Services.
   
   ![image20][image20]
6. Přepnutí na hello prostředí úprav v prohlížeči pro vaši aplikaci.
   
   ![image21][image21]
7. Nainstaluje se webové rozšíření pro vaši aplikaci. Rozšíření rychle a snadno přidat funkce tooapps v Azure. Všimněte si některé hello jiné typy rozšíření k dispozici v následující snímek obrazovky hello.
   
   ![image22][image22]
8. Po instalaci sady Visual Studio Online rozšíření hello kliknutím na Přejít.
   
   ![image23][image23]
9. Prohlížeč kartě otevře, kde uvidíte vývoj IDE přímo v prohlížeči hello. V prohlížeči Chrome je prostředí hello oznámení níže.
   
   ![image24][image24]
10. Můžete provést několik aktivity, například upravit soubory, přidat soubory a složky a stahovat obsah z hello živý web. Zkontrolujte soubor SamplePage.html toohello rychlé upravit.
    
    ![image25][image25]
11. Ve chvíli se automaticky uloží změny hello. Pokud přejdete zpět toohello stránky, uvidíte hello změny. Nezapomeňte, že takovéto živé úpravy, nejspíš nejsou vhodné pro produkční prostředí. Nástroje hello však provádějte je velmi snadné toomake rychlé změny pro vývojáře a testovací prostředí.
    
    ![image26][image26]
    
    ![image27][image27]
12. Přesunout zpět toohello okno nástroje a v kategorii hello vývoj, klikněte na Test výkonnosti.
    
    ![image28][image28]
13. Je nutné tooset účet team services. Podrobnosti najdete tady: [Vytvoření účtu služby Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
14. Klikněte na nový toocreate testu výkonnosti.
    
    ![image29][image29]
    
    Konfigurace hello různé hodnoty a klikněte na tlačítko Spustit Test dole hello hello dialogu tooinitiate testu výkonnosti.
    
    ![image30][image30]
    
    ![image31][image31]
15. Jakmile hello testovací spuštění, můžete monitorovat stav hello.
    
    ![image32][image32]
    
    Po dokončení testu hello kliknutím na hello výsledek obsahuje další podrobnosti.
    
    ![image33][image33]
16. V tomto příkladu jste vytvořili malý test spustit, je omezený data tooanalyze, takže je může zobrazit různé metriky, stejně jako znovu spustit test z tohoto zobrazení. Hello portálu Azure umožňuje vytváření, provádění a analýza testy výkonnosti webu jednoduchý proces. snímky obrazovky Hello níže zobrazte údaje o výkonu hello.
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a>Sledování a řešení potíží s aplikací
Azure poskytuje mnoho funkcí pro sledování a řešení potíží se spuštěnými aplikacemi.

1. V hello portál Azure pro webovou aplikaci příkaz nástroje.
   
   ![image37][image37]
2. V kategorii hello Poradce při potížích Všimněte si hello různé možnosti pro použití nástroje tootroubleshoot potenciální problémy s spuštěné aplikaci. Můžete třeba monitorovat živé přenosy HTTP, povolit samoopravování, zobrazit protokoly a další.
   
   ![image38][image38]
3. Vyberte zobrazení některé kódy HTTP get tooquickly metriky lokality.
   
   ![image39][image39]
4. Vyberte Diagnostics as a Service. Vyberte typ své aplikace, pak klikněte na tlačítko Spustit.
   
   ![image40][image40]
   
   Začne sběr.
   
   ![image41][image41]
5. Můžete se rozhodnout hello příslušný protokol toodiagnose potenciální problémy. Je nutné toosee protokolování tooenable všechna dostupná data hello možnosti, jako jsou protokoly HTTP.
   
   ![image42][image42]
   
   Kliknutím na soubor výpis stavu paměti hello můžete stáhnout a analyzovat nástroj DebugDiag toohelp sestavy analýzy najít potenciální problémy.
   
   ![image43][image43]
6. tooview více dat, budete potřebovat další protokolování tooenable. V hello portálu Azure přejděte toohello webové aplikace a zvolte nastavení.
   
   ![image44][image44]
7. Posuňte se dolů toohello funkce kategorie a zvolte diagnostické protokoly.
   
      ![image45][image45]
8. Všimněte si, hello různé možnosti pro protokolování. Zapněte protokolování webového serveru a klikněte na Uložit.
   
   ![image46][image46]
9. Přesunout zpět toohello nástroje oblast pro hello aplikace a zvolte diagnostiky jako službu a klikněte na shromažďování dat hello toorerun spustit.
   
   ![image47][image47]
10. S povoleným nastavením protokolování hello HTTP, zobrazí data shromážděná pro protokoly HTTP.
    
    ![image48][image48]
11. Kliknutím na soubor protokolu hello HTML, vytvořit bohaté sestavy založené na prohlížeči pro další šetření.
    
    ![image49][image49]
12. Přesun toohello nástroje kapitoly hello portál Azure pro aplikaci hello. Posuňte toohello nástroje oddílu a vyberte Průzkumníka procesů.
    
    ![image50][image50]
13. Když vyberete Process Explorer, můžete se podívat na podrobnosti o spuštěných procesech. Všimněte si níže můžete přejít k podrobnostem procesy a i ukončit procesy z hello portálu Azure.
    
    ![image51][image51]
    
    ![image52][image52]
14. Přesunete okno nastavení toohello zpět na levé straně hello. Klikněte na Novou žádost o podporu.
    
    ![image53][image53]
15. V okně hello na hello správné můžete vyplnit podrobnosti o problémech s hello, zadejte kontaktní informace a i odeslání diagnostických dat. Hello portálu Azure umožňuje práce s podporu společnosti Microsoft integrované prostředí.
    
    ![image54][image54]
    
    ![image55][image55]
    
    Hello portálu Azure pomáhá zajistit výkonný a známých nástrojů prostředí toohelp monitorování a řešení potíží s naše spuštěné aplikace. Jste také možné tootake akce rychle provedením úloh, jako je například recyklace procesů, povolování a zakazování různých kolekcí dat a i integrace s profesionální podporu společnosti Microsoft.

## <a name="general-application-management"></a>Obecná správa aplikací
Při správě aplikací, často potřebují tooperform širokou škálu aktivity, například konfigurace strategii zálohování, implementace a Správa poskytovatelů identit a konfigurace řízení přístupu na základě rolí. Jako s hello dalších činnostech DevOps, hello platformy Azure integruje přímo do portálu hello tyto úlohy.

1. tooensure se udržuje hello webové aplikace před únikem potřebujete tooconfigure zálohy. Přejděte toohello nastavení oblast pro vaši webovou aplikaci.
   
   ![image56][image56]
2. V okně hello hello správné posuňte se dolů toohello funkce kategorie.
   
    ![image57][image57]
3. Vyberte zálohování; Otevře se okno na hello správné.
   
   ![image58][image58]
4. Kliknutím na tlačítko Konfigurovat, vyberte účet úložiště v okně hello na hello správné.
   
   ![image59][image59]
5. Teď vytvořte a vyberte toohold kontejneru úložiště záloh. Kliknutím na vytvořit dole hello v okně hello. Pak vyberte kontejner hello.
   
   ![image60][image60]
6. Po zvolení hello kontejneru, můžete nakonfigurovat plány, stejně jako nastavení zálohy pro vaše databáze. Pro tento scénář klikněte na tlačítko hello uložit ikonu.
   
    ![image61][image61]
7. Po uložení, posuňte se zpět toohello okně na levém hello pro zálohy. Klikněte na tlačítko Zálohovat nyní tooback hello aplikace.
   
    ![image62][image62]
8. Za chvíli uvidíte, že se vytvořila záloha. Všimněte si hello obnovit nyní možnost na hello snímek obrazovky níže.
   
    ![image63][image63]
9. Klikněte na obnovit a prozkoumat hello možnosti toohello okně hello správné. Je možné, že že příslušné zálohování a snadno tooan obnovení dříve stavu podle potřeby. Hello portál Azure pomůže nám snadno povolit strategie obnovení po havárii jednoduché aplikace hello.
   
    ![image64][image64]
10. Přesunout zpět okno nastavení toohello na levé straně hello a v části funkce a zvolte ověřování/autorizace.
    
     ![image65][image65]
11. V okně hello na pravém hello vyberte aplikaci služby ověřování. Všimněte si hello různé možnosti, které můžete konfigurovat pomocí Oblíbené zprostředkovatele.
    
     ![image66][image66]
12. Vyberte poskytovatele správy hello podle svého výběru a Všimněte si hello možnosti oboru hello. Můžete zadejte ID aplikace a tajný klíč aplikace a rychle povolit ověřování Facebook pro aplikaci hello. Hello portálu Azure umožňuje ověřování, jako to řešení na klíč pro aplikace.
    
     ![image67][image67]
13. Okno nastavení toohello přesunout zpět a vyberte uživatele v kategorii Správa prostředků hello.
    
     ![image68][image68]
14. V okně hello na pravém hello zkontrolujte hello různé možnosti pro přidání rolí a uživatelů. Hello portálu Azure můžete snadno nastavit RBAC (řízení přístupu na základě rolí) pro aplikace hello.
    
     ![image69][image69]

## <a name="summary"></a>Souhrn
V tomto kurzu ukázán některé hello výkonu hello platformy Azure rychle povolením průběžné nasazování pro webovou aplikaci, provádění různých vývoj a testování aktivity, monitorování a řešení potíží s živé aplikace a nakonec správě klíče strategie například zotavení po havárii, identity a řízení přístupu na základě rolí. Hello platformy Azure umožňuje integrované možnosti pro tyto pracovní postupy DevOps a můžou pracovat efektivně protože zůstává v kontextu pro hello úlohy po ruce.

## <a name="next-steps"></a>Další kroky
* Azure Resource Manager je důležité pro povolení DevOps na hello platformy Azure.  toolearn více navštivte [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
* informace o nasazení služby Azure App Service najdete toolearn [nasazení vaší aplikace tooAzure služby App Service](../app-service-web/web-sites-deploy.md)

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
