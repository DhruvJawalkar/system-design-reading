#Design TikTok
- Features scope, #users, #tps estimate
- Data model/schema
- API design
- HLD
- Bottlenecks/ security/ caching

## Functional requirements 
- To be able to upload videos
- To be able to follow other users/ view their videos
- To be able to comment on videos
- Video cap size of 5MB
- comment - 160 max chars  
- #users - 10M, DAU - 1M
- #uploads per sec - 50 vids
- #comments per sec - 100K
- #feed req/ profile load req per sec - 500  
- 1 user follows 50 other users

- Newsfeed of following users, max latency req of 200ms
- available, reliable 

## DB Design
Entities
- User
- Post
- Comment

Post
PostID - int
UserID - 
videoUrl - varchar(256)
title - 
description - 

## API Design
- upload video 
  - /uploadVideo - HTTP POST
  - input - {
    video : File// video file multipart formdata
  }
  -output - {
    videoId : ''
  }

  - /addVideoMeta - HTTP POST
  - input - {
    videoId : '',
    title : '',
    description : ''
  }

- /feed - HTTP GET 
  - input - {
    userId : ''
  }

  - output - {
    top_10 : [{
        postId : '',
        videoUrl : '',
        numLikes : '',
        numComments : '',
        comments : [{
            commendId : '',
            commentText : '',
            userId : '',
            userName : '',
            userProfilePicUrl : '' 
        }]
    }]
  }
    

//HLD, Scaling, Bottlenecks
client    web server          LB     [application server]  Newsfeed Service                             LB      database                                                   LB   s3
          - uploadVideo                                     -Offline daily bulk task, pre compute top 10       - shard Posts/ Comments table acc to recency/location            - upload video acc to region return url
          - followUser
          - makeComment
          - fetchFeed
          - viewUserProfile     


//Security and Caching
client    web server  LB   Redis/caching (20%)    application server  Newsfeed Service    LB       database    LB     s3
                          - Cache feed, posts for hot users among DAU
                          