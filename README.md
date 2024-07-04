### ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​.
 
    SELECT description 
    FROM crime_scene_report as c
    WHERE c.date= 20180118  AND
          type='murder' AND 
          city = "SQL City";
          
- Running this code gives us description about the murder
- The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

  
      SELECT * from person
      WHERE address_street_name="Northwestern Dr"
      ORDER BY address_number DESC LIMIT 1;

- person ID - 14887

      select * 
      from person
      WHERE name LIKE '%Annabel%' and address_street_name = "Franklin Ave";
  
- person Id - 16371

      SELECT p.name,i.transcript
      FROM person as p
      JOIN interview as i
      ON i.person_id = p.id
      Where p.id = 14887 or p.id = 16371;

- This query will give the transcript on their enquiry
- Morty Schapiro	: I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".
- Annabel Miller :	I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

- Combining the clues:
 
      SELECT p.name
      FROM get_fit_now_check_in as c
      JOIN get_fit_now_member as m
      ON c.membership_id = m.id
      JOIN person as p
      ON p.id = m.person_id
      JOIN drivers_license as dl
      ON p.license_id = dl.id
      WHERE m.membership_status = 'gold' and c.membership_id LIKE "48z%" and dl.plate_number LIKE "%H42W%";
  
- We got the name and details of the murderer (Name)
- Jeremy Bowers
- seeing his transcript n enquiry:
 
      SELECT transcript
      FROM interview as i
      JOIN person as p
      ON p.id = i.person_id
      WHERE p.name = 'Jeremy Bowers';
- I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.
- ### The Brain behind the murder is a woman, height between 65" to 67"
- ### Hair Color is RED and has Tesla model S
- ### Attended SQL Symphony Conert 3 times in dec 2017
- Combining These Clues We end up with,
  
          SELECT distinct(p.name)
          FROM person as p 
          JOIN drivers_license as dl
          ON p.license_id = dl.id
          JOIN facebook_event_checkin as fb
          ON p.id = fb.person_id
          WHERE dl.height BETWEEN 65 AND 67 AND 
          	  dl.hair_color = 'red' AND 
          	  dl.car_make="Tesla" AND 
          	  dl.car_model ="Model S" AND
          	  dl.gender="female" AND
          	  fb.person_id in (
            		SELECT person_id
            		FROM facebook_event_checkin
            		WHERE event_name = 'SQL Symphony Concert' AND 
            			  date LIKE '201712%'
            		GROUP BY person_id
            		HAVING count(person_id)=3)
## WE GOT THE MASTER MIND BEHIND THIS MURDER -- Miranda Priestly --
