---
title: aaaHow tooconnect toodata zdroje | Microsoft Docs
description: "Jak tooarticle zvýraznění způsob, jakým tooconnect toodata zdroje zjišťovány s Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 4e6b27a5-cf75-4012-b88c-333c1fe638e8
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 01d659510c8e67c1238ed488f4eebf511aab7217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-toodata-sources"></a>Jak tooconnect toodata zdroje
## <a name="introduction"></a>Úvod
**Microsoft Azure Data Catalog** je plně spravovaná Cloudová služba, která slouží jako systém registrace a systém pro zjišťování zdrojů dat organizace. Jinými slovy **Azure Data Catalog** všechny o pomoc osoby zjišťovat, pochopit a používat zdroje dat a pomáhá organizacím tooget další hodnoty ze své stávající data. Klíčovým prvkem tento scénář používá hello zdroje dat – jakmile uživatel zjišťuje data a rozumí svůj účel, hello dalším krokem je, že zdroje dat toohello tooconnect tooput toouse jeho data.

## <a name="data-source-locations"></a>Umístění zdroje dat.
Během registrace zdroje dat **Azure Data Catalog** obdrží metadata o zdroj dat hello. Tato metadata zahrnuje hello podrobnosti o umístění zdroje dat hello. Podrobnosti Hello hello umístění se budou lišit od zdroj toodata zdroje dat, ale bude vždy obsahovat tooconnect informace potřebné hello. Hello umístění pro tabulku systému SQL Server například obsahuje hello název serveru, název databáze, název schématu a název tabulky, zatímco hello umístění pro sestavy SQL Server Reporting Services zahrnuje sestava toohello hello cesta a název serveru hello. Ostatní typy zdrojů dat bude mít umístění, která odráží hello struktura a možnosti hello zdroj systému.

## <a name="integrated-client-tools"></a>Integrovaného klienta nástroje
Hello nejjednodušší způsob, jak tooconnect tooa zdroj dat je toouse hello "Otevřít v..." nabídky v hello **Azure Data Catalog** portálu. Tato nabídka zobrazí seznam možností pro připojení toohello vybrané datové prostředky.
Pokud používáte hello výchozí dlaždice zobrazení, tato nabídka je k dispozici na hello každou dlaždici.

 ![Otevřením tabulku systému SQL Server v aplikaci Excel z hello data asset dlaždice](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Pokud používáte zobrazení seznamu hello, je k dispozici v panelu vyhledávání hello hello horní části okna portálu hello nabídka hello.

 ![Otevření sestavy služby SQL Server Reporting Services ve správci sestav z panelu Hledat hello](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Podporované klientské aplikace
Při použití hello "Otevřít v..." nabídky pro datové zdroje na portálu Azure Data Catalog hello, hello správné klientská aplikace musí být nainstalován na klientském počítači hello.

| Otevřít v aplikaci | Příponu souboru / protocol | Verze podporované aplikace |
| --- | --- | --- |
| Excel |ODC |Excel 2010 nebo novější |
| Aplikace Excel (horní 1000) |ODC |Excel 2010 nebo novější |
| Power Query |XLSX |Excel 2016 nebo aplikaci Excel 2010 nebo Excel 2013 s hello Power Query pro Excel add-in nainstalován |
| Power BI Desktop |.pbix |Power BI Desktop července 2016 nebo novější |
| SQL Server Data Tools |vsweb: / / |Visual Studio 2013 Update 4 nebo novější s nástrojů systému SQL Server nainstalován |
| Správce sestav |http:// |V tématu [požadavky na prohlížeč pro SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Data, vaše nástroje
Hello možnosti, které jsou k dispozici v nabídce hello bude záviset na typu hello aktuálně vybraný datový prostředek. Samozřejmě, ne všechny možné nástroje bude součástí hello "Otevřít v..." nabídce, ale je stále snadno tooconnect toohello datovému zdroji prostřednictvím jakéhokoli klienta nástroje. Vybrání datový prostředek v hello **Azure Data Catalog** portálu hello dokončení umístění se zobrazí v podokně Vlastnosti hello.

 ![Informace o připojení pro tabulku systému SQL Server](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

informace o připojení Hello, že podrobnosti se budou lišit od typ zdroje toodata typ zdroje dat, ale informace hello součástí portálu hello získáte vše, co že potřebujete tooconnect toohello zdroj dat v nástroji žádné klienta. Uživatelé mohou kopírovat podrobnosti připojení hello hello zdrojů dat, které budou zjištěny pomocí **Azure Data Catalog**, povolením toowork s daty hello v upřednostňovaném nástroji.

## <a name="connecting-and-data-source-permissions"></a>Připojení a data zdroje oprávnění
I když **Azure Data Catalog** díky zdroje dat zjistitelný, přístup samotná data toohello zůstává pod kontrolou hello hello datový zdroj vlastník nebo správce. Zjišťování zdroje dat v **Azure Data Catalog** uživatele neposkytuje žádné oprávnění tooaccess hello zdroj dat sám sebe.

toomake snazší pro uživatelé, kteří zjistit datové zdroje, ale nemají oprávnění tooaccess svá data, uživatelé může poskytnout informace v hello požádat o přístup k vlastnosti při zadávání poznámek ke zdroji dat. Informace, které jsou tady uvedené – včetně procesu toohello odkazy nebo kontaktní bod pro získání přístup ke zdroji dat – jsou poskytovány společně hello informace o umístění zdroje dat portálu hello.

 ![Informace o připojení se žádost o přístup pokynů](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Souhrn
Registrace k datovému zdroji prostřednictvím **Azure Data Catalog** díky dat zjistitelný zkopírováním strukturální a popisný metadat ze zdroje dat hello do hello katalogu služby. Jakmile zdroj dat má byl zaregistrován a zjistit, uživatelé se mohou připojit toohello zdroj dat z hello **Azure Data Catalog** portál "Otevřít v..." " nabídky nebo pomocí nástrojů jejich data výběru.

## <a name="see-also"></a>Viz také
* [Začínáme s Azure Data Catalog](data-catalog-get-started.md) kurz podrobné informace o tooconnect toodata zdroje.
