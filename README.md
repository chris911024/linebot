# 詐欺ㄟ之食物機器人
在進行實作前，先來看一下LINE Bot主要的執行架構
![image](https://user-images.githubusercontent.com/67829896/192131030-d5436b59-df1f-469e-a4a5-7093a8af48a0.png)
使用者透過LINE發送訊息時，LINE Platform將會進行接收，並且傳遞至我們所開發的LINE Bot執行邏輯運算後，透過LINE所提供的Messaging API回應訊息給LINE Platform，最後再將訊息傳遞給使用者。

其中Messaging API(Application Programming Interface)，就是LINE官方定義的回應訊息標準介面，包含Text(文字)、Sticker(貼圖)、Video(影片)、Audio(聲音)及Template(樣板)訊息等，完整的說明可以參考LINE的官方文件。(https://developers.line.biz/en/docs/messaging-api/overview/#%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%A9%9F)
# 執行架構
我就先以最基本的使用者發送什麼訊息，LINE Bot就回應什麼訊。架構如下
![image](https://user-images.githubusercontent.com/67829896/192131092-8e815f74-147f-4492-9993-652bb16c9204.png)
# 操作步驟
。建立Provider
。建立Messaging API channel
。設定LINE Bot憑證
。開發LINE Bot應用程式
。安裝Ngrok
。設定LINE Webhook URL
# 建立Provider
進入https://developers.line.biz/zh-hant/
![image](https://user-images.githubusercontent.com/67829896/192131133-fa87a875-5c99-4c8a-8325-2d1060bf1c4e.png)

完成帳戶的建立，就會看到開發者控制台
![image](https://user-images.githubusercontent.com/67829896/192131161-28e48320-962e-4155-afda-622852e8db71.png)

點擊「Create a new provider」，在按鈕下方就會出現輸入框
![image](https://user-images.githubusercontent.com/67829896/192131181-4ff5aa88-4b0d-47df-85ec-30bcf0733c1c.png)


# 建立Messaging API channel

![image](https://user-images.githubusercontent.com/67829896/192131202-20ba9ce2-d870-4914-a1be-c4d5a35b2c7e.png)

![image](https://user-images.githubusercontent.com/67829896/192131227-61af47c9-e1af-4099-a673-2e0e5509bb4d.png)

將這個頻道(Channel)與自己開發的應用程式連結，所以其中有兩個憑證會使用到

Channel secret(頻道密碼)：位於Basic settings頁籤中
![image](https://user-images.githubusercontent.com/67829896/192131249-a64d2e91-bfb6-41a5-b405-7f819ad15f95.png)

Channel access token(頻道憑證)：
![image](https://user-images.githubusercontent.com/67829896/192131267-f42d3bbb-da09-4aa1-884d-849f74b608c5.png)


# 設定LINE Bot憑證

先建立虛擬環境！這點很重要！執行任何程式都要先建置虛擬環境
pip install virtualenv
virtualenv venv
python -m venv venv
venv\Scripts\activate
pip freeze > requirements.txt
pip install -r requirements.txt
# 接著django建置
django-admin startproject mylinebot .  建立Django專案 
python manage.py startapp foodlinebot  建立Django應用程式 
python manage.py migrate  執行資料遷移(Migration)


# 專案架構


<img width="195" alt="image" src="https://user-images.githubusercontent.com/67829896/192131353-92c8e423-a71b-44d8-b43a-1fdd128790fa.png">


開啟專案主程式下的settings.py檔案，增加LINE Developers上所取得的兩個憑證設定，來與LINE頻道(Channel)進行連結


<img width="655" alt="image" src="https://user-images.githubusercontent.com/67829896/192131389-c9ff0bd8-7e2e-4b76-a475-d80c2c68136a.png">

在INSTALL_APPS的地方，加上剛剛所建立的Django應用程式(APP)


<img width="652" alt="image" src="https://user-images.githubusercontent.com/67829896/192131419-264f9804-8c07-40ed-9da3-be9497753a15.png"
－

開發LINE Bot應用程式
開啟Django應用程式(APP)的views.py檔案，這邊就是撰寫LINE Bot接收訊息後，所要執行的運算邏輯，這邊先以使用者發送什麼訊息，就回覆什麼訊息

     
 <img width="659" alt="image" src="https://user-images.githubusercontent.com/67829896/192131491-befd62d7-d43d-461e-997c-d62039074aed.png">


<img width="668" alt="image" src="https://user-images.githubusercontent.com/67829896/192131504-b7668700-a681-442f-95b0-5dc9650b25f0.png">

11、12行為取得settings.py中的LINE Bot憑證來進行Messaging API的驗證
在callback檢視函式中，當偵測到使用者有傳入的事件，就會透過Python迴圈進行讀取(第28行)，如果其中有訊息事件(第29行)，則回覆使用者所傳入的文字(第30~33行)。

Django應用程式(APP)下建立一個urls.py檔案
<img width="685" alt="image" src="https://user-images.githubusercontent.com/67829896/192131600-4bbb1e18-5d3c-4398-9b48-5324400728bf.png">


為了要將這個Django應用程式(APP)的網址也加入到專案主程式中，所以，在Django專案主程式下的urls.py檔案中，加入以下的網址設定:
<img width="671" alt="image" src="https://user-images.githubusercontent.com/67829896/192131614-243cbf5a-e48b-4a52-b2a9-5da2920df9e2.png">

－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

# 安裝Ngrok（依據作業系統進行下載）
需要輸入以下的指令
 ngrok authtoken <YOUR TOKEN>

 
 Django在本機運行的埠號為8000，所以輸入以下的指令
 ngrok http 8000
<img width="721" alt="image" src="https://user-images.githubusercontent.com/67829896/192131704-ab91e479-9daf-498e-aa15-68ad7d511f14.png">

 
 把這個網址填入LINE Webhook URL
<img width="627" alt="image" src="https://user-images.githubusercontent.com/67829896/192131729-bc9ef997-be64-4695-a565-0cd6b3ad6b0b.png">

 
# 執行LINE Bot
python manage.py runserver

 －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

 # 設定LINE Webhook URL

 Webhook
 
 ![image](https://user-images.githubusercontent.com/67829896/192131765-2670c439-12bf-4cb0-83b3-280660644875.png)

 
 －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

 
 ![image](https://user-images.githubusercontent.com/67829896/192131785-324991ca-86d1-48b3-9593-547e9f871536.png)




