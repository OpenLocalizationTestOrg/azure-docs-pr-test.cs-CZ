---
title: "aaaDownload Media Services prostředky tooyour počítači - Azure | Microsoft Docs"
description: "Další informace o počítači tooyour toodownload prostředky. Ukázky kódu jsou napsané v jazyce C# a použít hello sady Media Services SDK pro .NET."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 8908a1dd-3ffb-4f18-955d-4c8e2d82fc5d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 6c6e764720caa59d8371178a2682700345f7bc57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-deliver-an-asset-by-download"></a><span data-ttu-id="c3e69-104">Postupy: poskytování prostředek službou</span><span class="sxs-lookup"><span data-stu-id="c3e69-104">How to: Deliver an Asset by Download</span></span>
<span data-ttu-id="c3e69-105">Toto téma popisuje možnosti pro doručování média prostředky tooMedia služby nahrán.</span><span class="sxs-lookup"><span data-stu-id="c3e69-105">This topic discusses options for delivering media assets uploaded tooMedia Services.</span></span> <span data-ttu-id="c3e69-106">V mnoha případech aplikace můžete doručování obsahu Media Services.</span><span class="sxs-lookup"><span data-stu-id="c3e69-106">You can deliver Media Services content in numerous application scenarios.</span></span> <span data-ttu-id="c3e69-107">Můžete stáhnout média prostředky, nebo k nim přistupovat pomocí lokátoru.</span><span class="sxs-lookup"><span data-stu-id="c3e69-107">You can download media assets, or access them by using a locator.</span></span> <span data-ttu-id="c3e69-108">Můžete odeslat média obsahu tooanother aplikace nebo tooanother poskytovateli obsahu.</span><span class="sxs-lookup"><span data-stu-id="c3e69-108">You can send media content tooanother application or tooanother content provider.</span></span> <span data-ttu-id="c3e69-109">Pro lepší výkon a škálovatelnost můžete také doručovat obsah pomocí sítě doručování obsahu (CDN).</span><span class="sxs-lookup"><span data-stu-id="c3e69-109">For improved performance and scalability, you can also deliver content by using a Content Delivery Network (CDN).</span></span>

<span data-ttu-id="c3e69-110">Tento příklad ukazuje, jak toodownload datových zdrojů médií z Media Services tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="c3e69-110">This example shows how toodownload media assets from Media Services tooyour local computer.</span></span> <span data-ttu-id="c3e69-111">Hello kód dotazy hello úlohy přidružené k účtu Media Services hello ID úlohy a přístupů jeho **OutputMediaAssets** kolekce (což je sada hello jeden nebo více prostředků výstup média, která je výsledkem spuštění úlohy).</span><span class="sxs-lookup"><span data-stu-id="c3e69-111">hello code queries hello jobs associated with hello Media Services account by job ID and accesses its **OutputMediaAssets** collection (which is hello set of one or more output media assets that results from running a job).</span></span> <span data-ttu-id="c3e69-112">Tento příklad ukazuje, jak toodownload výstup médium, které můžete použít prostředky z úlohy, ale hello stejný přístup toodownload jiné prostředky.</span><span class="sxs-lookup"><span data-stu-id="c3e69-112">This  example shows how toodownload output media assets from a job, but you can apply hello same approach toodownload other assets.</span></span>

>[!NOTE]
><span data-ttu-id="c3e69-113">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="c3e69-113">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="c3e69-114">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="c3e69-114">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="c3e69-115">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="c3e69-115">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

    // Download hello output asset of hello specified job tooa local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how toodownload a single asset. 
        // However, you can iterate through hello OutputAssets
        // collection, and download all assets if there are many. 

        // Get a reference toohello job. 
        IJob job = GetJob(jobId);

        // Get a reference toohello first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];

        // Create a SAS locator toodownload hello asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);

        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };

        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use hello following event handler toocheck download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;

            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);

            Console.WriteLine("File download path:  " + localDownloadPath);

            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));

            outputFile.DownloadProgressChanged -= DownloadProgress;
        }

        Task.WaitAll(downloadTasks.ToArray());

        return outputAsset;
    }

    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



## <a name="media-services-learning-paths"></a><span data-ttu-id="c3e69-116">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="c3e69-116">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c3e69-117">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="c3e69-117">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="c3e69-118">Viz také</span><span class="sxs-lookup"><span data-stu-id="c3e69-118">See Also</span></span>
[<span data-ttu-id="c3e69-119">Doručování streamování obsahu</span><span class="sxs-lookup"><span data-stu-id="c3e69-119">Deliver streaming content</span></span>](media-services-deliver-streaming-content.md)

