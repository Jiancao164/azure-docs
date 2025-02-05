---
title: How to use drain mode to isolate session hosts - Azure Virtual Desktop
description: How to use drain mode to isolate session hosts to perform maintenance in Azure Virtual Desktop.
services: virtual-desktop
author: Heidilohr

ms.service: virtual-desktop
ms.topic: how-to
ms.date: 02/09/2024
ms.author: helohr 
---

# Use drain mode to isolate session hosts and apply patches

Drain mode isolates a session host when you want to apply patches and do maintenance without disrupting user sessions. When isolated, the session host won't accept new user sessions. Any new connections will be redirected to the next available session host. Existing connections in the session host will keep working until the user signs out or the administrator ends the session. When the session host is in drain mode, admins can also remotely connect to the server without going through the Azure Virtual Desktop service. You can apply this setting to both pooled and personal desktops.

## Prerequisites

If you're using either the Azure portal or PowerShell method, you'll need the following things:

- A host pool with at least one session host.
- An Azure account assigned the [Desktop Virtualization Session Host Operator](rbac.md#desktop-virtualization-session-host-operator) role.
- If you want to use Azure PowerShell locally, see [Use Azure CLI and Azure PowerShell with Azure Virtual Desktop](cli-powershell.md) to make sure you have the [Az.DesktopVirtualization](/powershell/module/az.desktopvirtualization) PowerShell module installed. Alternatively, use the [Azure Cloud Shell](../cloud-shell/overview.md).


## Enable drain mode

Here's how to enable drain mode using the Azure portal and PowerShell.

### [Portal](#tab/portal)

To turn on drain mode in the Azure portal:

1. Sign in to the [Azure portal](https://portal.azure.com).

1. Enter **Azure Virtual Desktop** into the search bar.

1. Under **Services**, select **Azure Virtual Desktop**.

1. At the Azure Virtual Desktop page, go the menu on the left side of the window and select **Host pools**. 

1. Select the host pool you want to isolate.

1. In the navigation menu, select **Session hosts**.

1. Next, select the hosts you want to turn on drain mode for, then select **Turn drain mode on**.

1. To turn off drain mode, select the host pools that have drain mode turned on, then select **Turn drain mode off**.

### [PowerShell](#tab/powershell)

You can set drain mode in PowerShell with the *AllowNewSessions* parameter, which is part of the [Update-AzWvdSessionhost](/powershell/module/az.desktopvirtualization/update-azwvdsessionhost) command.

[!INCLUDE [include-cloud-shell-local-powershell](includes/include-cloud-shell-local-powershell.md)]

2. Run this cmdlet to enable drain mode:

```powershell
$params = @{
   ResourceGroupName = "<resourceGroupName>"
   HostPoolName = "<hostpoolname>"
   Name = "<hostname>"
   AllowNewSession = $False
}

Update-AzWvdSessionHost @params
```

3. Run this cmdlet to disable drain mode:

```powershell
$params = @{
   ResourceGroupName = "<resourceGroupName>"
   HostPoolName = "<hostpoolname>"
   Name = "<hostname>"
   AllowNewSession = $True
}

Update-AzWvdSessionHost @params
```

>[!IMPORTANT]
>You'll need to run this command for every session host you're applying the setting to.

---

## Next steps

If you want to learn more about the Azure portal for Azure Virtual Desktop, check out [our tutorials](create-host-pools-azure-marketplace.md). If you're already familiar with the basics, check out some of the other features you can use with the Azure portal, such as [MSIX app attach](app-attach-azure-portal.md) and [Azure Advisor](../advisor/advisor-overview.md).

If you're using the PowerShell method and want to see what else the module can do, check out [Set up the PowerShell module for Azure Virtual Desktop](powershell-module.md) and our [PowerShell reference](/powershell/module/az.desktopvirtualization/).
