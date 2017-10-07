---
title: "aaaGet spuštění pomocí Storage Exploreru (Preview) | Microsoft Docs"
description: "Správa prostředků úložiště Azure Storage pomocí Storage Exploreru (Preview)"
services: storage
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1ed0f096-494d-49c4-ab71-f4164ee19ec8
ms.service: storage
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/17/2017
ms.author: kraigb
ms.openlocfilehash: 57737b51baace92858eb07c7dbc3139bd7e041f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-storage-explorer-preview"></a>Začínáme se Storage Explorerem (Preview)
## <a name="overview"></a>Přehled
Azure Storage Explorer (Preview) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage ve Windows, systému macOS a Linux. V tomto článku se dozvíte hello různých způsobů připojení tooand Správa účtům Azure storage.

![Microsoft Azure Storage Explorer (Preview)][15]

## <a name="prerequisites"></a>Požadavky
* [Stažení a instalace Storage Exploreru (Preview)](http://www.storageexplorer.com)

## <a name="connect-tooa-storage-account-or-service"></a>Připojit tooa účtu úložiště nebo službě
Storage Explorer (Preview) nabízí několik způsobů tooconnect toostorage účty. Můžete například provést následující věci:
* Připojte toostorage účty přidružené k vašemu předplatnému Azure.
* Připojte toostorage účty a služby, které jsou sdíleny z jiných předplatných Azure.
* Připojit tooand spravovat místní úložiště pomocí hello emulátoru úložiště Azure. 

Kromě toho můžete pracovat s účty úložiště v globálním i národním Azure:

* [Připojit tooan předplatného Azure](#connect-to-an-azure-subscription): spravovat prostředky úložiště, které patří tooyour předplatného Azure.
* [Práce s místním vývojovém úložiště](#work-with-local-development-storage): spravovat místní úložiště pomocí hello emulátoru úložiště Azure.
* [Připojte úložiště tooexternal](#attach-or-detach-an-external-storage-account): spravovat prostředky úložiště, které patří tooanother předplatného Azure nebo které jsou v rámci Azure národních cloudů pomocí názvu, klíč a koncové body účtu úložiště hello.
* [Připojte účet úložiště pomocí SAS](#attach-storage-account-using-sas): spravovat prostředky úložiště, které patří tooanother předplatného Azure pomocí sdíleného přístupového podpisu (SAS).
* [Připojení služby pomocí SAS](#attach-service-using-sas): spravovat konkrétní službu úložiště (kontejner objektů blob, fronty nebo tabulky) patřící tooanother předplatného Azure pomocí SAS.

## <a name="connect-tooan-azure-subscription"></a>Připojit tooan předplatného Azure
> [!NOTE]
> Pokud nemáte účet Azure, můžete se [zaregistrovat k bezplatné zkušební verzi](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) nebo si [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
>
>

1. V nástroji Storage Explorer (Preview) vyberte **Azure Account Settings** (Nastavení účtu Azure).

    ![Nastavení účtu Azure][0]

2. Hello levém podokně zobrazí všechny účty Microsoft hello, které jste přihlášení. tooconnect tooanother účtu, vyberte možnost **přidat účet**a pak postupujte podle pokynů toosign hello pomocí účtu Microsoft, který je přidružen alespoň jeden aktivní předplatné Azure.

    >[!NOTE]
    >Připojení toonational Azure (například Azure v Německu, Azure Government a Azure China prostřednictvím přihlášení) není aktuálně podporováno. Najdete v části hello "připojení nebo odpojení externího účtu úložiště" části jak účty Azure storage toonational tooconnect.

3. Poté, co se úspěšně přihlásíte účtem Microsoft, hello vlevo, že hello se zobrazí v podokně předplatná Azure, které jsou přidružené k tomuto účtu. Vyberte hello předplatná Azure, které chcete toowork s a potom vyberte **použít**. (Výběr **všechny odběry** přepínačů výběr všech nebo žádných hello uvedených předplatných Azure.)

    ![Výběr předplatných Azure][3]  
    Hello levém podokně zobrazí hello účty úložiště přidružené ke hello vybraná předplatná Azure.

    ![Vybraná předplatná Azure][4]

## <a name="connect-tooan-azure-stack-subscription"></a>Připojení odběru tooan Azure zásobníku

Informace o předplatném Azure zásobníku připojování tooan najdete v tématu [připojit Storage Explorer tooan předplatné Azure zásobníku](azure-stack/azure-stack-storage-connect-se.md).

## <a name="work-with-local-development-storage"></a>Práce s místním vývojovým úložištěm
Pomocí Storage Exploreru (Preview) můžete pracovat s místním úložištěm pomocí emulátoru úložiště Azure hello. Tento přístup umožňuje zapisovat kód proti a testovací úložiště, aniž byste museli mít nasazený v Azure, účet úložiště, protože hello účet úložiště je emulovaných podle hello emulátoru úložiště Azure.

> [!NOTE]
> Hello emulátoru úložiště Azure v současné době podporuje pouze pro systém Windows.
>
>

1. V levém podokně hello Storage Exploreru (Preview) rozbalte hello **(místní a připojené)** > **účty úložiště** > **(vývoj)** uzlu.

    ![Místní vývojový uzel][21]

2. Pokud jste ještě nenainstalovali hello emulátoru úložiště Azure, jste výzvami toodo Ano prostřednictvím informačního panelu. Pokud se zobrazí informační panel hello, vyberte **stažení hello nejnovější verzi**a pak nainstalujte emulátor hello.

    ![Výzva ke stažení emulátoru úložiště Azure Storage][22]

3. Po nainstalování emulátoru hello můžete vytvořit a pracovat s místní objekty BLOB, fronty a tabulky. toolearn jak toowork s účtem úložiště pro každý typ, najdete v jednom z následujících hello:

    * [Správa prostředků Azure Blob Storage](vs-azure-tools-storage-explorer-blobs.md)
    * Správa prostředků Azure File Share Storage: *Připravuje se*
    * Správa prostředků Azure Queue Storage: *Připravuje se*
    * Správa prostředků Azure Table Storage: *Připravuje se*

## <a name="attach-or-detach-an-external-storage-account"></a>Připojení nebo odpojení externího účtu úložiště
Pomocí Storage Exploreru (Preview) můžete připojit tooexternal účty úložiště tak, aby účty úložiště můžete snadno sdílet. Tato část vysvětluje, jak tooattach too(and detach from) externím účtům úložiště.

### <a name="get-hello-storage-account-credentials"></a>Získání přihlašovacích údajů účtu úložiště hello
tooshare externího účtu úložiště, hello vlastník tohoto účtu musíte nejprve získat přihlašovací údaje hello (název účtu a klíč) pro účet hello a potom sdílet informace s hello osobě, která chce tooattach toothat (externímu) účtu. Přihlašovací údaje účtu úložiště hello prostřednictvím hello portálu Azure můžete získat pomocí tohoto postupu hello následující:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. Vyberte **Procházet**.

3. Vyberte **Účty úložiště**.

4. Na hello **účty úložiště** okně, vyberte hello požadovaného účtu úložiště.

5. Na hello **nastavení** okně hello vybraný účet úložiště, vyberte **přístupové klíče**.

    ![Možnost Přístupové klíče][5]

6. Na hello **přístupové klíče** okno, kopie hello **název účtu úložiště** a **key1** hodnoty pro použití při připojení toohello účet úložiště.

    ![Přístupové klíče][6]

### <a name="attach-tooan-external-storage-account"></a>Připojte tooan externího účtu úložiště
tooattach tooan externí účet úložiště, je třeba název a klíč účtu hello. část "Get hello přihlašovacích údajů účtu úložiště" Hello vysvětluje, jak hello tooobtain tyto hodnoty z portálu Azure. Ale hello portálu, se nazývá klíč účtu hello **key1**. Takže pokud Storage Explorer (Preview) požaduje klíč účtu, zadáte hello **key1** hodnotu.

1. Ve Storage Exploreru (Preview), vyberte **připojení úložiště tooAzure**.

    ![Možnost úložiště tooAzure připojení][23]

2. V hello **připojit tooAzure úložiště** dialogovém okně zadejte klíč účtu hello (hello **key1** hodnotu z hello portál Azure) a potom vyberte **Další**.

    > [!NOTE]
    > Můžete zadat připojovací řetězec úložiště hello z účtu úložiště na national Azure. Účty úložiště Německo tooconnect tooAzure, zadejte například připojovací řetězce podobné toohello následující: 
    >
    >* DefaultEndpointsProtocol=https
    >* AccountName=cawatest03
    >* AccountKey=<klíč_účtu_úložiště>
    >* EndpointSuffix=core.cloudapi.de
    
    >Můžete získat připojovací řetězec hello hello Azure portálu v hello stejný způsobem popsaným v hello části "Získání přihlašovacích údajů účtu úložiště hello".

    ![TooAzure úložiště dialogové okno připojení][24]

3. V hello **připojit externí úložiště** dialogové okno, v hello **název účtu** pole, zadejte název účtu úložiště hello, zadejte další požadované nastavení a potom vyberte **Další**.

    ![Dialogové okno Připojit externí úložiště][8]

4. V hello **připojení Souhrn** dialogové okno pole, zkontrolujte informace o hello. Pokud chcete toochange cokoli, vyberte **zpět** a znovu zadejte hello požadovaného nastavení. 

5. Vyberte **Connect** (Připojit).

6. Po úspěšně připojen, hello externí účet úložiště zobrazí s **(External)** připojí toohello název účtu úložiště.

    ![Výsledek připojení tooan externího účtu úložiště][9]

### <a name="detach-from-an-external-storage-account"></a>Odpojení od externího účtu úložiště
1. Klikněte pravým tlačítkem na hello externího účtu úložiště má toodetach a pak vyberte **odpojení**.

    ![Možnost Odpojit od úložiště][10]

2. V potvrzovací zprávě hello vyberte **Ano** tooconfirm hello odpojení od hello externí účet úložiště.

## <a name="attach-a-storage-account-by-using-an-sas"></a>Připojení účtu úložiště pomocí sdíleného přístupového podpisu (SAS)
[SAS](storage/common/storage-dotnet-shared-access-signature-part-1.md) umožňuje Dobrý den, správce předplatného Azure udělit dočasný přístup účtu úložiště tooa bez nutnosti tooprovide přihlašovací údaje předplatného Azure.

tooillustrate tento scénář umožňuje vyslovte tohoto uživatele je správce předplatného Azure a uživatele chce tooaccess b tooallow účtu úložiště po omezenou dobu s určitými oprávněními:

1. Uživatele a vygeneruje SAS (tvořený hello připojovací řetězec pro účet úložiště hello) pro určité časové období a s hello požadovaného oprávnění.

2. Sdílené složky uživatele hello SAS s osobou hello (v našem příkladu b), který chce, aby se účet úložiště toohello přístup.  

3. Uživatel b se pomocí Storage Exploreru (Preview) tooattach toohello účet, který patří tooUserA pomocí hello zadaný SAS.

### <a name="get-an-sas-for-hello-account-you-want-tooshare"></a>Získat SAS pro hello účet, že který má tooshare
1. Ve Storage Exploreru (Preview), klikněte pravým tlačítkem na účet úložiště hello chcete sdílet a potom vyberte **sdíleného přístupového podpisu**.

    ![Možnost místní nabídky Získat sdílený přístupový podpis][13]

2. V hello **sdíleného přístupového podpisu** dialogovém okně zadejte hello časový rámec a oprávnění, které chcete použít pro účet hello a pak vyberte **vytvořit**.

    ![Dialogové okno Získat SAS][14]  
    Hello **sdíleného přístupového podpisu** dialogové okno a zobrazí hello SAS.

3. Další toohello **připojovací řetězec**, vyberte **kopie** toocopy ho toohello schránky a potom vyberte **Zavřít**.

### <a name="attach-toohello-shared-account-by-using-hello-sas"></a>Připojte toohello sdíleného účtu pomocí hello SAS
1. Ve Storage Exploreru (Preview), vyberte **připojení úložiště tooAzure**.

    ![Možnost úložiště tooAzure připojení][23]

2. V hello **připojit tooAzure úložiště** dialogové okno, zadejte hello připojovací řetězec a pak vyberte **Další**.

    ![TooAzure úložiště dialogové okno připojení][24]

3. V hello **připojení Souhrn** dialogové okno pole, zkontrolujte informace o hello. Vyberte změny toomake **zpět**a pak zadejte nastavení hello. 

4. Vyberte **Connect** (Připojit).

5. Po je připojen, hello účet úložiště zobrazí s **(SAS)** připojí toohello název účtu, který jste zadali.

    ![Výsledek připojené tooan účtu pomocí SAS][17]

## <a name="attach-a-service-by-using-an-sas"></a>Připojení služby pomocí sdíleného přístupového podpisu (SAS)
část "Připojit pomocí SAS účtu úložiště" Hello vysvětluje, jak správce předplatného Azure udělit dočasný přístup účtu úložiště tooa generování a sdílení SAS pro účet úložiště hello. Podobně je možné sdílený přístupový podpis (SAS) vygenerovat pro konkrétní službu (kontejner objektů blob, frontu nebo tabulku) v rámci účtu úložiště.  

### <a name="generate-an-sas-for-hello-service-that-you-want-tooshare"></a>Generovat SAS, které chcete tooshare služby hello
V tomto kontextu může být službou kontejner objektů blob, fronta nebo tabulka. toogenerate hello SAS služby uvedené v tématu:

* [Získat hello SAS pro kontejner objektů blob](vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
* Získat hello SAS pro sdílené složky: *již brzy*
* Získat hello SAS pro frontu: *již brzy*
* Získat hello SAS pro tabulku: *již brzy*

### <a name="attach-toohello-shared-account-service-by-using-hello-sas"></a>Připojení toohello sdíleného účtu služby pomocí hello SAS
1. Ve Storage Exploreru (Preview), vyberte **připojení úložiště tooAzure**.

    ![Možnost úložiště tooAzure připojení][23]

2. V hello **připojit tooAzure úložiště** dialogové okno, zadejte hello identifikátor URI pro SAS a pak vyberte **Další**.

    ![TooAzure úložiště dialogové okno připojení][24]

3. V hello **připojení Souhrn** dialogové okno pole, zkontrolujte informace o hello. Vyberte změny toomake **zpět**a pak zadejte nastavení hello. 

4. Vyberte **Connect** (Připojit).

5. Po je připojen, hello nově připojená služba zobrazí v části hello **(Service SAS)** uzlu.

    ![Výsledek připojení tooa sdílené služby pomocí SAS][20]

## <a name="search-for-storage-accounts"></a>Vyhledávání účtů úložiště
Pokud máte dlouhý seznam účtů úložiště, je rychlý způsob toolocate konkrétní účet úložiště toouse hello vyhledávacího pole hello horní části levého podokna hello.

Při psaní do vyhledávacího pole hello hello levé podokno účty úložiště hello zobrazí, které odpovídají hello hledané hodnotě, kterou jste zadali až toothat bodu. Například vyhledávání pro úložiště všechny účty, jejichž název obsahuje **tarcher** se zobrazí v hello následující snímek obrazovky:

![Vyhledávání účtu úložiště][11]

## <a name="next-steps"></a>Další kroky
* [Správa prostředků služby Azure Blob Storage pomocí Storage Exploreru (Preview)](vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
[25]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-certificate-azure-stack.png
[26]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/export-root-cert-azure-stack.png
[27]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/import-azure-stack-cert-storage-explorer.png
[28]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-target-azure-stack.png
[29]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-azure-stack-account.png
[30]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/select-accounts-azure-stack.png
[31]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/azure-stack-storage-account-list.png
