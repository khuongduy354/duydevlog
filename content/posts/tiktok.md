+++
title = 'Tiktok devlog'
date = 2024-02-18T22:24:58+07:00
description = "A study case where I implement Tóp Tóp, a Social Media mobile app in React Native + Express.js"
draft = false
+++ 
# Tiktok Clone  
**Programming blog** 
I want to learn backend API development through making a tiktok API. 
It'd be very basic, I can use to store my private photos/videos, beside that, 
I don't use it much, it's primarily for learning.

Firstly, I list out some features I'd like to implement:  
 
- Authentication/Authorization: email, password based
- Upload videos 
- Users follow users 
- Allow to update users profile
- Like, comment videos
- Basic feed generation for users  

# Tech stack      
Let use Express.js for backend, PostgresSQL (provider: ElephantSQL) for database. For frontend, I managed to find 
a React Native tiktok clone repo, so I forked it from there. I had no knowledge of React Native, but I've used React before, so I learnt to setup and launch 
using expo, the code is very similar to React. Pretty much I have to do was add some extra UI components, and populate 
the UI by fetching the server. For cloud storage I used Cloudinary, it's free and easy to setup. To sum up:  

- Express.js 
- PostgresSQL (ElephantSQL)  
- React Native 
- Cloudinary

# Implementations

### Database  
I pick SQL as most data are related, ElephantSQL is free for 25mb which is enough to toy with. I draw schema using dbdesigner.net, 
then execute CREATE TABLE statements in ElephantSQL. The schema here: [Schema](https://github.com/khuongduy354/tiktok-api/blob/master/tiktok-db-diagram.png).  
If I do it again I'd pick supabase instead, ElephantSQL doesnt have a Table visualizer and is pretty inconvenient.

To interact with database using Express.js, at first, I use pg package and raw SQL query (string). It gave me so much freedom, but it's ugly and later I learnt
that it's vulnerable to SQL injection, so I change to knex.js, which is a query builder. Im not a fan of ORM though, as it adds overhead to my project.    

### Auth  
Classic email password auth, I use bcrypt to hash user password and store to database, and every login will grant a JWT token.   
- Flow for signup: insert user row, with hashed password, and return a JWT token. 
- For signin: user types email, password, that password is then hashed and compare to the one in database, if match, allow continue and return a JWT token. 
- For every request that need auth (create video for e.g): that request must have Bearer <token> in Authorization header. I use Express middleware to validate 
that, and decide if user can make request. 

### Design and setting up endpoints 
I use postman for testing, it get the job done very well for me. I learn to setup environment variables, setting scripts to save JWT token after   
a sign in request, and that token is included in every request that need auth.
 
### File uploading 
I use multer.js package, with Cloudinary to store videos/images. Multer can take form data, and add file's buffer as req.file.buffer, 
and cloudinary allowing uploading using buffer, so I just stream straight from buffer to Cloudinary. There's no need for temporary local file,
because multer allow in-memory storage. 

### Feed generation
About feed generation, there're only 2 modes: all videos or only followers videos. I intend to make a public/private flag for video 
later, but for now "all" will query all videos in database, randomly, limited to 10-20 videos and return to user. For followers' videos, I add
an inner join for follower. I still leave this part as raw sql, cuz it's hard to make in knex.  

Scroll to bottom attempt to fetch more. 

### Other stuffs 
For other data, just CRUD all the way. I learnt to make some very complex sql query, join a few tables, aggregate,... 

# Issues (things im questioning)
- User index: currently I'm using SQL auto increment as it's simple, but I think enumerable userIDs arent secured. Therefore, all my other projects use UUID4.  
- Atomicity of uploading video to Cloudinary, and inserting Video data in table: a case where video is uploaded to Cloudinary but failed to insert in table, 
which cause the video in Cloudinary unused. Maybe I'll attempt to delete video in Cloudinary if SQL insertion fails.  
- Asymmetric unique value pair in Following table: I don't know if it's a correct term, but it's like this: Following is the linking table of followers and followee.  
That table has 2 columns: follower_id and followee_id. For example a value pair: (1,2) is unique and cant be duplicate, (2,1) is okay.  
A symmetric unique value pair is like a Friend table: (1,2) and (2,1) are considered conflict and cant be in the same table!   
I'm not setting that constraint yet, because don't know how to.
To sum up: 
A follows B differ from B follows A 
But 
A friend of B same as B friend of A  

# How to upgrade this project/Future intentions
- A push notification server is needed, fanout to followees...
- Caching: especially feed data (hot topic and stuffs), should be add later, after video classification added (trending, popular, new,...)  
- Recommendation system: machine learning to track user preference ?  
- Message: a websocket server, I've done many in web projects, but not in mobile app yet 


# Deployment   
- For frontend, I use eas and export to .apk file, very basic. One issue I encounter is image src with http protocol dont work, 
so i need to change them to https and it works! 
- For backend, I use Vercel, at first, my public folder isn't empty, so It keeps popping error. Then i clear it and it works, 
turns out Backend on vercel can't hold any local file, lucky I use multer in memory storage and not as local files.










