---
title: "Nastavené průběžné doručování pro cloud services pomocí sady Visual Studio Online | Microsoft Docs"
description: "Naučte se nastavit průběžné doručování Azure cloudových aplikací bez uložení klíč úložiště diagnostiky konfiguračních souborů služby"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 7e70f92d4d1333ca6cbac5876e5ccbc763bd915c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-to-azure-using-visual-studio-online"></a>Securely uložit úložiště diagnostiky klíč cloudové služby a instalační program průběžnou integraci a nasazení do Azure pomocí sady Visual Studio Online
 Je běžnou praxí zdroj projekty otevřít v současné době. Ukládání tajné klíče aplikace v konfiguračních souborech již není bezpečné postupem jako ohrožení zabezpečení jsou viditelné z úniku z ovládacích prvků veřejné zdrojů tajných klíčů. Ukládání tajný klíč, protože není ve formátu prostého textu v souboru v kanálu průběžnou integraci zabezpečené buď vzhledem k tomu, že servery sestavení může být sdílené prostředky v Cloudovém prostředí. Tento článek vysvětluje, jak Visual Studio a Visual Studio Online snižuje riziko z hlediska zabezpečení během vývoje a průběžnou integraci procesu.

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>Odebrat tajné klíče úložiště diagnostiky v konfiguračním souboru projektu
Rozšíření diagnostiky cloud Services vyžaduje úložiště Azure pro ukládání diagnostiky výsledků. Dříve připojovací řetězec úložiště je zadána v souborech cloudové služby konfigurace (.cscfg) a může být vráceny se změnami do správy zdrojového kódu. V nejnovější verzi sady Azure SDK jsme změnili chování pouze ukládat částečné připojovací řetězec s klíčem nahrazuje token. Následující kroky popisují, jak funguje nástrojů nové cloudové služby:

### <a name="1-open-the-role-designer"></a>1. Otevřete roli návrháře
* Dvakrát klikněte na tlačítko nebo klikněte pravým tlačítkem na roli cloudové služby otevřete návrhář Role

