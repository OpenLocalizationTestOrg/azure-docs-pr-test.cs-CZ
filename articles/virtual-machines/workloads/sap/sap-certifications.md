---
title: Certifikace Microsoft Azure pro SAP | Microsoft Docs
description: "Aktualizovaný seznam aktuální konfigurace a certifikátů SAP na platformě Azure."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/29/2017
ms.author: rclaus
ms.custom: 
ms.openlocfilehash: e4d5c78299903659a18aa9b8d04495e215bcca0d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>Certifikace SAP a konfigurace systémem Microsoft Azure

SAP a Microsoft mají dlouhou spolupráci ve silné spolupráci, která má oboustranně zákazníkům. Společnost Microsoft průběžně aktualizuje jeho platforma a odeslání nové podrobnosti certifikační SAP za účelem zajištění Microsoft Azure je nejlepší platformě, na které se mají spouštět úlohy SAP. Následující tabulky popisují naše podporované konfigurace a seznam narůstají certifikace. 

## <a name="sap-hana-certifications"></a>Certifikace SAP HANA

| Produkt SAP | Podporovaný operační systém | Nabídky Azure |
| --- | --- | --- |
| SAP HANA Developer Edition (včetně klientský software HANA skládá z SQLODBC, pouze ODBO – Windows, rozhraní ODBC, ovladače JDBC, HANA studio a HANA databáze) |Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise | Počítač D-Series rodiny |
| HANA One |Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise |DS14_v2 (při všeobecné dostupnosti) |
| SAP HANA S NEBO 4 |Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise |Řízené dostupnost pro GS5, SAP HANA v Azure (velké instance) |
| Suite on HANA, OLTP |Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise |GS5 pro nasazení jednoho uzlu pro scénáře nevýrobní prostředí SAP HANA v Azure (velké instance) |
| HANA Enterprise for BW, OLAP |Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise |GS5 pro jeden uzel nasazení SAP HANA v Azure (velké instance) |
| SAP HANA BW NEBO 4 |Red Hat Enterprise Linux operačního systému SUSE Linux Enterprise |GS5 pro jeden uzel nasazení SAP HANA v Azure (velké instance) |

## <a name="sap-netweaver-certifications"></a>Certifikace SAP NetWeaver
Microsoft Azure má certifikaci pro následující produkty SAP, se zárukou plné podpory od Microsoftu i SAPu.

| Produkt SAP | Hostovaného operačního systému | RDBMS | Typy virtuálních počítačů |
| --- | --- | --- | --- |
| SAP Business Suite Software |Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows a Oracle Linux pouze), DB2, SAP App Service Environment |A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5 |
| SAP Business All-in-One |Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (Windows a Oracle Linux pouze), DB2, SAP App Service Environment |A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5 |
| SAP BusinessObjects BI |Windows |Není k dispozici |A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5 |
| SAP NetWeaver |Windows, operačního systému SUSE Linux Enterprise, Red Hat Enterprise Linux |SQL Server, Oracle (Windows a Oracle Linux pouze), DB2, SAP App Service Environment |A5 na A11 D11 k D14 DS11 k DS14, DS11_v2 k DS15_v2, spravuje organizace GS1 k GS5 |
