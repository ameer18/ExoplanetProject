import SwiftUI
#This line imports the SwiftUI framework, which provides tools for building user interfaces across Apple platforms.

struct ContentView: View {
    @State private var messageText = ""
    @State private var messages: [Model] = [Model(image: "", answer: "Welcome to the ExoplanetAI")]
    let botResponse = BotResponse()
    @State private var image = [String]()
    var model = Model(image: "", answer: "")
    

    }
    
   
    }
}
#This is the ContentView struct, which conforms to the View protocol. It defines the main view of the app. It contains the messageText and messages state variables, which store the user's input message and the array of messages exchanged in the chat. It also includes the botResponse instance, which will be used to get responses from the bot. The image and model variables seem to be unused in the provided code.

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

#This is the body of the ContentView view, which defines the app's user interface. It uses a VStack to stack the different elements vertically. The HStack at the top contains the app title and an image icon. The Scroll view is used to display the chat messages, passing the messages array as a parameter. The HStack at the bottom contains a TextField for user input and a Button to send the message.

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

#The sendMessage function is called when the user sends a message. It first appends the user's message to the messages array with the user identifier. It then clears the messageText variable. After a delay of 1 second, it retrieves a response from the botResponse instance by calling the getBotResponse function. It appends the bot's response to the messages array, along with an optional image URL if present.

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

#This is a preview provider for the ContentView. It allows you to see a preview of the view during development.
Overall, the code sets up the main user interface and functionality of the Exoplanet Explorer Chatbot app using SwiftUI. It handles user input, displays chat messages, and interacts with the botResponse instance to get bot responses.













