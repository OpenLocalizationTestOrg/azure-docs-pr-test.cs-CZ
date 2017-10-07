---
title: "aaaCopy data z úložiště objektů Blob tooSQL databázi – Azure | Microsoft Docs"
description: "Tento kurz ukazuje, jak toouse aktivitu kopírování v objektu pro vytváření dat Azure kanálu toocopy data z databáze tooSQL úložiště objektů Blob."
keywords: "objekt BLOB sql, úložiště objektů blob kopírování dat"
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: a2c3fb8a4ddd63b0b6b3e75903b7a7eaf188fda4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-copy-data-from-blob-storage-toosql-database-using-data-factory"></a>Kurz: Kopírování dat z úložiště objektů Blob tooSQL databáze pomocí objektu pro vytváření dat
> [!div class="op_single_selector"]
> * [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

V tomto kurzu vytvoříte objekt pro vytváření dat kanál toocopy dat z databáze tooSQL úložiště objektů Blob.

Hello aktivita kopírování provádí přesun dat hello v Azure Data Factory. Používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem. V tématu [aktivity přesunu dat](data-factory-data-movement-activities.md) článku podrobnosti o aktivitě kopírování hello.  

> [!NOTE]
> Podrobný přehled o hello služba Data Factory, najdete v části hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článku.
>
>

## <a name="prerequisites-for-hello-tutorial"></a>Předpoklady pro kurz hello
Než začnete tento kurz, musíte mít hello následující požadavky:

* **Předplatné Azure**.  Pokud nemáte předplatné, můžete si během několika minut bezplatně vytvořit zkušební účet. V tématu hello [bezplatné zkušební verze](http://azure.microsoft.com/pricing/free-trial/) článku.
* **Účet úložiště Azure**. Použít úložiště objektů blob hello jako **zdroj** úložiště dat v tomto kurzu. Pokud nemáte účet úložiště Azure, najdete v části hello [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account) najdete v článku kroky toocreate jeden.
* **Azure SQL Database**. Použít databázi Azure SQL jako **cílové** úložiště dat v tomto kurzu. Pokud nemáte Azure SQL database, můžete použít v hello kurzu, najdete v tématu [jak toocreate a konfigurace Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate jeden.
* **SQL Server 2012 nebo 2014 nebo Visual Studio 2013**. Použít SQL Server Management Studio nebo Visual Studio toocreate ukázkovou databázi a tooview hello Výsledná data v databázi hello.  

## <a name="collect-blob-storage-account-name-and-key"></a>Shromažďovat název účtu úložiště objektů blob a klíč
Budete potřebovat účet hello názvu a klíče účtu úložiště Azure účet toodo v tomto kurzu. Poznamenejte si **název účtu** a **klíč účtu** pro váš účet úložiště Azure.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **další služby** v levé nabídce vyberte hello **účty úložiště**.

    ![Procházet - účty úložiště](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. V hello **účty úložiště** okně, vyberte hello **účtu úložiště Azure** chcete toouse v tomto kurzu.
4. Vyberte **přístupové klíče** v části **nastavení**.
5. Klikněte na tlačítko **kopie** (obrázek) tlačítko vedle příliš**název účtu úložiště** textové pole a uložit a vložte ji někam (například: do textového souboru).
6. Opakujte předchozí krok toocopy hello nebo zapište hello **key1**.

    ![Přístupový klíč k úložišti](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Zavřete všechna okna hello kliknutím **X**.

## <a name="collect-sql-server-database-user-names"></a>Shromažďování systému SQL server, databáze, uživatelská jména
Názvy hello serveru Azure SQL, databáze a toodo uživatele je nutné v tomto kurzu. Zapište názvy **server**, **databáze**, a **uživatele** pro vaši databázi Azure SQL.

1. V hello **portál Azure**, klikněte na tlačítko **další služby** na hello vlevo a vyberte **databází SQL**.
2. V hello **okna databáze SQL**, vyberte hello **databáze** chcete toouse v tomto kurzu. Zapište hello **název databáze**.  
3. V hello **databáze SQL** okně klikněte na tlačítko **vlastnosti** pod **nastavení**.
4. Zapište hello hodnoty pro **název serveru** a **přihlašovací jméno správce serveru**.
5. Zavřete všechna okna hello kliknutím **X**.

## <a name="allow-azure-services-tooaccess-sql-server"></a>Povolit službám Azure tooaccess SQL serveru
Ujistěte se, že **povolit přístup k službám tooAzure** nastavení zapnuté **ON** pro server Azure SQL tak, že hello služba Data Factory k přístup serveru Azure SQL. tooverify a zapněte toto nastavení hello následující kroky:

1. Klikněte na tlačítko **další služby** rozbočovače na levé straně hello a klikněte na **servery SQL**.
2. Vyberte svůj server a v části **NASTAVENÍ** klikněte na **Brána firewall**.
3. V hello **nastavení brány Firewall** okně klikněte na tlačítko **ON** pro **povolit přístup k službám tooAzure**.
4. Zavřete všechna okna hello kliknutím **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Příprava úložiště objektů Blob a databáze SQL
Nyní připravte Azure blob storage a Azure SQL database hello kurzu provedením hello následující kroky:  

1. Spusťte Poznámkový blok. Zkopírujte následující text hello a uložte ho jako **emp.txt** příliš**C:\ADFGetStarted** složky na pevném disku.

    ```
    John, Doe
    Jane, Doe
    ```
2. Pomocí nástrojů, jako [Azure Storage Explorer](http://storageexplorer.com/) toocreate hello **adftutorial** kontejneru a tooupload hello **emp.txt** toohello kontejner souborů.

    ![Azure Storage Explorer. Kopírování dat z databáze tooSQL úložiště objektů Blob](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Použití hello následující SQL skriptu toocreate hello **emp** tabulky v databázi SQL Azure.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **Pokud máte SQL Server 2012 nebo 2014 v počítači nainstalována:** postupujte podle pokynů v [Správa Azure SQL Database pomocí SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server a spustit hello ze skriptu SQL. Tento článek používá hello [portál Azure classic](http://manage.windowsazure.com), není hello [nový portál Azure](https://portal.azure.com), tooconfigure brány firewall pro server Azure SQL.

    Pokud klient nemá povolený tooaccess hello Azure SQL server, musíte tooconfigure brány firewall pro přístup k vaší Azure SQL serveru tooallow z vašeho počítače (IP adresa). V tématu [v tomto článku](../sql-database/sql-database-configure-firewall-settings.md) kroky tooconfigure hello brány firewall pro server Azure SQL.

## <a name="create-a-data-factory"></a>Vytvoření datové továrny
Když jste dokončili hello požadavky. Můžete vytvořit objekt pro vytváření dat pomocí jedné z následujících způsobů hello. Klikněte na jednu z možností hello v rozevíracím seznamu hello hello horní nebo hello následující odkazy tooperform hello kurzu.     

* [Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md)
* [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Šablona Azure Resource Manageru](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat. Ho nebudou transformovat vstupní data tooproduce výstupní data. Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: vytvoření vaší první dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).
> 
> Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit. Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md). 
