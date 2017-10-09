
Hello kód pro všechny funkce hello v aplikaci pro danou funkci žije v kořenové složce, která obsahuje konfigurační soubor hostitele a jeden nebo více podsložek, z nichž každá obsahovat hello kód pro samostatné funkce, jako hello následující ukázka:

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

Hello *host.json* soubor obsahuje některé konfigurace specifické pro modul runtime a nachází v kořenové složce hello hello funkce aplikace. Informace o nastaveních, které jsou k dispozici, najdete v části [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) hello WebJobs.Script úložiště Wiki.

Jednotlivé funkce má složku, která obsahuje jeden nebo více souborů, hello function.json konfiguraci a další závislosti.

