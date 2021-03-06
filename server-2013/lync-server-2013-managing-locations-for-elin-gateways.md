﻿---
title: Lync Server 2013：管理 ELIN 閘道的位置
TOCTitle: 管理 ELIN 閘道的位置
ms:assetid: ced79c13-4e7e-4034-95cd-6fc913f4f222
ms:mtpsurl: https://technet.microsoft.com/zh-tw/library/JJ205288(v=OCS.15)
ms:contentKeyID: 49292363
ms.date: 08/10/2015
mtps_version: v=OCS.15
ms.translationtype: HT
---

# 在 Lync Server 2013 中管理 ELIN 閘道的位置

 

_**上次修改主題的時間：** 2015-03-09_

若要讓 Lync Server 自動將位置提供給網路內的用戶端，必須執行下列動作：

  - 在 位置資訊服務資料庫填入網路接線圖，並在 CompanyName 欄位中加入「緊急位置識別碼」(ELIN)。

  - 發佈位置以供網路中的用戶端使用。

  - 將 ELIN 上傳至公用交換電話網路 (PSTN) 電訊廠商的「自動位置識別」(ALI) 資料庫。

如需如何執行這些工作的詳細資訊，請參閱部署文件中的＜ [在 Lync Server 2013 中設定位置資料庫](lync-server-2013-configure-the-location-database.md)＞。

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398811.note(OCS.15).gif" title="note" alt="note" />附註：</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>在使用 Lync Server 管理命令介面 命令發佈位置，並複寫到集區的本機存放區之前，用戶端無法使用新增到中央位置資料庫的位置。如需詳細資訊，請參閱部署文件中的＜ <a href="lync-server-2013-publish-the-location-database.md">在 Lync Server 2013 中發佈位置資料庫</a>＞。</td>
</tr>
</tbody>
</table>


本節說明在規劃更新和維護位置資料庫時應要考量的事項。

## 規劃緊急位置

使用 ELIN 閘道時，您要在 位置資訊服務資料庫為每個位置填入市民地址 (建築內的特定位置) 和至少一個 ELIN。最好可在規劃階段期間，決定要如何命名位置以及想要如何指派 ELIN。

## 規劃位置名稱

含有建築物內特定位置的 位置資訊服務**Location**欄位長度上限為 20 個字元 (包括空格)。請嘗試在有限的長度內包括下列各項：

  - 簡單易懂且可識別 119 致電者位置的名稱，確保緊急救護員到達市民地址時能迅速找到該特定位置。此位置名稱可包含建築物編號、樓層編號、側廳指示項、房號等。避免使用只有員工才知道的暱稱，其可能會讓緊急救護員前往錯誤的位置。

  - 位置識別碼，幫助使用者輕鬆看出其 Lync 用戶端已取得正確的位置。Lync 用戶端會在其標頭自動串連和顯示找到的 **Location** 與 **City** 欄位。建議您在每個位置識別碼加上建築物的街道地址 (例如 \<街道編號\> 1 樓)。不加上街道地址，一般位置識別碼 (例如 1 樓) 可泛指城市中的任何建築物。

  - 如果是由無線存取點決定的概略位置，您可能想要加上 Near的字詞 (例如 1234 號 1 樓附近)。

## 規劃 ELIN

在您決定如何將建築物空間劃分成位置之後，必須決定要指派給每個位置的 ELIN 個數。例如，在多樓層或多租戶建築物中，可以將不同的緊急區域指派給建築物中的不同區域。一般來說，會將建築物的每一樓層指定為一個位置。接著，將一或多個 ELIN 指派給每個位置，用以在撥打緊急電話時做為撥打號碼。請連絡您的 PSTN 電訊廠商以取得可用於 ELIN 的電話號碼。下表提供一個特定街道地址的位置範例。

### 位置和 ELIN 指派範例

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>建築物區域</th>
<th>位置</th>
<th>ELIN</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>一樓</p></td>
<td><p>1</p></td>
<td><p>425-555-0100</p></td>
</tr>
<tr class="even">
<td><p>二樓</p></td>
<td><p>2</p></td>
<td><p>425-555-0111</p></td>
</tr>
<tr class="odd">
<td><p>三樓</p></td>
<td><p>3</p></td>
<td><p>425-555-0123</p></td>
</tr>
</tbody>
</table>


定義的位置應符合下列需求：

  - 關於每個位置的最大涵蓋區域及每個街道地址的位置數目，均遵守地方與國家/區域法規。

  - 相當明確，可以輕鬆找到緊急電話的來電者。

## 填入位置資料庫

下列問題可協助您決定如何填入位置資料庫。

  - **您將使用何種程序填入位置資料庫？**  
    資料會出現在何處，以及需要執行哪些步驟將資料轉換成位置資料庫所需的格式？您將個別加入位置，或是使用 CSV 檔大量加入位置？

<!-- end list -->

  - **您是否有已包含位置對應的協力廠商資料庫？**  
    使用 Lync Server 的次要 位置資訊服務選項連線至協力廠商資料庫，就能使用離線平台將位置分組並進行管理。此方法的好處在於，除了將位置與網路識別碼產生關聯之外，您還可以將位置與使用者產生關聯。這表示， 位置資訊服務可以將多個自次要 位置資訊服務產生的位址傳回 Lync Server 用戶端。然後使用者可以選擇最適合的位置。
    
    若要與 位置資訊服務整合，協力廠商資料庫必須遵循 Lync Server 位置要求/回應結構描述。如需詳細資訊，請參閱 [http://go.microsoft.com/fwlink/?linkid=213819\&clcid=0x404](http://go.microsoft.com/fwlink/?linkid=213819%26clcid=0x404)。如需部署次要 位置資訊服務的詳細資訊，請參閱部署文件中的＜ [在 Lync Server 2013 中設定次要位置資訊服務](lync-server-2013-configure-a-secondary-location-information-service.md)＞。

如需填入位置資料庫的詳細資訊，請參閱部署文件中的＜ [在 Lync Server 2013 中設定位置資料庫](lync-server-2013-configure-the-location-database.md)＞。

## 維護位置資料庫

填入位置資料庫之後，您必須發展可隨著網路組態變更而更新資料庫的策略。下列問題將協助您決定如何維護位置資料庫。

  - **您要如何更新位置資料庫？**  
    有數種情況需要更新位置資料庫，包括新增無線存取點 (WAP)、辦公室重新佈線 (造成指派不同的交換器) 以及擴充子網路。您將直接更新每個個別位置，或是使用 CSV 檔大量更新所有位置？

<!-- end list -->

  - **您要使用 SNMP 應用程式將 Lync 用戶端的 MAC 位址來與連接埠和交換器識別碼比對嗎？**  
    如果您使用 SNMP 應用程式，就必須發展可讓 SNMP 應用程式與位置資料庫之間的交換器底座與連接埠資訊保持一致的手動程序。如果 SNMP 應用程式傳回資料庫中沒有的底座 IP 位址或連接埠識別碼， 位置資訊服務即無法將位置傳回用戶端。

