Requirements:
- User should be able to chat with another user
- Register a user
- User should then be able to add friends
- Should be able to start a chat with a friend
- Be able to show user active status

- DAU - 1M users
- TPS - 100,000 message exchanges during peak hour

- Available
- Reliability
- Max latency for receiving a message when online 1 sec

  
# DB Design
Entities
- User
- UserFriend
- Message

Message
id
text
timestamp

50M Messages per day

# API Design
- HTTP get 
 /getActiveConversations
- input : {
 'userId' : ''
}

 - output - {
    "conversations" : [{
 'id' : ''
'friends' : [{}]
'messages' : [{}]
}
]
}

- HTTP GET
/getChatHistory

input - {
 'conversationId' :
}

output - {
'messages' : [{}]
}

- HTTP GET
/anyRecentMessages

# HLD, Bottlenecks, Scaling
- 100,000 messages per second - Max DB write requests
- 100,000 active chats
- At max 10,000 concurrent req capacity for application server
- Partitioning the DB according to recently sent messages in the past day/ week

 
Client  Web Server                  LB          Application Server        Message Queue    Service       DB
       - /getActiveConversations                  


# Caching