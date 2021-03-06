﻿---
title: 在 Lync Server 2013 中變更封存資料庫選項
TOCTitle: 在 Lync Server 2013 中變更封存資料庫選項
ms:assetid: 3775f09d-65b0-48bc-8a4d-d97bd0c3423c
ms:mtpsurl: https://technet.microsoft.com/zh-tw/library/JJ204814(v=OCS.15)
ms:contentKeyID: 49290589
ms.date: 08/10/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中變更封存資料庫選項

 

_**上次修改主題的時間：** 2012-11-01_

如果您使用 SQL Server 儲存體來封存任何使用者的儲存體，以作為部署封存的方式，則可對資料庫儲存體做出下列變更：

  - 使用不同的 SQL Server 資料庫來封存儲存體。這包括主要封存資料庫及任何用於 SQL Server 鏡像的資料庫。

  - 切換至 Microsoft Exchange 整合，將封存資料及檔案儲存在 Exchange 2013 伺服器上。如果所有使用者的主伺服器皆在 Exchange 2013 伺服器上，且想對於部署中所有的使用者使用 Microsoft Exchange 儲存體，則應從拓撲中移除 SQL Server 存放區資料庫。

如要做其中任一變更，您必須執行拓撲產生器、做出變更，然後再次發行拓撲。您可以使用拓撲產生器來執行此一作業。請勿指定 \[封存 SQL Server 儲存區\] 或 \[啟用 SQL Server 儲存區鏡像\] 資訊，除非某些 Lync 使用者的主伺服器不在 Exchange 2013 伺服器上。

## 變更封存資料庫選項

1.  在執行 Lync Server 2013 或安裝 Lync Server 系統管理工具的電腦上，以本機 Users 群組成員的帳戶 (或者擁有對等使用者權限的帳戶) 身分登入。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg398811.note(OCS.15).gif" title="note" alt="note" />附註：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>您可以使用本機 Users 群組成員的帳戶來定義拓撲，但是若要發行拓撲 (在拓撲中新增元件的必要作業)，則必須使用 <strong>Domain Admins</strong> 群組及 <strong>RTCUniversalServerAdmins</strong> 群組成員的帳戶 (或者使用擁有對等權限的帳戶)，而且對於用在 Lync Server 2013 檔案存放區的檔案共用，擁有完全控制權限 (即讀取、寫入及修改)，讓拓撲產生器可以設定必要的判別存取控制清單 (DACL)。</td>
    </tr>
    </tbody>
    </table>


2.  啟動拓撲產生器。

3.  在主控台樹狀目錄中，瀏覽至部署封存的前端集區，然後按一下要變更資料庫選項的前端集區名稱。

4.  在 \[動作\] 功能表上，按一下 \[編輯屬性\]。

5.  在 **\[編輯內容\]** 對話方塊中，按一下 **\[一般\]**。

6.  向下捲動至 **\[封存\]**。

7.  在 **\[封存\]** 中，執行下列動作：
    
      - 如要變更至不同的現有 SQL Server 儲存區，在 \[封存 SQL Server 儲存區\] 的下拉式清單方塊中，執行下列動作：
        
          - 如要使用現有的 SQL Server 儲存區，在下拉式清單方塊中，按一下要使用的 SQL Server 儲存區名稱。
        
          - 如要指定新的 SQL Server 儲存區，按一下 **\[新增\]**，然後在 **\[定義新的 SQL Server 儲存區\]** 對話方塊中，執行下列動作：
            
              - 如要使用現有的 SQL Server 儲存區，在下拉式清單方塊中，按一下要使用的 SQL Server 儲存區名稱。
            
              - 如要指定新的 SQL Server 儲存區，按一下 **\[新增\]**，然後在 **\[定義新的 SQL Server 儲存區\]** 對話方塊中，執行下列動作：
                
                  - 在 **\[SQL Server FQDN\]** 中，指定要建立新 SQL Server 儲存區的伺服器 FQDN。
                
                  - 按一下 **\[預設執行個體\]** 以使用預設執行個體，或者要指定不同的執行個體的話，按一下 **\[具名執行個體\]**，然後指定要使用的執行個體。
                
                  - 如果指定的 SQL Server 執行個體有鏡像關係，則選取 **\[此 SQL 執行個體有鏡像關聯\]** 核取方塊，然後在 **\[鏡像連接埠號碼\]** 中指定連接埠號碼。
    
      - 如要新增 SQL Server 儲存區作為鏡像，或者變更至不同的現有 SQL Server 儲存區作為 SQL Server 儲存區鏡像，則選取 **\[啟用 SQL Server 儲存區鏡像\]**，然後執行下列動作：
        
          - 如要使用現有的 SQL Server 儲存區作為鏡像，在 **\[封存 SQL Server 儲存區鏡像\]** 下拉式清單方塊中，按一下要用於鏡像的 SQL Server 儲存區。
        
          - 如要指定新的 SQL Server 儲存區作為鏡像，按一下 **\[新增\]**，然後在 **\[定義新的 SQL Server 儲存區\]** 對話方塊中，執行下列其中一項動作：
            
            1.  在 **\[SQL Server FQDN\]** 中，指定要建立新 SQL Server 儲存區的 SQL Server FQDN。
            
            2.  按一下 **\[預設執行個體\]** 以使用預設執行個體，或者要指定不同的執行個體的話，按一下 **\[具名執行個體\]**，然後指定要使用的執行個體。
            
            3.  如果指定的 SQL Server 執行個體有鏡像關係，則選取 **\[此 SQL 執行個體有鏡像關聯\]** 核取方塊，然後在 **\[鏡像連接埠號碼\]** 中指定連接埠號碼。
        
          - 如果啟用 SQL Server 鏡像且要新增或變更 SQL Server 鏡像見證 (第三個獨立 SQL Server 執行個體，可偵測主要 SQL Server 伺服器及鏡像執行個體的健全狀態)，則選取 **\[使用 SQL Server 鏡像見證以啟用自動容錯移轉\]** 核取方塊，然後執行下列其中一項動作：
            
            1.  在 **\[SQL Server FQDN\]** 中，指定要建立新 SQL Server 鏡像見證的伺服器 FQDN。
            
            2.  按一下 **\[預設執行個體\]** 以使用預設執行個體，或者要指定不同的執行個體的話，按一下 **\[具名執行個體\]**，然後指定要作為鏡像見證的執行個體。
            
            3.  如果指定的 SQL Server 執行個體有鏡像關係，則選取 **\[此 SQL 執行個體有鏡像關聯\]** 核取方塊，然後在 **\[鏡像連接埠號碼\]** 中指定連接埠號碼。
    
      - 如要切換至 Microsoft Exchange 整合，將封存資料及檔案儲存在 Exchange 2013 伺服器上 (如果部署中所有使用者的主伺服器皆在 Exchange 2013 伺服器上)，請刪除封存資料庫的所有資訊。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg412908.important(OCS.15).gif" title="important" alt="important" />重要事項：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>如果有任何 Lync 使用者的主伺服器不在 Exchange 2013 伺服器上，請勿刪除 SQL Server 儲存區資訊。</td>
    </tr>
    </tbody>
    </table>


8.  如要儲存設定，按一下 **\[確定\]**。
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg412908.important(OCS.15).gif" title="important" alt="important" />重要事項：</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>在拓撲產生器中所做的變更，在發行新的拓撲之後才會發揮作用。如需詳細資訊，請參閱部署文件中的＜<a href="lync-server-2013-publishing-the-updated-topology-to-add-archiving-databases.md">發佈已更新的拓撲以新增封存資料庫</a>＞。</td>
    </tr>
    </tbody>
    </table>

