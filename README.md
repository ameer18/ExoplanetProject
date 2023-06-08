import SwiftUI
#This line imports the SwiftUI framework, which provides tools for building user interfaces across Apple platforms.



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

swift


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

import SwiftUI

struct Scroll: View {
    
    var messages: [Model]
    
    var body: some View {
        // ... View code ...
    }
}
#This is the Scroll struct, which conforms to the View protocol. It defines a custom view for displaying the chat messages. It takes an array of Model objects as input, representing the chat messages.

var body: some View {
    ScrollView {
        VStack(alignment: .leading) {
            ForEach(messages, id: \.self) { message in
                ChatBubbleView(message: message, isUserMessage: message.answer.contains("[USER]"))
            }
        }
    }
}
#This is the body of the Scroll view, which defines the layout and appearance of the chat message view. It uses a ScrollView to enable scrolling through the chat messages. Inside the ScrollView, it uses a VStack to stack the chat bubbles vertically. The ForEach loop iterates over the messages array and creates a ChatBubbleView for each message. The ChatBubbleView receives the message and a boolean flag indicating whether the message is from the user or the bot.

struct Scroll_Previews: PreviewProvider {
    static var previews: some View {
        Scroll(messages: [Model(image: "kepler62f", answer: "hello")])
    }
}
#This is a preview provider for the Scroll view. It allows you to see a preview of the view during development. In this case, it creates a preview with a single message for demonstration purposes.

Overall, the Scroll view is responsible for displaying the chat messages in a scrollable view. It uses a ScrollView and a VStack to arrange the chat bubbles vertically. The chat bubbles are created using the ChatBubbleView and are generated based on the messages passed into the view.


import SwiftUI
struct ChatBubbleView: View {
    var message: Model
    var isUserMessage: Bool
    
    var body: some View {
        // ... View code ...
    }
}
#This is the ChatBubbleView struct, which conforms to the View protocol. It represents a single chat bubble in the conversation. It takes a Model object representing the message and a boolean flag indicating whether the message is from the user or the bot.

var body: some View {
    HStack {
        if isUserMessage {
            VStack {
                // ... User message view code ...
            }
        } else {
            VStack {
                // ... Bot message view code ...
            }
        }
    }
}
#This is the body of the ChatBubbleView view. It defines the layout and appearance of the chat bubble based on whether it's a user message or a bot message. If isUserMessage is true, it displays the view for user messages. Otherwise, it displays the view for bot messages.

if isUserMessage {
    VStack {
        Text(message.answer)
            .padding()
            .foregroundColor(.white)
            .background(Color.blue.opacity(0.9))
            .cornerRadius(10)
            .padding(.horizontal, 16)
            .padding(.bottom, 10)
    }
} else {
    VStack {
        Text(message.answer)
            .padding()
            .background(Color.gray.opacity(0.15))
            .cornerRadius(10)
            .padding(.horizontal, 16)
            .padding(.bottom, 10)

        if !isUserMessage {
            if !message.image!.isEmpty {
                Image(uiImage: (UIImage(named: message.image ?? "") ?? UIImage()))
                    .resizable()
                    .scaledToFit()
                    .frame(width: UIScreen.main.bounds.width - 40)
            }
        }
    }
}
#This code segment defines the views for user and bot messages. For user messages, it displays a blue chat bubble with white text. For bot messages, it displays a gray chat bubble with the message text. If the message contains an image (message.image is not empty), it adds an Image view displaying the image.

struct ChatBubbleView_Previews: PreviewProvider {
    static var previews: some View {
        ChatBubbleView(message: Model(image: "kepler62f", answer: "Hello"), isUserMessage: false)
    }
}
#This is a preview provider for the ChatBubbleView view. It allows you to see a preview of the view during development. In this case, it creates a preview with a sample message for demonstration purposes.
The ChatBubbleView struct represents the visual appearance of a chat bubble, customized based on whether it's a user or a bot message. It uses Text and Image views to display the message content and the optional image.

import Foundation
#This line imports the Foundation framework, which provides fundamental data types, collections, and operating system services to Swift.

struct BotResponse {
    func getBotResponse(message: String) -> [String] {
        // ... Bot response code ...
    }
}
#This is the BotResponse struct, which encapsulates the logic for generating bot responses. It has a single function getBotResponse that takes a user message as input and returns an array of strings representing the bot's response.

func getBotResponse(message: String) -> [String] {
    let tempMessage = message.lowercased()
    print(tempMessage)
    // ... Bot response conditions ...
}
#This function processes the user's message by converting it to lowercase using the lowercased() method. It also prints the lowercase message to the console for debugging purposes.
The function then uses a series of if and else if conditions to determine the appropriate response based on the user's message. Each condition checks if the tempMessage contains specific keywords or phrases using the contains() method. If a match is found, the function returns an array of strings representing the bot's response.

return ["An exoplanet is a planet which resembles earth in terms of size, its distance from the star, atmosphere and gravity alongside other factors. Most exoplanets are in the habitable zone of its own star."]
#This line represents a bot response. It returns an array with a single string, which is the explanation of what an exoplanet is.
The code follows the same pattern for other bot responses, providing information about specific exoplanets and answering related questions.

else {
    return ["That's cool"]
}
#This else block represents a default response if none of the conditions match the user's message. In this case, it returns an array with the string "That's cool."
The BotResponse struct provides the logic for generating bot responses based on the user's input. It uses a series of conditional statements to match specific keywords or phrases and returns the corresponding response.
Overall, the BotResponse struct complements the chatbot functionality by determining the appropriate responses to user queries about exoplanets.













