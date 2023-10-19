# Real-Time Chat with Task Board
# PART-1
import React, { Component } from 'react';

class ChatApp extends Component {
  constructor() {
    super();
    this.state = {
      messages: [],
      newMessage: '',
    };
  }

  handleMessageChange = (event) => {
    this.setState({ newMessage: event.target.value });
  };

  handleSendMessage = () => {
    const { messages, newMessage } = this.state;
    if (newMessage.trim() !== '') {
      messages.push(newMessage);
      this.setState({ messages, newMessage: '' });
    }
  };

  render() {
    const { messages, newMessage } = this.state;

    return (
      <div>
        <div className="chat-messages">
          {messages.map((message, index) => (
            <div key={index} className="message">
              {message}
            </div>
          ))}
        </div>
        <div className="chat-input">
          <input
            type="text"
            value={newMessage}
            onChange={this.handleMessageChange}
            placeholder="Type your message"
          />
          <button onClick={this.handleSendMessage}>Send</button>
        </div>
      </div>
    );
  }
}

export default ChatApp;

# This code provides a basic structure for a chat application in React. You would need to style it and integrate real-time communication functionality with a server for a live chat experience. Consider using libraries like Socket.io, Firebase, or GraphQL subscriptions for real-time communication.



# PART-2
# To create a MongoDB database for a live chat room, you'll need to set up your MongoDB server, define a schema, and create collections to store chat messages and user information. Here's a basic example using the Mongoose library in Node.js, assuming you have MongoDB installed and running. Please note that this is a simplified example, and in a real-world scenario, you would add more features and validation.

Set Up Node.js Project:
Make sure you have Node.js and MongoDB installed.
Install Dependencies:
Run the following command to install the necessary packages, including Mongoose.
npm install mongoose express body-parser


# PART-3
Create a MongoDB Connection:
Create a file (e.g., db.js) to set up your MongoDB connection:
const mongoose = require('mongoose');

const mongoURI = 'mongodb://localhost/live-chat-room'; // Replace with your MongoDB URI

mongoose.connect(mongoURI, { useNewUrlParser: true, useUnifiedTopology: true });

module.exports = mongoose;


# PART-4
Define a Chat Message Schema:
Create a schema for chat messages in a separate file (e.g., models/Message.js):
const mongoose = require('mongoose');

const messageSchema = new mongoose.Schema({
  text: String,
  user: String,
  timestamp: {
    type: Date,
    default: Date.now,
  },
});

module.exports = mongoose.model('Message', messageSchema);

# PART-5
Express API for Chat:
Create an Express API to handle chat-related operations (e.g., sending and retrieving messages).
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const Message = require('./models/Message');

app.use(bodyParser.json());

// Create a new message
app.post('/messages', async (req, res) => {
  try {
    const { text, user } = req.body;
    const message = new Message({ text, user });
    await message.save();
    res.status(201).send('Message saved.');
  } catch (err) {
    res.status(500).send(err);
  }
});

// Retrieve chat messages
app.get('/messages', async (req, res) => {
  try {
    const messages = await Message.find().sort({ timestamp: -1 }).limit(20);
    res.status(200).json(messages);
  } catch (err) {
    res.status(500).send(err);
  }
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});

# PART-6
Run your Express server using the following command:
node your-server-file.js
# Now e have a basic MongoDB database set up for a live chat room. You can create and retrieve messages using the API endpoints /messages. For a production application, you'd need to secure your API, add user authentication, and handle real-time updates using WebSocket libraries like Socket.io or GraphQL subscriptions.
