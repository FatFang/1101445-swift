<h1>HW1</h1>
<table>
 <tr>
   <td>
     <img src="https://raw.githubusercontent.com/FatFang/1101445-swift/main/hw1.jpg">
   </td>
   <td>
   ```swift
    import SwiftUI

    struct ContentView: View {
        @State var show: Bool = false
        @State var viewChangedInfo = CGSize.zero
        var body: some View {
            ZStack{
                VStack {
                    Image("Self") //大頭照圖片
                        .resizable()
                        .aspectRatio(contentMode: .fill)
                        .frame(width: 120, height: 120)
                        .clipShape(Circle())
                        .padding(.top, 30)
                        .shadow(color: .purple, radius:12, x: 1, y:1)
                        .padding(.bottom,5)
                    Image(systemName: "brain").font(.system(size:20))
                        .foregroundColor(.purple)
                        .shadow(color: .blue, radius:10, x: 1, y:1)
                        .padding(.bottom,5)
                    Text("姓名：楊子枋")
                        .background(Color.black)
                        .cornerRadius(5, antialiased: /*@START_MENU_TOKEN@*/true/*@END_MENU_TOKEN@*/)
                        .font(.headline)
                        .padding(.bottom,5)
                    Text("學號：1101445")
                        .background(Color.black)
                        .cornerRadius(5, antialiased: /*@START_MENU_TOKEN@*/true/*@END_MENU_TOKEN@*/)
                        .font(.headline).padding(.bottom,5)
                    Text("努力奮鬥,享受生活")
                        .background(Color.black)
                        .cornerRadius(5, antialiased: /*@START_MENU_TOKEN@*/true/*@END_MENU_TOKEN@*/)
                        .font(.headline).padding(.bottom,5)
                    Divider() //分割
                    BView(text: "0909899465", imageName: "phone.fill")
                    BView(text: "s1101445@mail.yzu.edu.tw", imageName: "envelope.fill")
                    Divider() 
                    InfoView()
                        .rotationEffect(Angle(degrees: show ? 5 : 0))
                        .animation(.interpolatingSpring(mass: 1, stiffness: 100, damping: 10, initialVelocity: 0))
                        .offset(x: viewChangedInfo.width,y: viewChangedInfo.height)
                        .onTapGesture {
                            self.show.toggle()
                        }
                        .gesture(DragGesture().onChanged({ (view) in
                            self.viewChangedInfo = view.translation
                            self.show = true
                        })
                            .onEnded({ (view) in
                                self.viewChangedInfo = CGSize.zero
                                self.show = false
                            }))
                }
            }
        }
    }
    struct InfoView: View {
        var body: some View {
            VStack {
                Spacer()
                HStack {
                    VStack(alignment: .leading) {
                        Spacer()
                        Text("楊子枋")
                            .bold()
                            .foregroundColor(Color.white)
                            .font(.system(size:30)) 
                        Text("Student")
                            .bold()
                            .foregroundColor(Color.yellow)
                    }
                    Spacer()
                    Image("Self")
                        .resizable()
                        .aspectRatio(contentMode: .fill)
                        .frame(width: 90, height: 90, alignment: .center)
                        .foregroundColor(Color.clear)
                        .cornerRadius(10)
                        .clipShape(Circle())
                        .overlay(Circle().stroke(Color.yellow, lineWidth: 3))
                        .shadow(radius: 30)
                        .offset(y:15)
                }
                .padding(/*@START_MENU_TOKEN@*/10/*@END_MENU_TOKEN@*/)
                Text("電話: 0909899465\nMail: s1101445@mail.yzu.edu.tw")
                    .frame(width: 340, height: 100, alignment: .center)
                    .background(Color.purple).foregroundColor(.black).bold()
            }
            .frame(width: 340, height: 190, alignment: .center)
            .background(Color.black)
            .cornerRadius(20)
            .shadow(radius: 20)
        }
    }
    struct BView: View {
        let text: String
        let imageName: String
        var body: some View {
            RoundedRectangle(cornerRadius: 25)
                .fill(Color.white)
                .frame(height: 35)
                .overlay(HStack {
                    Image(systemName: imageName)
                        .foregroundColor(.purple)
                    Text(text)
                        .foregroundColor(Color.black)
                })
                .padding(.all)
        }
    }

   ```
   </td>
 </tr>
</table>
