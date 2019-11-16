# Secret Messenger
###### Version: 2.1
A game for the BBC Micro:bit. The user can send and receive encrypted messages from connected Micro:bits.

## Description
This project is a small application written for the [BBC Micro:bit microcontroller](https://microbit.org/). Using Morse code characters, users can send letters, numbers and words to Micro:bits that are connected by radio. The messages sent by one Micro:bit is encrypted, so only Micro:bits that have the same encryption keys can read each other's messages.

## Tools
This app is written in JavaScript with the Microsoft MakeCode editor. Run this app by using one of the methods:
- Downloading and copying the hex code into your Micro:bit board
- Load the code using one of the [official simulators](https://microbit.org/code/).

## Controls
Here are some specific controls that the app make used of in the simulator
- **Buttons:** Click on the either the A or the B button to activate it
- **Logo UP:** Hover the mouse at the bottom of the Micro:bit simulator to make the board have its logo up
- **Logo DOWN:** Hover the mouse at the top of the Micro:bit simulator to make the board have its logo down
- **Shake:** Press the SHAKE button that appears when appropriate

## Functionalities
### Typing
On startup, the board is blank and will be in "Message Input" mode. Type in characters with Morse Code by using the buttons (A: "dot", B: "dash"). Putting the logo up after each character adn the board will display the character. Enter the next character by either continue pressing buttons, or putting the logo up again to clear the display first.
### Typing Words
Each character typed will continuously be stored in a input queue in until user sends it out, which will also clear the input queue. Currently this is the only way to clear an unwated character/word.
### Sending Message
Shaking the board will send out a message to the other Micro:bits from the board's current input. This also clears the input to prepare for the next words. Sending messages will also displays the message being sent on the board. Sending message will send the number of 
messages sent to the receiving board.
### Receiving Message
When a board receive a message, it will decode the message and display it.
### Encryption Key Input Mode
The app features a message encryption algorithm that prevents boards with unmatched encryption keys to view each other's messages. Putting the board in logo down position in "Message Input" mode will put the board in the "Encryption Key Input" mode. The following actions take place whenever the board enters this mode:
1. The board flashes a "Target" icon twice
2. The board displays the current Encryption Key which is a string of 0's and 1's. If the encryption key is empty, the board will display the phrase "No Key". The key of the board is empty by default.
3. The board flashes a "Target" icon once, signaling that it has finished displaying the Encryption Key
4. All following inputs from the buttons will be recorded as the new encryption key (A: 0, B: 1), replacing the old one.
5. Putting the board logo down will end the "Encryption Key Input" mode and flashes a "Small Diamond" icon once. This will put the board back in "Message Input" mode

### Encryption and Decryption Algorithm
The board employs a homebrewed method of encryption and decryption algorithm. While it is well known that a homebrewed algorithm in cryptography almost always guarantees to be a weak one, ultimately this assignment's purpose was not to teach cryptography, so it is the author's deliberation to employs this method. The encryption and decryption happens every time a message is sent out or received.
The encryption and decryption process makes use of internal variables that keep track of number of messages sent and received,
therefore even if the key between the two Micro:bits is known, the message will still be protected since these number of messages
are shared between the boards. These variables are reset every time the board enter "Encryption Key Input" mode

#### Final Hidden Key
The encryption key is converted to a modified "final hidden key" every time a new key is inputted by the user.
1. Get the length of the current encryption key as a decimal number.
2. Convert this number into a binary number.
3. Concatenate to this binary number the old encryption key, which will only consists of binary number, to make a "final binary number"
4. This "final binary number" is the final hidden key

#### Encoding Key
1. Get the number of messages sent and convert into binary
2. Get the number of messages received and convert into binary
3. Add "final binary number" and sent message binary and received message binary together to make encoding binary number
4. Convert this encoding binary number into decimal to make the encoding key

#### Encryption
1. Get the "encoding key" which should be a binary number and convert it to a decimal number - "final decimal"

*For each character in the sent message:*

2. Convert the character into a decimal number - "char decimal"
3. "char decimal" + "final decimal" * n\[index of the char in the input string\] = some number
4. Subtract this number with the length of the alphabet + number until it is smaller than that length = final number
5. Convert this final number into a character, which is now encoded

#### Decoding Key
1. Get the number of messages sent and convert into binary
2. Get the number of messages received and convert into binary
3. Add "final binary number" and received message binary and sent message binary together to make decoding binary number
4. Convert this decoding binary number into decimal to make the decoding key


#### Decryption
1. Get the "decoding key" which should be a binary number and convert it to a decimal number - "final decimal"

*For each character in the received message:*

2. Convert the character into a decimal number - "char decimal"
3. Calculate "final decimal" * n\[index of the char in the input string\] = 1st big number
4. Add the length of the alphabet + number to "char decimal" = 2nd big number until larger or equal to "1st big number"
5. 2nd big number - 1st big number = final number
6. Convert "final number" into a character to get the decoded character

## Future Development
This project can benefit greatly from improvements in a few key aspects
### Cryptography
As mentioned earlier, a homebrewed encryption algorithm is almost never a good idea in a real application. Therefore, an established method/protocol should be applied to this program, or a great deal more time and effort should be spent on research for the encryption for this program.
### Hardware
The Micro:bit board is meant to only be a introductory microcontroller, therefore it has few controls that a developer can make use. Further iterations of this project should be made with Raspberry Pi or Arduino. However, there *are* a few advantages to this constraint of hardware too; namely: a developer must utilize all of the board's available inputs as controls scheme and it is harder for a user to make incorrect inputs.
### User Experience
From a user experience standpoint, there is a few glaring issues with this iteration of the program. 
1. The program requires the users to know the encryption code beforehand and must be re-enter everytime. It would be better if the boards automate this process for the users, without letting unauthorized boards view the messages (much like how a real messaging app would work).
2. Since the board display is limited, there is a huge cognitive load issue when the user needs to remember the message they are sending.
3. There is limited methods to undo a mistake.
4. It is possible for boards to be out of sync with each other with number of messages sent and received, making encryption errorneous
### Performance
For a game, this program works just fine. However, it would be hard to scale this project, since there are many things that are not yet optimized for performance and speed, such as the binary-decimal conversion functions.

## Old Version
Included in this project is version 1.0 of the project. It is simply a Morse Code Translator. It performs the Typing functionality of Version 2.x only. It ends the character input when the board is shaken instead of having its logo up.
