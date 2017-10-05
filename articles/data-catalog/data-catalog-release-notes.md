---
title: "Poznámky k verzi služby Azure Data Catalog | Microsoft Docs"
description: "Poznámky k verzi pro Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d3db9bee0558cac5fb4f5b8fb4ab67a35ce0f141
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-release-notes"></a><span data-ttu-id="e70d1-103">Poznámky k verzi služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="e70d1-103">Azure Data Catalog release notes</span></span>
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e70d1-104">Poznámky pro 20. listopadu 2015 verzi služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="e70d1-104">Notes for the November 20, 2015 release of Azure Data Catalog</span></span>
### <a name="opening-data-sources-in-power-bi-desktop"></a><span data-ttu-id="e70d1-105">Otevírání zdroje dat v Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="e70d1-105">Opening Data Sources in Power BI Desktop</span></span>
<span data-ttu-id="e70d1-106">Při použití volby "Otevřít v Power BI Desktop" z **Azure Data Catalog** portálu, uživatelé setkat s jedním z dva problémy v aplikaci Power BI Desktop:</span><span class="sxs-lookup"><span data-stu-id="e70d1-106">When using the "Open in Power BI Desktop" option from the **Azure Data Catalog** portal, users may encounter one of two problems in the Power BI Desktop application:</span></span>

* <span data-ttu-id="e70d1-107">Zobrazí se dialogové okno s názvem "Nelze pro otevřete dokument"</span><span class="sxs-lookup"><span data-stu-id="e70d1-107">A dialog box with the title "Unable to Open Document" is displayed</span></span>
* <span data-ttu-id="e70d1-108">Otevře aplikace Power BI Desktop, ale soubor zdá být prázdné.</span><span class="sxs-lookup"><span data-stu-id="e70d1-108">The Power BI Desktop application opens, but the file appears to be empty</span></span>

