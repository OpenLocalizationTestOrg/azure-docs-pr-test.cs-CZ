---
title: "databáze služby Analysis Services aaaAzure zálohování a obnovení | Microsoft Docs"
description: "Popisuje, jak toobackup a obnovení Azure Analysis Services databáze."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: cf0a782d237a95fdfa5ef628f998bd053aac0d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore"></a>Zálohování a obnovení

Zálohování databází tabulkový model v Azure Analysis Services je mnohem hello stejné jako u místní Analysis Services. Hello základní rozdíl je, kam můžete ukládat soubory zálohy. Záložní soubory musí být uložena tooa kontejneru v [účtu úložiště Azure](../storage/common/storage-create-storage-account.md). Můžete použít účet úložiště a kontejneru, který už máte, nebo je lze vytvořit při konfiguraci nastavení úložiště pro svůj server.

> [!NOTE]
> Vytvoření účtu úložiště, může mít za následek začne fakturovat nová služba. Další, najdete v části toolearn [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Zálohy se ukládají s příponou abf. Pro tabulkové modely v paměti jsou uloženy data modelu a metadata. Tabulkové modely DirectQuery ukládají pouze metadata modelu. Zálohy lze komprimovat a šifrování, v závislosti na zvolených možností hello. 



## <a name="configure-storage-settings"></a>Konfigurovat nastavení úložiště
Před zahájením zálohování, musíte pro váš server tooconfigure nastavení úložiště.


### <a name="tooconfigure-storage-settings"></a>Nastavení úložiště tooconfigure
1.  Na portálu Azure > **nastavení**, klikněte na tlačítko **zálohování**.

    ![Zálohy v nastavení](./media/analysis-services-backup/aas-backup-backups.png)

2.  Klikněte na tlačítko **povoleno**, pak klikněte na tlačítko **nastavení úložiště**.

    ![Povolení](./media/analysis-services-backup/aas-backup-enable.png)

3. Vyberte účet úložiště nebo vytvořte novou.

4. Vyberte kontejner nebo vytvořte novou.

    ![Vyberte kontejner](./media/analysis-services-backup/aas-backup-container.png)

5. Uložte nastavení zálohování.

    ![Uložit nastavení zálohování](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Zálohování

### <a name="toobackup-by-using-ssms"></a>toobackup pomocí aplikace SSMS

1. V aplikaci SSMS, klikněte pravým tlačítkem na databázi > **zálohování**.

2. V **příkaz Backup Database** > **záložní soubor**, klikněte na tlačítko **Procházet**.

3. V hello **uložit soubor jako** dialogové okno, ověřte cestu ke složce hello a potom zadejte název pro záložní soubor hello. 

4. V hello **příkaz Backup Database** dialogové okno, vyberte možnosti.

    **Povolit souboru přepsat** – vyberte tuto možnost toooverwrite záložní soubory hello stejný název. Pokud tuto možnost nevyberete, uložíte soubor hello nemůže mít hello stejný název jako soubor, který již existuje v hello stejné umístění.

    **Použít komprese** – vyberte tento záložní soubor možnost toocompress hello. Komprimované záložní soubory ušetřit místo na disku, ale vyžadují mírně zvýší využití procesoru. 

    **Šifrování záložní soubor** – vyberte tento záložní soubor možnost tooencrypt hello. Tato možnost vyžaduje záložní soubor uživatel zadal heslo toosecure hello. heslo Hello brání čtení hello zálohování dat jiným způsobem než operaci obnovení. Pokud si zvolíte tooencrypt zálohy, uložte hello heslo na bezpečné místo.

5. Klikněte na tlačítko **OK** toocreate a uložit záložní soubor hello.


### <a name="powershell"></a>PowerShell
Použití [zálohování ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) rutiny.

## <a name="restore"></a>Obnovení
Při obnovování, záložní soubor musí být v účtu úložiště hello, kterou jste nakonfigurovali pro váš server. Pokud potřebujete toomove záložní soubor z účtu místní tooyour umístění úložiště, použijte [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) nebo hello [AzCopy](../storage/common/storage-use-azcopy.md) nástroj příkazového řádku. 



> [!NOTE]
> Pokud se obnovení ze serveru místní, musíte odebrat všechny uživatele domény hello z rolí hello model a přidat je zpět toohello role jako uživatelů Azure Active Directory.
> 
> 

### <a name="toorestore-by-using-ssms"></a>toorestore pomocí aplikace SSMS

1. V aplikaci SSMS, klikněte pravým tlačítkem na databázi > **obnovení**.

2. V hello **příkaz Backup Database** dialogové okno, v **záložní soubor**, klikněte na tlačítko **Procházet**.

3. V hello **najít soubory databáze** dialogové okno, vyberte hello soubor má toorestore.

4. V **obnovte databázi**, vyberte databázi hello.

5. Zadejte možnosti. Možnosti zabezpečení se musí shodovat hello možnosti zálohování, který jste použili při zálohování.


### <a name="powershell"></a>PowerShell

Použití [obnovení ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) rutiny.


## <a name="related-information"></a>Související informace

[Účty úložiště Azure](../storage/common/storage-create-storage-account.md)  
[Vysoká dostupnost](analysis-services-bcdr.md)     
[Správa služby Azure Analysis Services](analysis-services-manage.md)
