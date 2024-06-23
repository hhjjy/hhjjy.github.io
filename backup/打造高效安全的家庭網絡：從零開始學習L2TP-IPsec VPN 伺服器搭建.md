# 前置作業
1. 設定好pfSense的網絡設置，並確定可以網路連線
    - 確保你的pfSense已安裝並基本配置完畢，包括WAN和LAN接口的設置。確認WAN接口可以正常連接到互聯網，並且LAN接口可以與內網設備正常通信。
2. 確定ISP提供的是公網IP而不是ISP大內網
    - 檢查你的WAN接口是否獲得了公網IP地址。可以通過pfSense的狀態頁面（Status > Interfaces）查看WAN接口的IP地址，並使用線上工具（如WhatIsMyIP）驗證該IP是否為公網IP。
    - 如果你的IP地址屬於私有IP地址範圍（例如10.x.x.x、172.16.x.x-172.31.x.x、192.168.x.x），則可能是ISP提供的是NAT網絡，需要與ISP聯繫以獲取公網IP。
# VPN設定
## IPsec 設定
1. Mobile Client 啟用IKE extension (請按照順序設定否則會出錯)

  ![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/c9ad3dfe-26f5-4eea-a270-38f9df2fb752)

2. 設定預共享密鑰

![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/2ab5610d-44d8-47f7-bf3e-f3fdd60f0a9c)


3. 設定階段1

![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/f25aa04a-51c1-4d4d-beb8-b47fcfde7660)
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/78a569a3-81bf-4745-8bd1-72251cbb9d45)

4. 設定階段2

![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/c04df8ba-8768-4c4b-aa5b-c00fc6b87eb8)




## 2. 配置L2TP

1. L2TP基本設定

![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/b2a46589-7acc-4fd6-bce5-b969155c9cbd)

2. 創建一個用戶

![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/30a7e911-1557-41fd-ad5e-cfe291659d71)



## 配置防火牆
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/2f86ec96-878d-4a58-a6fc-8b1b7e885e72)



## 測試

1. 測試
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/ce77142a-4f45-4644-a543-79e8d141e7de)

2. Debug 
  這邊建議如果要Deubg，可以使用` tail -f /var/log/ppp.log` 這個指令可以幫你檢查在連線的過程中是哪一步驟失敗了，再從那個步驟去軟路由內改到成功。另外如果失敗的原因找不出來，可以用Wireshark把傳送過程的封包擷取並閱讀，大概就能判斷問題在哪。

![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/e05f0b0e-e1bd-4b78-a8fc-a2718cfdaabb)





