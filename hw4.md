<h1>HW4:tableView與各式課堂練習</h1>
<p>作業介紹：
  首先有串接登入、註冊系統(利用json跟userDefault儲存帳號密碼，下次登入app帳密還存在)，登入跟註冊都有防呆跟歷史紀錄比對。
  登入後到達MainView，有淺略介紹TOMICA的歷史跟背景，下列tableView中的introduce會進入列表，每個列表有不同車型盒裝的圖片及名稱，點進去後會有各個車盒介紹。
  tableView中的product會進入圓餅圖，呈現TOMICA截至今日發售的車型盒個數據。
  tableView中的setting會進入各項上課教的設定項目，尤其是appstorage，這個我最印像深刻，也是啟發我想做登入註冊系統的出發點，也在上課及研究中發現ipad跟mac中swiftui的差距及限制，不過appstorage雖然儲存有限且限制多，但還是一個不錯使用的暫存用法。
                                            
</p>

<p>
  以下是所有程式碼
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

<p>
  MainView
</p>

  ```swift
  
import SwiftUI

struct MainView: View {
    var body: some View {
        VStack {
            Text("TOMICA品牌介紹")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundStyle(.primary)
                .background(Color.red)
                .cornerRadius(10.0)
                .padding(.top,20)
            TabView{
                Group{
                    WelcomeView()
                        .tabItem{
                            Image(systemName: "rosette")
                            Text("Home")
                        }
                    CourseListView()
                        .tabItem{
                            Image(systemName: "list.dash")
                            Text("Introduce")
                        }
                    GraphView()
                        .tabItem{
                            Image(systemName: "photo.artframe")
                            Text("Product")
                        }
                    SettingView()
                        .tabItem{
                            Image(systemName: "wrench.and.screwdriver")
                            Text("Product")
                        }
                }
                .toolbarBackground(Color.black, for: .tabBar)
                .toolbarBackground(.visible, for: .tabBar)
            }
            .tint(.yellow)
        }
    }
}

struct FullImageRow: View{
    var thisCourse:Course
    var body: some View{
        ZStack{
            Image(thisCourse.image)
                .resizable()
                .frame(height: 180)
                .aspectRatio(contentMode: .fit)
                .cornerRadius(10)
                .overlay(
                    Rectangle()
                        .foregroundColor(.black)
                        .cornerRadius(10)
                        .opacity(0.2)
                )
            Text(thisCourse.name)
                .font(.system(.title))
                .fontWeight(.black)
                .foregroundColor(.white)
                .offset(x: 0.0, y: 50.0)
        }
    }
}

  ```

<p>
  LoginView
</p>

  ```swift
  
import SwiftUI

struct LoginView: View {
    @Binding var LoginSucess:Bool
    
    @State private var goToSignup = false
    @State private var account: String = ""
    @State private var password: String = ""
    @State private var showingAlert = false
    @State private var alertMessage = ""
    @State private var isLoginSuccessful = false
    @State private var isLogin = ""
    //    let UserData = UserDefaults.standard.array(forKey: "ac2") as? [[String]] ?? []
    var body: some View {
        NavigationView {
            VStack {
                
                TextField("Account", text: $account)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                    .frame(width: 200)
                    .padding()
                
                SecureField("Password", text: $password)
                    .textFieldStyle(RoundedBorderTextFieldStyle())
                    .frame(width: 200)
                    .padding()
                
                Button("Login") {
                    
                    let UserData = UserDefaults.standard.array(forKey: "ac2") as? [[String]] ?? []
                    isLogin = checkLoginData(loginData: loadJsonFile("User_account.json"), enterAccount: account, enterPassword: password,UserDefaultDATA: UserData)
                    print(isLogin)
                    if isLogin == "All"{
                        showingAlert = true
                        alertMessage = "登入成功"
                        self.LoginSucess.toggle()
                    }else if isLogin == "password"{
                        showingAlert = true
                        alertMessage = "密碼錯誤"
                    }else{
                        showingAlert = true
                        alertMessage = "帳號不存在"
                    }
                }
                .alert(isPresented: $showingAlert) {
                    Alert(title: Text("通知"), message: Text(alertMessage), dismissButton: .default(Text("確認")))
                }
                .padding()
                
                Button("sign up"){
                    self.goToSignup = true
                }
                NavigationLink(destination: SignupView(), isActive: $goToSignup) {
                    EmptyView()
                }

            }
            
            
        }
    }
}

struct LoginView_Previews: PreviewProvider {
    static var previews: some View {
        LoginView(LoginSucess: .constant(false))
    }
}

  ```

