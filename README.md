# Who-killed-the-CEO-SQL-Case-Study

# SQL Murder Mystery: “Who Killed the CEO?”

Select * from alibis;

Select * from calls;

Select * from employees;

Select * from evidence;

Select * from keycard_logs;

# Analyse where and when crime happened

Select room as Crime_location,
entry_time as Time_entered,
exit_time as time_exited
from keycard_logs
where room='CEO Office'
order by entry_time;



# cross check who accessed the critical area at that time

Select k.employee_Id, e.name,e.role, k.room, k.entry_time,k.exit_time from keycard_logs k
join employees e on k.employee_id=e.employee_id
where k.room='CEO Office'
and k.entry_time='2025-10-15 20:50:00';


# Question 3 Cross check the alibis with actual logs
Select e.name, a.claimed_location,k.room,a.claim_time from alibis A
join keycard_logs k
on a.employee_id=k.employee_id
join employees e
on e.employee_id=a.employee_id
where a.claim_time between k.entry_time and k.exit_Time 
and claimed_location<>K.room;





# Investigating the suspicious calls made around the time.

 Select e.name as caller_name, c.receiver_id, c.call_time,c.duration_sec
 from calls c
 join employees as e on 
 e.employee_id=c.caller_id
 where call_time between '2025-10-15 20:45:00' and '2025-10-15 21:15:00';




# Match evidence with movement and claims
Select distinct e.name,ev.room,ev.description from evidence ev 
join keycard_logs k
on ev.room=k.room
join employees e 
on e.employee_id=k.employee_id;




# Combine all findings to identify the killer

Select distinct e.employee_id,e.name from employees e join keycard_logs k 
on e.employee_id=k.employee_id
join alibis as a
on e.employee_id=a.employee_id
where k.room='CEO Office'
and '2025-10-15 21:00:00' between entry_time and exit_time 
and a.claimed_location<>'CEO Office';


** As per the Database investigation The Killer was found
Using keycard_logs, I identify employees present in the CEO Office at the time of crime then
I cross verified their and found the mismatch between claimed and actual locations
Finally, combining access logs and fales alibi analysis I concluded that David Kumar(Emp_Id 4) was 
present at the crime location and fales alibi making him a killer **

