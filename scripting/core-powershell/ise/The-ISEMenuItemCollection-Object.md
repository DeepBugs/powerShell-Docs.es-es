---
title: El objeto ISEMenuItemCollection
ms.date: 2016-05-11
keywords: powershell,cmdlet
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0c0f5484-3320-408e-8534-5bd1c8e48512
translationtype: Human Translation
ms.sourcegitcommit: 3222a0ba54e87b214c5ebf64e587f920d531956a
ms.openlocfilehash: 563bfc58e545a9e67eb9dd89d8d28e1aa2a33f1c

---

# El objeto ISEMenuItemCollection
  Un objeto **ISEMenuItemCollection** es una colección de objetos **ISEMenuItem**. Es una instancia de la clase Microsoft.PowerShell.Host.ISE.ISEMenuItemCollection. Un ejemplo es el objeto **$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus** que se utiliza para personalizar el menú **Complemento** en el Entorno de scripting integrado (ISE) de Windows PowerShell®.

## Método

### Add\(string DisplayName, System.Management.Automation.ScriptBlock Action, System.Windows.Input.KeyGesture Shortcut \)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Agrega un elemento de menú a la colección.

 **DisplayName**
 El nombre para mostrar del menú que se va a agregar.

 **Action**
 El objeto **System.Management.Automation.ScriptBlock** que especifica la acción que está asociada con este elemento de menú.

 **Shortcut**
 El método abreviado de teclado para la acción.

 **Returns**
 El objeto ISEMenuItem que acaba de agregar.

```
# Create an Add-ons menu with an fast access key and a shortcut.
# Note the use of "_"  as opposed to the "&" for mapping to the fast access key letter for the menu item.
$menuAdded = $psISE.CurrentPowerShellTab.AddOnsMenu.SubMenus.Add("_Process",{get-process},"Alt+P")
```

### Borrar\(\)
  Se admite en Windows PowerShell ISE 2.0 y versiones posteriores. 

 Quita todos los submenús del elemento de menú.

```
# Remove all custom submenu items from the AddOns menu
$psISE.CurrentPowerShellTab.AddOnsMenu.Submenus.Clear()

```

## Véase también
 [El objeto ISEMenuItem](The-ISEMenuItem-Object.md) 
 [El modelo de objetos de scripting de ISE de Windows PowerShell](The-Windows-PowerShell-ISE-Scripting-Object-Model.md) 
 [Referencia del modelo de objetos de ISE de Windows PowerShell](Windows-PowerShell-ISE-Object-Model-Reference.md) 
 [La jerarquía del modelo de objetos de ISE](The-ISE-Object-Model-Hierarchy.md)

  



<!--HONumber=Aug16_HO4-->