<span data-ttu-id="e70d1-109">Pro každé situaci lze vyřešit problém stahuje a instaluje nejnovější verze Power BI Desktop z [PowerBI.com](https://powerbi.com).</span><span class="sxs-lookup"><span data-stu-id="e70d1-109">For each situation, the problem can be resolved by downloading and installing the latest version of Power BI Desktop from [PowerBI.com](https://powerbi.com).</span></span>

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e70d1-110">Poznámky pro 13. listopadu 2015 verzi služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="e70d1-110">Notes for the November 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-to-teradata"></a><span data-ttu-id="e70d1-111">Registrace a připojení k Teradata</span><span class="sxs-lookup"><span data-stu-id="e70d1-111">Registering and connecting to Teradata</span></span>
<span data-ttu-id="e70d1-112">Při připojování k Teradata zdroje dat uživatelům musí mít nainstalované správné ovladače Teradata ODBC odpovídající bitová verze (32bitová nebo 64bitová verze) softwaru používán.</span><span class="sxs-lookup"><span data-stu-id="e70d1-112">When connecting to Teradata data sources users must have installed the correct Teradata ODBC driver that match the bitness (32-bit or 64-bit) of the software being used.</span></span>

<span data-ttu-id="e70d1-113">Od této ADC datum vydání, nejnovější [ovladač Teradata ODBC pro windows (verze 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) je kompatibilní s Office 2013, ale ne při Office 2016.</span><span class="sxs-lookup"><span data-stu-id="e70d1-113">As of this ADC release date, the most recent [Teradata ODBC driver for windows ( version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatible with Office 2013, but not with Office 2016.</span></span>

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="e70d1-114">Poznámky pro 13. července 2015 verzi služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="e70d1-114">Notes for the July 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-to-oracle-database"></a><span data-ttu-id="e70d1-115">Registrace a připojení k databázi Oracle</span><span class="sxs-lookup"><span data-stu-id="e70d1-115">Registering and connecting to Oracle Database</span></span>
<span data-ttu-id="e70d1-116">Při připojování ke zdrojům dat Oracle Database uživatelům musí mít nainstalované správné ovladače Oracle odpovídající bitová verze (32bitová nebo 64bitová verze) softwaru používá.</span><span class="sxs-lookup"><span data-stu-id="e70d1-116">When connecting to Oracle Database data sources users must have installed the correct Oracle drivers that match the bitness (32-bit or 64-bit) of the software being used.</span></span>

* <span data-ttu-id="e70d1-117">Při registraci zdroje dat Oracle na počítači se systémem Windows 32-bit, použije se 32bitové ovladače Oracle</span><span class="sxs-lookup"><span data-stu-id="e70d1-117">When registering Oracle data sources on a computer running 32-bit Windows, the 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e70d1-118">Při registraci zdroje dat Oracle na počítači se systémem Windows 64-bit, použije se 64bitové ovladače Oracle</span><span class="sxs-lookup"><span data-stu-id="e70d1-118">When registering Oracle data sources on a computer running 64-bit Windows, the 64-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e70d1-119">Při připojování ke zdrojům dat Oracle pomocí aplikace Excel v počítači se systémem 32bitovou verzi systému Microsoft Office, včetně na 64bitovém systému Windows, bude použit 32bitové ovladače Oracle</span><span class="sxs-lookup"><span data-stu-id="e70d1-119">When connecting to Oracle data sources using Excel on a computer running the 32-bit version of Microsoft Office, including on 64-bit Windows, the 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="e70d1-120">Při připojení ke zdrojům dat Oracle pomocí aplikace Excel v počítači se systémem 64bitové verze systému Microsoft Office, budou použity ovladače Oracle 64-bit</span><span class="sxs-lookup"><span data-stu-id="e70d1-120">When connecting to Oracle data sources using Excel on a computer running the 64-bit version of Microsoft Office, the 64-bit Oracle drivers will be used</span></span>

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a><span data-ttu-id="e70d1-121">Registrace a připojení k SQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="e70d1-121">Registering and connecting to SQL Server Reporting Services</span></span>
<span data-ttu-id="e70d1-122">Podpora pro zdroje dat služby SQL Server Reporting Services (SSRS) je aktuálně omezená nativního režimu jen pro servery.</span><span class="sxs-lookup"><span data-stu-id="e70d1-122">Support for SQL Server Reporting Services (SSRS) data sources is currently limited to Native Mode servers only.</span></span> <span data-ttu-id="e70d1-123">Podpora služby SSRS v režimu serveru SharePoint přidá v novější verzi.</span><span class="sxs-lookup"><span data-stu-id="e70d1-123">Support for SSRS in SharePoint mode will be added in a later release.</span></span>

### <a name="opening-data-assets-in-excel"></a><span data-ttu-id="e70d1-124">Otevírání datových prostředků v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="e70d1-124">Opening data assets in Excel</span></span>
<span data-ttu-id="e70d1-125">Při otevírání datových prostředků v aplikaci Microsoft Excel z **Azure Data Catalog** portálu, může se uživatelům výzva s **oznámení o zabezpečení aplikace Microsoft Excel** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e70d1-125">When opening data assets in Microsoft Excel from the **Azure Data Catalog** portal, users may be prompted with a **Microsoft Excel Security Notice** dialog box.</span></span> <span data-ttu-id="e70d1-126">Toto je standardní, můžete vybrat očekávané chování a uživatelé **povolit** pokračujte.</span><span class="sxs-lookup"><span data-stu-id="e70d1-126">This is standard, expected behavior, and users can select **Enable** to continue.</span></span>

<span data-ttu-id="e70d1-127">Další informace najdete v tématu [povolit nebo zakázat výstrahy zabezpečení týkající se propojení a souborů z podezřelé weby](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span><span class="sxs-lookup"><span data-stu-id="e70d1-127">For more information, see [Enable or disable security alerts about links and files from suspicious websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span></span>

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a><span data-ttu-id="e70d1-128">Proxy server, zásad konfigurace a data registrace zdroje</span><span class="sxs-lookup"><span data-stu-id="e70d1-128">Proxy and policy configuration and data source registration</span></span>
<span data-ttu-id="e70d1-129">Uživatelé mohou nastat situace, kdy se nemohou přihlásit k portálu Azure Data Catalog, ale při pokusu o přihlášení k nástroj registrace zdroje dat narazí chybovou zprávu, která je zabraňuje v přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e70d1-129">Users may encounter a situation where they can log on to the Azure Data Catalog portal, but when they attempt to log on to the data source registration tool they encounter an error message that prevents them from logging on.</span></span>

<span data-ttu-id="e70d1-130">Existují dvě možné příčiny tohoto problému chování:</span><span class="sxs-lookup"><span data-stu-id="e70d1-130">There are two potential causes for this problem behavior:</span></span>

<span data-ttu-id="e70d1-131">**Příčina 1: Konfigurace služby Active Directory Federation Services** nástroj registrace zdroje dat používá ověřování založené na formulářích k ověření přihlášení uživatelů pro službu Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e70d1-131">**Cause 1: Active Directory Federation Services configuration** The data source registration tool uses Forms Authentication to validate user logons against Active Directory.</span></span> <span data-ttu-id="e70d1-132">Pro úspěšné přihlášení musí být povolené ověřování pomocí formulářů ve globální zásady ověřování správcem služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e70d1-132">For successful logon, Forms Authentication must be enabled in the Global Authentication Policy by an Active Directory administrator.</span></span>

<span data-ttu-id="e70d1-133">V některých situacích může dojít toto chování chyba, pouze pokud je uživatel v podnikové síti, nebo jenom v případě, že se uživatel připojuje z vnějšku firemní sítě.</span><span class="sxs-lookup"><span data-stu-id="e70d1-133">In some situations, this error behavior may occur only when the user is on the company network, or only when the user is connecting from outside the company network.</span></span> <span data-ttu-id="e70d1-134">Globální zásady ověřování umožňuje metody ověřování pro intranetové a připojení k síti extranet povolení samostatně.</span><span class="sxs-lookup"><span data-stu-id="e70d1-134">The Global Authentication Policy allows authentication methods to be enabled separately for intranet and extranet connections.</span></span> <span data-ttu-id="e70d1-135">Pokud je ověřování pomocí formulářů není povoleno pro síť, ze kterého se uživatel připojuje, může dojít k chybám přihlášení.</span><span class="sxs-lookup"><span data-stu-id="e70d1-135">Logon errors may occur if Forms Authentication is not enabled for the network from which the user is connecting.</span></span>

<span data-ttu-id="e70d1-136">Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="e70d1-136">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>

<span data-ttu-id="e70d1-137">**Příčina 2: Konfigurace proxy serveru sítě** Pokud podniková síť používá proxy server, nástroj pro registraci nemusí být možné se připojit k Azure Active Directory prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="e70d1-137">**Cause 2: Network proxy configuration** If the corporate network uses a proxy server, the registration tool may not be able to connect to Azure Active Directory through the proxy.</span></span> <span data-ttu-id="e70d1-138">Uživatele můžete zajistit, aby nástroj pro registraci úpravou konfiguračního souboru nástroje, přidání této části do souboru:</span><span class="sxs-lookup"><span data-stu-id="e70d1-138">Users can ensure that the registration tool by editing the tool’s configuration file, adding this section to the file:</span></span>

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


<span data-ttu-id="e70d1-139">Vyhledejte soubor RegistrationTool.exe.config, spusťte nástroj pro registraci a pak otevřete nástroj Správce úloh systému Windows.</span><span class="sxs-lookup"><span data-stu-id="e70d1-139">To locate the RegistrationTool.exe.config file, launch the registration tool, and then open the Windows Task Manager utility.</span></span> <span data-ttu-id="e70d1-140">Na kartě Podrobnosti ve Správci úloh klikněte pravým tlačítkem na RegistrationTool.exe a rozbalovací nabídce vyberte příkaz Otevřít umístění souboru.</span><span class="sxs-lookup"><span data-stu-id="e70d1-140">On the Details tab in Task manager, right-click on RegistrationTool.exe and choose Open file location from the pop-up menu.</span></span>
