---
source-git-commit: 305df8acb8c41369b34e2c20d9c7df6824222931
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---
# 投稿

感謝您選擇投稿！

為本專案貢獻內容時應遵循的准則如下。

## 行為準則

此專案遵守 Adobe [行為準則](code-of-conduct.md)。透過參與，
您應遵守此准則。 如發現不良行為，請向
[Grp-opensourceoffice@adobe.com](mailto:Grp-opensourceoffice@adobe.com)。

## 投稿人指南檔案

請參閱[貢獻者指南](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html)。

## 有疑問嗎？

請先提出問題。 此專案的現有提交者需要達到
在專案方向和問題討論串中的問題解決方案上達成共識
（適當時）。

## 貢獻者授權合約

本專案的所有協力廠商投稿人必須隨附已簽署的投稿人
授權合約。 這可授予Adobe重新使用您稿件的許可權
做為專案的一部分。 [簽署我們的CLA](https://opensource.adobe.com/cla.html)。 您
您只需要提交Adobe CLA一次，因此如果您之前已送出CLA，
一切準備就緒！

## 程式碼檢閱

所有提交內容皆應以提取請求的形式提出，且須經過稽核
依專案提交者。 閱讀[GitHub的提取請求檔案](https://docs.github.com/articles/about-pull-requests/)
以取得傳送提取請求的詳細資訊。

最後，請於下列時間遵循[提取請求範本](PULL_REQUEST_TEMPLATE.md)：
提交提取請求！

## 從貢獻者晉升提交者

我們樂於見到社群成員共襄盛舉！ 如果您不滿足於參與者的工作
並成為具有完整寫入存取權且在專案中擁有發言權的提交者，您必須
受邀加入專案。 現有的提交者會採用內部提名
邀請前必須達成懶惰共識（沈默表示同意）的程式
發行日期。 如果您覺得自己符合資格且想更深入參與，
歡迎連絡現有的提交者，與對方討論相關事宜。

## 安全性問題

不應在此問題追蹤器上回報安全性問題。 請改為[向我們的安全性專家提出問題](https://helpx.adobe.com/security/alertus.html)

## 最新資訊

如果您的變更引進新主題、重大更新或需要強調的更正，您可以從提取要求內文直接在[新增功能區段](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/overview#whats-new)新增簡短說明。

新增「新增功能」反白顯示：

1. 在結尾的提取要求內文中加入具有適當說明的`whatsnew`標籤。 說明應提供有關變更的前後關聯以及指向一或多個目標主題的連結。 使用以下格式（程式碼區塊引號僅供呈現，請勿將其納入提取請求內文中）：

   ```text
   whatsnew
   Short description of the change in the [target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html).
   ```

   或者，如果有多個主題：

   ```text
   whatsnew
   Short description of the changes in the [first target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html), [second target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-target-topic.html), and [third target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/third-target-topic.html).
   ```

   您也可以將清單用於多個反白專案：

   ```text
   whatsnew
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

   ```text
   whatsnew
   The following changes were made to the documentation:
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

1. 新增支援的標籤，以指出變更型別。 支援的標籤包括每種變更型別的標籤，例如：

   - `new-topic` — 新主題
   - `major-update` — 針對可能包含內容、結構或功能重大變更的主要更新
   - `technical` — 技術變更不被視為重大更新，但仍需要注意

**重要：**

1. `whatsnew`部分必須從`whatsnew`標籤開始，並且位於提取請求主體的結尾。
1. 變更的說明必須包含工作連結。 請確定連結正確且指向預期的主題。 如果是新主題，請在合併提取請求並發佈新主題後，確認連結運作正常。 可以在提取請求合併後修正連結。

例如，搜尋存放庫中的已關閉提取要求，瞭解現有醒目提示的格式設定，並與[新增功能區段](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/overview#whats-new)進行比較，瞭解它們在檔案中的顯示方式。
