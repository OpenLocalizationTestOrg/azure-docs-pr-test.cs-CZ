---
title: "Poznámky k verzi aaaAzure katalogu Data Catalog | Microsoft Docs"
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
ms.openlocfilehash: 661826f66020ba72a885c6b14522b53c8b469d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-release-notes"></a><span data-ttu-id="7db26-103">Poznámky k verzi služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="7db26-103">Azure Data Catalog release notes</span></span>
## <a name="notes-for-hello-november-20-2015-release-of-azure-data-catalog"></a><span data-ttu-id="7db26-104">Poznámky k verzi 20. listopadu 2015 hello služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="7db26-104">Notes for hello November 20, 2015 release of Azure Data Catalog</span></span>
### <a name="opening-data-sources-in-power-bi-desktop"></a><span data-ttu-id="7db26-105">Otevírání zdroje dat v Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="7db26-105">Opening Data Sources in Power BI Desktop</span></span>
<span data-ttu-id="7db26-106">Při použití možnosti "Otevřít v Power BI Desktop" hello z hello **Azure Data Catalog** portálu, uživatelé setkat s jedním z dva problémy v aplikaci Power BI Desktop hello:</span><span class="sxs-lookup"><span data-stu-id="7db26-106">When using hello "Open in Power BI Desktop" option from hello **Azure Data Catalog** portal, users may encounter one of two problems in hello Power BI Desktop application:</span></span>

* <span data-ttu-id="7db26-107">Zobrazí se dialogové okno s názvem hello "Nelze tooOpen dokumentu"</span><span class="sxs-lookup"><span data-stu-id="7db26-107">A dialog box with hello title "Unable tooOpen Document" is displayed</span></span>
* <span data-ttu-id="7db26-108">Otevře Hello aplikace Power BI Desktop, ale soubor hello se zobrazí toobe prázdný</span><span class="sxs-lookup"><span data-stu-id="7db26-108">hello Power BI Desktop application opens, but hello file appears toobe empty</span></span>