<p>
  SignupView
</p>

  ```swift
  
import SwiftUI

struct SignupView: View {
    @State private var newAccount: String = ""
    @State private var newPassword: String = ""
    @State private var confirmPassword: String = ""
    @State private var showingAlert = false
    @State private var alertMessage = ""
    @AppStorage("Account") var Account:String = ""
    @AppStorage("password") var Password:String = ""
    //    let data = UserDefaults.standard.stringArray(forKey: "data1") as? [String]
    var body: some View {
        VStack {
            TextField("New Account", text: $newAccount)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()
            
            SecureField("New Password", text: $newPassword)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()
            
            SecureField("Confirm Password", text: $confirmPassword)
                .textFieldStyle(RoundedBorderTextFieldStyle())
                .padding()
            
            
            Button("Register") {
                registerUser()
                //                print(data)
            }
            .alert(isPresented: $showingAlert) {
                Alert(title: Text("通知"), message: Text(alertMessage), dismissButton: .default(Text("確認")))
            }
            .padding()
        }
        .navigationBarTitle("Register")
        //        .navigationBarBackButtonHidden(true)
    }
    
    private func registerUser() {
        // 檢查帳號不為空
        if newAccount.isEmpty {
            alertMessage = "帳號不能為空白"
            showingAlert = true
            return
        }
        
        // 檢查密碼和確認密碼是否一致
        if newPassword != confirmPassword {
            alertMessage = "密碼確認錯誤"
            showingAlert = true
            return
        }
        
        let sucess = saveLoginData(loginDataAccount: newAccount, loginDataPassword: newPassword,loginData: loadJsonFile("User_account.json"))
        if sucess == "noAdd"{
            alertMessage = "帳號已存在"
            showingAlert = true
            return
        }
        Account = newAccount
        Password = newPassword
        alertMessage = "註冊成功！"
        showingAlert = true
    }
}

struct RegisterView_Previews: PreviewProvider {
    static var previews: some View {
        SignupView()
    }
}


  ```

<p>
  JsonUtils
</p>

  ```swift
  
import SwiftUI

func loadJsonFile<T: Decodable>(_ filename: String) -> T {
    let data: Data
    
    guard let file = Bundle.main.url(forResource: filename, withExtension: nil)
    else {
        fatalError("Couldn't find \(filename) in main bundle.")
    }
    
    
    do {
        data = try Data(contentsOf: file)
    } catch {
        fatalError("Couldn't load \(filename) from main bundle:\n\(error)")
    }
    
    do {
        let decoder = JSONDecoder()
        return try decoder.decode(T.self, from: data)
    } catch {
        fatalError("Couldn't parse \(filename) as \(T.self):\n\(error)")
    }
}

  ```

<p>
  AccountData
</p>

  ```swift
  
import SwiftUI
import PlaygroundSupport

struct AccountData: Identifiable,Decodable,Encodable {
    var id: Int
    var account: String
    var password: String
}


func checkLoginData(loginData: [AccountData],enterAccount: String,enterPassword: String,UserDefaultDATA: [[String]]) -> String{
    var result: String = "error"
    for _loginData in loginData{
        if enterAccount == _loginData.account && enterPassword == _loginData.password{
            result = "All"
            break
        }else if enterAccount == _loginData.account && enterPassword != _loginData.password{
            result = "password"
            break
        }else{
            result = "account"
        }
    }
    if result == "All"{
        return result
    }
    
    for d in UserDefaultDATA{
        print(d[0],d[1])
        if enterAccount == d[0] && enterPassword == d[1]{
            result = "All"
            break
        }else if enterAccount == d[0] && enterPassword != d[1]{
            result = "password"
            break
        }else{
            result = "account"
        }
    }
    return result
}


func saveLoginData(loginDataAccount: String,loginDataPassword: String,loginData: [AccountData]) -> String{
    var result: String = "add"
    for _loginData in loginData{
        if loginDataAccount == _loginData.account {
            result = "noAdd"
            break
        }
    }
    if result == "noAdd"{
        return result
    }
    let UserDefaultData = UserDefaults.standard.array(forKey: "ac2") as? [[String]] ?? []
    var dataArray = [[String]]()
    print(loginDataAccount)
    for ud in UserDefaultData{
        dataArray.append(ud)
    }    
    dataArray.append([loginDataAccount,loginDataPassword])
    UserDefaults.standard.set(dataArray,forKey: "ac2")
    let d = UserDefaults.standard.array(forKey: "ac2") as? [[String]] ?? []
    print(d)
    print("-----")
    return result
}

  ```

