﻿---
title: Lync Server 2013：定義封存需求
TOCTitle: 定義組織的封存需求
ms:assetid: ce0fc0f6-7704-4b80-bf19-a1fa9818fc7a
ms:mtpsurl: https://technet.microsoft.com/zh-tw/library/JJ205276(v=OCS.15)
ms:contentKeyID: 49292344
ms.date: 08/10/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中定義封存需求

 

_**上次修改主題的時間：** 2012-10-09_

如果您的組織必須遵守法令規範，可以部署封存以啟用 Lync Server 2013 立即訊息 (IM) 與會議功能的封存支援。如需關於可封存內容類型的詳細資訊，請參閱規劃文件中的 [Lync Server 2013 中的封存概觀](lync-server-2013-overview-of-archiving.md)。

若要實作封存，首先您必須決定要如何滿足貴組織的封存需求。這需要判斷下列項目：

  - **要在何時部署封存**。您可以在初次部署 Lync Server 2013 時部署封存，或將封存新增到現有的部署。若要部署封存，請使用 拓撲產生器將封存新增到拓撲，然後再發行拓撲。

  - **是否要封存內部或外部通訊**。您可以對內部通訊 (內部使用者之間的通訊) 和 (或) 外部通訊 (通訊至少有一方是位於您內部網路之外的使用者) 啟用封存。您可以為整個組織指定這些選項，也可以為特定的網站和集區指定這些選項。預設不會啟用任何選項。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398811.note(OCS.15).gif" title="note" alt="note" />附註：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果您使用 Microsoft Exchange 整合來存放封存的資料， Exchange 設定將控制是否封存 Lync 通訊。如果部署中包含多個樹系，您必須讓 Lync Server 和 Exchange 之間的設定同步。內部或外部通訊的封存，只能以 Lync 原則控制。就整合了 Exchange 的封存而言，兩者會一起封存或不封存。</td>
    </tr>
    </tbody>
    </table>


  - **為何要啟用封存**。您可以在全域層級為整個部署啟用及停用封存，也可以為特定的網站和使用者啟用及停用封存。在這每一個層級，您都要指定是否對 IM 工作階段 (對等) 和 (或) 會議 (多方工作階段的會議) 啟用封存。預設會停用封存。

  - **封存對貴組織的使用者而言多重要**。如果封存在貴組織裡至為重要，您可以指定 Lync Server 2013 以關鍵模式執行，如此會在封存失敗時封鎖 IM 和會議工作階段。例如：
    
      - 如果封存服務暫時無法將訊息傳送給資料庫佇列或將訊息插入資料庫，則部署中的 IM 與會議功能都會遭封鎖，直到封存支援恢復為止。
    
      - 如果會議使用者上傳檔案，但該檔案無法複製到封存檔案存放區，部署中的會議功能會遭到封鎖直到問題解決為止，但 IM 功能不會遭到封鎖。
    
    您可以在全域層級、網站層級和集區層級設定此選項。預設不會啟用關鍵模式。

  - **是否要使用 Microsoft Exchange 整合**。此選項會將封存儲存區與您的 Exchange 2013 儲存區整合，使得 Lync Server 封存資料和 Exchange 2013 封存資料一起存放於 Exchange 中。您可以使用 Microsoft Exchange 整合來儲存隸屬於 Exchange 2013 之使用者 (如果其信箱設為就地保留) 的封存資料。如果您沒有 Exchange 2013 部署、不想與它整合，或是有任何 Lync 使用者不隸屬於 Exchange 2013，您可以另外部署封存資料庫 (使用 SQL Server 存放來自 Lync 通訊的封存資料)。您可以在全域層級、網站層級和集區層級設定 Microsoft Exchange 整合選項。預設不會啟用 Microsoft Exchange 整合。

  - **要如何管理封存資料**。封存資料庫並不適合長期保留用途，且 Lync Server 2013 不提供電子探索解決方案來搜尋封存的資料，因此資料必須移到其他儲存區。 Lync Server 不提供工作階段匯出工具來匯出封存的資料或是建立封存資料的可搜尋文字記錄。在全域原則，以及您所建立的每個網站和使用者原則中，您可以啟用資料清除，並指定下列其中一個選項：
    
      - 在經過指定的天數之後，同時清除匯出的封存資料與儲存的封存資料。可以指定的最低天數為 1 天。可以指定的最高天數為 2562 天。
    
      - 只清除匯出的封存資料。此選項會清除所有已經匯出，且被工作階段匯出工具標成可安心刪除的記錄。
    
    您可以在全域層級、網站層級和集區層級設定此選項。預設不會啟用清除。

