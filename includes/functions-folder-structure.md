
<span data-ttu-id="eadbb-101">Kód pro všechny funkce v aplikaci pro danou funkci žije v kořenové složce, která obsahuje konfigurační soubor hostitele a jeden nebo více podsložek, z nichž každý obsahuje kód pro samostatné funkce, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="eadbb-101">The code for all of the functions in a given function app lives in a root folder that contains a host configuration file and one or more subfolders, each of which contain the code for a separate function, as in the following example:</span></span>

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

<span data-ttu-id="eadbb-102">*Host.json* soubor obsahuje některé konfigurace specifické pro modul runtime a nachází v kořenové složce funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="eadbb-102">The *host.json* file contains some runtime-specific configuration and sits in the root folder of the function app.</span></span> <span data-ttu-id="eadbb-103">Informace o nastaveních, které jsou k dispozici, najdete v části [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) na stránkách wiki WebJobs.Script úložiště.</span><span class="sxs-lookup"><span data-stu-id="eadbb-103">For information on settings that are available, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) in the WebJobs.Script repository wiki.</span></span>

<span data-ttu-id="eadbb-104">Jednotlivé funkce má složku, která obsahuje jeden nebo více souborů, function.json konfiguraci a další závislosti.</span><span class="sxs-lookup"><span data-stu-id="eadbb-104">Each function has a folder that contains one or more code files, the function.json configuration and other dependencies.</span></span>

