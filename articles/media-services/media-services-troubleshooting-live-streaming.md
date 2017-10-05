---
title: "Příručka pro řešení potíží pro živé streamování | Microsoft Docs"
description: "Toto téma nabízí návrhy na řešení problémů živé streamování."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: fa91baf7c494941fccf0e6ca38b930f3c2a521ce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a><span data-ttu-id="4d4d6-103">Průvodce řešením potíží s živým streamováním</span><span class="sxs-lookup"><span data-stu-id="4d4d6-103">Troubleshooting guide for live streaming</span></span>
<span data-ttu-id="4d4d6-104">Toto téma nabízí návrhy na řešení problémů některé živé streamování.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-104">This topic gives suggestions on how to troubleshoot some live streaming problems.</span></span>

## <a name="issues-related-to-on-premises-encoders"></a><span data-ttu-id="4d4d6-105">Problémy související s místními kodéry</span><span class="sxs-lookup"><span data-stu-id="4d4d6-105">Issues related to on-premises encoders</span></span>
<span data-ttu-id="4d4d6-106">Tato část poskytuje návrhy na řešení potíží s problémy související s místními kodéry, které jsou nakonfigurované k odeslání datový proud s jednou přenosovou rychlostí do AMS kanály, které jsou povolené kódování v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-106">This section gives suggestions on how to troubleshoot problems related to on-premises encoders that are configured to send a single bitrate stream to AMS channels that are enabled for live encoding.</span></span>

### <a name="problem-would-like-to-see-logs"></a><span data-ttu-id="4d4d6-107">Problém: Rádi viděli protokoly</span><span class="sxs-lookup"><span data-stu-id="4d4d6-107">Problem: Would like to see logs</span></span>
* <span data-ttu-id="4d4d6-108">**Potenciální problém**: Nelze najít protokoly kodér, který vám může pomoci při ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-108">**Potential issue**: Can't find encoder logs that might help in debugging issues.</span></span>
  
  * <span data-ttu-id="4d4d6-109">**Telestream Wirecast**: obvykle najdete v části C:\Users protokoly\{uživatelské jméno} \AppData\Roaming\Wirecast\\</span><span class="sxs-lookup"><span data-stu-id="4d4d6-109">**Telestream Wirecast**: You can usually find logs under C:\Users\{username}\AppData\Roaming\Wirecast\\</span></span> 
  * <span data-ttu-id="4d4d6-110">**Elementární živé**: můžete najít na portálu pro správu obsahuje odkazy na protokoly.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-110">**Elemental Live**: You can find has links to logs on the management portal.</span></span> <span data-ttu-id="4d4d6-111">Klikněte na **statistiky**, pak **protokoly**.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-111">Click on **Stats**, then **Logs**.</span></span> <span data-ttu-id="4d4d6-112">Na **soubory protokolu** stránky, uvidíte seznam protokolů pro všechny položky LiveEvent, vyberte příslušnou možnost, odpovídající aktuální relace.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-112">On the **Log Files** page, you will see a list of logs for all the LiveEvent items; select the one matching your current session.</span></span> 
  * <span data-ttu-id="4d4d6-113">**Flash Live kodér médií**: můžete najít **adresář protokolu...**  přechodem na **kódování protokolu** kartě.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-113">**Flash Media Live Encoder**: You can find the **Log Directory...** by navigating to the **Encoding Log** tab.</span></span>

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a><span data-ttu-id="4d4d6-114">Problém: Není žádná možnost pro výstup vytvořeného progresivní datového proudu</span><span class="sxs-lookup"><span data-stu-id="4d4d6-114">Problem: There is no option for outputting a progressive stream</span></span>
* <span data-ttu-id="4d4d6-115">**Potenciální problém**: používá kodér nepodporuje automaticky zrušit prokládání.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-115">**Potential issue**: The encoder being used doesn't automatically deinterlace.</span></span> 
  
    <span data-ttu-id="4d4d6-116">**Řešení potíží**: Vyhledejte odstraňování prokládání možnost v rámci rozhraní kodér.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-116">**Troubleshooting steps**: Look for a de-interlacing option within the encoder interface.</span></span> <span data-ttu-id="4d4d6-117">Po povolení algoritmy pro odstranění prokládání znovu zkontrolujte nastavení progresivní výstup.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-117">Once de-interlacing is enabled, check again for progressive output settings.</span></span> 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a><span data-ttu-id="4d4d6-118">Problém: Pokusili několik nastavení výstupní kodér a stále se nedaří připojit.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-118">Problem: Tried several encoder output settings and still unable to connect.</span></span>
