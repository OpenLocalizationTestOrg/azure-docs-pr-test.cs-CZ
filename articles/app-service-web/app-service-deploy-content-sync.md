---
title: "aaaSync obsah ze složky tooAzure cloudové služby App Service"
description: "Zjistěte, jak toodeploy aplikace tooAzure služby App Service přes obsah synchronizovat ze složky cloudu."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: e1c6d53a427c36126d9cdb33cc21b4126b9d9c2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sync-content-from-a-cloud-folder-tooazure-app-service"></a>Synchronizace obsahu ze složky tooAzure cloudové služby App Service
Tento kurz ukazuje, jak toodeploy příliš[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) podle synchronizuje svůj obsah z oblíbených cloudových úložiště služby, jako je Dropbox a OneDrive. 

## <a name="overview"></a>Přehled nasazení obsahu synchronizace
nasazení synchronizace obsahu na vyžádání Hello používá technologii hello [modul nasazení Kudu](https://github.com/projectkudu/kudu/wiki) integrované se službou App Service. V hello [portálu Azure](https://portal.azure.com), lze určit složku v cloudovém úložišti, pracovat s kódu aplikace a obsah v této složce a tooApp synchronizační službu s hello klikněte tlačítko. Synchronizace obsahu využívá pro proces hello Kudu pro sestavení a nasazení. 

## <a name="contentsync"></a>Jak obsah tooenable synchronizovat nasazení
tooenable obsahu sync z hello [portálu Azure](https://portal.azure.com), postupujte takto:

1. V okně vaší aplikace v hello portálu Azure, klikněte na tlačítko **nastavení** > **zdroj nasazení**. Klikněte na tlačítko **zvolit zdroj**, pak vyberte **OneDrive** nebo **Dropbox** jako hello zdroj pro nasazení. 
   
    ![Synchronizace obsahu](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Kvůli základním rozdílům v hello rozhraní API **Onedrivu pro firmy** v tuto chvíli není podporován. 
   > 
   > 
2. Dokončení hello autorizace pracovního postupu tooenable služby App Service tooaccess definovány konkrétní předem určenou cestu pro OneDrive nebo Dropbox, kde veškerý obsah vaší služby App Service bude uložena.  
    Po hello autorizace služby App Service platformy získáte možnost toocreate hello složku obsahu v rámci hello určené cestu obsahu nebo toochoose existující složku obsahu v této cestě určené obsahu. cesty obsahu Hello uvedených v části vaší cloudové úložiště účty používané pro synchronizaci služby App Service jsou hello následující:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Po hello lze inicializovat obsahu synchronizace hello počáteční synchronizace obsahu na vyžádání z portálu Azure hello. Historie nasazení je k dispozici s hello **nasazení** okno.
   
    ![Historie nasazení](./media/app-service-deploy-content-sync/onedrive_sync.png)

Další informace o nasazení Dropbox je k dispozici v části [nasazení ze Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 

