---
title: 教程 - 结合使用 Azure 部署槽位和 Terraform 来预配基础结构
description: 有关使用 Terraform 和 Azure 提供程序部署槽位的教程
ms.topic: tutorial
ms.date: 11/07/2019
ms.openlocfilehash: 68c790b4fad442d94e6ac82d1a545b8554d2dd4f
ms.sourcegitcommit: 28688c6ec606ddb7ae97f4d0ac0ec8e0cd622889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2019
ms.locfileid: "74159180"
---
# <a name="tutorial-provision-infrastructure-with-azure-deployment-slots-using-terraform"></a>教程：结合使用 Azure 部署槽位和 Terraform 来预配基础结构

可以使用 [Azure 部署槽位](/azure/app-service/deploy-staging-slots)交换应用的不同版本。 该功能有助于最大程度地降低中断部署造成的影响。 

本文将会通过 GitHub 和 Azure 演练两个应用的部署，以演示部署槽位的示例用法。 一个应用托管在生产槽位中。 另一个应用托管在过渡槽位中。 （名称“生产”和“暂存”是任意名称。 它们可以是适用于你的情形的任何内容。）配置部署槽位后，可根据需要使用 Terraform 在两个槽位之间交换。

## <a name="prerequisites"></a>先决条件

- **Azure 订阅**：如果还没有 Azure 订阅，可以在开始前创建一个 [免费帐户](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。

- **GitHub 帐户**：需要通过一个 [GitHub](https://www.github.com) 帐户来克隆和使用测试 GitHub 存储库。

## <a name="create-and-apply-the-terraform-plan"></a>创建并应用 Terraform 计划

1. 浏览到 [Azure 门户](https://portal.azure.com)。

1. 打开 [Azure Cloud Shell](/azure/cloud-shell/overview)。 如果事先未选择环境，请选择“Bash”作为环境。 

    ![Cloud Shell 提示符](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. 切换到 `clouddrive` 目录。

    ```bash
    cd clouddrive
    ```

1. 创建名为 `deploy` 的目录。

    ```bash
    mkdir deploy
    ```

1. 创建名为 `swap` 的目录。

    ```bash
    mkdir swap
    ```

1. 使用 `ls` bash 命令验证是否已成功创建这两个目录。

    ![创建目录后的 Clould Shell](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. 切换到 `deploy` 目录。

    ```bash
    cd deploy
    ```

1. 在 Cloud Shell 中，创建名为 `deploy.tf` 的文件。

    ```bash
    code deploy.tf
    ```

1. 在编辑器中粘贴以下代码：

    ```hcl
    # Configure the Azure provider
    provider "azurerm" { }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = azurerm_resource_group.slotDemo.location
        resource_group_name = azurerm_resource_group.slotDemo.name
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = azurerm_resource_group.slotDemo.location
        resource_group_name = azurerm_resource_group.slotDemo.name
        app_service_plan_id = azurerm_app_service_plan.slotDemo.id
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = azurerm_resource_group.slotDemo.location
        resource_group_name = azurerm_resource_group.slotDemo.name
        app_service_plan_id = azurerm_app_service_plan.slotDemo.id
        app_service_name    = azurerm_app_service.slotDemo.name
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

1. 创建该文件后，请验证其内容。

    ```bash
    cat deploy.tf
    ```

1. 初始化 Terraform。

    ```bash
    terraform init
    ```

1. 创建 Terraform 计划。

    ```bash
    terraform plan
    ```

1. 预配 `deploy.tf` 配置文件中定义的资源。 （在提示符下输入 `yes` 确认操作。）

    ```bash
    terraform apply
    ```

1. 关闭 Cloud Shell 窗口。

1. 在 Azure 门户的主菜单中，选择“资源组”。 

    ![门户中的“资源组”选项](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. 在“资源组”选项卡上，选择“slotDemoResourceGroup”。  

    ![Terraform 创建的资源组](./media/terraform-slot-walkthru/resource-group.png)

现在会看到 Terraform 创建的所有资源。

![Terraform 创建的资源](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>克隆测试项目

在测试创建以及换入和换出部署槽位之前，需要从 GitHub 克隆测试项目。

1. 浏览到 [GitHub 上的 awesome-terraform 存储库](https://github.com/Azure/awesome-terraform)。

1. 克隆 **awesome-terraform** 存储库。

    ![克隆 GitHub awesome-terraform 存储库](./media/terraform-slot-walkthru/fork-repo.png)

1. 遵照所有提示，将此存储库克隆到环境中。

## <a name="deploy-from-github-to-your-deployment-slots"></a>从 GitHub 部署到部署槽位

克隆测试项目存储库后，通过以下步骤配置部署槽位：

1. 在 Azure 门户的主菜单中，选择“资源组”。 

1. 选择“slotDemoResourceGroup”。 

1. 选择“slotAppService”。 

1. 选择“部署选项”。 

    ![应用服务资源的部署选项](./media/terraform-slot-walkthru/deployment-options.png)

1. 在“部署选项”选项卡上，依次选择“选择源”、“GitHub”。   

    ![选择部署源](./media/terraform-slot-walkthru/select-source.png)

1. 在 Azure 建立连接并显示所有选项后，选择“授权”。 

1. 在“授权”选项卡上选择“授权”，并提供所需的凭据让 Azure 访问你的 GitHub 帐户。   

1. 在 Azure 验证 GitHub 凭据后，将会显示一条消息，指出已完成授权过程。 选择“确定”关闭“授权”选项卡。  

1. 选择“选择组织”并选择自己的组织。 

1. 选择“选择项目”。 

1. 在“选择项目”选项卡上，选择“awesome-terraform”项目。  

    ![选择 awesome-terraform 项目](./media/terraform-slot-walkthru/choose-project.png)

1. 选择“选择分支”。 

1. 在“选择分支”选项卡上，选择“master”。  

    ![选择 master 分支](./media/terraform-slot-walkthru/choose-branch-master.png)

1. 在“部署选项”选项卡上，选择“确定”。  

现在，已部署生产槽位。 若要部署暂存槽位，请执行前面的步骤，并进行以下修改：

- 在步骤 3 中，选择“slotAppServiceSlotOne”资源。 

- 在步骤 13 中，选择 working 分支而不是 master 分支。

    ![选择 working 分支](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>测试应用部署

在前面的部分中，我们设置了要从 GitHub 中不同分支部署的两个槽位 - **slotAppService** 和 **slotAppServiceSlotOne**。 让我们预览 Web 应用，以验证是否已成功部署这些应用。

1. 在 Azure 门户的主菜单中，选择“资源组”。 

1. 选择“slotDemoResourceGroup”。 

1. 选择“slotAppService”或“slotAppServiceSlotOne”。  

1. 在概述页上，选择“URL”。 

    ![在概述选项卡上选择“URL”以呈现应用](./media/terraform-slot-walkthru/resource-url.png)

1. 根据所选的应用，会看到以下结果：
    - **slotAppService** Web 应用 - 蓝色页面，其标题为“槽位演示应用 1”。  
    - **slotAppServiceSlotOne** Web 应用 - 绿色页面，其标题为“槽位演示应用 2”。 

    ![预览应用以测试是否已正确部署这些应用](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>交换两个部署槽位

若要测试两个部署槽位的交换，请执行以下步骤：
 
1. 切换到运行 **slotAppService**（具有蓝色页面的应用）的浏览器选项卡。 

1. 在单独的选项卡中返回到 Azure 门户。

1. 打开 Cloud Shell。

1. 切换到 **clouddrive/swap** 目录。

    ```bash
    cd clouddrive/swap
    ```

1. 在 Cloud Shell 中，创建名为 `swap.tf` 的文件。

    ```bash
    code swap.tf
    ```

1. 在编辑器中粘贴以下代码：

    ```hcl
    # Configure the Azure provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

1. 初始化 Terraform。

    ```bash
    terraform init
    ```

1. 创建 Terraform 计划。

    ```bash
    terraform plan
    ```

1. 预配 `swap.tf` 配置文件中定义的资源。 （在提示符下输入 `yes` 确认操作。）

    ```bash
    terraform apply
    ```

1. 在 Terraform 交换槽后，返回到浏览器。 刷新页面。 

**slotAppServiceSlotOne** 过渡槽位中的 Web 应用现已交换到生产槽位，并以绿色呈现。 

![已交换部署槽位](./media/terraform-slot-walkthru/slots-swapped.png)

若要返回到应用的原始生产版本，请重新应用从 `swap.tf` 配置文件创建的 Terraform 计划。

```bash
terraform apply
```

交换应用后，会看到原始配置。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [详细了解如何在 Azure 中使用 Terraform](/azure/terraform)