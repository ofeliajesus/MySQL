### My Solution for Interviews_HackerRank problem


```
Select 
  contest_id, hacker_id, name
, sum(total_submission) as total_submission, sum(total_accepted_submission) as total_accepted_submission  
, sum(total_views) as total_views, sum(total_unique_views) as total_unique_views
from (
Select con.contest_id, con.hacker_id, con.name
, coalesce(sum(total_views),0) as total_views, coalesce(sum(total_unique_views)) as total_unique_views
, 0 as total_submission, 0 as total_accepted_submission 
from Contests con
inner join Colleges col on con.contest_id = col.contest_id
inner join Challenges cha on col.college_id = cha.college_id
left join View_Stats vs on cha.challenge_id = vs.challenge_id
Where total_views IS NOT NULL and total_unique_views IS NOT NULL
group by con.contest_id, con.hacker_id, con.name
UNION ALL
Select 
   con.contest_id, con.hacker_id, con.name
,  0 as total_views, 0 as total_unique_views   
,  coalesce(sum(total_submission),0) as total_submission, coalesce(sum(total_accepted_submission),0) as total_accepted_submission   
from Contests con
inner join Colleges col on con.contest_id = col.contest_id
inner join Challenges cha on col.college_id = cha.college_id
left join Submission_Stats ss on cha.challenge_id = ss.challenge_id
Where Total_submission IS NOT NULL and total_accepted_submission IS NOT NULL
group by con.contest_id, con.hacker_id, con.name
) as interviews
Group by contest_id, hacker_id, name
order by contest_id
```
#### If you want to test use the script below to create the Database objects  

```
#Script to create objects  I used MySQL

CREATE TABLE Contests (
    contest_id int NOT NULL,
    hacker_id int NOT NULL,
    name varchar(150),
    PRIMARY KEY (contest_id)
);


Create table Colleges
(
 college_id int NOT NULL,
 contest_id int,
 PRIMARY KEY (college_id),
 FOREIGN KEY (contest_id) REFERENCES Contests (contest_id)
);


Create table Challenges
(
 challenge_id int NOT NULL,
 college_id int,
 PRIMARY KEY (challenge_id),
 FOREIGN KEY (college_id) REFERENCES Colleges (college_id)
);


Create table View_Stats 
(
 challenge_id int NOT NULL,
 total_views int,
 total_unique_views int,
 FOREIGN KEY (challenge_id) REFERENCES Challenges (challenge_id)
);


Create table Submission_Stats
(
 challenge_id int NOT NULL,
 total_submission int,
 total_accepted_submission int,
 FOREIGN KEY (challenge_id) REFERENCES Challenges (challenge_id)
);


#Insert Data 
Insert into Contests (contest_id,hacker_id, name)
Select 66406, 17973,'Rose'
union all
Select 66556, 79153, 'Angela'
union all
Select 94828, 80275, 'Frank';


insert into Colleges ( college_id, contest_id)
Select 11219, 66406
union all
Select 32473, 66556
union all
Select 56685, 94828;


insert into Challenges (challenge_id, college_id)
Select 18765,11219
union all
Select 47127, 11219
union all
Select 60292, 32473
union all
Select 72974, 56685;

SET FOREIGN_KEY_CHECKS = 0;
insert into View_Stats (challenge_id, total_views, total_unique_views)
Select 47127,26, 19
union all
Select 47127, 15, 14
union all
Select 18765, 43, 10
union all
Select 18765, 72, 13
union all
Select 75516, 35, 17
union all
Select 60292, 11, 10
union all
Select 72974, 41, 15
union all
Select 75516, 75, 11;



insert into Submission_Stats (challenge_id, total_submission, total_accepted_submission)
Select 75516, 34, 12
union all
Select 47127,27, 10
union all
Select 47127, 56, 18
union all
Select 75516, 74, 12
union all
Select 75516, 83, 08
union all
Select 72974, 68, 24
union all
Select 72974, 82, 14
union all
Select 47127, 28, 11;

SET FOREIGN_KEY_CHECKS = 0;

```

##### Ofelia Jesus 
##### Senior Database and BI Analyst / Developer