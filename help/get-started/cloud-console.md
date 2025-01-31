---
title: 登入 [!DNL Cloud Console]
description: 瞭解雲端基礎結構上適用於Adobe Commerce的 [!DNL Cloud Console] 。
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# 登入[!DNL Cloud Console]

[!DNL Cloud Console]提供互動式方法來建置、管理和部署Commerce程式碼。 [!DNL Cloud Console]是更現代、更方便使用的體驗，並為未來的介面增強功能奠定了基礎。

[登入 [!DNL Cloud Console]](https://console.adobecommerce.com)以檢視您的專案清單。

![專案清單](../assets/ui-allprojects-list.png)

## 功能

新增或改善的功能包括：

- 清楚概述專案和環境特性
- 具有可排序歷史記錄的活動資料流
- 入門專案的手動備份管理和記錄
- 增強的記錄檢視
- 可排序清單
- 新增整合的簡單表單和指南
- 網頁內容可及性指引(WCAG)法規遵循

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

新增或改良的功能如下：

| 功能 | 功能改善 |
| -------------- | ----------------------------------- |
| [活動資料流](../cloud-guide/project/activity-stream.md) | 與執行、擱置或歷史動作的可排序清單互動。 選取活動並檢視記錄，或取消執行中的組建。 |
| [專案和環境總覽](../cloud-guide/project/overview.md#project-overview) | 開啟您的專案，並檢視專案詳細資訊和環境清單的概觀。 環境概觀提供有關環境狀態、應用程式存取和最近活動的更多詳細資訊。 |
| [整合表單](../cloud-guide/integrations/overview.md) | 使用簡單的表單和指引來新增整合，例如位元貯體或Slack通知。 |
| [專案清單](../cloud-guide/project/overview.md#cloud-console) | _所有專案_&#x200B;檢視會列出您有權存取的所有專案。 您可以按一下「**[!UICONTROL Show filters]**」並按型別、地區或計畫篩選專案清單。 |
| [變數可見性選項](../cloud-guide/environment/variable-levels.md) | 在建置或執行階段限制專案層級或環境層級變數的可見度。 |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## 主控台問題

**_我可以在哪裡找到快照功能_**？

對於[!DNL Starter]個專案，快照功能現在稱為&#x200B;_備份_。 您可以從[!DNL Cloud Console]建立[!DNL Starter]環境的手動備份，或從Cloud CLI建立快照。 您必須擁有環境的管理員角色。

從專案導覽列選取環境。 環境必須為作用中。 選取&#x200B;**[!UICONTROL Backups]**&#x200B;標籤。 目前，此選項不適用於Pro環境。

**_環境_**&#x200B;的已設定路由清單在哪裡？

您可以在環境的&#x200B;_服務_&#x200B;標籤上找到已設定的路由清單。

從專案導覽列選取環境。 選取&#x200B;**[!UICONTROL Services]**&#x200B;標籤。 **路由器**&#x200B;概述會顯示已設定的路由。 目前，您無法從新[!DNL Cloud Console]新增路由。

## 帳戶功能表

右上角是您的帳戶功能表。 按一下功能表的向下箭頭，然後選取&#x200B;**[!UICONTROL My Profile]**。 在&#x200B;_我的設定檔_&#x200B;檢視中，您可以控制使用者詳細資訊和顯示設定、管理[安全性驗證](../cloud-guide/project/user-access.md#user-authentication-requirements)、[API權杖](../cloud-guide/project/user-access.md#create-an-api-token)和[SSH金鑰](../cloud-guide/development/secure-connections.md)。