![Otevřete roli návrháře][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2. V části Diagnostika nové zaškrtávací políčko "nemáte odebrat z projektu tajný klíč úložiště" je přidána
* Pokud používáte emulátor místního úložiště, toto políčko je zakázána, protože neexistuje tajný klíč pro správu pro místní připojovací řetězec, který je UseDevelopmentStorage = true.

![Připojovací řetězec pro emulátor místního úložiště není tajné][1]

* Pokud vytváříte nový projekt, ve výchozím nastavení toto políčko je zaškrtnuté políčko. Výsledkem části klíče úložiště připojovacího řetězce úložiště vybrané nahrazován token. Hodnota tokenu budou nacházet ve složce data aplikací Roaming aktuálního uživatele, například: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService

> Upozorňujeme, že složce user\AppData je řídí přístup k přihlášení uživatele a je považován za na bezpečné místo pro uložení vývoj tajných klíčů.
> 
> 

![Úložiště klíč je uložen ve složce profilu uživatele][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3. Vyberte účet úložiště diagnostiky
* Vyberte účet úložiště z tohoto dialogového okna spustit kliknutím "..." . Všimněte si, jak vygenerovat připojovacího řetězce úložiště nebude mít klíč účtu úložiště.
* Příklad: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)

### <a name="4----debugging-the-project"></a>4.    Ladění projektu
* F5 spusťte ladění v sadě Visual Studio. Všechno, co by měly fungovat stejným způsobem jako před.
  ![Spuštění ladění místně][3]

### <a name="5-publish-project-from-visual-studio"></a>5. Publikování projektu v sadě Visual Studio
* Spustit dialogové okno publikování a pokračujte v přihlášení pokyny k publikování applicaion do Azure.

### <a name="6-additional-information"></a>6. Další informace
> Poznámka: Panelu nastavení v Návrháři role zůstanou, protože je teď. Pokud chcete používat funkci tajný správy pro diagnostiku, přejděte na kartu konfigurace.
> 
> 

![Přidejte nastavení][4]

> Poznámka: Pokud je povoleno, Application Insights klíč bude uložen jako prostý text. Klíč slouží pouze pro odeslání obsahu, žádná citlivá data budou v riziko ohrožení.
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>Sestavení a publikování projekt cloudových služeb, pomocí šablony sady Visual Studio online úloh
* Následující postup ukazuje, jak nastavit průběžnou integraci pro projekt cloudové služby pomocí sady Visual Studio online úlohy:
  ### <a name="1----obtain-a-vso-account"></a>1.    Získat účet VSO
* [Vytvoření sady Visual Studio Online účtu] [ Create Visual Studio Online account] Pokud již nemáte
* [Vytvoření týmového projektu] [ Create team project] ve vašem účtu sady Visual Studio online

### <a name="2----setup-source-control-in-visual-studio"></a>2.    Instalační program zdrojového kódu v sadě Visual Studio
* Připojení k týmovému projektu

![Připojení k týmovému projektu][5]

![Vyberte pro připojení k týmovému projektu][6]

* Přidání projektu do správy zdrojového kódu

![Přidání projektu do správy zdrojového kódu][7]

![Mapování projektu do zdrojové složky ovládací prvek][8]

* Zkontrolujte ve vašem projektu z Team Explorer

![Projekt do správy zdrojového kódu se změnami][9]

### <a name="3----configure-build-process"></a>3.    Konfigurace procesu sestavení
* Přejděte k vašemu týmovému projektu a přidat nové šablony procesu sestavení

![Přidání nového sestavení][10]

* Úlohu vyberte sestavení

![Přidat úlohu sestavení][11]

![Vyberte šablonu úloh sestavení Visual Studio][12]

* Upravte vstup úloh sestavení. Prosím přizpůsobit podle potřeby vašeho parametry sestavení

![Nakonfigurujte úlohu sestavení][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* Konfigurace sestavení proměnné

![Konfigurace sestavení proměnné][14]

* Přidat úloha nahrát rozevírací sestavení

![Zvolte publikování úlohu rozevírací sestavení][15]

![Konfigurace publikování úlohu rozevírací sestavení][16]

* Spuštění sestavení

![Nové sestavení fronty][17]

![Zobrazení souhrnu sestavení][18]

* Pokud sestavení úspěšné se zobrazí podobná níže výsledek

![Sestavení výsledek][19]

### <a name="4----configure-release-process"></a>4.    Konfigurace verze procesu
* Vytvořit novou verzi

![Vytvořit novou verzi][20]

* Vyberte úkol nasazení služby Azure Cloud Services

![Vyberte úlohu nasazení Azure Cloud Services][21]

* Jako klíč účtu úložiště není změnami do správy zdrojového kódu, je potřeba zadat tajný klíč pro nastavení rozšíření diagnostiky. Rozbalte **rozšířené možnosti pro vytvoření nové služby** část a upravte **klíče účtu úložiště diagnostiky** vstupní parametr. Tento vstup přebírá více řádků pár klíčových hodnot ve formátu **[RoleName]:$(StorageAccountKey)**

> Poznámka: Pokud váš účet úložiště diagnostiky v rámci stejného předplatného jako ve které budete publikovat aplikace cloudové služby, nemusíte zadejte klíč ve vstupu úlohy nasazení; nasazení prostřednictvím kódu programu získat informace o úložiště ze svého předplatného
> 
> 

![Konfigurace úlohy nasazení cloudové služby][22]

* Tajný klíč použití vytvářet proměnné, které chcete uložit klíče úložiště. K maskování proměnnou jako tajný klíč. klikněte na ikonu zámku na pravé straně vstupní proměnné

![Uložení klíčů k úložišti v proměnné tajný sestavení][23]

* Vytvoření verze a nasazení projektu do Azure

![Vytvořit novou verzi][24]

## <a name="next-steps"></a>Další kroky
Další informace o nastavení rozšíření diagnostiky pro Azure Cloud Services, najdete v tématu [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png
