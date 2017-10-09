---
title: "aaaInstall hello rozhraní příkazového řádku DC/OS | Microsoft Docs"
description: "Nainstalujte hello rozhraní příkazového řádku DC/OS."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontejnery, mikroslužby, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: b077c05beff9a5638486ea5efe9df31089e32701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
> [!NOTE]
> To slouží k práci s clustery ACS založenými na DC/OS. Neexistuje žádné toodo nutné to pro clustery ACS založené na metodě Swarm.
> 
> 

První, [připojení clusteru tooyour ACS založenými na DC/OS](../articles/container-service/container-service-connect.md). Jakmile to uděláte, můžete nainstalovat hello rozhraní příkazového řádku DC/OS na klientský počítač s příkazy hello níže:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Pokud používáte starší verzi Pythonu, můžou se zobrazovat upozornění „InsecurePlatformWarning“. Ta můžete bezpečně ignorovat.

V pořadí tooget byla spuštěna bez restartovat své prostředí spusťte příkaz:

```bash
source ~/.bashrc
```

Pokud spustíte nové prostředí, nebude tento krok nutný.

Teď si můžete ověřit, že hello je nainstalované rozhraní příkazového řádku:

```bash
dcos --help
```