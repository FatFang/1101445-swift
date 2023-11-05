<h1>HW3:個人收藏櫃</h1>
<p>收藏櫃大略介紹：有最近買的樂高、一直以來都在收藏的tomica汽車、最近看的漫畫書、很愛的鞋子</p>

  ```swift
  
import SwiftUI

struct ContentView: View {
    var body: some View {
        ZStack{
            Image("Back")
            VStack {
                TitleView()
                HStack {
                    ZStack{
                        collect1View(imageName: "姬路城")
                        Text("售價：4000")
                            .font(.system(size: 15))
                            .foregroundColor(.yellow)
                            .padding(.all, 5)
                            .background(Color.black)
                            .opacity(0.7)
                    }
                    ZStack{
                        collect1View(imageName: "樂高Car")
                        Text("售價：5200")
                            .font(.system(size: 15))
                            .foregroundColor(.yellow)
                            .padding(.all, 5)
                            .background(Color.black)
                            .opacity(0.7)
                    }
                    ZStack{
                        collect1View(imageName: "Tomica")
                        Text("平均售價：105-500")
                            .font(.system(size: 15))
                            .foregroundColor(.yellow)
                            .padding(.all, 5)
                            .background(Color.black)
                            .opacity(0.7)
                    }
                    
                    
                }
                ZStack {
                    bookView()
                    Text("目前正在連載中")
                        .font(.system(size: 20))
                        .foregroundColor(.gray)
                        .padding(.all, 5)
                        .background(Color.black)
                        .opacity(0.7)
                }
                HStack {
                    ZStack{
                        collect2View(imageName: "NIKE-Dunk")
                        
                        Text("最常穿的鞋子")
                            .font(.system(size: 15))
                            .foregroundColor(.yellow)
                            .padding(.all, 5)
                            .background(Color.gray)
                            .opacity(0.7)
                    }
                    ZStack{
                        collect2View(imageName: "NB990")
                        Text("買過最貴的")
                            .font(.system(size: 15))
                            .foregroundColor(.yellow)
                            .padding(.all, 5)
                            .background(Color.gray)
                            .opacity(0.7)
                    }
                    ZStack{
                        collect2View(imageName: "NB550")
                        Text("想買的")
                            .font(.system(size: 15))
                            .foregroundColor(.yellow)
                            .padding(.all, 5)
                            .background(Color.gray)
                            .opacity(0.7)
                    }
                }
            }
        }
    }
}

struct TitleView: View {
    var body: some View {
        VStack(alignment: .center, spacing: 2) {
            Text("Hunter's")
                .font(.system(size: 30))
            Text("收藏櫃")
                .font(.title)
                .foregroundColor(Color.yellow)
        }
    }
}
struct collect1View: View {
    var imageName: String
    var body: some View {
        VStack {
            Image(imageName)
                .resizable()
                .aspectRatio(contentMode: .fit)
                .frame(width: /*@START_MENU_TOKEN@*/100/*@END_MENU_TOKEN@*/,height: 200, alignment: .center)
                .padding(.bottom, -50)
            Text(imageName.capitalized)
                .fontWeight(.bold)
                .font(.system(size: 25))
        }
        .frame(minWidth: 0, idealWidth: 100, maxWidth: .infinity, minHeight: 0, idealHeight: 100, maxHeight: .infinity, alignment: .center)
        .padding(.bottom, 80)
    }
}
struct collect2View: View {
    var imageName: String
    var body: some View {
        VStack {
            Image(imageName)
                .resizable()
                .aspectRatio(contentMode: .fit)
                .frame(width: 100,height: 200, alignment: .center)
                .padding(.bottom, -60)
            Text(imageName.capitalized)
                .fontWeight(.bold)
                .font(.system(size: 20))
        }
        .frame(minWidth: 0, idealWidth: 100, maxWidth: .infinity, minHeight: 0, idealHeight: 100, maxHeight: .infinity, alignment: .center)
        .padding(.bottom, 0)
//        .padding(.top,60)
    }
}
struct bookView: View {
    var body: some View {
        VStack {
            Image("Book")
                .resizable()
                .aspectRatio(contentMode: .fit)
                .padding(.all, 15)
            Text("不死不運漫畫書")
                .fontWeight(.bold)
                .font(.system(size: 20))
        }
        .frame(minWidth: 0, idealWidth: 200, maxWidth: .infinity, minHeight: 0, idealHeight: 200, maxHeight: .infinity, alignment: .center)
        .padding(.bottom,-20)
    }
}

  ```
# HW3 Demo圖片
<img width="40%" src="https://raw.githubusercontent.com/FatFang/1101445-swift/main/hw1.jpg">
