We had a case story in Hebrew (to be translated here in English) from which we drew an ERD. From the ERD we built schemas.
**Case history:**
The thirty-seventh Israeli government (also known as the sixth Netanyahu government) received the confidence of the twenty-fifth Knesset and was sworn in on December 29, 2022. Since then, the government has been feverishly engaged in implementing and promoting many legislative actions. The legislation is met with strong opposition from the opposition and parts of the public, and each vote requires both the coalition and From the opposition to mobilize the members of the Knesset to support/oppose the legislation Regarding the update, the member of the Knesset making the update must know the affiliation of the Knesset members by party. The update should of course include the details of the law in question brought to the vote, and will include, among other things, the subject of the law (e.g. the gift law), the proposal number (e.g. proposal number P2303/25/ which was brought to a vote under vote No. 3, see link), the member of Knesset who initiated the law, the government office to which the law relates, the budgetary cost, as well as the type of reading (preliminary/first/second/third). Also, the system must record the The actual votes on the various laws.

**Question 1 -** **Create an ERD diagram for the above description - attached as a picture.**

**Question 2 -** **Schema construction**

**solution-**

Member of Knesset (id, party, name, Knesset government office)

Updating (id1 (FK), id2(FK), update number (FK), voting number (FK), offer number (FK) execution date, arrival status, updating content)

Update (update number, voting number (FK), offer number (FK))

Voting (voting number, offer number (FK), voting date)

Passes (voting number (FK), offer number (FK), call type)

Law (offer number, budgetary cost, government office, subject of law, law government office, id (FK))

Choice (voting number (FK), offer number (FK), id (FK), voting content)

**solution after feedbek-**

GovernmentOffice(officeId, officeName )

Politician(politicianID , firstName , lastName , partyName , officeId(FK))

Rule(offerID ,title ,callType ,officeId(FK))

Offer(offerID(FK), politicianID(FK))

Update(intentionToAttend ,updateTime, politicianID(FK) ,politicianID(FK), offerID(FK)) 

Vote(voteID,  voteResult , politicianID(FK), offerID(FK))

BelongsTo(politicianID(FK), officeId(FK))

**Question 3 -**
**A.** **Create a database for the agreement you made.**

**solution-** 

CREATE database KNESSET;

use KNESSET;
 
create table GovernmentOffice(

officeId int,

officeName varchar(30),

primary key (officeId)

);

 create table Politician(
 
politicianID int ,

firstName varchar(30),

lastNme varchar(30),

partyName varchar(30),

GovernmentOffice int,

foreign key(GovernmentOffice) references GovernmentOffice(officeId),

primary key(politicianID,GovernmentOffice)

);

 create table Rule(
 
offerID int,

title varchar(30),

budget int,

callType varchar(30),

GovernmentOffice int,

foreign key(GovernmentOffice) references GovernmentOffice(officeId),

primary key(offerID,GovernmentOffice)

);
  
create table Offer(

Politician int,

Rule int,

foreign key(Rule) references Rule(offerID),

foreign key(Politician) references Politician(politicianID),

primary key(Rule,Politician)

);
 
create table Update_(

PoliticianA int,

PoliticianB int,

Rule int,

intentionToAttend bit,

updateTime varchar(30),

foreign key(PoliticianA) references Politician(politicianID),

foreign key(PoliticianB) references Politician(politicianID),

foreign key(Rule) references Rule(offerID),

primary key(PoliticianA,PoliticianB,Rule)

);
 
create table Vote(

voteID int,

voteResult bit,

Politician int,

Rule int,

foreign key(Politician) references Politician(politicianID),

foreign key(Rule) references Rule(offerID),

primary key(voteId,Politician,Rule)

);
 
create table BelongsTo(

Politician int,

GovernmentOffice int,

foreign key(Politician) references Politician(politicianID),

foreign key(GovernmentOffice) references GovernmentOffice(officeId),

primary key (Politician,GovernmentOffice)

);

**B.** **Add at least 5 records to each of the tables**

**solution-**
INSERT INTO GovernmentOffice value(10, 'Department of Transportation');

INSERT INTO GovernmentOffice value(20, 'Department of Defence');

INSERT INTO GovernmentOffice value(30, 'Department of Economy');

INSERT INTO GovernmentOffice value(40, 'Department of Health');

INSERT INTO GovernmentOffice value(50, 'Department of Foreign Affairs');

INSERT INTO GovernmentOffice value(0,'Does not belong to office'); 
 
INSERT INTO Politician value(111,'Miri ','Regev','Halicud',10);

INSERT INTO Politician value(222,'Yoav','Galant','Halicud',20);

INSERT INTO Politician value(333,'Bezalel ','Smotritz','Hazionit Hadatit',30);

