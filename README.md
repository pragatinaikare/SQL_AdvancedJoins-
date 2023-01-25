## Hacker Rank challenge:
Level: Hard

Julia conducted a  15 days of learning SQL contest. The start date of the contest was March 01, 2016 and the end date was March 15, 2016.
Write a query to print total number of unique hackers who made at least 1 submission each day (starting on the first day of the contest), and find the hacker_id and name of the hacker who made maximum number of submissions each day. If more than one such hacker has a maximum number of submissions, print the lowest hacker_id. The query should print this information for each day of the contest, sorted by the date

Input Format

The following tables hold contest data:
Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker
Column     |   Type
-----------| ---------
Hacker_Id  |   Int
Name       |   String 


Submissions: The submission_date is the date of the submission, submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission, and score is the score of the submissions

Column    	 |   Type
---------------- | --------------
submission_date  |   Date
submission_id    |   Int 
Hacker_Id        |   Int 
score	         |   Int




#### Answer:
SELECT SUBMISSION_DATE
	,(
		SELECT COUNT(DISTINCT HACKER_ID)
		FROM SUBMISSIONS Sub1
		WHERE Sub1.SUBMISSION_DATE = Sub.SUBMISSION_DATE
			AND (
				SELECT COUNT(DISTINCT Sub3.SUBMISSION_DATE)
				FROM SUBMISSIONS Sub3
				WHERE Sub3.HACKER_ID = Sub1.HACKER_ID
					AND Sub3.SUBMISSION_DATE < Sub.SUBMISSION_DATE
				) = DATEDIFF(Sub.SUBMISSION_DATE, '2016-03-01')
		)
	,(
		SELECT HACKER_ID
		FROM SUBMISSIONS Sub1
		WHERE Sub1.SUBMISSION_DATE = Sub.SUBMISSION_DATE
		GROUP BY HACKER_ID
		ORDER BY COUNT(SUBMISSION_ID) DESC
			,HACKER_ID LIMIT 1
		) AS TMP
	,(
		SELECT NAME
		FROM HACKERS
		WHERE HACKER_ID = TMP
		)
FROM (
	SELECT DISTINCT SUBMISSION_DATE
	FROM SUBMISSIONS
	) Sub
GROUP BY SUBMISSION_DATE;