<span data-ttu-id="7db26-109">Pro každé situaci hello problému stahuje a instaluje hello nejnovější verzi aplikace Power BI Desktop z [PowerBI.com](https://powerbi.com).</span><span class="sxs-lookup"><span data-stu-id="7db26-109">For each situation, hello problem can be resolved by downloading and installing hello latest version of Power BI Desktop from [PowerBI.com](https://powerbi.com).</span></span>

## <a name="notes-for-hello-november-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="7db26-110">Poznámky k verzi 13. listopadu 2015 hello služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="7db26-110">Notes for hello November 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-tooteradata"></a><span data-ttu-id="7db26-111">Registrace a připojení tooTeradata</span><span class="sxs-lookup"><span data-stu-id="7db26-111">Registering and connecting tooTeradata</span></span>
<span data-ttu-id="7db26-112">Při připojování tooTeradata zdroje dat uživatelům musí mít nainstalované správné ovladače Teradata ODBC hello odpovídající hello počtu bitů (32bitová nebo 64bitová verze) hello softwaru používán.</span><span class="sxs-lookup"><span data-stu-id="7db26-112">When connecting tooTeradata data sources users must have installed hello correct Teradata ODBC driver that match hello bitness (32-bit or 64-bit) of hello software being used.</span></span>

<span data-ttu-id="7db26-113">Od této ADC datum vydání, hello nejnovější [ovladač Teradata ODBC pro windows (verze 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) je kompatibilní s Office 2013, ale ne při Office 2016.</span><span class="sxs-lookup"><span data-stu-id="7db26-113">As of this ADC release date, hello most recent [Teradata ODBC driver for windows ( version 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatible with Office 2013, but not with Office 2016.</span></span>

## <a name="notes-for-hello-july-13-2015-release-of-azure-data-catalog"></a><span data-ttu-id="7db26-114">Poznámky k verzi 13. července 2015 hello služby Azure Data Catalog</span><span class="sxs-lookup"><span data-stu-id="7db26-114">Notes for hello July 13, 2015 release of Azure Data Catalog</span></span>
### <a name="registering-and-connecting-toooracle-database"></a><span data-ttu-id="7db26-115">Registrace a připojení tooOracle databáze</span><span class="sxs-lookup"><span data-stu-id="7db26-115">Registering and connecting tooOracle Database</span></span>
<span data-ttu-id="7db26-116">Při připojování uživatelů zdroje dat databáze tooOracle mají nainstalovánu hello správné Oracle ovladače, které odpovídají hello počtu bitů (32bitová nebo 64bitová verze) hello softwaru používán.</span><span class="sxs-lookup"><span data-stu-id="7db26-116">When connecting tooOracle Database data sources users must have installed hello correct Oracle drivers that match hello bitness (32-bit or 64-bit) of hello software being used.</span></span>

* <span data-ttu-id="7db26-117">Při registraci zdroje dat Oracle na počítači se systémem Windows 32-bit, použije se hello 32bitové Oracle ovladače</span><span class="sxs-lookup"><span data-stu-id="7db26-117">When registering Oracle data sources on a computer running 32-bit Windows, hello 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="7db26-118">Při registraci zdroje dat Oracle na počítači se systémem Windows 64-bit, použije se hello 64-bit Oracle ovladače</span><span class="sxs-lookup"><span data-stu-id="7db26-118">When registering Oracle data sources on a computer running 64-bit Windows, hello 64-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="7db26-119">Při připojování tooOracle zdroje dat pomocí aplikace Excel v počítači se systémem hello 32bitovou verzi systému Microsoft Office, včetně na 64bitovém systému Windows, bude použit hello 32bitové Oracle ovladače</span><span class="sxs-lookup"><span data-stu-id="7db26-119">When connecting tooOracle data sources using Excel on a computer running hello 32-bit version of Microsoft Office, including on 64-bit Windows, hello 32-bit Oracle drivers will be used</span></span>
* <span data-ttu-id="7db26-120">Při připojování tooOracle zdroje dat pomocí aplikace Excel v počítači se systémem hello 64bitová verze systému Microsoft Office, použije se hello 64-bit Oracle ovladače</span><span class="sxs-lookup"><span data-stu-id="7db26-120">When connecting tooOracle data sources using Excel on a computer running hello 64-bit version of Microsoft Office, hello 64-bit Oracle drivers will be used</span></span>

### <a name="registering-and-connecting-toosql-server-reporting-services"></a><span data-ttu-id="7db26-121">Registrace a připojení tooSQL Server Reporting Services</span><span class="sxs-lookup"><span data-stu-id="7db26-121">Registering and connecting tooSQL Server Reporting Services</span></span>
<span data-ttu-id="7db26-122">Podpora pro zdroje dat služby SQL Server Reporting Services (SSRS) je aktuálně omezená tooNative režimu pouze servery.</span><span class="sxs-lookup"><span data-stu-id="7db26-122">Support for SQL Server Reporting Services (SSRS) data sources is currently limited tooNative Mode servers only.</span></span> <span data-ttu-id="7db26-123">Podpora služby SSRS v režimu serveru SharePoint přidá v novější verzi.</span><span class="sxs-lookup"><span data-stu-id="7db26-123">Support for SSRS in SharePoint mode will be added in a later release.</span></span>

### <a name="opening-data-assets-in-excel"></a><span data-ttu-id="7db26-124">Otevírání datových prostředků v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="7db26-124">Opening data assets in Excel</span></span>
<span data-ttu-id="7db26-125">Při otevření datové prostředky v aplikaci Microsoft Excel z hello **Azure Data Catalog** portálu, může se uživatelům výzva s **oznámení o zabezpečení aplikace Microsoft Excel** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7db26-125">When opening data assets in Microsoft Excel from hello **Azure Data Catalog** portal, users may be prompted with a **Microsoft Excel Security Notice** dialog box.</span></span> <span data-ttu-id="7db26-126">Toto je standardní, můžete vybrat očekávané chování a uživatelé **povolit** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="7db26-126">This is standard, expected behavior, and users can select **Enable** toocontinue.</span></span>

<span data-ttu-id="7db26-127">Další informace najdete v tématu [povolit nebo zakázat výstrahy zabezpečení týkající se propojení a souborů z podezřelé weby](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span><span class="sxs-lookup"><span data-stu-id="7db26-127">For more information, see [Enable or disable security alerts about links and files from suspicious websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).</span></span>

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a><span data-ttu-id="7db26-128">Proxy server, zásad konfigurace a data registrace zdroje</span><span class="sxs-lookup"><span data-stu-id="7db26-128">Proxy and policy configuration and data source registration</span></span>
<span data-ttu-id="7db26-129">Uživatelé mohou nastat situace, kdy může přihlásit na portál Azure Data Catalog toohello, ale když se pokoušejí toolog na toohello nástroj registrace zdroje dat narazí chybovou zprávu, která brání protokolování.</span><span class="sxs-lookup"><span data-stu-id="7db26-129">Users may encounter a situation where they can log on toohello Azure Data Catalog portal, but when they attempt toolog on toohello data source registration tool they encounter an error message that prevents them from logging on.</span></span>

<span data-ttu-id="7db26-130">Existují dvě možné příčiny tohoto problému chování:</span><span class="sxs-lookup"><span data-stu-id="7db26-130">There are two potential causes for this problem behavior:</span></span>

<span data-ttu-id="7db26-131">**Příčina 1: Konfigurace služby Active Directory Federation Services** nástroj registrace zdroje dat hello používá ověřování pomocí formulářů toovalidate přihlášení uživatelů pro službu Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7db26-131">**Cause 1: Active Directory Federation Services configuration** hello data source registration tool uses Forms Authentication toovalidate user logons against Active Directory.</span></span> <span data-ttu-id="7db26-132">Pro úspěšné přihlášení musí být povolené ověřování pomocí formulářů ve hello globální zásady ověřování správcem služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7db26-132">For successful logon, Forms Authentication must be enabled in hello Global Authentication Policy by an Active Directory administrator.</span></span>

<span data-ttu-id="7db26-133">V některých situacích toto chování chybě může dojít pouze v případě, že uživatel hello je v síti společnosti hello nebo jenom v případě, že uživatel hello připojuje z mimo hello podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="7db26-133">In some situations, this error behavior may occur only when hello user is on hello company network, or only when hello user is connecting from outside hello company network.</span></span> <span data-ttu-id="7db26-134">Hello globální zásady ověřování umožňuje toobe metody ověřování pro intranet a extranet připojení povoleno samostatně.</span><span class="sxs-lookup"><span data-stu-id="7db26-134">hello Global Authentication Policy allows authentication methods toobe enabled separately for intranet and extranet connections.</span></span> <span data-ttu-id="7db26-135">Pokud je ověřování pomocí formulářů není povoleno pro hello síť, ze které hello se uživatel připojuje, může dojít k chybám přihlášení.</span><span class="sxs-lookup"><span data-stu-id="7db26-135">Logon errors may occur if Forms Authentication is not enabled for hello network from which hello user is connecting.</span></span>

<span data-ttu-id="7db26-136">Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="7db26-136">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>

<span data-ttu-id="7db26-137">**Příčina 2: Konfigurace proxy serveru sítě** Pokud hello podniková síť používá proxy server, nástroj pro registraci hello nemusí být možné tooconnect tooAzure služby Active Directory přes proxy server hello.</span><span class="sxs-lookup"><span data-stu-id="7db26-137">**Cause 2: Network proxy configuration** If hello corporate network uses a proxy server, hello registration tool may not be able tooconnect tooAzure Active Directory through hello proxy.</span></span> <span data-ttu-id="7db26-138">Uživatele můžete zajistit tento nástroj pro registraci hello úpravou konfiguračního souboru nástroje hello, přidání tohoto souboru toohello části:</span><span class="sxs-lookup"><span data-stu-id="7db26-138">Users can ensure that hello registration tool by editing hello tool’s configuration file, adding this section toohello file:</span></span>

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


<span data-ttu-id="7db26-139">toolocate hello RegistrationTool.exe.config souboru, spusťte nástroj pro registraci hello a pak otevřete nástroj Správce úloh systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="7db26-139">toolocate hello RegistrationTool.exe.config file, launch hello registration tool, and then open hello Windows Task Manager utility.</span></span> <span data-ttu-id="7db26-140">Na kartě Podrobnosti hello ve Správci úloh klikněte pravým tlačítkem na RegistrationTool.exe a hello rozbalovací nabídce vyberte příkaz Otevřít umístění souboru.</span><span class="sxs-lookup"><span data-stu-id="7db26-140">On hello Details tab in Task manager, right-click on RegistrationTool.exe and choose Open file location from hello pop-up menu.</span></span>
