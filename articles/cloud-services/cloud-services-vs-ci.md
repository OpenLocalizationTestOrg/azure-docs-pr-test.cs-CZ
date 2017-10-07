---
title: "aaaContinuous doručení pro cloud services pomocí sady Visual Studio Online | Microsoft Docs"
description: "Zjistěte, jak tooset až nastavené průběžné doručování pro Azure cloudových aplikací bez uložení diagnostiky úložiště klíčů toohello služby konfigurační soubory"
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
ms.openlocfilehash: dc87d049e46daf8b8a26ee4450ebd9b7910f287b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-tooazure-using-visual-studio-online"></a>Securely uložit úložiště diagnostiky klíč cloudové služby a instalační program průběžnou integraci a nasazení tooAzure pomocí sady Visual Studio Online
 V současné době je běžné projekty zdrojů tooopen postupem. Ukládání tajné klíče aplikace v konfiguračních souborech již není bezpečné postupem jako ohrožení zabezpečení jsou viditelné z úniku z ovládacích prvků veřejné zdrojů tajných klíčů. Ukládání tajný klíč, protože není ve formátu prostého textu v souboru v kanálu průběžnou integraci zabezpečené buď vzhledem k tomu, že servery sestavení může být sdílené prostředky na hello cloudového prostředí. Tento článek vysvětluje, jak Visual Studio a Visual Studio Online snižuje hello zabezpečení se během vývoje a průběžnou integraci procesu.

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a>Odebrat tajné klíče úložiště diagnostiky v konfiguračním souboru projektu
Rozšíření diagnostiky cloud Services vyžaduje úložiště Azure pro ukládání diagnostiky výsledků. Připojovací řetězec úložiště hello dříve je uvedená v souborech konfigurace (.cscfg) hello cloudové služby a mohou být vráceny se změnami toosource řízení. V nejnovější verzi sady Azure SDK hello jsme změnili hello chování tooonly úložiště částečné připojovací řetězec s klíčem hello nahrazuje token. Hello následující kroky popisují, jak funguje hello nové cloudové služby nástrojů:

### <a name="1-open-hello-role-designer"></a>1. Otevřete roli designer hello
* Dvakrát klikněte na tlačítko nebo klikněte pravým tlačítkem na roli návrháře tooopen role cloudové služby

![Otevřete roli návrháře][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a>2. V části Diagnostika nové zaškrtávací políčko "nemáte odebrat z projektu tajný klíč úložiště" je přidána
* Pokud používáte emulátor místního úložiště hello, toto políčko je zakázána, protože neexistuje žádný tajný toomanage pro hello místní připojovací řetězec, který je UseDevelopmentStorage = true.

![Připojovací řetězec pro emulátor místního úložiště není tajné][1]

* Pokud vytváříte nový projekt, ve výchozím nastavení toto políčko je zaškrtnuté políčko. Výsledkem hello úložiště klíče oddílu nahrazován token hello vybraný připojovací řetězec úložiště. Hello hodnota tokenu hello najdete ve složce data aplikací Roaming hello aktuálního uživatele, například: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService

> Poznámka: Tato složka user\AppData hello je řídí přístup k přihlášení uživatele a je považován za tajné klíče bezpečné místo toostore vývoj.
> 
> 

![Úložiště klíč je uložen ve složce profilu uživatele][2]

### <a name="3-select-a-diagnostics-storage-account"></a>3. Vyberte účet úložiště diagnostiky
* Vyberte účet úložiště z hello dialogového okna spustit kliknutím na tlačítko "..." hello . Všimněte si, jak vygenerovat hello připojovacího řetězce úložiště nebude mít hello klíč účtu úložiště.
* Příklad: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)

### <a name="4----debugging-hello-project"></a>4.    Ladění projektu hello
* Ladění v sadě Visual Studio toostart F5. Všechno, co by měla fungovat v hello stejný způsobem jako předtím.
  ![Spuštění ladění místně][3]

### <a name="5-publish-project-from-visual-studio"></a>5. Publikování projektu v sadě Visual Studio
* Spuštění hello dialogového okna publikování a pokračujte v přihlášení pokyny toopublish hello applicaion tooAzure.

