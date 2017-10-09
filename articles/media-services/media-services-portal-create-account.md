---
title: "aaaCreate účtu Azure Media Services s hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello k vytvoření účtu Azure Media Services s hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: c551e158-aad6-47b4-931e-b46260b3ee4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: juliako
ms.openlocfilehash: fdad93d5d470fc08380670ec0f6c2d33468b1492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-media-services-account-using-hello-azure-portal"></a>Vytvoření účtu Azure Media Services pomocí hello portálu Azure
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-create-account.md)
> * [PowerShell](media-services-manage-with-powershell.md)
> * [REST](https://docs.microsoft.com/rest/api/media/mediaservice)
> 
> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Hello portál Azure poskytuje způsob, jak tooquickly vytvoření účtu Azure Media Services (AMS). Můžete použít tooaccess váš účet Media Services, který toostore můžete povolit, šifrovat, kódovat, spravovat a Streamovat mediální obsah v Azure. V době hello vytvořit účet Media Services, můžete také vytvořit přidružený účet úložiště (nebo použijte existující) v hello stejné zeměpisné oblasti jako hello účtu Media Services.

Tento článek popisuje některé běžné koncepty a ukazuje, jak toocreate Media Services účet s hello portálu Azure.

## <a name="concepts"></a>Koncepty
Přístup ke službě Media Services vyžaduje dva přidružené účty:

* Účet Media Services. Váš účet poskytuje přístup tooa sadu cloudové služby Media Services, které jsou k dispozici v Azure. Účet Media Services neukládá samotný mediální obsah. Místo toho ukládají metadata o hello média obsahu a úlohách zpracování médií v účtu. V době hello vytvoříte účet hello vyberte oblast služby Media Services k dispozici. datové centrum, které ukládá hello záznamů metadat pro váš účet, je Hello oblast, kterou vyberete.
  
* Účet úložiště Azure. Účty úložiště se musí nacházet v hello stejné zeměpisné oblasti jako hello účtu Media Services. Při vytváření účtu Media Services můžete zvolit buď stávající účet úložiště v hello stejné oblasti, nebo můžete vytvořit nový účet úložiště v hello stejné oblasti. Pokud odstraníte účet Media Services, nebudou odstraněny hello objektů BLOB v souvisejícím účtu úložiště.

> [!NOTE]
> Informace o dostupnosti funkcí služby Azure Media Services v různých oblastech najdete v tématu popisujícím [dostupnost funkcí AMS v datových centrech](scenarios-and-availability.md#availability).

## <a name="create-an-ams-account"></a>Vytvoření účtu AMS
Hello kroků v tomto tématu zobrazit jak toocreate AMS účtu.

1. Přihlaste se na hello [portál Azure](https://portal.azure.com/).
2. Klikněte na **+Nové** > **Web + mobilní zařízení** > **Media Services**.
   
    ![Media Services – vytvoření](./media/media-services-create-account/media-services-new1.png)
3. V okně **VYTVOŘIT ÚČET MEDIA SERVICES** zadejte požadované hodnoty.
   
    ![Media Services – vytvoření](./media/media-services-create-account/media-services-new3.png)
   
   1. V **název účtu**, zadejte název nového účtu AMS hello hello. Název účtu Media Services je všechny malých písmen nebo číslic bez mezer a je 3 too24 znaků.
   2. V odběru vyberte mezi hello různých předplatných Azure, které máte přístup.
   3. V **skupiny prostředků**, vyberte hello nový nebo existující prostředek.  Skupina prostředků je kolekce prostředků, které sdílejí životní cyklus, oprávnění a zásady. Další informace najdete [tady](../azure-resource-manager/resource-group-overview.md#resource-groups).
   4. V **umístění**, hello vyberte zeměpisnou oblast, která bude použité toostore hello médií a záznamů metadat pro váš účet Media Services. Tato oblast bude použité tooprocess a vysílání datových proudů. V rozevíracím seznamu hello se zobrazí pouze hello dostupné oblasti Media Services. 
   5. V **účet úložiště**, vyberte úložiště objektů blob tooprovide účet úložiště hello mediálního obsahu z vašeho účtu Media Services. Můžete vybrat existující účet úložiště v hello stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit účet úložiště. Nový účet úložiště je vytvořen v hello stejné oblasti. Hello pravidla pro účet úložiště, že jsou názvy hello stejné jako u účtů Media Services.
      
       Další informace o ukládání a úložištích najdete [tady](../storage/common/storage-introduction.md).
   6. Vyberte **Pin toodashboard** toosee hello průběh nasazení účtu hello.
4. Klikněte na tlačítko **vytvořit** v hello dolní části formuláře hello.
   
    Po úspěšném vytvoření účtu hello načtení stránky přehled. V hello streamování koncový bod tabulky hello účet bude mít výchozí koncový bod v hello streamování **Zastaveno** stavu. 

    >[!NOTE]
    >Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. 
   
## <a name="toomanage-your-ams-account"></a>toomanage vašeho účtu AMS

toomanage vašeho účtu AMS (například připojení toohello AMS rozhraní API prostřednictvím kódu programu, nahrávání videí, kódování assetů, konfigurace ochrany obsahu a sledovat průběh úlohy) vyberte **nastavení** na levé straně hello portálu hello. Z hello **nastavení**, přejděte tooone hello k dispozici oken (například: **přístup pomocí rozhraní API**, **prostředky**, **úlohy**, **Obsahu ochrany**).


## <a name="next-steps"></a>Další kroky

Soubory teď můžete nahrát do účtu AMS. Další informace najdete v tématu [Nahrání souborů](media-services-portal-upload-files.md).

Pokud máte v plánu prostřednictvím kódu programu tooaccess AMS rozhraní API, přečtěte si téma [hello přístup k rozhraní API služby Azure Media Services pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

