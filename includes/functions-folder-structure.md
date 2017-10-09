
<span data-ttu-id="687cf-101">Hello kód pro všechny funkce hello v aplikaci pro danou funkci žije v kořenové složce, která obsahuje konfigurační soubor hostitele a jeden nebo více podsložek, z nichž každá obsahovat hello kód pro samostatné funkce, jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="687cf-101">hello code for all of hello functions in a given function app lives in a root folder that contains a host configuration file and one or more subfolders, each of which contain hello code for a separate function, as in hello following example:</span></span>

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

<span data-ttu-id="687cf-102">Hello *host.json* soubor obsahuje některé konfigurace specifické pro modul runtime a nachází v kořenové složce hello hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="687cf-102">hello *host.json* file contains some runtime-specific configuration and sits in hello root folder of hello function app.</span></span> <span data-ttu-id="687cf-103">Informace o nastaveních, které jsou k dispozici, najdete v části [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) hello WebJobs.Script úložiště Wiki.</span><span class="sxs-lookup"><span data-stu-id="687cf-103">For information on settings that are available, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in hello WebJobs.Script repository wiki.</span></span>

<span data-ttu-id="687cf-104">Jednotlivé funkce má složku, která obsahuje jeden nebo více souborů, hello function.json konfiguraci a další závislosti.</span><span class="sxs-lookup"><span data-stu-id="687cf-104">Each function has a folder that contains one or more code files, hello function.json configuration and other dependencies.</span></span>

