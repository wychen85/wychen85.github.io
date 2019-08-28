---
layout: post
title:  "在 Windows10 運行 Ubuntu 子系統 (ft. 將子系統移動到其他硬碟)"
date:   2019-08-28 08:49:00 +0800
categories: ubuntu
---

## STEP 1 : 轉成開發人員模式

先點擊螢幕左下角的放大鏡圖示

![](https://miro.medium.com/max/236/1*7sJgfZbKAbvMVcXml-IR3Q.png)

然後搜尋『開發人員專用設定』

![](https://miro.medium.com/max/934/1*sh5tJOeW942aTzjhsC9k2w.png)

<br/>

接著勾選『開發人員模式』

![](https://miro.medium.com/max/1363/1*wt_QEkkcOfn7OR0f-6eQpA.png)

<br/>

---

<br/>

## STEP 2 : 打開 Linux Bash 功能

在螢幕左下方的放大鏡搜尋『開啟或關閉 Windows 功能』

![](https://miro.medium.com/max/630/1*R21BPCn0xMdd6d-Ubm_UPQ.png)

<br/>

將『適用於 Linux 的 Windows 子系統』的選項打開後按確定

![](https://miro.medium.com/max/608/1*2bGkrMbcu4ZA2IRShFUJDg.png)

<br/>

---

<br/>

## STEP 3 : 安裝 Ubuntu 子系統

使用螢幕下方的放大鏡搜尋『Microsoft Store』並開啟

接著在商店裡搜尋『Ubuntu』

![](https://miro.medium.com/max/1583/1*VQJKpwNTopTz7mst_OnkHg.png)

> 你可以發現有 `Ubuntu`, `Ubuntu 16.04 LTS`, `Ubuntu 18.04 LTS`，而在這裡我安裝了『Ubuntu 16.04 LTS』

<br/>

![](https://miro.medium.com/max/1585/1*MYnW-3YQfSEHD7O1itL6Dg.png)

<br/>

---

<br/>

## STEP 4 : 開啟 Linux 子系統的 Bash Shell

在螢幕左下角的放大鏡搜尋『Bash』並開啟

![](https://miro.medium.com/max/615/1*K_K5BuCdnm_gZ1wlUoKV9A.png)

<br/>

開啟後即可開始使用 Ubuntu 16.04 的 Bash Shell

![](https://miro.medium.com/max/1029/1*dgPZNb2a6z4bjqvOATDcQg.png)

<br/>

到這裡就大功告成了，但有 Linux 子系統的 Root File System 是預設安裝在系統碟裡的，而通常你的系統碟都是容量小的 SSD，僅作為放置 Windows 作業系統使用，若 Linux 子系統也放置於此，長期使用下來後可能會佔據不少的容量

因此，從下個步驟開始我會教你如何將放置於系統碟的 Linux 子系統搬到其他的硬碟，如果你是有此需求的人就繼續往下看，沒有的話就不需要做以下步驟囉~

<br/>

---

<br/>

## STEP 5 : 安裝 choco

請你在螢幕左下角的放大鏡搜尋『Windows Power Shell』，並對他點擊右鍵來使用**系統管理員**執行

執行後請輸入以下指令，並按 Enter

{% highlight bash %}
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString(‘https://chocolatey.org/install.ps1'))
{% endhighlight %}

![](https://miro.medium.com/max/1131/1*IjjqGEF0_EeRxjrFxJyMhA.png)

<br/>

---

<br/>

## STEP 6 : 安裝 LxRunOffline

安裝完 choco 之後，繼續在 Windows Power Shell 中輸入以下指令來安裝 `LxRunOffline`

{% highlight bash %}
choco install lxrunoffline
{% endhighlight %}

<br/>

---

<br/>

## STEP 7 : 在另一個硬碟建資料夾並設置權限

以我為例子，我的系統碟是 C 槽，而現在我想將 Linux 子系統移動到 D 槽，所以我在 **D 槽建一個資料夾名為 wsl**

![](https://miro.medium.com/max/1186/1*KnW_GkXhAq_HKkzTCNi-KA.png)

<br/>

接著繼續在剛剛的 Windows Power Shell 上輸入指令『whoami』來查看你的使用者名稱。以我為例子，我的輸出為 `laptop-binqens5\chenw`，因此 `chenw` 就是我的使用者名稱

![](https://miro.medium.com/max/365/1*_c6kUm-LPPQpRo7ZmRIAIg.png)

<br/>

使用以下指令來給予你的 D:\wsl 資料夾權限

{% highlight bash %}
icacls D:\wsl /grant "chenw:(OI)(CI)(F)"
{% endhighlight %}

![](https://miro.medium.com/max/781/1*8l8BYiFVDmPxstwxdNL9lw.png)

> 請注意記得將『chenw』換成你自己的使用者名稱

<br/>

---

<br/>

## STEP 8 : 使用 LxRunOffline 來移動 Linux 子系統

繼續在剛剛的 Windows Power Shell 上輸入以下指令

> 在這裡請注意，如果你剛剛在 Microsoft store 裡面下載的是 `Ubuntu 18.04 LTS` 的話，請將以下指令的 `Ubuntu-16.04` 改為 `Ubuntu 18.04`。而倘若你剛剛下載的是 `Ubuntu`，則請將以下指令的 `Ubuntu-16.04` 改為 `Ubuntu`

{% highlight bash %}
lxrunoffline move -n Ubuntu-16.04 -d D:\wsl\installed\Ubuntu-16.04
{% endhighlight %}

執行這個指令後，可能會有一些 WARNING 訊息，而這是正常的，所以請不用太在意，移動 Linux 子系統的過程需要幾分鐘的時間，務必耐心等候，不要隨意關閉 Windows Power Shell

完成之後你可以用以下指令來確認你的 Linux 子系統是否成功移動到我們自己創建的資料夾中

{% highlight bash %}
lxrunoffline get-dir -n Ubuntu-16.04
{% endhighlight %}

![](https://miro.medium.com/max/693/1*1589HtoiqIVih-hQC0h83w.png)

<br/>

若你的步驟跟我一樣，則輸出訊息應該會跟我一樣，到這裡恭喜你成功了 !

<br/>

---

<br/>

## STEP 9 : 開啟你的 Linux 子系統

請使用以下指令來開啟你的 Linux 子系統 Bash Shell

{% highlight bash %}
wsl
{% endhighlight %}

![](https://miro.medium.com/max/570/1*6iCCanRlPxJPvGcmlrymYw.png)

<br/>

如此一來你在 Linux 子系統上做的任何更新或者安裝的任何東西都不會占用你的系統碟容量，而是占用你的 D 槽或其他你所設置的硬碟

<br/>

---

<br/>

## 參考文獻

- [Windows 10 運行 Ubuntu Linux 的 Bash Shell 原生執行環境教學](https://blog.gtwang.org/windows/how-to-get-ubuntu-and-bash-running-on-windows-10/)

- [Move WSL (Bash on Windows) root filesystem to another hard drive?](https://stackoverflow.com/questions/38779801/move-wsl-bash-on-windows-root-filesystem-to-another-hard-drive)

- [A full-featured utility for managing Windows Subsystem for Linux (WSL)](https://github.com/DDoSolitary/LxRunOffline)
