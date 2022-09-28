# The State of Colorado Congressional Electiion Analysis.
## 1. Overview
#### Seth and Tom needs help in auditing the recently held election in State of Colorado. They have dataset for all the ballots casted and now want simple coding to determine total number of votes casted, complete list of candidates, votes each candidate received, percentage of votes received by each candidates, counties voter turnouts, largest counties by vote and the winner of the election. These auditing could be easily done by the use of tool such as Excel, however, Seth and his team wanted to create a program that can be used for any and all type of election in future. The objective of this module was to create a code in python which audits the election results and can be used in future as well. 
## 2. Election-Audit Results
- The total number of votes casted in the congressional election was 369,711.   
- Breakdown of the votes and vote percentage for each county. 
    
    - Jefferson County: 10.5% (38,855)
    - Denver County: 82.8% (306,055)
    - Arapahoe County: 3.1% (24,801)
    
- The county with the largest number of vote is Denver County. 
- Breakdown of votes and vote percentage for each Candidate.

    - Charles Casper Stockham: 23.0% (85,213)
    - Diana DeGette: 73.8% (272,892)
    - Raymon Anthony Doane: 3.1% (11,606)
 - Diana DeGette won the election with total votes of 272,892 and won by 73.8% compared to other candidates.  

## 3. Election-Audit Summary 

#### This python script written for congressional election analysis can be used by anyone and at any level of elections. The minor modifications can be made to use this election analysis script. Lets take and example of presedential election, presedential election will have extra data columns such as state, city, precinct along with county. For complete Presedential election audit, we can initialize appropriate variables for states, city and precinct, and add if statements to check, append and calculatetotal votes, vote turnouts winner, popular votes for states, city and precinct.  

The script for current election analysis is available below. 

```
# -*- coding: UTF-8 -*-
"""PyPoll Homework Challenge starter code."""

# Add our dependencies that we will be using to achieve our goal of tally count.
# The csv module implements classes to read and write tabular data in CSV format.
import csv

# The os module provides a portable way of using operating system dependent functionality.
import os

# The os.path module implements some useful functions on pathnames.
# Add a variable to load a file from a path.
file_to_load = os.path.join('..','Resources', 'election_results.csv')
# Add a variable to save the file to a path.
file_to_save = os.path.join('analysis', 'election_results.txt')

# Start initial this module's variables that we will be using through out the module
# 
#
#
#

# Initialize a total vote counter.
total_votes = 0

# Candidate Options and candidate votes.
candidate_options = []
candidate_votes = {}

# 1: Create a county list and county votes dictionary.
county_list = []
county_votes = {}

# Track the winning candidate, vote count and percentage
winning_candidate = ""
winning_count = 0
winning_percentage = 0

# 2: Track the largest county and county voter turnout.
largest_county_turnout = ""
largest_county_turnout_count = 0
largest_county_percentage = 0

# Read the csv and convert it into a list of dictionaries
with open(file_to_load) as election_data:
    # Read the file object with the reader function.
    reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file.
    for row in reader:

        # Add to the total vote count
        total_votes += 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write a decision statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_list:

            # 4b: Add the existing county to the list of counties.
            county_list.append(county_name)

            # 4c: Begin tracking the county's vote count.
            county_votes[county_name] = 0

        # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1


# Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n"
    )
    print(election_results, end="")

    # Save the final vote count to the text file.
    txt_file.write(election_results)

    # 6a: Write a repetition statement to get the county from the county dictionary.
    for county in county_list:

        # 6b: Initialize a variable to hold the county’s votes as they are retrieved from the county votes dictionary.
        county_vote = county_votes.get(county)

        # 6c: Calculate the percent of total votes for the county.
        county_vote_percentage = float(county_vote) / float(total_votes) * 100

        # 6d: Print the county results to the terminal.
        county_results = f"{county}: {county_vote_percentage:.1f}% ({county_vote:,})\n"

        # Print the counties to test.
        print(county_results)

        # 6e: Save the county votes to a text file.
        txt_file.write(county_results)

        # 6f: Write a decision statement to determine the winning county and get its vote count.
        if (county_vote > largest_county_turnout_count) and (
            county_vote_percentage > largest_county_percentage
        ):
            # True
            largest_county_turnout_count = county_vote
            largest_county_percentage = county_vote_percentage
            largest_county_turnout = county

    # 7: Print the county with the largest turnout to the terminal.
    winning_county_print = (
        f"-------------------------\n"
        f"Largest County Turnout: {largest_county_turnout}\n"
        f"-------------------------\n"
    )
    print(winning_county_print)

    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(winning_county_print)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n"

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n"
    )
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)
    ```
    
