---
title: "aaaUpload souborů do účtu Azure Media Services pomocí Aspera | Microsoft Docs"
description: "Tento kurz vás provede kroky hello nahrávání souborů do účtu úložiště, která je přidružena k účtu Media Services pomocí hello ** Aspera serveru na vyžádání ** služby v Azure."
services: media-services
documentationcenter: 
author: johndeu
manager: cfowler
editor: 
ms.assetid: 8812623a-b425-4a0f-9e05-0ee6c839b6f9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 7213f016cc1b7f262b14db7b39b478a03970d1c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-into-a-media-services-account-using-hello-aspera-server-on-demand-service-on-azure"></a>Nahrání souborů do účtu Media Services pomocí hello Aspera serveru na vyžádání služby v Azure

## <a name="overview"></a>Přehled

**Aspera** je software pro vysokorychlostní přenos souborů. **Aspera Server On Demand** pro Azure umožňuje vysokorychlostní nahrávání a stahování velkých souborů přímo do úložiště objektů Azure Blob. Informace o **Aspera na vyžádání**, najdete v části hello [Aspera cloudu](http://cloud.asperasoft.com/) lokality. 
  
**Aspera serveru na vyžádání** pro Azure je možné zakoupit z hello [Azure marketplace](https://azure.microsoft.com/en-us/marketplace/). V pořadí toocomplete nákupu **Aspera Server na vyžádání** pro Azure, prosím přihlaste se k Azure Marketplace pomocí účtu Windows Live ID.

Tento kurz vás provede kroky hello nahrávání souborů do účtu úložiště, která je přidružena k účtu Media Services pomocí hello **Aspera Server na vyžádání** služby v Azure. 

Příklad, který ukazuje, jak funguje toouse Azure s Aspera a služba Media Services můžete nalézt [zde](https://github.com/Azure-Samples/media-services-dotnet-functions-integration/tree/master/103-aspera-ingest).

>[!NOTE]
>Existuje limit toohello maximální velikost souboru podporována pro zpracování pomocí služby Azure Media Services procesory médií (sad Management Pack). Najdete v tématu [to](media-services-quotas-and-limitations.md) téma podrobné informace o omezení velikosti souborů hello.
>

## <a name="prerequisites"></a>Požadavky 

toocomplete tohoto kurzu potřebujete:

* Windows Live ID
* [Účet Azure](https://azure.microsoft.com). Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
* [Účet Azure Media Services](media-services-portal-create-account.md).

## <a name="purchase-aspera-on-demand-for-azure"></a>Nákup služby Aspera On Demand pro Azure

Po přihlášení do Azure Marketplace postupujte podle těchto základních kroků toocomplete nákupu produktu Aspera na vyžádání pro Azure.

1. Vyhledejte Asperu a vyberte „Server On Demand“.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera001.png)

2. Zkontrolujte hello plány předplatného a klikněte na 'zaregistrovat.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera002.png)

3. Vyplňte hello specifika pro váš Server na vyžádání předplatného.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera003.png)

4. Klikněte na hello **cenová úroveň** a vyberte požadované měsíční svazku panelu dílčí hello. V hello **plánování podrobnosti** panel, vyberte **OK**. Potom v hello **zvolte cenovou úroveň** panelu, klikněte na tlačítko **vyberte**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera004.png)

5. Klikněte na **právní podmínky** tooview a přijměte právní podmínky hello panelu dílčí hello. Až si přečtete hello právní podmínky, klikněte na tlačítko **nákupu**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera005.png)

6. Dokončení nákupu hello kliknutím **vytvořit**.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera006.png)

7. Hello řídicí panel Azure bude oznamujeme, že ho se zřizováním služby hello.  Po dokončení se při zřizování, můžete najít nové předplatné hello prohledáním ve vašich prostředků pro název hello hello služby. Jakmile naleznete hello služby, klikněte na něm portál pro správu služeb toolaunch hello.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera007.png)

8. Spuštění portálu pro správu Aspera hello. Jakmile naleznete novou Aspera službu, můžete získat přístup k portálu pro správu toohello, kliknutím na hello služby.  Otevře se nový panel. Z v rámci tento nový panel, musíte tooclick na hello **název prostředku** vaší nové služby.  V hello následující snímek obrazovky je název prostředku hello 'AsperaTransferDemo'. Když kliknete na název prostředku hello, se spustí další panely. Na nově otevřeném panelu uvidíte odkaz Správa. Klikněte na hello "Správa" odkaz toolaunch hello Aspera portálu pro správu služby.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera008.png)

9. Kliknutím na hello spravovat odkaz, zobrazí se stránka toohello registrace, který je vyžadován tooaccess hello služby.

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera009.png)

10. V tomto okamžiku byste měli mít přístup toohello Aspera portál pro správu služeb, kde můžete vytvořit přístupové klíče, stáhnout Aspera klientů a licence, zobrazení využití a další informace o hello rozhraní API.

    Hello následující snímek obrazovky ukazuje vytvoření hello přístup. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera010.png)

    Hello následující snímek obrazovky ukazuje použití hello reporting rozhraní portálu hello. 

   ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera011.png)

## <a name="upload-files-with-aspera"></a>Nahrávání souborů pomocí Aspery

1. Stáhněte a nainstalujte software klienta Aspera hello:
    
    * [Modul plug-in prohlížeče](http://downloads.asperasoft.com/connect2/)
    * [Plně funkční klient](http://downloads.asperasoft.com/en/downloads/2)

2. Proveďte svůj první přenos. Tootransfer klienta pořadí toouse hello Aspera s hello Aspera přenosu služby je třeba toocomplete hello následující: 

    1. Vytvořte přístupový klíč, pomocí portálu Aspera hello.  
    2. Stažení, instalace a licencí hello Aspera klienta (softwaru naleznete v portálu Aspera hello).  

    >[!NOTE]
    >Přečtěte si informace o konfiguraci příručce klienta Aspera hello.
    
    3. Načtení některých informací o účtu úložiště, která souvisí s vaším účtem média Azure pomocí hello [portál Azure](https://portal.azure.com/). Konkrétně, název a klíč a název kontejneru objektu blob úložiště hello toowhich chcete tooplace svůj obsah. 

        * informace o úložiště hello tooget z portálu hello: najít váš účet úložiště, klikněte na hello přístupových klíčů a klíč název a hello hello kopie vašeho účtu.
        * název kontejneru hello tooget: najít váš účet úložiště, vyberte **objekty BLOB**vyberte hello název kontejneru hello chcete tooupload hello obsah do. 

    Zde je snímek obrazovky hello hello Aspera klienta **Správce připojení** kde musíte zadat typ hello "Azure" úložiště a přihlašovací údaje, jakož i hello kontejner objektů blob.

    ![Aspera](./media/media-services-upload-files-with-aspera/media-services-upload-files-with-aspera012.png)

## <a name="resources"></a>Zdroje

následující prostředky Hello byly uvedené v tomto článku. 

* [Modul plug-in prohlížeče Connect](http://downloads.asperasoft.com/connect2/)
* [Příručka modulu Connect](http://downloads.asperasoft.com/en/documentation/8)
* [Klient Aspera](http://downloads.asperasoft.com/en/downloads/2)
* [Příručka klienta](http://downloads.asperasoft.com/en/documentation/2)

## <a name="next-steps"></a>Další kroky

Teď můžete [zkopírovat objekty blob z účtu úložiště do účtu AMS](media-services-copying-existing-blob.md#copy-blobs-from-a-storage-account-into-an-ams-account).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

