---
title: ISESnippetObject
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 98bc8113-c3cd-4201-bdb9-9d9bdb7e266c
translationtype: Human Translation
ms.sourcegitcommit: 3222a0ba54e87b214c5ebf64e587f920d531956a
ms.openlocfilehash: 4f244b21454c2db929688d11cdf3f46d474ce0db

---

# ISESnippetObject
  Un objeto **ISESnippet** es una instancia de la clase Microsoft.PowerShell.Host.ISE.ISESnippet. Los miembros de la colección **$psISE.CurrentPowerShellTab.Snippets** son todos ejemplos de objetos **ISESnippet**. La manera más fácil de crear un fragmento de código es usar el cmdlet [New-IseSnippet&#91;PSITPro5_ISE&#93;](https://technet.microsoft.com/en-us/library/0a6339a3-2683-4a8e-8929-90ad9a95c3e0).

## Propiedades

###  <a name="DisplayName"></a> Autor
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 La propiedad de solo lectura que obtiene el nombre del autor del fragmento de código.

```
# Get the author of the first snippet item
$psISE.CurrentPowerShellTab.Snippets.Item(0).Author

```

###  <a name="Action"></a> CodeFragment
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 La propiedad de solo lectura que obtiene el fragmento de código que va a insertar en el editor.

```
# Get the code fragment associated with the first snippet item.
$psISE.CurrentPowerShellTab.Snippets.Item(0).CodeFragment

```

###  <a name="Shortcut"></a> Directa
  Se admite en Windows PowerShell ISE 3.0 y versiones posteriores y no está presente en las versiones anteriores. 

 La propiedad de solo lectura que obtiene el método abreviado de teclado de Windows para el elemento de menú.

```
# Get the shortcut for the first submenu item.
$psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Clear()
$psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("_Process",{get-process},"Alt+P")
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus[0].Shortcut
```

## Véase también
 [El objeto ISESnippetCollection](The-ISESnippetCollection-Object.md) 
 [El modelo de objetos de scripting de ISE de Windows PowerShell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referencia del modelo de objetos de ISE de Windows PowerShell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [La jerarquía del modelo de objetos de ISE](The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Aug16_HO4-->


