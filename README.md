MongoDB Task:

1.	Find all the topics and tasks which are thought in the month of October:

For topics collection:

   Db.topics.find({
   date: 
   {
   $gte : ISODate(“2025-10-01”), 
   $lte: ISODate(“2025-10-31”)}});

This will fetch data from topics collection where the date is from October 1 to 31.

For tasks collection:  

   Db.tasks.find({ 
   date: 
   {
   $gte :ISODate(“2025-10-01”), 
   $lte:ISODate(“2025-10-31”)}});

   This will fetch the data from tasks collection where the date is from October 1 to 31.

2.	Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020: 
        Db.company_dives.find({date: {$gte:ISODate(“2020-10-15”, $lte:ISODate(“2020-10-31”)}})
         This will return company_drive collection data the satisfying condition is from oct 15 to 31st.

3.	Find all the company drives and students who are appeared for the placement.
4.	
          Db.company_drives.aggregate([{
                 $lookup:{    From:”users”,
                        localField : “attended_students”,
                      foreignField:”user_id”,
                       as: “Attended_Students”}},
  	                    { $project:{
                       Company:1,
                       Drive_date:1,
                    Attended_Students:{name:1, user_id:1}}}]).

   it joins the company_drive collection with users collection.it will return the matched data and in the Attened_Students field. to display company name, drive date, user name and id has given 1 to project the data. here Attened_Students will be an array of objects.  
  	

5. Find the number of problems solved by each user in Codekata
   
Db.codekata.aggregate([{
 
 $lookup: {
      from: "users",                     
      localField: "user_id",        
      foreignField: "_id",                
     as: "user_info"               
    }
  },
  { $unwind: "$user_info" },   
      {
    $project: {
      _id: 0,                       
      name: "$user_info.name",     
      email: "$user_info.email",   
      problems_solved: 1           
    }
  }])
here used codekata and user collection to this query. lookup returns the array of objects in the field usre_info. to access this used unwind to concept. it will convert the array of objects to normal object. to access easily we use unwind. else we can use $arrayElemAt to get values.

4.	Find all the mentors with who has the mentee's count more than 15
Db.mentors.aggregate([

{ $project:   {
              name: 1,
      mentee_count: { $size: "$mentees" }
    }  },
  {
    $match: {
      mentee_count: { $gt: 15 }
    } }])

here mentees have userid's. so first finding size of the mentees for each mentor. then we check the condition greater than 15. 

6.Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020:

db.attendance.find({
  status: "Absent",
  date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") }
})	
using greater than equal to and lesserthan equal to check the absenties. 

db.tasks.find({
  submitted: false,
  date: { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") }
})
 using greater than equal to and lesserthan equal to check the task submission.

