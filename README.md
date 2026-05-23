In this code we are going to create a Rule Based AI Chatbot.
Our main Goal is to Create a simple rule-based chatbot that responds to predefined user inputs.
Key Requirements are Handle greetings and exit commands, Use if-else logic for responses, Run in a continuous loop.
In this code we learn the concept of Control flow, decisionmaking logic, basic AI concepts
Code Explanation:
Firstly, we created a dictionary with predefined keys as user input and response is defined as output.
After that a method is defined that takes input as parameter ,then convert them in lower case and remove whitespaces.
      Then a conditional if statement to handle exit condition.
      In case of any input that is not present in dictionary this will be handle by a built-in .get function, this condition is required because we have limited chat-bot.
      This function will return the reply on th bases of condition.
After that a method is defined that run the chat-bot.
      Display user interface with some messages.
      Then, a while statement that takes input from user 
          call the get_response function and pass the input as an argument.
          it run untill the exit condition will not true.

At the end of code we have starting point of program that call the other functions.
