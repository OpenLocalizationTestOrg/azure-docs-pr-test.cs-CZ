---
title: "aaaConnect tooAzure služby Analysis Services | Microsoft Docs"
description: "Zjistěte, jak tooconnect tooand načíst data ze serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 5df94492feb48034f156b72e83e1009683988fc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-analysis-services-server"></a>Připojení serveru Azure Analysis Services tooan

Tento článek popisuje připojování tooa serveru pomocí modelování dat a správu aplikace jako SQL Server Management Studio (SSMS) nebo SQL Server Data Tools (SSDT). Nebo s klientem služby generování sestav aplikace, jako je Microsoft Excel, Power BI Desktop nebo vlastních aplikací. Připojení tooAzure Analysis Services používat protokol HTTPS.

## <a name="client-libraries"></a>Klientské knihovny
[Získat nejnovější knihovny klienta hello](analysis-services-data-providers.md)

Všechny server tooa připojení, bez ohledu na typ, vyžadovat aktualizované AMO ADOMD.NET a OLEDB knihovny tooconnect tooand rozhraní klienta se serverem služby Analysis Services. Pro aplikace SSMS, rozšíření SSDT, aplikace Excel 2016 a Power BI knihovny klienta nejnovější hello instalaci nebo aktualizaci s měsíční verzemi. V některých případech je však možné že Hello nemusí mít nejnovější aplikace. Například když zásady zpoždění aktualizace nebo aktualizace Office 365 jsou na hello odložené kanálu.

## <a name="server-name"></a>Název serveru

Při vytváření serveru služby Analysis Services v Azure, zadejte jedinečný název a hello oblasti, kde hello server je toobe vytvořen. Při zadání názvu serveru hello v připojení, je schéma pojmenování server hello:

```
<protocol>://<region>/<servername>
```
 Kde protokol je řetězec **asazure**, je oblast hello identifikátor Uri, které bylo vytvořeno hello serveru (například westus.asazure.windows.net) a servername je název hello jedinečný serveru v rámci oblasti hello.

### <a name="get-hello-server-name"></a>Získat název serveru hello
V **portál Azure** > serveru > **přehled** > **název serveru**, název kopie hello celý server. Pokud ostatní uživatelé ve vaší organizaci příliš připojení toothis serveru, můžete tento název serveru sdílet s nimi. Při zadání názvu serveru, se musí použít celou cestu hello.

![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Připojovací řetězec

Při připojování tooAzure Analysis Services pomocí hello tabulkový objektový Model, použijte hello následující formáty řetězců připojení:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrované ověřování služby Azure Active Directory
Integrované ověřování převezme hello mezipaměti přihlašovacích údajů Azure Active Directory, pokud je k dispozici. V opačném případě se zobrazí okno hello přihlášení k Azure.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory ověřování pomocí uživatelského jména a hesla

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Ověřování systému Windows (integrované zabezpečení)
Použijte účet Windows hello systémem hello aktuálním procesu.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Připojit pomocí souboru ODC
Ve starších verzích aplikace Excel uživatelé tooan Azure Analysis Services server připojte pomocí souboru (ODC) Office Data Connection. Další, najdete v části toolearn [vytvořit soubor dat ODC (Office Connection)](analysis-services-odc.md).


## <a name="next-steps"></a>Další kroky
[Připojit pomocí aplikace Excel](analysis-services-connect-excel.md)    
[Připojit k Power BI](analysis-services-connect-pbi.md)   
[Správa serveru](analysis-services-manage.md)   

