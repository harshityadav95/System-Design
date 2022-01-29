# Mongo DB

```
Launching the Mongo DB shell
>> mongo

List all database  
>> show dbs

Select a database from list (admin is in list)
>> use admin 

show collections
>> show collections

Use js in shell to run query such as 
>> db.system.version.find()
// will list all value

>> db.system.version.count()
//will list count of collections

Create a database
>> use myFirstDatabase

Create new collection
>> db.createCollection("myFirstCollection")

Now Show Dbs
>> show dbs

Show Collections
>> show collections

----------------------------------------------

Drop a database
>> db.dropdatabase()

create database
>> use blog

Insert into collection
>> db.posts.insert({
title:"mongodb rules",
body:" my first mongodb document",

// time in 24hrs
date:: ISODate("2020-0901T18:30:00Z"),

tags:["mongodb","databases"],

likes:2,
author: {
        name:"Harshit",
        surname:"Yadav"},
location:{
        type:"Point",
        coordinates:[-112.45,37.76]
        
        }
})

List all Documents
>>db.posts.find()

List all documets in pretty format
>>db.posts.find().pretty()

----------------------------------------

Schema can also be enforced in a database

Find blog Post in a database
>> db.posts.find({title:"Second Post"}).pretty()

Sort titles by Ascending Order
>> db.posts.find().sort({title:1}).pretty()

Sort titles by descending Order
>> db.posts.find().sort({title:-1}).pretty()

Find Posts with greater then 2 Likes and sort
>> db.posts.find({likes:{$gt:2}}).sort({likes: -1}).pretty()

Find Posts with greater equal to 2 Likes
>> db.posts.find({likes:{$gte:2}}).pretty()

Find Posts with  equal to 2 Likes 
>> db.posts.find({likes:{$eq:2}}).pretty()


Update a field in document
>> db.post.update ({
title:" Second Post",      
},
{
 $set: {
         tags:['tag1'],
         location: {
         type :"Point",
         coordinates :[-73.97,40.77]
 
         }
 }
 })
 
 Delete a field in document
 >> db.posts.update(
 {
         title:"Second Post"
         
 },
 {
     $unset: {
     tags: ""
     }    
 }
 )
 
 ---------------------------------------------
 
 Insert many documents in database
 >> db.posts.insertMany(
 [
         {
                 title:"Awesone post",
                 body:"Body of awesome post",
                 date: ISODate("2019-11-01T03:30:00Z)
                 
         },
          {
                 title:"Awesone post",
                 body:"Body of awesome post",
                 date: ISODate("2019-11-01T03:30:00Z)
                 likes:3,
                 
         },
                   {
                 title:"Awesone post",
                 body:"Body of awesome post",
                 date: ISODate("2019-11-01T03:30:00Z)
                 location:{
                        type:"Point",
                        coordinates:[-112.45,37.76]
        
        }
                                 
         },

 ]
 )
 
 
 Update one  documents with a field
 >> db.posts.update()
 (        
         {},
         {        
                 $set:{
                         published:false:
                 }
         }
 )
 
 
Update All  documents with a field
 >> db.posts.update()
 (        
         {},
         {        
                 $set:{
                         published:false:
                 }
         },
         {
                 multi:true
         }
 )
 
 AND operator queries
 >> db.posts.find(
         {
            $and:[
            {
                    title:"Awesome Post"
            },
            {
                 likes:{
                         $exists:true
                       }   
            }
            
            ]     
         }
         
 )
 
 
 -----------------------------------------
```
