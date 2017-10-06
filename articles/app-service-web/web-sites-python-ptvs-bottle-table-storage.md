---
title: "aaaBottle a Azure Table Storage na Azure pomocí nástroje Python Tools 2.2 pro Visual Studio"
description: "Zjistěte, jak toouse hello Python Tools pro Visual Studio toocreate Bottle aplikace, která ukládá data ve službě Azure Table Storage a nasadit hello webové aplikace tooAzure App Service Web Apps."
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
ms.openlocfilehash: 25b9eb002b8748483d5b9458b7b5860a958b4bb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a>Bottle a úložiště Azure Table v Azure s nástroji Python Tools 2.2 pro Visual Studio
V tomto kurzu použijeme [Python Tools pro Visual Studio] dotazuje toocreate jednoduchou webovou aplikaci pomocí jednoho z ukázkových šablon PTVS hello. V tomto kurzu je také k dispozici [video](https://www.youtube.com/watch?v=GJXDGaEPy94).

Hello hlasovací webové aplikace definuje abstrakci pro své úložiště, takže můžete snadno přepínat mezi různými typy úložiště (v paměti, Azure Table Storage, MongoDB).

Jsme dozvíte, jak toocreate Azure Storage účet, jak tooconfigure hello toouse webové aplikace Azure Table Storage a jak toopublish hello webovou aplikaci příliš[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

V tématu hello [středisku pro vývojáře Python] další články, které se týkají vývoj Azure App Service Web Apps s nástroji PTVS pomocí Bottle, Flask a Django webové rozhraní, se službami MongoDB, Azure Table Storage, MySQL a SQL Database. Když tento článek se zaměřuje na služby App Service, hello postup je podobný jako při vývoji [Azure Cloud Services].

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2015
* [Python Tools 2.2 pro Visual Studio]
* [Python Tools 2.2 pro Visual Studio – ukázky VSIX]
* [Azure SDK Tools for VS 2015]
* [Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-hello-project"></a>Vytvoření projektu hello
V této části vytvoříme projekt sady Visual Studio pomocí vzorové šablony. Jsme vytvoříte virtuální prostředí a nainstalujte požadované balíčky. Potom budete spustíme aplikace hello místně pomocí hello výchozí v paměti úložiště.

1. V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.
2. šablony projektů z hello Hello [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou k dispozici v části **Python**, **ukázky**. Vyberte **hlasování Bottle webového projektu** a klikněte na tlačítko OK toocreate hello projektu.
   
     ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. Bude výzvami tooinstall externí balíčky. Vyberte možnost **Instalovat do virtuálního prostředí**.
   
     ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. Vyberte **Python 2.7** nebo **Python 3.4** jako základní překladač hello.
   
     ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. Potvrďte, že hello aplikace funguje tak, že stisknete `F5`. Ve výchozím nastavení používá aplikace hello jako úložiště v paměti, která nevyžaduje žádnou konfiguraci. Všechna data bude ztracena, jakmile je zastavena hello webový server.
6. Klikněte na tlačítko **vytvořit ukázková hlasování**, klikněte na hlasování a Hlasujte.
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu úložiště Azure
operace úložiště toouse, potřebujete účet úložiště Azure. Pomocí následujícího postupu můžete vytvořit účet úložiště.

1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/).
2. Klikněte na tlačítko hello **nový** ikonu na hello horní pravé hello portál, pak klikněte na tlačítko **Data + úložiště** > **účet úložiště**.  Klikněte na tlačítko hello **vytvořit** tlačítko, pak zadejte účet úložiště hello jedinečný název a vytvořte novou [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) pro ni.
   
      ![Rychlé vytvoření](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    Po vytvoření účtu úložiště hello hello **oznámení** tlačítko bude flash zelená **úspěch** a se otevře okno účtu úložiště hello tooshow patří toohello nový prostředek skupiny vytvořit.
3. Klikněte na tlačítko hello **přístupové klíče** část v okně účtu úložiště hello. Poznamenejte si název účtu hello a key1.
   
      ![Klíče](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    Potřebujeme bude tato informace tooconfigure projektu v další části hello.

## <a name="configure-hello-project"></a>Konfigurace hello projektu
V této části nakonfigurujeme naše aplikace toouse hello účet úložiště, který jsme právě vytvořili. Potom jsme budete místní spuštění aplikace hello.

1. V sadě Visual Studio, klikněte pravým tlačítkem na uzel projektu v Průzkumníku řešení a vyberte **vlastnosti**. Klikněte na hello **ladění** kartě.
   
     ![Nastavení pro ladění projektu](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. Nastavení hello hodnot proměnných prostředí vyžaduje aplikace hello v **ladění serveru příkaz**, **prostředí**.
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   To bude nastavení proměnných prostředí hello když jste **spustit ladění**. Chcete-li hello proměnné toobe nastavený, pokud jste **spustit bez ladění**, sada hello stejné hodnoty v části **spustit příkaz serveru** také.
   
   Alternativně můžete definovat proměnné prostředí pomocí ovládacích panelů Windows hello. Toto je lepší volbou, pokud chcete tooavoid ukládání přihlašovacích údajů ve zdrojovém kódu / souboru projektu. Všimněte si, že budete potřebovat toorestart Visual Studio pro hello nové prostředí hodnoty toobe k dispozici toohello aplikace.
3. Hello kód, který implementuje úložiště Azure Table Storage hello je v **models/azuretablestorage.py**. V tématu hello [dokumentace] Další informace o tom, toouse služby Table z Pythonu.
4. Spuštění aplikace hello s `F5`. Hlasování, které jsou vytvořeny pomocí **vytvořit ukázková hlasování** a hello data odeslaná při hlasování budou serializována v Azure Table Storage.
   
   > [!NOTE]
   > Hello virtuální prostředí Python 2.7 může způsobit přerušení k výjimce v sadě Visual Studio.  Stiskněte klávesu `F5` toocontinue načítání hello webového projektu.
   > 
   > 
5. Procházet toohello **o** tooverify stránky, které aplikace hello používá hello **Azure Table Storage** úložiště.
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a>Prozkoumejte hello Azure Table Storage
Je snadno tooview a upravit tabulky úložiště pomocí Průzkumníku cloudu v sadě Visual Studio. V této části použijeme Průzkumníka serveru tooview hello obsahu tabulek aplikace hello hlasování.

> [!NOTE]
> To vyžaduje toobe nástroje Microsoft Azure nainstalovaná, které jsou k dispozici jako součást hello [Azure SDK for .NET].
> 
> 

1. Otevřete **cloudu Explorer**. Rozbalte položku **účty úložiště**, váš účet úložiště, pak **tabulky**.
   
     ![Průzkumník cloudu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. Dvakrát klikněte na hello **hlasování** nebo **volby** tabulky tooview hello obsah hello tabulky v okně dokumentu a také přidat, odebrat nebo upravit entity.
   
     ![Tabulky výsledků dotazu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a>Publikování hello webové aplikace tooAzure služby App Service
Hello .NET SDK služby Azure poskytuje snadno toodeploy tooAzure vaší webové aplikace služby App Service.

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na uzel projektu hello a vyberte **publikovat**.
   
     ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. Klikněte na položku **Microsoft Azure Web Apps**.
3. Klikněte na **nový** toocreate novou webovou aplikaci.
4. Vyplňte následující pole hello a klikněte na tlačítko **vytvořit**.
   
   * **Název webové aplikace**
   * **Plán služby App Service**
   * **Skupina prostředků**
   * **Oblast**
   * Nechte **databázový server** nastavit příliš**žádná databáze.**
5. Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.
6. Webový prohlížeč se automaticky otevře toohello publikované webové aplikace. Pokud toohello o stránce, se zobrazí, že používá hello **v paměti** úložiště, není hello **Azure Table Storage** úložiště.
   
   Je to způsobeno proměnné prostředí hello nejsou nastaveny na hello instanci webové aplikace v Azure App Service, takže používá hello výchozí hodnoty zadané v **settings.py**.

## <a name="configure-hello-web-apps-instance"></a>Konfigurace instance webové aplikace hello
V této části nakonfigurujeme proměnných prostředí pro instanci webové aplikace hello.

1. V [portálu Azure], otevřete okno hello webovou aplikaci kliknutím **Procházet** > **App Services** > název vaší webové aplikace.
2. V okně vaší webové aplikace, klikněte na tlačítko **všechna nastavení**, pak klikněte na tlačítko **nastavení aplikace**.
3. Projděte dolů toohello **nastavení aplikace** tématu a nastavte hodnoty hello **úložiště\_název**, **úložiště\_název** a  **ÚLOŽIŠTĚ\_klíč** jak je popsáno v hello **konfigurovat hello projektu** část výše.
   
     ![Nastavení aplikace](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. Klikněte na **Uložit**. Poté, co jste přijali hello oznámení, že byly použity změny hello, klikněte na **Procházet** z hlavní okně hello webové aplikace.
5. Měli byste vidět hello webové aplikace pracovní podle očekávání, pomocí hello **Azure Table Storage** úložiště.
   
   Blahopřejeme!
   
     ![Webový prohlížeč](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a>Další kroky
Použijte tyto odkazy toolearn Další informace o nástrojích Python Tools pro Visual Studio, Bottle a úložiště tabulek Azure.

* [Dokumentace nástrojů Python Tools pro Visual Studio]
  * [Webové projekty]
  * [Projekty cloudových služeb]
  * [Vzdálené ladění v Microsoft Azure]
* [Bottle dokumentace]
* [Azure Storage]
* [Azure SDK pro Python]
* [Jak tooUse hello služby úložiště Table z Pythonu]

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentace]:../cosmos-db/table-storage-how-to-use-python.md
[Jak tooUse hello služby úložiště Table z Pythonu]:../cosmos-db/table-storage-how-to-use-python.md


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