您可用來控制封存的方法如下：

  - **封存原則**。您可以使用一或多個封存原則，啟用及停用內部與外部通訊的封存。預設不會啟用封存。若要啟用或停用位於部署中之內部通訊和 (或) 外部通訊的封存，您可以使用預設的全域原則。全域原則是無法刪除的。您可以指定一或多個選用網站原則，為特定網站啟用或停用內部及外部通訊的封存。您也可以指定一或多個使用者原則，為特定的使用者和使用者群組來啟用或停用封存。使用者層級的原則優先於網站原則。網站層級的原則優先於全域層級的原則。使用者層級的原則只會對設為使用該原則的特定使用者實作。群組的立即訊息與會議只有在至少其中一個參與者的原則已設為啟用封存時，才會受到封存。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398811.note(OCS.15).gif" title="note" alt="note" />附註：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果您使用 Microsoft Exchange 整合，則對於所有隸屬於 Exchange 2013 伺服器的使用者而言， Exchange 2013 原則優先於 Lync Server 封存原則。</td>
    </tr>
    </tbody>
    </table>


  - **封存設定**。您使用一或多項封存設定來指定本主題先前所述的大多數封存選項，只有啟用內部與外部通訊的封存時除外 (那是使用封存原則來設定，如上項所述)。封存設定包含預設的全域設定和選用的網站與集區設定。全域設定是無法刪除的。集區層級的設定優先於網站層級的設定。網站層級的設定優先於全域層級的設定。

在需求分析過程中，您需要判斷要如何設定全域封存設定和全域封存原則。您也需要判斷自己對於任何網站層級封存設定、集區層級封存設定、網站層級封存原則和使用者層級封存原則的需求。

如果您為某個前端集區或 Standard Edition Server 部署封存，便應該為部署中的所有其他前端集區和 Standard Edition Server 啟用封存。需要這麼做的原因是，需要將己身通訊封存的使用者可能會受邀參加不同集區上裝載的群組 IM 交談或會議。如果裝載交談或會議的集區上未啟用封存，便可能不會封存所有的會議資料。封存仍然會發生於已啟用封存的使用者以及所有 IM 訊息，但可能不會封存會議內容和事件。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398811.note(OCS.15).gif" title="note" alt="note" />附註：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>為了能夠委派系統管理工作，同時又維持貴組織的安全性標準， Lync Server 2013 使用角色型存取控制 (RBAC)。使用 RBAC 時，系統管理權限的授與是透過對使用者指派預先定義的系統管理角色來達成。使用者若要設定 Lync 封存原則與封存組態，必須被指派 CsArchivingAdministrator 角色 (除非設定是直接在已部署封存的伺服器上進行，而非從另一部電腦遠端進行)。如需關於 RBAC 的詳細資訊，請參閱規劃文件中的 <a href="lync-server-2013-planning-for-role-based-access-control.md">在 Lync Server 2013 中規劃角色型存取控制</a>。如需封存部署時所需的使用者權限與角色清單，請參閱同時出現在規劃文件及部署文件中的 <a href="lync-server-2013-deployment-checklist-for-archiving.md">Lync Server 2013 中封存的部署檢查單</a>。<br />
如果您使用 Microsoft Exchange 整合，則需要有適當的系統管理員權限才能設定 Exchange 原則。如需詳細資訊，請參閱 Exchange 2013 文件。</td>
</tr>
</tbody>
</table>

