<h1>Bonus-mid:日期section</h1>
<p>日期元件：選日期後，text會依照選擇的日期標準化輸出</p>

  ```swift
  
import SwiftUI

struct ContentView: View {
    let displayFontType = [".default",".rounded",".monospaced",".serif"];
    @State var displayFontSelected = 0
    @State var IsDeepScheme = false 
    @State var colorArray:Array = [255.0,255.0,255.0]
    @State var stepperValue = 0
    @State var sliderValue = 0.0
    @State var selectedDate = Date()
    var body: some View {
        NavigationView{
            Form( content: {
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
# Bonus-mid Demo圖片
<img width="30%" src="https://raw.githubusercontent.com/FatFang/1101445-swift/main/B1.jpg">
<img width="30%" src="https://raw.githubusercontent.com/FatFang/1101445-swift/main/B2.jpg">
<img width="30%" src="https://raw.githubusercontent.com/FatFang/1101445-swift/main/B3.jpg">