INSERT INTO Politician value(444,'Moshe ','Arbel','Shas',40);

INSERT INTO Politician value(555,'Eliyahu ','Cohen','Halicud',50);

INSERT INTO Politician value(666,'a ','b','Halicud',0);

INSERT INTO Politician value(777,'c ','d','Halicud',0);

INSERT INTO Politician value(888,'e ','f','Halicud',0);

INSERT INTO Politician value(999,'g ','h','Halicud',0);

INSERT INTO Politician value(990,'n ','c','Halicud',0);
 
INSERT INTO Rule value(789,'A', 10000000, 'First', 10);

INSERT INTO Rule value(456,'B', 95000000,'Second',20 );

INSERT INTO Rule value(123,'C', 150000000,'Third',30 );

INSERT INTO Rule value(741,'D',80000000, 'Pre-reading',40 );

INSERT INTO Rule value(852,'E',120000000,'First',50 );
 
INSERT INTO Offer value(111,789);

INSERT INTO Offer value(222,456);

INSERT INTO Offer value(333,123);

INSERT INTO Offer value(444,741);

INSERT INTO Offer value(555,852);
 
INSERT INTO Update_ value(111,666,789,1,'11:30');

INSERT INTO Update_ value(222,777,456,1,'13:00');

INSERT INTO Update_ value(333,888,123,0,'09:00');

INSERT INTO Update_ value(444,999,741,0,'15:00');

INSERT INTO Update_ value(555,990,852,1,'08:00');
 
INSERT INTO Vote value(1,1,111,789);

INSERT INTO Vote value(1,1,666,789);

INSERT INTO Vote value(2,0,222,456);

INSERT INTO Vote value(2,1,777,456);

INSERT INTO Vote value(3,0,333,123);

INSERT INTO Vote value(4,1,444,741);

INSERT INTO Vote value(5,1,555,852);

INSERT INTO Vote value(5,1,990,852);

 
INSERT INTO BelongsTo value(111,10);

INSERT INTO BelongsTo value(222,20);

INSERT INTO BelongsTo value(333,30);

INSERT INTO BelongsTo value(444,40);

INSERT INTO BelongsTo value(555,50);