<p>
  SettingView
</p>

  ```swift
  
import SwiftUI

struct SettingView: View {
    let displayFontType = [".default",".rounded",".monospaced",".serif"];
    @State var displayFontSelected = 0
    @State var IsDeepScheme = false 
    @State var colorArray:Array = [255.0,255.0,255.0]
    @State var stepperValue = 0
    @State var sliderValue = 0.0
    @State var selectedDate = Date()
    @AppStorage("UserName") var UserName:String = ""
    var body: some View {
        NavigationView{
            Form( content: {
                Section(content:{
                    TextField("Enter your name",text: $UserName)
                }, header: {
                    Text("UserName")
                })
                Section(header: Text("字型設定")){
                    Picker(selection:$displayFontSelected,
                           label:Text("字型選擇(\(displayFontSelected))")){
                        ForEach(0..<displayFontType.count,id:\.self){
                            Text(self.displayFontType[$0])
                        }
                    }
                }
                Section(header: Text("背景風格")){
                    Toggle(isOn:$IsDeepScheme){
                        Text("深色(\(String(IsDeepScheme)))")
                    }
                }
                Section(header: Text("計數器")){
                    Stepper("Stepper(\(stepperValue))",
                            onIncrement: {stepperValue+=1},
                            onDecrement: {stepperValue-=1})
                }
                Section(header: Text("滑桿(\(sliderValue,specifier:"%.2f")")){
                    Slider(value: $sliderValue, in:0...1)
                }
                Section(header: Text("日期")) { 
                    DatePicker(formatDate(selectedDate), selection: $selectedDate, in: ...Date(), displayedComponents: .date)
                }
            })
        }
    }
}
func formatDate(_ date: Date) -> String {
    let dateFormatter = DateFormatter()
    dateFormatter.dateFormat = "YYYY-MM-dd"
    return dateFormatter.string(from: date)
}

  ```

<p>
  CourseListView
</p>

  ```swift
  
import SwiftUI

struct Course:Identifiable{
    var id = UUID()
    var name:String
    var image:String
    var description:String
    var isFeature:Bool
}

var courses = [
    Course(name: "tomica基本介紹", image: "TomicaLogo2", description: "TOMICA自1970年開始於市場上發售，由最初僅有6款的汽車模型，發展至今已推出數千款以上的小汽車種類並擁有眾多系列，產品行銷世界30餘國；品牌成立50多年來，全球累積銷售超過6億7千萬台，其魅力可說是深入每個家庭",isFeature: false),
    Course(name: "一般盒車型", image: "RedCar", description: "一般盒車型的價格範圍在150～200NT，所有的車輛的輪子統一，車型以現代新車為主，是全盒種中最便宜的。",isFeature: true),
    Course(name: "黑紅盒車型", image: "BlackCar", description: "紅黑盒車型的價格範圍在250～300NT，車子主要以復古車為主，且車輛的輪子每台都不同，是依照車輛真實的來模擬，是成為Tomica收藏者的入門款",isFeature: true),
    Course(name: "無極限車型", image: "Unlimited", description: "無極限車型價格範圍在350～400NT，車子主要以電影或是歷史名車為主，包裝使用吊卡方式來呈現，相當有質感",isFeature: true),
    Course(name: "載運車系列", image: "Big", description: "載運車的價格是695NT",isFeature: true)
]

struct BasicImageRow: View{
    var thisCourse:Course 
    var body: some View{
        HStack{
            Image(thisCourse.image)
                .resizable()
                .frame(width: 40, height: 40)
                .cornerRadius(5)
            Text(thisCourse.name)
        }
    }
}

struct CourseDetailView:View{
    @Environment(\.presentationMode) var presentationMode 
    var thisCourse:Course
    var body: some View{
        ScrollView{
            VStack{
                Image(thisCourse.image)
                    .resizable()
                    .aspectRatio(contentMode: .fill)
                    .clipped()
                Text(thisCourse.name)
                    .font(.system(.title, design:.rounded))
                    .fontWeight(.black)
                Spacer()
                Text(thisCourse.description)
                    .font(.system(.subheadline, design:.rounded))
                    .fontWeight(.light)
                Spacer()
            }
        }
        .overlay(
            HStack{
                Spacer()
                VStack{
                    Button(action:{
                        self.presentationMode.wrappedValue.dismiss()
                    },label:{
                        Image(systemName:"chevron.down.circle.fill")
                            .font(.largeTitle)
                            .foregroundColor(.white)
                    })
                    .padding(.trailing, 20)
                    .padding(.top, 40)
                    Spacer()
                }
            }
        )
    }
}
struct CourseListView: View{
    @State var showDetailView = false
    @State var selectedCourse:Course?
    var body: some View{
        NavigationView{
            List(courses){ courseItem in
                if courseItem.isFeature{
                    FullImageRow(thisCourse: courseItem)
                        .onTapGesture{
                            self.showDetailView = true
                            self.selectedCourse = courseItem
                        }
                }else{
                    BasicImageRow(thisCourse: courseItem)
                        .onTapGesture{
                            self.showDetailView = true
                            self.selectedCourse = courseItem
                        }
                }
            }
            .sheet(item: self.$selectedCourse){ thisCourse in
                CourseDetailView(thisCourse: thisCourse)
            }
            .navigationBarTitle("介紹列表")
        }
    }
}

  ```

