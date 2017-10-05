---
title: "Běžné příčiny cloudové služby rolí recyklace | Microsoft Docs"
description: "Cloudové služby role, kterou najednou recykluje může způsobit významné výpadky. Tady jsou některé běžné problémy, které způsobí role recyklace, které mohou pomoci zkrátit dobu prostojů."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 533930d1-8035-4402-b16a-cf887b2c4f85
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e55009c72b977ee4a30f6c71043bde483849f78f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="common-issues-that-cause-roles-to-recycle"></a><span data-ttu-id="17cd0-104">Běžné potíže, které můžou způsobit recyklaci rolí</span><span class="sxs-lookup"><span data-stu-id="17cd0-104">Common issues that cause roles to recycle</span></span>
<span data-ttu-id="17cd0-105">Tento článek popisuje některé běžné příčiny problémů s nasazením a poskytuje tipy k řešení potíží k řešení těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="17cd0-105">This article discusses some of the common causes of deployment problems and provides troubleshooting tips to help you resolve these problems.</span></span> <span data-ttu-id="17cd0-106">Jako ukazatel toho, zda existuje problém s aplikací je, když se nepodaří spustit instanci role, nebo ji cyklů mezi stavy inicializace, zaneprázdněn a ukončení.</span><span class="sxs-lookup"><span data-stu-id="17cd0-106">An indication that a problem exists with an application is when the role instance fails to start, or it cycles between the initializing, busy, and stopping states.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a><span data-ttu-id="17cd0-107">Chybějící závislosti modulu runtime</span><span class="sxs-lookup"><span data-stu-id="17cd0-107">Missing runtime dependencies</span></span>
<span data-ttu-id="17cd0-108">Pokud roli v aplikaci závisí na žádném sestavení, které není součástí rozhraní .NET Framework nebo Azure spravovanou knihovnu, je nutné explicitně zahrnout toto sestavení v balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="17cd0-108">If a role in your application relies on any assembly that is not part of the .NET Framework or the Azure managed library, you must explicitly include that assembly in the application package.</span></span> <span data-ttu-id="17cd0-109">Uvědomte si, že nejsou dostupné v Azure ve výchozím nastavení ostatní platformy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="17cd0-109">Keep in mind that other Microsoft frameworks are not available on Azure by default.</span></span> <span data-ttu-id="17cd0-110">Pokud vaše role závisí na takových rozhraní, musíte přidat tyto sestavení do balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="17cd0-110">If your role relies on such a framework, you must add those assemblies to the application package.</span></span>

<span data-ttu-id="17cd0-111">Než sestavení a balíček aplikace, ověřte následující:</span><span class="sxs-lookup"><span data-stu-id="17cd0-111">Before you build and package your application, verify the following:</span></span>

* <span data-ttu-id="17cd0-112">Pokud pomocí sady Visual studio, zkontrolujte **místní kopie** je nastavena na **True** pro každé odkazované sestavení v projektu, který není součástí sady Azure SDK nebo rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="17cd0-112">If using Visual studio, make sure the **Copy Local** property is set to **True** for each referenced assembly in your project that is not part of the Azure SDK or the .NET Framework.</span></span>
* <span data-ttu-id="17cd0-113">Ujistěte se, že v souboru web.config neodkazuje na žádné nepoužité sestavení v elementu kompilace.</span><span class="sxs-lookup"><span data-stu-id="17cd0-113">Make sure the web.config file does not reference any unused assemblies in the compilation element.</span></span>
* <span data-ttu-id="17cd0-114">**Akce sestavení** z každé .cshtml soubor pro **obsahu**.</span><span class="sxs-lookup"><span data-stu-id="17cd0-114">The **Build Action** of every .cshtml file is set to **Content**.</span></span> <span data-ttu-id="17cd0-115">To zajistí, že soubory se zobrazí správně v balíčku a umožňuje další odkazované soubory se objeví v balíčku.</span><span class="sxs-lookup"><span data-stu-id="17cd0-115">This ensures that the files will appear correctly in the package and enables other referenced files to appear in the package.</span></span>

