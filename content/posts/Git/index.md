---
title: "Git"
date: 2021-11-09T21:16:22+08:00
---

## git flow概念參考:

[Git Branching_好吃、新奇、又好玩](https://learngitbranching.js.org/?locale=zh_TW) 

[Git Branching fakeTeamwork指令](https://github.com/pcottle/learnGitBranching/issues/381)

## 貼紙概念參考:

[為你自己學Git_高見龍](https://gitbook.tw/)

[UI介紹_配合指令](https://www.notion.so/UI-_-d3e07877097341b4a24ea5a8cecef905)

# 安裝設定

所有的git指令皆是要打在Git Bash裡(此為安裝完Git就會有的東西)

![GitBash.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/GitBash.png)

需先設定本機的使用者名稱與email

```bash
git config --global user.name "君の名は"
git config --global user.email "你的email"
```

## 其他環境設定

- [編譯器設定:VSCode](https://w3c.hexschool.com/git/4ac1e4e1)：

```bash
git config --global core.editor "code --wait"`
```

- [git 其他全域參數設定](https://stackoverflow.com/questions/30024353/how-to-use-visual-studio-code-as-default-editor-for-git)

# 為何使用Git

# 想要有備份還原功能

## 一切的開始(備份/還原)

在開始前，請記得先用git bash程式切換至要讓git管理的檔案夾後，打上

```bash
git init
```

git 才會開始幫你管理檔案備份喔! 此指令一個檔案夾內只要做一次就好。

現在，如果檔案夾內有一份test.html文件，並且想使用git來幫助我們對這份檔案進行備份/還原的操作，則在git bash視窗中打上

### 備份指令(git commit)

```bash
git add test.html
git commit -m "1/1號，將test.html 文件交由git控管"
```

### 代碼解釋

`git add ...` ⇒ 勾選這次要備份的文件 

`git commit -m "..."` ⇒ 備份，並為這次的備份寫上備註，-m的m代表message的意思。

但由於此份檔案沒有被修改過，是沒辦法看出備份跟還原的功能的，所以讓我們先假設時間來到了1/2號，我們編輯過了此份test.html文件，由於此份text.html文件已修改過的關係，所以需要再將它備份一次，再執行一次上述指令

```bash
git add test.html
git commit -m "1/2號，編輯test.html，新增一些段落"
```

由git flow的圖形來看會像是這樣

![master.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/master.png)

這裡我在寫commit message的時候都有將日期寫上，但實際操作是可以不用的，因為git會自動幫我們紀錄我們是何時備份的，我們可以使用

```bash
git log
```

來幫助我們查看每一個commit到底記錄了甚麼 

![git log畫面.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/git_log%E7%95%AB%E9%9D%A2.png)

這邊可以看到除了備份的時間和當初寫的備份描述外，還可以看到每個commit後面都接了一串英數混合字串，這個是git自動為每個commit命名的編號(使用SHA-1得到)，當我們要指定還原到哪一次備份的時候就需要用到這些編號了。

### 還原指令(git checkout)

```bash
git checkout 1849273139bde9189f835b03f007654157aef465
```

### 代碼解釋

`git checkout ...` =>選擇指定版本的備份文件，可以看成是將文件還原至指定的版本會比較好理解。

可以看到由於不想要有重複名稱的關係，git幫我們自動命名的版次編號都非常長，為了方便使用，git允許讓你只要輸入編號的前面一小段就好(長度不限，只要你輸入的字串足夠讓git不會分不出你要指定的是哪個版本) 因此可以將上面的指令可以改成這樣

```bash
git checkout 184927
```

但為了方便之後講解，我會改用git flow圖示中的代碼ex:C0、C1…來稱呼

```bash
git checkout C0
```

實際操作這樣寫是不會成功的，除非剛好有個版次編號開頭叫C0

git flow圖示長這樣

![還原.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/%E9%82%84%E5%8E%9F.png)

圖示中的HEAD代表的是當前文件版本，更詳細的描述會在待會的貼紙功能介紹。

做完checkout步驟後就已經將檔案還原至1/1的版本了!

# 進一步功能:貼紙功能

備份還原講完了，那還有甚麼需要講的? 欸~如果git只有備份還原的功能的話，是不會成為現在最受歡迎的版控軟體的， 這邊在進一步的為大家介紹他的貼紙功能，(貼紙是為了方便理解所稱呼的，實際的運作原理大家可以再多看其他人寫的git介紹)。 

相信從上面的介紹中的git flow圖例展示中大家都有發現除了代表不同版本文件的圓形(C0、C1)和版本描述外，圖例中還有master、HEAD兩個圖示，你可以將它看成像是這個東西 

![3M標籤貼.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/3M%E6%A8%99%E7%B1%A4%E8%B2%BC.png)

## Git預設給你兩張標籤貼:master、HEAD

先解釋HEAD貼紙的功用 

HEAD: Git在你目前使用的文件版本上貼了一個HEAD貼紙， 告訴你目前是在使用這個版本喔!`也就是你checkout指定了哪個文件HEAD就會貼到那個文件上。`

讓我們用這個思路來看之前的備份還原是怎麼操作的 

## 如果只是備份

```bash
git add test.htmlgit commit -m "1/2號，編輯test.html，新增一些段落"
```

![master.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/master%201.png)

每一次備份(commit)，Git會自動幫我們把當下選擇的標籤貼(master為我們目前選擇的標籤貼)和HEAD標籤貼貼到當前的版本上，master*中的*代表HEAD貼紙是貼在master這個貼紙上的，可以先看成有兩張貼紙master和Head貼在C1上。

![master&HEAD.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/masterHEAD.png)

### 需要還原的時候

```bash
git checkout C0
```

![還原.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/%E9%82%84%E5%8E%9F%201.png)

可以看到git一樣會將Head貼在你目前所使用的版本文件上，告訴你目前在使用這個版本，然而master卻留在了原本的位置上。 這樣的好處在於當你想回到最新的版本的時候，原本應該是需要

```bash
git checkout C1
```

> 注意:C0、C1在實際操作應該會是一組SHA-1的字串，需要使用git log查看
> 

現在可以改為這樣

```bash
git checkout master
```

就能回到最新版本了，不用再去查看那個git為我們自動命名的醜醜的版次編號了。 

> 可以看成原本是要去記我看到書中的第幾頁，
但我們在該頁貼上了一個master的標籤貼，因此不再需要記頁碼，只要手放到那個貼紙上一翻就回到了原本看的頁面了
> 

看到這裡想必很多人已經在想，那如果有更多貼紙就更好了!

我可以在每個我想做標記的地方貼上標籤貼作註記，這樣之後就都不需要再使用`git log`查看版本號了。

沒錯!所以git 提供了`git branch`指令幫你創建貼紙

## 創建貼紙(git branch)

```bash
git branch 我是一張貼紙
```

### 代碼解釋

`git branch ...`=>創建一張名稱為…的貼紙

<aside>
😄 至於為什麼不叫label要叫branch，看下去就知道了

</aside>

git flow圖示長這樣

![git branch.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/git_branch.png)

回到實際狀況的應用，我在1/3突然想到了一個新功能，但不確定能不能做出來，所以我可以先請git幫我新增一張名為newDesign的貼紙

```bash
git branch newDesign
```

git flow 

![newDesign.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/newDesign.png)

但這時候我其實還沒有告訴git我要選擇這張貼紙，因此你需要再使用

```bash
git checkout newDesign
```

來告訴git我現在要選擇這張貼紙 

git flow 

![SelectNewDesign.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/SelectNewDesign.png)

> 注意*從master跑到newDesign這張貼紙上了，
代表你目前的HEAD貼紙是貼在newDesign上。
代表你當前的檔案版本為newDesign所在的檔案版本。
> 

git也提供了指令

```bash
git checkout -b 貼紙名稱
```

幫助你去選擇指定的貼紙，如果沒有這張貼紙則創建一個，並選擇它。

接下來就是繼續你的開發流程了 只要有修改過檔案就重複備份的操作

```bash
git add text.html
git commit "1/3號，新增功能1"
```

由於你現在選擇的是newDesign這張貼紙，

使用commit的時候git只會自動幫你把newDesign跟HEAD兩張貼紙貼到當前版本，

master貼紙自然就留在了原本的版本上。

git flow 

![newDesignStep1.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/newDesignStep1.png)

假設1個月後，你終於發現這個功能根本做不出來，整個檔案死機了，這時候使用git的好處就出來了，你只要

```bash
git checkout master
```

git就自動幫你把檔案復原成當初不會死機的版本了(還原到master標籤在的那個版本) 

git flow 

![re-backToMaster.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/re-backToMaster.png)

那如果你根本是做出了一個超酷的功能呢? 

你不會想要將master標籤繼續留在那個又舊又老的版本上(畢竟你可能完全不會再用到那個版本了對吧!)，

你可以這樣做

## 更新、合併(git merge)

```bash
git checkout master
git merge newDesign
```

### 代碼解釋

`git checkout master` =>選擇master標籤貼 

`git merge newDesign`=>將master標籤貼貼到newDesign目前的版本上 

git flow 

![merge.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/merge.png)

# 那麼來聊聊為什麼要叫branch

實際上，一個專案裡面不會只有一個檔案，當使用git在控管一個專案的資料夾時，該專案資料夾可能會有好幾份文件，

假設目前要新增一個功能，我是新增一個檔案去寫該功能的程式碼，而使用git做版控的時候，就需要以下指令

```bash
git checkout -b 功能B
git add 檔案B
git commit -m "新增檔案B"
```

git flow 

![新增檔案Ｂ.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/%E6%96%B0%E5%A2%9E%E6%AA%94%E6%A1%88%EF%BC%A2.png)

但這時候你發現有個原本的功能，只要稍加修改就能改善你整個專案的速度，你可能會選擇這樣做

```bash
git checkout master
git add 檔案A
git commit -m "修改檔案A內容改善專案速度"
```

```bash
git add 檔案C
git commit -m "修改檔案C內容改善專案速度"
```

git flow 

![mutipleBranch.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/mutipleBranch.png)

從gitflow來看是不是就像是你的專案長出了2條分支的版本管控了呢? 

這也是為什麼要叫branch的由來，

> 實際上master這條分支上會有版本更動通常是發生在多人協作的時候。
另外git add 有可以一次選擇很多檔案的功能`git add --all`or`git add .`，不需要像這邊一個檔案一個檔案的慢慢加入，這邊只是舉例方便才這樣操作
> 

而當我功能B做完的時候我可以選擇這樣做

```bash
git checkout master
git merge 功能B
```

### 代碼解釋

`git checkout master`=>選擇master標籤貼的分支，與master標籤貼。

`git merge 功能B`=>與功能B標籤貼所在的分支做合併

只要這兩條分支沒有動到相同的檔案的話，git會自動幫我們把兩條分支內有更動到的檔案整合在一起，並且產生一個合併過後的版本，但注意喔，在之前的講解，以newDesign做例子的時候原本的說法是我選擇了master這個標籤貼，並把它貼到newDesign當下所在的版本上，但那是在兩個標籤貼處於同一分支，沒有分支要合併的情況下。

再回頭看看目前的情況，git已經幫我們將兩個分支做合併，並產生了一個新的版本，你卻把master往功能B所在的版本貼不是很奇怪嗎?功能B標籤貼所在的版本是沒合併的版本耶，所以git當然是會自動的把master貼紙貼到合併後的版本上！

git flow 

![mergeMutilpleBranch.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/mergeMutilpleBranch.png)

> 只要你是進行兩個分支的合併的話，git會將你目前選擇的標籤貼往合併後的版本上貼的，
可以這樣去思考，執行git merge就是會將你當下選擇的標籤貼與要merge的標籤貼所在的版本(分支)作合併，
最後將你當下選擇的標籤貼貼到合併過後的最新版本上。
> 

## 如果發生衝突了怎麼辦?

有時候你就是有可能在不同的分支上對同一份文件做了修改，這時候git會很貼心的提醒你，因為在兩個分支都有修改過XXX檔案，所以發生衝突了。 

以上面的例子而言，假設我功能B這個分支在修改時，不只新增了檔案B，也不小心修改到了檔案A，而且當初add的時候是使用`git add .` 的指令，把全部檔案都備份了，那麼當選擇了master貼紙並使用merge的時候就會產生以下錯誤

```bash
❯ git merge 功能B
Auto-merging 檔案A
CONFLICT (content): Merge conflict in 檔案A
Automatic merge failed; fix conflicts and then commit the result.
```

> 不要期待git能幫你分辨哪個分支上的修改才是你需要的，
有提醒你就已經很不錯了，不過git會幫你把兩個分支的修改都呈現出來，要保留哪一個就需要交由妳自己來判斷囉。
> 

如果是簡單的檔案，可以直接點開該檔案，git會自動幫你把兩份文件有修改過但git無法判斷要保留哪個的地方呈現出來，如下圖所示

```css
body{    
	color: red;
}
h1{
<<<<<<< HEAD    
	color: blue
=======    
	color: red
>>>>>>> 功能B}
```

`=======`上半部到`<<<<<<< HEAD`的部分是當前分支的文件內容

`=======`下半部到`>>>>>>> 功能B`的部分是要合併的分支的該文件內容，

將你不想保留的程式碼移除後再將該文件儲存，就可以再繼續merging動作了。 

如何繼續merging? 首先需要再次勾選造成衝突的文件。

```bash
git add 檔案A
```

並再做一次備份指令

```bash
git commit -m "conflict fixed"
```

merge完成。

## 如果是git無法判斷的複雜檔案呢

你可以先取消merge，取消方法請見以下網站

[取消merge_stackoverflow](https://stackoverflow.com/questions/5741407/how-to-undo-a-git-merge-with-conflicts) 

[假裝merge](https://gitbook.tw/posts/2018-07-20-git-merge-dry-run)

並再兩個分支中做切換，查看到底哪個分支中的檔案修改是你要的，之後再執行一次`merge`指令 當跳出conflict衝突訊息的時候 使用

```bash
git checkout --ours 檔案A
```

此指令可以保留目前選擇的貼紙所在分支的檔案 

使用

```bash
git checkout --theirs 檔案A
```

則可以保留要合併的分支的檔案 

之後再次勾選造成衝突的檔案，並執行備份指令即可

```bash
git add 檔案A
git commit -m "conflict fixed"
```

> 如果你很確定哪個分支的檔案是你要保留的話，是可以不用先取消merge的
> 

# 更進一步功能:遠端備份

## 在GitHub上建立專案

[SSH keys 設定](https://youtu.be/CeC_qyQHiCE)

[Clone方式](https://progressbar.tw/posts/3)

[本機Push方式](https://gitbook.tw/chapters/github/push-to-github.html)

## [環境設定](https://w3c.hexschool.com/git/fd426d5a)

### 設定遠端伺服器(遠端檔案儲存位置)

要使用遠端功能需先設定遠端伺服器的位置

```bash
git remote add origin git@github.com:...
```

### 代碼解釋

在當前git控管的資料夾上新增遠端伺服器的位置。

`origin`⇒可任意取名的遠端伺服器簡稱，代表的是後面的url，慣例叫origin。

`git@github.com:...`此處需替換成你遠端的儲存檔案位置 可以在GitHub上的專案這個位置看到 

![GitHub遠端位置.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/GitHub%E9%81%A0%E7%AB%AF%E4%BD%8D%E7%BD%AE.png)

> 如果使用clone方式創建一個git控管專案在本機，則可以不用此步驟，
github會自動幫你把遠端伺服器位置設定用好，並將伺服器簡稱設為origin
> 

## 將本機內容上傳給遠端

在講遠端之前先講一些簡單的規則 

- 本機端checkout可以選到遠端標籤貼所指向的版本，但無法選到遠端的標籤貼，
會處於detached HEAD狀態。[something about Remote Label](https://stackoverflow.com/questions/21651185/git-merge-a-remote-branch-locally)
- 本機的標籤貼不能貼到遠端上，因此會需要在新創遠端標籤貼或使用遠端原本就有的標籤貼。
- 貼紙只能在同一條分支上使用。
- 操作遠端的指令`push`、`fetch`，通常會順便更新遠端的標籤貼所指向的版本。

## 上傳(git push)

公式

```bash
git push -u 遠端簡稱 要上傳的本機標籤貼所在分支:要選擇的遠端伺服器標籤貼
```

範例

```bash
git push -u origin master:master
```

### 代碼解釋

將master標籤貼所在的分支內容推向(上傳給)origin這個位置，且如果origin上原本沒有master這個標籤貼，則會新創一個新的遠端的master標籤貼。 

`-u` 為某條分支設定[upstream](https://gitbook.tw/chapters/github/push-to-github.html)的意思，設定預設的[本機標籤貼:遠端標籤貼]對應，設定一次之後下次可以直接

```bash
git push
```

git 就會自動判斷你要將master這條支線推上去後要以哪個遠端標籤貼做對應。

`origin`⇒當初設定遠端位置的時候給的遠端位置簡稱。 

`master`⇒選擇要推送的本機標籤貼所在分支。 

`:master`⇒要代替本機標籤貼的遠端的標籤貼名稱，如不存在該標籤貼則建立新的遠端標籤貼，

如果要設定的遠端分支的標籤貼與本機的標籤貼名稱一樣，則可以簡寫成

```bash
git push -u origin 要推的本機分支名稱
```

git flow

```bash
git push origin master
```

![push.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/push.png)

從git flow中有沒有注意到，Server與Local中間還存在了一個時空夾縫，

當你執行git的遠端操作指令時，git就會幫你在你的本機端劃出一個這樣子的空間，

不管從本機上傳到遠端或是從遠端下載到本機，都會經過這個空間，

這個空間主要的目的是將遠端操作當下的本地或遠端的最新版本、分支狀態都一併的移到或複製一份到這裡來，

但要注意的是，遠端指令操作完，這個空間的時間就凍結了，`所以雖然之後你依然可以對留在這個空間內的版本做操作，但新產生的版本是無法放在這個空間的。`

那這個空間的時間甚麼時候會再次流動呢?

就是你下一次執行遠端操作的時候! 

> Note:使用上傳指令push是將本機的最新狀態移到時空夾縫中，
使用下載指令fetch則是將遠端的最新狀態複製到時空夾縫中。
> 

> Note: push到遠端，因為本機的標籤貼是不能貼到遠端的，所以會需要產生一張新的標籤貼在遠端與時空夾缝中代替本機端的標籤貼(圖示中的粉紅色標籤貼)，這也是為什麼我們指令的規則長那樣，如果上述的push指令改這樣
> 

```bash
git push origin master:main
```

則git flow長這樣

![RemoteMasterToMain.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/RemoteMasterToMain.png)

## 而為什麼會有一個時空夾縫存在呢?

`主要是為了避免再多人協作的時候造成版本不統一的情況`(通常會上傳到遠端都會有多人協作的需求) 

讓我們延續上圖繼續說下去，假設距離上次push到遠端後，你在本機修又改了好幾個版本，終於要再次上傳遠端了 

> 要注意!由於push結束後，時空夾縫的時間就凍結了，之後commit的新版本是不會放在時空夾縫的喔~
> 

git flow 

![LocalUpdate.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/LocalUpdate.png)

當你下了push指令後，git並不會立即幫你把Local端的備份資料上傳，

而是會先去開啟時空夾縫，並且將當初留下的狀態與目前遠端上的資料比對一下，

結果發現，欸~有人比你先更新了遠端的資料了欸!為了避免你沒follow到，所以你不準上傳你的檔案! 

git flow 

![PushNG.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/PushNG.png)

> 如果這時候強制要上傳，則由於你本機端的C1後面接的是C2，但遠端C1後面接的是A2，
則強制上傳後C2就會將A2的版本覆蓋掉喔!
> 

> 這邊可以看出為什麼git要讓時空夾縫的時間凍結了，
因為git要保留前一次上傳或下載的狀態與遠端的最新版本做比較啦!
> 

那這時候我們應該怎麼辦? `git fetch`指令就派上用場啦! 

## 下載(git fetch)

公式

```bash
git fetch 伺服器簡稱 伺服器上的標籤:本機上的標籤
```

範例

```bash
git fetch origin main
```

### 代碼解釋

`git fetch`⇒如果完全不加後面的參數，會將遠端上的所有分支、標籤貼狀態都下載到你的時空夾縫中 

`origin`⇒當初設定的伺服器簡稱 

`main`⇒選擇要下載的伺服器標籤所在分支，並且選擇一個本機上的標籤來貼在遠端標籤貼原本指向的版本，由於此處選擇的是當初在時空夾縫新生成並用來代表遠端上的同名標籤貼，故由`main:origin/main`簡化成`main` 

git flow 

![fetch.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/fetch.png)

這時候你會發現，時空夾縫中的分支與本地分支就像在兩條平行線，版本互相沒有交集，你該怎麼辦?

別忘了我們有一個本機端的指令merge，只要這樣

```bash
git checkout master
git merge origin/main
```

### 代碼解釋

`git checkout mater`⇒選擇本地master標籤貼和其版本 

`git merge origin/main`⇒merge遠端標籤貼在的版本，遠端的標籤貼名稱需在前面加上當初取的伺服器簡稱(origin)

git flow

![fetchMerge.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/fetchMerge.png)

> 一樣的，由於fetch結束後，時空夾縫的時間就凍結了，
merge過後的新版本是不能放在時空夾縫中的!
> 

### rebase版本

merge與rebase的差異請見

[rebase vs merge](https://www.notion.so/rebase-vs-merge-9be5c099abd2433890802748d7e554a3)

![fetchRebase.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/fetchRebase.png)

如果這時候又有人新增資料到遠端上的版本，

則 git flow 

![MoreRemote.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/MoreRemote.png)

可以看到，時空夾縫與遠端的狀態又不一致了，

所以需要再次更新你的時空夾縫狀態才能將資料上傳了， 

甚麼?你嫌fetch完又要merge太慢? 那麼你一定要知道`git pull`這個指令了

## 下載(git pull)

公式

```bash
git pull 伺服器簡稱 伺服器上的標籤:本機上的標籤
```

範例

```bash
git checkout master
git pull origin main
```

### 代碼解釋

`git checkout master`⇒選擇等等要與遠端合併的本機標籤貼所指向的版本。 

`git pull`⇒就是將`git fetch`+`git merge`合併為一個步驟做完。 

> 如果完全不加後面的參數，會將目前所選擇的本機標籤貼的上游(上游在push有解釋過)，複製到時空夾縫中，
並與當下所選擇的本機標籤貼版本合併。
> 

`origin`⇒當初設定的伺服器簡稱 

`main`⇒選擇要下載的伺服器標籤所在分支，並且選擇一個本機上的標籤來貼在遠端標籤貼原本指向的版本，

由於此處選擇的是當初在時空夾縫新生成並用來代表遠端上的同名標籤貼，故由`main:origin/main`簡化成`main`，

最後要記得，merge是將你目前所選擇的標籤貼貼到最新的版本上，因此是將master標籤貼貼到最新的版本上(還記得在pull前我們先用`checkout master`選擇了master標籤貼嗎?)。 

git flow

![Pull.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/Pull.png)

### rebase版本

公式

```bash
git pull 伺服器簡稱 伺服器上的標籤:本機上的標籤 --rebase
```

範例

```bash
git checkout master
git pull origin main --rebase
```

### 代碼解釋

`-rebase`⇒代表我不要使用merge而是使用rebase的方式合併 

git flow

![Pull--rebase.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/Pull--rebase.png)

最後，終於沒有人在我們上傳前去更新遠端的版本了，所以我們再次使用push上傳

```bash
git push origin master:main
```

git flow 

![gitPushMerge版.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/gitPushMerge%E7%89%88.png)

rebase版本 

![gitPushRebase.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/gitPushRebase.png)

# Pull Request

參考資料: [如何設定原作的遠端專案](https://gitbook.tw/chapters/github/syncing-a-fork.html)

Pull Request

請求(Request)原專案主人拉回(Pull)你改過的專案至原專案。 

是一個使用GitHub flow開發流程會使用到的一個功能，所以這邊稍微介紹一下如何使用Pull Request。

首先，由於主要的專案上，為了避免沒有審核就讓任何人修改專案的備份版本，所以不允許你直接將修改過的版本上傳過去。 

因此，你需要先使用[`fork`](https://gitbook.tw/chapters/github/pull-request.html)方式從主專案的GitHub帳號複製一份專案到你自己的GitHub帳號內，這樣你之後對於這個專案才有`要求修改的權限`，

`也就是說這樣之後你才能用你的GitHub帳號對這個專案發送`Pull Request。 

> fork原意是叉子的意思，仔細看看git版本的分支是否就長得像叉子的形狀呢? 
也可以這樣理解，fork就是將別人的專案叉一分到自己的GitHub上作使用。
> 

fork完且從自己的GitHub帳號將專案clone到自己的電腦後，

git flow長這樣 

![Fork.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/Fork.png)

但記得我們之前說時空夾縫的用途是甚麼嗎?

是為了避免遠端的版本如果有更新了，你的本機端沒follow到，但看看你的遠端(GitHub帳號)，有人會更新版本到你fork過來的專案嗎?答案是沒有人…

只有原專案的GitHub帳號才會有人更新版本上去，

因此時空夾縫應該存在於你的本機與原專案的GitHub帳號之間，

為此我們稍微修改一下git flow 

![AfterFork.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/AfterFork.png)

> 實際上你的本機與你的GitHub fork過來的專案之間還是存在時空夾縫的，只是由於你的遠端與時空夾縫的專案版本只要不存在差異，你是永遠感受不到它的存在。
> 

由於目前你與你的遠端不存在時空夾縫，因此只要你有備份的版本新增，都可以任意的push專案到你的GitHub上。 

那甚麼時候才會感受到時空夾縫的存在呢? 

就是當你覺得你這個版本真的不錯，想請求原專案的主人將它merge回至原專案的時候，

當我們從自己的GitHub上按下Pull Request按鈕時，

git就會從時空夾縫中，將時間停留於當初fork過來的時間點的版本與原專案目前的版本作比較。

git flow 

![Pull Request.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/Pull_Request.png)

這時候如果發現版本不一致了怎麼辦? 

從前面所學，你知道需要先更新原專案最新的版本到時空夾縫中，因此我們需要在你的本機上再新增一個遠端位置，而這個遠端位置就是原專案的GitHub帳號位置

```bash
git remote add sourceRemote https://github.com/...
```

### 代碼解釋

在當前git控管的資料夾上新增遠端伺服器的位置 

`sourceRemote`⇒可任意取名的遠端伺服器簡稱，代表的是後面的url，因為代表的是原專案位置，所以這邊我們取名稱為sourceRemote。

`https://github.com/...`⇒此處填的是原專案的遠端位置。

可以使用`git remote -v`查看目前有幾個遠端位置

```bash
git remote -v
sourceRemote    https://github.com/... (fetch)
sourceRemote    https://github.com/... (push)
origin  https://github.com/... (fetch)
origin  https://github.com/... (push)
```

### 代碼解釋

`origin`⇒此遠端位置是當初從你的GitHub帳號clone專案下來的時候git自動幫你設置的簡稱，代表的位置是你的GitHub帳號。 

`sourceRemote`⇒我們剛剛設置的遠端位置的簡稱，代表的是原專案遠端位置。

最後與遠端教學介紹的那樣，再將最新的版本下載下來與本機的分支作連結(pull)，

```bash
git pull sourceRemote main
```

git flow 

![PullRemote.png](Git%20b279ad6cc87143ffad4bbc8cf8078934/PullRemote.png)

就可以再push回自己的GitHub帳號，並且再發送Pull Request即可。

而原專案作者如果同意這次的改動， 在自己帳號的這個專案Pull Request頁面點擊Merged按鈕即可收下，由於網路上已經有很多人把這個流程介紹得很清楚了，此處就不再細講。

# 貼紙不是越多越好

貼紙太多不容易記，因此產生了開發流程 

Git flow 

[GitHub flow](https://awdr74100.github.io/2020-05-11-git-githubflow/)

# 重點回顧

- 在重要的備份版本上留下一張貼紙吧!
- 進行git操作前，先確認是否選擇了一張你要的貼紙後(branch)，再進行操作。
- 貼紙只能在同一條分支上使用，靠merge、rebase將分支合併在一起吧!
- 上傳(push)遠端失敗代表你需要先更新(pull)一下你的時空夾縫!

# 其他參考資料:

[Git & GitHub 教學手冊](https://w3c.hexschool.com/git/cfdbd310)

[用GitHub架網站: ](https://www.notion.so/GitHub-0a3c70c518e446e7a6dba33c03289466)