<p>
  WelcomeView
</p>

  ```swift
  
import SwiftUI

struct WelcomeView: View{
    @AppStorage("UserName") var UserName:String = ""
    var body: some View{
        VStack{
            Image("TomicaLogo")
                .resizable()
                .aspectRatio(contentMode: .fit)
                .cornerRadius(50.0)
            Text("歷經近百年依舊屹立不搖的汽車模型品牌")
                .fontWeight(.heavy)
                .lineSpacing(20)
                .font(.system(size: 32.0))
                .foregroundColor(.white)
                .frame(width: 350, height: 150, alignment: .center)
                .background(Color.red)
                .cornerRadius(20.0)
                .multilineTextAlignment(.center)
            Text("User: " + UserName)
        }
    }
}

  ```

<p>
  GraphView
</p>

  ```swift
  
import SwiftUI

struct GraphView: View {
    var body: some View {
        VStack{
            Text("TOMICA個車型比例圖")
                .font(.largeTitle)
                .fontWeight(.heavy)
                .foregroundStyle(.primary)
                .cornerRadius(10.0)
                .padding(.top,20)
            ZStack{
                Path{ path in
                    path.move(to: CGPoint(x: 200, y: 200))
                    path.addArc(center: .init(x: 200, y: 200), radius: 150,
                                startAngle: Angle(degrees: 0.0), endAngle: Angle(degrees: 170),
                                clockwise: false)
                }
                .fill(Color(.systemYellow))
                //            .offset(x: 10.0,y: 10.0)
                .overlay(
                    Text("一般盒")
                        .font(.system(size: 30))
                        .bold()
                        .offset(x: 40,y: 45)
                )
                Path{ path in
                    path.move(to: CGPoint(x: 200, y: 200))
                    path.addArc(center: .init(x: 200, y: 200), radius: 150,
                                startAngle: Angle(degrees: 140), endAngle: Angle(degrees: 200),
                                clockwise: false)
                }
                .fill(Color(.systemTeal))
                .overlay(
                    Text("載運車")
                        .font(.system(size: 30))
                        .bold()
                        .offset(x: -130,y: 60)
                )
                Path{ path in
                    path.move(to: CGPoint(x: 200, y: 200))
                    path.addArc(center: .init(x: 200, y: 200), radius: 150,
                                startAngle: Angle(degrees: 150), endAngle: Angle(degrees: 250),
                                clockwise: false)
                }
                .fill(Color(.systemRed))
                .overlay(
                    Text("紅黑盒")
                        .font(.system(size: 30))
                        .bold()
                        .offset(x: -75,y: -60)
                )
                Path{ path in
                    path.move(to: CGPoint(x: 200, y: 200))
                    path.addArc(center: .init(x: 200, y: 200), radius: 150,
                                startAngle: Angle(degrees: 250), endAngle: Angle(degrees: 360),
                                clockwise: false)
                }
                .fill(Color(.systemPurple))
                .overlay(
                    Text("無極限")
                        .font(.system(size: 30))
                        .bold()
                        .offset(x: 60,y: -90)
                )
            }
        }
        
        
    }
}

  ```