### <a name="6-additional-information"></a>6. Další informace
> Poznámka: panel nastavení hello v Návrháři role hello zůstanou, protože je teď. Pokud chcete funkci tajný správy hello toouse pro diagnostiku, přejděte toohello kartu konfigurace.
> 
> 

![Přidejte nastavení][4]

> Poznámka: Pokud je povoleno, hello Application Insights se má uložit klíč jako prostý text. Hello klíč se používá pouze pro odeslání obsahu, tak žádné citlivá data budou v riziko ohrožení.
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a>Sestavení a publikování projekt cloudových služeb, pomocí šablony sady Visual Studio online úloh
* Hello následující postup ukazuje, jak toosetup průběžnou integraci pro cloudové služby projektu pomocí sady Visual Studio online úloh:
  ### <a name="1----obtain-a-vso-account"></a>1.    Získat účet VSO
* [Vytvoření sady Visual Studio Online účtu] [ Create Visual Studio Online account] Pokud již nemáte
* [Vytvoření týmového projektu] [ Create team project] ve vašem účtu sady Visual Studio online

### <a name="2----setup-source-control-in-visual-studio"></a>2.    Instalační program zdrojového kódu v sadě Visual Studio
* Připojit tooa týmového projektu

![Připojení tooteam projektu][5]

![Vyberte tooconnect týmového projektu pro][6]

* Přidání ovládacího prvku vašeho projektu toosource

![Přidání ovládacího prvku toosource projektu][7]

![Mapování složky projektu tooa zdroj ovládacího prvku][8]

* Zkontrolujte ve vašem projektu z Team Explorer

![Zkontrolujte v toosource řízení projektu][9]

### <a name="3----configure-build-process"></a>3.    Konfigurace procesu sestavení
* Procházet tooyour týmový projekt a přidejte nové šablony procesu sestavení

![Přidání nového sestavení][10]

* Úlohu vyberte sestavení

![Přidat úlohu sestavení][11]

![Vyberte šablonu úloh sestavení Visual Studio][12]

* Upravte vstup úloh sestavení. Prosím přizpůsobit hello sestavení, potřebné parametry podle tooyour

![Nakonfigurujte úlohu sestavení][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* Konfigurace sestavení proměnné

![Konfigurace sestavení proměnné][14]

* Přidat úloha tooupload sestavení pokles

![Zvolte publikování úlohu rozevírací sestavení][15]

![Konfigurace publikování úlohu rozevírací sestavení][16]

* Spustit hello sestavení

![Nové sestavení fronty][17]

![Zobrazení souhrnu sestavení][18]

* je-li hello sestavení úspěšně zobrazí se podobné toobelow výsledek

![Sestavení výsledek][19]

### <a name="4----configure-release-process"></a>4.    Konfigurace verze procesu
* Vytvořit novou verzi

![Vytvořit novou verzi][20]

* Vyberte úlohu nasazení služby Azure Cloud Services hello

![Vyberte úlohu nasazení Azure Cloud Services][21]

* Jako hello klíč účtu úložiště není zaškrtnuté v ovládacím prvku toosource, potřebujeme toospecify hello tajný klíč pro nastavení rozšíření diagnostiky. Rozbalte hello **rozšířené možnosti pro vytvoření nové služby** část a upravte hello **klíče účtu úložiště diagnostiky** vstupní parametr. Tento vstup přebírá více řádků pár klíčových hodnot ve formátu hello **[RoleName]:$(StorageAccountKey)**

> Poznámka: Pokud váš diagnostiky, které účet úložiště je pod hello stejnému předplatnému jako ve které budete publikovat aplikace hello cloudové služby, nemáte tooenter hello klíč ve vstupu úlohy nasazení hello; Hello nasazení prostřednictvím kódu programu získat informace o hello úložiště ze svého předplatného
> 
> 

![Konfigurace úlohy nasazení cloudové služby][22]

* Tajný klíč použití sestavení proměnné toosave klíče úložiště. vstupní proměnné toomask proměnnou jako tajný klíč. klikněte na ikonu zámku hello na pravé straně hello hello

![Uložení klíčů k úložišti v proměnné tajný sestavení][23]

* Vytvořit verzi a nasadit vaši tooAzure projektu

![Vytvořit novou verzi][24]

## <a name="next-steps"></a>Další kroky
toolearn Další informace o nastavení rozšíření diagnostiky pro Azure Cloud Services najdete v tématu [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]

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
