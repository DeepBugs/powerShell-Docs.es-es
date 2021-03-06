---
title: "Nuevos escenarios y características de WMF 5.1 (versión preliminar)"
ms.date: 2016-07-13
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: keithb
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: 57049ff138604b0e13c8fd949ae14da05cb03a4b
ms.openlocfilehash: 01d7ac9815a8650f36150e36b4f6942f451dc368

---

# Nuevos escenarios y características de WMF 5.1 (versión preliminar) #

> Nota: Esta información es preliminar y está sujeta a cambios.

## Ediciones de PowerShell ##
A partir de la versión 5.1, PowerShell está disponible en diferentes ediciones que denotan distintos conjuntos de características y compatibilidad con varias plataformas.

- **Desktop Edition:** basado en .NET Framework y proporciona compatibilidad con scripts y módulos destinados a las versiones de PowerShell que se ejecutan en las ediciones de superficie completa de Windows como Server Core y Windows Desktop.
- **Core Edition:** basado en .NET Core y proporciona compatibilidad con scripts y módulos destinados a las versiones de PowerShell que se ejecutan en las ediciones de superficie completa de Windows como Nano Server y Windows IoT.

**Más información acerca de las ediciones de PowerShell**
- [Determine la edición de ejecución de PowerShell]()
- [Declare la compatibilidad de un módulo con versiones específicas de PowerShell]()
- [Filtre los resultados de Get-Module por CompatiblePSEditions]()
- [Impida la ejecución de scripts, a menos que se ejecuten en una edición compatible de PowerShell]()

## Cmdlets del catálogo  

Hemos agregado dos nuevos cmdlets en el módulo [Microsoft.Powershell.Secuity](https://technet.microsoft.com/en-us/library/hh847877.aspx) para generar y validar los archivos de catálogo de Windows.  

###New-FileCatalog 
--------------------------------

Este cmdlet permite crear un archivo de catálogo de Windows para un conjunto de carpetas y archivos. Este archivo de catálogo contiene hashes para todos los archivos de las rutas de acceso especificadas. Los usuarios pueden distribuir el conjunto de carpetas junto con el correspondiente archivo de catálogo que representa a esas carpetas. Esta información es útil para validar si se han realizado cambios en las carpetas desde que se creó el catálogo.    

```PowerShell
New-FileCatalog [-CatalogFilePath] <string> [[-Path] <string[]>] [-CatalogVersion <int>] [-WhatIf] [-Confirm] [<CommonParameters>]
```
Se admiten las versiones 1 y 2 del catálogo. La versión 1, utiliza el algoritmo hash SHA1 para crear los hashes de archivo y la versión 2 utiliza SHA256. La versión 2 del catálogo no es compatible con *Windows Server 2008 R2* o *Windows 7*. Deberá utilizar la versión 2 del catálogo en *Windows 8*, *Windows Server 2012* y sistemas operativos posteriores.  

![](../../images/NewFileCatalog.jpg)

Esto crea el archivo de catálogo. 

![](../../images/CatalogFile1.jpg)  

![](../../images/CatalogFile2.jpg) 

Para comprobar la integridad del archivo de catálogo (Pester.cat en el ejemplo anterior) fírmelo mediante el cmdlet [Set-AuthenticodeSignature](https://technet.microsoft.com/library/hh849819.aspx).   


###Test-FileCatalog 
--------------------------------

Este cmdlet permite validar el catálogo que representa un conjunto de carpetas. 

```PowerShell
Test-FileCatalog [-CatalogFilePath] <string> [[-Path] <string[]>] [-Detailed] [-FilesToSkip <string[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

![](../../images/TestFileCatalog.jpg)

Este cmdlet compara los hashes y rutas de acceso relativas de los archivos que se encuentran en el *catálogo* con los de los que están en el *disco*. Si se detecta cualquier error de coincidencia entre los hashes y las rutas de acceso de los archivos devuelve el estado *ValidationFailed*. Los usuarios pueden recuperar toda esta información mediante la marca *-Detailed*. También muestra el estado de firma del catálogo en el campo *Firma* lo cual es igual que llamar al cmdlet [Get-AuthenticodeSignature](https://technet.microsoft.com/en-us/library/hh849805.aspx) en el archivo de catálogo. El usuario también puede omitir cualquier archivo durante la validación mediante el parámetro *FilesToSkip -*. 


## Caché de análisis de módulo ##
A partir de WMF 5.1, PowerShell proporciona control sobre el archivo que se utiliza para almacenar en la caché los datos acerca de un módulo, como los comandos que exporta.

De manera predeterminada, esta caché se almacena en el archivo `${env:LOCALAPPDATA}\Microsoft\Windows\PowerShell\ModuleAnalysisCache`.
La caché normalmente se lee en el inicio al buscar un comando y se escribe en un subproceso en segundo plano después de que se importa un módulo.

Para cambiar la ubicación predeterminada de la caché, establezca la variable de entorno PSModuleAnalysisCachePath antes de iniciar PowerShell. Los cambios en esta variable de entorno solo afectarán a los procesos secundarios.
El valor debe asignar un nombre a una ruta de acceso completa (nombre de archivo incluido) en la que PowerShell tiene permiso para crear y escribir archivos.
Para deshabilitar la caché del archivo, establezca este valor en una ubicación no válida, como por ejemplo:

```PowerShell
$env:PSModuleAnalysisCachePath = 'nul'
```

Esto establece la ruta de acceso en un dispositivo no válido. Si PowerShell no puede escribir en la ruta de acceso, se devuelve ningún error, pero puede se pueden ver informes del error a través de un seguimiento:

```PowerShell
Trace-Command -PSHost -Name Modules -Expression { Import-Module Microsoft.PowerShell.Management -Force }
```

Al escribir en la caché, PowerShell buscará si hay módulos que no existen, con el fin de evitar que la caché sea demasiado grande.
A veces no se desea que se realicen estas comprobaciones, en cuyo caso se pueden desactivar estableciendo

```PowerShell
$env:PSDisableModuleAnalysisCacheCleanup = 1
```

El establecimiento de esta variable de entorno surtirá efecto de inmediato en el proceso actual.

##Especificación de la versión del módulo

En WMF 5.1, `using module` se comporta de la misma forma que las restantes construcciones relacionadas con módulos de PowerShell. Antes no había forma de especificar una versión concreta del módulo; si había varias versiones, aparecía un error.


En WMF 5.1:

* Puede utilizar la `ModuleSpecification`tabla hash[ ](https://msdn.microsoft.com/en-us/library/jj136290(v=vs.85).aspx). Esta tabla hash tiene el mismo formato que `Get-Module -FullyQualifiedName`.

**Ejemplo:** `using module @{ModuleName = 'PSReadLine'; RequiredVersion = '1.1'}`

* Si hay varias versiones del módulo de multiplicar, PowerShell usa la **misma lógica de resolución** que `Import-Module` y no devuelve ningún error (el mismo comportamiento que `Import-Module` y `Import-DscResource`).








##Mejoras en Pester
En WMF 5.1, se actualizó la versión de Pester que se incluye con PowerShell desde la versión 3.3.5 a 3.4.0, con la adición de confirmación de https://github.com/pester/Pester/pull/484/commits/3854ae8a1f215b39697ac6c2607baf42257b102e, lo que permite un mejor comportamiento para Pester en Nano. 

Puede revisar los cambios en las versiones 3.3.5 a 3.4.0 inspeccionando el archivo ChangeLog.md en: https://github.com/pester/Pester/blob/master/CHANGELOG.md



<!--HONumber=Jul16_HO3-->


