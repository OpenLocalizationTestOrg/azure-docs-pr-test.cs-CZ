---
title: "aaaAdd Azure Storage pomocí připojené služby v sadě Visual Studio | Microsoft Docs"
description: "Přidat aplikaci Azure Storage tooyour hello Visual Studio přidat připojení služby dialogovém okně"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 56b42063d86510b330e405108e28d50e6ba4da05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Přidání úložiště Azure pomocí Visual Studio připojené Services
Pomocí sady Visual Studio, můžete připojit žádné z následujících tooAzure úložiště pomocí hello hello **přidat připojení služby** dialogové okno:

- Cloudová služba jazyka C#
- Rozhraní .NET back-end mobilní služby
- Web ASP.NET nebo služby
- ASP.NET základní služby
- Služba Azure webové úlohy 

Hello funkci připojené služby přidá všechny odkazy hello potřeby a připojení kód tooyour projektu a odpovídajícím způsobem upravit konfigurační soubory. 

Po dokončení hello **přidat připojení služby** dokumentace s podrobnostmi o hello kroky požadované toostart práce úložiště objektů blob, fronty a tabulky se automaticky zobrazí dialogové okno.

## <a name="connect-tooazure-storage-using-hello-connected-services-dialog"></a>Připojit tooAzure úložiště pomocí služby připojené hello dialogové okno
1. Otevřete projekt v sadě Visual Studio

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na hello **připojené služby** uzlu a z hello kontextovou nabídku a vyberte **přidat připojení službě**.
   
    ![Přidat Azure připojení služby](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. V hello **připojené služby** vyberte **úložiště v cloudu s Azure Storage**.
   
    ![Přidejte úložiště Azure](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. V hello **Azure Storage** dialogovém okně vyberte stávající účet úložiště a vyberte **přidat**.
   
    Pokud potřebujete toocreate účet úložiště, přejděte toohello další krok. Jinak přejděte toostep 6.
    
    ![Přidat existující tooproject účtu úložiště](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. toocreate účet úložiště: 
   
   1. Vyberte **vytvořit nový účet úložiště** dolnímu hello hello dialogového okna.

   1. Vyplňte hello **vytvořit účet úložiště** dialogové okno a vyberte **vytvořit**.
      
       ![Nový účet úložiště Azure](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Když hello **Azure Storage** zobrazí dialog, hello nový účet úložiště se zobrazí v seznamu hello. Vyberte v seznamu hello hello nový účet úložiště a vyberte **přidat**.

1. Hello úložiště připojené služby se zobrazí pod hello **odkazy na službu** uzlu projektu.
   
## <a name="how-your-project-is-modified"></a>Jak se mění projektu
Po dokončení hello dialogové okno přidá odkazy na Visual Studio a upraví určité konfigurační soubory. určité změny Hello závisí na typu projektu hello: 

- Projekt ASP.NET - [co se stalo – projekty ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- Projekt ASP.NET Core - [co se stalo – projekty ASP.NET 5](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Projekt cloudové služby (webových rolí a rolí pracovního procesu) - [co se stalo – projekty cloudových služeb](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Projekt webové úlohy – [co se stalo – projekty webové úlohy](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Další kroky
- [Fóru MSDN: Úložiště Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Blog týmu Microsoft Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
- [Dokumentace k Azure Storage](https://docs.microsoft.com/azure/storage/)
