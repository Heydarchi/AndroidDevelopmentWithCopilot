# AndroidDevelopmentWithCopilot
This repo is an example on how use AI to develop an Android application.
This repo was used to compare the three new AI code assistant:
- **GPT-4o**
- **Claude Sonnet 3.7**
- **Gemini 2.5**

The IDE used to compare them was VSCode. You can check the video through the link below:

[![AI Coding Assistants Battle: Copilot (GPT-4o) vs Claude 3.5 vs Gemini 1.5 – Android Dev Test!](https://img.youtube.com/vi/JVMevHEecAU/maxresdefault.jpg)](https://youtu.be/JVMevHEecAU)


## Project definition

Here you can find the project definition:


- The application enables users to have a simple chat over Bluetooth Classic.
- The pairing process will happen manually
- The application will have a service managing/controlling Bluetooth connection based on the diagram.
- The data will be transmitted between the service and a fragment through two AIDL interfaces that one of them will be implemented on the service package/side and another one will be implemented on the fragment package/side
- The received data on the fragment side will be stored in a ViewModel class and then will be reflected to UI
- The UI will be implemented through an XML layout and will be updated through the ViewModel


## Expectation

- The package name is com.aospinsight.copilot
- Use kotlin instead of Java. Also  consider the available files and classes
- Use build.gradle.kts
- Add the required permission to AndroidManifest
- Add the required configuration to gradle files to build aidl files
- Add the required packages or other configurations to the gradle files
- Implement a method to request the required permission in the MainActivity and call it I onCreate
- Create .aidl files based on the plantunml
- Implement BtAidlServiceImplementation and BtAidlClientImplementation to use them in the FirstFragment and the service
- The service should return BtAidlServiceImplementation  in onBind method
- Bind to the service from the FirstFragment in onCreate method
- The service injects BluetoothClasic as a parameter to BtAidlServiceImplementation and will start listening on receiving enableBT
- Upon receiving a message through Bluetooth the service will send the message through the register handle(IBtAidlService) to the FirstFragment
- Edit the XML layout of the FirstFragment to have a ListView showing the sent and received message history. At the bottom of that, a TextEdit and a Button will be responsible for getting the user message and sending it through the AIDL interface to the service, and the service will send it through the Bluetooth connection.
- Implement the ViewModel and the changes in the FirstFragment layout
- Create the classes based on the plantuml and implement the methods. You can add any required-missing method
- Try to create the file in app/src/main in the right package or create the required packages and folders
- Now we must have these classes and interfaces:
	IBluetooth, BluetoothClasic, BtBkService, IBtAidlCient, IBtAidlService, BtAidlServiceImplementation, BtAidlClientImplementation, BtViewModel
- Important : Now check the instructions and description one by one and carefully to ensure nothing is being missed


## Project Architecture

- Develop the application based on the plantuml diagram provided below which shows the architecture of the project. 
- Only the important aspects, relationships, and key features are defined. So you can add the rest of the requirements.

![The project architecture](project_architecture.png)


```
@startuml

abstract class IBluetooth{
    void enableBT(bool state)
    bool sendMessage(void onMsgCallback)
    # bool startListening()
    # void onMessageReceived(ByteArray[] receivedMsg)
    void isOpen()

}
class BluetoothClasic{
    # BluetoothDevice btDevice
    # BluetoothSocket btSocket

    void enableBT(bool state)
    bool sendMessage()
    # bool startListening(void onMsgCallback)
    # void onMessageReceived(ByteArray[] receivedMsg)
    void isOpen()
}

BluetoothClasic ..|> IBluetooth

note right of BluetoothClasic
    This class will be used to implement the IBluetooth interface.
    It will be used to send and receive messages from the bluetooth device.
    It will also be used to enable and disable the bluetooth device.
end note

class BtBkService{

    IBinder onBind(Intent intent)
}

interface IBtAidlCient{
    void onMessageReceived(ByteArray[] receivedMsg)
}

note left of IBtAidlCient
    This aidl file will be impleneted on the Fragment side.
    The Fragment will implement the BtAidlClientImplementation interface and will be registered to the service.
    The service will call the onMessageReceived method when a message is received.
end note

interface IBtAidlService{
    void onRegisterClient(IBtAidlCient client)
    void onUnregisterClient(IBtAidlCient client)
    void enableBT(bool state)
    bool sendMessage(void onMsgCallback)
}

note right of IBtAidlService
    This aidl file will be impleneted on the service side.
    The service will implement the BtAidlServiceImplementation and return it asBinder on the onBind method.
    The service will register the client on the onRegisterClient method and unregister it on the onUnregisterClient method.
    The client will call the sendMessage method to send a message to the service.    
end note

class BtAidlClientImplementation{
    void onMessageReceived(ByteArray[] receivedMsg)
}

class BtAidlServiceImplementation{
    void BtAidlServiceImplementation(IBluetooth btClassic)
    IBtAidlCient client
    IBinder asBinder()
    void onRegisterClient(IBtAidlCient client)
    void onUnregisterClient(IBtAidlCient client)
    void enableBT(bool state)
    bool sendMessage(void onMsgCallback)
}


IBtAidlService --> IBtAidlCient
BtAidlServiceImplementation ..|> IBtAidlService
BtAidlClientImplementation ..|> IBtAidlCient

class MainActivity{
    void requestPermissions() .. Request the required permissions
}

class FirstFragment 

class BtViewModel{
    void addReceivedMessage((ByteArray[] receivedMsg)
    void addSentMessage((ByteArray[] receivedMsg)
}



BtAidlServiceImplementation --> IBluetooth

BtBkService --> BluetoothClasic
BtBkService --> BtAidlServiceImplementation

MainActivity --> FirstFragment

FirstFragment --> IBtAidlCient
FirstFragment --> BtViewModel


@enduml
```
