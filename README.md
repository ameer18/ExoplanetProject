import SwiftUI

struct ContentView: View {
    @State private var messageText = ""
    @State private var messages: [Model] = [Model(image: "", answer: "Welcome to the ExoplanetAI")]
    let botResponse = BotResponse()
    @State private var image = [String]()
    var model = Model(image: "", answer: "")
    
    var body: some View {
        // ... View code ...
    }
    
    private func sendMessage(message: String) {
        // ... sendMessage function code ...
    }
}

var body: some View {
    VStack {
        HStack {
            Text("ExoplanetBot")
                .font(.largeTitle)
                .bold()
            
            Image(systemName: "bubble.left.fill")
                .font(.system(size: 29))
                .foregroundColor(.red)
          
        }

        Scroll(messages: messages)
        
        HStack {
            // ... Input TextField and Send Button code ...
        }
        .padding()
    }
}

private func sendMessage(message: String) {
    withAnimation {
        messages.append(Model(image: "", answer: "[USER]" + message))
        self.messageText = ""
    }
    
    DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
        withAnimation {
            let response = botResponse.getBotResponse(message: message)
           
            messages.append(Model(image: response.count > 1 ? response[1] : "", answer: response[0]))
          
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}













