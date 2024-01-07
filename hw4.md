<h1>HW4:tableView與各式課堂練習</h1>
<p>作業介紹：
  首先有串接登入、註冊系統(利用json跟userDefault儲存帳號密碼，下次登入app帳密還存在)，登入跟註冊都有防呆跟歷史紀錄比對。
  登入後到達MainView，有淺略介紹TOMICA的歷史跟背景，下列tableView中的introduce會進入列表，每個列表有不同車型盒裝的圖片及名稱，點進去後會有各個車盒介紹。
  tableView中的product會進入圓餅圖，呈現TOMICA截至今日發售的車型盒個數據。
  tableView中的setting會進入各項上課教的設定項目，尤其是appstorage，這個我最印像深刻，也是啟發我想做登入註冊系統的出發點，也在上課及研究中發現ipad跟mac中swiftui的差距及限制，不過appstorage雖然儲存有限且限制多，但還是一個不錯使用的暫存用法。
                                            
</p>

<p>
  ContentView
</p>

  ```swift
  

import SwiftUI

struct ContentView: View {
    @State var LoginSucess = false
    @State var overSucess = false
    var body: some View {
        ZStack{
            if LoginSucess || overSucess{
                MainView()
            }else{
                LoginView(LoginSucess: self.$LoginSucess)
            }
        }
    }
}

  ```
# HW3 Demo圖片
<img width="40%" src="https://raw.githubusercontent.com/FatFang/1101445-swift/main/hw3.jpg">
