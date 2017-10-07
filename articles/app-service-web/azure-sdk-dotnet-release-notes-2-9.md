---
title: "aaaAzure SDK pro .NET 2.9 poznámky"
description: "Poznámky k verzi sady Azure SDK pro .NET 2.9"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: c83d815b-fc19-4260-821e-7d2a7206dffc
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 96df2b80224190cc2093e6bf350eaec224ac2e98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sdk-for-net-29-release-notes"></a>Poznámky k verzi sady Azure SDK pro .NET 2.9

Toto téma obsahuje poznámky k verzi pro verze 2.9 a 2.9.6 sady Azure SDK pro .NET.

##<a name="azure-sdk-for-net-296-release-summary"></a>Azure SDK pro .NET 2.9.6 verze souhrn

Datum vydání: 11/16/2016
 
V této verzi byly zavedeny žádné toohello změny nejnovější sadu Azure SDK 2.9. Je také žádné tooleverage procesu upgradu potřeba tuto sadu SDK s existující projekty cloudové služby.

### <a name="visual-studio-2017-release-candidate"></a>Visual Studio 2017 Release Candidate

- V sadě Visual Studio 2017 RC tato verze hello Azure SDK pro .NET je součástí hello pracovního vytížení Azure. Všechny hello nástroje, které potřebujete toodo Azure development budou součástí Visual Studio RC 2017 do budoucna. Pro Visual Studio 2015 a Visual Studio 2013 hello SDK bude stále k dispozici prostřednictvím WebPI. Již Azure SDK pro .NET verze pro sadu Visual Studio 2013, když Visual Studio 2017 uvolní jako poslední produktu. Použijte tento odkaz toodownload Visual Studio 2017 RC: https://www.visualstudio.com/vs/visual-studio-2017-rc/

### <a name="azure-diagnostics"></a>Diagnostika Azure

- Změněné hello chování tooonly ukládat částečné připojovací řetězec s klíčem hello nahrazuje token pro připojovací řetězec úložiště diagnostiky cloudové služby. klíč Hello skutečné úložiště jsou teď uložená v složky profilu uživatele hello tak přístup se dá řídit. Visual Studio, bude číst klíč úložiště hello ze složky profilu uživatele pro místní ladění a proces publikování. 
- V odpovědi toohello změn popsané výše Visual Studio Online team rozšířené hello šablonu úloh nasazení Azure Cloud Services, uživatelé mohou zadat klíč hello úložiště pro nastavení rozšíření diagnostiky při publikování tooAzure v průběžnou integraci a nasazení.
- Provedli jsme ji možné toostore zabezpečené připojovací řetězec a tokenizaci pro Azure Diagnostics (WAD), toohelp řešení problémů s konfigurací napříč environements.
 
### <a name="windows-server-2016-virtual-machines"></a>Windows Server 2016 virtuální počítače

- Visual Studio teď podporuje nasazení cloudové služby tooOS rodiny 5 (Windows Server 2016) virtuálních počítačů. Pro existující cloudové služby, můžete změnit vaše nastavení tootarget hello nové řada operačního systému. Při vytváření nových cloudových služeb, pokud se rozhodnete toocreate hello služby pomocí rozhraní .net 4.6 nebo vyšší, bude použita výchozí služba toouse hello operačního systému rodiny 5.  Další informace, můžete zkontrolovat hello [skupina hostovaných operačních systémů podporují tabulky](https://azure.microsoft.com/en-us/documentation/articles/cloud-services-guestos-update-matrix/).

#### <a name="known-issues"></a>Známé problémy

- Azure .NET SDK 2.9.6 zavedená omezení, které blokuje nasazení projektů pomocí nepodporované .NET rozhraní (například .NET 4.6) tooany operačních systémů < 5. Alternativní řešení [zde](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9).

 
### <a name="azure-in-role-cache"></a>Mezipaměť hostovaná v instanci Role na Azure 

- Podpora pro Azure Cache v roli končí na 30. listopadu 2016. Další podrobnosti získáte kliknutím na [zde](https://azure.microsoft.com/en-us/blog/azure-managed-cache-and-in-role-cache-services-to-be-retired-on-11-30-2016/).

### <a name="azure-resource-manager-templates-for-azure-stack"></a>Šablony Azure Resource Manageru pro Azure zásobníku

- Zavedli jsme zásobník Azure jako cíl nasazení pro vaše šablony Azure Resource Manager.


## <a name="azure-sdk-for-net-29-summary"></a>Azure SDK pro .NET 2.9 souhrn

## <a name="overview"></a>Přehled
Tento dokument obsahuje hello poznámky k verzi pro hello Azure SDK pro .NET 2.9 verzi. 

Podrobné informace o aktualizacích v této verzi najdete v tématu hello [sadu Azure SDK 2.9 oznámení post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 pro Visual Studio 2015 Update 2 a Visual Studio "15" Preview
Tato aktualizace zahrnuje hello následující opravy chyb:

* Vystavení související tooREST rozhraní API klienta generování v které hello řetězce "Neznámý typ" se zobrazí jako hello název složky kód generace hello nebo hello název oboru názvů hello do hello vygenerovat kód.
* Vydejte související tooScheduled webové úlohy, ve které hello došlo ověřovací informace k selhání toobe předán procesu zřizování toohello plánovače.

Tato aktualizace zahrnuje hello následující nové funkce:

* Podpora pro sekundární App Services kartě "Služby" hello hello dialogové okno zřizování App Service. 

## <a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Nástroje Azure Data Lake pro Visual Studio 2015 Update 2
Tato aktualizace zahrnuje následující hello:

* **Nástroje Azure Data Lake** pro Visual Studio je nyní sloučeny do hello Azure SDK pro .NET verze. Nástroj Hello se automaticky nainstaluje při instalaci sady Azure SDK. 
  
    Hello nástroj se často aktualizuje, přejděte [sem](http://aka.ms/datalaketool) tooget hello aktualizace.
* **V Průzkumníku serveru** nyní vám umožní tooview všechny nebo vytvořit některé entity metadata U-SQL. Další informace najdete v tématu [to](https://azure.microsoft.com/documentation/services/data-lake-analytics/) blogu.

## <a name="hdinsight-tools"></a>Nástroje HDInsight
**Nástroje HDInsight** pro sadu Visual Studio teď podporuje HDInsight verze 3.3, včetně grafech Tez a dalších jazyků opravy.

## <a name="azure-resource-manager"></a>Azure Resource Manager
Tato verze přidává [KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) podporu pro šablony Resource Manageru.

## <a name="see-also"></a>Viz také
[Azure SDK 2.9 oznámení post](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)