## <a name="assembly-targets-wrong-platform"></a><span data-ttu-id="17cd0-116">Sestavení cílů chybné platformě.</span><span class="sxs-lookup"><span data-stu-id="17cd0-116">Assembly targets wrong platform</span></span>
<span data-ttu-id="17cd0-117">Azure je prostředí na 64-bit.</span><span class="sxs-lookup"><span data-stu-id="17cd0-117">Azure is a 64-bit environment.</span></span> <span data-ttu-id="17cd0-118">Rozhraní .NET sestavení zkompilovaného pro 32bitové cílové proto nebude fungovat v Azure.</span><span class="sxs-lookup"><span data-stu-id="17cd0-118">Therefore, .NET assemblies compiled for a 32-bit target won't work on Azure.</span></span>

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a><span data-ttu-id="17cd0-119">Role, vyvolá neošetřené výjimky během inicializace nebo ukončení</span><span class="sxs-lookup"><span data-stu-id="17cd0-119">Role throws unhandled exceptions while initializing or stopping</span></span>
<span data-ttu-id="17cd0-120">Jakékoli výjimky, které jsou vyvolány metody [RoleEntryPoint] třída, která obsahuje [OnStart], [OnStop], a [spustit] metody, jsou neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="17cd0-120">Any exceptions that are thrown by the methods of the [RoleEntryPoint] class, which includes the [OnStart], [OnStop], and [Run] methods, are unhandled exceptions.</span></span> <span data-ttu-id="17cd0-121">Pokud dojde k neošetřené výjimce v jednom z těchto metod, bude funkce role.</span><span class="sxs-lookup"><span data-stu-id="17cd0-121">If an unhandled exception occurs in one of these methods, the role will recycle.</span></span> <span data-ttu-id="17cd0-122">Pokud role je recyklace opakovaně, může vyvolání neošetřenou výjimku při každém pokusu spustit.</span><span class="sxs-lookup"><span data-stu-id="17cd0-122">If the role is recycling repeatedly, it may be throwing an unhandled exception each time it tries to start.</span></span>

## <a name="role-returns-from-run-method"></a><span data-ttu-id="17cd0-123">Vrátí role z metoda Run</span><span class="sxs-lookup"><span data-stu-id="17cd0-123">Role returns from Run method</span></span>
<span data-ttu-id="17cd0-124">[spustit] metoda má běžela bez omezení.</span><span class="sxs-lookup"><span data-stu-id="17cd0-124">The [Run] method is intended to run indefinitely.</span></span> <span data-ttu-id="17cd0-125">Pokud váš kód přepíše [spustit] metody ho měli režimu spánku po neomezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="17cd0-125">If your code overrides the [Run] method, it should sleep indefinitely.</span></span> <span data-ttu-id="17cd0-126">Pokud [spustit] metoda vrací výsledek, recykluje roli.</span><span class="sxs-lookup"><span data-stu-id="17cd0-126">If the [Run] method returns, the role recycles.</span></span>

## <a name="incorrect-diagnosticsconnectionstring-setting"></a><span data-ttu-id="17cd0-127">Nesprávné nastavení DiagnosticsConnectionString</span><span class="sxs-lookup"><span data-stu-id="17cd0-127">Incorrect DiagnosticsConnectionString setting</span></span>
<span data-ttu-id="17cd0-128">Pokud aplikace používá Azure Diagnostics, musíte zadat konfiguračním souboru služby `DiagnosticsConnectionString` nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="17cd0-128">If application uses Azure Diagnostics, your service configuration file must specify the `DiagnosticsConnectionString` configuration setting.</span></span> <span data-ttu-id="17cd0-129">Toto nastavení musí určovat připojení HTTPS k vašemu účtu úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="17cd0-129">This setting should specify an HTTPS connection to your storage account in Azure.</span></span>

<span data-ttu-id="17cd0-130">Abyste ověřili, že vaše `DiagnosticsConnectionString` před nasazením balíčku aplikace do Azure je nastavení správné, ověřte následující:</span><span class="sxs-lookup"><span data-stu-id="17cd0-130">To ensure that your `DiagnosticsConnectionString` setting is correct before you deploy your application package to Azure, verify the following:</span></span>  