**C. After presenting your solution, it turns out that the solution must include several additional elements:**
The current solution refers to documenting intentions of arriving or not arriving to vote only. You are now also required to document what the MK plans to vote for.
Since government ministries have different budgets, and since different bills have effects on the government ministry's budget, you must reflect (i) (the ministry's budget)
(it can be assumed that it does not change from year to year).
You must also document (ii) (the minister's role in the ministry, for example Galant) - Minister of Defense,
Smotritz - Minister in the Ministry of Defense, and the dates on which they served in their positions in the Ministry.
In addition, you must document (iii) the city, street and number of the building where the office is located (it can be assumed that there is only one building).

**solution-**

 alter table GovernmentOffice add office_budget int;
 
 alter table BelongsTo add role_in_office VARCHAR(40);
 
 alter table BelongsTo add Date_of_starting_position VARCHAR(20);
 
 alter table BelongsTo add Job_end_date VARCHAR(20);
 
 alter table GovernmentOffice add office_city VARCHAR(30);
 
 alter table GovernmentOffice add office_street VARCHAR(30);
 
 alter table GovernmentOffice add building_number int;
 

**D.** **We have updated the existing records and/or tables (in the DML context) so that they reflect the new requirements mentioned above.
If there is no need to change, provide a detailed explanation why there is no need to change.**

**solution-**

update  GovernmentOffice set office_budget = '800000' where OfficeId= 10
;                  
update  BelongsTo set role_in_office= 'Minister of Transportation' where GovernmentOffice = 10 and Politician = 111;

update  BelongsTo set Date_of_starting_position = '2021-01-01' where GovernmentOffice = 10 and Politician = 111;

update  BelongsTo set Job_end_date = '2023-01-01' where GovernmentOffice = 10 and Politician = 111;

update  GovernmentOffice set office_city= 'Tel-Aviv' where officeId= 10;

update  GovernmentOffice set office_street= 'Dizengoff' where officeId= 10;

update  GovernmentOffice set building_number= '1' where officeId= 10;

update  GovernmentOffice set office_budget = '700000' where OfficeId= 20;

update  GovernmentOffice set office_city= 'Tel-Aviv' where officeId= 20;

update  GovernmentOffice set office_street= 'Hahagana' where officeId= 20;

update  GovernmentOffice set building_number= '2' where officeId= 20;

update  BelongsTo set role_in_office= 'Defence Minister' where GovernmentOffice = 20 and Politician = 222;

update  BelongsTo set Date_of_starting_position = '2021-01-01' where GovernmentOffice = 20 and Politician = 222;

update  BelongsTo set Job_end_date = '2023-01-01' where GovernmentOffice = 20 and Politician = 222;


update  GovernmentOffice set office_budget = '750000' where OfficeId= 30;

update  GovernmentOffice set office_city= 'Tel-Aviv' where officeId= 30;

update  GovernmentOffice set office_street= 'Dizengoff' where officeId= 30;

update  GovernmentOffice set building_number= '3' where officeId= 30;

update  BelongsTo set role_in_office= 'Minister of Finance' where GovernmentOffice = 30 and Politician = 333;

update  BelongsTo set Date_of_starting_position = '2021-01-01' where GovernmentOffice = 30 and Politician = 333;

update  BelongsTo set Job_end_date = '2023-01-01' where GovernmentOffice = 30 and Politician = 333;


update  GovernmentOffice set office_budget = '550000' where OfficeId= 40;

update  GovernmentOffice set office_city= 'Ramat-Gan' where officeId= 40;

update  GovernmentOffice set office_street= 'Arnon' where officeId= 40;

update  GovernmentOffice set building_number= '4' where officeId= 40;

update  BelongsTo set role_in_office= 'Minister of Healthe' where GovernmentOffice = 40 and Politician = 444;

update  BelongsTo set Date_of_starting_position = '2021-01-01' where GovernmentOffice = 40 and Politician = 444;

update  BelongsTo set Job_end_date = '2023-01-01' where GovernmentOffice = 40 and Politician = 444;


update  GovernmentOffice set office_budget = '400000' where OfficeId= 50;

update  GovernmentOffice set office_city= 'Jerusalem' where officeId= 50;

update  GovernmentOffice set office_street= 'Joshua Ben Non' where officeId= 50;

update  GovernmentOffice set building_number= '5' where officeId= 50;

update  BelongsTo set role_in_office= 'Foreign Minister' where GovernmentOffice = 50 and Politician = 555;

update  BelongsTo set Date_of_starting_position = '2021-01-01' where GovernmentOffice = 50 and Politician = 555;

update  BelongsTo set Job_end_date = '2023-01-01' where GovernmentOffice = 50 and Politician = 555;


**E.** **Create a query that will give the number of government offices**

**solution-**

select count(officeId) as count_office_id

from GovernmentOffice

where officeId <> 0; 

**F.** **Present the names of the members of the Knesset who belong to a government ministry, as well as their position in the ministry, and the name of the ministry.**

**solution-**

select firstName,lastNme,role_in_office,officeName

from BelongsTo INNER join Politician on BelongsTo.Politician = Politician.politicianID 

inner join GovernmentOffice on BelongsTo.GovernmentOffice = GovernmentOffice.officeId

where officeId <> 0 ; 

**G.** **What is the voting percentage in the twenty-fifth Knesset?**

**solution-**

 select count(voteID)/(count(distinct voteID , Rule)*count(politicianID)) * 100 as vote_precent
 
   from Vote inner join Politician on Politician.politicianID=Vote.Politician;
   
**H.** **Present the total costs required for each government office to approve all the laws introduced in the twenty-fifth Knesset and associated with it.**

**solution-**

 select officeName , sum(budget) as sum_budget
 
from GovernmentOffice inner join Rule on GovernmentOffice.officeId = Rule.GovernmentOffice

group by officeName;

**I.** **They presented the names of the Knesset members who initiated laws, the total costs of which were over NIS 100,000,000. The display should be sorted by the total costs, and by the names of the members of the Knesset.**

**solution-**

select politicianID ,firstName,lastNme ,sum(Rule.budget)  as total_cost from Politician

inner join Rule on Politician.GovernmentOffice= Rule.GovernmentOffice

group by politicianID,firstName,lastNme

having sum(Rule.budget) > 100000000

order by total_cost DESC, Politician.lastNme  ASC;

**J.** **Create a new table that will contain the output of section I**

**solution-**

CREATE TABLE laws_over_100_milion

as(select politicianID ,firstName,lastNme ,sum(Rule.budget) as total_cost from Politician

inner join Rule on Politician.GovernmentOffice= Rule.GovernmentOffice

group by politicianID,firstName,lastNme

having sum(Rule.budget) > 100000000

order by total_cost DESC, Politician.lastNme ASC);

**K.** **Delete from the table you created in section J all the records in which the income is less than the average in the above table.**

**solution-**

CREATE TEMPORARY TABLE temp_delete AS

SELECT * FROM laws_over_100_milion

WHERE total_cost < (SELECT AVG(total_cost) FROM laws_over_100_milion);

DELETE FROM laws_over_100_milion

WHERE politicianID IN (SELECT politicianID FROM temp_delete);

DROP TEMPORARY TABLE IF EXISTS temp_delete;

