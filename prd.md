# Project Requirements Document
For use by agentic coders

Task: automatically scrape ianseo.net archery results data from 3 BUCS archery qualification events and combine outputs. Steps listed below, broken down by section. If additional libraries are needed, let me know.

- Itialisation
    - Extract input parameters from inputParams.txt
- Data Scraping
    - Use pandas in a .py file to scrape data from HTML tables at URLs such as https://www.ianseo.net/TourData/2026/26327/IQREO.php, where the 5 digit number is the tournamentID from inputParams.txt. Infer the format of the HTML tables and data types of all data. The 5 letter code at the end of the URL must be varied according to the below.
        - the first two letters are always IQ
        - the third letter is any one of: R, B, C, L - these stand for the four bowstyles of Recurve, Barebow, Compound and Longbow
        - the fourth letter is any one of: E, N - these stand for the two expereince levels of Expereinced and Novice
        - the fith letter is any one of: O, W - these stand for the two gender categories of Open and Women
    - All permutations of the above are possible, however some will not exist due to no archers entering in that category. This must be error handled and sliently skipped.
    - Each of these HTML tables should be appended into a single large dataframe, with one row for each archer. All data should be kept, and an additional feature called 'class (with Exp)' added with values of the the last 3 letters of the 5 letter code at the end of the URL. Which of the tournamentIDs the data comes from is not important.
- Data parsing
    - Add an another column called 'class' which removes the middle digit from the 3 letter code in 'class (with Exp)'
    - Calculate national rank by sorting score high to low, filtered by each class. Add this as a new column
    - Split the Country column by the delimiter ' - ' and return the first substring into a new column called 'club code' and the second into another column called 'club name'
    - Calculate the total number of archers in each class and transform it into a table of bowstyle against gender.
    - The number of archers who progress to the national finals is directly propotional to the number of entries to the qualifying events in each class. A minimum of 8 archers will always progress in each category and the total capacity for finals is given as a parameter in inputParams.txt. The number of archers progressing to finals in each class is given by max(8, entries_in_class * (250 / total_entries)).
    - Calculate the the number of archers who will progress to finals in each class. Round down to the nearest integer when needed. This is called the placelimit and is specific to each class
    - Filter the data of qualification score by each class and find the score of the archer with national ranking equal to the number of archers who will progress to finals.
    - Filter the data to where the club code matches that given in inputParams.txt. For these archers compute (placelimit - national ranking)/placelimit, noting that placelimit varies by class. Add this as a column called 'safety margin'. Safety margins less than 0 are archers who will not qualify.
- Outputs
    - the python script should output a single .xlsx file, with multiple sheets within it. Details of what each sheet shold contain are given below. For all sheets where results are printed, return national rank, archer name, club name and all score details. use appropriate descriptive sheet names
    - One sheet for each class with the results for that class printed, clearly indicating where the cut off to national finals is.
    - one sheet with a table of entry numbers per class as a table of bowstyle by gender
    - one sheet with a table of national rank to qualify to finals as a table of bowstyle by gender
    - one sheet with a table of score to qualify to finals as a table of bowstyle by gender
    - one sheet with a table of all archers in the club given by club code in inputParams.txt, their results and safety margin for qualification.
- Logging
    - Log when data scraping from each tournament is complete, and when the output has been generated.


# New Requirements 1
In addition to meeting the above requirements, adapt the code to alos do the following.
- Capatalise each word for all column headings
- Use letter apreviations for the sheet names for results by class, such as RO
- For 20y-1 and 20y-2 data, extract only the number before the '/' and remove all data after this
- for the club results output table, multiply the safety margin by 100 to make a % and sort descending by safety margin
- Change the entry_numbers sheet to be labelled as qualification entry numbers
- calculate number of people to qualify after checking if some categories have the minimum of 8 participants. At present there are 257 spaces allocated for the finals, but only 250 places. Check which categories need 8 spaces, subtract these from the total capacity, then do the proportionate allocation.
- Where scores are tied, the rank is decided first by who has more hits hits and then by golds. Apply these sorts to the dataframe after all data is read in and before national rank is calculated

# New Requirements 2
- Round safety margin to 2 decimal places.
- in club results table also report qualification score for the category of each archer, after their class but before the safety margin
- Rename Tot. to Score