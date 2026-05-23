<h2>DecodeLabs Rule Based AI Chatbot</h2>
<h3>Objective:</h3>
<ul>
      <li>In this code we are going to create a Rule Based AI Chatbot.</li>
      <li>Our main Goal is to Create a simple rule-based chatbot that responds to predefined user inputs.</li>
      <li>Key Requirements are Handle greetings and exit commands, Use if-else logic for responses, Run in a continuous loop.</li>
      <li>In this code we learn the concept of Control flow, decisionmaking logic, basic AI concepts.</li>
</ul>
<h3>Code Explanation:</h3>
<ol>
      <li>Firstly, we created a dictionary with predefined keys as user input and response is defined as output.</li>
      <li>After that a method is defined that takes input as parameter ,then convert them in lower case and remove whitespaces.</li>
            <li>Then a conditional if statement to handle exit condition.</li>
            <li>In case of any input that is not present in dictionary this will be handle by a built-in .get function, this condition is required because we have limited chat-bot.</li>
            <li>This function will return the reply on th bases of condition.</li>
      <li>After that a method is defined that run the chat-bot.</li>
      <li>Display user interface with some messages.</li>
            <li>Then, a while statement that takes input from user</li> 
            <li>call the get_response function and pass the input as an argument.</li>
            <li>it run untill the exit condition will not true.</li>

      <li>At the end of code we have starting point of program that call the other functions.</li>
</ol>

