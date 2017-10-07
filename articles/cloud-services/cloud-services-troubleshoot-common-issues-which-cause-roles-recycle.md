---
title: "aaaCommon způsobí, že cloudové služby rolí recyklace | Microsoft Docs"
description: "Cloudové služby role, kterou najednou recykluje může způsobit významné výpadky. Tady jsou některé běžné problémy, které způsobí role toobe recyklované, která vám může pomoci zkrátit dobu prostojů."
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
ms.openlocfilehash: 8fa152b33d2b22a8a02f834d4bc38519b4272f7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="common-issues-that-cause-roles-toorecycle"></a>Běžné problémy, které způsobí toorecycle role
Tento článek popisuje některé běžné příčiny problémů s nasazením hello a poskytuje řešení potíží s tipy toohelp vyřešit tyto problémy. Jako ukazatel toho, zda existuje problém s aplikací je při selhání hello role instance toostart, nebo ji cyklů mezi stavy hello inicializace, zaneprázdněn a ukončení.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Chybějící závislosti modulu runtime
Pokud roli v aplikaci závisí na jakékoli sestavení, které není součástí hello rozhraní .NET Framework nebo hello Azure spravovanou knihovnu, je nutné explicitně zahrnout toto sestavení v balíčku aplikace hello. Uvědomte si, že nejsou dostupné v Azure ve výchozím nastavení ostatní platformy Microsoft. Pokud vaše role závisí na takových framework, je nutné přidat balíček aplikace toohello těchto sestavení.

Před sestavení a balíček aplikace, ověřte následující hello:

* Pokud pomocí sady Visual studio, ujistěte se, zda text hello **místní kopie** vlastnost je nastavena příliš**True** pro každý odkazovaná sestavení v projektu, který není součástí hello Azure SDK nebo hello rozhraní .NET Framework.
* Ujistěte se, že soubor web.config hello neodkazuje na žádné nepoužité sestavení v elementu kompilace hello.
* Hello **akce sestavení** z každé .cshtml soubor je nastaven příliš**obsahu**. To zajistí hello soubory se zobrazí správně v balíčku hello a umožňuje tooappear další soubory, včetně souborů v balíčku hello.

## <a name="assembly-targets-wrong-platform"></a>Sestavení cílů chybné platformě.
Azure je prostředí na 64-bit. Rozhraní .NET sestavení zkompilovaného pro 32bitové cílové proto nebude fungovat v Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Role, vyvolá neošetřené výjimky během inicializace nebo ukončení
Jakékoli výjimky, které jsou vyvolány hello metody hello [RoleEntryPoint] třída, která obsahuje hello [OnStart], [OnStop], a [spustit]metody, jsou neošetřené výjimky. Pokud dojde k neošetřené výjimce v jednom z těchto metod, bude recyklujte hello role. Pokud hello role je recyklace opakovaně, může vyvolání pokaždé, když se pokusí o toostart k neošetřené výjimce.

## <a name="role-returns-from-run-method"></a>Vrátí role z metoda Run
Hello [spustit] metoda je určený toorun po neomezenou dobu. Pokud váš kód přepíše hello [spustit] metody ho měli režimu spánku po neomezenou dobu. Pokud hello [spustit] metoda vrací výsledek, recykluje hello role.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Nesprávné nastavení DiagnosticsConnectionString
Pokud aplikace používá Azure Diagnostics, konfiguračním souboru služby musíte zadat hello `DiagnosticsConnectionString` nastavení konfigurace. Toto nastavení by měl zadat účet úložiště tooyour připojení HTTPS v Azure.

tooensure, vaše `DiagnosticsConnectionString` před nasazením balíčku tooAzure vaše aplikace je nastavení správné, ověřte následující hello:  

* Hello `DiagnosticsConnectionString` nastavení body tooa platný účet úložiště v Azure.  
  Ve výchozím nastavení toto nastavení bodů toohello emulovaných účet úložiště, abyste před nasazením balíčku aplikace musí explicitně změnit toto nastavení. Pokud není toto nastavení změnit, je vyvolána výjimka, když se hello role instance pokusí monitorování diagnostiky toostart hello. To může způsobit hello role instance toorecycle po neomezenou dobu.
* Hello připojovací řetězec je uveden v následující hello [formátu](../storage/common/storage-configure-connection-string.md). (hello protokol musí být zadán jako protokol HTTPS.) Nahraďte *MyAccountName* hello název účtu úložiště a *MyAccountKey* vaším přístupovým klíčem:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Pokud vyvíjíte aplikace pomocí nástroje Azure pro sadu Microsoft Visual Studio, můžete použít hello vlastnost stránky tooset tuto hodnotu.

## <a name="exported-certificate-does-not-include-private-key"></a>Exportovaný certifikát neobsahuje privátní klíč
toorun webové role v rámci zabezpečení SSL, musíte zajistit, že svůj certifikát pro správu exportovaný obsahuje privátní klíč hello. Pokud používáte hello *správce certifikátů systému Windows* tooexport hello certifikát, být jisti tooselect **Ano** pro hello **privátní klíč exportovat hello** možnost. Hello certifikát musí být exportován ve formátu PFX hello, který je aktuálně podporované pouze formát hello.

## <a name="next-steps"></a>Další kroky
Zobrazení [řešení potíží s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pro cloudové služby.

Zobrazit další role recyklace scénáře v [řady blogu kevina Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[spustit]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