* <span data-ttu-id="17cd0-131">`DiagnosticsConnectionString` Nastavení bodů na platný účet úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="17cd0-131">The `DiagnosticsConnectionString` setting points to a valid storage account in Azure.</span></span>  
  <span data-ttu-id="17cd0-132">Ve výchozím nastavení toto nastavení odkazuje na účet emulované úložiště, takže musíte explicitně změnit toto nastavení před nasazením balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="17cd0-132">By default, this setting points to the emulated storage account, so you must explicitly change this setting before you deploy your application package.</span></span> <span data-ttu-id="17cd0-133">Pokud není toto nastavení změnit, je vyvolána výjimka, když se pokusí spustit monitorování diagnostiky role instance.</span><span class="sxs-lookup"><span data-stu-id="17cd0-133">If you do not change this setting, an exception is thrown when the role instance attempts to start the diagnostic monitor.</span></span> <span data-ttu-id="17cd0-134">To může způsobit instance role recyklace po neomezenou dobu.</span><span class="sxs-lookup"><span data-stu-id="17cd0-134">This may cause the role instance to recycle indefinitely.</span></span>
* <span data-ttu-id="17cd0-135">Připojovací řetězec je zadána v následujícím [formátu](../storage/common/storage-configure-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="17cd0-135">The connection string is specified in the following [format](../storage/common/storage-configure-connection-string.md).</span></span> <span data-ttu-id="17cd0-136">(Protokol musí být zadán jako protokol HTTPS.) Nahraďte *MyAccountName* s názvem účtu úložiště a *MyAccountKey* vaším přístupovým klíčem:</span><span class="sxs-lookup"><span data-stu-id="17cd0-136">(The protocol must be specified as HTTPS.) Replace *MyAccountName* with the name of your storage account, and *MyAccountKey* with your access key:</span></span>    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  <span data-ttu-id="17cd0-137">Pokud vyvíjíte aplikace pomocí nástroje Azure pro sadu Microsoft Visual Studio, můžete tuto hodnotu nastavit na stránkách vlastností.</span><span class="sxs-lookup"><span data-stu-id="17cd0-137">If you are developing your application by using Azure Tools for Microsoft Visual Studio, you can use the property pages to set this value.</span></span>

## <a name="exported-certificate-does-not-include-private-key"></a><span data-ttu-id="17cd0-138">Exportovaný certifikát neobsahuje privátní klíč</span><span class="sxs-lookup"><span data-stu-id="17cd0-138">Exported certificate does not include private key</span></span>
<span data-ttu-id="17cd0-139">Pokud chcete spustit webovou roli v rámci zabezpečení SSL, je nutné zajistit, že svůj certifikát pro správu exportovaný obsahuje privátní klíč.</span><span class="sxs-lookup"><span data-stu-id="17cd0-139">To run a web role under SSL, you must ensure that your exported management certificate includes the private key.</span></span> <span data-ttu-id="17cd0-140">Pokud použijete *správce certifikátů systému Windows* a vyexportovat si certifikát, je nutné vybrat **Ano** pro **exportovat soukromý klíč** možnost.</span><span class="sxs-lookup"><span data-stu-id="17cd0-140">If you use the *Windows Certificate Manager* to export the certificate, be sure to select **Yes** for the **Export the private key** option.</span></span> <span data-ttu-id="17cd0-141">Certifikát musí být exportován ve formátu PFX, který je formátem pouze aktuálně podporované.</span><span class="sxs-lookup"><span data-stu-id="17cd0-141">The certificate must be exported in the PFX format, which is the only format currently supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17cd0-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="17cd0-142">Next steps</span></span>
<span data-ttu-id="17cd0-143">Zobrazení [řešení potíží s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pro cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="17cd0-143">View more [troubleshooting articles](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="17cd0-144">Zobrazit další role recyklace scénáře v [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="17cd0-144">View more role recycling scenarios at [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[spustit]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
