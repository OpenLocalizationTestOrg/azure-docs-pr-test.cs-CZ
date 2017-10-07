---
title: "aaaAutomate nasazení vaší aplikace Azure pomocí nástroje příkazového řádku | Microsoft Docs"
description: "Zjistit informace o tom, jak můžete nasadit aplikaci Azure z příkazového řádku hello"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 8b65980c-eb75-44a2-8e0f-f9eb9e617d16
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: 3df66cc4bf4e6819ed0eee7278ac79dca2e5daa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Automatizovat nasazení vaší aplikace Azure pomocí nástroje příkazového řádku
Je možné automatizovat nasazení aplikace Azure pomocí nástroje příkazového řádku. Tento článek obsahuje seznam nástrojů dostupných a hello užitečné odkazy, které ukazují, jak toouse je v pracovním postupu nasazení. 

## <a name="msbuild"></a>Automatizovat nasazení pomocí nástroje MSBuild
Pokud používáte hello [Visual Studio IDE](#vs) pro vývoj, můžete použít [MSBuild](http://msbuildbook.com/) tooautomate nic můžete provést ve vaší IDE. Můžete nakonfigurovat buď MSBuild toouse [Web Deploy](#webdeploy) nebo [FTP/FTPS](#ftp) toocopy soubory. Nasazení webu také můžete automatizovat celou řadu dalších týkající se nasazení úloh, jako je nasazení databáze.

Další informace o použití nástroje MSBuild příkazového řádku nasazení najdete v tématu hello následující prostředky:

* [Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Desetinu v řadě kurzy o tooAzure nasazení pomocí sady Visual Studio. Ukazuje, jak toouse hello příkazového řádku toodeploy po nastavení Profily publikování v sadě Visual Studio.
* [Uvnitř hello Microsoft Build Engine: pomocí nástroje MSBuild a Team Foundation Build](http://msbuildbook.com/). Tištěné adresáře, která zahrnuje kapitolám o tom, toouse MSBuild pro nasazení.

## <a name="powershell"></a>Automatizovat nasazení v prostředí Windows PowerShell
Můžete provést nasazení funkce MSBuild nebo FTP z [prostředí Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx). Pokud tak učiníte, můžete také použít kolekci rutin prostředí Windows PowerShell, který toocall snadné správy rozhraní API Azure REST pro hello.

Další informace najdete v tématu hello následující prostředky:

* [Nasazení webové aplikace propojené tooa Githubu úložiště](app-service-web-arm-from-github-provision.md)
* [Zřízení webové aplikace s databází SQL](app-service-web-arm-with-sql-database-provision.md)
* [Zřídit a nasadit mikroslužeb předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Vytváření reálných cloudových aplikací s Azure - automatizovat všechno, co](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). Elektronická kniha kapitoly, která vysvětluje, jak ukázková aplikace hello ukazuje elektronická kniha hello používá prostředí Windows PowerShell skripty toocreate Azure testovací prostředí a nasaďte tooit. V tématu hello [prostředky](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) části odkazy tooadditional dokumentace Azure PowerShell.
* [Pomocí skriptů Windows PowerShell tooPublish tooDev a testovací prostředí](../vs-azure-tools-publishing-using-powershell-scripts.md). Jak toouse nasazení prostředí Windows PowerShell skripty sadou Visual Studio generuje.

## <a name="api"></a>Automatizovat nasazení pomocí rozhraní API pro správu rozhraní .NET
Můžete napsat kód C# tooperform MSBuild nebo FTP funkce pro nasazení. Pokud tak učiníte, můžete přistupovat funkce správy lokality tooperform REST API pro správu Azure hello.

Další informace najdete v tématu hello následujících prostředků:

* [Všechno, co automatizace pomocí knihovny správy hello Azure a .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Úvod toohello .NET správa toomore dokumentace k rozhraní API a odkazy.

## <a name="cli"></a>Nasazení z Azure rozhraní příkazového řádku (Azure CLI)
Hello příkazového řádku můžete v systému Windows, Mac nebo Linux toodeploy počítačů pomocí protokolu FTP. Pokud tak učiníte, můžete taky přejít hello Azure REST rozhraní API pro správu pomocí hello rozhraní příkazového řádku Azure.

Další informace najdete v tématu hello následujících prostředků:

* [Nástroje příkazového řádku Azure](https://azure.microsoft.com/downloads/). Stránka portálu v Azure.com informace nástroj příkazového řádku.

## <a name="webdeploy"></a>Nasazení z příkazového řádku nástroje nasazení webu
[Nasazení webové](http://www.iis.net/downloads/microsoft/web-deploy) je software společnosti Microsoft pro tooIIS nasazení, které nejen poskytuje inteligentního souboru synchronizovat funkce, ale také můžete provést nebo koordinaci mnoho dalších týkající se nasazení úlohy, které nelze automatizované při použití serveru FTP. Například nasazení webu můžete nasadit novou databázi nebo aktualizace databáze spolu s vaší webové aplikace. Nasazení webu také minimalizovat čas potřebný tooupdate hello existující lokalitu vzhledem k tomu, že ho můžete zkopírovat inteligentně pouze změněné soubory. Microsoft Visual Studio a Team Foundation Server je podpora pro nasazení webu předdefinované, ale můžete také Web Deploy přímo z nasazení tooautomate hello příkazového řádku. Příkazy nasadit webové jsou velmi výkonné, ale hello může být jeho zvládnutí.

## <a name="more-resources"></a>Další zdroje informací
Jiné automatizace toocommand-line možnost nasazení, jako je toouse cloudové služby [Chobotnice nasazení](http://en.wikipedia.org/wiki/Octopus_Deploy). Další informace najdete v tématu [tooAzure aplikace ASP.NET nasazení webů](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Další informace o nástrojích příkazového řádku najdete v tématu hello následujících prostředků:

* [Jednoduché webové aplikace: Nasazení](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog podle David Ebbo o nástroji napsal toomake je snazší toouse nasazení webu.
* [Webové nástroje pro nasazení](http://technet.microsoft.com/library/dd568996). Oficiální dokumentaci na webu Microsoft TechNet hello. Ze ale stále toostart vhodné místo.
* [Pomocí webové nasadit](http://www.iis.net/learn/publish/using-web-deploy). Oficiální dokumentaci na webu Microsoft IIS.NET hello. Také ze ale toostart vhodné místo.
* [Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení příkazového řádku](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). MSBuild je hello sestavení modul využívá sada Visual Studio a dá se taky použít z hello příkazového řádku toodeploy webové aplikace tooWeb aplikace. V tomto kurzu je součástí série, která je hlavně o nasazení sady Visual Studio.