* <span data-ttu-id="4d4d6-119">**Potenciální problém**: Azure kódování kanál nebyl správně resetován.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-119">**Potential issue**: Azure encoding channel was not properly reset.</span></span> 
  
    <span data-ttu-id="4d4d6-120">**Řešení potíží**: Ujistěte se, že kodér je už nabízení do AMS, zastavte a znovu nastavit kanál.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-120">**Troubleshooting steps**: Make sure the encoder is no longer pushing to AMS, stop and reset the channel.</span></span> <span data-ttu-id="4d4d6-121">Jednou spustit znovu, zkuste se připojit kodér s novým nastavením.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-121">Once running again, try connecting your encoder with the new settings.</span></span> <span data-ttu-id="4d4d6-122">Pokud tento problém pořád nevyřeší, zkuste vytvořit nový kanál zcela, někdy kanály se může stát poškozená po několika nezdařených pokusů o zadání.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-122">If this still does not correct the issue, try creating a new channel entirely, sometimes channels can become corrupt after several failed attempts.</span></span>  
* <span data-ttu-id="4d4d6-123">**Potenciální problém**: nejsou optimální velikost The GOP nebo nastavení klíčových snímků.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-123">**Potential issue**: The GOP size or key frame settings are not optimal.</span></span> 
  
    <span data-ttu-id="4d4d6-124">**Řešení potíží**: GOP doporučená velikost nebo @keyframe, které určuje interval je 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-124">**Troubleshooting steps**: Recommended GOP size or keyframe interval is 2 seconds.</span></span> <span data-ttu-id="4d4d6-125">Některé kodéry vypočítat toto nastavení v počet snímků, jiné zase sekund.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-125">Some encoders calculate this setting in number of frames, while others use seconds.</span></span> <span data-ttu-id="4d4d6-126">Příklad: při výstupu 30fps, velikost GOP by být 60 rámce, který je ekvivalentní na 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-126">For example: When outputting 30fps, the GOP size would be 60 frames, which is equivalent to 2 seconds.</span></span>  
* <span data-ttu-id="4d4d6-127">**Potenciální problém**: uzavřené porty blokují datového proudu.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-127">**Potential issue**: Closed ports are blocking the stream.</span></span> 
  
    <span data-ttu-id="4d4d6-128">**Řešení potíží**: při streamování prostřednictvím RTMP, zkontrolujte nastavení brány firewall nebo proxy, potvrďte, že jsou otevřené odchozí porty 1935 a 1936.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-128">**Troubleshooting steps**: When streaming via RTMP, check firewall and/or proxy settings to confirm that outbound ports 1935 and 1936 are open.</span></span> <span data-ttu-id="4d4d6-129">Při použití RTP streamování, potvrďte, že odchozí port 2010 je otevřen.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-129">When using RTP streaming, confirm that outbound port 2010 is open.</span></span> 

### <a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a><span data-ttu-id="4d4d6-130">Problém: Při konfiguraci kodér do datového proudu s protokol RTP, neexistuje žádný místo pro zadání názvu hostitele.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-130">Problem: When configuring the encoder to stream with the RTP protocol, there is no place to enter a host name.</span></span>
* <span data-ttu-id="4d4d6-131">**Potenciální problém**: mnoho RTP kodéry zakázat u názvů hostitele, a bude nutné získat IP adresu.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-131">**Potential issue**: Many RTP encoders do not allow for host names, and an IP address will need to be acquired.</span></span>  
  
    <span data-ttu-id="4d4d6-132">**Řešení potíží**: Chcete-li najít IP adresu, otevřete příkazový řádek na libovolném počítači.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-132">**Troubleshooting steps**: To find the IP address, open a command prompt on any computer.</span></span> <span data-ttu-id="4d4d6-133">Chcete-li to provést v systému Windows, otevřete Spouštěče spustit (WIN + R) a zadejte "cmd" otevřete.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-133">To do this in Windows, open the Run launcher (WIN + R) and type “cmd” to open.</span></span>  
  
    <span data-ttu-id="4d4d6-134">Jakmile příkazovém řádku je otevřený, zadejte "Ping [AMS název hostitele]".</span><span class="sxs-lookup"><span data-stu-id="4d4d6-134">Once the command prompt is open, type "Ping [AMS Host Name]".</span></span> 
  
    <span data-ttu-id="4d4d6-135">Název hostitele může být odvozen vynecháním číslo portu z Azure Ingestované adresy URL, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="4d4d6-135">The host name can be derived by omitting the port number from the Azure Ingest URL, as highlighted in the following example:</span></span> 
  
    <span data-ttu-id="4d4d6-136">RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 /</span><span class="sxs-lookup"><span data-stu-id="4d4d6-136">rtp://test2-amstest009.rtp.channel.mediaservices.windows.net:2010/</span></span> 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> <span data-ttu-id="4d4d6-138">Pokud po provedení kroků řešení potíží, které stále nemůžete použít datový proud úspěšně, odešlete lístek podpory pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4d4d6-138">If after following the troubleshooting steps you still cannot successfully stream, submit a support ticket using the Azure portal.</span></span>
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="4d4d6-139">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="4d4d6-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4d4d6-140">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="4d4d6-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

