---
title: "aaaClient knihovny potřebné pro připojení služby Analysis Services tooAzure | Microsoft Docs"
description: "Popisuje klienta knihovny potřebné pro klientské aplikace a nástroje tooconnect Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 74ba5c05ba76c6587c5aed38f200a1ba469aa4f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="client-libraries-for-connecting-tooazure-analysis-services"></a>Klientské knihovny pro připojení tooAzure Analysis Services

Knihovny klienta jsou nezbytné pro nástroje tooconnect tooAnalysis služby servery a klientské aplikace. 

Služba Analysis Services, používají tři knihovny klienta. ADOMD.NET a Analysis Services Management Objects (AMO), jsou spravované klientské knihovny. Zprostředkovatel OLE DB služby Analysis Services Hello (MSOLAP DLL) je nativní klientské knihovny. Obvykle jsou všechny tři nainstalovány na hello stejnou dobu. Azure Analysis Services vyžaduje hello nejnovější verze. 

Microsoft klientské aplikace, třeba Power BI Desktop a Excel nainstalovat všechny tři klientské knihovny. V závislosti na verzi hello nebo četnost aktualizací, ale klientské knihovny nemusí být hello nejnovější verze vyžadované službou Azure Analysis Services. Hello totéž platí i toocustom aplikace nebo dalších rozhraní, jako je například AsCmd, TNÍ, ADOMD.NET. Tyto aplikace vyžadují ruční instalaci hello knihovny. Hello klientské knihovny pro ruční instalaci jsou zahrnuty v balíčcích funkcí systému SQL Server jako distribuovatelného balíčky. Tyto knihovny klienta však jsou vázané toohello verze systému SQL Server a nemusí být hello nejnovější.  

Klientské knihovny pro připojení klientů se liší od požadované tooconnect zprostředkovatelé dat ze zdroje dat tooa serveru Azure Analysis Services. toolearn Další informace o připojení zdroje dat, najdete v části [připojení zdroje dat](analysis-services-datasource.md).

## <a name="download-hello-latest-client-libraries"></a>Stáhněte si nejnovější knihovny klienta hello  
Použijte následující klientské knihovny, pokud jsou v produkčním prostředí a nevyžadují plně vydaná a podporovaných verzích hello.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Další kroky
[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)    
[Propojení s Power BI](analysis-services-connect-pbi.md)
