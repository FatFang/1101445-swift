<h1>HW2:剪刀石頭布遊戲</h1>
<p>功能概述：</p>
<p> (1)玩家可自行選擇出的拳(剪刀、石頭、布)，且玩家選擇後不的更改，也就是防呆，若按其他選擇則不影響</p>
<p> (2)對手是電腦，與玩家PK，電腦會隨機時間停止且隨機選擇出的拳，模擬對手</p>
<p> (3)中間有提示字，告訴玩家目前場次的狀況</p>
<p> (4)有計分板，計算玩家與電腦的分數，平手不算分</p>
<p> (5)若先達到3分獲勝，且會有彈跳視窗告訴玩家是贏是輸，然後重新遊戲</p>

# HW2 Demo影片的 [通道](https://youtu.be/qv4Kt2t4g7U?si=5AdgHxw54POoOeEI)

  ```swift
  
import SwiftUI
import Combine

var SciBri:Double = 0
var RockBri:Double = 0
var ClothBri:Double = 0
var ESciBri:Double = 0
var ERockBri:Double = 0
var EClothBri:Double = 0
var checkMychoice = true
var timecount = 0
var MyChoice:Int = 0
var EnemyChoice:Int = 0
var WhoWin:Int = 0
var WhoWinTmp:Int = 0
var opt = 1
var enterCount = 0
var userChoseIf = false
var ESocre = 0
var MyScore = 0
var finalWin = 0
var textInsex = 0
var finalText = "" 
struct ContentView: View {
    @State var showAlert = false
    @State private var currentImageIndex = 0
    let images = ["Sci", "Cloth", "Rock"] 
//    let whoWins = ["START","WIN","TIE","LOSE"]
    let RPS = ["Wait for choice","Scissors","Paper","Rock"]
    var body: some View {
        ZStack {
            Color.yellow.edgesIgnoringSafeArea(.all)
            VStack{
                HStack {
                    Text("對手分數：")
                        .font(.system(size: 40))
                        .opacity(Double(opt))
                        .foregroundColor(.black)
                    Text(String(ESocre))
                        .font(.system(size: 40))
                        .opacity(Double(opt))
                        .foregroundColor(.red)
                }
                HStack {
                    Image(images[currentImageIndex]) //大頭照圖片
                        .resizable()
                        .brightness(0) 
                        .aspectRatio(contentMode: .fill)
                        .frame(width: 130, height: 130)
                        .clipShape(Circle())
                        .padding(.bottom,5)
                        .onAppear {
                            startTimer()
                        }
                }
                
//                Text(RPS[EnemyChoice]).font(.system(size:50)).padding(.bottom,-30).padding(.top,-5)
//                Text(whoWins[WhoWin]).font(.system(size:100)) 
                Text(whoWinsText(for: WhoWin))
                    .font(.system(size: 60))
                    .opacity(Double(opt))
                    .foregroundColor(.purple)
//                Text(RPS[MyChoice]).font(.system(size:50)).padding(.bottom,-5)
                HStack {
                    Button(action: {
                        if !userChoseIf{
                            userChoseIf = true
                            MyChoice = 1
                            RockBri = 0
                            SciBri = -0.4
                            ClothBri = 0
                            //                        delay = Int.random(in: 1...3)
                            let delay = Int.random(in: 1...3)
                            print(delay)
                            let deadline = DispatchTime.now() + .seconds(delay)
                            DispatchQueue.main.asyncAfter(deadline: deadline) {
                                checkMychoice = false
                            }
                        }
                    }, label: {
                        Image("Sci") //大頭照圖片
                            .resizable()
                            .brightness(SciBri) 
                            .aspectRatio(contentMode: .fill)
                            .frame(width: 100, height: 100)
                            .clipShape(Circle())
                            .padding(.bottom,5)
                        
                    })
                    Button(action: {
                        if !userChoseIf{
                            userChoseIf = true
                            MyChoice = 2
                            RockBri = 0
                            SciBri = 0
                            ClothBri = -0.4
                            let delay = Int.random(in: 1...3)
                            print(delay)
                            let deadline = DispatchTime.now() + .seconds(delay)
                            DispatchQueue.main.asyncAfter(deadline: deadline) {
                                checkMychoice = false
                            }
                        }
                    }, label: {
                        Image("Cloth") //大頭照圖片
                            .resizable()
                            .brightness(ClothBri) 
                            .aspectRatio(contentMode: .fill)
                            .frame(width: 100, height: 100)
                            .clipShape(Circle())
                            .padding(.bottom,5)
                    })
                    Button(action: {
                        if !userChoseIf{
                            userChoseIf = true
                            MyChoice = 3
                            RockBri = -0.4
                            SciBri = 0
                            ClothBri = 0
                            let delay = Int.random(in: 1...3)
                            print(delay)
                            let deadline = DispatchTime.now() + .seconds(delay)
                            DispatchQueue.main.asyncAfter(deadline: deadline) {
                                checkMychoice = false
                            }
                        }
                    }, label: {
                        Image("Rock") //大頭照圖片
                            .resizable()
                            .brightness(RockBri) 
                            .aspectRatio(contentMode: .fill)
                            .frame(width: 100, height: 100)
                            .clipShape(Circle())
                            .padding(.bottom,5)
                    })
                }
                HStack {
                    Text("你的分數：")
                        .font(.system(size: 40))
                        .opacity(Double(opt))
                        .foregroundColor(.black)
                    Text(String(MyScore))
                        .font(.system(size: 40))
                        .opacity(Double(opt))
                        .foregroundColor(.red)
                }
            }
        }
        .onReceive(Just(finalWin)) { newValue in
            if newValue != 0 {
                showAlert = true
            }
        }
        .alert(isPresented: $showAlert) {
            Alert(title: Text(finalText), dismissButton: .default(Text("了解")){
                finalWin = 0
                showAlert = false
            })
        }
    }
    func startTimer() {
        Timer.scheduledTimer(withTimeInterval: 0.08, repeats: true) { timer in
            if checkMychoice == true || enterCount == 0{
                currentImageIndex = (currentImageIndex + 1) % images.count
                EnemyChoice = currentImageIndex + 1
                if checkMychoice == false{
                    WhoWin = judge(EC: EnemyChoice,MC: MyChoice)
                    enterCount = 1
                }else{
                    if userChoseIf{
                        WhoWin = 4
                    }else{
                        WhoWin = 0
                    }
                }
            }else{
                opt = 1
                print(1)
                timecount += 1
                if timecount == 20{
                    timecount = 0
                    checkMychoice = true
                    RockBri = 0
                    SciBri = 0
                    ClothBri = 0
//                    WhoWin = judge(EC: EnemyChoice,MC: MyChoice)
                    enterCount = 0
                    userChoseIf = false
                    finalText = checkWin()
                    showAlert = false
                }
            }
//            print(WhoWin)
        }
    }
}
func checkWin() -> String{
    let text = ["", "你贏得最後的勝利！", "你居然輸給電腦！"] 
    if ESocre == 3 || MyScore == 3{
        finalWin = 1
    }
    var idx = 0
    if ESocre == 3{
        idx = 2
        ESocre = 0
        MyScore = 0
    }
    if MyScore == 3{
        idx = 1
        ESocre = 0
        MyScore = 0
    }
    return text[idx]
}
func judge(EC:Int,MC:Int) -> Int{
//    print(EC)
//    print(MC)
    if EC == MC{
        return 2
    }else if MC == 1 {
        if EC == 2{
            MyScore += 1
            return 1
        }else if EC == 3{
            ESocre += 1
            return 3
        }
    }else if MC == 2 {
        if EC == 1{
            ESocre += 1
            return 3
        }else if EC == 3{
            MyScore += 1
            return 1
        }
    }else if MC == 3 {
        if EC == 1{
            MyScore += 1
            return 1
        }else if EC == 2{
            ESocre += 1
            return 3
        }
    }
    return 0
}
func modefyEN(EC:Int){
    if EC == 1{
        ERockBri = 0
        ESciBri = -0.4
        EClothBri = 0
    }else if EC == 2{
        ERockBri = 0
        ESciBri = 0
        EClothBri = -0.4
    }else if EC == 3{
        ERockBri = -0.4
        ESciBri = 0
        EClothBri = 0
    }
}
func whoWinsText(for winner: Int) -> String {
    let whoWins = ["請選擇","你贏了","平手","你輸了","等待對手選擇"]
    print(winner)
     return whoWins[winner]
}

  ```
