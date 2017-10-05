---
title: "Bottle a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio"
description: "Naučte se používat nástroje Python Tools pro sadu Visual Studio k vytvoření aplikace Bottle, která ukládá data ve službě Azure Table Storage a nasazení webové aplikace do Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: f075124b-db79-4e51-b394-09187dd6c634
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: fb25f03607ac6e9af46b47f54e830e0283dd1b0a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a>Bottle a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio
V tomto kurzu použijeme [Python Tools pro Visual Studio] vytvořit jednoduchou hlasovací webové aplikace pomocí jedné z ukázkových šablon PTVS. V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=GJXDGaEPy94).

Hlasovací webové aplikace definuje abstrakci pro své úložiště, takže můžete snadno přepínat mezi různými typy úložiště (v paměti, Azure Table Storage, MongoDB).

Jsme dozvíte, jak vytvořit účet úložiště Azure, jak nakonfigurovat webovou aplikaci k používání Azure Table Storage a jak publikovat webovou aplikaci do [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Najdete v článku [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami MongoDB, Azure Table Storage, MySQL a SQL Database. Ačkoli se tento článek zaměřuje na službu App Service, postup při vývoji služeb [Azure Cloud Services] je obdobný.

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2015
* [Python Tools 2.2 pro Visual Studio]
* [Python Tools 2.2 pro Visual Studio – ukázky VSIX]
* [Azure SDK Tools for VS 2015]
* [Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-the-project"></a>Vytvoření projektu
V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony. Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky. Jsme budete pak spusťte aplikaci místně pomocí výchozí úložiště v paměti.

1. V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.
2. Šablony projektů z [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou dostupné v části **Python**, **Ukázky**. Vyberte **hlasování Bottle webového projektu** a klikněte na tlačítko OK a vytvořte tak projekt.
   
     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. Zobrazí se výzva k instalaci externích balíčků. Vyberte možnost **Instalovat do virtuálního prostředí**.
   
     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. Jako základní překladač vyberte jazyk **Python 2.7** nebo **Python 3.4**.
   
     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. Stisknutím klávesy `F5` se ujistěte, zda aplikace pracuje. Ve výchozím nastavení používá aplikace jako úložiště v paměti, která nevyžaduje žádnou konfiguraci. Při zastavení webového serveru, dojde ke ztrátě všech dat.
6. Klikněte na tlačítko **vytvořit ukázková hlasování**, klikněte na hlasování a Hlasujte.
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure
Chcete-li použít operace úložiště, potřebujete účet úložiště Azure. Pomocí následujícího postupu můžete vytvořit účet úložiště.

1. Přihlaste se [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **nový** ikonu na horní pravé portálu, pak klikněte na tlačítko **Data + úložiště** > **účet úložiště**.  Klikněte na tlačítko **vytvořit** tlačítko, pak zadejte jedinečný název účtu úložiště a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.
   
      ![Rychlé vytvoření](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    Po vytvoření účtu úložiště **oznámení** tlačítko bude flash zelená **úspěch** a okno účtu úložiště je otevřená a zobrazit tak, že patří do nové skupiny prostředků, který jste vytvořili.
3. Klikněte **přístupové klíče** část v okně účtu úložiště. Poznamenejte si název účtu a key1.
   
      ![Klíče](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    Potřebujeme bude tyto informace ke konfiguraci projektu v další části.

## <a name="configure-the-project"></a>Konfigurace projektu
V této části nakonfigurujeme naší aplikaci, aby používala účet úložiště, který jsme právě vytvořili. Potom jsme budete místní spuštění aplikace.

1. V sadě Visual Studio, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a vyberte **vlastnosti**. Klikněte na **ladění** kartě.
   
     ![Nastavení pro ladění projektu](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. Nastavte hodnoty proměnných prostředí, které jsou požadované aplikací v **ladění serveru příkaz**, **prostředí**.
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   Nastaví proměnné prostředí při jste **spustit ladění**. Pokud chcete, aby proměnné, které chcete být nastavený, pokud jste **spustit bez ladění**, nastavte stejné hodnoty v části **spustit příkaz serveru** také.
   
   Alternativně můžete definovat proměnné prostředí pomocí ovládacího panelu Windows. Toto je lepší volbou, pokud se chcete vyhnout ukládání přihlašovacích údajů ve zdrojovém kódu / souboru projektu. Všimněte si, že budete muset restartovat Visual Studio pro nové hodnoty prostředí být k dispozici pro aplikaci.
3. Kód, který implementuje úložiště Azure Table Storage je v **models/azuretablestorage.py**. Najdete v článku [dokumentace] Další informace o tom, jak používat služby Table z Pythonu.
4. Spusťte aplikaci klávesou `F5`. Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a data odeslaná při hlasování budou serializována v Azure Table Storage.
   
   > [!NOTE]
   > Virtuální prostředí Python 2.7 může způsobit přerušení k výjimce v sadě Visual Studio.  Stiskněte klávesu `F5` Chcete-li pokračovat v načítání webového projektu.
   > 
   > 
5. Vyhledejte **o** a ověřte, že je aplikace pomocí **Azure Table Storage** úložiště.
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-the-azure-table-storage"></a>Prozkoumejte Azure Table Storage
Je snadné zobrazení a úprava tabulek úložiště pomocí Průzkumníku cloudu v sadě Visual Studio. V této části použijeme v Průzkumníku serveru zobrazit obsah tabulek aplikace hlasování.

> [!NOTE]
> To vyžaduje Microsoft Azure nástroje pro instalaci, které jsou k dispozici jako součást [Azure SDK for .NET].
> 
> 

1. Otevřete **cloudu Explorer**. Rozbalte položku **účty úložiště**, váš účet úložiště, pak **tabulky**.
   
     ![Průzkumník cloudu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. Dvakrát klikněte na **hlasování** nebo **volby** tabulky k zobrazení obsahu tabulky v okně dokumentu a také přidat, odebrat nebo upravit entity.
   
     ![Tabulky výsledků dotazu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Publikování webové aplikace do služby Azure App Service
Sada Azure .NET SDK poskytuje snadný způsob, jak nasadit webovou aplikaci do služby Azure App Service.

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte možnost **Publikovat**.
   
     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. Klikněte na položku **Microsoft Azure Web Apps**.
3. Kliknutím na možnost **Nové** vytvořte novou webovou aplikaci.
4. Vyplňte následující pole a klikněte na tlačítko **vytvořit**.
   
   * **Název webové aplikace**
   * **Plán služby App Service**
   * **Skupina prostředků**
   * **Oblast**
   * Položku **Databázový server** ponechte nastavenou na možnost **Bez databáze**
5. Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.
6. Automaticky se otevře webový prohlížeč s publikovanou webovou aplikací. Pokud přejdete o stránce, uvidíte, že používá **v paměti** úložiště, není **Azure Table Storage** úložiště.
   
   Je to způsobeno proměnné prostředí nejsou nastavte u instance webové aplikace v Azure App Service, takže používá výchozí hodnoty zadané v **settings.py**.

## <a name="configure-the-web-apps-instance"></a>Konfigurace instance webové aplikace
V této části nakonfigurujeme proměnných prostředí pro instanci webové aplikace.

1. V [portálu Azure], otevřete okno webové aplikace kliknutím **Procházet** > **App Services** > název vaší webové aplikace.
2. V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nastavení aplikace**.
3. Přejděte dolů k položce **nastavení aplikace** tématu a nastavte hodnoty pro **úložiště\_název**, **úložiště\_název** a **úložiště\_klíč** jak je popsáno v **konfigurace projektu** část výše.
   
     ![Nastavení aplikace](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. Klikněte na **Uložit**. Poté, co jste obdrží oznámení, že byly použity změny, klikněte na **Procházet** v okně hlavní webové aplikace.
5. Měli byste vidět webové aplikace funguje podle očekávání, pomocí **Azure Table Storage** úložiště.
   
   Blahopřejeme!
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a>Další kroky
Další informace o nástrojích Python Tools pro Visual Studio, Bottle a Azure Table Storage na následujících odkazech.

* [Dokumentace nástrojů Python Tools pro Visual Studio]
  * [Webové projekty]
  * [Projekty cloudových služeb]
  * [Vzdálené ladění v Microsoft Azure]
* [Bottle dokumentace]
* [Azure Storage]
* [Azure SDK pro Python]
* [Jak používat služby úložiště Table z Pythonu]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentace]:../cosmos-db/table-storage-how-to-use-python.md
[Jak používat služby úložiště Table z Pythonu]:../cosmos-db/table-storage-how-to-use-python.md


<!--External Link references-->
[portálu Azure]: https://portal.azure.com
[Azure SDK for .NET]: http://azure.microsoft.com/downloads/
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=624025
[Python Tools 2.2 pro Visual Studio – ukázky VSIX]: http://go.microsoft.com/fwlink/?LinkId=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Bottle dokumentace]: http://bottlepy.org/docs/dev/index.html
[Vzdálené ladění v Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webové projekty]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK pro Python]: https://github.com/Azure/azure-sdk-for-python
