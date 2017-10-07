---
title: "aaaEnable nebo zakázání monitorování virtuálních počítačů Azure"
description: "Popisuje, jak tooEnable nebo zakázat monitorování virtuálních počítačů Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 39c2211e4e5f3ad364513b52c5acc70c00b432fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a><span data-ttu-id="d9403-103">Povolte nebo zakažte monitorování virtuálních počítačů Azure</span><span class="sxs-lookup"><span data-stu-id="d9403-103">Enable or Disable Azure VM Monitoring</span></span>

<span data-ttu-id="d9403-104">Tato část popisuje, jak se tooenable nebo zakažte monitorování na virtuální počítače, spuštěné v Azure.</span><span class="sxs-lookup"><span data-stu-id="d9403-104">This section describes how tooenable or disable monitoring on Virtual machines running on Azure.</span></span> <span data-ttu-id="d9403-105">Můžete povolit nebo zakázat monitorování pomocí portálu hello nebo rozhraní příkazového řádku Azure pro Mac, Linux a Windows (hello příkazového řádku Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="d9403-105">You can enable or disable monitoring using hello portal or Azure Command-line Interface for Mac, Linux, and Windows (hello Azure CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d9403-106">Tento dokument popisuje 2.3 verzi hello rozšíření diagnostiky Linux, která se už nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="d9403-106">This document describes version 2.3 of hello Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="d9403-107">Verze 2.3 bude až do 30. června 2018 podporována.</span><span class="sxs-lookup"><span data-stu-id="d9403-107">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="d9403-108">Místo toho lze povolit verze 3.0 hello rozšíření diagnostiky Linux.</span><span class="sxs-lookup"><span data-stu-id="d9403-108">Version 3.0 of hello Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="d9403-109">Další informace najdete v tématu [hello dokumentaci](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="d9403-109">For more information, see [hello documentation](./diagnostic-extension.md).</span></span>

## <a name="enable--disable-monitoring-through-hello-azure-portal"></a><span data-ttu-id="d9403-110">Povolit / zakázat monitorování hello na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d9403-110">Enable / Disable Monitoring through hello Azure portal</span></span>

<span data-ttu-id="d9403-111">Můžete povolit monitorování vašeho virtuálního počítače Azure, který poskytuje data o vaší instance v období 1 minutu.</span><span class="sxs-lookup"><span data-stu-id="d9403-111">You can enable  monitoring of your Azure VM, which provides data about your instance in 1-minute periods.</span></span> <span data-ttu-id="d9403-112">(použít změny úložiště).</span><span class="sxs-lookup"><span data-stu-id="d9403-112">(storage changes apply).</span></span> <span data-ttu-id="d9403-113">Podrobné diagnostická data bude k dispozici pro hello virtuálních počítačů v portálu grafy hello nebo prostřednictvím hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d9403-113">Detailed diagnostics data is then available for hello VM in hello portal graphs or through hello API.</span></span> <span data-ttu-id="d9403-114">Ve výchozím nastavení portál Azure umožňuje monitorování na základě hostitele omezenou sadu metriky.</span><span class="sxs-lookup"><span data-stu-id="d9403-114">By default, Azure portal enables host-based monitoring of a limited set of metrics.</span></span> <span data-ttu-id="d9403-115">Můžete povolit sledování metrik z ve virtuálním počítači při hello, který je spuštěný virtuální počítač nebo v zastaveném stavu.</span><span class="sxs-lookup"><span data-stu-id="d9403-115">You can enable monitoring of metrics from within a VM while hello VM is running or in stopped state.</span></span>

* <span data-ttu-id="d9403-116">Otevřete hello Azure portálu na [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d9403-116">Open hello Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
* <span data-ttu-id="d9403-117">V levé navigační hello klikněte na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d9403-117">In hello left navigation, click Virtual machines.</span></span>
* <span data-ttu-id="d9403-118">Ve virtuálních počítačích hello seznamu vyberte instanci spuštěná nebo zastavená.</span><span class="sxs-lookup"><span data-stu-id="d9403-118">In hello list Virtual machines, select a running or stopped instance.</span></span> <span data-ttu-id="d9403-119">Otevře se okno Hello "Virtuální počítač".</span><span class="sxs-lookup"><span data-stu-id="d9403-119">hello "Virtual machine" blade opens.</span></span>
* <span data-ttu-id="d9403-120">Klikněte na tlačítko všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="d9403-120">Click All settings.</span></span>
* <span data-ttu-id="d9403-121">Klikněte na tlačítko diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="d9403-121">Click Diagnostics.</span></span>
* <span data-ttu-id="d9403-122">Změnit stav tooOn nebo vypnutí.</span><span class="sxs-lookup"><span data-stu-id="d9403-122">Change status tooOn or Off.</span></span> <span data-ttu-id="d9403-123">Můžete také vybrat v této úrovni hello okno monitorování podrobnosti, které byste chtěli tooenable pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d9403-123">You can also pick in this blade hello level of monitoring details you would like tooenable for your virtual machine.</span></span>

![Povolit nebo zakázat monitorování prostřednictvím hello portálu Azure.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a><span data-ttu-id="d9403-125">Povolit / zakázat monitorování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d9403-125">Enable / Disable Monitoring with Azure CLI</span></span>

<span data-ttu-id="d9403-126">tooenable monitorování pro virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="d9403-126">tooenable monitoring for an Azure VM.</span></span>

* <span data-ttu-id="d9403-127">Vytvořte soubor (s názvem například PrivateConfig.json):</span><span class="sxs-lookup"><span data-stu-id="d9403-127">Create a file (named such as PrivateConfig.json):</span></span>

```json
{
        "storageAccountName":"hello storage account tooreceive data",
        "storageAccountKey":"hello key of hello account"
}
```

* <span data-ttu-id="d9403-128">Povolte rozšíření hello prostřednictvím rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="d9403-128">Enable hello extension via Azure CLI.</span></span>

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

<span data-ttu-id="d9403-129">Další informace najdete v tématu [výkonu pomocí rozšíření diagnostiky Linux tooMonitor Linux Virtuálního počítače a diagnostických dat](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d9403-129">For more information, see [Using Linux Diagnostic Extension tooMonitor Linux VM’s performance and diagnostic data](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
